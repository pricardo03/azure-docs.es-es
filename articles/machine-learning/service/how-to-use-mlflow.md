---
title: Uso de MLflow con Azure Machine Learning Service
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo registrar métricas y artefactos con la biblioteca MLflow en Azure Machine Learning Service.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: nibaccam
ms.topic: conceptual
ms.date: 06/10/2019
ms.custom: seodec18
ms.openlocfilehash: d0bc4620d0c55d6e94a3d99c39ab405dab2743e5
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67461647"
---
# <a name="use-mlflow-with-azure-machine-learning-service-preview"></a>Uso de MLflow con Azure Machine Learning Service (versión preliminar)

En este artículo se muestra cómo usar el URI de seguimiento y la API de registro de MLflow (conocidos de forma conjunta como "Seguimiento de MLflow") con Azure Machine Learning Service para llevar a cabo un seguimiento y un registro de los artefactos y las métricas de los experimentos en el [área de trabajo de Azure Machine Learning Service](https://docs.microsoft.com/azure/machine-learning/service/concept-azure-machine-learning-architecture#workspace). Si ya usa el Seguimiento de MLflow para los experimentos, el área de trabajo proporciona una ubicación centralizada, segura y escalable para almacenar los modelos y las métricas de entrenamiento.

[MLflow](https://www.mlflow.org) es una biblioteca de código abierto para administrar el ciclo de vida de los experimentos de aprendizaje automático. El [Seguimiento de MLFlow](https://mlflow.org/docs/latest/quickstart.html#using-the-tracking-api) es un componente de MLflow que lleva a cabo un registro y un seguimiento de las métricas de ejecución de entrenamiento y los artefactos del modelo, independientemente de si los experimentos se ejecutan de forma local, en una máquina virtual o en un clúster de proceso remoto.
![Diagrama de MLflow con Azure Machine Learning](media/how-to-use-mlflow/mlflow-diagram.png)

## <a name="compare-mlflow-and-azure-machine-learning-clients"></a>Comparación entre los clientes de MLflow y Azure Machine Learning

 En la tabla siguiente se resumen los distintos clientes que pueden usar Azure Machine Learning Service y sus respectivas funcionalidades.

 El Seguimiento de MLflow ofrece funciones de registro de métricas y almacenamiento de artefactos que de otra manera solo están disponibles a través del [SDK de Python de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

| | Seguimiento de MLflow | Azure Machine Learning <br> SDK de Python |  Azure Machine Learning <br> CLI | Portal de Azure|
|-|-|-|-|-|
| Administración del área de trabajo |   | ✓ |  ✓ | ✓  |
| Uso de almacenes de datos  |   | ✓ |  ✓ |    |
| Métricas de registro      | ✓ | ✓ |    |    |
| Carga de artefactos | ✓ | ✓ |    |    |
| Visualización de métricas     | ✓ | ✓ | ✓  | ✓ |
| Administración de procesos   |   | ✓ | ✓  | ✓ |
| Implementación de modelos    |   | ✓ |   ✓ | ✓ |

## <a name="prerequisites"></a>Requisitos previos

* [Instale MLflow](https://mlflow.org/docs/latest/quickstart.html).
* [Instale el SDK de Python de Azure Machine Learning en el equipo local y cree un área de trabajo de Azure Machine Learning](setup-create-workspace.md#sdk). El SDK proporciona conectividad para que MLflow acceda al área de trabajo.

## <a name="track-local-runs"></a>Seguimiento de ejecuciones locales

Instale el paquete `azureml-contrib-run` que va a usar el Seguimiento de MLflow con Azure Machine Learning en los experimentos ejecutados localmente en un editor de código o Jupyter Notebook.

```shell
pip install azureml-contrib-run
```

>[!NOTE]
>El espacio de nombres azureml.contrib cambia con frecuencia, ya que estamos trabajando para mejorar el servicio. Por lo tanto, todo el contenido de este espacio de nombres debe considerarse como una versión preliminar, no completamente compatible con Microsoft.

Importe las clases `mlflow` y [`Workspace`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py) para acceder al URI de seguimiento de MLflow y configurar el área de trabajo.

En el código siguiente, el método `get_mlflow_tracking_uri()` asigna un dirección URI de seguimiento única al área de trabajo (`ws`) y `set_tracking_uri()` apunta el URI de seguimiento de MLflow a esa dirección.

```Python
import mlflow
from azureml.core import Workspace

ws = Workspace.from_config()

mlflow.set_tracking_uri(ws.get_mlflow_tracking_uri())
```

>[!NOTE]
>El URI de seguimiento es válido hasta una hora como máximo. Si reinicia el script después de un tiempo de inactividad, use la API get_mlflow_tracking_uri para obtener un nuevo URI.

Establezca el nombre del experimento de MLflow con `set_experiment()` y comience la ejecución de entrenamiento con `start_run()`. Después, use `log_metric()` para activar la API de registro de MLflow y empezar a registrar las métricas de la ejecución de entrenamiento.

```Python
experiment_name = "experiment_with_mlflow"
mlflow.set_experiment(experiment_name)

with mlflow.start_run():
    mlflow.log_metric('alpha', 0.03)
```

## <a name="track-remote-runs"></a>Seguimiento de ejecuciones remotas

Las ejecuciones remotas permiten entrenar modelos en procesos más eficaces, como máquinas virtuales habilitadas para GPU o clústeres de Proceso de Machine Learning. Consulte [Configuración de destinos de proceso del entrenamiento del modelo](how-to-set-up-training-targets.md) para obtener información sobre las distintas opciones de proceso.

Configure el proceso y el entorno de ejecución de entrenamiento con la clase [`Environment`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py). Incluya paquetes de PIP `mlflow` y `azure-contrib-run` en la sección [`CondaDependencies`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) del entorno. Después, construya [`ScriptRunConfig`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) con el proceso remoto como destino de proceso.

```Python

from azureml.core import Environment
from azureml.core.conda_dependencies import CondaDependencies
from azureml.core import ScriptRunConfig

exp = Experiment(workspace = "my_workspace",
                 name = "my_experiment")

mlflow_env = Environment(name="mlflow-env")

cd = CondaDependencies.create(pip_packages = ["mlflow", "azureml-contrib-run"])

mlflow_env.python.conda_dependencies=cd

src = ScriptRunConfig(source_directory="./my_script_location", script="my_training_script.py")

src.run_config.target = "my-remote-compute-compute"
src.run_config.environment = mlflow_env
```

En el script de entrenamiento, importe `mlflow` para usar las API de registro de MLflow y empiece a registrar las métricas de ejecución.

```Python
import mlflow

with mlflow.start_run():
    mlflow.log_metric("example", 1.23)
```

Con esta configuración del proceso y de la ejecución de entrenamiento, use el método `Experiment.submit("train.py")` para enviar una ejecución. Esto establece automáticamente el URI de seguimiento de MLflow y dirige el registro de MLflow al área de trabajo.

```Python
run = exp.submit(src)
```

## <a name="view-metrics-and-artifacts-in-your-workspace"></a>Visualización de las métricas y los artefactos en el área de trabajo

Las métricas y los artefactos procedentes del registro de MLflow se conservan en el área de trabajo de [Azure Portal](https://portal.azure.com). Para verlos en cualquier momento, vaya al área de trabajo y busque el experimento por nombre.

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no tiene pensado usar los artefactos o las métricas registradas en el área de trabajo, la funcionalidad para eliminarlos de forma individual no está disponible actualmente. Por ello, deberá eliminar el grupo de recursos que contiene la cuenta de almacenamiento y el área de trabajo para no incurrir en cargos:

1. En Azure Portal, seleccione **Grupos de recursos** a la izquierda del todo.

   ![Eliminación en Azure Portal](media/how-to-use-mlflow/delete-resources.png)

1. En la lista, seleccione el grupo de recursos que creó.

1. Seleccione **Eliminar grupo de recursos**.

1. Escriba el nombre del grupo de recursos. A continuación, seleccione **Eliminar**.

## <a name="example-notebooks"></a>Cuadernos de ejemplo

En [MLflow con cuadernos de Azure ML](https://aka.ms/azureml-mlflow-examples) encontrará una explicación de los conceptos de este artículo.

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de modelos](how-to-deploy-and-where.md).
