---
title: Configuración de destinos de proceso para el entrenamiento de modelos con el servicio Azure Machine Learning | Microsoft Docs
description: Obtenga información acerca de cómo seleccionar y configurar los entornos de entrenamiento (destinos de proceso) utilizados para entrenar los modelos de aprendizaje automático. El servicio Azure Machine Learning le permite cambiar fácilmente los entornos de entrenamiento. Inicie el entrenamiento de forma local y, si necesita escalar horizontalmente, cambie a un destino de proceso en la nube.
services: machine-learning
author: heatherbshapiro
ms.author: hshapiro
ms.reviewer: larryfr
manager: cgronlun
ms.service: machine-learning
ms.component: core
ms.topic: article
ms.date: 09/24/2018
ms.openlocfilehash: 7eacc475145dac61db1717f1860e22cedd022262
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51231454"
---
# <a name="select-and-use-a-compute-target-to-train-your-model"></a>Selección y uso de un destino de proceso para entrenar el modelo

Con el servicio Azure Machine Learning, puede entrenar el modelo en distintos entornos. Estos entornos, llamados __destinos de proceso__, pueden ser locales o en la nube. En este documento, aprenderá sobre los destinos de proceso admitidos y cómo usarlos.

Un destino de proceso es el recurso que ejecuta el script de entrenamiento o que hospeda el modelo cuando se implementa como un servicio web. Se pueden crear y administrar mediante el SDK de Azure Machine Learning o la CLI. Si dispone de destinos de proceso creados por otro proceso (por ejemplo, Azure Portal o la CLI de Azure), para agregarlos, puede asociarlos al área de trabajo del servicio Azure Machine Learning.

Puede comenzar con ejecuciones locales en la máquina y, a continuación, escalar vertical y horizontalmente a otros entornos, como las instancias de Data Science Virtual Machine con GPU o Azure Batch AI. 

>[!NOTE]
> El código de este artículo se ha probado con el SDK de Azure Machine Learning versión 0.168. 

## <a name="supported-compute-targets"></a>Destinos de proceso admitidos

El servicio Azure Machine Learning admite los siguientes destinos de proceso:

|Destino de proceso| Aceleración de GPU | Ajuste automatizado de hiperparámetros | Selección automatizada del modelo | Se puede usar en las canalizaciones|
|----|:----:|:----:|:----:|:----:|
|[Equipo local](#local)| Es posible | &nbsp; | ✓ | &nbsp; |
|[Data Science Virtual Machine (DSVM)](#dsvm) | ✓ | ✓ | ✓ | ✓ |
|[Azure Batch AI](#batch)| ✓ | ✓ | ✓ | ✓ |
|[Azure Databricks](#databricks)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[Análisis con Azure Data Lake](#adla)| &nbsp; | &nbsp; | &nbsp; | ✓[*](#pipeline-only) |
|[HDInsight de Azure](#hdinsight)| &nbsp; | &nbsp; | &nbsp; | ✓ |

> [!IMPORTANT]
> <a id="pipeline-only"></a>*Azure Databricks y Azure Data Lake Analytics __solo__ se pueden usar en una canalización. Para más información sobre las canalizaciones, consulte el documento [Canalizaciones y Azure Machine Learning](concept-ml-pipelines.md).

__[Azure Container Instances (ACI)](#aci)__ también se puede usar para entrenar modelos. Es una oferta de nube sin servidor que es económica y cuya creación y uso resultan sencillos. ACI no admite la aceleración de GPU, el ajuste automatizado de hiperparámetros ni la selección automatizada de modelos. Además, no se puede usar en una canalización.

Los elementos diferenciadores clave entre los destinos de proceso son:
* __Aceleración de GPU__: las GPU están disponibles con Data Science Virtual Machine y Azure Batch AI. Puede tener acceso a una GPU en el equipo local, según el hardware, los controladores y los marcos de trabajo instalados.
* __Ajuste automatizado de hiperparámetros__: la optimización automatizada de hiperparámetros de Azure Machine Learning le ayuda a encontrar los mejores hiperparámetros para un modelo.
* __Selección automática del modelo__: el servicio Azure Machine Learning puede recomendar de forma inteligente la selección de algoritmos e hiperparámetros al generar un modelo. La selección automatizada del modelo le ayuda a converger en un modelo de alta calidad con más rapidez que si se prueban diferentes combinaciones de forma manual. Para más información, consulte [Tutorial: Automatically train a classification model with Azure Automated Machine Learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento automático de un modelo de clasificación con Azure Machine Learning).
* __Canalizaciones__: el servicio Azure Machine Learning permite combinar diferentes tareas, como el aprendizaje y la implementación, en una canalización. Las canalizaciones se pueden ejecutar en paralelo o en secuencia, y proporcionan un mecanismo de automatización confiable. Para más información, consulte el documento [Build machine learning pipelines with Azure Machine Learning service](concept-ml-pipelines.md) (Creación de canalizaciones de aprendizaje automático con el servicio Azure Machine Learning).

Puede usar el SDK de Azure Machine Learning, la CLI de Azure o Azure Portal para crear los destinos de proceso. También puede usar destinos de proceso existentes si los agrega (asocia) en el área de trabajo.

> [!IMPORTANT]
> No puede asociar una instancia existente de Azure Container Instances al área de trabajo. En su lugar, debe crear una nueva instancia.
>
> No se pueden crear recursos de Azure HDInsight, Azure Databricks o Azure Data Lake Store en un área de trabajo. En su lugar, debe crear el recurso y, luego, asociarlo al área de trabajo.

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

Cambiar de un destino de proceso a otro implica crear una [configuración de ejecución](concept-azure-machine-learning-architecture.md#run-configuration). La configuración de ejecución define cómo se ejecuta el script en el destino de proceso.

## <a name="training-scripts"></a>Scripts de entrenamiento

Cuando se inicia una ejecución de entrenamiento, se envía todo el directorio que contiene los scripts de entrenamiento. Se crea una instantánea y se envía al destino de proceso. Para más información, consulte las [instantáneas](concept-azure-machine-learning-architecture.md#snapshot).

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

Para ver una instancia de Jupyter Notebook en la que se muestra cómo se realiza el entrenamiento en un entorno administrado por el usuario, consulte [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).
  
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

Para ver una instancia de Jupyter Notebook en la que se muestra cómo se realiza el entrenamiento en un entorno administrado por el sistema, consulte [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local/02.train-on-local.ipynb).

## <a id="dsvm"></a>Data Science Virtual Machine

El equipo local puede no tener el proceso o los recursos de GPU necesarios para entrenar el modelo. En esta situación, puede escalar vertical u horizontalmente el proceso de entrenamiento si agrega más destinos de proceso, como Data Science Virtual Machines (DSVM).

> [!WARNING]
> Azure Machine Learning solo admite máquinas virtuales que ejecuten Ubuntu. Al crear una máquina virtual o seleccionar una existente, debe seleccionar una que use Ubuntu.

Los pasos siguientes utilizan el SDK para configurar una máquina virtual Data Science Virtual Machine (DSVM) como un destino de entrenamiento:

1. Crear o asociar una máquina virtual
    
    * Para crear una nueva máquina virtual DSVM, compruebe primero si tiene una con el mismo nombre; en caso contrario, cree una nueva máquina virtual:
    
        ```python
        from azureml.core.compute import DsvmCompute
        from azureml.core.compute_target import ComputeTargetException

        compute_target_name = 'mydsvm'

        try:
            dsvm_compute = DsvmCompute(workspace = ws, name = compute_target_name)
            print('found existing:', dsvm_compute.name)
        except ComputeTargetException:
            print('creating new.')
            dsvm_config = DsvmCompute.provisioning_configuration(vm_size = "Standard_D2_v2")
            dsvm_compute = DsvmCompute.create(ws, name = compute_target_name, provisioning_configuration = dsvm_config)
            dsvm_compute.wait_for_completion(show_output = True)
        ```
    * Para asociar una máquina virtual existente como un destino de proceso, debe proporcionar el nombre de dominio completo, el nombre de inicio de sesión y la contraseña de la máquina virtual.  En el ejemplo, reemplace ```<fqdn>``` con el nombre de dominio completo público de la máquina virtual o con la dirección IP pública. Reemplace ```<username>``` y ```<password>``` con el usuario SSH y la contraseña para la máquina virtual:

        ```python
        from azureml.core.compute import RemoteCompute

        dsvm_compute = RemoteCompute.attach(ws,
                                        name="attach-dsvm",
                                        username='<username>',
                                        address="<fqdn>",
                                        ssh_port=22,
                                        password="<password>")

        dsvm_compute.wait_for_completion(show_output=True)
    
   It takes around 5 minutes to create the DSVM instance.

1. Create a configuration for the DSVM compute target. Docker and conda are used to create and configure the training environment on DSVM:

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

1. Para eliminar los recursos de proceso cuando haya terminado, use el siguiente código:

    ```python
    dsvm_compute.delete()
    ```

Para ver una instancia de Jupyter Notebook en la que se muestra cómo se realiza el entrenamiento en una máquina virtual de Data Science Virtual Machine, consulte [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb).

## <a id="batch"></a>Azure Batch AI

Si tarda mucho tiempo en entrenar el modelo, puede usar Azure Batch AI para distribuir el entrenamiento en un clúster de recursos de proceso en la nube. Batch AI también puede configurarse para habilitar un recurso de la GPU.

En el ejemplo siguiente se busca en un clúster de Batch AI existente por su nombre. Si no se encuentra, se crea:

```python
from azureml.core.compute import BatchAiCompute
from azureml.core.compute import ComputeTarget
import os

# choose a name for your cluster
batchai_cluster_name = os.environ.get("BATCHAI_CLUSTER_NAME", ws.name + "gpu")
cluster_min_nodes = os.environ.get("BATCHAI_CLUSTER_MIN_NODES", 1)
cluster_max_nodes = os.environ.get("BATCHAI_CLUSTER_MAX_NODES", 3)
vm_size = os.environ.get("BATCHAI_CLUSTER_SKU", "STANDARD_NC6")
autoscale_enabled = os.environ.get("BATCHAI_CLUSTER_AUTOSCALE_ENABLED", True)


if batchai_cluster_name in ws.compute_targets():
    compute_target = ws.compute_targets()[batchai_cluster_name]
    if compute_target and type(compute_target) is BatchAiCompute:
        print('found compute target. just use it. ' + batchai_cluster_name)
else:
    print('creating a new compute target...')
    provisioning_config = BatchAiCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                vm_priority = 'lowpriority', # optional
                                                                autoscale_enabled = autoscale_enabled,
                                                                cluster_min_nodes = cluster_min_nodes, 
                                                                cluster_max_nodes = cluster_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, batchai_cluster_name, provisioning_config)
    
    # can poll for a minimum number of nodes and for a specific timeout. 
    # if no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current BatchAI cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

Para asociar un clúster existente de Batch AI como destino de proceso, debe proporcionar el identificador de recurso de Azure. Para obtener el identificador de recurso en Azure Portal, siga estos pasos:
1. Busque el servicio `Batch AI` en **Todos los servicios**.
1. Haga clic en el nombre del área de trabajo a la que pertenece el clúster.
1. Seleccione el clúster.
1. Haga clic en **Propiedades**.
1. Copie el **identificador**.

El ejemplo siguiente usa el SDK para asociar un clúster al área de trabajo. En el ejemplo, reemplace `<name>` con cualquier nombre para el proceso. No tiene que coincidir con el nombre del clúster. Reemplace `<resource-id>` por el identificador de recursos de Azure detallado anteriormente:

```python
from azureml.core.compute import BatchAiCompute
BatchAiCompute.attach(workspace=ws,
                      name=<name>,
                      resource_id=<resource-id>)
```

También puede comprobar el estado del trabajo y el clúster de Batch AI con los siguientes comandos de la CLI de Azure:

- Compruebe el estado del clúster. Con `az batchai cluster list`, puede ver cuántos nodos se ejecutan.
- Compruebe el estado del trabajo. Con `az batchai job list`, puede ver cuántos trabajos se ejecutan.

Se tarda aproximadamente cinco minutos en crear el clúster de Batch AI.

Para ver una instancia de Jupyter Notebook en la que se muestra cómo se realiza el entrenamiento en un clúster de Batch AI, consulte [https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/training/03.train-hyperparameter-tune-deploy-with-tensorflow/03.train-hyperparameter-tune-deploy-with-tensorflow.ipynb).

## <a name='aci'></a>Azure Container Instances (ACI)

Azure Container Instances son contenedores aislados que tienen tiempos de inicio más rápidos y no requieren que el usuario administre las máquinas virtuales. El servicio Azure Machine Learning usa contenedores de Linux, que están disponibles en las regiones westus, eastus, westeurope, northeurope, westus2 y southeastasia. Para más información, consulte [Disponibilidad en regiones](https://docs.microsoft.com/azure/container-instances/container-instances-quotas#region-availability). 

El ejemplo siguiente muestra cómo usar el SDK para crear un destino de proceso de ACI y usarlo para entrenar un modelo: 

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

# create a new runconfig object
run_config = RunConfiguration()

# signal that you want to use ACI to run script.
run_config.target = "containerinstance"

# ACI container group is only supported in certain regions, which can be different than the region the Workspace is in.
run_config.container_instance.region = 'eastus'

# set the ACI CPU and Memory 
run_config.container_instance.cpu_cores = 1
run_config.container_instance.memory_gb = 2

# enable Docker 
run_config.environment.docker.enabled = True

# set Docker base image to the default CPU-based image
run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE

# use conda_dependencies.yml to create a conda environment in the Docker image
run_config.environment.python.user_managed_dependencies = False

# auto-prepare the Docker image when used for the first time (if it is not already prepared)
run_config.auto_prepare_environment = True

# specify CondaDependencies obj
run_config.environment.python.conda_dependencies = CondaDependencies.create(conda_packages=['scikit-learn'])
```

En la creación de un destino de proceso de ACI, puede tardarse desde unos segundos a algunos minutos.

Para ver una instancia de Jupyter Notebook en la que se muestra cómo se realiza el entrenamiento en Azure Container Instances, consulte [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci/03.train-on-aci.ipynb).

## <a id="databricks"></a>Azure Databricks

Azure Databricks es un entorno basado en Apache Spark de la nube de Azure. Se puede usar como destino de proceso al entrenar modelos con una canalización de Azure Machine Learning.

> [!IMPORTANT]
> Un destino de proceso de Azure Databricks solo se puede usar en una canalización de Machine Learning.
>
> Para usarlo para entrenar a su modelo, primero debe crear un área de trabajo de Azure Databricks. Para crear estos recursos, consulte el documento [Ejecución de un trabajo de Spark en Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Para asociar Azure Databricks como destino de proceso, debe usar el SDK de Azure Machine Learning y proporcionar la siguiente información:

* __Nombre de proceso__: el nombre que desea asignar a este recurso de proceso.
* __Id. de recurso__: el identificador de recurso del área de trabajo de Azure Databricks. El texto siguiente es un ejemplo del formato de este valor:

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.Databricks/workspaces/<databricks-workspace-name>
    ```

    > [!TIP]
    > Para obtener el identificador de recurso, use el siguiente comando de la CLI de Azure. Reemplace `<databricks-ws>` por el nombre del área de trabajo de Databricks:
    > ```azurecli-interactive
    > az resource list --name <databricks-ws> --query [].id
    > ```

* __Token de acceso__: el token de acceso usado para autenticarse en Azure Databricks. Para generar un token de acceso, consulte el documento [Autenticación](https://docs.azuredatabricks.net/api/latest/authentication.html).

El código siguiente muestra cómo asociar Azure Databricks como destino de proceso:

```python
databricks_compute_name = os.environ.get("AML_DATABRICKS_COMPUTE_NAME", "<databricks_compute_name>")
databricks_resource_id = os.environ.get("AML_DATABRICKS_RESOURCE_ID", "<databricks_resource_id>")
databricks_access_token = os.environ.get("AML_DATABRICKS_ACCESS_TOKEN", "<databricks_access_token>")

try:
    databricks_compute = ComputeTarget(workspace=ws, name=databricks_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('databricks_compute_name {}'.format(databricks_compute_name))
    print('databricks_resource_id {}'.format(databricks_resource_id))
    print('databricks_access_token {}'.format(databricks_access_token))
    databricks_compute = DatabricksCompute.attach(
             workspace=ws,
             name=databricks_compute_name,
             resource_id=databricks_resource_id,
             access_token=databricks_access_token
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
* __Id. de recurso__: el identificador de recurso de la cuenta de Data Lake Analytics. El texto siguiente es un ejemplo del formato de este valor:

    ```text
    /subscriptions/<your_subscription>/resourceGroups/<resource-group-name>/providers/Microsoft.DataLakeAnalytics/accounts/<datalakeanalytics-name>
    ```

    > [!TIP]
    > Para obtener el identificador de recurso, use el siguiente comando de la CLI de Azure. Reemplace `<datalakeanalytics>` por el nombre de su cuenta de Data Lake Analytics:
    > ```azurecli-interactive
    > az resource list --name <datalakeanalytics> --query [].id
    > ```

El código siguiente muestra cómo asociar Data Lake Analytics como destino de proceso:

```python
adla_compute_name = os.environ.get("AML_ADLA_COMPUTE_NAME", "<adla_compute_name>")
adla_resource_id = os.environ.get("AML_ADLA_RESOURCE_ID", "<adla_resource_id>")

try:
    adla_compute = ComputeTarget(workspace=ws, name=adla_compute_name)
    print('Compute target already exists')
except ComputeTargetException:
    print('compute not found')
    print('adla_compute_name {}'.format(adla_compute_name))
    print('adla_resource_id {}'.format(adla_resource_id))
    adla_compute = AdlaCompute.attach(
             workspace=ws,
             name=adla_compute_name,
             resource_id=adla_resource_id
         )
    
    adla_compute.wait_for_completion(True)
```

> [!TIP]
> Las canalizaciones de Azure Machine Learning solo pueden trabajar con datos almacenados en el almacén de datos predeterminado de la cuenta de Data Lake Analytics. Si los datos con los que necesita trabajar están en un almacén no predeterminado, puede usar [`DataTransferStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.data_transfer_step.datatransferstep?view=azure-ml-py) para copiar los datos antes del entrenamiento.

## <a id="hdinsight"></a>Asociación de un clúster HDInsight 

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
from azureml.core.compute import HDInsightCompute

try:
    # Attaches a HDInsight cluster as a compute target.
    HDInsightCompute.attach(ws,name = "myhdi",
                            address = "<fqdn>",
                            username = "<username>",
                            password = "<password>")
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

Para ver una instancia de Jupyter Notebook en la que se muestra cómo se realiza el entrenamiento con Spark en HDInsight, consulte [https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark/05.train-in-spark.ipynb).

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

1. Escriba un nombre para el destino de proceso.
1. Seleccione el tipo de proceso que asociar a __Entrenamiento__. 

    > [!IMPORTANT]
    > No todos los tipos de proceso se pueden crear mediante Azure Portal. Actualmente, los tipos que se pueden crear para entrenamiento son:
    > 
    > * Máquina virtual
    > * Batch AI

1. Seleccione __Crear nuevo__ y rellene el formulario necesario. 
1. Seleccione __Crear__
1. Para ver el estado de la operación de creación, seleccione el destino de proceso en la lista.

    ![Ver la lista de procesos](./media/how-to-set-up-training-targets/View_list.png) Verá los detalles del destino de proceso.
    ![Ver detalles](./media/how-to-set-up-training-targets/vm_view.PNG)
1. Ahora puede enviar una ejecución en estos destinos como se ha detallado antes.

### <a name="reuse-existing-compute-in-your-workspace"></a>Reutilización de un proceso existente en el área de trabajo

Siga los pasos anteriores para ver la lista de destinos de proceso y, a continuación, siga estos pasos para reutilizar un destino de proceso:

1. Haga clic en el signo **+** para agregar un destino de proceso.
2. Escriba un nombre para el destino de proceso.
3. Seleccione el tipo de proceso que asociar a Entrenamiento.

    > [!IMPORTANT]
    > No todos los tipos de proceso se pueden asociar mediante el portal.
    > Actualmente, los tipos que se pueden asociar para entrenamiento son:
    > 
    > * Máquina virtual
    > * Batch AI

1. Seleccione 'Usar existente'.
    - Al asociar los clústeres de Batch AI, seleccione el destino de proceso en la lista desplegable, seleccione el área de trabajo de Batch AI y el clúster de Batch AI, y, a continuación, haga clic en **Crear**.
    - Al conectar una máquina virtual, escriba la dirección IP, la combinación de nombre de usuario y contraseña, las claves pública y privada, y el puerto, y haga clic en Crear.

    > [!NOTE]
    > Microsoft recomienda que use claves SSH, ya que son más seguras que las contraseñas. Las contraseñas son vulnerables a los ataques por fuerza bruta, mientras que las claves SSH se basan en las firmas criptográficas. Para información acerca de cómo crear claves SSH para usarlas con Azure Virtual Machines, consulte los siguientes documentos:
    >
    > * [Creación y uso de claves SSH en Linux o macOS]( https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys)
    > * [Creación y uso de claves SSH en Windows]( https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)

5. Para ver el estado de aprovisionamiento, seleccione el destino de proceso de la lista.
6. Ahora puede enviar una ejecución en estos destinos.

## <a name="examples"></a>Ejemplos
Los cuadernos siguientes muestran los conceptos de este artículo:
* [01.getting-started/02.train-on-local/02.train-on-local.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/02.train-on-local)
* [01.getting-started/04.train-on-remote-vm/04.train-on-remote-vm.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/04.train-on-remote-vm)
* [01.getting-started/03.train-on-aci/03.train-on-aci.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/03.train-on-aci)
* [01.getting-started/05.train-in-spark/05.train-in-spark.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/01.getting-started/05.train-in-spark)
* [tutorials/01.train-models.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/01.train-models.ipynb)

Obtenga estos cuadernos: [!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Referencia de SDK de Azure Machine Learning](https://aka.ms/aml-sdk)
* [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md)
* [Lugar de implementación de modelos](how-to-deploy-and-where.md)
* [Build machine learning pipelines with Azure Machine Learning service](concept-ml-pipelines.md) (Creación de canalizaciones de aprendizaje automático con el servicio Azure Machine Learning).
