---
title: Destinos de proceso del entrenamiento del modelo
titleSuffix: Azure Machine Learning service
description: Configurar los entornos de entrenamiento (destinos de proceso) del entrenamiento del modelo de Machine Learning. Es fácil cambiar entre entornos de entrenamiento. Inicie el entrenamiento de forma local. Si necesita escalar horizontalmente, cambie a un destino de proceso basado en la nube.
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
ms.openlocfilehash: 9037f6d7602f186bc30e55acbc050280bca134ee
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53794471"
---
# <a name="set-up-compute-targets-for-model-training"></a>Configuración de destinos de proceso del entrenamiento del modelo

Con el servicio Azure Machine Learning, puede entrenar un modelo en distintos recursos de proceso. Dichos recursos, denominados __destinos de proceso__, pueden ser locales o estar en la nube. En este artículo, aprenderá sobre los destinos de proceso admitidos y cómo usarlos.

Un destino de proceso es un recurso en el que se ejecuta el script de entrenamiento o en el que se hospeda el modelo cuando se implementa como servicio web. Los destinos de proceso se pueden crear y administrar mediante el SDK de Azure Machine Learning, Azure Portal o la CLI de Azure. Si tiene destinos de proceso creados mediante cualquier otro servicio (por ejemplo, un clúster de Azure HDInsight), puede usarlos si los adjunta al área de trabajo de Azure Machine Learning Service.

Azure Machine Learning admite tres amplias categorías de destinos de proceso:

* __Local__: el equipo local o una máquina virtual (VM) basada en la nube que se usen como entorno de desarrollo y experimentación. 
* __Proceso administrado__: Proceso de Azure Machine Learning es una oferta de proceso administrado por Azure Machine Learning Service. La oferta permite crear fácilmente procesos de uno o varios nodos para el entrenamiento, la realización de pruebas y la inferencia de lotes.
* __Proceso adjunto__: también puede traer su propio proceso en la nube de Azure y adjuntarlo a Azure Machine Learning. Encontrará más información sobre los tipos de proceso admitidos y cómo usarlos en las secciones siguientes.


## <a name="supported-compute-targets"></a>Destinos de proceso admitidos

El servicio Azure Machine Learning tiene distintas modalidades de soporte técnico en los distintos destinos de proceso. Un ciclo de vida de desarrollo de modelos típico comienza con el desarrollo y la experimentación en una pequeña cantidad de datos. En esta etapa, se recomienda usar un entorno local, como el equipo local o una máquina virtual basada en la nube. Si escala verticalmente su entrenamiento a conjuntos de datos mayores o realiza un entrenamiento distribuido, use un entorno de Proceso de Azure Machine Learning para crear un clúster con uno o varios nodos que se escale automáticamente cada vez que se envíe una ejecución. También puede adjuntar su propio recurso de proceso, aunque el soporte técnico para varios escenarios puede variar como se describe en la tabla siguiente:

|Destino de proceso| Aceleración de GPU | Automatizado<br/> ajuste de hiperparámetros | Automatizado</br> aprendizaje automático | Canalización descriptiva|
|----|:----:|:----:|:----:|:----:|
|[Equipo local](#local)| Es posible | &nbsp; | ✓ | &nbsp; |
|[Proceso de Azure Machine Learning](#amlcompute)| ✓ | ✓ | ✓ | ✓ |
|[Máquina virtual remota](#vm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | ✓ | ✓[*](#pipeline-only) |
|[Análisis con Azure Data Lake](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[HDInsight de Azure](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>__*___Azure Databricks y Azure Data Lake Analytics solo se pueden usar en una canalización._<br/>
> Para más información sobre las canalizaciones, consulte [Canalizaciones de Azure Machine Learning](concept-ml-pipelines.md).
>
> El entorno de Proceso de Azure Machine Learning se debe crear desde dentro de un área de trabajo. Las instancias existentes no se pueden adjuntar a un área de trabajo.
>
> Los restantes destinos de proceso se deben crear fuera de Azure Machine Learning y luego se adjuntan a su área de trabajo.
>
> Cuando se entrena un modelo, algunos destinos de proceso utilizan imágenes de contenedor de Docker. La imagen base de GPU debe usarse solo en Microsoft Azure Services. En el caso del entrenamiento de modelos, los servicios incluyen:
> * Proceso de Azure Machine Learning
> * Azure Kubernetes Service
> * Windows Data Science Virtual Machine (DSVM)

## <a name="workflow"></a>Flujo de trabajo

El flujo de trabajo para desarrollar e implementar un modelo con Azure Machine Learning sigue estos pasos:

1. Los scripts de entrenamiento del aprendizaje automático se desarrollan en Python.
1. Se crea y configura el destino de proceso o se adjunta uno existente.
1. Se envían los scripts de entrenamiento al destino de proceso.
1. Se inspeccionan los resultados para buscar el mejor modelo.
1. Se registra el modelo en el registro del modelo.
1. Se implementa el modelo.

> [!NOTE]
> El script de entrenamiento no está vinculado a un destino de proceso específico. Puede entrenarlo inicialmente en el equipo local y después cambiar los destinos de proceso sin tener que volver a escribir el script de entrenamiento.
> 
> Cada vez que asocie un destino de proceso a su área de trabajo, bien mediante la creación de un proceso administrado o la asociación de un proceso existente, especifique un nombre para el proceso. El nombre debe tener una longitud comprendida entre dos y 16 caracteres.

Para cambiar de un destino de proceso a otro, necesita una [configuración de ejecución](concept-azure-machine-learning-architecture.md#run-configuration). La configuración de ejecución define cómo se ejecuta el script en el destino de proceso.

## <a name="training-scripts"></a>Scripts de entrenamiento

Al empezar a ejecutar un entrenamiento, se crea una instantánea del directorio que contiene los scripts de entrenamiento y se envía al destino de proceso. Para más información, consulte [Instantánea](concept-azure-machine-learning-architecture.md#snapshot).

## <a id="local"></a>Equipo local

Al entrenar localmente, se usa el SDK para enviar la operación de entrenamiento. Puede entrenar con un entorno administrado por el usuario o por el sistema.

### <a name="user-managed-environment"></a>Entorno administrado por el usuario

En un entorno administrado por el usuario, asegúrese de que todos los paquetes necesarios están disponibles en el entorno de Python donde se ejecuta el script. El fragmento de código siguiente es un ejemplo de cómo configurar el entrenamiento de un entorno administrado por el usuario:

```python
from azureml.core.runconfig import RunConfiguration

# Edit a run configuration property on the fly.
run_config_user_managed = RunConfiguration()

run_config_user_managed.environment.python.user_managed_dependencies = True

# Choose a specific Python environment by pointing to a Python path. For example:
# run_config.environment.python.interpreter_path = '/home/ninghai/miniconda3/envs/sdk2/bin/python'
```

  
### <a name="system-managed-environment"></a>Entorno administrado por el sistema

Los entornos administrados por el sistema dependen de Conda para administrar las dependencias. Conda crea un archivo denominado **conda_dependencies.yml** que contiene una lista de dependencias. Puede pedir al sistema que genere un nuevo entorno de Conda y ejecutar los scripts allí. Los entornos administrados por el sistema pueden reutilizarse más adelante, siempre y cuando el archivo conda_dependencies.yml permanezca sin cambios. 

La configuración inicial de un nuevo entorno puede tardar varios minutos en completarse, según el tamaño de las dependencias necesarias. El fragmento de código siguiente muestra cómo crear un entorno administrado por el sistema que dependa de scikit-learn:

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config_system_managed = RunConfiguration()

run_config_system_managed.environment.python.user_managed_dependencies = False
run_config_system_managed.auto_prepare_environment = True

# Specify the conda dependencies with scikit-learn

run_config_system_managed.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

## <a id="amlcompute"></a>Proceso de Azure Machine Learning

Proceso de Azure Machine Learning es una infraestructura de proceso administrado que permite al usuario crear fácilmente un proceso de uno o varios nodos. El proceso se crea dentro de la región de su área de trabajo y es un recurso que se puede compartir con otros usuarios del área de trabajo. El proceso se escala verticalmente de forma automática cuando se envía un trabajo y se puede colocar en una instancia de Azure Virtual Network. El proceso se ejecuta en un entorno con contenedores y empaqueta las dependencias del modelo en un contenedor de Docker.

Proceso Azure Machine Learning Compute para distribuir el proceso de entrenamiento en un clúster de nodos de proceso de CPU o GPU de la nube. Para más información sobre los tamaños de máquina virtual que incluyen GPU, consulte [Tamaños de máquinas virtuales optimizadas para GPU](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-gpu).

> [!NOTE]
> Proceso de Azure Machine Learning tiene límites predeterminados, como el número de núcleos que se pueden asignar. Para más información, consulte [Administración y solicitud de cuotas para recursos de Azure](https://docs.microsoft.com/azure/machine-learning/service/how-to-manage-quotas).

Puede crear un entorno de Proceso de Azure Machine Learning a petición al programar una ejecución, o bien como un recurso persistente.

### <a name="run-based-creation"></a>Creación basada en ejecución

Puede crear un entorno de Proceso de Azure Machine Learning como destino de proceso en tiempo de ejecución. El proceso se crea automáticamente para su ejecución y se escala verticalmente al número de **max_nodes** que especifique en la configuración de ejecución. El proceso se elimina automáticamente una vez completada la ejecución.

> [!IMPORTANT]
> La creación basada en la ejecución de un entorno de Proceso de Azure Machine Learning se encuentra actualmente en su versión preliminar. No use la creación basada en la ejecución si usa el ajuste de hiperparámetros o el aprendizaje automático automatizado. Para usar cualquiera de ellos, cree el entorno de Proceso de Azure Machine Learning antes de enviar una ejecución.

```python
from azureml.core.compute import ComputeTarget, AmlCompute

# First, list the supported VM families for Azure Machine Learning Compute
AmlCompute.supported_vmsizes()

from azureml.core.runconfig import RunConfiguration

# Create a new runconfig object
run_config = RunConfiguration()

# Signal that you want to use AmlCompute to execute the script
run_config.target = "amlcompute"

# AmlCompute is created in the same region as your workspace
# Set the VM size for AmlCompute from the list of supported_vmsizes
run_config.amlcompute.vm_size = 'STANDARD_D2_V2'

```

### <a name="persistent-compute-basic"></a>Proceso persistente: Básica

Un entorno de Proceso de Azure Machine Learning persistente se puede reutilizar en varios trabajos. El proceso se puede compartir con otros usuarios del área de trabajo y se conserva entre un trabajo y otro.

Para crear un recurso de entorno de Proceso de Azure Machine Learning persistente, especifique las propiedades **vm_size** y **max_nodes**. Azure Machine Learning usará valores predeterminados inteligentes para las demás propiedades. El proceso aplica la escalabilidad automática hasta cero nodos cuando no se usan y crea máquinas virtuales dedicadas para ejecutar los trabajos cuando es necesario. 

* **vm_size**: la familia máquina virtual de los nodos creados por el Proceso de Azure Machine Learning.
* **max_nodes**: el número máximo de nodos hasta los que se aumenta automáticamente cuando ejecuta un trabajo en Proceso de Azure Machine Learning.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that the cluster doesn't already exist
try:
    cpu_cluster = ComputeTarget(workspace=ws, name=cpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2',
                                                           max_nodes=4)
    cpu_cluster = ComputeTarget.create(ws, cpu_cluster_name, compute_config)

cpu_cluster.wait_for_completion(show_output=True)

```

### <a name="persistent-compute-advanced"></a>Proceso persistente: Avanzado

También puede configurar varias propiedades avanzadas cuando cree un entorno de Proceso de Azure Machine Learning. Estas propiedades permiten crear un clúster persistente de tamaño fijo o dentro de una instancia existente de Azure Virtual Network de su suscripción.

Además de las propiedades **vm_size** y **max_nodes**, también puede usar las siguientes propiedades:

* **min_nodes**: el número mínimo de nodos hasta el que se reduce cuando se ejecuta un trabajo en un entorno de Proceso de Azure Machine Learning. El valor mínimo predeterminado es cero (0) nodos.
* **vm_priority**: el tipo de máquina virtual que se debe usar al crear un recurso de entorno de Proceso de Azure Machine Learning. Elija entre **dedicated** (valor predeterminado) y **lowpriority**. Las máquinas virtuales de prioridad baja utilizan el exceso de capacidad de Azure. Estas máquinas virtuales son menos costosas, pero se pueden adelantar ejecuciones cuando se usen estas máquinas virtuales.
* **idle_seconds_before_scaledown**: La cantidad de tiempo de inactividad que se debe esperar después de que se complete una ejecución y antes de que se aplique la escalabilidad automática hasta el número de **min_nodes**. El tiempo de inactividad predeterminado es 120 segundos.
* **vnet_resourcegroup_name**: El grupo de recursos de la red virtual __existente__. El entorno de Proceso de Azure Machine Learning se crea dentro de esta red virtual.
* **vnet_name**: El nombre de la red virtual. La red virtual debe estar en la misma región que el área de trabajo de Azure Machine Learning.
* **subnet_name**: el nombre de una subred de la red virtual. A los recursos del entorno de Proceso de Azure Machine Learning se les asignan direcciones IP de este intervalo de subred.

> [!TIP]
> Cuando cree un recurso de entorno de Proceso de Azure Machine Learning persistente, puede actualizar las propiedades, como el número de **min_nodes** o **max_nodes**. Para actualizar una propiedad, llame a la función `update()` de la propiedad.

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
cpu_cluster_name = "cpucluster"

# Verify that the cluster doesn't already exist 
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

Azure Machine Learning también admite la posibilidad de que traiga su propio recurso de proceso y lo adjunte a su área de trabajo. Uno de estos tipos de recursos es una máquina virtual remota arbitraria, siempre que se pueda acceder desde Azure Machine Learning Service. El recurso puede ser una máquina virtual de Azure, un servidor remoto de la organización o local. En concreto, dada la dirección IP y las credenciales (nombre de usuario y contraseña o clave SSH), puede usar cualquier máquina virtual a la que se pueda acceder para las ejecuciones remotas.
Puede usar un entorno de Conda integrado en el sistema, un entorno de Python existente o un contenedor de Docker. Cuando ejecute con un contenedor de Docker, el motor de Docker debe estar en ejecución en la máquina virtual. La funcionalidad de máquina virtual remota es especialmente útil cuando se quiere un entorno de desarrollo y experimentación basado en la nube más flexible que el equipo local.

> [!TIP]
> Utilice la DSVM como la máquina virtual de Azure de su elección para este escenario. Esta máquina virtual es un entorno de desarrollo de inteligencia artificial y ciencia de datos preconfigurado de Azure. La máquina virtual ofrece un conjunto de herramientas y marcos protegidos durante todo el ciclo de vida del desarrollo de aprendizaje automático. Para obtener más información sobre cómo usar la DSVM con Azure Machine Learning, consulte [Configuración del entorno de desarrollo](https://docs.microsoft.com/azure/machine-learning/service/how-to-configure-environment#dsvm).

> [!WARNING]
> Azure Machine Learning solo admite máquinas virtuales que ejecuten Ubuntu. Al crear una máquina virtual o elegir una máquina virtual existente, debe seleccionar una máquina virtual que use Ubuntu.

Los pasos siguientes utilizan el SDK para configurar una DSVM como destino de entrenamiento:

1. Para asociar una máquina virtual existente como destino de proceso, debe proporcionar el nombre de dominio completo (FQDN), el nombre de usuario y la contraseña de la máquina virtual. En el ejemplo, reemplace \<fqdn> por el FQDN de la máquina virtual o por la dirección IP pública. Reemplace \<username> y \<password> por el nombre de usuario y contraseña de SSH para la máquina virtual.

    ```python
    from azureml.core.compute import RemoteCompute, ComputeTarget
    
    # Create the compute config
    attach_config = RemoteCompute.attach_configuration(address = "ipaddress",
                                                       ssh_port=22,
                                                       username='<username>',
                                                       password="<password>")

    # If you use SSH instead of a password, use this code:
    #                                                  ssh_port=22,
    #                                                  username='<username>',
    #                                                  password=None,
    #                                                  private_key_file="path-to-file",
    #                                                  private_key_passphrase="passphrase")

    # Attach the compute target
    compute = ComputeTarget.attach(ws, "attach-dsvm", attach_config)

    compute.wait_for_completion(show_output=True)
    ```

1. Cree una configuración para el destino de proceso de Data Science Virtual Machine. Docker y Conda se utilizan para crear y configurar el entorno de entrenamiento en la DSVM.

    ```python
    from azureml.core.runconfig import RunConfiguration
    from azureml.core.conda_dependencies import CondaDependencies

    # Load into memory the cpu-dsvm.runconfig file created in the previous attach operation
    run_config = RunConfiguration(framework = "python")

    # Set the compute target to the Linux DSVM
    run_config.target = compute_target_name

    # Use Docker in the remote VM
    run_config.environment.docker.enabled = True

    # Use the CPU base image
    # To use GPU in DSVM, you must also use the GPU base Docker image "azureml.core.runconfig.DEFAULT_GPU_IMAGE"
    run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE
    print('Base Docker image is:', run_config.environment.docker.base_image)

    # Ask the system to provision a new conda environment based on the conda_dependencies.yml file
    run_config.environment.python.user_managed_dependencies = False

    # Prepare the Docker and conda environment automatically when they're used for the first time
    run_config.prepare_environment = True

    # Specify the CondaDependencies object
    run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])

    ```

## <a id="databricks"></a>Azure Databricks

Azure Databricks es un entorno basado en Apache Spark de la nube de Azure. El entorno se puede usar como destino de proceso al entrenar modelos con una canalización de Azure Machine Learning.

> [!IMPORTANT]
> Un destino de proceso de Azure Databricks solo se puede usar en una canalización de Machine Learning.
>
> Debe crear un área de trabajo de Azure Databricks para usarla para entrenar el modelo. Para crear estos recursos, consulte [Ejecución de un trabajo de Spark en Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Para asociar Azure Databricks como destino de proceso, debe usar el SDK de Azure Machine Learning y proporcionar la siguiente información:

* __Nombre de proceso__: el nombre que se va a asignar a este recurso de proceso.
* __Nombre de área de trabajo de Databricks__: el nombre del área de trabajo de Azure Databricks.
* __Token de acceso__: el token de acceso usado para autenticarse en Azure Databricks. Para generar un token de acceso, consulte [Autenticación](https://docs.azuredatabricks.net/api/latest/authentication.html).

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

    # Create the attach config
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

Azure Data Lake Analytics es una plataforma de análisis de macrodatos de la nube de Azure. La plataforma se puede usar como destino de proceso al entrenar modelos con una canalización de Azure Machine Learning.

> [!IMPORTANT]
> Un destino de proceso de Azure Data Lake Analytics solo se puede usar en una canalización de Machine Learning.
>
> Debe crear una cuenta de Azure Data Lake Analytics para usarla para entrenar el modelo. Para crear este recurso, consulte [Introducción a Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal).

Para asociar Data Lake Analytics como destino de proceso, debe usar el SDK de Azure Machine Learning y proporcionar la siguiente información:

* __Nombre de proceso__: el nombre que se va a asignar a este recurso de proceso.
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
    
    # Create the attach config
    attach_config = AdlaCompute.attach_configuration(resource_group = adla_resource_group,
                                                     account_name = adla_account_name)
    # Attach the ADLA
    adla_compute = ComputeTarget.attach(
             ws,
             adla_compute_name,
             attach_config
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Las canalizaciones de Azure Machine Learning solo pueden funcionar con datos almacenados en el almacén de datos predeterminado de la cuenta de Data Lake Analytics. Si los datos que necesita están en un almacén distinto al predeterminado, puede usar una operación [DataTransferStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) para copiar los datos antes de entrenar el modelo.

## <a id="hdinsight"></a>Azure HDInsight 

Azure HDInsight es una plataforma popular para el análisis de macrodatos. La plataforma proporciona Apache Spark, que se puede usar para entrenar el modelo.

> [!IMPORTANT]
> Debe crear el clúster de HDInsight antes de usarlo para entrenar el modelo. Para crear un clúster de Spark en HDInsight, consulte [Creación de un clúster de Spark en HDInsight](https://docs.microsoft.com/azure/hdinsight/spark/apache-spark-jupyter-spark-sql).
>
> Al crear el clúster, debe especificar un nombre de usuario SSH y una contraseña. Anote estos valores, ya que los necesitará al usar HDInsight como destino de proceso.
>
> Una vez creado el clúster, tiene el FQDN \<clustername>.azurehdinsight.net, donde \<clustername> es el nombre que proporcionó para el clúster. Necesitará la dirección de FQDN (o la dirección IP pública del clúster) para usarla como destino de proceso.

Para configurar HDInsight como destino de proceso, debe proporcionar el FQDN, el nombre de usuario y la contraseña para el clúster de HDInsight. El ejemplo siguiente usa el SDK para asociar un clúster al área de trabajo. En el ejemplo, reemplace \<fqdn> por el FQDN público del clúster o por la dirección IP pública. Reemplace \<username> y \<password> por el nombre de usuario y contraseña de SSH para el clúster.

> [!NOTE]
> Para encontrar el FQDN del clúster, vaya Azure Portal y seleccione el clúster de HDInsight. En __Información general__, puede ver el FQDN en la entrada __URL__. Para obtener el FQDN, quite el prefijo https:\// del principio de la entrada.

![Obtener el FQDN de un clúster de HDInsight en Azure Portal](./media/how-to-set-up-training-targets/hdinsight-overview.png)

```python
from azureml.core.compute import HDInsightCompute, ComputeTarget

try:
    # Attach an HDInsight cluster as a compute target
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

# Configure the HDInsight run
# Load the runconfig object from the myhdi.runconfig file generated in the previous attach operation
run_config = RunConfiguration.load(project_object = project, run_config_name = 'myhdi')

# Ask the system to prepare the conda environment automatically when it's used for the first time
run_config.auto_prepare_environment = True
```

## <a name="submit-training-runs"></a>Enviar una ejecución de entrenamiento

Hay dos maneras de enviar una serie de entrenamientos:

* Enviar una ejecución de entrenamiento mediante un objeto `ScriptRunConfig`.
* Enviar una ejecución de entrenamiento mediante un objeto `Pipeline`.

> [!IMPORTANT]
> Los destinos de proceso Azure Databricks y Azure Datalake Analytics solo se pueden usar en una canalización.
>
> No se puede usar el destino de proceso local en una canalización.

### <a name="scriptrunconfig-object"></a>Objeto ScriptRunConfig

El patrón de código para enviar una ejecución de entrenamiento con el objeto `ScriptRunConfig` es el mismo para todos los tipos de destinos de proceso:

1. Cree un objeto `ScriptRunConfig` con la configuración de ejecución para el destino de proceso.
1. Envíe la ejecución.
1. Espere a que la ejecución se complete.

En el ejemplo siguiente se usa la configuración para el destino de proceso local administrado por el sistema que creó anteriormente:

```python
src = ScriptRunConfig(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
run = exp.submit(src)
run.wait_for_completion(show_output = True)
```


### <a name="pipeline-object"></a>Objeto de canalización

El patrón de código para enviar una ejecución de entrenamiento con un objeto `Pipeline` es el mismo para todos los tipos de destinos de proceso:

1. Agregue un paso al objeto `Pipeline` para el recurso de proceso.
1. Envíe una ejecución mediante la canalización.
1. Espere a que la ejecución se complete.

En el ejemplo siguiente se usa el destino de proceso de Azure Databricks que creó anteriormente:

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

# List of steps to run
steps = [dbStep]
pipeline = Pipeline(workspace=ws, steps=steps)
pipeline_run = Experiment(ws, 'Demo_experiment').submit(pipeline)
pipeline_run.wait_for_completion()
```

Para más información sobre las canalizaciones de Machine Learning, consulte [Canalizaciones y Azure Machine Learning](concept-ml-pipelines.md).

Para ver cuadernos de Jupyter de ejemplo que muestran cómo entrenar mediante una canalización, consulte [https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline](https://github.com/Azure/MachineLearningNotebooks/tree/master/pipeline).

## <a name="access-computes-in-the-azure-portal"></a>Acceso a procesos en Azure Portal

Desde Azure Portal, puede acceder a destinos de proceso que están asociados con el área de trabajo. 

### <a name="view-compute-targets"></a>Ver destinos de proceso

Para ver los destinos de proceso del área de trabajo, use los pasos siguientes:

1. Navegue hasta [Azure Portal](https://portal.azure.com) y abra el área de trabajo.
1. En __Aplicaciones__, seleccione __Proceso__.

    ![Ver destinos de proceso](./media/how-to-set-up-training-targets/azure-machine-learning-service-workspace.png)

### <a name="create-a-compute-target"></a>Creación de un destino de proceso

Siga los pasos anteriores para ver la lista de destinos de proceso. A continuación, siga estos pasos para crear un destino de proceso:

1. Haga clic en el signo más (+) para agregar un destino de proceso.

    ![Agregar un destino de proceso](./media/how-to-set-up-training-targets/add-compute-target.png)

1. Escriba un nombre para el destino de proceso.
1. Seleccione **Proceso de Machine Learning** como tipo de proceso que se usará en __Entrenamiento__.

    > [!IMPORTANT]
    > Solo puede crear un entorno de Proceso de Azure Machine Learning como recurso administrado para entrenamiento.

1. Rellene el formulario. Proporcione valores para las propiedades necesarias, especialmente para **Familia de máquinas virtuales** y **Nodos máximos** que se van a utilizar para acelerar el proceso. 
1. Seleccione __Crear__.
1. Vea el estado de la operación de creación seleccionando el destino de proceso en la lista:

    ![Seleccionar un destino de proceso para ver el estado de la operación de creación](./media/how-to-set-up-training-targets/View_list.png)

1. Verá los detalles del destino de proceso:

    ![Ver los detalles del destino de proceso](./media/how-to-set-up-training-targets/compute-target-details.png)

Ahora puede enviar una ejecución a los destinos de proceso, tal como se describió anteriormente.


### <a name="reuse-existing-compute-targets"></a>Reutilizar destinos de proceso existentes

Siga los pasos descritos previamente para ver la lista de destinos de proceso. A continuación, siga estos pasos para reutilizar un destino de proceso:

1. Haga clic en el signo más (+) para agregar un destino de proceso.
1. Escriba un nombre para el destino de proceso.
1. Seleccione el tipo de proceso que va a adjuntar para __Entrenamiento__:

    > [!IMPORTANT]
    > No todos los tipos de proceso se pueden adjuntar desde Azure Portal.
    > Los tipos de proceso que se pueden adjuntar actualmente para entrenamiento incluyen:
    >
    > * Una máquina virtual remota
    > * Azure Databricks
    > * Análisis con Azure Data Lake
    > * HDInsight de Azure

1. Rellene el formulario y proporcione valores para las propiedades necesarias.

    > [!NOTE]
    > Microsoft recomienda que use claves SSH, que son más seguras que las contraseñas. Las contraseñas son vulnerables a ataques por fuerza bruta. Las claves SSH se basan en las firmas criptográficas. Para información sobre cómo crear claves SSH para usarlas con Azure Virtual Machines, consulte los siguientes documentos:
    >
    > * [Creación y uso de claves SSH en Linux o macOS](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Creación y uso de claves SSH en Windows](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

1. Seleccione __Adjuntar__.
1. Consulte el estado de la operación de adjuntar seleccionando el destino de proceso en la lista.

Ahora puede enviar una ejecución a estos destinos de proceso, tal como se describió anteriormente.

## <a name="notebook-examples"></a>Ejemplos de cuadernos

Para obtener ejemplos, consulte los cuadernos de las siguientes ubicaciones:

* [how-to-use-azureml/training](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training)
* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Referencia de SDK de Azure Machine Learning](https://aka.ms/aml-sdk)
* [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md)
* [Lugar de implementación de modelos](how-to-deploy-and-where.md)
* [Build machine learning pipelines with Azure Machine Learning service](concept-ml-pipelines.md) (Creación de canalizaciones de aprendizaje automático con el servicio Azure Machine Learning).
