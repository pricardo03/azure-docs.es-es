---
title: Creación de experimentos de ML automatizados
titleSuffix: Azure Machine Learning
description: El aprendizaje automático automatizado elige un algoritmo y genera un modelo listo para la implementación. Obtenga información sobre las opciones que puede usar para configurar experimentos de aprendizaje automático automatizado.
author: cartacioS
ms.author: sacartac
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 03/09/2020
ms.custom: seodec18
ms.openlocfilehash: 6f49529b0599f36ae4a26939bbbe171a45a1a53a
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79127183"
---
# <a name="configure-automated-ml-experiments-in-python"></a>Configuración de experimentos de ML automatizado en Python
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En esta guía, aprenderá a definir diversos valores de configuración de los experimentos de aprendizaje automático automatizado con el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py). El aprendizaje automático automatizado elige un algoritmo e hiperparámetros, y genera un modelo listo para la implementación. Se pueden usar varias opciones para configurar experimentos de aprendizaje automático automatizado.

Para ver ejemplos de experimentos de aprendizaje automático automatizado, consulte [Tutorial: Train a classification model with automated machine learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento de un modelo de clasificación con aprendizaje automático automatizado) o [Entrenamiento de modelos con aprendizaje automático automatizado en la nube mediante Azure Machine Learning](how-to-auto-train-remote.md).

Opciones de configuración disponibles en el aprendizaje automático automatizado:

* Selección del tipo de experimento: Clasificación, regresión o predicción de las series temporales
* Origen de datos, formatos y datos de captura
* Elección el destino del proceso: local o remoto
* Configuración de experimentos de aprendizaje automático automatizado
* Ejecución de un experimento de aprendizaje automático automatizado
* Explorar las métricas del modelo
* Registrar e implementar el modelo

Si prefiere una experiencia sin código, también puede [crear experimentos de aprendizaje automático automatizados en Azure Machine Learning Studio](how-to-use-automated-ml-for-ml-models.md).

## <a name="select-your-experiment-type"></a>Seleccione el tipo de experimento

Antes de comenzar el experimento, debe determinar el tipo de problema de aprendizaje automático que va a resolver. El aprendizaje automático automatizado admite tipos de tareas de clasificación, regresión y previsión. Más información sobre los [tipos de tareas](how-to-define-task-type.md).

El aprendizaje automático automatizado admite los siguientes algoritmos durante el proceso de optimización y automatización. Como usuario, no hay ninguna necesidad de especificar el algoritmo.

clasificación | Regresión | Previsión de series temporales
|-- |-- |--
[Regresión logística](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Red elástica](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Red elástica](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#classification)|[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#regression)|[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#regression)
[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[SVC lineal](https://scikit-learn.org/stable/modules/svm.html#classification)|[Lazo LARS](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[Lazo LARS](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[Clasificación de vectores de soporte (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)|[Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)| [Xgboost](https://xgboost.readthedocs.io/en/latest/parameter.html)
[Clasificador DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNClassifier)|[Regresor DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor) | [Regresor DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/DNNRegressor)|
[Clasificador lineal DNN](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearClassifier)|[Regresión lineal](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)|[Regresión lineal](https://www.tensorflow.org/api_docs/python/tf/estimator/LinearRegressor)
[Bayes naive](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|[Regresor lineal rápida](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.fastlinearregressor?view=nimbusml-py-latest)|[Auto-ARIMA](https://www.alkaline-ml.com/pmdarima/modules/generated/pmdarima.arima.auto_arima.html#pmdarima.arima.auto_arima)
[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|[Regresor descendente de gradiente en línea](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.onlinegradientdescentregressor?view=nimbusml-py-latest)|[Prophet](https://facebook.github.io/prophet/docs/quick_start.html)
|[Clasificador de perceptrón promedio](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.averagedperceptronbinaryclassifier?view=nimbusml-py-latest)||ForecastTCN
|[Clasificador SVM lineal](https://docs.microsoft.com/python/api/nimbusml/nimbusml.linear_model.linearsvmbinaryclassifier?view=nimbusml-py-latest)||

Use el parámetro `task` en el constructor `AutoMLConfig` para especificar el tipo de experimento.

```python
from azureml.train.automl import AutoMLConfig

# task can be one of classification, regression, forecasting
automl_config = AutoMLConfig(task = "classification")
```

## <a name="data-source-and-format"></a>Formato y origen de datos

El aprendizaje automático automatizado es compatible con los datos que residen en el escritorio local o en la nube, como Azure Blob Storage. Los datos se pueden leer en **DataFrame de Pandas** o en **TabularDataset de Azure Machine Learning**.  [Más información sobre los conjuntos de datos](how-to-create-register-datasets.md).

Requisitos para los datos de entrenamiento:
- Los datos deben estar en formato tabular.
- El valor que se va a predecir, la columna de destino, debe estar en los datos.

En los siguientes ejemplos de código se muestra cómo almacenar los datos en estos formatos.

* TabularDataset
  ```python
  from azureml.core.dataset import Dataset
  from azureml.opendatasets import Diabetes
  
  tabular_dataset = Diabetes.get_tabular_dataset()
  train_dataset, test_dataset = tabular_dataset.random_split(percentage=0.1, seed=42)
  label = "Y"
  ```

* Dataframe de Pandas

    ```python
    import pandas as pd
    from sklearn.model_selection import train_test_split

    df = pd.read_csv("your-local-file.csv")
    train_data, test_data = train_test_split(df, test_size=0.1, random_state=42)
    label = "label-col-name"
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Capturar datos para ejecutar un experimento en el proceso remoto

En el caso de las ejecuciones remotas, los datos de aprendizaje deben ser accesibles desde el proceso remoto. La clase [`Datasets`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.dataset.dataset?view=azure-ml-py) del SDK expone la funcionalidad para:

* transferir datos fácilmente de archivos estáticos u orígenes de dirección URL a su área de trabajo
* poner sus datos a disposición de los scripts de entrenamiento al ejecutarse en recursos de proceso en la nube

Consulte el [procedimiento](how-to-train-with-datasets.md#option-2--mount-files-to-a-remote-compute-target) para ver un ejemplo del uso de la clase `Dataset` para montar datos en su destino de proceso.

## <a name="train-and-validation-data"></a>Datos de entrenamiento y validación

Puede especificar conjuntos distintos de entrenamiento y validación directamente en el constructor `AutoMLConfig`.

### <a name="k-folds-cross-validation"></a>Validación cruzada de K iteraciones

Use la opción de configuración `n_cross_validations` para especificar el número de validaciones cruzadas. El conjunto de datos de entrenamiento se dividirá aleatoriamente en `n_cross_validations` iteraciones del mismo tamaño. Durante cada ronda de validación cruzada, una de las iteraciones se usará para la validación del modelo entrenado en las iteraciones restantes. Este proceso se repite durante `n_cross_validations` rondas hasta que cada una se utiliza una vez como conjunto de validación. Se notifican las puntuaciones promedio de todas las rondas de `n_cross_validations` y el modelo correspondiente se puede volver a entrenar en todo el conjunto de datos de entrenamiento.

### <a name="monte-carlo-cross-validation-repeated-random-sub-sampling"></a>Validación de Monte Carlo entre (submuestra aleatoria repetida)

Use `validation_size` para especificar el porcentaje del conjunto de datos de entrenamiento que se debe usar para la validación y use `n_cross_validations` para especificar el número de validaciones cruzadas. Durante cada ronda de validación cruzada, un subconjunto de tamaño `validation_size` se seleccionará aleatoriamente para la validación del modelo entrenado en los datos restantes. Por último, se notifican las puntuaciones promedias de todas las rondas `n_cross_validations` y el modelo correspondiente se puede volver a entrenar en todo el conjunto de datos de entrenamiento. No se admite Monte Carlo para la predicción de series temporales.

### <a name="custom-validation-dataset"></a>Conjunto de datos de validación personalizado

Use un conjunto de datos de validación personalizado si la división aleatoria no es aceptable, normalmente, datos de serie temporal o datos desequilibrados. Puede especificar un conjunto de datos de validación propio. El modelo se evaluará en el conjunto de datos de validación especificado en lugar de un conjunto de datos aleatorio.

## <a name="compute-to-run-experiment"></a>Proceso para ejecutar el experimento

A continuación, determine dónde se va a entrenar el modelo. Un experimento de entrenamiento de aprendizaje automático automatizado se puede ejecutar en las opciones de proceso siguientes:
*    La máquina local, como un escritorio local o un equipo portátil: generalmente, cuando haya un pequeño conjunto de datos y siga en la fase de exploración.
*    Una máquina remota en la nube: [Azure Machine Learning Managed Compute](concept-compute-target.md#amlcompute) es un servicio administrado que permite entrenar modelos de aprendizaje automático en clústeres de máquinas virtuales de Azure.

    Visite este [sitio de GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) para obtener ejemplos de cuadernos con destinos de proceso locales y remotos.

*   Un clúster de Azure Databricks en su suscripción de Azure. Puede encontrar más detalles aquí: [Configuración del clúster de Azure Databricks para ML automatizado](how-to-configure-environment.md#azure-databricks)

    Consulte este [sitio de GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-databricks/automl) para ver ejemplos de cuadernos con Azure Databricks.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Establecer la configuración de experimento

Se pueden usar varias opciones para configurar el experimento de aprendizaje automático automatizado. Estos parámetros se establecen al crear una instancia un objeto `AutoMLConfig`. Consulte la [clase AutoMLConfig](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py) para obtener una lista completa de parámetros.

Estos son algunos ejemplos:

1.    Experimento de clasificación con AUC ponderado como métrica principal con los minutos de tiempo de espera del experimento establecidos en 30 minutos y 2 iteraciones de validación cruzada.

    ```python
    automl_classifier=AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        experiment_timeout_minutes=30,
        blacklist_models=['XGBoostClassifier'],
        training_data=train_data,
        label_column_name=label,
        n_cross_validations=2)
    ```
2.    A continuación, se incluye un ejemplo de un conjunto de experimentos de regresión que finaliza después de sesenta iteraciones con cinco iteraciones de validación cruzada.

    ```python
    automl_regressor = AutoMLConfig(
        task='regression',
        experiment_timeout_minutes=60,
        whitelist_models=['kNN regressor'],
        primary_metric='r2_score',
        training_data=train_data,
        label_column_name=label,
        n_cross_validations=5)
    ```

Los tres valores diferentes del parámetro `task` (el tercer tipo de tarea es `forecasting` y usa el mismo grupo de algoritmos que las tareas `regression`) determinan la lista de modelos que se aplican. Use los parámetros `whitelist` o `blacklist` para modificar aún más las iteraciones con los modelos disponibles para incluir o excluir. La lista de modelos admitidos se puede encontrar en [Clase SupportedModels](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels) para ([clasificación](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.classification), [previsión](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.forecasting) y [regresión](https://docs.microsoft.com/python/api/azureml-train-automl-client/azureml.train.automl.constants.supportedmodels.regression)).

El servicio de validación del aprendizaje automático automatizado requerirá que se establezca `experiment_timeout_minutes` en un tiempo de espera mínimo de 15 minutos para evitar los errores por tiempo de espera en el experimento.

### <a name="primary-metric"></a>Métrica principal
La métrica principal determina la métrica que se utilizará durante el entrenamiento del modelo para la optimización. Las métricas disponibles que puede seleccionar vienen determinadas por el tipo de tarea que elige y en la siguiente tabla se muestran métricas principales válidas para cada tipo de tarea.

|clasificación | Regresión | Previsión de series temporales
|-- |-- |--
|accuracy| spearman_correlation | spearman_correlation
|AUC_weighted | normalized_root_mean_squared_error | normalized_root_mean_squared_error
|average_precision_score_weighted | r2_score | r2_score
|norm_macro_recall | normalized_mean_absolute_error | normalized_mean_absolute_error
|precision_score_weighted |

Obtenga información acerca de las definiciones específicas de estas métricas en [Descripción de los resultados de aprendizaje automático automatizado](how-to-understand-automated-ml.md).

### <a name="data-featurization"></a>Caracterización de datos

En cada experimento de aprendizaje automático automatizado, los datos se [escalan y se normalizan automáticamente](concept-automated-ml.md#preprocess) para ayudar a *determinados* algoritmos que dependen de características que se encuentran en diferentes escalas.  Sin embargo, también puede habilitar la caracterización adicional, como la atribución de valores que faltan, la codificación y las transformaciones. [Más información sobre qué caracterización se incluye](how-to-use-automated-ml-for-ml-models.md#featurization).

Al configurar los experimentos, puede habilitar la configuración avanzada `featurization`. En la tabla siguiente se muestra la configuración aceptada para la caracterización de la clase [`AutoMLConfig`](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

|Configuración de la caracterización | Descripción |
| ------------- | ------------- |
|`"featurization":`&nbsp;`'FeaturizationConfig'`| Indica que se debe usar un paso personalizado de caracterización. [Aprenda a personalizar la caracterización](how-to-configure-auto-train.md#customize-feature-engineering).|
|`"featurization": 'off'`| Indica que el paso de caracterización no debe realizarse automáticamente.|
|`"featurization": 'auto'`| Indica que, como parte del preprocesamiento, los [pasos de caracterización y protección](how-to-use-automated-ml-for-ml-models.md#advanced-featurization-options) se realizan automáticamente.|

> [!NOTE]
> Los pasos de la caracterización del aprendizaje automático automatizado (normalización de características, control de los datos que faltan, conversión de valores de texto a numéricos, etc.) se convierten en parte del modelo subyacente. Cuando se usa el modelo para realizar predicciones, se aplican automáticamente a los datos de entrada los mismos pasos de caracterización que se aplican durante el entrenamiento.

### <a name="time-series-forecasting"></a>Previsión de series temporales
La tarea `forecasting` de serie temporal requiere parámetros adicionales en el objeto de configuración:

1. `time_column_name`: parámetro obligatorio que define el nombre de la columna de los datos de aprendizaje que contienen una serie temporal válida.
1. `max_horizon`: define la duración del tiempo que desea predecir en función de la periodicidad de los datos de aprendizaje. Por ejemplo si tiene datos de entrenamiento con intervalos de agregación diaria, defina la distancia en días para la que desea que se entrene el modelo.
1. `grain_column_names`: define el nombre de las columnas que contienen datos de serie temporal individuales en los datos de aprendizaje. Por ejemplo, si está previendo las ventas de una marca determinada por tienda, definiría las columnas de la tienda y de la marca como sus columnas de agregación. Se crearán series temporales y previsiones para cada detalle o agrupación. 

Para ver ejemplos de la configuración usada a continuación, consulte el [cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb).

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

automl_config = AutoMLConfig(task = 'forecasting',
                             debug_log='automl_oj_sales_errors.log',
                             primary_metric='normalized_root_mean_squared_error',
                             experiment_timeout_minutes=20,
                             training_data=train_data,
                             label_column_name=label,
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
* `stack_meta_learner_kwargs`: parámetros opcionales que se van a pasar al inicializador del metaaprendizaje. Estos parámetros y tipos de parámetro reflejan los parámetros y tipos de parámetro del constructor del modelo correspondiente y se reenvían a dicho constructor.

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
        experiment_timeout_minutes=30,
        training_data=train_data,
        label_column_name=label,
        n_cross_validations=5,
        **ensemble_settings
        )
```

El entrenamiento de conjunto está habilitado de forma predeterminada, pero se puede deshabilitar mediante los parámetros booleanos `enable_voting_ensemble` y `enable_stack_ensemble`.

```python
automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        experiment_timeout_minutes=30,
        training_data=data_train,
        label_column_name=label,
        n_cross_validations=5,
        enable_voting_ensemble=False,
        enable_stack_ensemble=False
        )
```

## <a name="run-experiment"></a>Ejecutar experimento

Para una instancia de ML automatizado, se crea un objeto `Experiment`, que es un objeto con nombre en un objeto `Workspace` que se usa para ejecutar experimentos.

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
Hay unas cuantas opciones que puede definir para finalizar el experimento.
1. Sin criterios: si no se define ningún parámetro de salida, el experimento continuará hasta que no se realice ningún progreso adicional en la métrica principal.
1. Salir transcurrido un tiempo determinado: el uso de `experiment_timeout_minutes` en su configuración le permite definir los minutos que debe tardar un experimento en seguir ejecutándose.
1. Salir una vez alcanzada una puntuación: al usarse `experiment_exit_score` se completará el experimento una vez alcanzada una puntuación de métrica principal.

### <a name="explore-model-metrics"></a>Explorar las métricas del modelo

Puede ver los resultados del entrenamiento en un widget o en línea si se encuentra en un bloc de notas. Vea [Seguimiento y evaluación de modelos](how-to-track-experiments.md#view-run-details) para obtener más información.

## <a name="understand-automated-ml-models"></a>Descripción de los modelos de ML automatizado

Cualquier modelo generado mediante ML automatizado incluye los siguientes pasos:
+ Diseño de características automatizadas (si `"featurization": 'auto'`)
+ Escalado o normalización y algoritmo con valores de hiperparámetro

Se obtiene de manera clara esta información a partir de la salida del modelo fitted_model de ML automatizado.

```python
automl_config = AutoMLConfig(…)
automl_run = experiment.submit(automl_config …)
best_run, fitted_model = automl_run.get_output()
```

### <a name="automated-feature-engineering"></a>Diseño de características automatizadas

Consulte la lista de preprocesamiento y del [diseño de características automatizadas](concept-automated-ml.md#preprocess) que ocurre cuando `"featurization": 'auto'`.

En este ejemplo:
+ Hay cuatro características de entrada: A (numérico), B (numérico), C (numérico), D (fecha y hora)
+ La característica numérica C se puede quitar porque es una columna de identificador con todos los valores únicos
+ A las características numéricas A y B les faltan valores y, por tanto, los atribuye la media
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
  >Utilice 'timeseriestransformer' para la tarea = 'forecasting'; en caso contrario, utilice 'datatransformer' para la tarea 'regression' o 'classification'.

+ API 2: `get_featurization_summary()` devuelve un resumen de la caracterización para todas las características de entrada.

  Uso:
  ```python
  fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
  ```

  >[!Note]
  >Utilice 'timeseriestransformer' para la tarea = 'forecasting'; en caso contrario, utilice 'datatransformer' para la tarea 'regression' o 'classification'.

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
   
### <a name="customize-feature-engineering"></a>Diseño de características personalizadas
Para personalizar el diseño de características, especifique `"featurization": FeaturizationConfig`.

Entre las personalizaciones admitidas se incluyen:

|Personalización|Definición|
|--|--|
|Actualización del propósito de la columna|Reemplazar el tipo de característica para la columna especificada.|
|Actualización de parámetros del transformador |Actualizar los parámetros para el transformador especificado. Actualmente admite Imputer (media, más frecuente y mediana) y HashOneHotEncoder.|
|Quitar columnas |Las columnas que se van a quitar de la caracterización.|
|Transformadores de bloque| Los transformadores de bloque que se van a usar en el proceso de características.|

Cree el objeto FeaturizationConfig mediante llamadas a API:
```python
featurization_config = FeaturizationConfig()
featurization_config.blocked_transformers = ['LabelEncoder']
featurization_config.drop_columns = ['aspiration', 'stroke']
featurization_config.add_column_purpose('engine-size', 'Numeric')
featurization_config.add_column_purpose('body-style', 'CategoricalHash')
#default strategy mean, add transformer param for for 3 columns
featurization_config.add_transformer_params('Imputer', ['engine-size'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['city-mpg'], {"strategy": "median"})
featurization_config.add_transformer_params('Imputer', ['bore'], {"strategy": "most_frequent"})
featurization_config.add_transformer_params('HashOneHotEncoder', [], {"number_of_bits": 3})
```

### <a name="scalingnormalization-and-algorithm-with-hyperparameter-values"></a>Escalado o normalización y algoritmo con valores de hiperparámetro:

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

La siguiente salida de ejemplo es para una canalización mediante un algoritmo concreto (LogisticRegression con RobustScalar, en este caso).

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

### <a name="predict-class-probability"></a>Predicción de la probabilidad de las clases

Los modelos generados mediante el aprendizaje automático automatizado tienen objetos contenedores que reflejan la funcionalidad de su clase de origen de código abierto. La mayoría de los objetos contenedores de modelos de clasificación devueltos por el aprendizaje automático automatizado implementan la función `predict_proba()`, que acepta una muestra de datos similares a una matriz o de matriz dispersa de las características (valores X), y devuelve una matriz de n dimensiones de cada muestra y la probabilidad de la clase respectiva.

Suponiendo que ha recuperado el modelo mejor ejecutado y ajustado con las mismas llamadas anteriores, puede llamar a `predict_proba()` directamente desde el modelo ajustado, proporcionando un ejemplo de `X_test` en el formato adecuado según el tipo de modelo.

```python
best_run, fitted_model = automl_run.get_output()
class_prob = fitted_model.predict_proba(X_test)
```

Si el modelo subyacente no admite la función `predict_proba()` o el formato es incorrecto, se producirá una excepción específica de la clase del modelo. Vea los documentos de referencia [RandomForestClassifier](https://scikit-learn.org/stable/modules/generated/sklearn.ensemble.RandomForestClassifier.html#sklearn.ensemble.RandomForestClassifier.predict_proba) y [XGBoost](https://xgboost.readthedocs.io/en/latest/python/python_api.html) para obtener ejemplos de cómo se implementa esta función para los distintos tipos de modelos.

<a name="explain"></a>

## <a name="model-interpretability"></a>Interoperabilidad del modelo

La interpretabilidad del modelo permite comprender por qué los modelos hacen predicciones y los valores de importancia de las características subyacentes. El SDK incluye varios paquetes para habilitar las características de interpretabilidad del modelo, tanto en el momento del entrenamiento como en el de inferencia, para los modelos implementados y locales.

Vea el [procedimiento](how-to-machine-learning-interpretability-automl.md) de los ejemplos de código sobre cómo habilitar características de interpretabilidad específicamente dentro de experimentos de aprendizaje automático automatizados.

Para información general sobre cómo se pueden habilitar las explicaciones del modelo y la importancia de las características en otras áreas del SDK fuera del aprendizaje automático automatizado, consulte el artículo de [conceptos](how-to-machine-learning-interpretability.md) sobre la interpretabilidad.

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [cómo y dónde implementar un modelo](how-to-deploy-and-where.md).

Obtenga más información sobre [cómo entrenar un modelo de regresión con aprendizaje automático automatizado](tutorial-auto-train-models.md) o [cómo entrenar con aprendizaje automático automatizado en un recurso remoto](how-to-auto-train-remote.md).
