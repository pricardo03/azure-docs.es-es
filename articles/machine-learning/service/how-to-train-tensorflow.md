---
title: Entrenamiento y registro de modelos de TensorFlow
titleSuffix: Azure Machine Learning service
description: En este artículo se muestra cómo entrenar y registrar un modelo de TensorFlow mediante Azure Machine Learning Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 06/10/2019
ms.custom: seodec18
ms.openlocfilehash: 67263df319063cdf21dadea257dcab05ba0d5f7b
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839993"
---
# <a name="train-and-register-tensorflow-models-at-scale-with-azure-machine-learning-service"></a>Entrenamiento y registro de modelos de TensorFlow a escala con Azure Machine Learning Service

En este artículo se muestra cómo entrenar y registrar un modelo de TensorFlow mediante Azure Machine Learning Service. Se usa el popular [conjunto de datos MNIST](http://yann.lecun.com/exdb/mnist/) para clasificar dígitos escritos a mano mediante una red neuronal profunda creada con la [biblioteca TensorFlow de Python](https://www.tensorflow.org/overview).

TensorFlow es un marco de proceso de código abierto utilizado habitualmente para crear redes neuronales profundas (DNN). Con Azure Machine Learning Service, puede escalar horizontalmente con rapidez trabajos de entrenamiento de código abierto mediante recursos de proceso elásticos en la nube. También puede realizar un seguimiento de las ejecuciones de entrenamientos, los modelos de versión, los modelos de implementación y mucho más.

Con independencia de que desarrolle un modelo de TensorFlow desde el principio o lleve un [modelo existente](how-to-deploy-existing-model.md) a la nube, Azure Machine Learning Service le puede ayudar a crear modelos para entornos de producción.

## <a name="prerequisites"></a>Requisitos previos

Ejecute este código en uno de estos entornos:

 - Máquina virtual de cuadernos de Azure Machine Learning: no se necesitan descargas ni instalación

     - Complete el [inicio rápido de cuadernos basados en la nube](quickstart-run-cloud-notebook.md) para crear un servidor de cuadernos dedicado en el que se habrán cargado previamente el SDK y el repositorio de ejemplos.
    - En la carpeta de ejemplos en el servidor de cuadernos, vaya a este directorio: carpeta **how-to-use-azureml > training-with-deep-learning > train-hyperparameter-tune-deploy-with-tensorflow**, para encontrar un cuaderno completado y expandido. 
 
 - Su propio servidor de Jupyter Notebook

     - [Instalación del SDK de Azure Machine Learning para Python](setup-create-workspace.md#sdk)
    - [Creación de un archivo de configuración del área de trabajo](setup-create-workspace.md#write-a-configuration-file)
    - [Descarga de los archivos de script de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow) `mnist-tf.py` y `utils.py`
     
    También puede encontrar una [versión de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/train-hyperparameter-tune-deploy-with-tensorflow.ipynb) completada de esta guía en la página de ejemplos de GitHub. El cuaderno incluye secciones expandidas que abarcan el ajuste de hiperparámetros inteligente, la implementación de modelos y los widgets del cuaderno.

## <a name="set-up-the-experiment"></a>Configuración del experimento

En esta sección, para configurar el experimento de entrenamiento, se cargan los paquetes de Python necesarios, se inicializa un área de trabajo, se crea un experimento y se cargan los datos y scripts de entrenamiento.

### <a name="import-packages"></a>Importación de paquetes

En primer lugar, importe las bibliotecas de Python necesarias.

```Python
import os
import urllib
import shutil
import azureml

from azureml.core import Experiment
from azureml.core import Workspace, Run

from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException
```

### <a name="initialize-a-workspace"></a>Inicialización de un área de trabajo

El [área de trabajo de Azure Machine Learning Service](concept-workspace.md) es el recurso de nivel superior del servicio. Proporciona un lugar centralizado para trabajar con todos los artefactos que cree. En el SDK de Python, puede acceder a los artefactos del área de trabajo mediante la creación de un objeto [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py).

Cree un objeto del área de trabajo a partir del archivo `config.json` creado en la [sección de requisitos previos](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-an-experiment"></a>Creación de un experimento

Cree un experimento y una carpeta para almacenar los scripts de entrenamiento. En este ejemplo, cree un experimento llamado "tf-mnist".

```Python
script_folder = './tf-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='tf-mnist')
```

### <a name="upload-dataset-and-scripts"></a>Carga del conjunto de datos y los scripts

El [almacén de datos](how-to-access-data.md) es un lugar donde se pueden almacenar datos y acceder a ellos mediante el montaje o la copia de los datos en el destino de proceso. Cada área de trabajo proporciona un almacén de datos predeterminado. Cargue los datos y scripts de entrenamiento al almacén de datos para que sean fácilmente accesibles durante el entrenamiento.

1. Descargue el conjunto de datos de MNIST al entorno local.

    ```Python
    os.makedirs('./data/mnist', exist_ok=True)

    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename = './data/mnist/train-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename = './data/mnist/train-labels.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/mnist/test-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename = './data/mnist/test-labels.gz')
    ```

1. Cargue el conjunto de datos de MNIST al almacén de datos predeterminado.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/mnist', target_path='mnist', overwrite=True, show_progress=True)
    ```

1. Cargue el script de entrenamiento de TensorFlow `tf_mnist.py` y el archivo auxiliar, `utils.py`.

    ```Python
    shutil.copy('./tf_mnist.py', script_folder)
    shutil.copy('./utils.py', script_folder)
    ```

## <a name="create-a-compute-target"></a>Creación de un destino de proceso

Cree un destino de proceso en el que ejecutar el trabajo de TensorFlow. En este ejemplo, cree un clúster de proceso de Azure Machine Learning habilitado para GPU.

```Python
cluster_name = "gpucluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Para más información sobre los destinos de proceso, consulte el artículo [¿Qué es un destino de proceso?](concept-compute-target.md)

## <a name="create-a-tensorflow-estimator"></a>Creación de un estimador de TensorFlow

El [estimador de TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) proporciona una manera sencilla de iniciar un trabajo de entrenamiento de TensorFlow en un destino de proceso.

El estimador de TensorFlow se implementa a través de la clase genérica [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py), que se puede usar para admitir cualquier marco de trabajo. Para más información sobre cómo entrenar modelos con el estimador genérico, consulte [Entrenamiento de modelos con Azure Machine Learning mediante un estimador](how-to-train-ml-models.md).

Si el script de entrenamiento necesita paquetes adicionales pip o conda para ejecutarse, puede hacer que los paquetes se instalen en la imagen de Docker resultante. Para ello, pase sus nombres mediante los argumentos `pip_packages` y `conda_packages`.

```Python
script_params = {
    '--data-folder': ws.get_default_datastore().as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.01
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='tf_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
                 use_gpu=True)
```

## <a name="submit-a-run"></a>Envío de una ejecución

El [objeto Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) proporciona la interfaz para el historial de ejecución mientras se ejecuta el trabajo y cuando se ha completado.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Durante la ejecución de Run, pasa por las fases siguientes:

- **Preparación**: se crea una imagen de Docker según el estimador de TensorFlow. La imagen se carga en el registro de contenedor del área de trabajo y se almacena en memoria caché para ejecuciones posteriores. Los registros también se transmiten al historial de ejecución y se pueden consultar para supervisar el progreso.

- **Escalado**: el clúster intenta escalar verticalmente si el clúster de Batch AI requiere más nodos para realizar la ejecución de los que se encuentran disponibles actualmente.

- **Running**: todos los scripts de la carpeta de scripts se cargan en el destino de proceso, se montan o se copian los almacenes de datos y se ejecuta entry_script. Las salidas de stdout y la carpeta ./logs se transmiten al historial de ejecución y se pueden usar para supervisar la ejecución.

- **Posprocesamiento**: la carpeta ./outputs de la ejecución se copia en el historial de ejecución.

## <a name="register-or-download-a-model"></a>Registro o descarga de un modelo

Una vez que haya entrenado el modelo, puede registrarlo en el área de trabajo. El registro del modelo permite almacenar los modelos y crear versiones de ellos en el área de trabajo para simplificar la [administración e implementación de modelos](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='tf-dnn-mnist', model_path='outputs/model')
```

También puede descargar una copia local del modelo mediante el uso del objeto Run. En el script de entrenamiento, `mnist-tf.py`, un objeto de protector de TensorFlow guarda el modelo en una carpeta local (local para el destino de proceso). Puede usar el objeto Run para descargar una copia.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="distributed-training"></a>Entrenamiento distribuido

El estimador [`TensorFlow`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) también admite el entrenamiento distribuido en clústeres de CPU y GPU. Puede ejecutar fácilmente los trabajos distribuidos de TensorFlow y Azure Machine Learning Service se ocupará de administrar la orquestación.

Azure Machine Learning Service admite dos métodos de entrenamiento distribuido en TensorFlow:

- Entrenamiento distribuido [basado en MPI](https://www.open-mpi.org/) mediante el marco [Horovod](https://github.com/uber/horovod)
- [TensorFlow distribuido](https://www.tensorflow.org/deploy/distributed) nativo mediante el método de servidor de parámetros

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod) es un marco de código abierto para el entrenamiento distribuido desarrollado por Uber. Ofrece una ruta de acceso fácil a los trabajos de GPU distribuidos de TensorFlow.

Para usar Horovod, especifique un objeto [`MpiConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.mpiconfiguration?view=azure-ml-py) para el parámetro `distributed_training` en el constructor de TensorFlow. Este parámetro garantiza que se instala la biblioteca de Horovod para su uso en el script de entrenamiento.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=MpiConfiguration(),
                      framework_version='1.13',
                      use_gpu=True)
```

### <a name="parameter-server"></a>Servidor de parámetro

También puede ejecutar [TensorFlow distribuido nativo](https://www.tensorflow.org/deploy/distributed), que utiliza el modelo de servidor del parámetro. En este método, entrena a través de un clúster de servidores de parámetro y los trabajadores. Los trabajadores calculan los degradados durante el entrenamiento, mientras que los servidores de parámetro agregan los degradados.

Para usar el método del servidor de parámetros, especifique un objeto [`TensorflowConfiguration`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.runconfig.tensorflowconfiguration?view=azure-ml-py) para el parámetro `distributed_training` en el constructor de TensorFlow.

```Python
from azureml.train.dnn import TensorFlow

distributed_training = TensorflowConfiguration()
distributed_training.worker_count = 2

# Tensorflow constructor
estimator= TensorFlow(source_directory=project_folder,
                      compute_target=compute_target,
                      script_params=script_params,
                      entry_script='script.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_training=distributed_training,
                      use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="define-cluster-specifications-in-tfconfig"></a>Definición de las especificaciones del clúster en "TF_CONFIG"

También necesitará las direcciones de red y los puertos del clúster para [`tf.train.ClusterSpec`](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), por lo que Azure Machine Learning establece la variable de entorno `TF_CONFIG` por usted.

La variable de entorno `TF_CONFIG` es una cadena JSON. Este es un ejemplo de la variable para un servidor de parámetro:

```JSON
TF_CONFIG='{
    "cluster": {
        "ps": ["host0:2222", "host1:2222"],
        "worker": ["host2:2222", "host3:2222", "host4:2222"],
    },
    "task": {"type": "ps", "index": 0},
    "environment": "cloud"
}'
```

Para la API [`tf.estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator) de alto nivel de TensorFlow, TensorFlow analizará la variable `TF_CONFIG` y creará la especificación del clúster automáticamente.

Para la API básica de nivel inferior de TensorFlow para el entrenamiento, analice la variable `TF_CONFIG` y cree `tf.train.ClusterSpec` en el código de entrenamiento.

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha entrenado y registrado un modelo de TensorFlow. Para obtener información sobre cómo implementar un modelo en un clúster habilitado para GPU, continúe con nuestro artículo de implementación del modelo GPU.

[Cómo implementar para la inferencia con GPU](how-to-deploy-inferencing-gpus.md)
[Cómo supervisar con Tensorboard](how-to-monitor-tensorboard.md)
