---
title: ¿Qué le va a suceder a Azure Batch AI? | Microsoft Docs
description: Obtenga información sobre lo que le va a suceder a Azure Batch AI y la opción de proceso de Azure Machine Learning Service.
services: batch-ai
author: garyericson
ms.service: batch-ai
ms.topic: overview
ms.date: 2/14/2019
ms.author: garye
ms.openlocfilehash: 87dcf18a2517561e3166726f8f1f1a70c2ec7713
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447810"
---
# <a name="whats-happening-to-azure-batch-ai"></a>¿Qué le va a suceder a Azure Batch AI?

**El servicio Azure Batch AI se va a retirar en marzo.** Las funciones de entrenamiento y puntuación a escala de Batch AI están ahora disponibles en el [Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md), que está disponible con carácter general desde el 4 de diciembre de 2018.

Junto con muchas otras funciones de aprendizaje automático, Azure Machine Learning Service incluye un destino de proceso administrado basado en la nube para el entrenamiento, la implementación y la puntuación de modelos de aprendizaje automático. Este destino de proceso se llama [Proceso de Machine Learning](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute). [Empezar a migrar y usarlo hoy](#migrate). Puede interactuar con Azure Machine Learning Service a través de sus [SDK de Python](../machine-learning/service/quickstart-create-workspace-with-python.md), la interfaz de línea de comandos y [Azure Portal](../machine-learning/service/quickstart-get-started.md).

## <a name="support-timeline"></a>Escala de tiempo del soporte técnico

| Date | Detalles de soporte del servicio Batch AI |
| ---- |-----------------|
| Diciembre&nbsp;14&#x2c;&nbsp;2018| Puede usar las suscripciones existentes de Azure Batch AI igual que antes. Sin embargo, no son posibles **nuevas suscripciones** y no se van a realizar nuevas inversiones.|
| Marzo&nbsp;31&#x2c;&nbsp;2019 | Tras esta fecha, las suscripciones existentes de Batch AI ya no funcionarán. |

## <a name="how-does-azure-machine-learning-service-compare"></a>Comparación con Azure Machine Learning Service
Se trata de un servicio en la nube que se usa para entrenar, implementar, automatizar y administrar modelos de aprendizaje automático, y todo ello con la gran capacidad de escalado que proporciona la nube. Conozca qué es [Azure Machine Learning Service en esta introducción](../machine-learning/service/overview-what-is-azure-ml.md).
 

En un ciclo de vida típico de desarrollo de un modelo se pueden distinguir las siguientes fases: preparación de datos, entrenamiento, experimentación e implementación. Este ciclo de un extremo a otro se puede orquestar mediante canalizaciones de Machine Learning.

![Diagrama de flujo](./media/overview-what-happened-batch-ai/lifecycle.png)


[Más información sobre cómo funciona el servicio y sus conceptos principales](../machine-learning/service/concept-azure-machine-learning-architecture.md). Muchos de los conceptos del flujo de trabajo de entrenamiento de modelos son similares a los conceptos existentes en Batch AI. 

En concreto, esta es una tabla de cómo debe considerarlos:
 
|Servicio Batch AI|  Servicio Azure Machine Learning|
|:--:|:---:|
|Área de trabajo|Área de trabajo|
|Clúster|   Proceso de tipo `AmlCompute`|
|Servidores de archivos|DataStores|
|Experimentos|Experimentos|
|Trabajos|Ejecuciones (permite ejecuciones anidadas)|
 
Esta es otra vista de la misma tabla que le ayudará a visualizar mejor las cosas:
 
**Jerarquía de Batch AI**
![Diagrama de flujo](./media/overview-what-happened-batch-ai/batchai-heirarchy.png) 
 
**Jerarquía de Azure Machine Learning Service**
![Diagrama de flujo](./media/overview-what-happened-batch-ai/azure-machine-learning-service-heirarchy.png) 

### <a name="platform-capabilities"></a>Funcionalidades de plataforma
Azure Machine Learning Service aporta un gran conjunto de nuevas funcionalidades, como una pila de entrenamiento -> implementación de un extremo a otro que puede usar para el desarrollo de inteligencia artificial sin tener que administrar ningún recurso de Azure. En esta tabla se compara la compatibilidad con las características para el entrenamiento entre los dos servicios.

|Característica|Servicio BatchAI|Servicio Azure Machine Learning|
|-------|:-------:|:-------:|
|Elección del tamaño de máquina virtual |CPU/GPU    |CPU/GPU. También admite FPGA para la realización de inferencias|
|Clúster preparado para inteligencia artificial (controladores, Docker, etc.)|  Sí |Sí|
|Preparación del nodo| Sí|    Sin |
|Elección de la familia de sistema operativo   |Parcial    |Sin |
|Máquinas virtuales dedicadas y de prioridad baja  |Sí    |Sí|
|Escalado automático   |Sí    |Sí (de forma predeterminada)|
|Tiempo de espera para la escalabilidad automática  |Sin  |Sí|
|SSH    |Sí|   Sí|
|Montaje de nivel del clúster |Sí (FileShares, Blobs, NFS, personalizado)   |Sí (montar o descargar el almacén de datos)|
|Entrenamiento distribuido|  Sí |Sí|
|Modo de ejecución del trabajo|    Máquina virtual o contenedor|    Contenedor|
|Imagen de contenedor personalizada|    Sí |Sí|
|Cualquier kit de herramientas    |Sí    |Sí (ejecutar script de Python)|
|JobPreparation|    Sí |Aún no|
|Montaje en el nivel de trabajo |Sí (FileShares, Blobs, NFS, personalizado)   |Sí (FileShares, Blobs)|
|Supervisión de trabajos     |Mediante GetJob|    Mediante el historial de ejecución (información más completa, entorno de ejecución personalizado para insertar más métricas)|
|Recuperar registros de trabajos y archivos o modelos |   Mediante ListFiles y API de Storage  |Mediante el servicio de artefacto|
 |Compatibilidad con Tensorboard   |Sin |    Sí|
|Cuotas de nivel de familia de máquinas virtuales |Sí    |Sí (con la capacidad anterior transmitida)|
 
Además de la tabla anterior, hay características en Azure Machine Learning Service que tradicionalmente no se admitían en BatchAI.

|Característica|Servicio BatchAI|Azure Machine Learning Service|
|-------|:-------:|:-------:|
|Preparación del entorno    |Sin  |Sí (preparación y carga de Conda en ACR)|
|Ajuste de hiperparámetros  |Sin |    Sí|
|Administración de modelos   |Sin  |Sí|
|Operacionalización e implementación| Sin   |Mediante AKS y ACI|
|Preparación de los datos   |Sin  |Sí|
|Destinos de proceso    |Máquinas virtuales de Azure  |Local, BatchAI (como AmlCompute), DataBricks, HDInsight|
|Automated Machine Learning |Sin |    Sí|
|Procesos  |Sin  |Sí|
|Puntuación por lotes  |Sí    |Sí|
|Compatibilidad con el portal o la CLI|    Sí |Sí|


### <a name="programming-interfaces"></a>Interfaces de programación

En esta tabla se presentan las diversas interfaces de programación disponibles para cada servicio.
    
|Característica|Servicio BatchAI|Azure Machine Learning Service|
|-------|:-------:|:-------:|
|SDK    |Java, C#, Python, Nodejs   |Python (basado en la configuración de ejecución y basado en el estimador para plataformas comunes)|
|CLI    |Sí    |Aún no|
|Azure Portal   |Sí    |Sí (excepto envío de trabajos)|
|API DE REST   |Sí    |Sí, pero distribuido entre microservicios|




## <a name="why-migrate"></a>¿Por qué migrar?

La actualización del servicio BatchAI en versión preliminar a Azure Machine Learning Service con disponibilidad general le proporciona una mejor experiencia gracias a conceptos que son más fáciles de usar, como estimadores y conjuntos de datos. También garantiza Acuerdos de Nivel de Servicio (SLA) de Azure de nivel de disponibilidad general y soporte técnico al cliente.

Azure Machine Learning Service también proporciona nueva funcionalidad, como aprendizaje automático automatizado, ajuste de hiperparámetros y canalizaciones de aprendizaje automático, que son útiles en cargas de trabajo de AI a gran escala. La posibilidad de poner en marcha un modelo entrenado sin cambiar a un servicio independiente ayuda a completar el bucle de ciencia de datos: desde la preparación de los datos (con el SDK de preparación de datos) hasta la puesta en marcha y la supervisión del modelo.

<a name="migrate"></a>

## <a name="how-do-i-migrate"></a>¿Cómo realizo una migración?

Antes de seguir los pasos de esta guía de migración que ayudan a asignar comandos entre los dos servicios, se recomienda dedicar algún tiempo a familiarizarse con Azure Machine Learning Service mediante su [documentación](../machine-learning/service/overview-what-is-azure-ml.md), lo que incluye el [tutorial en Python](../machine-learning/service/tutorial-train-models-with-aml.md).

Para evitar interrupciones en las aplicaciones y beneficiarse de las características más recientes, siga estos pasos antes del 31 de marzo de 2019:

1. Cree un área de trabajo de Azure Machine Learning Service y empiece:
    + [Inicio rápido basado en Python](../machine-learning/service/quickstart-create-workspace-with-python.md)
    + [Inicio rápido basado en Azure Portal](../machine-learning/service/quickstart-get-started.md)

1. Configure una instancia de [Proceso de Machine Learning](../machine-learning/service/how-to-set-up-training-targets.md#amlcompute) para entrenar el modelo.

1. Actualice los scripts para usar el Proceso de Machine Learning.


### <a name="sdk-migration"></a>Migración del SDK

La compatibilidad actual del SDK de Azure Machine Learning Service se logra mediante varios SDK de Python. El SDK principal se actualiza aproximadamente cada dos semanas y se puede instalar desde PyPi mediante el comando siguiente:

```python
pip install --upgrade azureml-sdk[notebooks]
```

Configure el entorno e instale el SDK mediante estas [instrucciones de inicio rápido](../machine-learning/service/quickstart-create-workspace-with-python.md#install-the-sdk)

Una vez que se abre un cuaderno de Jupyter con el kernel que apunta al entorno de Conda pertinente, así es como se asignan los comandos de los dos servicios:


#### <a name="create-a-workspace"></a>Crear un área de trabajo
El concepto de inicialización de un área de trabajo mediante un archivo configuration.json en BatchAI se corresponde básicamente con el uso de un archivo de configuración en Azure ML.

En **Batch AI**, se hizo de esta manera:

```python
sys.path.append('../../..')
import utilities as utils

cfg = utils.config.Configuration('../../configuration.json')
client = utils.config.create_batchai_client(cfg)

utils.config.create_resource_group(cfg)
_ = client.workspaces.create(cfg.resource_group, cfg.workspace, cfg.location).result()
```

**Azure Machine Learning Service**, pruebe:

```python
from azureml.core.workspace import Workspace

ws = Workspace.from_config()
print('Workspace name: ' + ws.name, 
      'Azure region: ' + ws.location, 
      'Subscription id: ' + ws.subscription_id, 
      'Resource group: ' + ws.resource_group, sep = '\n')
```

Además, también puede crear un área de trabajo directamente si especifica los parámetros de configuración, por ejemplo:

```python
from azureml.core import Workspace
# Create the workspace using the specified parameters
ws = Workspace.create(name = workspace_name,
                      subscription_id = subscription_id,
                      resource_group = resource_group, 
                      location = workspace_region,
                      create_resource_group = True,
                      exist_ok = True)
ws.get_details()

# write the details of the workspace to a configuration file to the notebook library
ws.write_config()
```

Más información sobre la clase de área de trabajo de AML en la [documentación de referencia del SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py).


#### <a name="create-a-compute-cluster"></a>Creación de un clúster de proceso
Azure Machine Learning admite varios destinos de proceso, algunos de los cuales se administran mediante el servicio y otros se pueden asociar al área de trabajo (por ejemplo, un clúster de HDInsight o una máquina virtual remota. Más información sobre los diversos [destinos de proceso](../machine-learning/service/how-to-set-up-training-targets.md). El concepto de creación de un clúster de proceso de BatchAI se corresponde con la creación de un clúster de AmlCompute en Azure ML. La creación de Amlcompute adopta una configuración de proceso parecida al modo en que se pasan los parámetros en BatchAI. Un aspecto a destacar es que la escalabilidad automática está activado de forma predeterminada en el clúster de AmlCompute, mientras que está desactivado de forma predeterminada en BatchAI.

En **Batch AI**, se hizo de esta manera:

```python
nodes_count = 2
cluster_name = 'nc6'

parameters = models.ClusterCreateParameters(
    vm_size='STANDARD_NC6',
    scale_settings=models.ScaleSettings(
        manual=models.ManualScaleSettings(target_node_count=nodes_count)
    ),
    user_account_settings=models.UserAccountSettings(
        admin_user_name=cfg.admin,
        admin_user_password=cfg.admin_password or None,
        admin_user_ssh_public_key=cfg.admin_ssh_key or None,
    )
)
_ = client.clusters.create(cfg.resource_group, cfg.workspace, cluster_name, parameters).result()
```

En **Azure Machine Learning Service**, pruebe:

```python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# Choose a name for your CPU cluster
gpu_cluster_name = "nc6"

# Verify that cluster does not exist already
try:
    gpu_cluster = ComputeTarget(workspace=ws, name=gpu_cluster_name)
    print('Found existing cluster, use it.')
except ComputeTargetException:
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6',
                                                           vm_priority='lowpriority',
                                                           min_nodes=1,
                                                           max_nodes=2,
                                                           idle_seconds_before_scaledown='300',
                                                           vnet_resourcegroup_name='<my-resource-group>',
                                                           vnet_name='<my-vnet-name>',
                                                           subnet_name='<my-subnet-name>')
    gpu_cluster = ComputeTarget.create(ws, gpu_cluster_name, compute_config)

gpu_cluster.wait_for_completion(show_output=True)
```

Más información sobre la clase AMLCompute en la [documentación de referencia del SDK](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py). Tenga en cuenta que en la configuración anterior, solo son obligatorias las propiedades vm_size y max_nodes; el resto, como VNets son únicamente para la configuración avanzada del clúster.


#### <a name="monitoring-status-of-your-cluster"></a>Supervisión del estado del clúster
Esta tarea es más sencilla en Azure ML, como puede ver a continuación.

En **Batch AI**, se hizo de esta manera:

```python
cluster = client.clusters.get(cfg.resource_group, cfg.workspace, cluster_name)
utils.cluster.print_cluster_status(cluster)
```

En **Azure Machine Learning Service**, pruebe:

```python
gpu_cluster.get_status().serialize()
```

#### <a name="getting-reference-to-a-storage-account"></a>Obtención de una referencia a una cuenta de Storage
El concepto de un almacenamiento de datos como blob se simplifica en Azure ML con el objeto DataStore. De forma predeterminada, el área de trabajo de Azure ML crea una cuenta de almacenamiento, pero puede asociar también su propio almacenamiento como parte de la creación del área de trabajo. 

En **Batch AI**, se hizo de esta manera:

```python
azure_blob_container_name = 'batchaisample'
blob_service = BlockBlobService(cfg.storage_account_name, cfg.storage_account_key)
blob_service.create_container(azure_blob_container_name, fail_on_exist=False)
```

En **Azure Machine Learning Service**, pruebe:

```python
ds = ws.get_default_datastore()
print(ds.datastore_type, ds.account_name, ds.container_name)
```

Más información sobre el registro de cuentas de almacenamiento adicionales o la obtención de una referencia a otro almacén de datos registrado en la [documentación de Azure ML Service](../machine-learning/service/how-to-access-data.md#create-a-datastore).


#### <a name="downloading-and-uploading-data"></a>Descarga y carga de datos 
Con cualquiera de los servicios, puede cargar los datos en la cuenta de almacenamiento fácilmente mediante la referencia de almacén de datos anterior. En BatchAI, también se va a implementar el script de entrenamiento como parte del recurso compartido de archivos, si bien también verá cómo puede especificarlo como parte de la configuración de su trabajo en el caso de Azure ML.

En **Batch AI**, se hizo de esta manera:

```python
mnist_dataset_directory = 'mnist_dataset'
utils.dataset.download_and_upload_mnist_dataset_to_blob(
    blob_service, azure_blob_container_name, mnist_dataset_directory)

script_directory = 'tensorflow_samples'
script_to_deploy = 'mnist_replica.py'

blob_service.create_blob_from_path(azure_blob_container_name,
                                   script_directory + '/' + script_to_deploy, 
                                   script_to_deploy)
```


En **Azure Machine Learning Service**, pruebe:

```python
import os
import urllib
os.makedirs('./data', exist_ok=True)
download_url = 'https://s3.amazonaws.com/img-datasets/mnist.npz'
urllib.request.urlretrieve(download_url, filename='data/mnist.npz')

ds.upload(src_dir='data', target_path='mnist_dataset', overwrite=True, show_progress=True)

path_on_datastore = ' mnist_dataset/mnist.npz' ds_data = ds.path(path_on_datastore) print(ds_data)
```

#### <a name="create-an-experiment"></a>Creación de un experimento
Como se mencionó anteriormente, el concepto de experimento en Azure ML es similar al de BatchAI. Cada experimento puede tener ejecuciones individuales, de forma parecida a cómo tenemos trabajos en BatchAI. Azure ML también permite tener una jerarquía bajo cada ejecución primaria, para ejecuciones secundarias individuales.

En **Batch AI**, se hizo de esta manera:

```python
experiment_name = 'tensorflow_experiment'
experiment = client.experiments.create(cfg.resource_group, cfg.workspace, experiment_name).result()
```

En **Azure Machine Learning Service**, pruebe:

```python
from azureml.core import Experiment

experiment_name = 'tensorflow_experiment'
experiment = Experiment(ws, name=experiment_name)
```


#### <a name="submit-a-job"></a>Enviar un trabajo
Una vez que crea un experimento, tiene diferentes maneras de enviar una ejecución. En este ejemplo, se va a intentar crear un modelo de aprendizaje profundo mediante TensorFlow y se usará para ello un estimador de Azure ML. Un [estimador](../machine-learning/service/how-to-train-ml-models.md) es simplemente una función de contenedor en la configuración de ejecución subyacente, que facilita el envío de ejecuciones; actualmente solo se admite para Pytorch y TensorFlow. Gracias al concepto de almacenes de datos, también podrá ver lo fácil que es especificar las rutas de acceso de montaje. 

En **Batch AI**, se hizo de esta manera:

```python
azure_file_share = 'afs'
azure_blob = 'bfs'
args_fmt = '--job_name={0} --num_gpus=1 --train_steps 10000 --checkpoint_dir=$AZ_BATCHAI_OUTPUT_MODEL --log_dir=$AZ_BATCHAI_OUTPUT_TENSORBOARD --data_dir=$AZ_BATCHAI_INPUT_DATASET --ps_hosts=$AZ_BATCHAI_PS_HOSTS --worker_hosts=$AZ_BATCHAI_WORKER_HOSTS --task_index=$AZ_BATCHAI_TASK_INDEX'

parameters = models.JobCreateParameters(
     cluster=models.ResourceId(id=cluster.id),
     node_count=2,
     input_directories=[
        models.InputDirectory(
            id='SCRIPT',
            path='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}/{1}'.format(azure_blob, script_directory)),
        models.InputDirectory(
            id='DATASET',
            path='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}/{1}'.format(azure_blob, mnist_dataset_directory))],
     std_out_err_path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
     output_directories=[
        models.OutputDirectory(
            id='MODEL',
            path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
            path_suffix='Models'),
        models.OutputDirectory(
            id='TENSORBOARD',
            path_prefix='$AZ_BATCHAI_JOB_MOUNT_ROOT/{0}'.format(azure_file_share),
            path_suffix='Logs')
     ],
     mount_volumes=models.MountVolumes(
            azure_file_shares=[
                models.AzureFileShareReference(
                    account_name=cfg.storage_account_name,
                    credentials=models.AzureStorageCredentialsInfo(
                        account_key=cfg.storage_account_key),
                    azure_file_url='https://{0}.file.core.windows.net/{1}'.format(
                        cfg.storage_account_name, azure_file_share_name),
                    relative_mount_path=azure_file_share)
            ],
            azure_blob_file_systems=[
                models.AzureBlobFileSystemReference(
                    account_name=cfg.storage_account_name,
                    credentials=models.AzureStorageCredentialsInfo(
                        account_key=cfg.storage_account_key),
                    container_name=azure_blob_container_name,
                    relative_mount_path=azure_blob)
            ]
        ),
     container_settings=models.ContainerSettings(
         image_source_registry=models.ImageSourceRegistry(image='tensorflow/tensorflow:1.8.0-gpu')),
     tensor_flow_settings=models.TensorFlowSettings(
         parameter_server_count=1,
         worker_count=nodes_count,
         python_script_file_path='$AZ_BATCHAI_INPUT_SCRIPT/'+ script_to_deploy,
         master_command_line_args=args_fmt.format('worker'),
         worker_command_line_args=args_fmt.format('worker'),
         parameter_server_command_line_args=args_fmt.format('ps'),
     )
)
```

El envío del trabajo propiamente dicho en BatchAI se realiza mediante la función de creación.

```python
job_name = datetime.utcnow().strftime('tf_%m_%d_%Y_%H%M%S')
job = client.jobs.create(cfg.resource_group, cfg.workspace, experiment_name, job_name, parameters).result()
print('Created Job {0} in Experiment {1}'.format(job.name, experiment.name))
```

La información completa de este fragmento de código de entrenamiento (incluido el archivo mnist_replica.py que habíamos cargado en el recurso compartido de archivos anterior) se puede encontrar en el [repositorio de GitHub de cuadernos de ejemplo de BatchAI](https://github.com/Azure/BatchAI/tree/2238607d5a028a0c5e037168aefca7d7bb165d5c/recipes/TensorFlow/TensorFlow-GPU-Distributed).

En **Azure Machine Learning Service**, pruebe:

```python
from azureml.train.dnn import TensorFlow

script_params={
    '--num_gpus': 1,
    '--train_steps': 500,
    '--input_data': ds_data.as_mount()

}

estimator = TensorFlow(source_directory=project_folder,
                       compute_target=gpu_cluster,
                       script_params=script_params,
                       entry_script='tf_mnist_replica.py',
                       node_count=2,
                       worker_count=2,
                       parameter_server_count=1,   
                       distributed_backend='ps',
                       use_gpu=True)
```

Toda la información de este fragmento de código de entrenamiento (incluido el archivo tf_mnist_replica.py) puede encontrarse en el [repositorio de GitHub de cuadernos de ejemplo de Azure ML](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/distributed-tensorflow-with-parameter-server). El almacén de datos propiamente dicho se puede montar en los nodos individuales, o los datos de entrenamiento se pueden descargar en el propio nodo. Para más información sobre cómo hacer referencia al almacén de datos en el estimador, consulte la [documentación de Azure ML Service](../machine-learning/service/how-to-access-data.md#access-datastores-for-training). 

El envío de una ejecución en Azure ML se realiza mediante la función de envío.

```python
run = experiment.submit(estimator)
print(run)
```

Otra manera de especificar parámetros para la ejecución es usar una configuración de ejecución, que resulta útil especialmente para definir un entorno de entrenamiento personalizado. Puede encontrar más información en este [cuaderno de AmlCompute de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-on-amlcompute/train-on-amlcompute.ipynb). 

#### <a name="monitor-your-run"></a>Supervisión de la ejecución
Una vez que se envía una ejecución, puede esperar a que se complete o supervisarla en Azure ML mediante widgets limpios de Jupyter que puede invocar directamente desde el código. También puede extraer el contexto de una ejecución anterior si recorre en bucle los diversos experimentos de un área de trabajo y las ejecuciones individuales dentro de cada experimento.

En **Batch AI**, se hizo de esta manera:

```python
utils.job.wait_for_job_completion(client, cfg.resource_group, cfg.workspace, 
                                  experiment_name, job_name, cluster_name, 'stdouterr', 'stdout-wk-0.txt')

files = client.jobs.list_output_files(cfg.resource_group, cfg.workspace, experiment_name, job_name,
                                      models.JobsListOutputFilesOptions(outputdirectoryid='stdouterr')) 
for f in list(files):
    print(f.name, f.download_url or 'directory')
```


En **Azure Machine Learning Service**, pruebe:

```python
run.wait_for_completion(show_output=True)

from azureml.widgets import RunDetails
RunDetails(run).show()
```

Esta es una instantánea de cómo se cargaría el widget en el cuaderno para examinar sus registros en tiempo real: ![Diagrama de supervisión de widgets](./media/overview-what-happened-batch-ai/monitor.png)



#### <a name="editing-a-cluster"></a>Edición de un clúster
Eliminar un clúster es sencillo. Además, Azure ML también permite actualizar un clúster desde el cuaderno en caso de que quiera escalarlo a un número mayor de nodos o aumentar el tiempo de espera de inactividad antes de reducirlo verticalmente. No se permite sin embargo cambiar el tamaño de máquina virtual del clúster propiamente dicho, puesto que se requiere una nueva implementación en el back-end.

En **Batch AI**, se hizo de esta manera:
```python
_ = client.clusters.delete(cfg.resource_group, cfg.workspace, cluster_name)
```

En **Azure Machine Learning Service**, pruebe:

```python
gpu_cluster.delete()

gpu_cluster.update(min_nodes=2, max_nodes=4, idle_seconds_before_scaledown=600)
```

## <a name="support"></a>Soporte técnico

La retirada de BatchAI está prevista para el 31 de marzo y ya se está impidiendo el registro de nuevas suscripciones en el servicio, salvo que se eleve una excepción mediante soporte técnico para que se incluya en la lista de permitidos.  Si tiene alguna pregunta o comentario al migrar a Azure Machine Learning Service, puede ponerse en contacto con nosotros en [Azure Batch AI Training Preview](mailto:AzureBatchAITrainingPreview@service.microsoft.com).

Azure Machine Learning Service es un servicio disponible con carácter general. Esto significa que se incluye con un Acuerdo de Nivel de Servicio (SLA) y diversos planes de soporte técnico para elegir.

Los precios de uso de la infraestructura de Azure, bien mediante el servicio BatchAI o el servicio Azure Machine Learning Service, no deberían variar, ya que, en ambos casos, solo se modifica el precio por el proceso subyacente. Para más información, consulte la [calculadora de precios](https://azure.microsoft.com/pricing/details/machine-learning-service/).

Vea la disponibilidad regional entre los dos servicios en [Azure Portal](https://azure.microsoft.com/global-infrastructure/services/?products=batch-ai,machine-learning-service&regions=all).


## <a name="next-steps"></a>Pasos siguientes

+ Lea la [información general de Azure Machine Learning Service](../machine-learning/service/overview-what-is-azure-ml.md).

+ [Configure un destino de proceso para el entrenamiento de modelos](../machine-learning/service/how-to-set-up-training-targets.md) con Azure Machine Learning Service.

+ Revise la [hoja de ruta de Azure](https://azure.microsoft.com/updates/) para información de otras actualizaciones de servicios de Azure.
