---
title: 'Tutorial número 2: Entrenamiento de un modelo de regresión con aprendizaje automático: servicio Azure Machine Learning'
description: Aprenda a generar un modelo mediante el aprendizaje automático automatizado.  Azure Machine Learning realiza automáticamente el procesamiento previo de los datos, y la selección del algoritmo y de los hiperparámetros. Después, el modelo final se implementa con el servicio Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: tutorial
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.openlocfilehash: 0c4b9c31c4ae8a6a7a7044887c9af051966c745e
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52879538"
---
# <a name="tutorial-2-train-a-regression-model-with-automated-machine-learning"></a>Tutorial número 2: Entrenamiento de un modelo de regresión con aprendizaje automático automatizado

Este tutorial es la **segunda parte de dos**. En el tutorial anterior, [preparó los datos de taxi de Nueva York para el modelado de regresión](tutorial-data-prep.md).

Ahora, está listo para comenzar a crear el modelo con el servicio Azure Machine Learning. En esta parte del tutorial, utilizará los datos preparados y generará automáticamente un modelo de regresión para predecir las tarifas de los taxis. Mediante las funcionalidades de aprendizaje automático del servicio, puede definir las restricciones y objetivos del aprendizaje automático, iniciar el proceso de aprendizaje automático y permitir que se produzca la selección de algoritmos y el ajuste de hiperparámetros. La técnica de ML automatizado recorre en iteración muchas combinaciones de algoritmos e hiperparámetros hasta que encuentra el mejor modelo según su criterio.

![diagrama de flujo](./media/tutorial-auto-train-models/flow2.png)

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Configurar un entorno de Python y a importar los paquetes del SDK
> * Configurar un área de trabajo de Azure Machine Learning
> * Entrenar automáticamente un modelo de regresión
> * Ejecutar el modelo de forma local con parámetros personalizados
> * Exploración de los resultados
> * Registro del mejor modelo

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLfree) antes de empezar.

>[!NOTE]
> El código de este artículo se ha probado con el SDK de Azure Machine Learning, versión 1.0.0


## <a name="prerequisites"></a>Requisitos previos

> * [Tutorial sobre la ejecución de la preparación de datos](tutorial-data-prep.md).
> * Un entorno configurado de aprendizaje automático, por ejemplo, Azure Notebooks, un entorno local de Python o Data Science Virtual Machine. [Configurar](samples-notebooks.md) el aprendizaje automático.

## <a name="get-the-notebook"></a>Obtención del cuaderno

Para su comodidad, este tutorial está disponible como un [cuaderno de Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/tutorials/regression-part2-automated-ml.ipynb). Ejecute el cuaderno `regression-part2-automated-ml.ipynb` en Azure Notebooks o en su propio servidor de cuadernos de Jupyter.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

## <a name="import-packages"></a>Importar paquetes
Importe los paquetes de Python que necesite para este tutorial.


```python
import azureml.core
import pandas as pd
from azureml.core.workspace import Workspace
from azureml.train.automl.run import AutoMLRun
import time
import logging

```

## <a name="configure-workspace"></a>Configuración del área de trabajo

Cree un objeto de área de trabajo desde el área de trabajo existente. Un `Workspace` es una clase que acepta la información de la suscripción y los recursos de Azure, y crea un recurso en la nube para supervisar y realizar un seguimiento de las ejecuciones del modelo. `Workspace.from_config()` lee el archivo **aml_config/config.json** y carga los detalles en un objeto denominado `ws`.  En el resto del código de este tutorial se usa `ws`.

Cuando tenga un objeto del área de trabajo, especifique un nombre para el experimento, y cree y registre un directorio local con el área de trabajo. El historial de todas las ejecuciones se registra en el experimento especificado y en [Azure Portal](https://portal.azure.com).


```python
ws = Workspace.from_config()
# choose a name for the run history container in the workspace
experiment_name = 'automated-ml-regression'
# project folder
project_folder = './automated-ml-regression'

import os

output = {}
output['SDK version'] = azureml.core.VERSION
output['Subscription ID'] = ws.subscription_id
output['Workspace'] = ws.name
output['Resource Group'] = ws.resource_group
output['Location'] = ws.location
output['Project Directory'] = project_folder
pd.set_option('display.max_colwidth', -1)
pd.DataFrame(data=output, index=['']).T
```

## <a name="explore-data"></a>Exploración de los datos

Use el objeto de flujo de datos que se creó en el tutorial anterior. Abra y ejecute el flujo de datos y revise los resultados.


```python
import azureml.dataprep as dprep
package_saved = dprep.Package.open(".\dflow")
dflow_prepared = package_saved.dataflows[0]
dflow_prepared.get_profile()
```




<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Escriba</th>
      <th>Min</th>
      <th>max</th>
      <th>Recuento</th>
      <th>Falta el recuento</th>
      <th>No falta el recuento</th>
      <th>Falta el porcentaje</th>
      <th>Recuento de errores</th>
      <th>Recuento vacío</th>
      <th>Cuantil 0,1 %</th>
      <th>Cuantil 1 %</th>
      <th>Cuantil 5 %</th>
      <th>Cuantil 25 %</th>
      <th>Cuantil 50 %</th>
      <th>Cuantil 75 %</th>
      <th>Cuantil 95 %</th>
      <th>Cuantil 99 %</th>
      <th>Cuantil 99,9 %</th>
      <th>Media</th>
      <th>Desviación estándar</th>
      <th>Variance</th>
      <th>Asimetría</th>
      <th>Curtosis</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>vendor</th>
      <td>FieldType.STRING</td>
      <td>1</td>
      <td>VTS</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_weekday</th>
      <td>FieldType.STRING</td>
      <td>Viernes</td>
      <td>Miércoles</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_hour</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>23</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>3.57523</td>
      <td>3</td>
      <td>9.91106</td>
      <td>15.9327</td>
      <td>19</td>
      <td>22.0225</td>
      <td>23</td>
      <td>23</td>
      <td>14.2326</td>
      <td>6.34926</td>
      <td>40.3131</td>
      <td>-0.693335</td>
      <td>-0.459336</td>
    </tr>
    <tr>
      <th>pickup_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5.32313</td>
      <td>4.92308</td>
      <td>14.2214</td>
      <td>29.5244</td>
      <td>44.6436</td>
      <td>56.3767</td>
      <td>58.9798</td>
      <td>59</td>
      <td>29.4635</td>
      <td>17.4396</td>
      <td>304.14</td>
      <td>0.00440324</td>
      <td>-1.20458</td>
    </tr>
    <tr>
      <th>pickup_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>4.99286</td>
      <td>4.91954</td>
      <td>14.6121</td>
      <td>29.9239</td>
      <td>44.5221</td>
      <td>56.6792</td>
      <td>59</td>
      <td>59</td>
      <td>29.6225</td>
      <td>17.3868</td>
      <td>302.302</td>
      <td>-0.0227466</td>
      <td>-1.19409</td>
    </tr>
    <tr>
      <th>dropoff_weekday</th>
      <td>FieldType.STRING</td>
      <td>Viernes</td>
      <td>Miércoles</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>dropoff_hour</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>23</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>3.23217</td>
      <td>2.93333</td>
      <td>9.92334</td>
      <td>15.9135</td>
      <td>19</td>
      <td>22.2739</td>
      <td>23</td>
      <td>23</td>
      <td>14.1815</td>
      <td>6.45578</td>
      <td>41.677</td>
      <td>-0.691001</td>
      <td>-0.500215</td>
    </tr>
    <tr>
      <th>dropoff_minute</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5.1064</td>
      <td>5</td>
      <td>14.2051</td>
      <td>29.079</td>
      <td>44.2937</td>
      <td>56.6338</td>
      <td>58.9984</td>
      <td>59</td>
      <td>29.353</td>
      <td>17.4241</td>
      <td>303.598</td>
      <td>0.0142562</td>
      <td>-1.21531</td>
    </tr>
    <tr>
      <th>dropoff_second</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>59</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>5.03373</td>
      <td>5</td>
      <td>14.7471</td>
      <td>29.598</td>
      <td>45.3216</td>
      <td>56.1044</td>
      <td>58.9728</td>
      <td>59</td>
      <td>29.7923</td>
      <td>17.481</td>
      <td>305.585</td>
      <td>-0.0281313</td>
      <td>-1.21965</td>
    </tr>
    <tr>
      <th>store_forward</th>
      <td>FieldType.STRING</td>
      <td>N</td>
      <td>Y</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <th>pickup_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.0782</td>
      <td>-73.7365</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.0763</td>
      <td>-73.9625</td>
      <td>-73.9629</td>
      <td>-73.949</td>
      <td>-73.9279</td>
      <td>-73.8667</td>
      <td>-73.8304</td>
      <td>-73.8232</td>
      <td>-73.7698</td>
      <td>-73.9139</td>
      <td>0.0487111</td>
      <td>0.00237277</td>
      <td>0.402697</td>
      <td>-0.613516</td>
    </tr>
    <tr>
      <th>pickup_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.5755</td>
      <td>40.8799</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.6329</td>
      <td>40.7131</td>
      <td>40.7116</td>
      <td>40.7214</td>
      <td>40.7581</td>
      <td>40.8051</td>
      <td>40.8489</td>
      <td>40.8676</td>
      <td>40.8777</td>
      <td>40.7652</td>
      <td>0.0483485</td>
      <td>0.00233758</td>
      <td>0.228088</td>
      <td>-0.598862</td>
    </tr>
    <tr>
      <th>dropoff_longitude</th>
      <td>FieldType.DECIMAL</td>
      <td>-74.0857</td>
      <td>-73.7209</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>-74.0788</td>
      <td>-73.9856</td>
      <td>-73.9858</td>
      <td>-73.959</td>
      <td>-73.9367</td>
      <td>-73.8848</td>
      <td>-73.8155</td>
      <td>-73.7767</td>
      <td>-73.7335</td>
      <td>-73.9207</td>
      <td>0.055961</td>
      <td>0.00313163</td>
      <td>0.648649</td>
      <td>0.0229141</td>
    </tr>
    <tr>
      <th>dropoff_latitude</th>
      <td>FieldType.DECIMAL</td>
      <td>40.5835</td>
      <td>40.8797</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>40.5977</td>
      <td>40.6954</td>
      <td>40.6951</td>
      <td>40.7275</td>
      <td>40.7582</td>
      <td>40.7884</td>
      <td>40.8504</td>
      <td>40.868</td>
      <td>40.8786</td>
      <td>40.7595</td>
      <td>0.0504621</td>
      <td>0.00254642</td>
      <td>0.0484179</td>
      <td>-0.0368799</td>
    </tr>
    <tr>
      <th>passengers</th>
      <td>FieldType.DECIMAL</td>
      <td>1</td>
      <td>6</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>6</td>
      <td>6</td>
      <td>2.32979</td>
      <td>1.79978</td>
      <td>3.2392</td>
      <td>0.834099</td>
      <td>-1.11111</td>
    </tr>
    <tr>
      <th>cost</th>
      <td>FieldType.DECIMAL</td>
      <td>0</td>
      <td>444</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>7059.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0.0</td>
      <td>0</td>
      <td>3.01808</td>
      <td>3.0125</td>
      <td>5.91545</td>
      <td>9.49055</td>
      <td>16.5816</td>
      <td>33.5638</td>
      <td>51.9924</td>
      <td>81.1368</td>
      <td>12.9112</td>
      <td>11.6447</td>
      <td>135.599</td>
      <td>8.6842</td>
      <td>269.818</td>
    </tr>
  </tbody>
</table>

Puede preparar los datos para el experimento agregando columnas a `dflow_x` para que sean características para la creación del modelo. Puede definir `dflow_y` para que sea nuestro valor de predicción: costo.

```python
dflow_X = dflow_prepared.keep_columns(['pickup_weekday', 'dropoff_latitude', 'dropoff_longitude','pickup_hour','pickup_longitude','pickup_latitude','passengers'])
dflow_y = dflow_prepared.keep_columns('cost')
```

### <a name="split-data-into-train-and-test-sets"></a>Dividir los datos en conjuntos de entrenamiento y prueba

Ahora puede dividir los datos en conjuntos de entrenamiento y prueba mediante la función `train_test_split` de la biblioteca `sklearn`. Esta función segrega los datos entre el conjunto de datos "x" (características) para el entrenamiento del modelo y el conjunto de datos "y" (valores a predecir) para la realización de pruebas. El parámetro `test_size` determina el porcentaje de datos que se va a asignar a las pruebas. El parámetro `random_state` establece un valor de inicialización para el generador aleatorio, de forma que las divisiones de entrenamiento o prueba son siempre deterministas.

```python
from sklearn.model_selection import train_test_split

x_df = dflow_X.to_pandas_dataframe()
y_df = dflow_y.to_pandas_dataframe()

x_train, x_test, y_train, y_test = train_test_split(x_df, y_df, test_size=0.2, random_state=123)
# flatten y_train to 1d array
y_train.values.flatten()
```

Ahora tiene los paquetes necesarios y los datos listos para el entrenamiento automático del modelo.

## <a name="automatically-train-a-model"></a>Entrenamiento automático de un modelo

Para entrenar automáticamente un modelo:
1. Defina la configuración de la ejecución del experimento
1. Envíe el experimento para la optimización de modelos

### <a name="define-settings-for-autogeneration-and-tuning"></a>Definición de la configuración para la generación automática y la optimización

Defina los parámetros del experimento y la configuración de los modelos para la generación automática y la optimización. Vea la lista completa de [valores](how-to-configure-auto-train.md).


|Propiedad| Valor en este tutorial |DESCRIPCIÓN|
|----|----|---|
|**iteration_timeout_minutes**|10|Límite de tiempo en minutos para cada iteración|
|**iterations**|30|Número de iteraciones. En cada iteración, el modelo se entrena con datos con canalización específica|
|**primary_metric**| spearman_correlation | Métrica que desea optimizar.|
|**preprocess**| True | True permite al experimento realizar el procesamiento previo en la entrada.|
|**verbosity**| logging.INFO | Controla el nivel de registro.|
|**n_cross_validationss**|5|Número de divisiones de validación cruzada



```python
automl_settings = {
    "iteration_timeout_minutes" : 10,
    "iterations" : 30,
    "primary_metric" : 'spearman_correlation',
    "preprocess" : True,
    "verbosity" : logging.INFO,
    "n_cross_validations": 5
}
```


```python
from azureml.train.automl import AutoMLConfig

# local compute
automated_ml_config = AutoMLConfig(task = 'regression',
                             debug_log = 'automated_ml_errors.log',
                             path = project_folder,
                             X = x_train.values,
                             y = y_train.values.flatten(),
                             **automl_settings)
```

### <a name="train-the-automatic-regression-model"></a>Entrenamiento del modelo de regresión automática

Inicie el experimento para ejecutarlo localmente. Pase el objeto `automated_ml_config` definido al experimento y establezca la salida en `true` para ver el progreso durante el experimento.


```python
from azureml.core.experiment import Experiment
experiment=Experiment(ws, experiment_name)
local_run = experiment.submit(automated_ml_config, show_output=True)
```

    Parent Run ID: AutoML_83117da4-07e3-473a-b83e-99471bfa9e09
    *******************************************************************************************
    ITERATION: The iteration being evaluated.
    PIPELINE: A summary description of the pipeline being evaluated.
    DURATION: Time taken for the current iteration.
    METRIC: The result of computing score on the fitted pipeline.
    BEST: The best observed score thus far.
    *******************************************************************************************
    
     ITERATION   PIPELINE                                       DURATION      METRIC      BEST
             0   MaxAbsScaler ExtremeRandomTrees                0:00:21       0.6498    0.6498
             1   MinMaxScaler GradientBoosting                  0:00:22       0.6624    0.6624
             2   StandardScalerWrapper KNN                      0:00:18       0.7267    0.7267
             3   StandardScalerWrapper GradientBoosting         0:00:18       0.5003    0.7267
             4    Ensemble                                      0:00:38       0.6659    0.7267
    

## <a name="explore-the-results"></a>Exploración de los resultados

Explore los resultados del entrenamiento automático con un widget de Jupyter o examinando el historial del experimento.

### <a name="option-1-add-a-jupyter-widget-to-see-results"></a>Opción 1: Agregar un widget de Jupyter para ver los resultados

Si va a usar una instancia de Jupyter Notebook, use el widget de este para ver un grafo y una tabla de todos los resultados.


```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```

![Detalles de la ejecución del widget de Jupyter](./media/tutorial-auto-train-models/jup-widget-auto.png)

### <a name="option-2-get-and-examine-all-run-iterations-in-python"></a>Opción 2: Obtener y examinar todas las iteraciones de ejecución de Python

Como alternativa, puede recuperar el historial de cada experimento y explorar las métricas individuales de cada ejecución de iteración.

```python
children = list(local_run.get_children())
metricslist = {}
for run in children:
    properties = run.get_properties()
    metrics = {k: v for k, v in run.get_metrics().items() if isinstance(v, float)}
    metricslist[int(properties['iteration'])] = metrics

import pandas as pd
rundata = pd.DataFrame(metricslist).sort_index(1)
rundata
```




<div>
<style scoped> .dataframe tbody tr th:only-of-type { vertical-align: middle; }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>explained_variance</th>
      <td>0.113810</td>
      <td>0.093514</td>
      <td>-0.010248</td>
      <td>0.005867</td>
      <td>0.108187</td>
    </tr>
    <tr>
      <th>mean_absolute_error</th>
      <td>7.004893</td>
      <td>6.348354</td>
      <td>6.493000</td>
      <td>7.045597</td>
      <td>6.646850</td>
    </tr>
    <tr>
      <th>median_absolute_error</th>
      <td>4.834063</td>
      <td>3.503244</td>
      <td>3.321553</td>
      <td>4.349547</td>
      <td>4.389995</td>
    </tr>
    <tr>
      <th>normalized_mean_absolute_error</th>
      <td>0.077832</td>
      <td>0.070537</td>
      <td>0.072144</td>
      <td>0.078284</td>
      <td>0.073854</td>
    </tr>
    <tr>
      <th>normalized_median_absolute_error</th>
      <td>0.053712</td>
      <td>0.038925</td>
      <td>0.036906</td>
      <td>0.048328</td>
      <td>0.048778</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_error</th>
      <td>0.117819</td>
      <td>0.120518</td>
      <td>0.126141</td>
      <td>0.124289</td>
      <td>0.118340</td>
    </tr>
    <tr>
      <th>normalized_root_mean_squared_log_error</th>
      <td>0.177689</td>
      <td>0.163360</td>
      <td>0.168101</td>
      <td>0.178250</td>
      <td>0.168685</td>
    </tr>
    <tr>
      <th>r2_score</th>
      <td>0.104661</td>
      <td>0.064075</td>
      <td>-0.036158</td>
      <td>-0.004403</td>
      <td>0.096976</td>
    </tr>
    <tr>
      <th>root_mean_squared_error</th>
      <td>10.603744</td>
      <td>10.846632</td>
      <td>11.352731</td>
      <td>11.185972</td>
      <td>10.650593</td>
    </tr>
    <tr>
      <th>root_mean_squared_log_error</th>
      <td>0.801531</td>
      <td>0.736896</td>
      <td>0.758279</td>
      <td>0.804062</td>
      <td>0.760913</td>
    </tr>
    <tr>
      <th>spearman_correlation</th>
      <td>0.549825</td>
      <td>0.562435</td>
      <td>0.526702</td>
      <td>0.500302</td>
      <td>0.565857</td>
    </tr>
    <tr>
      <th>spearman_correlation_max</th>
      <td>0.549825</td>
      <td>0.562435</td>
      <td>0.562435</td>
      <td>0.562435</td>
      <td>0.565857</td>
    </tr>
  </tbody>
</table>
</div>

## <a name="retrieve-the-best-model"></a>Recuperación del mejor modelo

Seleccione la mejor canalización en nuestras iteraciones. El método `get_output` en `automl_classifier` devuelve la mejor ejecución y el modelo ajustado de la última invocación de ajuste. Hay sobrecargas en `get_output` que le permiten recuperar la mejor ejecución y el modelo ajustado para cualquier lógica registrada o para una iteración concreta.

```python
best_run, fitted_model = local_run.get_output()
print(best_run)
print(fitted_model)
```

## <a name="register-the-model"></a>Registro del modelo

Registre el modelo en un área de trabajo del servicio Azure Machine Learning.


```python
description = 'Automated Machine Learning Model'
tags = None
local_run.register_model(description=description, tags=tags)
local_run.model_id # Use this id to deploy the model as a web service in Azure
```

## <a name="test-the-best-model-accuracy"></a>Prueba de la precisión del mejor modelo

Use el mejor modelo para ejecutar predicciones en el conjunto de datos de prueba. La función `predict` usa el mejor modelo y predice los valores de "y" (precio del recorrido) a partir del conjunto de datos `x_test`. Imprima los 10 primeros valores de costo predichos a partir de `y_predict`.

```python
y_predict = fitted_model.predict(x_test.values)
print(y_predict[:10])
```

Compare los valores de costo predichos con los valores reales. Use la trama de datos `y_test` y conviértala en una lista para compararla con los valores predichos. La función `mean_absolute_error` toma dos matrices de valores y calcula el error medio absoluto entre ellos. En este ejemplo, un error medio absoluto de 3,5 significaría que como promedio, el modelo predice el costo con un margen de error de más/menos 3,5 comparado con el valor real.

```python
from sklearn.metrics import mean_absolute_error

y_actual = y_test.values.flatten().tolist()
mean_absolute_error(y_actual, y_predict)
```

    [ 3.16213051 ]

Ejecute el siguiente código para calcular el valor del MAPE (error medio absoluto porcentual) usando los conjuntos de datos `y_actual` y `y_predict` completos. Esta métrica calcula una diferencia absoluta entre cada valor predicho y cada valor real, suma todas las diferencias y, posteriormente, expresa esa suma como un porcentaje del total de los valores reales.

```python
sum_actuals = sum_errors = 0

for actual_val, predict_val in zip(y_actual, y_predict):
    abs_error = actual_val - predict_val
    if abs_error < 0:
        abs_error = abs_error * -1

    sum_errors = sum_errors + abs_error
    sum_actuals = sum_actuals + actual_val

mean_abs_percent_error = sum_errors / sum_actuals
print("Model MAPE:")
print(mean_abs_percent_error)
print()
print("Model Accuracy:")
print(1 - mean_abs_percent_error)
```

    Model MAPE:
    0.22424976634422172

    Model Accuracy:
    0.7757502336557782

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial de aprendizaje automático, ha:

> [!div class="checklist"]
> * Configurado un área de trabajo y preparado datos para un experimento
> * Realizado un entrenamiento mediante un modelo de regresión automatizado localmente con parámetros personalizados
> * Explorado y revisado los resultados del entrenamiento
> * Registrado el mejor modelo

[Implemente el modelo](tutorial-deploy-models-with-aml.md) con Azure Machine Learning.
