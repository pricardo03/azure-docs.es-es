---
title: Entrenamiento de modelos de aprendizaje automático con estimadores
titleSuffix: Azure Machine Learning service
description: Aprenda a realizar un entrenamiento de nodo único y distribuido de modelos de aprendizaje automático tradicional y modelos de aprendizaje profundo con la clase Estimator de Azure Machine Learning Service
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 005854a51916d36bbad56f1296f17fa687020359
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251408"
---
# <a name="train-models-with-azure-machine-learning"></a>Entrenamiento de modelos con Azure Machine Learning

El entrenamiento de modelos de Machine Learning, especialmente las redes neuronales profundas, suele ser una tarea de procesos intensivos y que requiere mucho tiempo. Una vez que haya terminado de escribir su script de aprendizaje y ejecutarse en un pequeño subconjunto de datos en su equipo local, es muy probable que desee escalar verticalmente su carga de trabajo.

Para facilitar el entrenamiento, el SDK de Azure Machine Learning para Python proporciona una abstracción de alto nivel, la clase Estimator, que permite a los usuarios entrenar fácilmente sus modelos en el ecosistema de Azure. Puede crear y usar un [`Estimator`objeto](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.estimator?view=azure-ml-py) para enviar cualquier código de aprendizaje que desee ejecutar en un equipo remoto, ya se trate de una ejecución de nodo único o de un entrenamiento distribuido en un clúster de GPU. En los trabajos PyTorch y TensorFlow, Azure Machine Learning también proporciona los estimadores `PyTorch` y `TensorFlow` personalizados que facilitan el uso de estos marcos.

## <a name="train-with-an-estimator"></a>Entrenamiento con un estimador

Una vez que ha creado el [área de trabajo](concept-azure-machine-learning-architecture.md#workspace) y ha configurado el [entorno de desarrollo](how-to-configure-environment.md), el entrenamiento de un modelo en Azure Machine Learning implica los siguientes pasos:  
1. Creación de un [destino de proceso remoto](how-to-set-up-training-targets.md)
2. Carga de los [datos de aprendizaje](how-to-access-data.md) (opcional)
3. Creación del [script de aprendizaje](tutorial-train-models-with-aml.md#create-a-training-script)
4. Creación de un objeto `Estimator`
5. Enviar su trabajo de aprendizaje

Este artículo se centra en los pasos 4 y 5. Para obtener información sobre los pasos del 1 al 3, consulte el [tutorial de entrenamiento de un modelo](tutorial-train-models-with-aml.md) para ver un ejemplo.

### <a name="single-node-training"></a>Entrenamiento de nodo único

Utilice un objeto `Estimator` para una ejecución de entrenamiento de nodo único en proceso remoto en Azure para un modelo scikit-learn. Debería haber creado su objeto de [destino de proceso](how-to-set-up-training-targets.md#amlcompute) `compute_target` y su objeto de [almacén de datos](how-to-access-data.md) `ds`.

```Python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])
```

Este fragmento de código especifica los parámetros siguientes para el constructor `Estimator`.

Parámetro | DESCRIPCIÓN
--|--
`source_directory`| Directorio local que contiene todo el código necesario para el trabajo de aprendizaje. Esta carpeta se copia desde el equipo local al proceso remoto 
`script_params`| Diccionario que especifica los argumentos de línea de comandos para el script de aprendizaje `entry_script`, en forma de pares <argumento de línea de comandos, valor>
`compute_target`| El destino de proceso remoto que el script de entrenamiento ejecutará, en este caso, un clúster de Proceso de Azure Machine Learning ([AmlCompute](how-to-set-up-training-targets.md#amlcompute)).
`entry_script`| Ruta del archivo (relativa al `source_directory`) del script de aprendizaje que se va a ejecutar en el proceso remoto. Este archivo y los archivos adicionales de los que depende deben encontrarse en esta carpeta
`conda_packages`| Lista de paquetes de Python para instalarse mediante Conda que necesita el script de aprendizaje.  
El constructor tiene otro parámetro llamado `pip_packages` que puede usar para los paquetes de pip necesarios

Ahora que ha creado el objeto `Estimator`, envíe el trabajo de aprendizaje que se va a ejecutar en el proceso remoto mediante una llamada a la función `submit` en el objeto `experiment` del [experimento](concept-azure-machine-learning-architecture.md#experiment). 

```Python
run = experiment.submit(sk_est)
print(run.get_details().status)
```

> [!IMPORTANT]
> **Carpetas especiales**: dos carpetas, *outputs* y *logs*, reciben un tratamiento especial por parte de Azure Machine Learning. Durante el entrenamiento, si escribe archivos en las carpetas llamadas *outputs* y *logs* relativas al directorio raíz (`./outputs` y `./logs`, respectivamente), estos archivos se cargarán automáticamente en su historial de ejecución, por lo que tendrá acceso a estos una vez completada su ejecución.
>
> Para crear artefactos durante el entrenamiento (por ejemplo, archivos de modelo, puntos de control, archivos de datos o imágenes trazadas), escríbalos en la carpeta `./outputs`.
>
> De forma similar, puede escribir cualquier registro de la ejecución del entrenamiento en la carpeta `./logs`. Para usar la [integración TensorBoard](https://aka.ms/aml-notebook-tb) de Azure Machine Learning, asegúrese de escribir sus registros de TensorBoard en esta carpeta. Mientras su ejecución está en curso, podrá iniciar TensorBoard y transmitir estos registros.  Posteriormente, también podrá restaurar los registros desde cualquiera de sus ejecuciones anteriores.
>
> Por ejemplo, para descargar un archivo escrito en la carpeta *outputs* en su equipo local después de su ejecución de entrenamiento remoto: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Entrenamiento distribuido e imágenes de Docker personalizadas

Hay dos escenarios de entrenamiento adicionales que puede llevar a cabo con el objeto `Estimator`:
* Uso de una imagen de Docker personalizada
* Entrenamiento distribuido en un clúster de varios nodos

El siguiente código muestra cómo llevar a cabo el entrenamiento distribuido para un modelo CNTK. Además, en lugar de usar las imágenes de Azure Machine Learning predeterminadas, supone que tiene su propia imagen de Docker personalizada para el entrenamiento.

Debería haber creado su objeto de [destino de proceso](how-to-set-up-training-targets.md#amlcompute) `compute_target`. Puede crear el estimador de la siguiente manera:

```Python
from azureml.train.estimator import Estimator

estimator = Estimator(source_directory='./my-cntk-proj',
                      compute_target=compute_target,
                      entry_script='train.py',
                      node_count=2,
                      process_count_per_node=1,
                      distributed_backend='mpi',     
                      pip_packages=['cntk==2.5.1'],
                      custom_docker_base_image='microsoft/mmlspark:0.12')
```

El código anterior expone los siguientes parámetros nuevos para el constructor `Estimator`:

Parámetro | DESCRIPCIÓN | Valor predeterminado
--|--|--
`custom_docker_base_image`| Nombre de la imagen que desea usar. Solo puede proporcionar imágenes disponibles en repositorios de Docker públicos (en este caso, Docker Hub). Para usar una imagen desde un repositorio de Docker privado, utilice el parámetro `environment_definition` del constructor en su lugar | `None`
`node_count`| Número de nodos que se usará para el trabajo de aprendizaje. | `1`
`process_count_per_node`| Número de procesos (o "trabajos") que se ejecutarán en cada nodo. En este caso, usa las GPU `2` disponibles en cada nodo.| `1`
`distributed_backend`| Back-end para iniciar el entrenamiento distribuido, que ofrece el estimador mediante MPI.  Para llevar a cabo entrenamiento paralelo o distribuido (por ejemplo, `node_count` > 1 o `process_count_per_node` > 1 o ambos), establezca `distributed_backend='mpi'`. La implementación de MPI usada por AML es [MPI abierto](https://www.open-mpi.org/).| `None`

Por último, envíe el trabajo de aprendizaje:
```Python
run = experiment.submit(cntk_est)
```

## <a name="examples"></a>Ejemplos
Para un cuaderno que entrena un modelo sklearn, consulte:
* [tutorials/img-classification-part1-training.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/img-classification-part1-training.ipynb)

Para los cuadernos de aprendizaje profundo distribuido, consulte:
* [how-to-use-azureml/training-with-deep-learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning)

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md)
* [Entrenar modelos de PyTorch](how-to-train-pytorch.md)
* [Entrenar modelos de TensorFlow](how-to-train-tensorflow.md)
* [Ajustar hiperparámetros](how-to-tune-hyperparameters.md)
* [Implementar un modelo entrenado](how-to-deploy-and-where.md)
