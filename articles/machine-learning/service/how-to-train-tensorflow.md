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
ms.date: 02/21/2019
ms.custom: seodec18
ms.openlocfilehash: b41098907f801f7dae839a470249834b02c8d519
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57338559"
---
# <a name="train-tensorflow-and-keras-models-with-azure-machine-learning-service"></a>Entrenar modelos de TensorFlow y Keras con el servicio Azure Machine Learning

Para el entrenamiento de red neuronal profunda (DNN) con TensorFlow, Azure Machine Learning proporciona una clase personalizada de `TensorFlow` de `Estimator`. El SDK de Azure [TensorFlow](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.tensorflow?view=azure-ml-py) Estimador (no a ser vinculada con la [ `tf.estimator.Estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator) clase) le permite enviar fácilmente los trabajos de aprendizaje de TensorFlow de nodo único y distribuida se ejecuta en Azure proceso.

## <a name="single-node-training"></a>Entrenamiento de nodo único
El entrenamiento con el estimador de `TensorFlow` es similar a usar el [base`Estimator`](how-to-train-ml-models.md), así que primero debe leer el artículo de instrucciones y comprender los conceptos que presenta.
  
Para ejecutar un trabajo de TensorFlow, cree una instancia de un objeto `TensorFlow`. Debería haber creado su objeto de [destino de proceso](how-to-set-up-training-targets.md#amlcompute) `compute_target`.

```Python
from azureml.train.dnn import TensorFlow

script_params = {
    '--batch-size': 50,
    '--learning-rate': 0.01,
}

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train.py',
                    conda_packages=['scikit-learn'], # in case you need scikit-learn in train.py
                    use_gpu=True)
```

En este caso, se especifican los siguientes parámetros al constructor de TensorFlow:

Parámetro | DESCRIPCIÓN
--|--
`source_directory` | Directorio local que contiene todo el código necesario para el trabajo de aprendizaje. Esta carpeta se copia desde el equipo local al proceso remoto
`script_params` | Diccionario que especifica los argumentos de línea de comandos para el script de aprendizaje `entry_script`, en forma de pares <argumento de línea de comandos, valor>
`compute_target` | El destino de proceso remoto que el script de entrenamiento ejecutará, en este caso, un clúster de Proceso de Azure Machine Learning ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)).
`entry_script` | Ruta del archivo (relativa al `source_directory`) del script de aprendizaje que se va a ejecutar en el proceso remoto. Este archivo y los archivos adicionales de los que depende deben encontrarse en esta carpeta
`conda_packages` | Lista de paquetes de Python para instalarse mediante Conda que necesita el script de aprendizaje. En este caso se utiliza el script de entrenamiento `sklearn` para cargar los datos, por lo que debe indicar que se instalará este paquete.  El constructor tiene otro parámetro llamado `pip_packages` que puede usar para todos los paquetes de PIP necesitados
`use_gpu` | Establezca esta marca en `True` para aprovechar la GPU para el entrenamiento. De manera predeterminada, su valor es `False`.

Puesto que utiliza el Estimador de TensorFlow, el contenedor utilizado para el entrenamiento tendrá incluirá el paquete de TensorFlow y las dependencias relacionadas necesarias para el entrenamiento en CPU y GPU de forma predeterminada.

Después, envíe el trabajo de TensorFlow:
```Python
run = exp.submit(tf_est)
```

## <a name="keras-support"></a>Compatibilidad con Keras
[Keras](https://keras.io/) es una API de Python de DNN alto nivel populares que admite TensorFlow, CNTK o Theano como back-ends. Si utilizas TensorFlow como back-end, puede usar fácilmente el Estimador TensFlow para entrenar un modelo de Keras. Este es un ejemplo de un Estimador de TensorFlow con Keras le agregadas:

```Python
from azureml.train.dnn import TensorFlow

keras_est = TensorFlow(source_directory='./my-keras-proj',
                       script_params=script_params,
                       compute_target=compute_target,
                       entry_script='keras_train.py',
                       pip_packages=['keras'], # just add keras through pip
                       use_gpu=True)
```
El constructor de Estimador TensorFlow anterior indica al servicio de Azure Machine Learning para instalar Keras a través de pip para el entorno de ejecución. Y su `keras_train.py` , a continuación, se puede importar la API de Keras para entrenar un modelo de Keras. Para obtener un ejemplo completo, explore [este cuaderno de Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-keras/train-hyperparameter-tune-deploy-with-keras.ipynb).

## <a name="distributed-training"></a>Entrenamiento distribuido
El Estimador de TensorFlow también permite entrenar los modelos a escala a través de clústeres de GPU y CPU de las máquinas virtuales de Azure. Puede ejecutar fácilmente el entrenamiento de TensorFlow distribuido con unas pocas llamadas de API, mientras que Azure Machine Learning administrará en segundo plano toda la infraestructura y orquestación necesaria para llevar a cabo estas cargas de trabajo.

Azure Machine Learning admite dos métodos de aprendizaje distribuidos en TensorFlow:
* entrenamiento distribuido basado en MPI mediante el marco [Horovod](https://github.com/uber/horovod)
* [TensorFlow distribuido](https://www.tensorflow.org/deploy/distributed) nativo mediante el método de parámetro de servidor

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) es una plataforma de ring-allreduce de código abierto para entrenamiento distribuido desarrollado por Uber.

Para ejecutar TensorFlow distribuido mediante el marco Horovod, cree el objeto de TensorFlow como sigue:

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    process_count_per_node=1,
                    distributed_backend='mpi',
                    use_gpu=True)
```

El código anterior expone los siguientes nuevos parámetros para el constructor de TensorFlow:

Parámetro | DESCRIPCIÓN | Valor predeterminado
--|--|--
`node_count` | Número de nodos que se usará para el trabajo de aprendizaje. | `1`
`process_count_per_node` | Número de procesos (o "trabajos") que se ejecutarán en cada nodo.|`1`
`distributed_backend` | Back-end para iniciar el entrenamiento distribuido, que ofrece el estimador mediante MPI. Si desea llevar a cabo aprendizaje paralelo o distribuido (por ejemplo, `node_count`> 1 o `process_count_per_node`> 1 o ambos) con MPI (y Horovod), establezca `distributed_backend='mpi'`. La implementación de MPI utilizada por Azure Machine Learning es [Open MPI](https://www.open-mpi.org/). | `None`

El ejemplo anterior ejecutará un entrenamiento distribuido con dos trabajos, un trabajo por nodo.

Horovod y sus dependencias se instalarán automáticamente, para que pueda importar en el script de entrenamiento `train.py` como sigue:

```Python
import tensorflow as tf
import horovod
```

Por último, envíe el trabajo de TensorFlow:
```Python
run = exp.submit(tf_est)
```

### <a name="parameter-server"></a>Servidor de parámetro
También puede ejecutar [TensorFlow distribuido nativo](https://www.tensorflow.org/deploy/distributed), que utiliza el modelo de servidor del parámetro. En este método, entrena a través de un clúster de servidores de parámetro y los trabajadores. Los trabajadores calculan los degradados durante el entrenamiento, mientras que los servidores de parámetro agregan los degradados.

Construya el objeto de TensorFlow:

```Python
from azureml.train.dnn import TensorFlow

tf_est = TensorFlow(source_directory='./my-tf-proj',
                    script_params={},
                    compute_target=compute_target,
                    entry_script='train.py',
                    node_count=2,
                    worker_count=2,
                    parameter_server_count=1,
                    distributed_backend='ps',
                    use_gpu=True)
```

Preste atención a los siguientes parámetros del constructor de TensorFlow en el código anterior:

Parámetro | DESCRIPCIÓN | Valor predeterminado
--|--|--
`worker_count` | Número de trabajadores. | `1`
`parameter_server_count` | Número de servidores de parámetro. | `1`
`distributed_backend` | Back-end que se usará para distribuir el entrenamiento. Para realizar el entrenamiento distribuido mediante el servidor de parámetro, establezca `distributed_backend='ps'` | `None`

#### <a name="note-on-tfconfig"></a>Nota sobre `TF_CONFIG`
También necesitará las direcciones de red y puertos del clúster para el [`tf.train.ClusterSpec`](https://www.tensorflow.org/api_docs/python/tf/train/ClusterSpec), por lo que Azure Machine Learning establece la variable de entorno `TF_CONFIG` por usted.

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

Si usa un nivel alto de TensorFlow [ `tf.estimator` ](https://www.tensorflow.org/api_docs/python/tf/estimator) API, TensorFlow se analizarán esto `TF_CONFIG` variable y el clúster de la compilación de especificaciones para usted. 

Si, en su lugar, usa API de núcleo de nivel inferior de TensorFlow para el entrenamiento, tiene que analizar la variable `TF_CONFIG` y compilar el `tf.train.ClusterSpec` usted en el código de entrenamiento. En [este ejemplo](https://aka.ms/aml-notebook-tf-ps), lo haría en **su script de entrenamiento** como sigue:

```Python
import os, json
import tensorflow as tf

tf_config = os.environ.get('TF_CONFIG')
if not tf_config or tf_config == "":
    raise ValueError("TF_CONFIG not found.")
tf_config_json = json.loads(tf_config)
cluster_spec = tf.train.ClusterSpec(cluster)

```

Cuando termine de escribir el script de entrenamiento y de crear el objeto de TensorFlow, puede enviar el trabajo de aprendizaje:
```Python
run = exp.submit(tf_est)
```

## <a name="examples"></a>Ejemplos

Explore distintos [blocs de notas en el aprendizaje profundo distribuido en Github](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes
* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md)
* [Ajustar los hiperparámetros](how-to-tune-hyperparameters.md)
* [Implementar un modelo entrenado](how-to-deploy-and-where.md)
