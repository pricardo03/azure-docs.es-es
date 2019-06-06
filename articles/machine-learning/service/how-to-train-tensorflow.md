---
title: Entrenar y registre los modelos de TensorFlow
titleSuffix: Azure Machine Learning service
description: Este artículo muestra cómo entrenar y registrar un modelo de TensorFlow con el servicio Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.date: 05/28/2019
ms.custom: seodec18
ms.openlocfilehash: 4a6f9734a7b2b59035efcbb0f4e2d75f47e053be
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66515593"
---
# <a name="train-and-register-tensorflow-models-at-scale-with-azure-machine-learning-service"></a>Entrenar y registrar TensorFlow modelos a escala con el servicio Azure Machine Learning

Este artículo muestra cómo entrenar y registrar un modelo de TensorFlow con el servicio Azure Machine Learning. Vamos a usar el popular [conjunto de datos MNIST](http://yann.lecun.com/exdb/mnist/) para clasificar los dígitos escritos a mano mediante una red neuronal profunda compilada con la [biblioteca de TensorFlow Python](https://www.tensorflow.org/overview).

Con el servicio de Azure Machine Learning, podrá escalar rápidamente sus trabajos de aprendizaje de código abierto con los recursos de proceso en la nube elástica. También podrá realizar un seguimiento de sus ejecuciones de entrenamiento, modelos de la versión, implementar los modelos y mucho más.

Si va a desarrollar un modelo de TensorFlow desde el principio o lleva un modelo existente a la nube, puede crear modelos para entornos de producción con el servicio de Azure Machine Learning.

## <a name="prerequisites"></a>Requisitos previos

- Instalar el [de Azure Machine Learning SDK para Python](setup-create-workspace.md#sdk). Opcional: crear un `config.json` archivo de configuración.
- Descargue el [archivos de script de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow) `mnist-tf.py` y `utils.py`

También puede encontrar un completado [versión Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-tensorflow/train-hyperparameter-tune-deploy-with-tensorflow.ipynb) de esta guía en nuestra página de ejemplos de Github. El Bloc de notas incluye expandidas secciones que abarcan el ajuste de hiperparámetros inteligentes e implementación de modelo.

## <a name="set-up-the-experiment"></a>Configure el experimento

Esta sección configura el experimento de entrenamiento, cargar los paquetes de python necesarios, inicializando un área de trabajo, creación de un experimento y cargar los datos de entrenamiento y scripts de entrenamiento mediante el SDK de Python.

### <a name="import-packages"></a>Importación de paquetes

En primer lugar, se deberá importar las bibliotecas de Python necesarias.

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

### <a name="initialize-a-workspace"></a>Inicializar un área de trabajo

El [área de trabajo de servicio de Azure Machine Learning](concept-workspace.md) es el recurso de nivel superior para el servicio. Proporciona un lugar centralizado para trabajar con todos los artefactos que cree. En el SDK de Python, puede acceder a los artefactos de área de trabajo mediante la creación de un [ `workspace` ](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py) objeto.

Si ha completado el paso opcional en el [sección Requisitos previos](#prerequisites), puede usar `Workspace.from_config()` para crear rápidamente un objeto de área de trabajo de los detalles que se almacenan en el archivo de configuración.

```Python
ws = Workspace.from_config()
```

También puede crear un área de trabajo de forma explícita:

```Python
ws = Workspace.create(name='<workspace-name>',
                      subscription_id='<azure-subscription-id>',
                      resource_group='<choose-a-resource-group>',
                      create_resource_group=True,
                      location='<select-location>' # For example: 'eastus2'
                      )
```

### <a name="create-an-experiment"></a>Creación de un experimento

Crear un experimento y una carpeta para almacenar los scripts de entrenamiento. En este ejemplo, cree un experimento denominado "tf-mnist".

```Python
script_folder = './tf-mnist'
os.makedirs(script_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='tf-mnist')
```

### <a name="upload-dataset-and-scripts"></a>Cargar conjunto de datos y las secuencias de comandos

El [datastore](how-to-access-data.md) es un lugar donde se pueden almacenar datos y acceso de montaje o copiar los datos en el destino de proceso. Cada área de trabajo proporciona un almacén de datos de forma predeterminada. Se cargará nuestros datos y scripts de entrenamiento para que sean fácilmente accesibles durante el entrenamiento.

1. Descargue el conjunto de datos MNIST localmente.

    ```Python
    os.makedirs('./data/mnist', exist_ok=True)

    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-images-idx3-ubyte.gz', filename = './data/mnist/train-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/train-labels-idx1-ubyte.gz', filename = './data/mnist/train-labels.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-images-idx3-ubyte.gz', filename = './data/mnist/test-images.gz')
    urllib.request.urlretrieve('http://yann.lecun.com/exdb/mnist/t10k-labels-idx1-ubyte.gz', filename = './data/mnist/test-labels.gz')
    ```

1. Cargue el conjunto de datos MNIST en el almacén de datos de forma predeterminada.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/mnist', target_path='mnist', overwrite=True, show_progress=True)
    ```

1. Cargar el script de aprendizaje de TensorFlow `tf_mnist.py`y el archivo auxiliar, `utils.py`.

    ```Python
    shutil.copy('./tf_mnist.py', script_folder)
    shutil.copy('./utils.py', script_folder)
    ```

## <a name="create-a-compute-target"></a>Creación de un destino de proceso

Crear un destino de proceso para que se ejecute su trabajo de TensorFlow. En este ejemplo, crearemos un clúster de proceso basados en GPU de Azure Machine Learning. Para obtener una lista de aprendizaje disponibles, los destinos de proceso, consulte [en este artículo](how-to-set-up-training-targets.md#compute-targets-for-training)

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

## <a name="create-a-tensorflow-estimator"></a>Crear un Estimador de TensorFlow

El [TensorFlow Estimador](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) proporciona una manera sencilla de iniciar un trabajo de aprendizaje de TensorFlow en un destino de proceso. Creará una imagen de docker que tiene instalado de TensorFlow.

El Estimador de TensorFlow se implementa mediante el modelo genérico [ `estimator` ](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) (clase), que se puede usar para admitir cualquier marco de trabajo. Para obtener más información sobre cómo entrenar modelos con el Estimador de genérico, vea [entrenar modelos con Azure Machine Learning con Estimador](how-to-train-ml-models.md)

Si su script de entrenamiento debe adicional pip o conda paquetes para ejecutarlos, puede hacer que los paquetes instalados en la imagen de docker resultante pasando sus nombres mediante el `pip_packages` y `conda_packages` argumentos.

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

## <a name="submit-a-run"></a>Envía una ejecución

El [ejecutar objeto](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) proporciona la interfaz para el historial de ejecución mientras se ejecuta el trabajo y cuando se ha completado.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Que se ejecute la ejecución, pasará a través de las siguientes fases:

- **Preparando**: Se crea una imagen de docker según el Estimador de TensorFlow. La imagen se carga en container registry del área de trabajo y se almacena en caché para las ejecuciones posteriores. Los registros también se transmiten en el historial de ejecución y se pueden consultar para supervisar el progreso.

- **Escalado**: El clúster intentará escalar verticalmente si el clúster de Batch AI requiere más nodos para realizar la ejecución que se encuentran disponibles.

- **Running**: Todos los scripts en la carpeta de scripts se cargan en el destino de proceso, se monta los almacenes de datos o se copian y se ejecuta el entry_script. Genera una salida en stdout y. / carpeta de registros se transmiten en el historial de ejecución y puede usarse para supervisar la ejecución.

- **Posprocesamiento**: El. / salidas de carpeta de la ejecución se copia en el historial de ejecución.

## <a name="register-or-download-a-model"></a>Registrar o descargar un modelo

Una vez que haya entrenado el modelo, puede registrarlo en el área de trabajo. Registro del modelo le permite tienda y la versión de los modelos en el área de trabajo para simplificar [administración e implementación de modelo](concept-model-management-and-deployment.md).

```Python
model = run.register_model(model_name='tf-dnn-mnist', model_path='outputs/model')
```

También puede descargar una copia local del modelo utilizando el objeto de ejecución. En el script de entrenamiento `mnist-tf.py`, un objeto de protector de TensorFlow persista el modelo en una carpeta local (local para el destino de proceso). Podemos usar para descargar una copia del objeto de ejecución.

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

El [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) Estimador también admite el aprendizaje distribuido a través de clústeres de CPU y GPU. Puede ejecutar fácilmente los trabajos de TensorFlow distribuidos y va a administrar la orquestación para el servicio de Azure Machine Learning.

Servicio Azure Machine Learning admite dos métodos de aprendizaje distribuido en TensorFlow:

- [En función de MPI](https://www.open-mpi.org/) distribuidas entrenamiento mediante la [Horovod](https://github.com/uber/horovod) framework
- Nativo [distribuidas TensorFlow](https://www.tensorflow.org/deploy/distributed) mediante el parámetro del método de servidor

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod) es un marco de código abierto para el aprendizaje distribuido desarrolladas por Uber. Ofrece una ruta de acceso fácil a los trabajos de GPU TensorFlow distribuidos.

Para usar Horovod, especifique `mpi` para el `distributed_training` parámetro del constructor del estimador de TensorFlow. Horovod se instalará para que su uso en el script de entrenamiento.

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

Para usar el parámetro del método de servidor, especifique `ps` para el `distributed_training` parámetro del constructor del estimador de TensorFlow.

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
                      distributed_backend=distributed_training,
                      use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="define-cluster-specifications-in-tfconfig"></a>Definir las especificaciones del clúster en 'TF_CONFIG'

También necesitará las direcciones de red y puertos del clúster para el [ `tf.train.ClusterSpec` ](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), por lo que Azure Machine Learning establece el `TF_CONFIG` variable de entorno para usted.

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

Nivel del TensorFlow más alto [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API, TensorFlow se analizarán esto `TF_CONFIG` variable y el clúster de la compilación de especificaciones para usted.

Para el núcleo de nivel inferior de TensorFlow API para el entrenamiento, analizar el `TF_CONFIG` variable y compilación el `tf.train.ClusterSpec` en el código de entrenamiento.

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

En este artículo, entrenar y registrar un modelo de TensorFlow en el servicio Azure Machine Learning. Para obtener información sobre cómo implementar un modelo, continúe con nuestro artículo de la implementación de modelo.

> [!div class="nextstepaction"]
> [Cómo y dónde implementar los modelos](how-to-deploy-and-where.md)
