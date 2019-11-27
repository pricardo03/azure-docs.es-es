---
title: Destinos de proceso remoto de ML automatizado
titleSuffix: Azure Machine Learning
description: Aprenda a crear modelos mediante el aprendizaje automático automatizado en un destino de proceso remoto de Azure Machine Learning con Azure Machine Learning.
services: machine-learning
author: cartacioS
ms.author: sacartac
ms.reviewer: sgilley
ms.service: machine-learning
ms.subservice: core
ms.workload: data-services
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 5104e6e037341c41a032f80287c6d56d17361d4c
ms.sourcegitcommit: a10074461cf112a00fec7e14ba700435173cd3ef
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73932194"
---
# <a name="train-models-with-automated-machine-learning-in-the-cloud"></a>Entrenamiento de modelos con aprendizaje automático automatizado en la nube

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../../includes/aml-applies-to-basic-enterprise-sku.md)]

En Azure Machine Learning, puede entrenar un modelo en los diferentes tipos de recursos de proceso que administra. El destino de proceso podría ser un equipo local o un recurso en la nube.

Mediante la incorporación de destinos de procesos adicionales como el proceso de Azure Machine Learning (AmlCompute), resulta fácil escalar vertical y horizontalmente el experimento de aprendizaje automático. AmlCompute es una infraestructura de proceso administrado que permite crear fácilmente un proceso de uno o varios nodos.

En este artículo, aprenderá a crear un modelo mediante ML automatizado con AmlCompute.

## <a name="how-does-remote-differ-from-local"></a>¿En qué se diferencia el modo remoto del local?

El tutorial "[Uso del aprendizaje automático para crear un modelo de regresión](tutorial-auto-train-models.md)" le enseña a usar un equipo local para entrenar un modelo con ML automatizado. El flujo de trabajo del entrenamiento local también se aplica a los destinos remotos. Sin embargo, con un proceso remoto, las iteraciones del experimento de ML automatizado se ejecutan de forma asincrónica. Esta funcionalidad le permite cancelar una iteración determinada, ver el estado de la ejecución y continuar trabajando en otras celdas del cuaderno de Jupyter. Para entrenar de forma remota, en primer lugar cree un destino de proceso remoto, como AmlCompute. A continuación, configure el recurso remoto y envíe el código allí.

En este artículo se muestran los pasos adicionales necesarios para ejecutar un experimento de ML automatizado en un destino AmlCompute remoto. Aquí se usa, en todo el código, el objeto del área de trabajo, `ws` del tutorial.

```python
ws = Workspace.from_config()
```

## <a name="create-resource"></a>Crear el recurso

Cree el destino AmlCompute en el área de trabajo (`ws`) si aún no existe.

**Tiempo estimado**: La creación del destino AmlCompute tarda aproximadamente cinco minutos.

```python
from azureml.core.compute import AmlCompute
from azureml.core.compute import ComputeTarget

amlcompute_cluster_name = "automlcl"  # Name your cluster
provisioning_config = AmlCompute.provisioning_configuration(vm_size="STANDARD_D2_V2",
                                                            # for GPU, use "STANDARD_NC6"
                                                            # vm_priority = 'lowpriority', # optional
                                                            max_nodes=6)
compute_target = ComputeTarget.create(
    ws, amlcompute_cluster_name, provisioning_config)

# Can poll for a minimum number of nodes and for a specific timeout.
# If no min_node_count is provided, it will use the scale settings for the cluster.
compute_target.wait_for_completion(
    show_output=True, min_node_count=None, timeout_in_minutes=20)
```

Ahora puede usar el objeto `compute_target` como destino del proceso remoto.

Entre las restricciones en los nombres del clúster se incluyen:
+ La longitud debe ser inferior a 64 caracteres.
+ No puede contener ninguno de los caracteres siguientes: {`\` ~ ! @ # $ % ^ & * ( ) = + _ [ ] { } \\\\ | ; : \' \\" , < > / ?.`

## <a name="access-data-using-tabulardataset-function"></a>Acceso a los datos mediante la función TabularDataset

Se han definido X e y como `TabularDataset`s, las cuales se pasan a Machine Learning Automatizado en AutoMLConfig. `from_delimited_files` establece de forma predeterminada `infer_column_types` en true, lo cual hará que se infiera el tipo de columnas automáticamente. 

Si desea establecer manualmente los tipos de columna, puede establecer el argumento `set_column_types` para poder establecer manualmente el tipo de cada una de las columnas. En el siguiente ejemplo de código, los datos proceden del paquete sklearn.

```python
# Create a project_folder if it doesn't exist
if not os.path.isdir('data'):
    os.mkdir('data')
    
if not os.path.exists(project_folder):
    os.makedirs(project_folder)

from sklearn import datasets
from azureml.core.dataset import Dataset
from scipy import sparse
import numpy as np
import pandas as pd

data_train = datasets.load_digits()

pd.DataFrame(data_train.data[100:,:]).to_csv("data/X_train.csv", index=False)
pd.DataFrame(data_train.target[100:]).to_csv("data/y_train.csv", index=False)

ds = ws.get_default_datastore()
ds.upload(src_dir='./data', target_path='digitsdata', overwrite=True, show_progress=True)

X = Dataset.Tabular.from_delimited_files(path=ds.path('digitsdata/X_train.csv'))
y = Dataset.Tabular.from_delimited_files(path=ds.path('digitsdata/y_train.csv'))

```

## <a name="create-run-configuration"></a>Creación de una configuración de ejecución

Para que las dependencias estén disponibles para el script get_data.py, defina un objeto `RunConfiguration` con `CondaDependencies` definido. Utilice este objeto para el parámetro `run_configuration` en `AutoMLConfig`.

```python
from azureml.core.runconfig import RunConfiguration
from azureml.core.conda_dependencies import CondaDependencies

run_config = RunConfiguration(framework="python")
run_config.target = compute_target
run_config.environment.docker.enabled = True
run_config.environment.docker.base_image = azureml.core.runconfig.DEFAULT_CPU_IMAGE

dependencies = CondaDependencies.create(
    pip_packages=["scikit-learn", "scipy", "numpy"])
run_config.environment.python.conda_dependencies = dependencies
```

Consulte este [cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/remote-amlcompute/auto-ml-remote-amlcompute.ipynb) para un ejemplo adicional de este modelo de diseño.

## <a name="configure-experiment"></a>Configuración del experimento
Especifique la configuración de `AutoMLConfig`.  (Consulte una [lista completa de parámetros](how-to-configure-auto-train.md#configure-experiment) y sus posibles valores).

```python
from azureml.train.automl import AutoMLConfig
import time
import logging

automl_settings = {
    "name": "AutoML_Demo_Experiment_{0}".format(time.time()),
    "iteration_timeout_minutes": 10,
    "iterations": 20,
    "n_cross_validations": 5,
    "primary_metric": 'AUC_weighted',
    "preprocess": False,
    "max_concurrent_iterations": 10,
    "verbosity": logging.INFO
}

automl_config = AutoMLConfig(task='classification',
                             debug_log='automl_errors.log',
                             path=project_folder,
                             compute_target=compute_target,
                             run_configuration=run_config,
                             X = X,
                             y = y,
                             **automl_settings,
                             )
```

## <a name="submit-training-experiment"></a>Envío del experimento de entrenamiento

Ahora, envíe la configuración para seleccionar automáticamente el algoritmo y los hiperparámetros, y entrenar el modelo.

```python
from azureml.core.experiment import Experiment
experiment = Experiment(ws, 'automl_remote')
remote_run = experiment.submit(automl_config, show_output=True)
```

Verá un resultado similar al del siguiente ejemplo:

    Running on remote compute: mydsvmParent Run ID: AutoML_015ffe76-c331-406d-9bfd-0fd42d8ab7f6
    ***********************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE:  A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    ***********************************************************************************************

     ITERATION     PIPELINE                               DURATION                METRIC      BEST
             2      Standardize SGD classifier            0:02:36                  0.954     0.954
             7      Normalizer DT                         0:02:22                  0.161     0.954
             0      Scale MaxAbs 1 extra trees            0:02:45                  0.936     0.954
             4      Robust Scaler SGD classifier          0:02:24                  0.867     0.954
             1      Normalizer kNN                        0:02:44                  0.984     0.984
             9      Normalizer extra trees                0:03:15                  0.834     0.984
             5      Robust Scaler DT                      0:02:18                  0.736     0.984
             8      Standardize kNN                       0:02:05                  0.981     0.984
             6      Standardize SVM                       0:02:18                  0.984     0.984
            10      Scale MaxAbs 1 DT                     0:02:18                  0.077     0.984
            11      Standardize SGD classifier            0:02:24                  0.863     0.984
             3      Standardize gradient boosting         0:03:03                  0.971     0.984
            12      Robust Scaler logistic regression     0:02:32                  0.955     0.984
            14      Scale MaxAbs 1 SVM                    0:02:15                  0.989     0.989
            13      Scale MaxAbs 1 gradient boosting      0:02:15                  0.971     0.989
            15      Robust Scaler kNN                     0:02:28                  0.904     0.989
            17      Standardize kNN                       0:02:22                  0.974     0.989
            16      Scale 0/1 gradient boosting           0:02:18                  0.968     0.989
            18      Scale 0/1 extra trees                 0:02:18                  0.828     0.989
            19      Robust Scaler kNN                     0:02:32                  0.983     0.989


## <a name="explore-results"></a>Exploración de los resultados

Puede usar el mismo [widget de Jupyter](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) que se muestra en el [tutorial de entrenamiento](tutorial-auto-train-models.md#explore-the-results) para ver un gráfico y la tabla de resultados.

```python
from azureml.widgets import RunDetails
RunDetails(remote_run).show()
```

Esta es una imagen estática del widget.  En el cuaderno, puede hacer clic en cualquier línea de la tabla para ver las propiedades de ejecución y los registros de salida para los que se ejecutan.   También puede usar la lista desplegable encima del grafo para ver un grafo de cada métrica disponible para cada iteración.

![tabla de widget](./media/how-to-auto-train-remote/table.png)
![trazado de widget](./media/how-to-auto-train-remote/plot.png)

El widget muestra una dirección URL que puede usar para ver y explorar los detalles de ejecución individuales.  

Si no está en un cuaderno de Jupyter, puede mostrar la URL desde la propia ejecución:

```
remote_run.get_portal_url()
```

La misma información está disponible en el área de trabajo.  Para obtener más información sobre estos resultados, consulte [Descripción de los resultados del aprendizaje automático automatizado](how-to-understand-automated-ml.md).

## <a name="example"></a>Ejemplo

En el siguiente [cuaderno](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression/auto-ml-regression.ipynb) se demuestran los conceptos de este artículo.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Pasos siguientes

* Más información acerca de [cómo configurar opciones para el aprendizaje automático](how-to-configure-auto-train.md).
* Consulte el [procedimiento](how-to-machine-learning-interpretability-automl.md) sobre cómo habilitar las características de interpretación de modelos en experimentos de ML automatizados.
