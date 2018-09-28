---
title: Entrenar modelos de TensorFlow con Azure Machine Learning
description: Obtenga información sobre cómo ejecutar el entrenamiento de modelos de TensorFlow distribuidos y de nodo único con el Estimador de TensorFlow
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: ba43593e90b78aaa0083faf4f8162a7663c0ad47
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974228"
---
# <a name="how-to-train-tensorflow-models"></a>Cómo entrenar modelos de TensorFlow

Para el entrenamiento de red neuronal profunda (DNN) con TensorFlow, Azure Machine Learning proporciona una clase personalizada de TensorFlow del Estimador. El Estimador de TensorFlow del SDK de Azure (que no debe confundirse con la clase [`tf.estimator.Estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator/Estimator)) le permite enviar fácilmente los trabajos de entrenamiento de TensorFlow de ejecuciones de nodo único y distribuidas en procesos de Azure.

## <a name="single-node-training"></a>Entrenamiento de nodo único
El entrenamiento con el Estimador de TensorFlow es similar a usar el [Estimador base](how-to-train-ml-models.md), así que primero debe leer el artículo de instrucciones y asegúrese de comprender los conceptos presentados en él.
  
Para ejecutar un trabajo de TensorFlow, cree una instancia de un objeto `TensorFlow`. Debería haber creado su objeto de [destino de proceso](how-to-set-up-training-targets.md#batch) `compute_target`.

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
                    conda_packages=['scikit-learn'],
                    use_gpu=True)
```

En este caso, se especifican los siguientes parámetros al constructor de TensorFlow:
* `source_directory`: el directorio local que contiene todo el código necesario para el trabajo de entrenamiento. Esta carpeta se copia desde el equipo local al proceso remoto
* `script_params`: un diccionario que especifica los argumentos de línea de comandos para el script de entrenamiento `entry_script`, en forma de pares <argumento de línea de comandos, valor>
* `compute_target`: el proceso remoto que ejecutará el script de entrenamiento, en este caso un clúster [Batch AI](how-to-set-up-training-targets.md#batch)
* `entry_script`: la ruta del archivo (relativa al `source_directory`) del script de aprendizaje que se va a ejecutar en el proceso remoto. Este archivo y los archivos adicionales de los que depende deben encontrarse en esta carpeta
* `conda_packages`: la lista de paquetes de Python para instalarse a través de Conda que necesita el script de entrenamiento. En este caso se utiliza el script de entrenamiento `sklearn` para cargar los datos, por lo que debe indicar que se instalará este paquete.  
El constructor tiene otro parámetro llamado `pip_packages` que puede usar para todos los paquetes de PIP necesitados
* `use_gpu`: establezca esta marca en `True` para aprovechar la GPU para el entrenamiento. De manera predeterminada, su valor es `False`.

Puesto que utiliza el Estimador de TensorFlow, el contenedor utilizado para el entrenamiento tendrá incluirá el paquete de TensorFlow y las dependencias relacionadas necesarias para el entrenamiento en CPU y GPU de forma predeterminada.

Después, envíe el trabajo de TensorFlow:
```Python
run = exp.submit(tf_est)
```

## <a name="distributed-training"></a>Entrenamiento distribuido
El Estimador de TensorFlow también permite entrenar los modelos a escala a través de clústeres de GPU y CPU de las máquinas virtuales de Azure. Puede ejecutar fácilmente el entrenamiento de TensorFlow distribuido con unas pocas llamadas de API, mientras que Azure Machine Learning administrará en segundo plano toda la infraestructura y orquestación necesaria para llevar a cabo estas cargas de trabajo.

Azure Machine Learning admite dos métodos de aprendizaje distribuidos en TensorFlow:
1. entrenamiento distribuido basado en MPI mediante el marco [Horovod](https://github.com/uber/horovod)
2. [TensorFlow distribuido](https://www.tensorflow.org/deploy/distributed) nativo mediante el método de parámetro de servidor

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) es un marco de ring-allreduce de código abierto para entrenamiento distribuido desarrollado por Uber.

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
* `node_count`: el número de nodos que se usará para el trabajo de aprendizaje. Este argumento tiene como valor predeterminado `1`
* `process_count_per_node`: el número de procesos (o “trabajos”) que se ejecutarán en cada nodo. Este argumento tiene como valor predeterminado `1`
* `distributed_backend`: el back-end para iniciar el entrenamiento distribuido, que ofrece el Estimador a través de MPI. El valor predeterminado de este argumento es `None`. Si quiere llevar a cabo un aprendizaje paralelo o distribuido (por ejemplo, `node_count` > 1, o `process_count_per_node` > 1, o bien ambos) con MPI (y Horovod), establezca `distributed_backend='mpi'`. La implementación de MPI utilizada por Azure Machine Learning es [Open MPI](https://www.open-mpi.org/).

El ejemplo anterior ejecutará un entrenamiento distribuido con dos trabajos, un trabajo por nodo.

Horovod y sus dependencias se instalarán automáticamente, para que simplemente pueda importarlo en el script de entrenamiento `train.py` como sigue:
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
* `worker_count`: el número de trabajadores. Este argumento tiene como valor predeterminado `1`
* `parameter_server_count`: el número de servidores de parámetro. Este argumento tiene como valor predeterminado `1`
* `distributed_backend`: el back-end que se usará para distribuir el entrenamiento. El valor predeterminado de este argumento es `None`. Con el fin de realizar el entrenamiento distribuido mediante el servidor de parámetro, deberá establecer `distributed_backend='ps'`

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

Si utiliza la API [`tf.estimator`](https://www.tensorflow.org/api_docs/python/tf/estimator) de alto nivel de TensorFlow, TensorFlow analizará esta variable `TF_CONFIG` y compilará la especificación del clúster por usted. 

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
Para ver un tutorial sobre el aprendizaje de TensorFlow de nodo único, vea:
* `training/03.train-tune-deploy-tensorflow/03.train-tune-deploy-tensorflow.ipynb`

Para ver un tutorial sobre TensorFlow distribuido con Horovod, vea:
* `training/04.distributed-tensorflow-with-horovod/04.distributed-tensorflow-with-horovod.ipynb`

Para ver un tutorial sobre TensorFlow distribuido nativo, vea:
* `training/05.distributed-tensorflow-with-parameter-server/05.distributed-tensorflow-with-parameter-server.ipynb`

Obtenga estos cuadernos:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes
* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md)
* [Ajustar los hiperparámetros](how-to-tune-hyperparameters.md)
* [Implementar un modelo entrenado](how-to-deploy-and-where.md)
