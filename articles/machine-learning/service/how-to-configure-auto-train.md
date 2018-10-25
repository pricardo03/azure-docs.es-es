---
title: 'Configurar su propio experimento de aprendizaje automático automatizado: Azure Machine Learning'
description: El aprendizaje automático automatizado elige un algoritmo y genera un modelo listo para la implementación. Obtenga información sobre las opciones que puede usar para configurar experimentos de aprendizaje automático automatizado.
author: nacharya1
ms.author: nilesha
ms.reviewer: sgilley
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 1aeb1315cfafbcdf3507a6e49d71e1f1e69b537c
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430194"
---
# <a name="configure-your-automated-machine-learning-experiment"></a>Configuración del experimento de aprendizaje automático automatizado

El aprendizaje automático automatizado (ML automatizado) elige un algoritmo e hiperparámetros y genera un modelo listo para la implementación. Además, el modelo se puede descargar para personalizarse aún más. Hay varias opciones que puede usar para configurar experimentos de ML automatizados. En esta guía, obtendrá información sobre cómo definir varias opciones de configuración.

Para ver ejemplos de ML automatizado, consulte [Tutorial: Train a classification model with automated machine learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento de un modelo de clasificación con aprendizaje automático automatizado) o [Entrenamiento de modelos con aprendizaje automático automatizado en la nube mediante Azure Machine Learning](how-to-auto-train-remote.md).

Opciones de configuración disponibles en el aprendizaje automático automatizado:

* Seleccionar el tipo de experimento; por ejemplo, clasificación y regresión 
* Origen de datos, formatos y datos de captura
* Elegir el destino del proceso (local o remoto)
* Configuración del experimento de ML automatizado
* Ejecutar un experimento de ML automatizado
* Explorar las métricas del modelo
* Registrar e implementar el modelo

## <a name="select-your-experiment-type"></a>Seleccione el tipo de experimento
Antes de comenzar el experimento, debe determinar el tipo de problema de aprendizaje automático que va a resolver. El ML automatizado admite dos categorías de aprendizaje supervisado: clasificación y regresión. El ML automatizado admite los siguientes algoritmos durante el proceso de optimización y automatización. Como usuario, no hay ninguna necesidad de especificar el algoritmo.
clasificación | Regresión
--|--
sklearn.linear_model.LogisticRegression | sklearn.linear_model.ElasticNet
sklearn.linear_model.SGDClassifier  | sklearn.ensemble.GradientBoostingRegressor
sklearn.naive_bayes.BernoulliNB | sklearn.tree.DecisionTreeRegressor
sklearn.naive_bayes.MultinomialNB | sklearn.neighbors.KNeighborsRegressor
sklearn.svm.SVC | sklearn.linear_model.LassoLars
sklearn.svm.LinearSVC | sklearn.linear_model.SGDRegressor
sklearn.calibration.CalibratedClassifierCV |    sklearn.ensemble.RandomForestRegressor
sklearn.neighbors.KNeighborsClassifier |    sklearn.ensemble.ExtraTreesRegressor
sklearn.tree.DecisionTreeClassifier |   lightgbm.LGBMRegressor
sklearn.ensemble.RandomForestClassifier |
sklearn.ensemble.ExtraTreesClassifier   |
sklearn.ensemble.GradientBoostingClassifier |
lightgbm.LGBMClassifier |


## <a name="data-source-and-format"></a>Formato y origen de datos
El ML automatizado es compatible con los datos que residen en el escritorio local o en la nube en Azure Blob Storage. Los datos pueden leerse en los formatos de datos compatibles con scikit-learn. Puede leer los datos en 1) matrices de Numpy X (características) e Y (variable de destino o etiqueta), o 2) dataframe de Pandas. 

Ejemplos:

1.  Matrices de Numpy

    ```python
    digits = datasets.load_digits()
    X_digits = digits.data 
    y_digits = digits.target
    ```

2.  Dataframe de Pandas

    ```python
    Import pandas as pd
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    le = LabelEncoder() 
    le.fit(df["Label"].values) 
    y = le.transform(df["Label"].values) 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Capturar datos para ejecutar un experimento en el proceso remoto

Si usa un proceso remoto para ejecutar el experimento, se debe ajustar la operación de captura de datos en un script de python independiente `get_data()`. Este script se ejecuta en el proceso remoto donde se ejecuta el experimento de ML automatizado. `get_data` elimina la necesidad de capturar los datos a través del cable para cada iteración. Sin `get_data`, el experimento generará un error cuando se ejecute en el proceso remoto.

A continuación se incluye un ejemplo de `get_data`:

```python
%%writefile $project_folder/get_data.py 
import pandas as pd 
from sklearn.model_selection 
import train_test_split 
from sklearn.preprocessing import LabelEncoder 
def get_data(): # Burning man 2016 data 
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"') 
    # get integer labels 
    le = LabelEncoder() 
    le.fit(df["Label"].values) 
    y = le.transform(df["Label"].values) 
    df = df.drop(["Label"], axis=1) 
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42) 
    return { "X" : df, "y" : y }
```

En su objeto `AutoMLConfig`, especifique el parámetro `data_script` y proporcione la ruta de acceso al archivo de script `get_data`, como se indica a continuación:

```python
automl_config = AutoMLConfig(****, data_script=project_folder + "/get_data.py", **** )
```

El script `get_data` puede devolver lo siguiente:
Clave | Escriba |    Se excluye mutuamente con | DESCRIPCIÓN
---|---|---|---
X | Dataframe de Pandas o matriz de Numpy | data_train, label, columns |  Todas las características que se usan para el aprendizaje
y | Dataframe de Pandas o matriz de Numpy |   label   | Datos de etiquetas que se usan para el aprendizaje. Para la clasificación, debe ser una matriz de enteros.
X_valid | Dataframe de Pandas o matriz de Numpy   | data_train, label | _Opcional_ Todas las características con las que se debe validar. Si no se especifica, X se divide entre "train" y "validate"
y_valid |   Dataframe de Pandas o matriz de Numpy | data_train, label | _Opcional_ Los datos de etiqueta con los que se va a validar. Si no se especifica, Y se divide entre "train" y "validate"
sample_weight | Dataframe de Pandas o matriz de Numpy |   data_train, label, columns| _Opcional_ Un valor de ponderación para cada ejemplo. Se usa cuando se quieren asignar ponderaciones distintas a los puntos de datos 
sample_weight_valid | Dataframe de Pandas o matriz de Numpy | data_train, label, columns |    _Opcional_ Un valor de ponderación para cada ejemplo de validación. Si no se especifica, sample_weight se divide entre "train" y "validate"
data_train |    Dataframe de Pandas |  X, y, X_valid, y_valid |    Todos los datos (características y etiqueta) que se usarán para el aprendizaje
label | string  | X, y, X_valid, y_valid |  Qué columna de data_train representa la etiqueta
columnas | Matriz de cadenas  ||  _Opcional_ Lista blanca de columnas que se usarán para las características
cv_splits_indices   | Matriz de enteros ||  _Opcional_ Lista de índices para dividir los datos para la validación cruzada

## <a name="train-and-validation-data"></a>Datos de entrenamiento y validación

Puede especificar un conjunto distinto de entrenamiento y validación mediante get_data() o, directamente, en el método `AutoMLConfig`.

## <a name="cross-validation-split-options"></a>Opciones de división de validación cruzada

### <a name="k-folds-cross-validation"></a>Validación cruzada de K iteraciones

Use la opción de configuración `n_cross_validations` para especificar el número de validaciones cruzadas. El conjunto de datos de entrenamiento se dividirá aleatoriamente en `n_cross_validations` iteraciones del mismo tamaño. Durante cada ronda de validación cruzada, una de las iteraciones se usará para la validación del modelo entrenado en las iteraciones restantes. Este proceso se repite durante `n_cross_validations` rondas hasta que cada una se utiliza una vez como conjunto de validación. Por último, se notifican las puntuaciones promedias de todas las rondas `n_cross_validations` y el modelo correspondiente se puede volver a entrenar en todo el conjunto de datos de entrenamiento.

### <a name="monte-carlo-cross-validation-aka-repeated-random-sub-sampling"></a>Validación cruzada de Monte Carlo (conocida como submuestra aleatoria repetida)

Use `validation_size` para especificar el porcentaje del conjunto de datos de entrenamiento que se debe usar para la validación y use `n_cross_validations` para especificar el número de validaciones cruzadas. Durante cada ronda de validación cruzada, un subconjunto de tamaño `validation_size` se seleccionará aleatoriamente para la validación del modelo entrenado en los datos restantes. Por último, se notifican las puntuaciones promedias de todas las rondas `n_cross_validations` y el modelo correspondiente se puede volver a entrenar en todo el conjunto de datos de entrenamiento.

### <a name="custom-validation-dataset"></a>Conjunto de datos de validación personalizado

Use un conjunto de datos de validación personalizado si la división aleatoria no es aceptable (normalmente, datos de serie temporal o datos desequilibrados). Con esto, puede especificar su propio conjunto de datos de validación. El modelo se evaluará en el conjunto de datos de validación especificado en lugar de un conjunto de datos aleatorio.

## <a name="compute-to-run-experiment"></a>Proceso para ejecutar el experimento

A continuación, determine dónde se va a entrenar el modelo. Se ejecuta un experimento de entrenamiento de ML en un destino de proceso que posee y administra. 

Las opciones de proceso admitidas son:
1.  La máquina local, como un escritorio local o un equipo portátil: generalmente, cuando haya un pequeño conjunto de datos y siga en la fase de exploración.
2.  Un equipo remoto en la nube ([Azure Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) que ejecuta Linux): tiene un conjunto de datos grande y quiere escalar verticalmente a una máquina grande disponible en la nube de Azure. 
3.  Clúster de Azure Batch AI: un clúster administrado que se puede configurar para escalar horizontalmente y ejecutar iteraciones de ML automatizado en paralelo. 

<a name='configure-experiment'/>
## <a name="configure-your-experiment-settings"></a>Establecer la configuración de experimento

Hay varios mandos que puede usar para configurar experimentos de ML automatizados. Estos parámetros se establecen al crear una instancia un objeto `AutoMLConfig`.

Estos son algunos ejemplos:

1.  Experimento de clasificación mediante AUC ponderado como la métrica principal con un tiempo máximo de 12 000 segundos por cada iteración. El experimento finaliza después de 50 iteraciones y 2 iteraciones de validación cruzada.

    ```python
    automl_classifier = AutoMLConfig(
        task='classification',
        primary_metric='AUC_weighted',
        max_time_sec=12000,
        iterations=50,
        X=X, 
        y=y,
        n_cross_validations=2)
    ```
2.  A continuación se incluye un ejemplo de un conjunto de experimentos de regresión que finaliza después de 100 iteraciones. Cada iteración tiene una duración de hasta 600 segundos con 5 iteraciones de validación cruzada.

    ````python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        primary_metric='r2_score',
        X=X, 
        y=y,
        n_cross_validations=5)
    ````

Esta tabla enumera la configuración de parámetros disponible para el experimento y sus valores predeterminados.

Propiedad |  DESCRIPCIÓN | Valor predeterminado
--|--|--
`task`  |Especifique el tipo de problema de aprendizaje automático. Los valores permitidos son <li>clasificación</li><li>regresión</li>    | None |
`primary_metric` |Métrica que quiere optimizar en la creación del modelo. Por ejemplo, si se especifica la precisión como primary_metric, el ML automatizado intenta encontrar un modelo con la máxima precisión. Solo puede especificar un valor de primary_metric por experimento. Los valores permitidos son <br/>**Clasificación**.<br/><li> accuracy  </li><li> AUC_weighted</li><li> precision_score_weighted </li><li> balanced_accuracy </li><li> average_precision_score_weighted </li><br/>**Regresión**. <br/><li> normalized_mean_absolute_error </li><li> spearman_correlation </li><li> normalized_root_mean_squared_error </li><li> normalized_root_mean_squared_log_error</li><li> R2_score    </li> | Para la clasificación: accuracy <br/>Para la regresión: spearman_correlation <br/> |
`exit_score` |  Puede establecer un valor de destino para primary_metric. Una vez que se encuentra un modelo que cumple el objetivo de primary_metric, el ML automatizado detendrá la iteración y el experimento finalizará. Si este valor no se establece (valor predeterminado), el experimento de ML automatizado seguirá ejecutando el número de iteraciones especificadas. Toma un doble valor. Si no se llega al destino, el ML automatizado continuará hasta que alcance el número de iteraciones especificadas.|   None
`iterations` |Número máximo de iteraciones. Cada iteración es igual a un trabajo de aprendizaje que da como resultado una canalización. La canalización es el modelo y el procesamiento previo de datos. Para obtener un modelo de alta calidad, use 250 o más | 100
`Concurrent_iterations`|    Número máximo de iteraciones que se ejecutarán en paralelo. Esta configuración funciona solo para el proceso remoto.|    1
`max_cores_per_iteration`   | Indica el número de núcleos en el destino de proceso que se usaría para el aprendizaje de una sola canalización. Si el algoritmo puede utilizar varios núcleos, esto aumenta el rendimiento en una máquina de varios núcleos. Puede establecerlo en -1 para usar todos los núcleos disponibles en la.|  1
`max_time_sec` |    Limita la cantidad de tiempo (segundos) que tarda una iteración determinada. Si una iteración supera la cantidad especificada, se cancela. Si no se establece, la iteración continúa ejecutándose hasta que finaliza. |   None
`n_cross_validations`   |Número de divisiones de validación cruzada| None
`validation_size`   |Tamaño del conjunto de validación establecido como porcentaje de toda la muestra de aprendizaje.|  None
`preprocess` | True/False <br/>True permite al experimento realizar el procesamiento previo en la entrada. A continuación se incluye un subconjunto de procesamiento previo<li>Faltan datos: imputa los datos que faltan (numérico con promedio, texto más repetido) </li><li>Valores de categorías: si el tipo de datos es numérico y el número de valores únicos es inferior al 5 %, se convierte a codificación "one-hot" </li><li>Etc. Para obtener una lista completa, visite [el repositorio de GitHub](https://aka.ms/aml-notebooks)</li><br/>Nota: Si los datos están dispersos no se puede usar preprocess = true |  False | 
`blacklist_algos`   | El experimento de ML automatizado prueba muchos algoritmos diferentes. Configure el ML automatizado para excluir determinados algoritmos del experimento. Resulta útil si es consciente de que los algoritmos no funcionan bien para su conjunto de datos. La exclusión de algoritmos puede ahorrarle recursos de procesos y tiempo de aprendizaje.<br/>Valores permitidos para la clasificación<br/><li>regresión logística</li><li>Clasificador SGD</li><li>MultinomialNB</li><li>BernoulliNB</li><li>SVM</li><li>LinearSVM</li><li>kNN</li><li>DT</li><li>RF</li><li>árboles adicionales</li><li>potenciación del gradiente</li><li>lgbm_classifier</li><br/>Valores permitidos para la regresión<br/><li>Red elástica</li><li>Regresor de potenciación del gradiente</li><li>Regresor DT</li><li>Regresor kNN</li><li>Lazo LARS</li><li>Regresor SGD</li><li>Regresor RF</li><li>regresor de árboles adicionales</li>|   None
`verbosity` |Controla el nivel de registro: INFO es el más detallado y CRITICAL, el menos detallado.<br/>Los valores permitidos son:<br/><li>logging.INFO</li><li>logging.WARNING</li><li>logging.ERROR</li><li>logging.CRITICAL</li>  | logging.INFO</li> 
`X` | Todas las características que se usan para el aprendizaje |  None
`y` |   Datos de etiquetas que se usan para el aprendizaje. Para la clasificación, debe ser una matriz de enteros.|  None
`X_valid`|_Opcional_ Todas las características con las que se debe validar. Si no se especifica, X se divide entre "train" y "validate" |   None
`y_valid`   |_Opcional_ Los datos de etiqueta con los que se va a validar. Si no se especifica, Y se divide entre "train" y "validate"    | None
`sample_weight` |   _Opcional_ Un valor de ponderación para cada ejemplo. Se usa cuando se quieren asignar ponderaciones distintas a los puntos de datos |   None
`sample_weight_valid`   |   _Opcional_ Un valor de ponderación para cada ejemplo de validación. Si no se especifica, sample_weight se divide entre "train" y "validate"   | None
`run_configuration` |   Objeto RunConfiguration.  Se usa para ejecuciones remotas. |None
`data_script`  |    Ruta de acceso a un archivo que contiene el método get_data.  Se usa para ejecuciones remotas.   |None


## <a name="run-experiment"></a>Ejecutar experimento

A continuación, podemos iniciar el experimento para ejecutar y generar un modelo para nosotros. Pase `AutoMLConfig` al método `submit` para generar el modelo.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Las dependencias se instalan por primera vez en una nueva instancia de DSVM.  El resultado puede tardar hasta 10 minutos en mostrarse.
>Establecer `show_output` como True genera un resultado que se muestra en la consola.


## <a name="explore-model-metrics"></a>Explorar las métricas del modelo
Puede ver los resultados en un widget o en línea si se encuentra en un bloc de notas. Consulte los detalles en para hacer un seguimiento de los modelos y evaluarlos. (Asegúrese de que el contenido AML contenga información relevante para el ML automatizado)

Las siguientes métricas se guardan en cada iteración
* AUC_macro
* AUC_micro
* AUC_weighted
* accuracy
* average_precision_score_macro
* average_precision_score_micro
* average_precision_score_weighted
* balanced_accuracy
* f1_score_macro
* f1_score_micro
* f1_score_weighted
* log_loss
* norm_macro_recall
* precision_score_macro
* precision_score_micro
* precision_score_weighted
* recall_score_macro
* recall_score_micro
* recall_score_weighted
* weighted_accuracy

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [cómo y dónde implementar un modelo](how-to-deploy-and-where.md).

Obtenga más información sobre [cómo entrenar un modelo de clasificación con ML automatizado](tutorial-auto-train-models.md) o [cómo entrenar con ML automatizado en un recurso remoto](how-to-auto-train-remote.md). 