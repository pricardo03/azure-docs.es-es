---
title: Destinos de proceso del entrenamiento del modelo
titleSuffix: Azure Machine Learning service
description: Configurar los entornos de entrenamiento (destinos de proceso) del entrenamiento del modelo de Machine Learning. Es fácil cambiar de entorno de entrenamiento. Inicie el entrenamiento de forma local y, si necesita escalar horizontalmente, cambie a un destino de proceso en la nube. Databricks
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 664d56daf3e70e2e5699d0c07331c466c60e06c5
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338869"
---
# <a name="set-up-compute-targets-for-model-training"></a>Configuración de destinos de proceso del entrenamiento del modelo

Con el servicio Azure Machine Learning, puede entrenar un modelo en distintos recursos de proceso. Dichos recursos, denominados __destinos de proceso__, pueden ser locales o estar en la nube. En este documento, aprenderá acerca de los destinos de proceso admitidos y cómo usarlos.

Un destino de proceso es un recurso en el que se ejecuta el script de entrenamiento o en el que se hospeda un modelo cuando se implementa como un servicio web. Los destinos de proceso se pueden crear y administrar mediante el SDK de Azure Machine Learning, Azure Portal o la CLI de Azure. Si tiene destinos de proceso creados mediante cualquier otro servicio (por ejemplo, un clúster de HDInsight), para usarlos debe adjuntarlos al área de trabajo del servicio Azure Machine Learning.

Azure Machine Learning admite tres amplias categorías de destinos de proceso:

* __Local__: un equipo local o una máquina virtual basada en la nube que se usen como un entorno de desarrollo y experimentación. 

* __Proceso administrado__ Proceso de Azure Machine Learning es una oferta de proceso administrada por el servicio de Azure Machine Learning. Permite crear fácilmente procesos de uno o varios nodos para el entrenamiento, la realización de pruebas y la inferencia de lotes.

* __Proceso adjunto__: también puede traer su propio proceso en la nube de Azure y adjuntarlo a Azure Machine Learning. A continuación encontrará más información acerca de los tipos de proceso admitidos y cómo usarlos.


## <a name="supported-compute-targets"></a>Destinos de proceso admitidos

El servicio Azure Machine Learning tiene distintas modalidades de soporte técnico en los distintos destinos de proceso. Un ciclo de vida de desarrollo de modelos típico comienza con el desarrollo y la experimentación en una pequeña cantidad de datos. En esta fase, se recomienda usar un entorno local. Por ejemplo, un equipo local o una máquina virtual basada en la nube. Si escala verticalmente su entrenamiento a conjuntos de datos grandes o realiza el entrenamiento distribuido, se recomienda usar Proceso de Azure Machine Learning para crear un clúster con uno o varios nodos que se escala automáticamente cada vez que se envía una ejecución. También puede adjuntar su propio recurso de proceso, aunque el soporte técnico para varios escenarios puede variar como se detalla a continuación:

|Destino de proceso| Aceleración de GPU | Ajuste automatizado de hiperparámetros | Automated Machine Learning | Canalización descriptiva|
|----|:----:|:----:|:----:|:----:|
|[Equipo local](#local)| Es posible | &nbsp; | ✓ | &nbsp; |
|[Proceso de Azure Machine Learning](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Máquina virtual remota](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | ✓ | ✓[*](#pipeline-only) |
|[Análisis con Azure Data Lake](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[HDInsight de Azure](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>__*__ Azure Databricks y Azure Data Lake Analytics __solo__ se pueden usar en una canalización. Para más información sobre las canalizaciones, consulte el documento [Canalizaciones y Azure Machine Learning](concept-ml-pipelines.md).

> [!IMPORTANT]
> Proceso de Azure Machine Learning se debe crear desde dentro de un área de trabajo. Las instancias existentes no se pueden adjuntar a un área de trabajo.
>
> Los restantes destinos de proceso se deben crear fuera de Azure Machine Learning y luego se adjuntan a su área de trabajo.

> [!NOTE]
> Algunos destinos de proceso utilizan imágenes de contenedor de Docker al entrenar un modelo. La imagen base de GPU debe usarse solo en Microsoft Azure Services. En el caso del entrenamiento de modelos, dichos servicios son:
>
> * Proceso de Azure Machine Learning
> * Azure Kubernetes Service
> * Data Science Virtual Machine

## <a name="workflow"></a>Flujo de trabajo

El flujo de trabajo para desarrollar e implementar un modelo con Azure Machine Learning sigue estos pasos:

1. Los scripts de entrenamiento del aprendizaje automático se desarrollan en Python.
1. Se crea y configura un destino de proceso, o se asocia uno existente.
1. Se envían los scripts de entrenamiento al destino de proceso.
1. Se inspeccionan los resultados para buscar el mejor modelo.
1. Se registra el modelo en el registro del modelo.
1. Se implementa el modelo.

> [!IMPORTANT]
> El script de entrenamiento no está vinculado a un destino de proceso específico. Puede entrenarlo inicialmente en el equipo local y después cambiar los destinos de proceso sin tener que volver a escribir el script de entrenamiento.

> [!TIP]
> Cada vez que asocie un destino de proceso a su área de trabajo, bien mediante la creación de un proceso administrado o la asociación de un proceso existente, deberá especificar un nombre para el proceso. Debe tener una longitud de entre 2 y 16 caracteres.

Cambiar de un destino de proceso a otro implica crear una [configuración de ejecución](concept-azure-machine-learning-architecture.md#run-configuration). La configuración de ejecución define cómo se ejecuta el script en el destino de proceso.

## <a name="training-scripts"></a>Scripts de entrenamiento

Al empezar a ejecutar un entrenamiento, se crea una instantánea del directorio que contiene los scripts de entrenamiento y se envía al destino de proceso. Para más información, consulte las [instantáneas](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Equipo local

Al entrenar localmente, se usa el SDK para enviar la operación de entrenamiento. Puede entrenar con un entorno administrado por el usuario o por el sistema.

### <a name="user-managed-environment"></a>Entorno administrado por el usuario

En un entorno administrado por el usuario, usted es responsable de garantizar que todos los paquetes necesarios estén disponibles en el entorno de Python que elija para ejecutar el script. El fragmento de código siguiente es un ejemplo de configuración del entrenamiento de un entorno administrado por el usuario:

```python
from azureml.core.runconfig import RunConfiguration

# Editing a run configuration property on-fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# You can choose a specific Python environment by pointing to a Python path 
#run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

  
### <a name="system-managed-environment"></a>Entorno administrado por el sistema

Los entornos administrados por el sistema dependen de Conda para administrar las dependencias. Conda crea un archivo denominado `conda_dependencies.yml` que contiene una lista de dependencias. A continuación, puede pedir al sistema que genere un nuevo entorno de Conda y ejecutar los scripts en él. Los entornos administrados por el sistema pueden reutilizarse más adelante, siempre y cuando los archivos `conda_dependencies.yml` permanezcan sin cambios. 

La configuración inicial de un nuevo entorno puede tardar varios minutos en completarse, según el tamaño de las dependencias necesarias. El fragmento de código siguiente muestra cómo crear un entorno administrado por el sistema que dependa de scikit-learn:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

## <a id="amlcompute"></a>Proceso de Azure Machine Learning

Proceso de Azure Machine Learning es una infraestructura de procesos administrada que permite al usuario crear fácilmente un proceso de único a varios nodos. Se crea __dentro de la región de su área de trabajo__ y es un recurso que se puede compartir con otros usuarios del área de trabajo. Se escala verticalmente automáticamente cuando se envía un trabajo y se puede colocarse en una instancia de Azure Virtual Network. Se ejecuta en un __entorno con contenedores__ y se empaquetan las dependencias del modelo en un contenedor de Docker.

Proceso Azure Machine Learning Compute para distribuir el proceso de entrenamiento en un clúster de nodos de proceso de CPU o GPU de la nube. Para más información acerca de los tamaños de máquina virtual que incluyen las GPU, consulte la documentación de los [tamaños de máquinas virtuales optimizadas para GPU](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

> [!NOTE]
> Proceso de Azure Machine Learning tiene límites predeterminados en aspectos tales como el número de núcleos que se pueden asignar. Para más información, consulte el documento [Administración y solicitud de cuotas para recursos de Azure](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

Puede crear una instancia de Proceso de Azure Machine Learning a petición al programar una ejecución, o bien como un recurso persistente.

### <a name="run-based-creation"></a>Creación basada en ejecución

Puede crear una instancia de Proceso de Azure Machine Learning como un destino de proceso en tiempo de ejecución. En ese caso, el proceso se crea automáticamente para la ejecución, se escala verticalmente al valor de max_nodes que especifique en la configuración de la ejecución y se __elimina automáticamente__ después de que se completa la ejecución.

> [!IMPORTANT]
> La creación basada en ejecución de una instancia de Azure Machine Learning está actualmente el estado Versión preliminar. No use la creación basada en ejecución si usa el ajuste de hiperparámetros o el aprendizaje automático automatizado. Si necesita usar cualquiera de ellos, cree la instancia de Azure Machine Learning Compute antes de enviar una ejecución.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

#Let us first list the supported VM families for Azure Machine Learning Compute
AmlCompute.supported_vmsizes()

from azureml.core.runconfig import RunConfiguration

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use AmlCompute to execute script.
run_config.target = "amlcompute"

# AmlCompute will be created in the same region as workspace. Set vm size for AmlCompute from the list returned above
run_config.amlcompute.vm_size = 'STANDARD_D2_V2'

```

### <a name="persistent-compute-basic"></a>Proceso persistente (Básico)

Una instancia de Proceso de Azure Machine Learning persistente se puede reutilizar en varios trabajos. Se pueden compartir con otros usuarios del área de trabajo y se mantiene entre un trabajo y otro.

Para crear un recurso de Azure Machine Learning Compute persistente, especifique los parámetros `vm_size` y `max_nodes`. Luego Azure Machine Learning usa valores predeterminados inteligentes para el resto de los parámetros.  Por ejemplo, el proceso se establece para reducir el escalado automático hasta cero nodos cuando no se usa y crear máquinas virtuales dedicadas para ejecutar sus trabajos cuando sea necesario. 

* **vm_size**: la familia de los nodos creados por Proceso de Azure Machine Learning.
* **max_nodes**: el número máximo de nodos que se escalan automáticamente mientras ejecuta un trabajo en Proceso de Azure Machine Learning.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```

### <a name="persistent-compute-advanced"></a>Proceso persistente (Avanzado)

Cuando cree una instancia de Proceso Azure Machine Learning puede configurar varias propiedades avanzadas.  Estas propiedades permiten crear un clúster persistente de tamaño fijo o dentro de una instancia existente de Azure Virtual Network de su suscripción.

Además de `vm_size` y `max_nodes` puede usar las siguientes propiedades:

* **min_nodes**: el número mínimo de nodos (el valor predeterminado es 0 nodos) que se reducen verticalmente mientras se ejecuta un trabajo en Proceso de Azure Machine Learning.
* **vm_priority**: elija entre máquinas virtuales "dedicadas" (valor predeterminado) y "de baja prioridad" al crear el una instancia de Azure Machine Learning Compute. Las máquinas virtuales de baja prioridad usan la capacidad de sobrante de Azure y, por consiguiente, son más baratas, pero con ellas se corre el riesgo de que se cambie su ejecución.
* **idle_seconds_before_scaledown**: tiempo de inactividad (el valor predeterminado es 120 segundos) que se espera después de la finalización de la ejecución antes realizar la escalabilidad automática a min_nodes.
* **vnet_resourcegroup_name**: Grupo de recursos de la red virtual __existente__. La instancia de Proceso de Azure Machine Learning se crea dentro de esta red virtual.
* **vnet_name**: nombre de la red virtual. La red virtual debe estar en la misma región que el área de trabajo de Azure Machine Learning.
* **subnet_name**: nombre de una subred de la red virtual A los recursos de Azure Machine Learning Compute se les asignarán direcciones IP de este intervalo de subred.

> [!TIP]
> Cuando crea un recurso de Azure Machine Learning Compute persistente también tiene la capacidad de actualizar sus propiedades, como min_nodes o max_nodes. Para ello, solo tiene que llamar a la función `update()`.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that cluster does not exist already
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           vm_priority='lowpriority',
                                                           min_nodes=2,
                                                           max_nodes=4,
                                                           idle_seconds_before_scaledown='300',
                                                           vnet_resourcegroup_name='<my-resource-group>',
                                                           vnet_name='<my-vnet-name>',
                                                           subnet_name='<my-subnet-name>')
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```


## <a id="vm"></a>Máquina virtual remota

Azure Machine Learning también admite la posibilidad de que traiga su propio recurso de proceso y lo adjunte a su área de trabajo. Uno de estos tipos de recursos es una máquina virtual remota arbitraria, siempre que se pueda acceder a ella desde el servicio Azure Machine Learning. Puede ser una máquina virtual de Azure o un servidor remoto de su organización o local. En concreto, dada la dirección IP y las credenciales (nombre de usuario/contraseña o clave SSH), puede usar cualquier máquina virtual a la que se pueda acceder para las ejecuciones remotas.
Puede usar un entorno de Conda integrado en el sistema, un entorno de Python existente o un contenedor de Docker. Parar que la ejecución se pueda realizar mediante un contenedor de Docker es preciso que un motor de Docker se ejecute en la máquina virtual. Esta funcionalidad es especialmente útil cuando se desea un entorno de desarrollo o experimentación basado en la nube más flexible que un equipo local.

> [!TIP]
> Se recomienda usar Data Science Virtual Machine como máquina virtual de Azure preferida para este escenario. Es un entorno de desarrollo de inteligencia artificial y ciencia de datos preconfigurado de Azure con un conjunto de herramientas y marcos protegidos durante todo el ciclo de vida del desarrollo de Machine Learning. Para más información acerca del uso de Data Science Virtual Machine con Azure Machine Learning, consulte el documento [Configuración de un entorno de desarrollo](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

> [!WARNING]
> Azure Machine Learning solo admite máquinas virtuales que ejecuten Ubuntu. Al crear una máquina virtual o seleccionar una existente, debe seleccionar una que use Ubuntu.

Los pasos siguientes utilizan el SDK para configurar una máquina virtual Data Science Virtual Machine (DSVM) como un destino de entrenamiento:

1. Para asociar una máquina virtual existente como un destino de proceso, debe proporcionar el nombre de dominio completo, el nombre de inicio de sesión y la contraseña de la máquina virtual.  En el ejemplo, reemplace ```<fqdn>``` con el nombre de dominio completo público de la máquina virtual o con la dirección IP pública. Reemplace ```<username>``` y ```<password>``` con el usuario SSH y la contraseña para la máquina virtual:

    ```python
    from azureml.core.compute import RemoteCompute, ComputeTarget
    
    # Create compute config.
    attach_config = RemoteCompute.attach_configuration(address = "ipaddress",
                                                       ssh_port=22,
                                                       username='<username>',
                                                       password="<password>")
    # If using SSH instead of a password, use this:
    #                                                  ssh_port=22,
    #                                                   username='<username>',
    #                                                   password=None,
    #                                                   private_key_file="path-to-file",
    #                                                   private_key_passphrase="passphrase")

    # Attach the compute
    compute = ComputeTarget.attach(ws, "attach-dsvm", attach_config)

    compute.wait_for_completion(show_output=True)
    ```

1. Cree una configuración para el destino de proceso de Data Science Virtual Machine. Docker y Conda se utilizan para crear y configurar el entorno de entrenamiento en DSVM:

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load the "cpu-dsvm.runconfig" file (created by the above attach operation) in memory
    run_config = RunConfiguration(framework = "python")

    # Set compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use CPU base image
    # If you want to use GPU in DSVM, you must also use GPU base Docker image azureml.core.runconfig.DEFAULT_GPU_IMAGE
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask system to provision a new one based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when used the first time.
    run_config.prepare_environment = True

    # specify CondaDependencies obj
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

## <a id="databricks"></a>Azure Databricks

Azure Databricks es un entorno basado en Apache Spark de la nube de Azure. Se puede usar como destino de proceso al entrenar modelos con una canalización de Azure Machine Learning.

> [!IMPORTANT]
> Un destino de proceso de Azure Databricks solo se puede usar en una canalización de Machine Learning.
>
> Para usarlo para entrenar a su modelo, primero debe crear un área de trabajo de Azure Databricks. Para crear estos recursos, consulte el documento [Ejecución de un trabajo de Spark en Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Para asociar Azure Databricks como destino de proceso, debe usar el SDK de Azure Machine Learning y proporcionar la siguiente información:

* __Nombre de proceso__: el nombre que desea asignar a este recurso de proceso.
* __Nombre de área de trabajo de Databricks__: el nombre del área de trabajo de Azure Databricks.
* __Token de acceso__: el token de acceso usado para autenticarse en Azure Databricks. Para generar un token de acceso, consulte el documento [Autenticación](https://docs.azuredatabricks.net/api/latest/authentication.html).

El código siguiente muestra cómo asociar Azure Databricks como destino de proceso:

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_workspace_name = os.environ.get("AML_DATABRICKS_WORKSPACE", "<databricks_workspace_name>")
databricks_resource_group = os.environ.get("AML_DATABRICKS_RESOURCE_GROUP", "<databricks_resource_group>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_workspace_name {}'.format(databricks_workspace_name))
    print('databricks_access_token {}'.format(databricks_access_token))

    # Create attach config
    attach_config = DatabricksCompute.attach_configuration(resource_group = databricks_resource_group,
                                                           workspace_name = databricks_workspace_name,
                                                           access_token = databricks_access_token)
    databricks_compute = ComputeTarget.attach(
             ws,
             databricks_compute_name,
             attach_config
         )
    
    databricks_compute.wait_for_completion(True)
```

## <a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics es una plataforma de análisis de macrodatos de la nube de Azure. Se puede usar como destino de proceso al entrenar modelos con una canalización de Azure Machine Learning.

> [!IMPORTANT]
> Un destino de proceso de Azure Data Lake Analytics solo se puede usar en una canalización de Machine Learning.
>
> Para usarlo para entrenar a su modelo, primero debe crear una cuenta de Azure Data Lake Analytics. Para crear este recurso, consulte la [Introducción a Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal) documento.

Para asociar Data Lake Analytics como destino de proceso, debe usar el SDK de Azure Machine Learning y proporcionar la siguiente información:

* __Nombre de proceso__: el nombre que desea asignar a este recurso de proceso.
* __Grupo de recursos__: el grupo de recursos que contiene la cuenta de Data Lake Analytics.
* __Nombre de cuenta__: El nombre de la cuenta de Data Lake Analytics.

El código siguiente muestra cómo asociar Data Lake Analytics como destino de proceso:

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_group = os.environ.get("AML_ADLA_RESOURCE_GROUP", "<adla_resource_group>")
adla_account_name = os.environ.get("AML_ADLA_ACCOUNT_NAME", "<adla_account_name>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_group))
    print('adla_account_name {}'.format(adla_account_name))
    # create attach config
    attach_config = AdlaCompute.attach_configuration(resource_group = adla_resource_group,
                                                     account_name = adla_account_name)
    # Attach ADLA
    adla_compute = ComputeTarget.attach(
             ws,
             adla_compute_name,
             attach_config
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Las canalizaciones de Azure Machine Learning solo pueden trabajar con datos almacenados en el almacén de datos predeterminado de la cuenta de Data Lake Analytics. Si los datos con los que necesita trabajar están en un almacén no predeterminado, puede usar [`DataTransferStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) para copiar los datos antes del entrenamiento.

## <a id="hdinsight"></a>Azure HDInsight 

HDInsight es una plataforma popular para el análisis de macrodatos. Proporciona Apache Spark, que se puede usar para entrenar el modelo.

> [!IMPORTANT]
> Debe crear el clúster de HDInsight antes de usarlo para entrenar el modelo. Para crear un clúster de Spark en HDInsight, consulte el documento [Inicio rápido: Creación de un clúster de Spark en HDInsight con una plantilla](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql).
>
> Al crear el clúster, debe especificar un nombre de usuario SSH y una contraseña. Tenga en cuenta estos valores, según sean necesarios al usar HDInsight como un destino de proceso.
>
> Una vez creado el clúster, tiene un nombre de dominio completo (FQDN) de `<clustername>.azurehdinsight.net`, donde `<clustername>` es el nombre proporcionado para el clúster. Necesitará esta dirección (o la dirección IP pública del clúster) para usarlo como un destino de proceso.

Para configurar HDInsight como un destino de proceso, debe proporcionar el nombre de dominio completo, el nombre de inicio de sesión y la contraseña para el clúster HDInsight. El ejemplo siguiente usa el SDK para asociar un clúster al área de trabajo. En el ejemplo, reemplace `<fqdn>` con el nombre de dominio completo público del clúster o con la dirección IP pública. Reemplace `<username>` y `<password>` con el usuario SSH y la contraseña para el clúster:

> [!NOTE]
> Para buscar el FQDN para el clúster, visite Azure Portal y seleccione el clúster de HDInsight. Desde la sección __Información general__, el FQDN es parte de la entrada __Dirección URL__. Basta con que quite el `https://` del principio del valor.
>
> ![Captura de pantalla de la información general del clúster HDInsight con la entrada de dirección URL resaltada](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute, ComputeTarget

try:
    # Attaches a HDInsight cluster as a compute target.
    attach_config = HDInsightCompute.attach_configuration(address = "fqdn-or-ipaddress",
                                                          ssh_port = 22,
                                                          username = "username",
                                                          password = None, #if using ssh key
                                                          private_key_file = "path-to-key-file",
                                                          private_key_phrase = "key-phrase")
    compute = ComputeTarget.attach(ws, "myhdi", attach_config)
except UserErrorException as e:
    print("Caught = {}".format(e.message))
    print("Compute config already attached.")

# Configure HDInsight run
# load the runconfig object from the "myhdi.runconfig" file generated by the attach operaton above.
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# ask system to prepare the conda environment automatically when used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-run"></a>Envío de una ejecución de entrenamiento

Hay dos maneras de enviar una serie de entrenamientos:

* Enviar un objeto `ScriptRunConfig`
* Enviar un objeto `Pipeline`

> [!IMPORTANT]
> Los destinos de proceso Azure Databricks y Azure Datalake Analytics solo se pueden usar en una canalización.
> No se puede usar el destino de proceso local en una canalización.

### <a name="submit-using-scriptrunconfig"></a>Enviar mediante `ScriptRunConfig`

El patrón de código para enviar una serie de entrenamientos mediante `ScriptRunConfig` es el mismo con independencia del destino de proceso:

* Cree un `ScriptRunConfig` objeto con la configuración de ejecución para el destino de proceso.
* Envíe la ejecución.
* Espere a que la ejecución se complete.

El ejemplo siguiente usa la configuración para el destino de proceso local administrado por el sistema que creó anteriormente en este documento:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```


### <a name="submit-using-a-pipeline"></a>Envío mediante una canalización

El patrón de código para enviar una serie de entrenamientos mediante una canalización es el mismo con independencia del destino de proceso:

* Agregue un paso a la canalización para el recurso de proceso.
* Envíe una ejecución mediante la canalización.
* Espere a que la ejecución se complete.

En el ejemplo siguiente se usa el destino de proceso de Azure Databricks que creó anteriormente en este documento:

```python
dbStep = DatabricksStep(
    name="databricksmodule",
    inputs=[step_1_input],
    outputs=[step_1_output],
    num_workers=1,
    notebook_path=notebook_path,
    notebook_params={'myparam': 'testparam'},
    run_name='demo run name',
    databricks_compute=databricks_compute,
    allow_reuse=False
)
# list of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Para más información sobre las canalizaciones de aprendizaje automático, consulte el documento [Canalizaciones y Azure Machine Learning](concept-ml-pipelines.md).

Para ver cuadernos de Jupyter de ejemplo que muestran el entrenamiento mediante una canalización, consulte [https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="view-and-set-up-compute-using-the-azure-portal"></a>Visualización y configuración del proceso mediante Azure Portal

Desde Azure Portal puede ver qué destinos de proceso están asociados con el área de trabajo. Para obtener la lista, siga estos pasos:

1. Visite [Azure Portal](https://portal.azure.com) y vaya a su área de trabajo.
2. Haga clic en el vínculo __Proceso__ en la sección __Aplicaciones__.

    ![Pestaña en la que se ve el proceso](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Creación de un destino de proceso

Siga los pasos anteriores para ver la lista de destinos de proceso y, a continuación, siga estos pasos para crear un destino de proceso:

1. Haga clic en el signo __+__ para agregar un destino de proceso.

    ![Agregar proceso ](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Escriba el nombre del destino de proceso.
1. Seleccione **Proceso de Machine Learning** como tipo de proceso que se usará en __Entrenamiento__

    > [!IMPORTANT]
    > Solo puede crear una instancia de Azure Machine Learning Compute como proceso administrado para el entrenamiento

1. Rellene el formulario necesario, especialmente la familia de máquinas virtuales y el número máximo de nodos que se usan para poner en marcha el proceso 
1. Seleccione __Crear__
1. Para ver el estado de la operación de creación, seleccione el destino de proceso en la lista.

    ![Ver lista de procesos](./media/how-to-set-up-training-targets/View_list.png)

1. Verá los detalles del destino de proceso.

    ![Ver detalles](./media/how-to-set-up-training-targets/compute-target-details.png)

1. Ya puede enviar una ejecución en estos destinos como se ha indicado.


### <a name="reuse-existing-compute-in-your-workspace"></a>Reutilización de un proceso existente en el área de trabajo

Siga los pasos anteriores para ver la lista de destinos de proceso y, a continuación, siga estos pasos para reutilizar un destino de proceso:

1. Haga clic en el signo **+** para agregar un destino de proceso.
2. Escriba el nombre del destino de proceso.
3. Seleccione el tipo de proceso que va a adjuntar a __Entrenamiento__.

    > [!IMPORTANT]
    > No todos los tipos de proceso se pueden asociar mediante el portal.
    > Actualmente, los tipos que se pueden asociar para entrenamiento son:
    > 
    > * Máquina virtual remota
    > * Databricks
    > * Data Lake Analytics
    > * HDInsight

1. Rellene el formulario necesario

    > [!NOTE]
    > Microsoft recomienda que use claves SSH, ya que son más seguras que las contraseñas. Las contraseñas son vulnerables a los ataques por fuerza bruta, mientras que las claves SSH se basan en las firmas criptográficas. Para información acerca de cómo crear claves SSH para usarlas con Azure Virtual Machines, consulte los siguientes documentos:
    >
    > * [Creación y uso de claves SSH en Linux o macOS]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Creación y uso de claves SSH en Windows]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Seleccione Adjuntar
1. Para ver el estado de la operación de adjuntar, seleccione el destino de proceso en la lista.
1. Ya puede enviar una ejecución en estos destinos como se ha indicado.

## <a name="examples"></a>Ejemplos
Consulte los cuadernos de las siguientes ubicaciones:
* [how-to-use-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)

* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Referencia de SDK de Azure Machine Learning](https://aka.ms/aml-sdk)
* [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md)
* [Lugar de implementación de modelos](how-to-deploy-and-where.md)
* [Build machine learning pipelines with Azure Machine Learning service](concept-ml-pipelines.md) (Creación de canalizaciones de aprendizaje automático con el servicio Azure Machine Learning).
