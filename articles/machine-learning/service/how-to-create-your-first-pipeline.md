---
title: Crear, ejecutar y realizar un seguimiento de las canalizaciones de ML
titleSuffix: Azure Machine Learning service
description: Cree y ejecute una canalización de aprendizaje automático con el SDK de Azure Machine Learning para Python. Las canalizaciones se usan para crear y administrar flujos de trabajo que unen las fases de aprendizaje automático (ML). Estas fases incluyen preparación de los datos, entrenamiento del modelo, implementación de modelo e inferencia.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sanpil
author: sanpil
ms.date: 01/08/2019
ms.custom: seodec18
ms.openlocfilehash: 8fe8b365974086ef530b83988c63eda338a6079f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58014580"
---
# <a name="create-and-run-a-machine-learning-pipeline-by-using-azure-machine-learning-sdk"></a>Crear y ejecutar una canalización de aprendizaje automático con el SDK de Azure Machine Learning

En este artículo aprenderá a crear, publicar, ejecutar y realizar un seguimiento de una [canalización de aprendizaje automático](concept-ml-pipelines.md) mediante el [SDK de Azure Machine Learning](https://aka.ms/aml-sdk).  Estas canalizaciones se usan para crear y administrar flujos de trabajo que unen varias fases de aprendizaje automático. Cada fase de una canalización, como la preparación de datos y el entrenamiento de modelos, puede incluir uno o más pasos.

Las canalizaciones que cree serán visibles para los miembros de su [área de trabajo](how-to-manage-workspace.md) de Azure Machine Learning Service. 

Recuerde que las canalizaciones usan destinos de proceso remotos para el cálculo y el almacenamiento de los datos intermedios y finales asociados a esa canalización. Asimismo, las canalizaciones pueden leer y escribir datos en y desde las ubicaciones de[Azure Storage](https://docs.microsoft.com/azure/storage/).

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Requisitos previos

* [Configure un entorno de desarrollo](how-to-configure-environment.md) para instalar el SDK de Azure Machine Learning.

* Cree un [área de trabajo de Azure Machine Learning](how-to-configure-environment.md#workspace) que contendrá todos los recursos de la canalización. 

  ```python
  ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
  ```

## <a name="set-up-machine-learning-resources"></a>Configurar los recursos de aprendizaje automático

Cree los recursos necesarios para ejecutar una canalización:

* Configure un almacén de datos que puede usar para obtener acceso a los datos necesarios en los pasos de la canalización.

* Configure un objeto `DataReference` para que apunte a los datos que se guardan o a los que puede acceder en un almacén de datos.

* Configure los [destinos de proceso](concept-azure-machine-learning-architecture.md#compute-target) en los que se ejecutarán los pasos de su canalización.

### <a name="set-up-a-datastore"></a>Configurar un almacén de datos
Un almacén de datos almacena los datos a los que la canalización puede obtener acceso. Cada área de trabajo tiene un almacén de datos predeterminado. Asimismo, puede registrar almacenes de datos adicionales. 

Cuando cree su área de trabajo, una instancia de [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) y una de [Azure Blob Storage ](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction) se adjuntan al área de trabajo de forma predeterminada. Azure Files es el almacén de datos predeterminado para un área de trabajo, pero también puede usar Blob Storage como un almacén de datos. Para obtener más información, consulte [Decisión sobre cuándo usar Azure Files, Azure Blobs o Azure Disks](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks). 

```python
# Default datastore (Azure file storage)
def_data_store = ws.get_default_datastore() 

# The above call is equivalent to this 
def_data_store = Datastore(ws, "workspacefilestore")

# Get blob storage associated with the workspace
def_blob_store = Datastore(ws, "workspaceblobstore")
```

Cargue archivos de datos o directorios en el almacén de datos para que pueda obtener acceso a ellos desde sus canalizaciones. En este ejemplo se usa la versión de Blob Storage del almacén de datos:

```python
def_blob_store.upload_files(
    ["./data/20news.pkl"],
    target_path="20newsgroups", 
    overwrite=True)
```

Una canalización consta de uno o varios pasos. Un paso es una unidad que se ejecuta en un destino de proceso. Los pasos pueden consumir orígenes de datos y producir datos "intermedios". Igualmente, un paso puede crear datos como un modelo, un directorio con archivos dependientes o datos temporales. Estos datos están disponibles para otros pasos más adelante en la canalización.

### <a name="configure-data-reference"></a>Configurar la referencia de datos

Acaba de crear un origen de datos al que se puede hacer referencia en una canalización como una entrada a un paso. Un objeto [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference) se encarga de representar un origen de datos en una canalización. El objeto `DataReference` apunta a los datos que están en o que son accesibles desde un almacén de datos.

```python
blob_input_data = DataReference(
    datastore=def_blob_store,
    data_reference_name="test_data",
    path_on_datastore="20newsgroups/20news.pkl")
```

Los datos intermedios (o salida de un paso) se representan mediante un objeto [PipelineData](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipelinedata?view=azure-ml-py). `output_data1` se crea como la salida de un paso y se usa como la entrada de uno o más pasos futuros. `PipelineData` introduce una dependencia de datos entre los pasos y crea un orden de ejecución implícito en la canalización.

```python
output_data1 = PipelineData(
    "output_data1",
    datastore=def_blob_store,
    output_name="output_data1")
```

## <a name="set-up-compute-target"></a>Configurar el destino de proceso

En Azure Machine Learning, el término __proceso__ (o __destino de proceso__) se refiere a las máquinas o clústeres que realizarán los pasos del cálculo en su canal de aprendizaje automático.   Consulte los [destinos de proceso del entrenamiento del modelo](how-to-set-up-training-targets.md) para obtener una lista completa de destinos de proceso y cómo crearlos y adjuntarlos a su área de trabajo.  El proceso para crear o adjuntar un destino de proceso es el mismo independientemente de si entrena un modelo o ejecuta un paso de la canalización. Después de crear y adjuntar el destino de proceso, utilice el objeto `ComputeTarget` en su [paso de canalización](#steps).

> [!IMPORTANT]
> No se admite la realización de operaciones de administración en destinos de proceso desde dentro de trabajos remotos. Puesto que las canalizaciones de aprendizaje automático se envían como un trabajo remoto, no use operaciones de administración en destinos de proceso desde dentro de la canalización.

A continuación se muestran ejemplos de creación y asociación de los destinos de proceso para:

* Proceso de Azure Machine Learning
* Azure Databricks 
* Análisis con Azure Data Lake

### <a name="azure-machine-learning-compute"></a>Proceso de Azure Machine Learning

Puede crear un proceso de Azure Machine Learning para ejecutar sus pasos.

```python
compute_name = "aml-compute"
 if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target: ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(vm_size = vm_size, # NC6 is GPU-enabled
                                                                min_nodes = 1, 
                                                                max_nodes = 4)
     # create the compute target
    compute_target = ComputeTarget.create(ws, compute_name, provisioning_config)
    
    # Can poll for a minimum number of nodes and for a specific timeout. 
    # If no min node count is provided it will use the scale settings for the cluster
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
    
     # For a more detailed view of current cluster status, use the 'status' property    
    print(compute_target.status.serialize())
```

### <a id="databricks"></a>Azure Databricks

Azure Databricks es un entorno basado en Apache Spark de la nube de Azure. Se puede usar como destino de proceso con una canalización de Azure Machine Learning.

Cree un área de trabajo de Azure Databricks antes de usarlo. Para crear estos recursos, consulte el documento [Ejecución de un trabajo de Spark en Azure Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal).

Para adjuntar Azure Databricks como destino de proceso, proporcione la información siguiente:

* __Nombre del proceso de Databricks__: el nombre que desea asignar a este recurso de proceso.
* __Nombre de área de trabajo de Databricks__: el nombre del área de trabajo de Azure Databricks.
* __Token de acceso de Databricks__: el token de acceso usado para autenticarse en Azure Databricks. Para generar un token de acceso, consulte el documento [Autenticación](https://docs.azuredatabricks.net/api/latest/authentication.html).

El código siguiente muestra cómo asociar Azure Databricks como destino de proceso con el SDK de Azure Machine Learning:

```python
import os
from azureml.core.compute import ComputeTarget, DatabricksCompute
from azureml.exceptions import ComputeTargetException

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
### <a id="adla"></a>Azure Data Lake Analytics

Azure Data Lake Analytics es una plataforma de análisis de macrodatos de la nube de Azure. Se puede usar como destino de proceso con una canalización de Azure Machine Learning.

Cree una cuenta de Azure Data Lake Analytics antes de usarlo. Para crear este recurso, consulte la [Introducción a Azure Data Lake Analytics](https://docs.microsoft.com/azure/data-lake-analytics/data-lake-analytics-get-started-portal) documento.

Para asociar Data Lake Analytics como destino de proceso, debe usar el SDK de Azure Machine Learning y proporcionar la siguiente información:

* __Nombre de proceso__: el nombre que desea asignar a este recurso de proceso.
* __Grupo de recursos__: el grupo de recursos que contiene la cuenta de Data Lake Analytics.
* __Nombre de cuenta__: El nombre de la cuenta de Data Lake Analytics.

El código siguiente muestra cómo asociar Data Lake Analytics como destino de proceso:

```python
import os
from azureml.core.compute import ComputeTarget, AdlaCompute
from azureml.exceptions import ComputeTargetException


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

## <a id="steps"></a>Construir los pasos de la canalización

Después de crear y adjuntar un destino de proceso al área de trabajo, está listo para definir un paso de la canalización. Hay muchos pasos integrados disponibles a través del SDK de Azure Machine Learning. Es la más básica de estos pasos una [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py), que ejecuta un script de Python en un destino de proceso especificado:

```python
trainStep = PythonScriptStep(
    script_name="train.py",
    arguments=["--input", blob_input_data, "--output", processed_data1],
    inputs=[blob_input_data],
    outputs=[processed_data1],
    compute_target=compute_target,
    source_directory=project_folder
)
```

Después de definir sus pasos, debe compilar la canalización mediante algunos o todos ellos.

>[!NOTE]
>No se carga ningún archivo o dato en Azure Machine Learning Service cuando define los pasos o compila la canalización.

```python
# list of steps to run
compareModels = [trainStep, extractStep, compareStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=[compareModels])
```

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
    compute_target=databricks_compute,
    allow_reuse=False
)
# List of steps to run
steps = [dbStep]

# Build the pipeline
pipeline1 = Pipeline(workspace=ws, steps=steps)
```

Para obtener más información, consulte el [pasos de la canalización de azure paquete](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py) y [canalización clase](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.pipeline%28class%29?view=azure-ml-py) referencia.

## <a name="submit-the-pipeline"></a>Enviar la canalización

Cuando envía la canalización, Azure Machine Learning Service comprueba las dependencias para cada paso y carga una instantánea del directorio de origen especificado. Si no se especifica ningún directorio de origen, se carga el directorio local actual.


```python
# Submit the pipeline to be run
pipeline_run1 = Experiment(ws, 'Compare_Models_Exp').submit(pipeline1)
pipeline_run1.wait_for_completion()
```

Cuando se ejecuta por primera vez una canalización, Azure Machine Learning:

* Descarga la instantánea del proyecto en el destino de proceso de la instancia de Blob Storage asociada al área de trabajo.
* Crea una imagen de docker correspondiente a cada paso en la canalización.
* Descarga la imagen de docker para cada paso en el destino de proceso del registro de contenedor.
* Monta el almacén de datos, si se especifica un objeto `DataReference`. Si no se admite el montaje, los datos se copian al destino de proceso.
* Ejecuta el paso en el destino de proceso especificado en la definición del paso. 
* Crea artefactos como registros, las propiedades stdout y stderr, métricas y resultados que especifica el paso. Estos artefactos se cargan y se guardan en el almacén de datos predeterminado del usuario.

![Diagrama de ejecución de un experimento como una canalización](./media/how-to-create-your-first-pipeline/run_an_experiment_as_a_pipeline.png)

Para obtener más información, consulte el [experimentar clase](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) referencia.

## <a name="publish-a-pipeline"></a>Publicar una canalización

Puede publicar una canalización para ejecutarla con diferentes entradas más adelante. Para que el punto de conexión REST de una canalización ya publicada acepte los parámetros, debe parametrizar la canalización antes de publicarla. 

1. Para crear un parámetro de canalización, use un objeto [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py) con un valor predeterminado.

   ```python
   pipeline_param = PipelineParameter(
     name="pipeline_arg", 
     default_value=10)
   ```

2. Agregue este objeto `PipelineParameter` como parámetro a cualquiera de los pasos de la canalización tal como se muestra a continuación:

   ```python
   compareStep = PythonScriptStep(
     script_name="compare.py",
     arguments=["--comp_data1", comp_data1, "--comp_data2", comp_data2, "--output_data", out_data3, "--param1", pipeline_param],
     inputs=[ comp_data1, comp_data2],
     outputs=[out_data3],    
     target=compute_target, 
     source_directory=project_folder)
   ```

3. Publique esta canalización para que acepte un parámetro cuando se invoque.

   ```python
   published_pipeline1 = pipeline1.publish(
       name="My_Published_Pipeline", 
       description="My Published Pipeline Description")
   ```

## <a name="run-a-published-pipeline"></a>Ejecutar una canalización publicada

Todas las canalizaciones publicadas tienen un punto de conexión REST. Este punto de conexión invoca la ejecución de la canalización desde sistemas externos, como los clientes que no son de Python. Este punto de conexión habilita la "repetibilidad administrada" en los escenarios de puntuación y nuevo entrenamiento.

Para invocar la ejecución de la canalización anterior, necesitará un token de encabezado de autenticación de Azure Active Directory, tal como se describe en la [clase AzureCliAuthentication](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py) u obtener más detalles en el notebook [Autenticación en Azure Machine Learning](https://aka.ms/pl-restep-auth).

```python
response = requests.post(published_pipeline1.endpoint, 
    headers=aad_token, 
    json={"ExperimentName": "My_Pipeline",
        "ParameterAssignments": {"pipeline_arg": 20}})
```
## <a name="view-results"></a>Visualización de los resultados

Ver la lista de todas las canalizaciones y sus detalles de ejecución:
1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).  

1. [Ver el área de trabajo](how-to-manage-workspace.md#view-a-workspace) para encontrar la lista de canalizaciones.
 ![lista de canalizaciones de Machine Learning](./media/how-to-create-your-first-pipeline/list_of_pipelines.png)
 
1. Seleccione una canalización específica para ver los resultados de la ejecución.

## <a name="next-steps"></a>Pasos siguientes
- Use [estos cuadernos de Jupyter en GitHub](https://aka.ms/aml-pipeline-readme) para explorar aún más canalizaciones de Machine Learning.
- Lea la ayuda de referencia del SDK para el paquete [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) y el paquete [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py).

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]
