---
title: Entrenamiento de la red neuronal de aprendizaje profundo con Keras
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo entrenar y registrar un modelo de clasificación de red neuronal profunda Keras que se ejecuta en TensorFlow con Azure Machine Learning Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.reviewer: peterlu
ms.date: 08/01/2019
ms.custom: seodec18
ms.openlocfilehash: bfe7f975539c76c1d369d111729820f4d0ada470
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2019
ms.locfileid: "68775077"
---
# <a name="train-and-register-a-keras-classification-model-with-azure-machine-learning-service"></a>Entrenamiento y registro de un modelo de clasificación Keras con Azure Machine Learning Service

En este artículo se muestra cómo entrenar y registrar un modelo de clasificación Keras creado en TensorFlow con Azure Machine Learning Service. Se usa el popular [conjunto de datos MNIST](http://yann.lecun.com/exdb/mnist/) para clasificar dígitos escritos a mano mediante una red neuronal profunda (DNN) creada con la [biblioteca Keras de Python](https://keras.io) que se ejecuta sobre [TensorFlow](https://www.tensorflow.org/overview).

Keras es una API de red neuronal general capaz de ejecutarse sobre otros marcos DNN conocidos para simplificar el desarrollo. Con Azure Machine Learning Service, puede escalar horizontalmente con rapidez trabajos de entrenamiento mediante recursos de proceso elásticos en la nube. También puede realizar el seguimiento de las ejecuciones de entrenamiento, los modelos de versión, implementar modelos y mucho más.

Con independencia de que desarrolle un modelo Keras desde el principio o lleve uno existente a la nube, el servicio Azure Machine Learning le puede ayudar a crear modelos para entornos de producción.

Consulte el [artículo conceptual](concept-deep-learning-vs-machine-learning.md) para información sobre las diferencias entre el aprendizaje automático y el aprendizaje profundo.

## <a name="prerequisites"></a>Requisitos previos

Ejecute este código en cualquiera de estos entornos:

 - Máquina virtual de Notebook de Azure Machine Learning: no se necesitan descargas ni instalación

     - Complete el [Tutorial: Configuración del entorno y el área de trabajo](tutorial-1st-experiment-sdk-setup.md) para crear un servidor de cuadernos dedicado en el que se habrán cargado previamente el SDK y el repositorio de ejemplos.
    - En la carpeta de ejemplos del servidor de cuadernos, vaya a este directorio: carpeta **how-to-use-azureml > training-with-deep-learning > train-hyperparameter-tune-deploy-with-keras**, para encontrar un cuaderno completado y expandido.

 - Su propio servidor de Jupyter Notebook

     - [Instalación del SDK de Azure Machine Learning para Python](setup-create-workspace.md#sdk)
    - [Creación de un archivo de configuración del área de trabajo](setup-create-workspace.md#write-a-configuration-file)
    - [Descarga de los archivos de script de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras) `mnist-keras.py` y `utils.py`

    También puede encontrar una [versión de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb) completada de esta guía en la página de ejemplos de GitHub. El cuaderno incluye secciones expandidas que abarcan el ajuste de hiperparámetros inteligente, la implementación de modelos y los widgets del cuaderno.

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

Cree un objeto de área de trabajo a partir del archivo `config.json` creado en la [sección de requisitos previos](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-an-experiment"></a>Creación de un experimento

Cree un experimento y una carpeta para almacenar los scripts de entrenamiento. En este ejemplo, cree un experimento denominado "keras-mnist".

```Python
script_folder = './keras-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='keras-mnist')
```

### <a name="upload-dataset-and-scripts"></a>Carga del conjunto de datos y los scripts

El [almacén de datos](how-to-access-data.md) es un lugar donde se pueden almacenar datos y acceder a ellos mediante su montaje o copia en el destino de proceso. Cada área de trabajo proporciona un almacén de datos predeterminado. Cargue los datos y scripts de entrenamiento al almacén de datos para que sean fácilmente accesibles durante el entrenamiento.

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

1. Cargue el script de entrenamiento de Keras, `keras_mnist.py`, y el archivo auxiliar, `utils.py`.

    ```Python
    shutil.copy('./keras_mnist.py', script_folder)
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

Para más información sobre los destinos de proceso, vea el artículo [¿Qué es un destino de proceso?](concept-compute-target.md)

## <a name="create-a-tensorflow-estimator-and-import-keras"></a>Creación de un estimador de TensorFlow e importación de Keras

El [estimador de TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) proporciona una manera sencilla de iniciar trabajos de entrenamiento de TensorFlow en el destino de proceso. Como Keras se ejecuta sobre TensorFlow, puede usar el estimador de TensorFlow e importar la biblioteca de Keras mediante el argumento `pip_packages`.

El estimador de TensorFlow se implementa a través de la clase genérica [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py), que se puede usar para admitir cualquier marco de trabajo. Además, cree un diccionario `script_params` que contenga la configuración del hiperparámetro DNN. Para obtener más información sobre cómo entrenar modelos con el estimador genérico, consulte [Entrenamiento de modelos con Azure Machine Learning mediante un objeto Estimator](how-to-train-ml-models.md).

```Python
script_params = {
    '--data-folder': ds.path('mnist').as_mount(),
    '--batch-size': 50,
    '--first-layer-neurons': 300,
    '--second-layer-neurons': 100,
    '--learning-rate': 0.001
}

est = TensorFlow(source_directory=script_folder,
                 entry_script='keras_mnist.py',
                 script_params=script_params,
                 compute_target=compute_target,
                 pip_packages=['keras', 'matplotlib'],
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

- **Posprocesamiento**: La carpeta ./outputs de la ejecución se copia en el historial de ejecución.

## <a name="register-the-model"></a>Registro del modelo

Una vez que haya entrenado el modelo de DNN, puede registrarlo en el área de trabajo. El registro del modelo permite almacenar los modelos y crear versiones de ellos en el área de trabajo para simplificar la [administración e implementación de modelos](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='keras-dnn-mnist', model_path='outputs/model')
```

También puede descargar una copia local del modelo. Esto puede ser útil para realizar trabajos de validación de modelo adicionales de forma local. En el script de entrenamiento, `mnist-keras.py`, un objeto de protector de TensorFlow guarda el modelo en una carpeta local (local para el destino de proceso). Puede usar el objeto Run para descargar una copia del almacén de datos.

```Python
# Create a model folder in the current directory
os.makedirs('./model', exist_ok=True)

for f in run.get_file_names():
    if f.startswith('outputs/model'):
        output_file_path = os.path.join('./model', f.split('/')[-1])
        print('Downloading from {} to {} ...'.format(f, output_file_path))
        run.download_file(name=f, output_file_path=output_file_path)
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha entrenado y registrado un modelo Keras en el servicio Azure Machine Learning. Para obtener información sobre cómo implementar un modelo, continúe con nuestro artículo sobre implementación de modelos.

> [!div class="nextstepaction"]
> [Cómo y dónde implementar los modelos](how-to-deploy-and-where.md)
