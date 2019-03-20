---
title: Auto-entrenar un modelo de previsión de series temporales
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el servicio Azure Machine Learning para entrenar una modelo de regresión de predicción utilizando había automatizada de aprendizaje automático de serie temporal.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.service: machine-learning
ms.subservice: core
ms.reviewer: trbye
ms.topic: conceptual
ms.date: 03/19/2019
ms.openlocfilehash: cc5aae0e46e181e8063a4e01a832e68eab0eae0e
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226623"
---
# <a name="auto-train-a-time-series-forecast-model"></a>Auto-entrenar un modelo de previsión de series temporales

En este artículo, aprenderá a entrenar un modelo de regresión pronóstico de series temporales con automatizada de machine learning en el servicio de Azure Machine Learning. Configuración de un modelo de pronóstico es similar a la configuración de un modelo de regresión estándar mediante el aprendizaje automático automatizadas, pero existen algunos pasos de preprocesamiento y opciones de configuración para trabajar con datos de serie temporal. Los ejemplos siguientes muestran cómo para:

* Preparar los datos para el modelado de series temporales
* Configure los parámetros de serie temporal específicos en un [ `AutoMLConfig` ](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py) objeto
* Ejecutar predicciones con los datos de serie temporal

## <a name="prerequisites"></a>Requisitos previos

* Un área de trabajo de Azure Machine Learning. Para crear el área de trabajo, consulte [Introducción a Azure Machine Learning Service](quickstart-get-started.md).
* En este artículo se supone una familiarización básica con la configuración de una experimento de aprendizaje de automático automatizada. Siga el [tutorial](tutorial-auto-train-models.md) o [procedimientos](how-to-configure-auto-train.md) para ver la máquina automatizada básica patrones de diseño de experimento de aprendizaje.

## <a name="preparing-data"></a>Preparación de datos

La diferencia más importante entre un tipo de tarea de regresión y regresión previsión tipo de tarea automatizada machine Learning está incluida una característica en los datos que representa una serie de tiempo válido. Una serie de tiempo regular tiene una frecuencia coherente y bien definida y tiene un valor en cada etapa de ejemplo de un intervalo de tiempo continuo. Tenga en cuenta la siguiente instantánea de un archivo `sample.csv`.

    week_starting,store,sales_quantity,week_of_year
    9/3/2018,A,2000,36
    9/3/2018,B,600,36
    9/10/2018,A,2300,37
    9/10/2018,B,550,37
    9/17/2018,A,2100,38
    9/17/2018,B,650,38
    9/24/2018,A,2400,39
    9/24/2018,B,700,39
    10/1/2018,A,2450,40
    10/1/2018,B,650,40

Este conjunto de datos es un ejemplo sencillo de los datos de ventas semanales de una empresa que tiene dos almacenes diferentes, A y B. Asimismo, hay una característica para `week_of_year` que permitirá que el modelo detectar la estacionalidad semanal. El campo `week_starting` representa una serie temporal limpia con frecuencia semanal y el campo `sales_quantity` es la columna de destino para ejecutar predicciones. Leer los datos en una trama de datos de Pandas y, después, usar el `to_datetime` función para asegurarse de la serie temporal es un `datetime` tipo.

```python
import pandas as pd
data = pd.read_csv("sample.csv")
data["week_starting"] = pd.to_datetime(data["week_starting"])
```

En este caso los datos ya están ordenados ascendente por el campo de hora `week_starting`. Sin embargo, al configurar un experimento, asegúrese de que la columna de tiempo deseado está ordenada en orden ascendente para generar una serie de tiempo válido. Suponga que los datos contienen 1000 registros y realizar una división determinista en los datos para crear el entrenamiento y conjuntos de datos de prueba. A continuación, separar el campo de destino `sales_quantity` crear la predicción entrenamiento y prueba establece.

```python
X_train = data.iloc[:950]
X_test = data.iloc[-50:]

y_train = X_train.pop("sales_quantity").values
y_test = X_test.pop("sales_quantity").values
```

> [!NOTE]
> Cuando se entrena un modelo para predecir valores futuros, asegúrese de todas las características que se usan para el entrenamiento se pueden usar al ejecutar predicciones para su horizonte previsto. Por ejemplo, al crear una previsión de demanda, incluida una característica para la cotización actual podría masivo aumentar la precisión del entrenamiento. Sin embargo, si piensa previsión con un horizonte largo, no es posible que pueda predecir con precisión los valores de acciones futuras correspondientes a los puntos de series temporales futuros y precisión del modelo podría verse afectado.

## <a name="configure-experiment"></a>Configuración del experimento

Para la previsión de las tareas automatizadas machine learning usa procesamiento previo y la estimación de los pasos que son específicos de los datos de serie temporal. Los siguientes pasos previos al procesamiento se ejecutará:

* Detectar la serie temporal muestra la frecuencia (por ejemplo, cada hora, diariamente, semanalmente) y crea nuevos registros de ausencia de puntos en el tiempo para realizar la serie continua.
* Imputar valores que faltan en las columnas de característica (con valores de columna medio) y el destino (a través de relleno de reenvío)
* Crear características basadas en el nivel de detalle para habilitar efectos fijos en las diferentes series
* Crear características basadas en tiempo para ayudar a los patrones estacionales de aprendizaje
* Codificar las variables de categorías cantidades numéricas

La `AutoMLConfig` objeto define la configuración y los datos necesarios para una tarea de aprendizaje de automático automatizadas. Al igual que un problema de regresión, defina los parámetros de entrenamiento estándar como tipo de tarea, el número de iteraciones, datos de entrenamiento y el número de validaciones de entre. Para la previsión de las tareas, hay parámetros adicionales que se deben establecer que afectan el experimento. La siguiente tabla explica cada parámetro y su uso.

| Parámetro | DESCRIPCIÓN | Obligatorio |
|-------|-------|-------|
|`time_column_name`|Se utiliza para especificar la columna de fecha y hora en los datos de entrada que se utiliza para compilar la serie temporal y deducir su frecuencia.|✓|
|`grain_column_names`|Nombres de definición de grupos de series individuales en los datos de entrada. Si no se define el nivel de detalle, el conjunto de datos se supone que una serie temporal.||
|`max_horizon`|Máximo había deseado horizonte de previsión en unidades de frecuencia de serie temporal.|✓|

Cree la configuración de serie temporal como un objeto de diccionario. Establecer el `time_column_name` a la `week_starting` campo del conjunto de datos. Definir la `grain_column_names` parámetro para asegurarse de que **dos diferentes grupos de series temporales** se crean para nuestros datos; uno para la tienda A y B., por último, establezca el `max_horizon` a 50 con el fin de predecir de toda la prueba establecido.

```python
time_series_settings = {
    "time_column_name": "week_starting",
    "grain_column_names": ["store"],
    "max_horizon": 50
}
```

Ahora cree un estándar `AutoMLConfig` objeto, especificando el `forecasting` tipo de tarea y envío del experimento. Al finalizar el modelo, recupere la iteración de ejecución mejor.

```python
from azureml.core.workspace import Workspace
from azureml.core.experiment import Experiment
from azureml.train.automl import AutoMLConfig
import logging

automl_config = AutoMLConfig(task='forecasting',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             X=X_train,
                             y=y_train,
                             n_cross_validations=5,
                             enable_ensembling=False,
                             verbosity=logging.INFO,
                             **time_series_settings)

ws = Workspace.from_config()
experiment = Experiment(ws, "forecasting_example")
local_run = experiment.submit(automl_config, show_output=True)
best_run, fitted_model = local_run.get_output()
```

> [!NOTE]
> Para el procedimiento de validación cruzada (VC), datos de serie temporal pueden infringir las suposiciones estadísticas básicas de la estrategia de validación cruzada K doblado canónica, por lo que el aprendizaje automático automatizadas implementa un procedimiento de validación de origen gradual para crear subconjuntos de validación cruzada para datos de serie temporal. Para usar este procedimiento, especifique el `n_cross_validations` parámetro en el `AutoMLConfig` objeto. Puede omitir la validación y el uso de conjuntos de su propia validación con el `X_valid` y `y_valid` parámetros.

## <a name="forecasting-with-best-model"></a>Con el mejor modelo de previsión

Usar la mejor iteración del modelo para predecir valores para el conjunto de datos de prueba.

```python
y_predict = fitted_model.predict(X_test)
y_actual = y_test.flatten()
```

Calcular RMSE (error cuadrático) entre el `y_test` valores reales y los valores pronosticados en `y_pred`.

```python
from sklearn.metrics import mean_squared_error
from math import sqrt

rmse = sqrt(mean_squared_error(y_actual, y_predict))
rmse
```

Ahora que el global se ha determinado la precisión del modelo, el siguiente paso más realista es utilizar el modelo para predecir valores futuros desconocidos. Solo tiene que proporcionar un conjunto de datos en el mismo formato que el conjunto de pruebas `X_test` pero con fechas futuras y la predicción resultante conjunto es los valores pronosticados para cada paso de la serie temporal. Suponga eran los últimos registros de serie temporal en el conjunto de datos para el inicio de la semana 31/12/2018. Para prever la demanda de la semana siguiente (o períodos de tantos como necesite para realizar la previsión, < = `max_horizon`), crear un único registro serie para cada almacén de tiempo para el inicio de la semana del 01/07/2019.

    week_starting,store,week_of_year
    01/07/2019,A,2
    01/07/2019,A,2

Repita los pasos necesarios para cargar estos datos a una trama de datos futuros y, a continuación, ejecute `best_run.predict(X_test)` para predecir valores futuros.

> [!NOTE]
> No se puede predecir valores para el número de períodos de mayores que el `max_horizon`. El modelo debe volver a entrenado con un horizonte de mayor tamaño para predecir valores futuros más allá del horizonte actual.

## <a name="next-steps"></a>Pasos siguientes

* Siga el [tutorial](tutorial-auto-train-models.md) aprender a crear experimentos con automated aprendizaje automático.
* Ver el [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk) documentación de referencia.
