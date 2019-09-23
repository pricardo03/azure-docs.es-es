---
title: 'Tutorial: Canalizaciones de aprendizaje automático de Azure para la puntuación por lotes'
titleSuffix: Azure Machine Learning
description: Cree una canalización de aprendizaje automático para ejecutar la puntuación por lotes en un modelo de clasificación de imágenes. Las canalizaciones de aprendizaje automático optimizan el flujo de trabajo con velocidad, portabilidad y reutilización para que pueda centrarse en sus conocimientos, el aprendizaje automático, en lugar de hacerlo en la infraestructura y la automatización.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 09/05/2019
ms.openlocfilehash: 15a11ba74262ec5a354f0cb3fe22c09167c8d5a6
ms.sourcegitcommit: d15b23e23328ce7502dd3d2846b49fd2d6d8209c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/16/2019
ms.locfileid: "71005394"
---
# <a name="use-azure-machine-learning-pipelines-for-batch-scoring"></a>Uso de canalizaciones de Azure Machine Learning para la puntuación por lotes

En este tutorial, usará canalizaciones de Azure Machine Learning para ejecutar un trabajo de puntuación por lotes. En este ejemplo se usa el modelo Tensorflow de red neuronal de circunvolución [Inception-V3](https://arxiv.org/abs/1512.00567) para clasificar las imágenes sin etiquetar. Después de compilar y publicar una canalización, configurará un punto de conexión REST para desencadenar la canalización desde cualquier biblioteca HTTP en cualquier plataforma.

Las canalizaciones de aprendizaje automático optimizan el flujo de trabajo con velocidad, portabilidad y reutilización para que pueda centrarse en sus conocimientos, el aprendizaje automático, en lugar de hacerlo en la infraestructura y la automatización. [Más información sobre las canalizaciones de aprendizaje automático](concept-ml-pipelines.md).

En este tutorial, aprenderá a realizar las tareas siguientes:

> [!div class="checklist"]
> * Configurar el área de trabajo y descargar datos de ejemplo
> * Crear objetos de datos para capturar y generar datos
> * Descargar, preparar y registrar el modelo en el área de trabajo
> * Aprovisionar destinos de proceso y crear un script de puntuación
> * Compilar, ejecutar y publicar una canalización
> * Habilitar un punto de conexión REST para la canalización

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Requisitos previos

* Complete la [parte 1 del tutorial de instalación](tutorial-1st-experiment-sdk-setup.md) si aún no tiene un área de trabajo de Azure Machine Learning o una máquina virtual de cuadernos.
* Después de completar el tutorial de instalación, abra el cuaderno **tutorials/tutorial-pipeline-batch-scoring-classification.ipynb** mediante el mismo servidor de cuadernos.

Este tutorial también está disponible en [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials) si quiere ejecutarlo en su propio [entorno local](how-to-configure-environment.md#local). Ejecute `pip install azureml-sdk[notebooks] azureml-pipeline-core azureml-pipeline-steps pandas requests` para obtener los paquetes requeridos.

## <a name="configure-workspace-and-create-datastore"></a>Configuración del área de trabajo y creación del almacén de datos

Cree un objeto de área de trabajo a partir del área de trabajo de Azure Machine Learning existente. 
+ Un [área de trabajo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) es una clase que acepta la información de recursos y suscripciones de Azure. También crea un recurso en la nube para supervisar y realizar un seguimiento de las ejecuciones del modelo. 

+ `Workspace.from_config()` lee el archivo **config.json** y carga los detalles de autenticación en un objeto denominado `ws`. En el resto del código de este tutorial se usa `ws`.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

### <a name="create-a-datastore-for-sample-images"></a>Creación de un almacén de datos para las imágenes de ejemplo

Obtenga el ejemplo de datos públicos de evaluación de ImageNet del contenedor de blobs público `sampledata` de la cuenta `pipelinedata`. Al llamar a `register_azure_blob_container()`, los datos están disponibles en el área de trabajo con el nombre `images_datastore`. Después, especifique el almacén de datos predeterminado del área de trabajo como almacén de datos de salida, que se usará para puntuar la salida en la canalización.

```python
from azureml.core.datastore import Datastore

batchscore_blob = Datastore.register_azure_blob_container(ws, 
                      datastore_name="images_datastore", 
                      container_name="sampledata", 
                      account_name="pipelinedata", 
                      overwrite=True)

def_data_store = ws.get_default_datastore()
```

## <a name="create-data-objects"></a>Creación de objetos de datos

Al compilar canalizaciones, se usan objetos `DataReference` para leer datos de los almacenes de datos del área de trabajo, y objetos `PipelineData` para transferir los datos intermedios entre los pasos de la canalización.

> [!Important]
> En este ejemplo de puntuación por lotes solo se usa un paso de canalización, pero en los casos de uso con varios pasos, el flujo habitual incluirá:
>
> 1. El uso de objetos `DataReference` como **entradas** para capturar datos sin procesar, algunas transformaciones y, luego, la **generación** de un objeto `PipelineData`.
>
> 2. El uso del **objeto de salida** `PipelineData` del paso anterior como *objeto de entrada*, repetido en los siguientes pasos.

En este escenario se crean objetos `DataReference` correspondientes a los directorios del almacén de datos para las imágenes de entrada y las etiquetas de clasificación (valores de prueba y). También se crea un objeto `PipelineData` para los datos de salida de puntuación por lotes.

```python
from azureml.data.data_reference import DataReference
from azureml.pipeline.core import PipelineData

input_images = DataReference(datastore=batchscore_blob, 
                             data_reference_name="input_images",
                             path_on_datastore="batchscoring/images",
                             mode="download"
                            )

label_dir = DataReference(datastore=batchscore_blob, 
                          data_reference_name="input_labels",
                          path_on_datastore="batchscoring/labels",
                          mode="download"                          
                         )

output_dir = PipelineData(name="scores", 
                          datastore=def_data_store, 
                          output_path_on_compute="batchscoring/results")
```

## <a name="download-and-register-the-model"></a>Descarga y registro del modelo

Descargue el modelo Tensorflow previamente entrenado para usarlo en la puntuación por lotes de la canalización. En primer lugar, cree un directorio local donde almacenar el modelo y, luego, descárguelo y extráigalo.

```python
import os
import tarfile
import urllib.request

if not os.path.isdir("models"):
    os.mkdir("models")
    
response = urllib.request.urlretrieve("http://download.tensorflow.org/models/inception_v3_2016_08_28.tar.gz", "model.tar.gz")
tar = tarfile.open("model.tar.gz", "r:gz")
tar.extractall("models")
```

Ahora, registrará el modelo en el área de trabajo, lo que le permite recuperarlo fácilmente en el proceso de canalización. En la función estática `register()`, el parámetro `model_name` es la clave que se usa para buscar el modelo en todo el SDK.

```python
from azureml.core.model import Model
 
model = Model.register(model_path="models/inception_v3.ckpt",
                       model_name="inception",
                       tags={"pretrained": "inception"},
                       description="Imagenet trained tensorflow inception",
                       workspace=ws)
```

## <a name="create-and-attach-remote-compute-target"></a>Creación y asociación del destino de proceso remoto

Dado que las canalizaciones de aprendizaje automático no se pueden ejecutar localmente, debe ejecutarlas en recursos en la nube. Nos referimos a ellos como destinos de proceso remotos, que son entornos de proceso virtuales reutilizables en los que se ejecutan experimentos y flujos de trabajo de aprendizaje automático. Ejecute el código siguiente para crear un destino [`AmlCompute`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) habilitado para GPU y asócielo al área de trabajo. Consulte el [artículo conceptual](https://docs.microsoft.com/azure/machine-learning/service/concept-compute-target) para más información sobre los destinos de proceso.


```python
from azureml.core.compute import AmlCompute, ComputeTarget
from azureml.exceptions import ComputeTargetException
compute_name = "gpu-cluster"

# checks to see if compute target already exists in workspace, else create it
try:
    compute_target = ComputeTarget(workspace=ws, name=compute_name)
except ComputeTargetException:
    config = AmlCompute.provisioning_configuration(vm_size="STANDARD_NC6",
                                                   vm_priority="lowpriority", 
                                                   min_nodes=0, 
                                                   max_nodes=1)

    compute_target = ComputeTarget.create(workspace=ws, name=compute_name, provisioning_configuration=config)
    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

## <a name="write-a-scoring-script"></a>Escritura de un script de puntuación

Para realizar la puntuación, creará un script de puntuación por lotes `batch_scoring.py` y lo escribirá en el directorio actual. El script toma imágenes de entrada, aplica el modelo de clasificación y envía las predicciones a un archivo de resultados.

El script `batch_scoring.py` toma los siguientes parámetros, que se pasan desde el paso de canalización que se crea más adelante en este tutorial:

- `--model_name`: el nombre del modelo que se usa.
- `--label_dir`: el directorio que contiene el archivo `labels.txt`. 
- `--dataset_path`: el directorio que contiene las imágenes de entrada.
- `--output_dir`: el script ejecutará el modelo en los datos y enviará `results-label.txt` a este directorio.
- `--batch_size`: el tamaño del lote que se usa para ejecutar el modelo

La infraestructura de canalizaciones usa la clase `ArgumentParser` para pasar los parámetros a los pasos de canalización. Por ejemplo, en el código siguiente al primer argumento `--model_name` se le da el identificador de propiedad `model_name`. En la función `main()`, se accede a esta propiedad mediante `Model.get_model_path(args.model_name)`.


```python
%%writefile batch_scoring.py

import os
import argparse
import datetime
import time
import tensorflow as tf
from math import ceil
import numpy as np
import shutil
from tensorflow.contrib.slim.python.slim.nets import inception_v3
from azureml.core.model import Model

slim = tf.contrib.slim

parser = argparse.ArgumentParser(description="Start a tensorflow model serving")
parser.add_argument('--model_name', dest="model_name", required=True)
parser.add_argument('--label_dir', dest="label_dir", required=True)
parser.add_argument('--dataset_path', dest="dataset_path", required=True)
parser.add_argument('--output_dir', dest="output_dir", required=True)
parser.add_argument('--batch_size', dest="batch_size", type=int, required=True)

args = parser.parse_args()

image_size = 299
num_channel = 3

# create output directory if it does not exist
os.makedirs(args.output_dir, exist_ok=True)


def get_class_label_dict(label_file):
    label = []
    proto_as_ascii_lines = tf.gfile.GFile(label_file).readlines()
    for l in proto_as_ascii_lines:
        label.append(l.rstrip())
    return label


class DataIterator:
    def __init__(self, data_dir):
        self.file_paths = []
        image_list = os.listdir(data_dir)
        self.file_paths = [data_dir + '/' + file_name.rstrip() for file_name in image_list]
        self.labels = [1 for file_name in self.file_paths]

    @property
    def size(self):
        return len(self.labels)

    def input_pipeline(self, batch_size):
        images_tensor = tf.convert_to_tensor(self.file_paths, dtype=tf.string)
        labels_tensor = tf.convert_to_tensor(self.labels, dtype=tf.int64)
        input_queue = tf.train.slice_input_producer([images_tensor, labels_tensor], shuffle=False)
        labels = input_queue[1]
        images_content = tf.read_file(input_queue[0])

        image_reader = tf.image.decode_jpeg(images_content, channels=num_channel, name="jpeg_reader")
        float_caster = tf.cast(image_reader, tf.float32)
        new_size = tf.constant([image_size, image_size], dtype=tf.int32)
        images = tf.image.resize_images(float_caster, new_size)
        images = tf.divide(tf.subtract(images, [0]), [255])

        image_batch, label_batch = tf.train.batch([images, labels], batch_size=batch_size, capacity=5 * batch_size)
        return image_batch


def main(_):
    label_file_name = os.path.join(args.label_dir, "labels.txt")
    label_dict = get_class_label_dict(label_file_name)
    classes_num = len(label_dict)
    test_feeder = DataIterator(data_dir=args.dataset_path)
    total_size = len(test_feeder.labels)
    count = 0

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
        with open(out_filename, "w") as result_file:
            i = 0
            while count < total_size and not coord.should_stop():
                test_images_batch = sess.run(test_images)
                file_names_batch = test_feeder.file_paths[i * args.batch_size:
                                                          min(test_feeder.size, (i + 1) * args.batch_size)]
                results = sess.run(probabilities, feed_dict={input_images: test_images_batch})
                new_add = min(args.batch_size, total_size - count)
                count += new_add
                i += 1
                for j in range(new_add):
                    result_file.write(os.path.basename(file_names_batch[j]) + ": " + label_dict[results[j]] + "\n")
                result_file.flush()
            coord.request_stop()
            coord.join(threads)

        shutil.copy(out_filename, "./outputs/")

if __name__ == "__main__":
    tf.app.run()

```

> [!TIP]
> La canalización de este tutorial solo tiene un paso y escribe la salida en un archivo, pero para canalizaciones de varios pasos, también se usa `ArgumentParser` para definir un directorio y escribir los datos de salida para la entrada en los pasos posteriores. Consulte el [cuaderno](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/nyc-taxi-data-regression-model-building/nyc-taxi-data-regression-model-building.ipynb) para ver un ejemplo de cómo pasar datos entre varios pasos de la canalización mediante el modelo de diseño `ArgumentParser`.

## <a name="build-and-run-the-pipeline"></a>Compilación y ejecución de la canalización

Antes de ejecutar la canalización, creará un objeto que defina el entorno de Python y las dependencias que necesita el script `batch_scoring.py`. La dependencia principal requerida es Tensorflow, pero también se instala `azureml-defaults` para los procesos en segundo plano desde el SDK. Cree un objeto `RunConfiguration` con las dependencias y especifique también la compatibilidad con Docker y GPU de Docker.

```python
from azureml.core.runconfig import DEFAULT_GPU_IMAGE
from azureml.core.runconfig import CondaDependencies, RunConfiguration

cd = CondaDependencies.create(pip_packages=["tensorflow-gpu==1.13.1", "azureml-defaults"])

amlcompute_run_config = RunConfiguration(conda_dependencies=cd)
amlcompute_run_config.environment.docker.enabled = True
amlcompute_run_config.environment.docker.base_image = DEFAULT_GPU_IMAGE
amlcompute_run_config.environment.spark.precache_packages = False
```

### <a name="parameterize-the-pipeline"></a>Parametrización de la canalización

Defina un parámetro personalizado para que la canalización controle el tamaño del lote. Después de que la canalización se ha publicado y expuesto mediante un punto de conexión REST, también se exponen los parámetros configurados, que se pueden especificar en la carga útil de JSON al volver a ejecutar la canalización con una solicitud HTTP.

Cree un objeto `PipelineParameter` para habilitar este comportamiento y defina un nombre y un valor predeterminado.

```python
from azureml.pipeline.core.graph import PipelineParameter
batch_size_param = PipelineParameter(name="param_batch_size", default_value=20)
```

### <a name="create-the-pipeline-step"></a>Crear el paso de canalización

Un paso de canalización es un objeto que encapsula todo lo que necesita para ejecutar una canalización, por ejemplo:

* Configuración del entorno y las dependencias
* El recurso de proceso en el que se ejecutará la canalización
* Los datos de entrada y salida, y cualquier parámetro personalizado
* Referencia a un script o a una lógica del SDK que se ejecutará durante el paso

Hay varias clases que se heredan de la clase principal [`PipelineStep`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py) para ayudar con la compilación de un paso mediante determinadas plataformas y pilas. En este ejemplo, se usa la clase [`PythonScriptStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) para definir la lógica del paso con un script de Python personalizado. Tenga en cuenta que si un argumento del script es una entrada al paso o una salida del paso, se deben definir **ambos** en la matriz `arguments`, **así como** en el parámetro `input` o `output`, respectivamente. 

En escenarios con más de un paso, una referencia a un objeto de la matriz `outputs` está disponible como una **entrada** en un paso posterior de la canalización.

```python
from azureml.pipeline.steps import PythonScriptStep

batch_score_step = PythonScriptStep(
    name="batch_scoring",
    script_name="batch_scoring.py",
    arguments=["--dataset_path", input_images, 
               "--model_name", "inception",
               "--label_dir", label_dir, 
               "--output_dir", output_dir, 
               "--batch_size", batch_size_param],
    compute_target=compute_target,
    inputs=[input_images, label_dir],
    outputs=[output_dir],
    runconfig=amlcompute_run_config
)
```

Para ver una lista de todas las clases para los diferentes tipos de pasos, consulte el [paquete de pasos](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py).

### <a name="run-the-pipeline"></a>Ejecución de la canalización

Ahora ejecute la canalización. En primer lugar, cree un objeto `Pipeline` con la referencia al área de trabajo y el paso de canalización que creó. El parámetro `steps` es una matriz de pasos y, en este caso, solo hay un paso para la puntuación por lotes. Para compilar canalizaciones con varios pasos, se colocan los pasos en orden en esta matriz.

Luego, use la función `Experiment.submit()` para enviar la canalización para su ejecución. También se especifica el parámetro personalizado `param_batch_size`. La función `wait_for_completion` generará registros durante el proceso de compilación de la canalización, lo que le permite ver el progreso actual.

> [!IMPORTANT]
> La primera ejecución de la canalización tarda aproximadamente **15 minutos**, ya que se deben descargar todas las dependencias, se crea una imagen de Docker y se aprovisiona o se crea el entorno de Python. Cuando se ejecuta de nuevo, tarda mucho menos, ya que se reutilizan esos recursos. Sin embargo, el tiempo de ejecución total depende de la carga de trabajo de los scripts y de los procesos que se ejecutan en cada paso de la canalización.

```python
from azureml.core import Experiment
from azureml.pipeline.core import Pipeline

pipeline = Pipeline(workspace=ws, steps=[batch_score_step])
pipeline_run = Experiment(ws, 'batch_scoring').submit(pipeline, pipeline_parameters={"param_batch_size": 20})
pipeline_run.wait_for_completion(show_output=True)
```

### <a name="download-and-review-output"></a>Descarga y revisión de la salida

Ejecute el siguiente código para descargar el archivo de salida creado a partir del script `batch_scoring.py` y, luego, explore los resultados de la puntuación.

```python
import pandas as pd

step_run = list(pipeline_run.get_children())[0]
step_run.download_file("./outputs/result-labels.txt")

df = pd.read_csv("result-labels.txt", delimiter=":", header=None)
df.columns = ["Filename", "Prediction"]
df.head(10)
```

<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Nombre de archivo</th>
      <th>Predicción</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>ILSVRC2012_val_00000102.JPEG</td>
      <td>Rhodesian ridgeback</td>
    </tr>
    <tr>
      <td>1</td>
      <td>ILSVRC2012_val_00000103.JPEG</td>
      <td>tripod</td>
    </tr>
    <tr>
      <td>2</td>
      <td>ILSVRC2012_val_00000104.JPEG</td>
      <td>typewriter keyboard</td>
    </tr>
    <tr>
      <td>3</td>
      <td>ILSVRC2012_val_00000105.JPEG</td>
      <td>silky terrier</td>
    </tr>
    <tr>
      <td>4</td>
      <td>ILSVRC2012_val_00000106.JPEG</td>
      <td>Windsor tie</td>
    </tr>
    <tr>
      <td>5</td>
      <td>ILSVRC2012_val_00000107.JPEG</td>
      <td>harvestman</td>
    </tr>
    <tr>
      <td>6</td>
      <td>ILSVRC2012_val_00000108.JPEG</td>
      <td>violin</td>
    </tr>
    <tr>
      <td>7</td>
      <td>ILSVRC2012_val_00000109.JPEG</td>
      <td>loudspeaker</td>
    </tr>
    <tr>
      <td>8</td>
      <td>ILSVRC2012_val_00000110.JPEG</td>
      <td>apron</td>
    </tr>
    <tr>
      <td>9</td>
      <td>ILSVRC2012_val_00000111.JPEG</td>
      <td>American lobster</td>
    </tr>
  </tbody>
</table>
</div>

## <a name="publish-and-run-from-rest-endpoint"></a>Publicación y ejecución desde el punto de conexión REST

Ejecute el código siguiente para publicar la canalización en el área de trabajo. En el área de trabajo del portal, puede ver los metadatos de la canalización, incluidos el historial de ejecución y las duraciones. También puede ejecutar la canalización manualmente desde el portal.

Además, la publicación de la canalización permite a un punto de conexión REST volver a ejecutar la canalización desde cualquier biblioteca HTTP en cualquier plataforma.

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring", description="Batch scoring using Inception v3 model", version="1.0")

published_pipeline
```

Para ejecutar la canalización desde el punto de conexión REST, primero necesita un encabezado de autenticación de tipo portador de OAuth2. En este ejemplo se usa la autenticación interactiva con fines ilustrativos, pero en la mayoría de los escenarios de producción que requieren autenticación automatizada o desatendida, use la autenticación de la entidad de servicio tal y como se [describe en este cuaderno](https://aka.ms/pl-restep-auth).

La autenticación de la entidad de servicio implica crear un **registro de aplicaciones** en **Azure Active Directory**, generar un secreto de cliente y conceder luego al **rol de la entidad de servicio acceso** al área de trabajo de aprendizaje automático. Luego, usará la clase [`ServicePrincipalAuthentication`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.serviceprincipalauthentication?view=azure-ml-py) para administrar el flujo de autenticación. 

Tanto `InteractiveLoginAuthentication` como `ServicePrincipalAuthentication` se heredan de `AbstractAuthentication`, y en ambos casos se usa la función `get_authentication_header()` de la misma manera para capturar el encabezado.

```python
from azureml.core.authentication import InteractiveLoginAuthentication

interactive_auth = InteractiveLoginAuthentication()
auth_header = interactive_auth.get_authentication_header()
```

Obtenga la dirección URL de REST de la propiedad `endpoint` del objeto de canalización publicado. También puede encontrar la dirección URL de REST en el área de trabajo del portal. Cree una solicitud HTTP POST al punto de conexión y especifique el encabezado de autenticación. Además, agregue un objeto de carga JSON con el nombre del experimento y el parámetro de tamaño de lote. Como recordatorio, `param_batch_size` se pasa a su script `batch_scoring.py` porque lo definió como un objeto `PipelineParameter` en la configuración del paso.

Realice la solicitud para desencadenar la ejecución. Acceda a la clave `Id` desde el diccionario de respuesta para obtener el valor del identificador de ejecución.

```python
import requests

rest_endpoint = published_pipeline.endpoint
response = requests.post(rest_endpoint, 
                         headers=auth_header, 
                         json={"ExperimentName": "batch_scoring",
                               "ParameterAssignments": {"param_batch_size": 50}})
run_id = response.json()["Id"]
```

Use el identificador de ejecución para supervisar el estado de la nueva ejecución. Esta operación tardará otros 10 o 15 minutos en ejecutarse y se parecerá a la ejecución de la canalización anterior por lo que, si no necesita ver otra ejecución de la canalización, puede omitir la visualización de la salida completa.

```python
from azureml.pipeline.core.run import PipelineRun
from azureml.widgets import RunDetails

published_pipeline_run = PipelineRun(ws.experiments["batch_scoring"], run_id)
RunDetails(published_pipeline_run).show()
```

## <a name="clean-up-resources"></a>Limpieza de recursos

No complete esta sección si planea ejecutar otros tutoriales de Azure Machine Learning.

### <a name="stop-the-notebook-vm"></a>Detención de la máquina virtual de Notebook

Si usó un servidor de cuadernos en la nube, detenga la máquina virtual cuando no la esté usando a fin de reducir el costo.

1. En el área de trabajo, seleccione **Máquinas virtuales de Notebook**.
1. En la lista, seleccione la máquina virtual.
1. Seleccione **Detener**.
1. Cuando esté listo para volver a usar el servidor, seleccione **Iniciar**.

### <a name="delete-everything"></a>Eliminar todo el contenido

Si no va a usar los recursos creados, elimínelos para no incurrir en cargos.

1. En Azure Portal, seleccione **Grupos de recursos** a la izquierda del todo.
1. En la lista, seleccione el grupo de recursos que creó.
1. Seleccione **Eliminar grupo de recursos**.
1. Escriba el nombre del grupo de recursos. A continuación, seleccione **Eliminar**.

También puede mantener el grupo de recursos pero eliminar una sola área de trabajo. Muestre las propiedades del área de trabajo y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial de canalizaciones de aprendizaje automático, realizó las siguientes tareas:

> [!div class="checklist"]
> * Compiló una canalización con dependencias del entorno para ejecutarse en un recurso de proceso de GPU remota.
> * Creó un script de puntuación para ejecutar predicciones por lotes con un modelo Tensorflow entrenado previamente.
> * Publicó una canalización y la habilitó para ejecutarla desde un punto de conexión REST.

Consulte el [repositorio de cuadernos](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines) para ver otros ejemplos de creación de canalizaciones con el SDK de aprendizaje automático.
