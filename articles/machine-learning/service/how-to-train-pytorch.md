---
title: Entrenamiento de modelos con PyTorch
titleSuffix: Azure Machine Learning service
description: Aprenda a ejecutar el entrenamiento distribuido de modelos de PyTorch y de nodo único con el estimador de PyTorch
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: minxia
author: mx-iao
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 9ae7795381f036bb819ce24554d8cea94ceb5552
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60818337"
---
# <a name="train-pytorch-models-with-azure-machine-learning-service"></a>Entrenamiento de modelos de PyTorch con Azure Machine Learning Service

Para el entrenamiento de red neuronal profunda (DNN) con PyTorch, Azure Machine Learning proporciona un personalizado [PyTorch](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.pytorch?view=azure-ml-py) clase de la `Estimator`. El estimador de `PyTorch` del SDK de Azure le permite enviar fácilmente los trabajos de entrenamiento de PyTorch para las ejecuciones de nodo único y distribuidas en Azure Compute.

## <a name="single-node-training"></a>Entrenamiento de nodo único
El entrenamiento con el estimador de `PyTorch` es similar a usar el [base`Estimator`](how-to-train-ml-models.md), así que primero debe leer el artículo de instrucciones y comprender los conceptos que presenta.
  
Para ejecutar un trabajo de PyTorch, cree una instancia de un objeto `PyTorch`. Debería haber creado su objeto de [destino de proceso](how-to-set-up-training-targets.md#amlcompute) `compute_target` y su objeto de [almacén de datos](how-to-access-data.md) `ds`.

```Python
from azureml.train.dnn import PyTorch

script_params = {
    '--data_dir': ds
}

pt_est = PyTorch(source_directory='./my-pytorch-proj',
                 script_params=script_params,
                 compute_target=compute_target,
                 entry_script='train.py',
                 use_gpu=True)
```

En este caso, se especifican los siguientes parámetros al constructor de PyTorch:

Parámetro | DESCRIPCIÓN
--|--
`source_directory` |  Directorio local que contiene todo el código necesario para el trabajo de aprendizaje. Esta carpeta se copia desde el equipo local al proceso remoto
`script_params` |  Especificar los argumentos de línea de comandos para el script de entrenamiento de diccionario `entry_script`, en forma de < argumento de línea de comandos, valor > pares.  Para especificar una marca en detallado `script_params`, utilice `<command-line argument, "">`.
`compute_target` |  El destino de proceso remoto que el script de entrenamiento ejecutará, en este caso, un clúster de Proceso de Azure Machine Learning ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)).
`entry_script` |  Ruta del archivo (relativa al `source_directory`) del script de aprendizaje que se va a ejecutar en el proceso remoto. Este archivo y los archivos adicionales de los que depende deben encontrarse en esta carpeta
`conda_packages` |  Lista de paquetes de Python para instalarse mediante Conda que necesita el script de aprendizaje. El constructor tiene otro parámetro llamado `pip_packages` que puede usar para todos los paquetes de PIP necesitados
`use_gpu` |  Establezca esta marca en `True` para aprovechar la GPU para el entrenamiento. De manera predeterminada, su valor es `False`.

Puesto que utiliza el estimador de `PyTorch`, el contenedor utilizado para el entrenamiento incluirá el paquete de PyTorch y las dependencias relacionadas necesarias para el entrenamiento en las CPU y los GPU.

A continuación, envíe el trabajo de PyTorch:
```Python
run = exp.submit(pt_est)
```

## <a name="distributed-training"></a>Entrenamiento distribuido
El estimador de `PyTorch` también permite entrenar los modelos a escala mediante clústeres de GPU y CPU de las máquinas virtuales de Azure. Puede ejecutar fácilmente el entrenamiento distribuido de PyTorch con unas pocas llamadas de API, mientras que Azure Machine Learning administrará en segundo plano toda la infraestructura y orquestación necesaria para llevar a cabo estas cargas de trabajo.

Actualmente, Azure Machine Learning admite el entrenamiento distribuido de PyTorch con MPI mediante la plataforma Horovod.

### <a name="horovod"></a>Horovod
[Horovod](https://github.com/uber/horovod) es una plataforma de ring-allreduce de código abierto para entrenamiento distribuido desarrollado por Uber.

Para ejecutar PyTorch distribuido mediante la plataforma Horovod, cree el objeto de PyTorch como sigue:

```Python
from azureml.train.dnn import PyTorch

pt_est = PyTorch(source_directory='./my-pytorch-project',
                 script_params={},
                 compute_target=compute_target,
                 entry_script='train.py',
                 node_count=2,
                 process_count_per_node=1,
                 distributed_backend='mpi',
                 use_gpu=True)
```

Este código expone los siguientes parámetros nuevos para el constructor de PyTorch:

Parámetro | DESCRIPCIÓN | Valor predeterminado
--|--|--
`node_count` |  Número de nodos que se usará para el trabajo de aprendizaje. | `1`
`process_count_per_node` |  Número de procesos (o "trabajos") que se ejecutarán en cada nodo. | `1`
`distributed_backend` |  Back-end para iniciar el entrenamiento distribuido, que ofrece el estimador mediante MPI.  Para llevar a cabo un aprendizaje paralelo o distribuido (por ejemplo, `node_count` > 1, o `process_count_per_node` > 1, o bien ambos) con MPI (y Horovod), establezca `distributed_backend='mpi'`. La implementación de MPI utilizada por Azure Machine Learning es [Open MPI](https://www.open-mpi.org/). | `None`

El ejemplo anterior ejecutará un entrenamiento distribuido con dos trabajos, un trabajo por nodo.

Horovod y sus dependencias se instalarán automáticamente, para que simplemente pueda importarlo en el script de entrenamiento `train.py` como sigue:
```Python
import torch
import horovod
```

Por último, envíe el trabajo de PyTorch distribuido:
```Python
run = exp.submit(pt_est)
```

## <a name="examples"></a>Ejemplos

Para los cuadernos de aprendizaje profundo distribuido, consulte:
* [how-to-use-azureml/training-with-deep-learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes
* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md)
* [Ajustar los hiperparámetros](how-to-tune-hyperparameters.md)
* [Implementar un modelo entrenado](how-to-deploy-and-where.md)
