---
title: Ejecutar predicciones por lotes en grandes cantidades de datos
titleSuffix: Azure Machine Learning service
description: Aprenda cómo hacer predicciones por lotes de forma asincrónica en grandes cantidades de datos gracias a Azure Machine Learning Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens, garye
ms.author: jordane
author: jpe316
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 4c38230c1d5a4dcad9a10b82d810f82515980076
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55245878"
---
# <a name="run-batch-predictions-on-large-data-sets-with-azure-machine-learning-service"></a>Ejecutar predicciones por lotes en grandes conjuntos de datos con Azure Machine Learning Service

En este artículo aprenderá a hacer predicciones de forma asincrónica con grandes cantidades de datos usando, para ello, Azure Machine Learning Service.

La predicción por lotes (o puntuación por lotes) proporciona una inferencia rentable con un rendimiento sin precedentes para aplicaciones asincrónicas. Las canalizaciones de predicción por lotes pueden escalarse para realizar inferencias en terabytes de datos de producción. La predicción por lotes está optimizada para las predicciones de alto rendimiento y de tipo "fire-and-forget" (envíelo y olvídese) cuando se trabaja con una gran cantidad de datos.

>[!TIP]
> Si su sistema requiere un procesamiento de baja latencia (esto es, procesar rápidamente un solo documento o un pequeño conjunto de documentos), use la [puntuación en tiempo real](how-to-consume-web-service.md) en lugar de la predicción por lotes.

En los pasos siguientes, se creará una [canalización de aprendizaje automático](concept-ml-pipelines.md) para registrar un modelo de visión de equipo previamente entrenado ([Inception-V3](https://arxiv.org/abs/1512.00567)). A continuación, usará el modelo previamente entrenado para realizar una puntuación por lotes en las imágenes disponibles en la cuenta de almacenamiento de blobs de Azure. Estas imágenes se usan para puntuar imágenes sin etiqueta del conjunto de datos [ImageNet](http://image-net.org/).

## <a name="prerequisites"></a>Requisitos previos

- Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe la [versión gratuita o de pago de Azure Machine Learning Service](http://aka.ms/AMLFree).

- Configure un entorno de desarrollo para instalar el SDK de Azure Machine Learning. Para obtener más información, consulte [Configurar un entorno de desarrollo para Azure Machine Learning](how-to-configure-environment.md).

- Cree un área de trabajo de Azure Machine Learning que contendrá todos los recursos de su canalización. Puede usar el siguiente código o, si quiere obtener más opciones, consulte [Crear un archivo de configuración del área de trabajo](how-to-configure-environment.md#workspace).

  ```python
  ws = Workspace.create(
     name = '<workspace-name>',
     subscription_id = '<subscription-id>',
     resource_group = '<resource-group>',
     location = '<workspace_region>',
     exist_ok = True)
  ```

## <a name="set-up-machine-learning-resources"></a>Configurar los recursos de aprendizaje automático

Los siguientes pasos configuran los recursos que necesita para ejecutar una canalización:

- Obtenga acceso al almacén de datos que ya tiene el modelo entrenado previamente, etiquetas de entrada e imágenes para puntuar (esto ya está configurado).
- Configure un almacén de datos para almacenar las salidas.
- Configure los objetos  `DataReference`  para que apunten a los datos en los almacenes de datos anteriores.
- Configure las máquinas o clústeres de cálculo donde se ejecutarán los pasos de la canalización.

### <a name="access-the-datastores"></a>Obtener acceso a los almacenes de datos

Primero, obtenga acceso al almacén de datos que tiene el modelo, las etiquetas y las imágenes.

Usará para ello un contenedor de blob público llamado *sampledata* en la cuenta *pipelinedata* que contiene imágenes del conjunto de evaluación de ImageNet. El nombre del almacén de datos para este contenedor público es *images_datastore*. Registre este almacén de datos con su espacio de trabajo:

```python
# Public blob container details
account_name = "pipelinedata"
datastore_name="images_datastore"
container_name="sampledata"
 
batchscore_blob = Datastore.register_azure_blob_container(ws,
                      datastore_name=datastore_name,
                      container_name= container_name,
                      account_name=account_name,
                      overwrite=True)
```

A continuación, configúrelo para usar el almacén de datos predeterminado en las salidas.

Cuando cree su área de trabajo, [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction)  y [Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction)  se adjuntan al área de trabajo de forma predeterminada. Azure Files es el almacén de datos predeterminado para un área de trabajo, pero también puede usar Blob Storage como un almacén de datos. Para obtener más información, consulte [Opciones de almacenamiento de Azure](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

```python
def_data_store = ws.get_default_datastore()
```

### <a name="configure-data-references"></a>Configurar las referencias de datos

Ahora debe hacer referencia a los datos en su canalización como entradas para los pasos de la canalización.

Un objeto [DataReference](https://docs.microsoft.com/python/api/azureml-core/azureml.data.data_reference.datareference)  se encarga de representar un origen de datos en una canalización. El objeto  `DataReference`  apunta a los datos que están en o que son accesibles desde un almacén de datos. Tenga en cuenta que necesita los objetos `DataReference` para el directorio que usa para las imágenes de entrada, el directorio en el que se almacena el modelo entrenado previamente, el directorio para las etiquetas y el directorio de salida.

```python
input_images = DataReference(datastore=batchscore_blob, 
                             data_reference_name="input_images",
                             path_on_datastore="batchscoring/images",
                             mode="download")
                           
model_dir = DataReference(datastore=batchscore_blob, 
                          data_reference_name="input_model",
                          path_on_datastore="batchscoring/models",
                          mode="download")                          
                         
label_dir = DataReference(datastore=batchscore_blob, 
                          data_reference_name="input_labels",
                          path_on_datastore="batchscoring/labels",
                          mode="download")                          
                         
output_dir = PipelineData(name="scores", 
                          datastore=def_data_store, 
                          output_path_on_compute="batchscoring/results")
```

### <a name="set-up-compute-target"></a>Configurar el destino de proceso

En Azure Machine Learning, el *proceso* (o *destino de proceso*) se refiere a las máquinas o clústeres que realizarán los pasos del cálculo en su canal de aprendizaje automático. Por ejemplo, puede crear un `Azure Machine Learning compute`.

```python
compute_name = "gpucluster"
compute_min_nodes = 0
compute_max_nodes = 4
vm_size = "STANDARD_NC6"

if compute_name in ws.compute_targets:
    compute_target = ws.compute_targets[compute_name]
    if compute_target and type(compute_target) is AmlCompute:
        print('Found compute target. just use it. ' + compute_name)
else:
    print('Creating a new compute target...')
    provisioning_config = AmlCompute.provisioning_configuration(
                     vm_size = vm_size, # NC6 is GPU-enabled
                     vm_priority = 'lowpriority', # optional
                     min_nodes = compute_min_nodes, 
                     max_nodes = compute_max_nodes)

    # create the cluster
    compute_target = ComputeTarget.create(ws, 
                        compute_name, 
                        provisioning_config)
    
    compute_target.wait_for_completion(
                     show_output=True, 
                     min_node_count=None, 
                     timeout_in_minutes=20)
```

## <a name="prepare-the-model"></a>Preparar el modelo

Antes de que pueda usar el modelo entrenado previamente, deberá descargar ese modelo y registrarlo en su área de trabajo.

### <a name="download-the-pretrained-model"></a>Descargar el modelo entrenado previamente

Descargue el modelo de Computer Vision entrenado previamente (InceptionV3) desde <http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz>. A continuación, extráigalo a la subcarpeta `models`.

```python
import os
import tarfile
import urllib.request

model_dir = 'models'
if not os.path.isdir(model_dir):
    os.mkdir(model_dir)

url="http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz"
response = urllib.request.urlretrieve(url, "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall(model_dir)
```

### <a name="register-the-model"></a>Registro del modelo

Aquí se explica cómo registrar el modelo:

```python
import shutil
from azureml.core.model import Model

# register downloaded model 
model = Model.register(
        model_path = "models/inception_v3.ckpt",
        model_name = "inception", # This is the name of the registered model
        tags = {'pretrained': "inception"},
        description = "Imagenet trained tensorflow inception",
        workspace = ws)
```

## <a name="write-your-scoring-script"></a>Escribir un script de puntuación

>[!Warning]
>El siguiente código es solo una muestra de lo que se encuentra en el archivo [batch_score.py](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/pipeline-batch-scoring/batch_scoring.py) que usa el [cuaderno de muestra](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/pipeline-batch-scoring/pipeline-batch-scoring.ipynb). Deberá crear su propio script de puntuación para su escenario.

El script `batch_score.py` toma las imágenes de entrada en  *dataset_path* y los modelos entrenados previamente en  *model_dir* y genera *results-label.txt*  en  *output_dir*.

```python
# Snippets from a sample scoring script
# Refer to the accompanying batch-scoring Notebook
# https://github.com/Azure/MachineLearningNotebooks/blob/master/pipeline/pipeline-batch-scoring.ipynb
# for the implementation script

# Get labels
def get_class_label_dict(label_file):
  label = []
  proto_as_ascii_lines = tf.gfile.GFile(label_file).readlines()
  for l in proto_as_ascii_lines:
    label.append(l.rstrip())
  return label

class DataIterator:
  # Definition of the DataIterator here
  
def main(_):
    # Refer to batch-scoring Notebook for implementation.
    label_file_name = os.path.join(args.label_dir, "labels.txt")
    label_dict = get_class_label_dict(label_file_name)
    classes_num = len(label_dict)
    test_feeder = DataIterator(data_dir=args.dataset_path)
    total_size = len(test_feeder.labels)

    # get model from model registry
    model_path = Model.get_model_path(args.model_name)
    with tf.Session() as sess:
        test_images = test_feeder.input_pipeline(batch_size=args.batch_size)
        with slim.arg_scope(inception_v3.inception_v3_arg_scope()):
            input_images = tf.placeholder(tf.float32, [args.batch_size, image_size, image_size, num_channel])
            logits, _ = inception_v3.inception_v3(input_images,
                                                        num_classes=classes_num,
                                                        is_training=False)
            probabilities = tf.argmax(logits, 1)

        sess.run(tf.global_variables_initializer())
        sess.run(tf.local_variables_initializer())
        coord = tf.train.Coordinator()
        threads = tf.train.start_queue_runners(sess=sess, coord=coord)
        saver = tf.train.Saver()
        saver.restore(sess, model_path)
        out_filename = os.path.join(args.output_dir, "result-labels.txt")
            
        # copy the file to artifacts
        shutil.copy(out_filename, "./outputs/")
```

## <a name="build-and-run-the-batch-scoring-pipeline"></a>Compilar y ejecutar la canalización de puntuación por lotes

Ya tiene todo lo que necesita para construir la canalización, así que ahora solo tiene que combinar estos elementos.

### <a name="prepare-the-run-environment"></a>Preparar el entorno de ejecución

Especifique las dependencias conda para su script. Necesitará este objeto más adelante, cuando cree el paso de canalización.

```python
from azureml.core.runconfig import DEFAULT_GPU_IMAGE

cd = CondaDependencies.create(pip_packages=["tensorflow-gpu==1.10.0", "azureml-defaults"])

# Runconfig
amlcompute_run_config = RunConfiguration(conda_dependencies=cd)
amlcompute_run_config.environment.docker.enabled = True
amlcompute_run_config.environment.docker.gpu_support = True
amlcompute_run_config.environment.docker.base_image = DEFAULT_GPU_IMAGE
amlcompute_run_config.environment.spark.precache_packages = False
```

### <a name="specify-the-parameter-for-your-pipeline"></a>Especifique el parámetro para la canalización.

Cree un parámetro de canalización mediante un objeto  [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py)  con un valor predeterminado.

```python
batch_size_param = PipelineParameter(
                    name="param_batch_size", 
                    default_value=20)
```

### <a name="create-the-pipeline-step"></a>Crear el paso de canalización

Cree el paso de canalización mediante el script, la configuración del entorno y los parámetros. Especifique el destino de proceso que ya adjuntó a su área de trabajo como el destino de ejecución del script. Use [PythonScriptStep](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) para crear el paso de la canalización.

```python
inception_model_name = "inception_v3.ckpt"

batch_score_step = PythonScriptStep(
    name="batch_scoring",
    script_name="batch_score.py",
    arguments=["--dataset_path", input_images, 
               "--model_name", "inception",
               "--label_dir", label_dir, 
               "--output_dir", output_dir, 
               "--batch_size", batch_size_param],
    compute_target=compute_target,
    inputs=[input_images, label_dir],
    outputs=[output_dir],
    runconfig=amlcompute_run_config,
    source_directory=scripts_folder
```

### <a name="run-the-pipeline"></a>Ejecución de la canalización

A continuación, ejecute la canalización y examine la salida que generó. La salida tiene una puntuación correspondiente a cada imagen de entrada.

```python
# Run the pipeline
pipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'batch_scoring').submit(pipeline, pipeline_params={"param_batch_size": 20})

# Wait for the run to finish (this might take several minutes)
pipeline_run.wait_for_completion(show_output=True)

# Download and review the output
step_run = list(pipeline_run.get_children())[0]
step_run.download_file("./outputs/result-labels.txt")

import pandas as pd
df = pd.read_csv("result-labels.txt", delimiter=":", header=None)
df.columns = ["Filename", "Prediction"]
df.head()
```

## <a name="publish-the-pipeline"></a>Publicación de la canalización

Cuando esté satisfecho con el resultado de la ejecución, publique la canalización para que pueda ejecutarla con diferentes valores de entrada más adelante. Cuando se publica una canalización, obtiene un punto de conexión de REST que permite la invocación de la canalización con el conjunto de parámetros que ya se han incorporado con [PipelineParameter](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.graph.pipelineparameter?view=azure-ml-py).

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring", 
    description="Batch scoring using Inception v3 model", 
    version="1.0")
```

## <a name="rerun-the-pipeline-by-using-the-rest-endpoint"></a>Volver a ejecutar la canalización mediante el punto de conexión REST

Para volver a ejecutar la canalización, necesitará un token de encabezado de autenticación de Azure Active Directory, tal como se describe en la [clase AzureCliAuthentication](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.azurecliauthentication?view=azure-ml-py).

```python
from azureml.pipeline.core import PublishedPipeline

rest_endpoint = published_pipeline.endpoint
# specify batch size when running the pipeline
response = requests.post(rest_endpoint, 
        headers=aad_token, 
        json={"ExperimentName": "batch_scoring",
               "ParameterAssignments": {"param_batch_size": 50}})

# Monitor the run
from azureml.pipeline.core.run import PipelineRun
published_pipeline_run = PipelineRun(ws.experiments["batch_scoring"], run_id)

RunDetails(published_pipeline_run).show()
```

## <a name="next-steps"></a>Pasos siguientes

Para ver cómo funciona esto de un extremo a otro, pruebe el cuaderno de puntuación por lotes en [GitHub](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines). 

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

