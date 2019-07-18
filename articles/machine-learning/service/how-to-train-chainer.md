---
title: Entrenamiento y registro de modelos de Chainer
titleSuffix: Azure Machine Learning service
description: En este artículo se muestra cómo entrenar y registrar un modelo de Chainer mediante Azure Machine Learning Service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 06/15/2019
ms.openlocfilehash: 5057a8cf6c8769761a1a1a9f513ec46d152289ab
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67488672"
---
# <a name="train-and-register-chainer-models-at-scale-with-azure-machine-learning-service"></a>Entrenamiento y registro de modelos de Chainer a escala con Azure Machine Learning Service

En este artículo se muestra cómo entrenar y registrar un modelo de Chainer mediante Azure Machine Learning Service. Usa el conocido [conjunto de datos MNIST](http://yann.lecun.com/exdb/mnist/) para clasificar los dígitos escritos a mano mediante una red neuronal profunda (DNN) creada con la [biblioteca Chainer de Python](https://Chainer.org) que se ejecuta sobre [numpy](https://www.numpy.org/).

Chainer es una API de red neuronal general capaz de ejecutarse sobre otros marcos DNN conocidos para simplificar el desarrollo. Con Azure Machine Learning Service, puede escalar horizontalmente con rapidez trabajos de entrenamiento mediante recursos de proceso elásticos en la nube. También puede realizar seguimiento de las ejecuciones de entrenamiento, los modelos de versión, los modelos de implementación y mucho más.

Con independencia de que desarrolle un modelo Chainer desde el principio o lleve uno existente a la nube, Azure Machine Learning Service le puede ayudar a crear modelos para entornos de producción.

Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="prerequisites"></a>Requisitos previos

Este código se ejecute en cualquiera de estos entornos:

- Máquina virtual de cuadernos de Azure Machine Learning: no se necesitan descargas ni instalación

    - Complete el [inicio rápido de cuadernos basados en la nube](quickstart-run-cloud-notebook.md) para crear un servidor de cuadernos dedicado en el que se habrán cargado previamente el SDK y el repositorio de ejemplos.
    - En la carpeta de ejemplos del servidor de cuadernos, busque un cuaderno completado y archivos en la carpeta **how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer**.  El cuaderno incluye secciones expandidas que abarcan el ajuste de hiperparámetros inteligente, la implementación de modelos y widgets de cuaderno.

- Su propio servidor de Jupyter Notebook

    - [Instalación del SDK de Azure Machine Learning para Python](setup-create-workspace.md#sdk)
    - [Creación de un archivo de configuración del área de trabajo](setup-create-workspace.md#write-a-configuration-file)
    - Descarga del archivo de script de ejemplo [chainer_mnist.py](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/chainer_mnist.py)
     - También puede encontrar una [versión de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/training-with-deep-learning/train-hyperparameter-tune-deploy-with-chainer/train-hyperparameter-tune-deploy-with-chainer.ipynb) completada de esta guía en la página de ejemplos de GitHub. El cuaderno incluye secciones expandidas que abarcan el ajuste de hiperparámetros inteligente, la implementación de modelos y widgets de cuaderno.

## <a name="set-up-the-experiment"></a>Configuración del experimento

En esta sección, para configurar el experimento de entrenamiento, se cargan los paquetes de Python necesarios, se inicializa un área de trabajo, se crea un experimento y se cargan los datos y scripts de entrenamiento.

### <a name="import-packages"></a>Importación de paquetes

En primer lugar, importe la biblioteca azureml.core de Python y muestre el número de versión.

```
# Check core SDK version number
import azureml.core

print("SDK version:", azureml.core.VERSION)
```

### <a name="initialize-a-workspace"></a>Inicialización de un área de trabajo

El [área de trabajo de Azure Machine Learning Service](concept-workspace.md) es el recurso de nivel superior del servicio. Proporciona un lugar centralizado para trabajar con todos los artefactos que cree. En el SDK de Python, puede acceder a los artefactos de área de trabajo mediante la creación de un objeto [`workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace.workspace?view=azure-ml-py).

Cree un objeto de área de trabajo a partir del archivo `config.json` creado en la [sección de requisitos previos](#prerequisites).

```Python
ws = Workspace.from_config()
```

### <a name="create-a-project-directory"></a>Creación de un directorio del proyecto
Cree un directorio que contendrá todo el código necesario del equipo local al que necesitará acceder desde el recurso remoto. Esto incluye el script de entrenamiento y los archivos adicionales de los que dependa el script de entrenamiento.

```
import os

project_folder = './chainer-mnist'
os.makedirs(project_folder, exist_ok=True)
```

### <a name="prepare-training-script"></a>Preparación del script de entrenamiento

En este tutorial, el script de entrenamiento **chainer_mnist.py** ya se proporciona. En la práctica, debería poder usar cualquier script de entrenamiento personalizado tal cual y ejecutarlo con Azure Machine Learning sin tener que modificar el código.

Para usar las funcionalidades de seguimiento y métricas de Azure Machine Learning, deberá agregar una pequeña cantidad de código de Azure Machine Learning dentro del script de entrenamiento.  El script de entrenamiento **chainer_mnist.py** muestra cómo registrar algunas métricas para la ejecución de Azure Machine Learning. Para ello, debe acceder al objeto `Run` de Azure Machine Learning dentro del script.

Copie el script de entrenamiento **chainer_mnist.py** en el directorio del proyecto.

```
import shutil

shutil.copy('chainer_mnist.py', project_folder)
```

### <a name="create-an-experiment"></a>Creación de un experimento

Cree un experimento y una carpeta para almacenar los scripts de entrenamiento. En este ejemplo, cree un experimento denominado "chainer-mnist".

```
from azureml.core import Experiment

experiment_name = 'chainer-mnist'
experiment = Experiment(ws, name=experiment_name)
```


## <a name="create-or-get-a-compute-target"></a>Creación u obtención de un destino de proceso

Para entrenar el modelo, necesitará un [destino de proceso](concept-compute-target.md). En este tutorial, usará el proceso administrado de Azure Machine Learning (AmlCompute) el recurso de proceso de entrenamiento remoto.

**La creación de AmlCompute tarda aproximadamente 5 minutos**. Si el AmlCompute con ese nombre ya está en el área de trabajo, este código omitirá el proceso de creación.  

```Python
from azureml.core.compute import ComputeTarget, AmlCompute
from azureml.core.compute_target import ComputeTargetException

# choose a name for your cluster
cluster_name = "gpu-cluster"

try:
    compute_target = ComputeTarget(workspace=ws, name=cluster_name)
    print('Found existing compute target.')
except ComputeTargetException:
    print('Creating a new compute target...')
    compute_config = AmlCompute.provisioning_configuration(vm_size='STANDARD_NC6', 
                                                           max_nodes=4)

    # create the cluster
    compute_target = ComputeTarget.create(ws, cluster_name, compute_config)

    compute_target.wait_for_completion(show_output=True)

# use get_status() to get a detailed status for the current cluster. 
print(compute_target.get_status().serialize())
```

Para obtener más información sobre los destinos de proceso, consulte el artículo [¿Qué es un destino de proceso?](concept-compute-target.md)

## <a name="create-a-chainer-estimator"></a>Creación de un estimador de Chainer

El [estimador de Chainer](https://docs.microsoft.com/python/api/azureml-train-core/azureml.train.dnn.chainer?view=azure-ml-py) proporciona una manera sencilla de iniciar trabajos de entrenamiento de Chainer en el destino de proceso.

El estimador de Chainer se implementa a través de la clase genérica [`estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py), que se puede usar para admitir cualquier marco de trabajo. Para obtener más información sobre cómo entrenar modelos con el estimador genérico, consulte [Entrenamiento de modelos con Azure Machine Learning mediante un objeto Estimator](how-to-train-ml-models.md).

```Python
from azureml.train.dnn import Chainer

script_params = {
    '--epochs': 10,
    '--batchsize': 128,
    '--output_dir': './outputs'
}

estimator = Chainer(source_directory=project_folder, 
                    script_params=script_params,
                    compute_target=compute_target,
                    pip_packages=['numpy', 'pytest'],
                    entry_script='chainer_mnist.py',
                    use_gpu=True)
```

## <a name="submit-a-run"></a>Envío de una ejecución

El [objeto Run](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py) proporciona la interfaz para el historial de ejecución mientras se ejecuta el trabajo y cuando se ha completado.

```Python
run = exp.submit(est)
run.wait_for_completion(show_output=True)
```

Durante la ejecución de Run, pasa por las fases siguientes:

- **Preparando**: se crea una imagen de Docker según el estimador de Chainer. La imagen se carga en el registro de contenedor del área de trabajo y se almacena en caché para ejecuciones posteriores. Los registros también se transmiten al historial de ejecución y se pueden consultar para supervisar el progreso.

- **Escalado**: el clúster intenta escalar verticalmente si el clúster de Batch AI requiere más nodos para realizar la ejecución de los que se encuentran disponibles.

- **Running**: todos los scripts de la carpeta de scripts se cargan en el destino de proceso, se montan o se copian los almacenes de datos, y se ejecuta entry_script. Las salidas de stdout y la carpeta ./logs se transmiten al historial de ejecución y se pueden usar para supervisar la ejecución.

- **Posprocesamiento**: la carpeta ./outputs de la ejecución se copia en el historial de ejecución.

## <a name="save-and-register-the-model"></a>Guardado y registro del modelo

Una vez que haya entrenado el modelo, puede guardarlo y registrarlo en el área de trabajo. El registro del modelo permite almacenar y crear versiones de los modelos en el área de trabajo para simplificar la [administración e implementación de modelos](concept-model-management-and-deployment.md).

Para guardar el modelo, agregue el código siguiente al script de entrenamiento, **chainer_mnist.py**. 

``` Python
    serializers.save_npz(os.path.join(args.output_dir, 'model.npz'), model)
```

Registre el modelo al área de trabajo con el código siguiente.

```Python
model = run.register_model(model_name='chainer-dnn-mnist', model_path='outputs/model.npz')
```



## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha entrenado un modelo de Chainer en Azure Machine Learning Service. 

* Para obtener información sobre cómo implementar un modelo, continúe con nuestro artículo sobre [implementación de modelos](how-to-deploy-and-where.md).

* [Ajustar los hiperparámetros](how-to-tune-hyperparameters.md)

* [Seguir métricas de ejecución durante el entrenamiento](how-to-track-experiments.md)
