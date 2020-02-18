---
title: 'Tutorial: Canalizaciones de ML para la puntuación por lotes'
titleSuffix: Azure Machine Learning
description: En este tutorial, compilará una canalización de aprendizaje automático para ejecutar puntuaciones por lotes en un modelo de clasificación de imágenes en Azure Machine Learning. Las canalizaciones de aprendizaje automático optimizan el flujo de trabajo con velocidad, portabilidad y reutilización para que pueda centrarse en sus conocimientos, el aprendizaje automático, en lugar de hacerlo en la infraestructura y la automatización.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: tutorial
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.date: 02/10/2020
ms.openlocfilehash: 3dc0af3f0d1236e902f6fa845fae95e3f2a500d1
ms.sourcegitcommit: 7c18afdaf67442eeb537ae3574670541e471463d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2020
ms.locfileid: "77116469"
---
# <a name="tutorial-build-an-azure-machine-learning-pipeline-for-batch-scoring"></a>Tutorial: Compilación de una canalización de Azure Machine Learning para la puntuación por lotes

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este tutorial usará una canalización de Azure Machine Learning para ejecutar un trabajo de puntuación por lotes. En este ejemplo se usa el modelo Tensorflow de red neuronal de circunvolución [Inception-V3](https://arxiv.org/abs/1512.00567) para clasificar las imágenes sin etiquetar. Después de compilar y publicar una canalización, configurará un punto de conexión REST para desencadenar la canalización desde cualquier biblioteca HTTP en cualquier plataforma.

Las canalizaciones de aprendizaje automático optimizan el flujo de trabajo con velocidad, portabilidad y reutilización para que pueda centrarse en sus conocimientos, el aprendizaje automático, en lugar de hacerlo en la infraestructura y la automatización. [Más información sobre las canalizaciones de aprendizaje automático](concept-ml-pipelines.md).

En este tutorial, va a completar las siguientes tareas:

> [!div class="checklist"]
> * Configurar el área de trabajo y descargar datos de ejemplo
> * Crear objetos de datos para capturar y generar datos
> * Descargar, preparar y registrar el modelo en el área de trabajo
> * Aprovisionar destinos de proceso y crear un script de puntuación
> * Compilar, ejecutar y publicar una canalización
> * Habilitar un punto de conexión REST para la canalización

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Prerrequisitos

* Complete la [parte 1 del tutorial de instalación](tutorial-1st-experiment-sdk-setup.md) si aún no tiene un área de trabajo de Azure Machine Learning o una máquina virtual de cuadernos.
* Una vez terminado el tutorial de instalación, use el mismo servidor de cuadernos para abrir el cuaderno *tutorials/machine-learning-pipelines-advanced/tutorial-pipeline-batch-scoring-classification.ipynb*.

Si desea ejecutar el tutorial de instalación en su propio [entorno local](how-to-configure-environment.md#local) puede acceder al tutorial en [GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/tutorials). Ejecute `pip install azureml-sdk[notebooks] azureml-pipeline-core azureml-pipeline-steps pandas requests` para obtener los paquetes requeridos.

## <a name="configure-workspace-and-create-a-datastore"></a>Configuración del área de trabajo y creación de un almacén de datos

Cree un objeto de área de trabajo a partir del área de trabajo de Azure Machine Learning existente.

- Un [área de trabajo](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) es una clase que acepta la información de recursos y suscripciones de Azure. También crea un recurso en la nube que puede usar para supervisar y realizar el seguimiento de las ejecuciones del modelo. 
- `Workspace.from_config()` lee el archivo `config.json` y carga los detalles de autenticación en un objeto denominado `ws`. El objeto `ws` se usa en el código en todo el tutorial.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

### <a name="create-a-datastore-for-sample-images"></a>Creación de un almacén de datos para las imágenes de ejemplo

En la cuenta `pipelinedata`, obtenga el ejemplo de datos públicos de evaluación de ImageNet del contenedor de blobs público `sampledata`. Una llamada a `register_azure_blob_container()` hace que los datos estén disponibles en el área de trabajo con el nombre `images_datastore`. A continuación, establezca el almacén de datos predeterminado del área de trabajo como almacén de datos de salida y úselo para puntuar las salidas de la canalización.

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

Al compilar una canalización, un objeto `DataReference` lee los datos del almacén de datos del área de trabajo. Un objeto `PipelineData` transfiere los datos intermedios entre los pasos de canalización.

> [!Important]
> En el ejemplo de puntuación por lotes de este tutorial solo se usa un paso de canalización. En los casos de uso con varios pasos, el flujo típico incluirá los siguientes:
>
> 1. Use objetos `DataReference` como *entradas* para capturar datos sin procesar, realizar algunas transformaciones y generar un objeto `PipelineData` de *salida*.
>
> 2. Use el `PipelineData` *objeto de salida* del paso anterior como *objeto de entrada*. Repítalo en pasos posteriores.

En este escenario se crean objetos `DataReference` correspondientes a los directorios del almacén de datos para las imágenes de entrada y las etiquetas de clasificación (valores de la prueba de la y). También se crea un objeto `PipelineData` para los datos de salida de puntuación por lotes.

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

Descargue el modelo Tensorflow previamente entrenado para usarlo en la puntuación por lotes de una canalización. En primer lugar, cree un directorio local donde almacenar el modelo y, luego, descárguelo y extráigalo.

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

A continuación registrará el modelo en el área de trabajo para poder recuperarlo fácilmente en el proceso de canalización. En la función estática `register()`, el parámetro `model_name` es la clave que se usa para buscar el modelo en todo el SDK.

```python
from azureml.core.model import Model
 
model = Model.register(model_path="models/inception_v3.ckpt",
                       model_name="inception",
                       tags={"pretrained": "inception"},
                       description="Imagenet trained tensorflow inception",
                       workspace=ws)
```

## <a name="create-and-attach-the-remote-compute-target"></a>Creación y asociación del destino de proceso remoto

Las canalizaciones de aprendizaje automático no se pueden ejecutar en un entorno local, por lo que debe ejecutarlas en recursos en la nube o *destinos de proceso remotos*. Un destino de proceso remoto es un entorno de proceso virtual reutilizable en el que se ejecutan los experimentos y los flujos de trabajo de aprendizaje automático. 

Ejecute el código siguiente para crear un destino [`AmlCompute`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.amlcompute.amlcompute?view=azure-ml-py) habilitado para GPU y asócielo al área de trabajo. Consulte el [artículo conceptual](https://docs.microsoft.com/azure/machine-learning/concept-compute-target) para más información sobre los destinos de proceso.


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

Para realizar la puntuación, cree un script de puntuación por lotes llamado `batch_scoring.py` y escríbalo en el directorio actual. El script toma imágenes de entrada, aplica el modelo de clasificación y envía las predicciones a un archivo de resultados.

El script `batch_scoring.py` toma los siguientes parámetros, que se pasan desde el paso de canalización que se crea más adelante en este tutorial:

- `--model_name`: nombre del modelo que se usa.
- `--label_dir`: directorio que contiene el archivo `labels.txt`.
- `--dataset_path`: directorio que contiene las imágenes de entrada.
- `--output_dir`: directorio de salida del archivo `results-label.txt` después de que el script ejecute el modelo en los datos.
- `--batch_size`: tamaño del lote que se usa para ejecutar el modelo.

La infraestructura de la canalización usa la clase `ArgumentParser` para pasar los parámetros a los pasos de canalización. Por ejemplo, en el código siguiente, al primer argumento `--model_name` se le da el identificador de propiedad `model_name`. En la función `main()` se utiliza `Model.get_model_path(args.model_name)` para acceder a esta propiedad.


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
> La canalización de este tutorial solo tiene un paso y escribe la salida en un archivo. En el caso de las canalizaciones de varios pasos, también se usa `ArgumentParser` para definir un directorio en el que escribir los datos de salida para la entrada en pasos posteriores. Consulte el [cuaderno`ArgumentParser` para ver un ejemplo de cómo pasar datos entre varios pasos de la canalización mediante el modelo de diseño ](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/nyc-taxi-data-regression-model-building/nyc-taxi-data-regression-model-building.ipynb).

## <a name="build-and-run-the-pipeline"></a>Compilación y ejecución de la canalización

Antes de ejecutar la canalización, cree un objeto que defina el entorno de Python y cree las dependencias que necesita el script `batch_scoring.py`. La dependencia principal requerida es Tensorflow, pero también se instala `azureml-defaults` para los procesos en segundo plano desde el SDK. Cree un objeto `RunConfiguration` con las dependencias. Además, especifique la compatibilidad con Docker y Docker-GPU.

```python
from azureml.core.runconfig import DEFAULT_GPU_IMAGE
azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

cd = CondaDependencies.create(pip_packages=["tensorflow-gpu==1.13.1", "azureml-defaults"])

amlcompute_run_config = RunConfiguration(conda_dependencies=cd)
amlcompute_run_config.environment.docker.enabled = True
amlcompute_run_config.environment.docker.base_image = DEFAULT_GPU_IMAGE
amlcompute_run_config.environment.spark.precache_packages = False
```

### <a name="parameterize-the-pipeline"></a>Parametrización de la canalización

Defina un parámetro personalizado para que la canalización controle el tamaño del lote. Una vez publicada la canalización y expuesta mediante un punto de conexión REST, también se exponen los parámetros configurados. Puede especificar parámetros personalizados en la carga de JSON cuando vuelva a ejecutar la canalización mediante una solicitud HTTP.

Cree un objeto `PipelineParameter` para habilitar este comportamiento y definir un nombre y un valor predeterminado.

```python
from azureml.pipeline.core.graph import PipelineParameter
batch_size_param = PipelineParameter(name="param_batch_size", default_value=20)
```

### <a name="create-the-pipeline-step"></a>Creación del paso de canalización

Un paso de canalización es un objeto que encapsula todo lo que necesita para ejecutar una canalización, por ejemplo:

* Configuración del entorno y las dependencias
* El recurso de proceso en el que se ejecutará la canalización
* Los datos de entrada y salida, y cualquier parámetro personalizado
* Referencia a un script o a una lógica del SDK que se ejecutará durante el paso

Varias clases heredan de la clase primaria [`PipelineStep`](https://docs.microsoft.com/python/api/azureml-pipeline-core/azureml.pipeline.core.builder.pipelinestep?view=azure-ml-py). Puede elegir clases para usar marcos o pilas concretos para compilar un paso. En este ejemplo, se usa la clase [`PythonScriptStep`](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps.python_script_step.pythonscriptstep?view=azure-ml-py) para definir la lógica del paso con un script de Python personalizado. Si un argumento del script es una entrada al paso o una salida del paso, se debe definir el argumento *tanto* en la matriz `arguments`, *como* en el parámetro `input` o `output`, respectivamente. 

En escenarios con más de un paso, las referencias a un objeto de la matriz `outputs` estarán disponibles como *entrada* en un paso posterior de la canalización.

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

Para ver una lista de todas las clases que se pueden usar para los diferentes tipos de pasos, consulte el [paquete de pasos](https://docs.microsoft.com/python/api/azureml-pipeline-steps/azureml.pipeline.steps?view=azure-ml-py).

### <a name="run-the-pipeline"></a>Ejecución de la canalización

Ahora ejecute la canalización. En primer lugar, cree un objeto `Pipeline` con la referencia al área de trabajo y el paso de canalización que creó. El parámetro `steps` es una matriz de pasos. En este caso, la puntuación por lotes consta de un solo paso. Para compilar canalizaciones con varios pasos, colóquelos en orden en esta matriz.

Luego, use la función `Experiment.submit()` para enviar la canalización para su ejecución. También se especifica el parámetro personalizado `param_batch_size`. La función `wait_for_completion` genera registros durante el proceso de compilación de la canalización que puede usar para ver el progreso.

> [!IMPORTANT]
> La primera ejecución de canalización tarda aproximadamente *15 minutos*. Se deben descargar todas las dependencias, se crea una imagen de Docker y se aprovisiona y se crea el entorno de Python. La repetición de la ejecución de la canalización tarda mucho menos, ya que esos recursos se reutilizan en lugar de crearse. Sin embargo, el tiempo de ejecución total de la canalización depende de la carga de trabajo de los scripts y de los procesos que se ejecutan en cada paso de la canalización.

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
      <td>Rhodesian Ridgeback</td>
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

## <a name="publish-and-run-from-a-rest-endpoint"></a>Publicación y ejecución desde un punto de conexión REST

Ejecute el código siguiente para publicar la canalización en el área de trabajo. En el área de trabajo de Azure Machine Learning Studio puede ver los metadatos de la canalización, incluidos el historial de ejecución y las duraciones. También puede ejecutar la canalización manualmente desde Studio.

La publicación de la canalización permite a un punto de conexión REST volver a ejecutar la canalización desde cualquier biblioteca HTTP en cualquier plataforma.

```python
published_pipeline = pipeline_run.publish_pipeline(
    name="Inception_v3_scoring", description="Batch scoring using Inception v3 model", version="1.0")

published_pipeline
```

Para ejecutar la canalización desde el punto de conexión REST, necesita un encabezado de autenticación de tipo portador de OAuth2. En el ejemplo siguiente se usa la autenticación interactiva, con fines ilustrativos; en la mayoría de los escenarios de producción que requieren autenticación automatizada o desatendida, use la autenticación de la entidad de servicio tal y como se [describe en este cuaderno](https://aka.ms/pl-restep-auth).

La autenticación de la entidad de servicio implica la creación de un *Registro de aplicación* en *Azure Active Directory*. Primero genere un secreto de cliente y, a continuación, conceda a la entidad de servicio el *acceso de rol* al área de trabajo de aprendizaje automático. Use la clase [`ServicePrincipalAuthentication`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.serviceprincipalauthentication?view=azure-ml-py) para administrar el flujo de autenticación. 

Tanto [`InteractiveLoginAuthentication`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.interactiveloginauthentication?view=azure-ml-py) como `ServicePrincipalAuthentication` heredan de `AbstractAuthentication`. En ambos caso, use la función [`get_authentication_header()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.authentication.abstractauthentication?view=azure-ml-py#get-authentication-header--) de la misma manera para capturar el encabezado:

```python
from azureml.core.authentication import InteractiveLoginAuthentication

interactive_auth = InteractiveLoginAuthentication()
auth_header = interactive_auth.get_authentication_header()
```

Obtenga la dirección URL de REST de la propiedad `endpoint` del objeto de canalización publicado. También puede encontrar la dirección URL de REST en el área de trabajo de Azure Machine Learning Studio. 

Compile una solicitud HTTP POST para el punto de conexión. Especifique el encabezado de autenticación en la solicitud. Agregue un objeto de carga JSON con el nombre del experimento y el parámetro de tamaño de lote. Como se indicó anteriormente en el tutorial, `param_batch_size` se pasa a su script `batch_scoring.py`, ya que se definió como objeto `PipelineParameter` en la configuración del paso.

Realice la solicitud para desencadenar la ejecución. Incluya código para acceder a la clave `Id` desde el diccionario de respuestas para obtener el valor del identificador de ejecución.

```python
import requests

rest_endpoint = published_pipeline.endpoint
response = requests.post(rest_endpoint, 
                         headers=auth_header, 
                         json={"ExperimentName": "batch_scoring",
                               "ParameterAssignments": {"param_batch_size": 50}})
run_id = response.json()["Id"]
```

Use el identificador de ejecución para supervisar el estado de la nueva ejecución. La nueva ejecución tardará otros 10-15 minutos 

y su aspecto será similar al de la canalización que se ejecutó anteriormente en el tutorial. Puede elegir no ver la salida completa.

```python
from azureml.pipeline.core.run import PipelineRun
from azureml.widgets import RunDetails

published_pipeline_run = PipelineRun(ws.experiments["batch_scoring"], run_id)
RunDetails(published_pipeline_run).show()
```

## <a name="clean-up-resources"></a>Limpieza de recursos

No complete esta sección si planea ejecutar otros tutoriales de Azure Machine Learning.

### <a name="stop-the-compute-instance"></a>Detención de la instancia de proceso

[!INCLUDE [aml-stop-server](../../includes/aml-stop-server.md)]

### <a name="delete-everything"></a>Eliminar todo el contenido

Si no va a usar los recursos creados, elimínelos para no incurrir en cargos:

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
1. En la lista de grupos de recursos, seleccione el que ha creado.
1. Seleccione **Eliminar grupo de recursos**.
1. Escriba el nombre del grupo de recursos. A continuación, seleccione **Eliminar**.

También puede mantener el grupo de recursos pero eliminar una sola área de trabajo. Muestre las propiedades del área de trabajo y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial de canalizaciones de aprendizaje automático, realizó las siguientes tareas:

> [!div class="checklist"]
> * Compiló una canalización con dependencias del entorno para ejecutarse en un recurso de proceso de GPU remota.
> * Creó un script de puntuación para ejecutar predicciones por lotes con un modelo Tensorflow entrenado previamente.
> * Publicó una canalización y la habilitó para ejecutarla desde un punto de conexión REST.

Para más ejemplos de cómo crear canalizaciones mediante el SDK de aprendizaje automático, consulte el [repositorio de cuadernos](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines).
