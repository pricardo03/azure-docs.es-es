---
title: Entrenar modelos de Machine Learning con Azure Machine Learning
description: Aprenda a realizar un entrenamiento de nodo único y distribuido de modelos de aprendizaje profundo y Machine Learning tradicionales con Azure Machine Learning Services
ms.author: minxia
author: mx-iao
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.date: 09/24/2018
ms.openlocfilehash: 50b808b5769f2160d765ecdb431d71856e651b33
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983589"
---
# <a name="how-to-train-models-with-azure-machine-learning"></a>Cómo entrenar modelos con Azure Machine Learning

El entrenamiento de modelos de Machine Learning, especialmente las redes neuronales profundas, suele ser una tarea de procesos intensivos y que requiere mucho tiempo. Una vez que haya terminado de escribir su script de aprendizaje y ejecutarse en un pequeño subconjunto de datos en su equipo local, es muy probable que desee escalar verticalmente su carga de trabajo.

Para facilitar el entrenamiento, el SDK de Python de Azure Machine Learning proporciona una abstracción de alto nivel, la clase de estimador, que permite a los usuarios entrenar fácilmente sus modelos en el ecosistema de Azure. Puede crear y usar un objeto de estimador para enviar cualquier código de aprendizaje que desee ejecutar en un equipo remoto, ya se trate de una ejecución de nodo único o de un entrenamiento distribuido en un clúster de GPU. En los trabajos PyTorch y TensorFlow, Azure Machine Learning también proporciona los estimadores PyTorch y TensorFlow correspondientes que facilitan el uso de estos marcos.

## <a name="train-with-an-estimator"></a>Entrenar con un estimador

Una vez que ha creado su [Área de trabajo](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace) y ha configurado su [entorno de desarrollo](how-to-configure-environment.md), el entrenamiento de un modelo en Azure Machine Learning implica los siguientes pasos:  
1. [Crear un destino de proceso remoto](how-to-set-up-training-targets.md)
2. [Cargar sus datos de aprendizaje](how-to-access-data.md) (opcional)
3. Crear su script de aprendizaje
4. Crear un objeto de estimador
5. Enviar su trabajo de aprendizaje

Este artículo se centra en los pasos 4 y 5. Para obtener información sobre los pasos del 1 al 3, consulte este [tutorial](tutorial-train-models-with-aml.md) para ver un ejemplo.

### <a name="single-node-training"></a>Entrenamiento de nodo único

El siguiente código le guía por una ejecución de entrenamiento de nodo único en proceso remoto en Azure para obtener un modelo scikit-learn. Debería haber creado su objeto de [destino de proceso](how-to-set-up-training-targets.md#batch) `compute_target` y su objeto de [almacén de datos](how-to-access-data.md) `ds`.

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

El fragmento de código anterior especifica los siguientes parámetros al constructor del estimador:
* `source_directory`: el directorio local que contiene todo el código necesario para el trabajo de aprendizaje. Esta carpeta se copia desde el equipo local al proceso remoto 
* `script_params`: un diccionario que especifica los argumentos de línea de comandos para el script de aprendizaje `entry_script`, en forma de pares <argumento de línea de comandos, valor>
* `compute_target`: el proceso remoto que ejecutará el script de aprendizaje, en este caso un clúster [Batch AI](how-to-set-up-training-targets.md#batch)
* `entry_script`: la ruta de acceso del archivo (relativa a la `source_directory`) del script de aprendizaje que se va a ejecutar en el proceso remoto. Este archivo y los archivos adicionales de los que depende deben encontrarse en esta carpeta
* `conda_packages`: la lista de paquetes de Python para instalarse a través de Conda que necesita el script de aprendizaje.  
El constructor tiene otro parámetro llamado `pip_packages` que puede usar para todos los paquetes de PIP necesitados

Ahora que ha creado su objeto de estimador, puede enviar el trabajo de aprendizaje que se va a ejecutar en el proceso remoto a través de una llamada a la función `submit` en su objeto de [experimento](concept-azure-machine-learning-architecture.md#experiment) `experiment`. 

```Python
run = experiment.submit(sk_est)
print(run.get_details().status)
```

> [!IMPORTANT]
> **Carpetas especiales**: dos carpetas, *outputs* y *logs*, reciben un tratamiento especial por parte de Azure Machine Learning. Durante el entrenamiento, si escribe archivos en carpetas llamadas *outputs* y *logs* relativas al directorio raíz (`./outputs` y `./logs`, respectivamente), estos archivos se cargarán automáticamente en su historial de ejecución, por lo que tendrá acceso a estos una vez completada su ejecución. 
>
> Para tener acceso a los artefactos creados durante el entrenamiento (por ejemplo, archivos de modelo, puntos de control, archivos de datos o imágenes trazadas), escríbalos en la carpeta `./outputs`.
>
> De forma similar, puede escribir cualquier registro desde su ejecución de entrenamiento en la carpeta `./logs`. Para usar la [integración TensorBoard](https://aka.ms/aml-notebook-tb) de Azure Machine Learning, asegúrese de escribir sus registros de TensorBoard en esta carpeta. Mientras su ejecución está en curso, podrá iniciar TensorBoard y transmitir estos registros.  Posteriormente, también podrá restaurar los registros desde cualquiera de sus ejecuciones anteriores.
>
> Por ejemplo, para descargar un archivo escrito en la carpeta *outputs* en su equipo local después de su ejecución de entrenamiento remoto: `run.download_file(name='outputs/my_output_file', output_file_path='my_destination_path')`

### <a name="distributed-training-and-custom-docker-images"></a>Entrenamiento distribuido e imágenes de Docker personalizadas

Hay dos escenarios de entrenamiento adicionales que puede llevar a cabo con el estimador:
1. Uso de una imagen de Docker personalizada
2. Entrenamiento distribuido en un clúster de varios nodos

El siguiente código muestra cómo llevar a cabo el entrenamiento distribuido para un modelo CNTK. Además, en lugar de usar las imágenes de Azure Machine Learning predeterminadas, supone que tiene su propia imagen de Docker personalizada, la cual desea utilizar para el entrenamiento.

Debería haber creado su objeto de [destino de proceso](how-to-set-up-training-targets.md#batch) `compute_target`. Puede crear el estimador de la siguiente manera:

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

El código anterior expone los siguientes nuevos parámetros para el constructor del estimador:
* `custom_docker_base_image`: el nombre de la imagen que desea usar. Solo puede proporcionar imágenes disponibles en repositorios de Docker (en este caso, Docker Hub). Para usar una imagen desde un repositorio de Docker privado, utilice el parámetro `environment_definition` del constructor en su lugar
* `node_count`: el número de nodos que se usará para el trabajo de aprendizaje. Este argumento tiene como valor predeterminado `1`
* `process_count_per_node`: el número de procesos (o “trabajos”) que se ejecutarán en cada nodo. En este caso, usa las GPU `2` disponibles en cada nodo. Este argumento tiene como valor predeterminado `1`
* `distributed_backend`: el back-end para iniciar el entrenamiento distribuido, que ofrece el estimador a través de MPI. El valor predeterminado de este argumento es `None`. Si desea llevar a cabo entrenamiento paralelo o distribuido (por ejemplo, `node_count` > 1, o `process_count_per_node` > 1 o ambos), establezca `distributed_backend='mpi'`. La implementación de MPI usada por AML es [MPI abierto](https://www.open-mpi.org/).

Por último, envíe el trabajo de aprendizaje:
```Python
run = experiment.submit(cntk_est)
```

## <a name="examples"></a>Ejemplos
Para ver un tutorial sobre cómo entrenar un modelo sklearn, consulte:
* `tutorials/01.train-models.ipynb`

Para ver un tutorial sobre el CNTK distribuido mediante un docker personalizado, consulte:
* `training/06.distributed-cntk-with-custom-docker/06.distributed-cntk-with-custom-docker.ipynb`

Obtenga estos blocs de notas:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md)
* [Entrenar modelos de PyTorch](how-to-train-pytorch.md)
* [Entrenar modelos de TensorFlow](how-to-train-tensorflow.md)
* [Ajustar hiperparámetros](how-to-tune-hyperparameters.md)
* [Implementar un modelo entrenado](how-to-deploy-and-where.md)
