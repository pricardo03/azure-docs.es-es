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
ms.date: 01/08/2019
ms.custom: seodec18
ms.openlocfilehash: 79d013e5836555547cbf254bb25c06add0a717e1
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295392"
---
# <a name="configure-automated-machine-learning-experiments"></a>Configuración de experimentos de aprendizaje automático automatizados

El aprendizaje automático automatizado elige un algoritmo e hiperparámetros, y genera un modelo listo para la implementación. Se pueden usar varias opciones para configurar experimentos de aprendizaje automático automatizado. En esta guía, obtendrá información sobre cómo definir varias opciones de configuración.

Para ver ejemplos de experimentos de aprendizaje automático automatizado, vea [Tutorial: Train a classification model with automated machine learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento de un modelo de clasificación con aprendizaje automático automatizado) o [Entrenamiento de modelos con aprendizaje automático automatizado en la nube mediante Azure Machine Learning](how-to-auto-train-remote.md).

Opciones de configuración disponibles en el aprendizaje automático automatizado:

* Selección del tipo de experimento: Clasificación, Regresión o Previsión
* Origen de datos, formatos y datos de captura
* Elección el destino del proceso: local o remoto
* Configuración de experimentos de aprendizaje automático automatizado
* Ejecución de un experimento de aprendizaje automático automatizado
* Explorar las métricas del modelo
* Registrar e implementar el modelo

## <a name="select-your-experiment-type"></a>Seleccione el tipo de experimento
Antes de comenzar el experimento, debe determinar el tipo de problema de aprendizaje automático que va a resolver. El aprendizaje automático automatizado admite tipos de tareas de clasificación, regresión y previsión.

Aunque las funciones de aprendizaje automático automatizado están disponibles con carácter general, **la previsión aún está en versión preliminar pública.**

El aprendizaje automático automatizado admite los siguientes algoritmos durante el proceso de optimización y automatización. Como usuario, no hay ninguna necesidad de especificar el algoritmo.

clasificación | Regresión | Previsión
|-- |-- |--
[Regresión logística](https://scikit-learn.org/stable/modules/linear_model.html#logistic-regression)| [Red elástica](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)| [Red elástica](https://scikit-learn.org/stable/modules/linear_model.html#elastic-net)
[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#sgd)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)
[Bayes naive](https://scikit-learn.org/stable/modules/naive_bayes.html#bernoulli-naive-bayes)|[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#regression)|[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#regression)
[Clasificación de vectores de soporte de C (SVC)](https://scikit-learn.org/stable/modules/svm.html#classification)|[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#regression)|[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#regression)
[SVC lineal](https://scikit-learn.org/stable/modules/svm.html#classification)|[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)|[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors-regression)
[K Vecinos más próximos](https://scikit-learn.org/stable/modules/neighbors.html#nearest-neighbors)|[Lazo LARS](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)|[Lazo LARS](https://scikit-learn.org/stable/modules/linear_model.html#lars-lasso)
[Árbol de decisión](https://scikit-learn.org/stable/modules/tree.html#decision-trees)|[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)|[Descenso de gradiente estocástico (SGD)](https://scikit-learn.org/stable/modules/sgd.html#regression)
[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)|[Bosque aleatorio](https://scikit-learn.org/stable/modules/ensemble.html#random-forests)
[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)|[Árboles extremadamente aleatorios](https://scikit-learn.org/stable/modules/ensemble.html#extremely-randomized-trees)
[Potenciación del gradiente](https://scikit-learn.org/stable/modules/ensemble.html#classification)|
[Light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)|


## <a name="data-source-and-format"></a>Formato y origen de datos
El aprendizaje automático automatizado es compatible con los datos que residen en el escritorio local o en la nube, como Azure Blob Storage. Los datos pueden leerse en los formatos de datos compatibles con scikit-learn. Puede leer los datos en:
* Matrices de Numpy X (características) e y (variable de destino o también denominada etiqueta)
* Dataframe de Pandas

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
    df = pd.read_csv("https://automldemods.blob.core.windows.net/datasets/PlayaEvents2016,_1.6MB,_3.4k-rows.cleaned.2.tsv", delimiter="\t", quotechar='"')
    # get integer labels
    df = df.drop(["Label"], axis=1)
    df_train, _, y_train, _ = train_test_split(df, y, test_size=0.1, random_state=42)
    ```

## <a name="fetch-data-for-running-experiment-on-remote-compute"></a>Capturar datos para ejecutar un experimento en el proceso remoto

Si usa un proceso remoto para ejecutar el experimento, se debe ajustar la operación de captura de datos en un script de python independiente `get_data()`. Este script se ejecuta en el proceso remoto donde se ejecuta el experimento de aprendizaje automático automatizado. `get_data` elimina la necesidad de capturar los datos a través del cable para cada iteración. Sin `get_data`, el experimento generará un error cuando se ejecute en el proceso remoto.

A continuación se incluye un ejemplo de `get_data`:

```python
%%writefile $project_folder/get_data.py
import pandas as pd
from sklearn.model_selection import train_test_split
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

El script `get_data` puede devolver:

Clave | Type |    Se excluye mutuamente con | DESCRIPCIÓN
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

### <a name="load-and-prepare-data-using-dataprep-sdk"></a>Carga y preparación de datos con el SDK de preparación de datos
Los experimentos de aprendizaje automático automatizados admiten la carga y las transformaciones de datos mediante el SDK para la preparación de datos. El uso del SDK proporciona la capacidad de

>* Opciones de carga desde varios tipos de archivos con inferencia de parámetros de análisis (codificación, separador, encabezados).
>* Opciones de conversión de tipo usando la inferencia durante la carga de archivos.
>* Compatibilidad con la conexión para MS SQL Server y Azure Data Lake Storage.
>* Adición de una columna mediante una expresión
>* Atribución de valores que faltan
>* Derivación de columnas por ejemplos
>* Filtros
>* Transformaciones personalizadas de Python

Para obtener información sobre el SDK de preparación de datos, consulte el artículo [Cómo preparar datos para el modelado](how-to-load-data.md).
A continuación se muestra un ejemplo de carga de datos mediante el SDK de preparación de datos.
```python
# The data referenced here was pulled from `sklearn.datasets.load_digits()`.
simple_example_data_root = 'https://dprepdata.blob.core.windows.net/automl-notebook-data/'
X = dprep.auto_read_file(simple_example_data_root + 'X.csv').skip(1)  # Remove the header row.
# You can use `auto_read_file` which intelligently figures out delimiters and datatypes of a file.

# Here we read a comma delimited file and convert all columns to integers.
y = dprep.read_csv(simple_example_data_root + 'y.csv').to_long(dprep.ColumnSelector(term='.*', use_regex = True))
```

## <a name="train-and-validation-data"></a>Datos de entrenamiento y validación

Puede especificar un conjunto distinto de entrenamiento y validación mediante get_data() o, directamente, en el método `AutoMLConfig`.

## <a name="cross-validation-split-options"></a>Opciones de división de validación cruzada

### <a name="k-folds-cross-validation"></a>Validación cruzada de K iteraciones

Use la opción de configuración `n_cross_validations` para especificar el número de validaciones cruzadas. El conjunto de datos de entrenamiento se dividirá aleatoriamente en `n_cross_validations` iteraciones del mismo tamaño. Durante cada ronda de validación cruzada, una de las iteraciones se usará para la validación del modelo entrenado en las iteraciones restantes. Este proceso se repite durante `n_cross_validations` rondas hasta que cada una se utiliza una vez como conjunto de validación. Se notifican las puntuaciones promedio de todas las rondas de `n_cross_validations` y el modelo correspondiente se puede volver a entrenar en todo el conjunto de datos de entrenamiento.

### <a name="monte-carlo-cross-validation-aka-repeated-random-sub-sampling"></a>Validación cruzada de Monte Carlo (conocida como submuestra aleatoria repetida)

Use `validation_size` para especificar el porcentaje del conjunto de datos de entrenamiento que se debe usar para la validación y use `n_cross_validations` para especificar el número de validaciones cruzadas. Durante cada ronda de validación cruzada, un subconjunto de tamaño `validation_size` se seleccionará aleatoriamente para la validación del modelo entrenado en los datos restantes. Por último, se notifican las puntuaciones promedias de todas las rondas `n_cross_validations` y el modelo correspondiente se puede volver a entrenar en todo el conjunto de datos de entrenamiento.

### <a name="custom-validation-dataset"></a>Conjunto de datos de validación personalizado

Use un conjunto de datos de validación personalizado si la división aleatoria no es aceptable (normalmente, datos de serie temporal o datos desequilibrados). Puede especificar un conjunto de datos de validación propio. El modelo se evaluará en el conjunto de datos de validación especificado en lugar de un conjunto de datos aleatorio.

## <a name="compute-to-run-experiment"></a>Proceso para ejecutar el experimento

A continuación, determine dónde se va a entrenar el modelo. Un experimento de entrenamiento de aprendizaje automático automatizado se puede ejecutar en las opciones de proceso siguientes:
*   La máquina local, como un escritorio local o un equipo portátil: generalmente, cuando haya un pequeño conjunto de datos y siga en la fase de exploración.
*   Una máquina remota en la nube: [Azure Machine Learning Managed Compute](concept-azure-machine-learning-architecture.md#managed-and-unmanaged-compute-targets) es un servicio administrado que permite entrenar modelos de aprendizaje automático en clústeres de máquinas virtuales de Azure.

Visite el [sitio de GitHub](https://github.com/Azure/MachineLearningNotebooks/tree/master/automl) para obtener cuadernos de ejemplo con destinos de proceso locales y remotos.

<a name='configure-experiment'></a>

## <a name="configure-your-experiment-settings"></a>Establecer la configuración de experimento

Se pueden usar varias opciones para configurar el experimento de aprendizaje automático automatizado. Estos parámetros se establecen al crear una instancia un objeto `AutoMLConfig`.

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

    ```python
    automl_regressor = AutoMLConfig(
        task='regression',
        max_time_sec=600,
        iterations=100,
        primary_metric='r2_score',
        X=X,
        y=y,
        n_cross_validations=5)
    ```

Hay tres valores de parámetro `task` diferentes, que determinan la lista de algoritmos que se aplicará.  Use los parámetros `whitelist` o `blacklist` para modificar aún más las iteraciones con los algoritmos disponibles para incluir o excluir.
* clasificación
    * LogisticRegression
    * SGD
    * MultinomialNaiveBayes
    * BernoulliNaiveBayes
    * SVM
    * LinearSVM
    * KNN
    * DecisionTree
    * RandomForest
    * ExtremeRandomTrees
    * LightGBM
    * GradientBoosting
    * TensorFlowDNN
    * TensorFlowLinearClassifier
* Regresión
    * ElasticNet
    * GradientBoosting
    * DecisionTree
    * KNN
    * LassoLars
    * SGD 
    * RandomForest
    * ExtremeRandomTree
    * LightGBM
    * TensorFlowLinearRegressor
    * TensorFlowDNN
* Previsión
    * ElasticNet
    * GradientBoosting
    * DecisionTree
    * KNN
    * LassoLars
    * SGD 
    * RandomForest
    * ExtremeRandomTree
    * LightGBM
    * TensorFlowLinearRegressor
    * TensorFlowDNN

Consulte la [clase AutoMLConfig](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig.automlconfig?view=azure-ml-py) para obtener una lista completa de parámetros.  

## <a name="data-pre-processing-and-featurization"></a>Preprocesamiento de datos y caracterización

Si usa `preprocess=True`, los pasos de preprocesamiento de datos siguientes se realizan de forma automática:
1.  Se quitan las características de cardinalidad alta o sin variación
    * Las características sin información útil se quitan de los de conjuntos de entrenamiento y validación. Son características en las que faltan todos los valores, que tienen el mismo valor en todas las filas o que tienen una cardinalidad muy alta (por ejemplo, hashes, identificadores o GUID).
1.  Atribución de valores que faltan
    *   Para las características numéricas, se atribuyen los valores que faltan con el promedio de los valores de la columna.
    *   Para las características de categorías, los valores que faltan se atribuyen con el valor más frecuente.
1.  Generación de características adicionales
    * Para las características de fecha y hora: año, mes, día, día de la semana, día del año, trimestre, semana del año, hora, minuto, segundo.
    * Para las características de texto: frecuencia del término en función del unigrama de palabra, bigramas y trigramas, vectorizador de recuento.
1.  Transformaciones y codificaciones
    * Las características numéricas con muy pocos valores únicos se transforman en características de categorías.
    * En función de la cardinalidad de las características de categorías, realice la codificación de etiqueta o la codificación "one-hot" (hash).

## <a name="run-experiment"></a>Ejecutar experimento

Envíe el experimento para ejecutar y generar un modelo. Pase `AutoMLConfig` al método `submit` para generar el modelo.

```python
run = experiment.submit(automl_config, show_output=True)
```

>[!NOTE]
>Las dependencias se instalan por primera vez en una máquina nueva.  El resultado puede tardar hasta 10 minutos en mostrarse.
>Establecer `show_output` en `True` genera un resultado que se muestra en la consola.


## <a name="explore-model-metrics"></a>Explorar las métricas del modelo
Puede ver los resultados en un widget o en línea si se encuentra en un bloc de notas. Vea [Seguimiento y evaluación de modelos](how-to-track-experiments.md#view-run-details) para obtener más información.


### <a name="classification-metrics"></a>Métricas de clasificación
Las métricas siguientes se guardan en cada iteración de una tarea de clasificación.

|Métrica principal|DESCRIPCIÓN|Cálculo|Parámetros adicionales
--|--|--|--|
AUC_Macro| AUC es el área bajo la Curva de característica operativa del receptor. Macro es la media aritmética del parámetro AUC para cada clase.  | [Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="macro"|
AUC_Micro| AUC es el área bajo la Curva de característica operativa del receptor. Micro se calcula de forma global mediante la combinación de los verdaderos positivos y los falsos positivos de cada clase| [Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html) | average="micro"|
AUC_Weighted  | AUC es el área bajo la Curva de característica operativa del receptor. Weighted es la media aritmética de la puntuación para cada clase, ponderada por el número de instancias verdaderas en cada clase.| [Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html)|average="weighted"
accuracy|La precisión es el porcentaje de las etiquetas de predicción que coinciden exactamente con las etiquetas verdaderas. |[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html) |None|
average_precision_score_macro|La precisión promedio resume una curva de precisión-recuperación como la media ponderada de las precisiones conseguidas en cada umbral, donde el aumento de la recuperación del umbral anterior se usa como peso. Macro es la media aritmética de la puntuación de precisión media de cada clase|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="macro"|
average_precision_score_micro|La precisión promedio resume una curva de precisión-recuperación como la media ponderada de las precisiones conseguidas en cada umbral, donde el aumento de la recuperación del umbral anterior se usa como peso. Micro se calcula de forma global mediante la combinación de los verdaderos positivos y los falsos positivos en cada límite|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="micro"|
average_precision_score_weighted|La precisión promedio resume una curva de precisión-recuperación como la media ponderada de las precisiones conseguidas en cada umbral, donde el aumento de la recuperación del umbral anterior se usa como peso. Weighted es la media aritmética de la puntuación de precisión promedio para cada clase, ponderada por el número de instancias verdaderas en cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html)|average="weighted"|
balanced_accuracy|La precisión equilibrada es la media aritmética de recuperación para cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
f1_score_macro|La puntuación F1 es la media armónica de precisión y recuperación. Macro es la media aritmética de la puntuación F1 para cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="macro"|
f1_score_micro|La puntuación F1 es la media armónica de precisión y recuperación. Micro se calcula de forma global mediante el recuento del total de verdaderos positivos, falsos negativos y falsos positivos.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="micro"|
f1_score_weighted|La puntuación F1 es la media armónica de precisión y recuperación. Media ponderada por frecuencia de clase de la puntuación F1 para cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.f1_score.html)|average="weighted"|
log_loss|Se trata de la función de pérdida que se usa en la regresión logística (multinomial) y sus extensiones como las redes neuronales, definida como la verosimilitud logarítmica negativa de las etiquetas verdaderas, dadas las predicciones de un clasificador probabilístico. Para un ejemplo único con la etiqueta verdadera yt en {0,1} y la probabilidad estimada yp de que yt = 1, la pérdida logarítmica es -log P(yt&#124;yp) = -(yt log(yp) + (1 - yt) log(1 - yp))|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)|None|
norm_macro_recall|La recuperación de macro normalizada es la recuperación de macro que se ha normalizado para que el rendimiento aleatorio tenga una puntuación de 0 y el rendimiento perfecto una puntuación de 1. Esto se logra mediante norm_macro_recall := (recall_score_macro - R)/(1 - R), donde R es el valor esperado de recall_score_macro para las predicciones aleatorias (es decir, R=0,5 para clasificación binaria y R=(1/C) para problemas de clasificación de clase C)|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average = "macro" y después (recall_score_macro - R)/(1 - R), donde R es el valor esperado de recall_score_macro para las predicciones aleatorias (es decir, R=0,5 para clasificación binaria y R=(1/C) para problemas de clasificación de clase C)|
precision_score_macro|La precisión es el porcentaje de los elementos etiquetados como una clase determinada que se encuentran realmente en esa clase. Macro es la media aritmética de la precisión para cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="macro"|
precision_score_micro|La precisión es el porcentaje de los elementos etiquetados como una clase determinada que se encuentran realmente en esa clase. Micro se calcula de forma global mediante el recuento del total de verdaderos positivos y falsos positivos.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="micro"|
precision_score_weighted|La precisión es el porcentaje de los elementos etiquetados como una clase determinada que se encuentran realmente en esa clase. Weighted es la media aritmética de la precisión para cada clase, ponderada por el número de instancias verdaderas en cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="weighted"|
recall_score_macro|Recuperación es el porcentaje de elementos que realmente están una clase determinada y que están correctamente etiquetados. Macro es la media aritmética de la recuperación para cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
recall_score_micro|Recuperación es el porcentaje de elementos que realmente están una clase determinada y que están correctamente etiquetados. Micro se calcula de forma global mediante el recuento del total de verdaderos positivos y falsos negativos.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="micro"|
recall_score_weighted|Recuperación es el porcentaje de elementos que realmente están una clase determinada y que están correctamente etiquetados. Weighted es la media aritmética de la recuperación para cada clase, ponderada por el número de instancias verdaderas en cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="weighted"|
weighted_accuracy|La precisión ponderada es la precisión donde el peso asignado a cada ejemplo es igual a la proporción de instancias verdades en la clase real de ese ejemplo.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight es un vector igual a la proporción de esa clase para cada elemento en el destino|

### <a name="regression-and-forecasting-metrics"></a>Métricas de regresión y previsión
Las métricas siguientes se guardan en cada iteración de una tarea de regresión o previsión.

|Métrica principal|DESCRIPCIÓN|Cálculo|Parámetros adicionales
--|--|--|--|
explained_variance|La varianza explicada es la proporción que tiene en cuenta un modelo matemático la variación de un conjunto de datos determinado. Es el porcentaje de reducción de la varianza de los datos originales con respecto a la varianza de los errores. Cuando la media de los errores es 0, es igual a la varianza explicada.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.explained_variance_score.html)|None|
r2_score|R2 es el coeficiente de determinación o el porcentaje de reducción de los errores cuadráticos en comparación con un modelo de referencia que da como resultado la media. Cuando la media de los errores es 0, es igual a la varianza explicada.|[Cálculo](https://scikit-learn.org/0.16/modules/generated/sklearn.metrics.r2_score.html)|None|
spearman_correlation|La correlación de Spearman es una medida no paramétrica de la monotonicidad de la relación entre dos conjuntos de datos. A diferencia de la correlación de Pearson, la correlación de Spearman no asume que los conjuntos de datos se distribuyen normalmente. Como sucede con otros coeficientes de correlación, este varía entre -1 y + 1, y 0 implica que no hay ninguna correlación. Las correlaciones de -1 o +1 implican una relación monotónica exacta. Las correlaciones positivas implican que cuando aumenta x, también lo hace y. Las correlaciones negativas implican que cuando aumenta x, y disminuye.|[Cálculo](https://docs.scipy.org/doc/scipy-0.16.1/reference/generated/scipy.stats.spearmanr.html)|None|
mean_absolute_error|El error absoluto medio es el valor esperado del valor absoluto de la diferencia entre el destino y la predicción.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|None|
normalized_mean_absolute_error|El error medio absoluto normalizado es un error medio absoluto dividido por el intervalo de los datos|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_absolute_error.html)|Se divide por el intervalo de los datos|
median_absolute_error|El error medio absoluto es la media de todas las diferencias absolutas entre el destino y la predicción. Esta pérdida es estable para los valores atípicos.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|None|
normalized_median_absolute_error|El error medio absoluto normalizado es un error medio absoluto dividido por el intervalo de los datos|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.median_absolute_error.html)|Se divide por el intervalo de los datos|
root_mean_squared_error|El error cuadrático medio es la raíz cuadrada de la diferencia cuadrática esperada entre el destino y la predicción.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|None|
normalized_root_mean_squared_error|El error cuadrático medio normalizado es el error cuadrático medio dividido por el intervalo de los datos.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_error.html)|Se divide por el intervalo de los datos|
root_mean_squared_log_error|El error logarítmico cuadrático medio es la raíz cuadrada del error logarítmico cuadrático esperado.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|None|
normalized_root_mean_squared_log_error|El error logarítmico cuadrático medio normalizado es el error logarítmico cuadrático medio dividido por el intervalo de los datos.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_squared_log_error.html)|Se divide por el intervalo de los datos|

## <a name="explain-the-model"></a>Explicación del modelo

Aunque las funciones de aprendizaje automático automatizado están disponibles con carácter general, **la característica de explicación del modelo aún está en versión preliminar pública.**

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

Puede visualizar el gráfico de importancia de características en el área de trabajo de Azure Portal. El gráfico también se muestra al usar el widget de Jupyter en un cuaderno. Para más información sobre los gráficos, consulte el artículo [Cuadernos de ejemplo de Azure Machine Learning Service](samples-notebooks.md).

```python
from azureml.widgets import RunDetails
RunDetails(local_run).show()
```
![Gráfico de importancia de características](./media/how-to-configure-auto-train/feature-importance.png)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [cómo y dónde implementar un modelo](how-to-deploy-and-where.md).

Obtenga más información sobre [cómo entrenar un modelo de regresión con Automated machine learning](tutorial-auto-train-models.md) o [cómo entrenar mediante automatizada de machine learning en un recurso remoto](how-to-auto-train-remote.md).
