---
title: Entrenamiento de modelos de Machine Learning con scikit-learn
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo ejecutar los scripts de entrenamiento de scikit-learn a escala empresarial mediante la clase estimator de SKlearn de Azure Machine Learning. Los scripts de ejemplo clasifican imágenes de flores Iris para crear un modelo de Machine Learning basado en el conjunto de datos de Iris de scikit-learn.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: maxluk
author: maxluk
ms.date: 08/02/2019
ms.custom: seodec18
ms.openlocfilehash: 98c04c50bc4a52e9b2e4e267895fdd94888885f5
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2019
ms.locfileid: "68775161"
---
# <a name="build-scikit-learn-models-at-scale-with-azure-machine-learning-service"></a>Creación de modelos de Scikit-learn a escala con Azure Machine Learning Service

En este artículo, aprenda a ejecutar los scripts de entrenamiento de scikit-learn a escala empresarial mediante la clase [estimator de SKlearn](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py) de Azure Machine Learning. 

Los scripts de ejemplo de este artículo se usan para clasifica imágenes de flores Iris para crear un modelo de Machine Learning basado en el [conjunto de datos de Iris](https://archive.ics.uci.edu/ml/datasets/iris) de scikit-learn.

Tanto si va a entrenar un modelo de scikit-learn de Machine Learning desde el principio como si va a incorporar un modelo existente a la nube, puede usar Azure Machine Learning para escalar horizontalmente trabajos de entrenamiento de código abierto mediante recursos de proceso en la nube elástica. Puede compilar, implementar y supervisar modelos de nivel de producción, así como crear versiones de dichos mismos, mediante Azure Machine Learning.

## <a name="prerequisites"></a>Requisitos previos

Ejecute este código en cualquiera de estos entornos:
 - Máquina virtual de Notebook de Azure Machine Learning: no se necesitan descargas ni instalación

    - Complete el [Tutorial: Configuración del entorno y el área de trabajo](tutorial-1st-experiment-sdk-setup.md) para crear un servidor de cuadernos dedicado en el que se habrán cargado previamente el SDK y el repositorio de ejemplos.
    - Para encontrar un cuaderno completado y expandido, en la carpeta de entrenamiento de ejemplos en el servidor de cuadernos, vaya a este directorio: carpeta **how-to-use-azureml > training > train-hyperparameter-tune-deploy-with-sklearn**.

 - Su propio servidor de Jupyter Notebook

    - [Instalación del SDK de Azure Machine Learning para Python](setup-create-workspace.md#sdk)
    - [Creación de un archivo de configuración del área de trabajo](setup-create-workspace.md#write-a-configuration-file)
    - Descarga del conjunto de datos y el archivo de script de ejemplo 
        - [iris dataset](https://archive.ics.uci.edu/ml/datasets/iris)
        - [`train_iris.py`](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn)
    - También puede encontrar una [versión de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training/train-hyperparameter-tune-deploy-with-sklearn/train-hyperparameter-tune-deploy-with-sklearn.ipynb) completada de esta guía en la página de ejemplos de GitHub. El cuaderno incluye una sección ampliada que abarca la optimización de hiperparámetros inteligentes y la recuperación del mejor modelo mediante las métricas principales.

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

### <a name="create-a-machine-learning-experiment"></a>Creación de un experimento de aprendizaje automático

Cree un experimento y una carpeta para almacenar los scripts de entrenamiento. En este ejemplo, cree un experimento denominado "sklearn-iris".

```Python
project_folder = './sklearn-iris'
os.makedirs(project_folder, exist_ok=True)

exp = Experiment(workspace=ws, name='sklearn-iris')
```

### <a name="upload-dataset-and-scripts"></a>Carga del conjunto de datos y los scripts

El [almacén de datos](how-to-access-data.md) es un lugar donde se pueden almacenar datos y acceder a ellos mediante su montaje o copia en el destino de proceso. Cada área de trabajo proporciona un almacén de datos predeterminado. Cargue los datos y los scripts de entrenamiento en el almacén de datos para que sean fácilmente accesibles durante el entrenamiento.

1. Cree el directorio para los datos.

    ```Python
    os.makedirs('./data/iris', exist_ok=True)
    ```

1. Cargue el conjunto de datos Iris en el almacén de datos predeterminado.

    ```Python
    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data/iris', target_path='iris', overwrite=True, show_progress=True)
    ```

1. Cargue el script de entrenamiento de scikit-learn, `train_iris.py`.

    ```Python
    shutil.copy('./train_iris.py', project_folder)
    ```

## <a name="create-or-get-a-compute-target"></a>Creación u obtención de un destino de proceso

Cree un destino de proceso en el que ejecutar el trabajo de scikit-learn. Scikit-learn solo admite el procesamiento de CUP de nodo único.

El código siguiente crea un proceso administrado de Azure Machine Learning (AmlCompute) para el recurso de proceso de entrenamiento remoto. La creación de AmlCompute tarda aproximadamente cinco minutos. Si el AmlCompute con ese nombre ya está en el área de trabajo, este código omitirá el proceso de creación.

```Python
cluster_name = "cpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_D2_V2', 
                                                           max_nodes=4)

    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Para más información sobre los destinos de proceso, vea el artículo [¿Qué es un destino de proceso?](concept-compute-target.md)

## <a name="create-a-scikit-learn-estimator"></a>Creación de un estimador de scikit-learn

El [estimador de scikit-learn](https://docs.microsoft.com/en-us/python/api/azureml-train-core/azureml.train.sklearn?view=azure-ml-py) proporciona una manera sencilla de iniciar un trabajo de entrenamiento de scikit-learn en un destino de proceso. Se implementa a través de la clase [`SKLearn`](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.sklearn.sklearn?view=azure-ml-py), que se puede usar para admitir el entrenamiento de CPU de nodo único.

Si su script de entrenamiento necesita paquetes adicionales PIP o Conda para ejecutarse, puede hacer que los paquetes se instalen en la imagen de docker resultante. Para ello, pase sus nombres mediante los argumentos `pip_packages` y `conda_packages`.

```Python
from azureml.train.sklearn import SKLearn

script_params = {
    '--kernel': 'linear',
    '--penalty': 1.0,
}

estimator = SKLearn(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    entry_script='train_iris.py'
                    pip_packages=['joblib']
                   )
```

## <a name="submit-a-run"></a>Envío de una ejecución

El [objeto Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) proporciona la interfaz para el historial de ejecución mientras se ejecuta el trabajo y cuando se ha completado.

```Python
run = experiment.submit(estimator)
run.wait_for_completion(show_output=True)
```

Durante la ejecución de Run, pasa por las fases siguientes:

- **Preparación**: se crea una imagen de Docker según el estimador de TensorFlow. La imagen se carga en el registro de contenedor del área de trabajo y se almacena en memoria caché para ejecuciones posteriores. Los registros también se transmiten al historial de ejecución y se pueden consultar para supervisar el progreso.

- **Escalado**: el clúster intenta escalar verticalmente si el clúster de Batch AI requiere más nodos para realizar la ejecución de los que se encuentran disponibles actualmente.

- **Running**: todos los scripts de la carpeta de scripts se cargan en el destino de proceso, se montan o se copian los almacenes de datos y se ejecuta entry_script. Las salidas de stdout y la carpeta ./logs se transmiten al historial de ejecución y se pueden usar para supervisar la ejecución.

- **Posprocesamiento**: la carpeta ./outputs de la ejecución se copia en el historial de ejecución.

## <a name="save-and-register-the-model"></a>Guardado y registro del modelo

Una vez que haya entrenado el modelo, puede guardarlo y registrarlo en el área de trabajo. El registro del modelo permite almacenar los modelos y crear versiones de ellos en el área de trabajo para simplificar la [administración e implementación de modelos](concept-model-management-and-deployment.md).

Para guardar el modelo, agregue el código siguiente al script de entrenamiento, train_iris.py. 

``` Python
import joblib

joblib.dump(svm_model_linear, 'model.joblib')
```

Registre el modelo en el área de trabajo con el código siguiente.

```Python
model = run.register_model(model_name='sklearn-iris', model_path='model.joblib')
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, entrenó y registró un modelo de clasificación de Machine Learning con scikit-learn en Azure Machine Learning Service.

* Para obtener información sobre cómo implementar un modelo, continúe con nuestro artículo sobre [implementación de modelos](how-to-deploy-and-where.md).

* [Ajuste de hiperparámetros](how-to-tune-hyperparameters.md).

* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md).

* Más información sobre [aprendizaje profundo frente a aprendizaje automático](concept-deep-learning-vs-machine-learning.md).