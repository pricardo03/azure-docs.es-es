---
title: Creación de experimentos de ML automatizados
titleSuffix: Azure Machine Learning service
description: El aprendizaje automático automatizado elige un algoritmo y genera un modelo listo para la implementación. Obtenga información sobre las opciones que puede usar para configurar experimentos de aprendizaje automático automatizado.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 07/10/2019
ms.custom: seodec18
ms.openlocfilehash: 6e29e0f89d9270a143d48cf6e85b479813e19d9d
ms.sourcegitcommit: fe50db9c686d14eec75819f52a8e8d30d8ea725b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2019
ms.locfileid: "69013640"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Configuración de experimentos de ML automatizado en Python

En esta guía, aprenderá a definir diversos valores de configuración de los experimentos de aprendizaje automático automatizado con el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py). El aprendizaje automático automatizado elige un algoritmo e hiperparámetros, y genera un modelo listo para la implementación. Se pueden usar varias opciones para configurar experimentos de aprendizaje automático automatizado.

Para ver ejemplos de experimentos de aprendizaje automático automatizado, vea [Tutorial: Train a classification model with automated machine learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento de un modelo de clasificación con aprendizaje automático automatizado) o [Entrenamiento de modelos con aprendizaje automático automatizado en la nube mediante Azure Machine Learning](how-to-auto-train-remote.md).

Opciones de configuración disponibles en el aprendizaje automático automatizado:

* Selección del tipo de experimento: Clasificación, regresión o predicción de las series temporales
* Origen de datos, formatos y datos de captura
* Elección el destino del proceso: local o remoto
* Configuración de experimentos de aprendizaje automático automatizado
* Ejecución de un experimento de aprendizaje automático automatizado
* Explorar las métricas del modelo
* Registrar e implementar el modelo

Si prefiere una experiencia sin código, también puede [crear experimentos de aprendizaje automático automatizado en Azure Portal](how-to-create-portal-experiments.md).

## <a name="select-your-experiment-type"></a>Seleccione el tipo de experimento

Antes de comenzar el experimento, debe determinar el tipo de problema de aprendizaje automático que va a resolver. El aprendizaje automático automatizado admite tipos de tareas de clasificación, regresión y previsión.

El aprendizaje automático automatizado admite los siguientes algoritmos durante el proceso de optimización y automatización. Como usuario, no hay ninguna necesidad de especificar el algoritmo.

clasificación | Regresión | Previsión de series temporales
|-- |-- |--
[Regresión logística](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Red elástica](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Red elástica](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#regression)|[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#regression)
[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[SVC lineal](https://scikit-learn.org/stable/modules/svm.html#classification)|[Lazo LARS](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[Lazo LARS](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[Clasificación de vectores de soporte de C (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[Clasificador DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[Regresor DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [Regresor DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[Clasificador lineal DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Regresión lineal](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Regresión lineal](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Bayes naive](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|
[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|

Use el parámetro `task` en el constructor `AutoMLConfig` para especificar el tipo de experimento.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task="classification")
```

## <a name="data-source-and-format"></a>Formato y origen de datos
El aprendizaje automático automatizado es compatible con los datos que residen en el escritorio local o en la nube, como Azure Blob Storage. Los datos pueden leerse en los formatos de datos compatibles con scikit-learn. Puede leer los datos en:
* Matrices de Numpy X (características) e y (variable de destino o también denominada etiqueta)
* Dataframe de Pandas

>[!Important]
> Requisitos para los datos de entrenamiento:
>* Los datos deben estar en formato tabular.
>* El valor que quiere predecir (columna de destino) debe estar presente en los datos.

Ejemplos:

*   Matrices de Numpy

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data
    y_digits = digits.target
    ```

*   Dataframe de Pandas

    ```python
    import pandas as pd
    from sklearn.model_selection import train_test_split
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    y = df["Label"]
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Capturar datos para ejecutar un experimento en el proceso remoto

En el caso de las ejecuciones remotas, debe hacer que los datos sean accesibles desde el proceso remoto. Para hacerlo, cargue los datos en el almacén de datos.

A continuación se muestra un ejemplo del uso de `datastore`:

```python
    import pandas as pd
    from sklearn import datasets

    data_train = datasets.load_digits()

    pd.DataFrame(data_train.data[100:,:]).to_csv("data/X_train.csv", index=False)
    pd.DataFrame(data_train.target[100:]).to_csv("data/y_train.csv", index=False)

    ds = ws.get_default_datastore()
    ds.upload(src_dir='./data', target_path='digitsdata', overwrite=True, show_progress=True)
```

### <a name="define-dprep-references"></a>Definición de las referencias de dprep

Defina X e y como referencia de dprep, que se pasará a un objeto automatizado `AutoMLConfig` de aprendizaje automático similar al siguiente:

```python

    X = dprep.auto_read_file(path=ds.path('digitsdata/X_train.csv'))
    y = dprep.auto_read_file(path=ds.path('digitsdata/y_train.csv'))


    automl_config = AutoMLConfig(task = 'classification',
                                 debug_log = 'automl_errors.log',
                                 path = project_folder,
                                 run_configuration=conda_run_config,
                                 X = X,
                                 y = y,
                                 **automl_settings
                                )
```

## <a name="train-and-validation-data"></a>Datos de entrenamiento y validación

Puede especificar un conjunto distinto de entrenamiento y validación en el método `AutoMLConfig`.

### <a name="k-folds-cross-validation"></a>Validación cruzada de K iteraciones

Use la opción de configuración `n_cross_validations` para especificar el número de validaciones cruzadas. El conjunto de datos de entrenamiento se dividirá aleatoriamente en `n_cross_validations` iteraciones del mismo tamaño. Durante cada ronda de validación cruzada, una de las iteraciones se usará para la validación del modelo entrenado en las iteraciones restantes. Este proceso se repite durante `n_cross_validations` rondas hasta que cada una se utiliza una vez como conjunto de validación. Se notifican las puntuaciones promedio de todas las rondas de `n_cross_validations` y el modelo correspondiente se puede volver a entrenar en todo el conjunto de datos de entrenamiento.

### <a name="monte-carlo-cross-validation-repeated-random-sub-sampling"></a>Validación de Monte Carlo entre (submuestra aleatoria repetida)

Use `validation_size` para especificar el porcentaje del conjunto de datos de entrenamiento que se debe usar para la validación y use `n_cross_validations` para especificar el número de validaciones cruzadas. Durante cada ronda de validación cruzada, un subconjunto de tamaño `validation_size` se seleccionará aleatoriamente para la validación del modelo entrenado en los datos restantes. Por último, se notifican las puntuaciones promedias de todas las rondas `n_cross_validations` y el modelo correspondiente se puede volver a entrenar en todo el conjunto de datos de entrenamiento. No se admite Monte Carlo para la predicción de series temporales.

### <a name="custom-validation-dataset"></a>Conjunto de datos de validación personalizado

Use un conjunto de datos de validación personalizado si la división aleatoria no es aceptable, normalmente, datos de serie temporal o datos desequilibrados. Puede especificar un conjunto de datos de validación propio. El modelo se evaluará en el conjunto de datos de validación especificado en lugar de un conjunto de datos aleatorio.

## <a name="compute-to-run-experiment"></a>Proceso para ejecutar el experimento

A continuación, determine dónde se va a entrenar el modelo. Un experimento de entrenamiento de aprendizaje automático automatizado se puede ejecutar en las opciones de proceso siguientes:
*   La máquina local, como un escritorio local o un equipo portátil: generalmente, cuando haya un pequeño conjunto de datos y siga en la fase de exploración.
*   Una máquina remota en la nube: [Azure Machine Learning Managed Compute](concept-compute-target.md#amlcompute) es un servicio administrado que permite entrenar modelos de aprendizaje automático en clústeres de máquinas virtuales de Azure.

Visite el [sitio de GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) para obtener cuadernos de ejemplo con destinos de proceso locales y remotos.

*   Un clúster de Azure Databricks en su suscripción de Azure. Puede encontrar más detalles aquí: [Configuración del clúster de Azure Databricks para ML automatizado](how-to-configure-environment.md#azure-databricks)

Consulte el [sitio de GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) para ver cuadernos de ejemplo con Azure Databricks.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Establecer la configuración de experimento

Se pueden usar varias opciones para configurar el experimento de aprendizaje automático automatizado. Estos parámetros se establecen al crear una instancia un objeto `AutoMLConfig`. Consulte la [clase AutoMLConfig](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py) para obtener una lista completa de parámetros.

Estos son algunos ejemplos:

1.  Experimento de clasificación mediante AUC ponderado como la métrica principal con un tiempo máximo de 12 000 segundos por cada iteración. El experimento finaliza después de 50 iteraciones y 2 iteraciones de validación cruzada.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        blacklist_models='XGBoostClassifier',
        X=X,
        y=y,
        n_cross_validations=2)
    ```
2.  A continuación se incluye un ejemplo de un conjunto de experimentos de regresión que finaliza después de 100 iteraciones. Cada iteración tiene una duración de hasta 600 segundos con 5 iteraciones de validación cruzada.

    ```python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        whitelist_models='kNN regressor'
        primary_metric='r2_score',
        X=X,
        y=y,
        n_cross_validations=5)
    ```

Los tres valores de parámetro `task` diferentes determinan la lista de algoritmos que se aplicará.  Use los parámetros `whitelist` o `blacklist` para modificar aún más las iteraciones con los algoritmos disponibles para incluir o excluir. Encontrará la lista de los modelos admitidos en la [clase SupportedAlgorithms](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.constants.supportedalgorithms?view=azure-ml-py).

### <a name="primary-metric"></a>Métrica principal
La métrica principal, como se muestra en los ejemplos anteriores, determina la métrica que se utilizará durante el entrenamiento del modelo para la optimización. La métrica principal que puede seleccionar viene determinada por el tipo de tarea que elija. A continuación se muestra una lista de las métricas disponibles.

|clasificación | Regresión | Previsión de series temporales
|-- |-- |--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

### <a name="data-preprocessing--featurization"></a>Caracterización y preprocesamiento de datos

En cada experimento de aprendizaje automático automatizado, los datos se [escalan y se normalizan automáticamente](concept-automated-ml.md#preprocess) para ayudar a que los algoritmos funcionen bien.  Sin embargo, también puede habilitar el preprocesamiento o la caracterización adicionales, como la atribución de valores que faltan, la codificación y las transformaciones. [Más información sobre qué caracterización se incluye](how-to-create-portal-experiments.md#preprocess).

Para habilitar la caracterización, especifique `"preprocess": True` para la clase [`AutoMLConfig` ](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

> [!NOTE]
> Los pasos previos al procesamiento del aprendizaje automático (normalización de características, control de los datos que faltan, conversión de valores de texto a numéricos, etc.) se convierten en parte del modelo subyacente. Cuando se utiliza el modelo para las predicciones, se aplican automáticamente a los datos de entrada los mismos pasos previos al procesamiento que se aplican durante el entrenamiento.

### <a name="time-series-forecasting"></a>Previsión de series temporales
Para el tipo de tarea de previsión de series temporales, tiene que definir parámetros adicionales.
1. time_column_name: este es un parámetro obligatorio que define el nombre de la columna de los datos de entrenamiento que contienen las series de fecha/hora.
1. max_horizon: define la duración del tiempo que desea predecir en función de la periodicidad de los datos de entrenamiento. Por ejemplo si tiene datos de entrenamiento con intervalos de agregación diaria, defina la distancia en días para la que desea que se entrene el modelo.
1. grain_column_names: este parámetro define el nombre de las columnas que contienen datos de series temporales individuales en los datos de entrenamiento. Por ejemplo, si está previendo las ventas de una marca determinada por tienda, definiría las columnas de la tienda y de la marca como sus columnas de agregación.

Vea el ejemplo de la configuración que se utiliza a continuación, el ejemplo del cuaderno está disponible [aquí](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb).

```python
# Setting Store and Brand as grains for training.
grain_column_names = ['Store', 'Brand']
nseries = data.groupby(grain_column_names).ngroups

# View the number of time series data with defined grains
print('Data contains {0} individual time-series.'.format(nseries))
```

```python
time_series_settings = {
    'time_column_name': time_column_name,
    'grain_column_names': grain_column_names,
    'drop_column_names': ['logQuantity'],
    'max_horizon': n_test_periods
}

automl_config = AutoMLConfig(task='forecasting',
                             debug_log='automl_oj_sales_errors.log',
                             primary_metric='normalized_root_mean_squared_error',
                             iterations=10,
                             X=X_train,
                             y=y_train,
                             n_cross_validations=5,
                             path=project_folder,
                             verbosity=logging.INFO,
                             **time_series_settings)
```

### <a name="ensemble"></a> Configuración de conjuntos

Los modelos de conjunto están habilitados de forma predeterminada y aparecen como las iteraciones de ejecución finales en una ejecución automatizada de aprendizaje automático. Los métodos de conjunto admitidos actualmente son votación y apilamiento. La votación se implementa como un voto blando mediante promedios ponderados y la implementación de apilamiento usa una implementación de dos niveles, donde el primer nivel tiene los mismos modelos que el conjunto de votación y el segundo modelo de nivel se usa para encontrar la combinación óptima de los modelos del primer nivel. Si usa modelos de ONNX **o** tiene habilitada la explicación del modelo, el apilamiento se deshabilitará y solo se utilizará la votación.

Hay varios argumentos predeterminados que se pueden proporcionar como `kwargs` en un objeto `AutoMLConfig` para modificar el comportamiento predeterminado del conjunto de pila.

* `stack_meta_learner_type`: el metaaprendizaje es un modelo entrenado en la salida de los modelos heterogéneos individuales. Los metaaprendizajes predeterminados son `LogisticRegression` para las tareas de clasificación (o `LogisticRegressionCV` si está habilitada la validación cruzada) y `ElasticNet` para las tareas de regresión y predicción (o `ElasticNetCV` si está habilitada la validación cruzada). Este parámetro puede ser una de las cadenas siguientes: `LogisticRegression`, `LogisticRegressionCV`, `LightGBMClassifier`, `ElasticNet`, `ElasticNetCV`, `LightGBMRegressor` o `LinearRegression`.
* `stack_meta_learner_train_percentage`: especifica la proporción del conjunto de entrenamiento (al elegir entrenar y el tipo de validación de entrenamiento) que se va a reservar para entrenar el metaaprendizaje. El valor predeterminado es `0.2`.
* `stack_meta_learner_kwargs`: parámetros opcionales que se van a pasar al inicializador del metaaprendizaje. Estos parámetros y tipos de parámetro reflejan los del constructor del modelo correspondiente y se reenvían a dicho constructor.

En el código siguiente se muestra un ejemplo de cómo especificar el comportamiento del conjunto personalizado en un objeto `AutoMLConfig`.

```python
ensemble_settings = {
    "stack_meta_learner_type": "LogisticRegressionCV",
    "stack_meta_learner_train_percentage": 0.3,
    "stack_meta_learner_kwargs": {
        "refit": True,
        "fit_intercept": False,
        "class_weight": "balanced",
        "multi_class": "auto",
        "n_jobs": -1
    }
}

automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        iterations=20,
        X=X_train,
        y=y_train,
        n_cross_validations=5,
        **ensemble_settings
        )
```

El entrenamiento de conjunto está habilitado de forma predeterminada, pero se puede deshabilitar mediante los parámetros booleanos `enable_voting_ensemble` y `enable_stack_ensemble`.

```python
automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        iterations=20,
        X=X_train,
        y=y_train,
        n_cross_validations=5,
        enable_voting_ensemble=False,
        enable_stack_ensemble=False
        )
```

## <a name="run-experiment"></a>Ejecutar experimento

Para una instancia automatizada de Machine Learning, se crea un objeto `Experiment`, que es un objeto con nombre en un objeto `Workspace` que se usa para ejecutar experimentos.

```python
from azureml.core.experiment import Experiment

ws = Workspace.from_config()

# Choose a name for the experiment and specify the project folder.
experiment_name = 'automl-classification'
project_folder = './sample_projects/automl-classification'

experiment = Experiment(ws, experiment_name)
```

Envíe el experimento para ejecutar y generar un modelo. Pase `AutoMLConfig` al método `submit` para generar el modelo.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Las dependencias se instalan por primera vez en una máquina nueva.  El resultado puede tardar hasta 10 minutos en mostrarse.
>Establecer `show_output` en `True` genera un resultado que se muestra en la consola.

### <a name="exit-criteria"></a>Criterios de salida
Hay unas cuantas opciones puede definir para completar el experimento.
1. Sin criterios: si no se define ninguna parámetro de salida, el experimento continuará hasta que no se realice ningún progreso adicional en la métrica principal.
1. Número de iteraciones: defina el número de iteraciones que ejecutará el experimento. Puede agregar opcionalmente iteration_timeout_minutes para definir un límite de tiempo en minutos por cada iteración.
1. Salir después de un periodo de tiempo: con experiment_timeout_minutes en la configuración puede definir cuánto tiempo en minutos debe continuar un experimento en ejecución.
1. Salir una vez alcanzada una puntuación: con experiment_exit_score puede elegir completar el experimento una vez alcanzada una puntuación en función de la métrica principal.

### <a name="explore-model-metrics"></a>Explorar las métricas del modelo

Puede ver los resultados del entrenamiento en un widget o en línea si se encuentra en un bloc de notas. Vea [Seguimiento y evaluación de modelos](how-to-track-experiments.md#view-run-details) para obtener más información.

## <a name="understand-automated-ml-models"></a>Descripción de los modelos de ML automatizado

Cualquier modelo generado mediante ML automatizado incluye los siguientes pasos:
+ Ingeniería de características automatizada (si preprocess=True)
+ Escalado o normalización y algoritmo con valores de hiperparámetro

Se obtiene de manera clara esta información a partir de la salida del modelo fitted_model de ML automatizado.

```python
automl_config = AutoMLConfig(…)
automl_run = experiment.submit(automl_config …)
best_run, fitted_model = automl_run.get_output()
```

### <a name="automated-feature-engineering"></a>Diseño de características automatizadas

Consulte la lista de preprocesamiento y del [diseño de características automatizadas](concept-automated-ml.md#preprocess) que ocurre cuando preprocess=True.

En este ejemplo:
+ Hay cuatro características de entrada: A (numérico), B (numérico), C (numérico), D (fecha y hora)
+ La característica numérica C se puede quitar porque es una columna de identificador con todos los valores únicos
+ Las características numéricas A y B tienen valores que faltan y, por tanto, se atribuyan como promedio
+ La característica de fecha y hora D se divide en 11 diferentes características de diseño

Utilice estas dos API en el primer paso del modelo ajustado para más información.  Consulte [este cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand).

+ API 1: `get_engineered_feature_names()` devuelve una lista de nombres de características diseñadas.

  Uso:
  ```python
  fitted_model.named_steps['timeseriestransformer']. get_engineered_feature_names ()
  ```

  ```
  Output: ['A', 'B', 'A_WASNULL', 'B_WASNULL', 'year', 'half', 'quarter', 'month', 'day', 'hour', 'am_pm', 'hour12', 'wday', 'qday', 'week']
  ```

  En esta lista se incluyen todos los nombres de las características de diseño.

  >[!Note]
  >Utilice 'timeseriestransformer' para la tarea = "forecasting"; en caso contrario, utilice 'datatransformer' para la tarea "regression" o "classification".

+ API 2: `get_featurization_summary()` devuelve un resumen de la caracterización para todas las características de entrada.

  Uso:
  ```python
  fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
  ```

  >[!Note]
  >Utilice 'timeseriestransformer' para la tarea = "forecasting"; en caso contrario, utilice 'datatransformer' para la tarea "regression" o "classification".

  Salida:
  ```
  [{'RawFeatureName': 'A',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'B',
    'TypeDetected': 'Numeric',
    'Dropped': 'No',
    'EngineeredFeatureCount': 2,
    'Tranformations': ['MeanImputer', 'ImputationMarker']},
   {'RawFeatureName': 'C',
    'TypeDetected': 'Numeric',
    'Dropped': 'Yes',
    'EngineeredFeatureCount': 0,
    'Tranformations': []},
   {'RawFeatureName': 'D',
    'TypeDetected': 'DateTime',
    'Dropped': 'No',
    'EngineeredFeatureCount': 11,
    'Tranformations': ['DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime','DateTime']}]
  ```

   Donde:

   |Output|Definición|
   |----|--------|
   |RawFeatureName|Nombre de columna o característica de entrada desde el conjunto de datos proporcionado.|
   |TypeDetected|Tipo de datos detectado de la característica de entrada.|
   |Dropped|Indica si la característica de entrada se ha quitado o se ha utilizado.|
   |EngineeringFeatureCount|Número de características que se generan por las transformaciones de diseño de características automatizadas.|
   |Transformaciones|Lista de transformaciones aplicadas a las características de entrada para generar características de diseño.|

### <a name="scalingnormalization-and-algorithm-with-hypermeter-values"></a>Escalado o normalización y algoritmo con valores de hiperparámetro:

Para entender el escalado o normalización y los valores de algoritmos o hiperparámetros de una canalización, utilice fitted_model.steps. [Más información sobre el escalado o normalización](concept-automated-ml.md#preprocess). Este es una salida de ejemplo:

```
[('RobustScaler', RobustScaler(copy=True, quantile_range=[10, 90], with_centering=True, with_scaling=True)), ('LogisticRegression', LogisticRegression(C=0.18420699693267145, class_weight='balanced', dual=False, fit_intercept=True, intercept_scaling=1, max_iter=100, multi_class='multinomial', n_jobs=1, penalty='l2', random_state=None, solver='newton-cg', tol=0.0001, verbose=0, warm_start=False))
```

Para obtener más información, use esta función auxiliar que se muestra en [este cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification/auto-ml-classification.ipynb).

```python
from pprint import pprint


def print_model(model, prefix=""):
    for step in model.steps:
        print(prefix + step[0])
        if hasattr(step[1], 'estimators') and hasattr(step[1], 'weights'):
            pprint({'estimators': list(
                e[0] for e in step[1].estimators), 'weights': step[1].weights})
            print()
            for estimator in step[1].estimators:
                print_model(estimator[1], estimator[0] + ' - ')
        else:
            pprint(step[1].get_params())
            print()


print_model(fitted_model)
```

Lo siguiente es una salida de ejemplo de una canalización mediante un algoritmo concreto (LogisticRegression con RobustScalar, en este caso).

```
RobustScaler
{'copy': True,
'quantile_range': [10, 90],
'with_centering': True,
'with_scaling': True}

LogisticRegression
{'C': 0.18420699693267145,
'class_weight': 'balanced',
'dual': False,
'fit_intercept': True,
'intercept_scaling': 1,
'max_iter': 100,
'multi_class': 'multinomial',
'n_jobs': 1,
'penalty': 'l2',
'random_state': None,
'solver': 'newton-cg',
'tol': 0.0001,
'verbose': 0,
'warm_start': False}
```

<a name="explain"></a>

## <a name="explain-the-model-interpretability"></a>Explicación del modelo (interoperabilidad)

El aprendizaje automático automatizado permite comprender la importancia de las características.  Durante el proceso de entrenamiento, puede obtener la importancia de características global para el modelo.  Para escenarios de clasificación, también puede obtener importancia de características de nivel de clase.  Debe proporcionar un conjunto de datos de validación (X_valid) para obtener la importancia de características.

Hay dos maneras de generar la importancia de características.

*   Una vez completado un experimento, puede usar el método `explain_model` en cualquier iteración.

    ```python
    from azureml.train.automl.automlexplainer import explain_model

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        explain_model(fitted_model, X_train, X_test)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

*   Para ver la importancia de características para todas las iteraciones, establezca la marca `model_explainability` en `True` en AutoMLConfig.

    ```python
    automl_config = AutoMLConfig(task = 'classification',
                                 debug_log = 'automl_errors.log',
                                 primary_metric = 'AUC_weighted',
                                 max_time_sec = 12000,
                                 iterations = 10,
                                 verbosity = logging.INFO,
                                 X = X_train,
                                 y = y_train,
                                 X_valid = X_test,
                                 y_valid = y_test,
                                 model_explainability=True,
                                 path=project_folder)
    ```

    Después, puede usar el método retrieve_model_explanation para recuperar la importancia de características para una iteración concreta.

    ```python
    from azureml.train.automl.automlexplainer import retrieve_model_explanation

    shap_values, expected_values, overall_summary, overall_imp, per_class_summary, per_class_imp = \
        retrieve_model_explanation(best_run)

    #Overall feature importance
    print(overall_imp)
    print(overall_summary)

    #Class-level feature importance
    print(per_class_imp)
    print(per_class_summary)
    ```

Puede visualizar el gráfico de importancia de características en el área de trabajo de Azure Portal. Muestre la dirección URL mediante el objeto de ejecución:

```
automl_run.get_portal_url()
```

Puede visualizar el gráfico de importancia de características en el área de trabajo de Azure Portal. El gráfico también se muestra al usar el [widget de Jupyter](https://docs.microsoft.com/python/api/azureml-widgets/azureml.widgets?view=azure-ml-py) `RunDetails` en un cuaderno. Para más información sobre los gráficos, consulte [Descripción de los resultados del aprendizaje automático automatizado](how-to-understand-automated-ml.md).

```Python
from azureml.widgets import RunDetails
RunDetails(automl_run).show()
```

![Gráfico de importancia de características](./media/how-to-configure-auto-train/feature-importance.png)

Para obtener más información sobre cómo se pueden habilitar las explicaciones del modelo y la importancia de las características en otras áreas del SDK fuera del aprendizaje automático automatizado, consulte el artículo de [conceptos](machine-learning-interpretability-explainability.md) sobre la interpretabilidad.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [cómo y dónde implementar un modelo](how-to-deploy-and-where.md).

Obtenga más información sobre [cómo entrenar un modelo de regresión con aprendizaje automático automatizado](tutorial-auto-train-models.md) o [cómo entrenar con aprendizaje automático automatizado en un recurso remoto](how-to-auto-train-remote.md).
