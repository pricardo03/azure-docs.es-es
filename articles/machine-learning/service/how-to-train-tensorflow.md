---
title: Entrenar modelos con TensorFlow & Keras
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo ejecutar nodo único y distribuida entrenamiento de modelos de TensorFlow y Keras con los estimadores TensorFlow y Keras
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 05/06/2019
ms.custom: seodec18
ms.openlocfilehash: 82c9aa961221b582bb16438f30a0584232164393
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65915103"
---
# <a name="train-tensorflow-and-keras-models-with-azure-machine-learning-service"></a>Entrenar modelos de TensorFlow y Keras con el servicio Azure Machine Learning

Puede ejecutar trabajos de aprendizaje de TensorFlow en proceso de Azure fácilmente mediante el uso de la [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) clase Estimador del SDK de Azure Machine Learning. El `TensorFlow` Estimador dirige el servicio de Azure Machine Learning para ejecutar el trabajo en un contenedor habilitado como TensorFlow para el entrenamiento de la red neuronal profunda (DNN).

El `TensorFlow` Estimador también proporciona una capa de abstracción sobre la ejecución, lo que significa que puede configurar fácilmente con parámetros se ejecuta en diferentes destinos de proceso sin modificar los scripts de entrenamiento.

## <a name="get-started"></a>Introducción

Puesto que la `TensorFlow` Estimador clase es similar a la base de [ `Estimator` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py), se recomienda leer primero el [base artículo de procedimientos de Estimador](how-to-train-ml-models.md) entender los conceptos exhaustiva.

Para empezar a trabajar con el servicio de Azure Machine Learning, [completar el tutorial rápido](quickstart-run-cloud-notebook.md). Una vez finalizado, tendrá una [área de trabajo de Azure Machine Learning](concept-workspace.md) y todos nuestros [cuadernos de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml) los de entrenamiento Dnn con TensorFlow y Keras incluidos.

## <a name="single-node-training"></a>Entrenamiento de nodo único

Para ejecutar un trabajo de TensorFlow, crear una instancia de un [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) objeto y enviarlo como un experimento.

El código siguiente crea una instancia de un Estimador TensorFlow y lo envía como un experimento. El script de entrenamiento `train.py` se ejecutará con los parámetros de la secuencia de comandos determinada. El trabajo se ejecutará en un habilitadas para GPU [destino de proceso](how-to-set-up-training-targets.md)y scikit-aprender will instalarse como una dependencia para `train.py`.

```Python
from azureml.train.dnn import TensorFlow

# training script parameters passed as command-line arguments
script_params = {
    '--batch-size': 50,
    '--learning-rate': 0.01,
}

# TensorFlow constructor
tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train.py', # relative path to your TensorFlow job
                    conda_packages=['scikit-learn'],
                    use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

## <a name="distributed-training"></a>Entrenamiento distribuido

El [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) Estimador también admite el aprendizaje distribuido a través de clústeres de CPU y GPU. Puede ejecutar fácilmente los trabajos de TensorFlow distribuidos y servicio de Azure Machine Learning va a administrar la infraestructura y la orquestación para usted.

Servicio Azure Machine Learning admite dos métodos de aprendizaje distribuido en TensorFlow:

* [En función de MPI](https://www.open-mpi.org/) distribuidas entrenamiento mediante la [Horovod](https://github.com/uber/horovod) framework
* Nativo [distribuidas TensorFlow](https://www.tensorflow.org/deploy/distributed) mediante el parámetro del método de servidor

### <a name="horovod"></a>Horovod

[Horovod](https://github.com/uber/horovod) es un marco de código abierto para el aprendizaje distribuido desarrolladas por Uber. Ofrece una ruta de acceso fácil a los trabajos de GPU TensorFlow distribuidos.

En el ejemplo siguiente se ejecuta un trabajo de entrenamiento distribuido mediante Horovod con dos trabajos que se distribuyen entre dos nodos.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py', # relative path to your TensorFlow job
                    node_count=2,
                    process_count_per_node=1,
                    distributed_backend='mpi', # specifies Horovod backend
                    use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

Horovod y sus dependencias se instalarán automáticamente, para que pueda importar en el script de entrenamiento.

```Python
import tensorflow as tf
import horovod
```

### <a name="parameter-server"></a>Servidor de parámetros

También puede ejecutar [TensorFlow distribuido nativo](https://www.tensorflow.org/deploy/distributed), que utiliza el modelo de servidor del parámetro. En este método, entrena a través de un clúster de servidores de parámetro y los trabajadores. Los trabajadores calculan los degradados durante el entrenamiento, mientras que los servidores de parámetro agregan los degradados.

El ejemplo siguiente ejecuta un trabajo de aprendizaje distribuida mediante el parámetro del método de servidor con cuatro trabajos distribuidos entre dos nodos.

```Python
from azureml.train.dnn import TensorFlow

# Tensorflow constructor
tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py', # relative path to your TensorFlow job
                    node_count=2,
                    worker_count=2,
                    parameter_server_count=1,
                    distributed_backend='ps', # specifies parameter server backend
                    use_gpu=True)

# submit the TensorFlow job
run = exp.submit(tf_est)
```

#### <a name="note-on-tfconfig"></a>Nota sobre `TF_CONFIG`

También necesitará las direcciones de red y puertos del clúster para el [ `tf.train.ClusterSpec` ](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), por lo que Azure Machine Learning establece el `TF_CONFIG` variable de entorno para usted.

La variable de entorno `TF_CONFIG` es una cadena JSON. Este es un ejemplo de la variable para un servidor de parámetro:

```
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

Para el núcleo de nivel inferior de TensorFlow API para el entrenamiento, analizar el `TF_CONFIG` variable y compilación el `tf.train.ClusterSpec` en el código de entrenamiento. En [este ejemplo](https://aka.ms/aml-notebook-tf-ps), lo haría en **su script de entrenamiento** como sigue:

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

## <a name="keras-support"></a>Compatibilidad con Keras

[Keras](https://keras.io/) es una API de Python de DNN de populares y de alto nivel que admite TensorFlow, CNTK y Theano como back-ends. Si utilizas TensorFlow como back-end, agregar Keras es tan sencillo como incluyendo un `pip_package` parámetro del constructor.

El ejemplo siguiente crea una instancia de un [ `TensorFlow` ](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) Estimador y lo envía como un experimento. El Estimador ejecuta el script de entrenamiento de Keras `keras_train.py`. El trabajo se ejecutará en un habilitadas para gpu [destino de proceso](how-to-set-up-training-targets.md) con Keras instalado como una dependencia a través de pip.

```Python
from azureml.train.dnn import TensorFlow

keras_est = TensorFlow(source_directory='./my-keras-proj',
                       script_params=script_params,
                       compute_target=compute_target,
                       entry_script='keras_train.py', # relative path to your TensorFlow job
                       pip_packages=['keras'], # add keras through pip
                       use_gpu=True)
```

## <a name="export-to-onnx"></a>Exportar a ONNX

Para obtener una inferencia optimizada con el [en tiempo de ejecución ONNX](concept-onnx.md), el modelo de TensorFlow entrenado se puede convertir al formato ONNX. Observe el [ejemplo](https://github.com/onnx/tensorflow-onnx/blob/master/examples/call_coverter_via_python.py).

## <a name="examples"></a>Ejemplos

Puede encontrar ejemplos de código de trabajo para las ejecuciones de TensorFlow distribuidas y de nodo único con varios marcos en [nuestra página de GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training-with-deep-learning).

## <a name="next-steps"></a>Pasos siguientes

* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md)
* [Ajustar los hiperparámetros](how-to-tune-hyperparameters.md)
* [Implementar un modelo entrenado](how-to-deploy-and-where.md)
