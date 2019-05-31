---
title: Selección y optimización automáticas del algoritmo de Machine Learning
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo Azure Machine Learning Service puede elegir de forma automática un algoritmo y generar un modelo a partir de él para ahorrar tiempo, mediante los parámetros y criterios que proporcione con el fin de seleccionar el mejor algoritmo para el modelo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 05/21/2019
ms.custom: seodec18
ms.openlocfilehash: 16d100256f9252b478500488c2dc5a01c7e6a0b5
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66418064"
---
# <a name="what-is-automated-machine-learning"></a>Descripción del aprendizaje automático

Automatizadas aprendizaje automático, también se denomina ML automatizada, es el proceso de automatizar las tareas de tiempo, iterativas de desarrollo de modelos de aprendizaje automático. Permite a los desarrolladores, analistas y científicos de datos crear modelos de aprendizaje automático con alta escala, la eficiencia y productividad al mismo tiempo que mantiene la calidad del modelo.

Desarrollo de modelos de aprendizaje automático tradicional consume muchos recursos, que requieren un conocimiento ni dominio significativo y tiempo para generar y comparar docenas de modelos. Aplicar ML automatizada cuando desee que Azure Machine Learning para entrenar y ajustar un modelo automáticamente con la métrica de destino que especifique. El servicio, a continuación, recorre en iteración los algoritmos de aprendizaje automático que se empareja con selecciones de características, donde cada iteración genera un modelo con una puntuación de aprendizaje. Cuanto mayor sea la puntuación, mejor será el modelo se considera "ajuste" los datos.

Con aprendizaje automático automatizada, a acelerar el tiempo necesario para obtener los modelos de aprendizaje automático listos para producción con eficiencia y gran facilidad.

## <a name="when-to-use-automated-ml"></a>Cuándo utilizar ML automatizada

ML automatizada Democratiza el proceso de desarrollo del modelo de aprendizaje automático y permite a los usuarios, independientemente de su experiencia de ciencia de datos para identificar una canalización de aprendizaje de máquina to-end para cualquier problema.

Los científicos de datos, los analistas y programadores sectores pueden usar Machine Learning automatizada para:

+ Implementar soluciones de aprendizaje automático sin amplios conocimientos de programación
+ Ahorre tiempo y recursos
+ Aproveche las mejores prácticas de ciencia de datos
+ Proporcionar una solución de problemas agile

## <a name="how-automated-ml-works"></a>Funciona de forma automatizada ML

Uso de **servicio Azure Machine Learning**, puede diseñar y ejecutar sus experimentos de aprendizaje automatizados ML con estos pasos:

1. **Identificar el problema de Machine Learning** resolverse: clasificación, previsión o regresión

1. **Especificar el origen y el formato de los datos de entrenamiento etiquetado**: Matrices de Numpy o Pandas dataframe

1. **Configurar el destino de proceso de entrenamiento del modelo**, como su [equipo local, Azure Machine Learning calcula, las máquinas virtuales remotas o Azure Databricks](how-to-set-up-training-targets.md).  Obtenga información sobre aprendizaje automatizado [en un recurso remoto](how-to-auto-train-remote.md).

1. **Configurar los parámetros de aprendizaje de automático automatizado** que determinan cuántas iteraciones a través de diferentes modelos, configuración de hiperparámetros, avanzada y características de preprocesamiento y qué métricas se deben observar al determinar el mejor modelo.  Puede configurar los valores para el experimento de entrenamiento automático [en Azure portal](how-to-create-portal-experiments.md) o [con el SDK de](how-to-configure-auto-train.md).

1. **Enviar la ejecución de aprendizaje.**

  ![Aprendizaje automático automatizado](./media/how-to-automated-ml/automl-concept-diagram2.png)

Durante el entrenamiento, el servicio de Azure Machine Learning crea una serie de en las canalizaciones paralelas que probar los parámetros y algoritmos diferentes. Se detendrá una vez que llega a los criterios de salida definidos en el experimento.

También puede inspeccionar la información de ejecución registrada, que contiene las métricas que se recopilan durante la ejecución. La ejecución de aprendizaje genera un objeto serializado de Python (`.pkl` archivo) que contiene el modelo y preprocesamiento de datos.

Mientras se automatiza la creación del modelo, también puede [conocer cómo importante o relevantes características son](how-to-configure-auto-train.md#explain) para los modelos generados.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Preprocesamiento

En cada experimento de aprendizaje automático automatizada, los datos se preprocesan utilizando los métodos predeterminados y, opcionalmente, a través de opciones avanzadas de preprocesamiento.

### <a name="automatic-preprocessing-standard"></a>Operación automática de preprocesamiento (estándar)

En cada experimento de aprendizaje automático automatizada, los datos automáticamente se escala o normalizados para ayudar a los algoritmos funcionan bien.  Durante el entrenamiento del modelo, una de las siguientes técnicas de escalado o normalización se aplicará a cada modelo.

|Escalado&nbsp;&&nbsp;normalización| DESCRIPCIÓN |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Estandarizar características quitando la media y escalado a la varianza de la unidad  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Transforma las características por cada característica por esa columna como mínimo y máximo de escalado  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Escalar cada característica, su valor absoluto máximo |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Esta características Scaler por su rango intercuartil |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Reducción de dimensionalidad lineal mediante descomposición en valores singulares de los datos al proyecto, a un espacio dimensional inferior |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Esta clase transformer realiza la reducción de dimensionalidad lineal por medio de descomposición en valores singulares truncados (SVD). Al contrario que el PCA, este programa de estimación no centrar los datos antes de calcular la descomposición en valores singulares. Esto significa que pueda trabajar eficazmente con matrices scipy.sparse |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Cada ejemplo (es decir, cada fila de la matriz de datos) con al menos un componente distinto de cero es volver a escala con independencia de otras muestras para que su norma (l1 o l2) es igual a uno |

### <a name="advanced-preprocessing-optional-featurization"></a>Advanced preprocesamiento: características opcionales

Características y preprocesamiento avanzadas adicionales también están disponibles, como la falta de imputación de valores, la codificación y las transformaciones. [Más información sobre qué características se incluye](how-to-create-portal-experiments.md#preprocess). Habilite a esta opción con:

+ Azure Portal: Seleccionar el **preprocesamiento** casilla de verificación en la **configuración avanzada** [con estos pasos](how-to-create-portal-experiments.md).

+ SDK de Python: Especificar `"preprocess": True` para el [ `AutoMLConfig` clase](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).

## <a name="ensemble-models"></a>Los modelos

Puede entrenar los modelos con aprendizaje automático automatizada con la [Caruana algoritmo de selección de conjunto con la inicialización de conjunto ordenada](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf). Aprendizaje conjunto mejora el rendimiento predictivo y los resultados de machine learning mediante la combinación de varios modelos en lugar de usar modelos de únicos. La iteración conjunto aparece como la última iteración de la ejecución.

## <a name="training-metric-output"></a>Resultado de la métrica de entrenamiento

Hay varias maneras para ver las métricas de precisión de entrenamiento para cada iteración de la ejecución.

* Usar el widget de Jupyter.
* Use la `get_metrics()` función en cualquier `Run` objeto.
* Ver las métricas en Azure portal en el experimento.

### <a name="classification-metrics"></a>Métricas de clasificación

Las siguientes métricas se guardan en cada iteración de ejecución para una tarea de clasificación.

|Métrica|DESCRIPCIÓN|Cálculo|Parámetros adicionales
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
norm_macro_recall|La recuperación de macro normalizada es la recuperación de macro que se ha normalizado para que el rendimiento aleatorio tenga una puntuación de 0 y el rendimiento perfecto una puntuación de 1. Esto se logra mediante norm_macro_recall := (recall_score_macro - R)/(1 - R), donde R es el valor esperado de recall_score_macro para las predicciones aleatorias (es decir, R=0,5 para clasificación binaria y R=(1/C) para problemas de clasificación de clase C)|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average = "macro" y después (recall_score_macro - R)/(1 - R), donde R es el valor esperado de recall_score_macro para las predicciones aleatorias (es decir, R=0,5 para clasificación binaria y R=(1/C) para problemas de clasificación de clase C)|
precision_score_macro|La precisión es el porcentaje de los elementos etiquetados como una clase determinada que se encuentran realmente en esa clase. Macro es la media aritmética de la precisión para cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="macro"|
precision_score_micro|La precisión es el porcentaje de los elementos etiquetados como una clase determinada que se encuentran realmente en esa clase. Micro se calcula de forma global mediante el recuento del total de verdaderos positivos y falsos positivos.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="micro"|
precision_score_weighted|La precisión es el porcentaje de los elementos etiquetados como una clase determinada que se encuentran realmente en esa clase. Weighted es la media aritmética de la precisión para cada clase, ponderada por el número de instancias verdaderas en cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html)|average="weighted"|
recall_score_macro|Recuperación es el porcentaje de elementos que realmente están una clase determinada y que están correctamente etiquetados. Macro es la media aritmética de la recuperación para cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="macro"|
recall_score_micro|Recuperación es el porcentaje de elementos que realmente están una clase determinada y que están correctamente etiquetados. Micro se calcula de forma global mediante el recuento del total de verdaderos positivos y falsos negativos.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="micro"|
recall_score_weighted|Recuperación es el porcentaje de elementos que realmente están una clase determinada y que están correctamente etiquetados. Weighted es la media aritmética de la recuperación para cada clase, ponderada por el número de instancias verdaderas en cada clase.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html)|average="weighted"|
weighted_accuracy|La precisión ponderada es la precisión donde el peso asignado a cada ejemplo es igual a la proporción de instancias verdades en la clase real de ese ejemplo.|[Cálculo](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)|sample_weight es un vector igual a la proporción de esa clase para cada elemento en el destino|

### <a name="regression-and-forecasting-metrics"></a>Métricas de regresión y previsión

Las siguientes métricas se guardan en cada iteración de ejecución para una tarea de predicción o regresión.

|Métrica|DESCRIPCIÓN|Cálculo|Parámetros adicionales
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


## <a name="use-with-onnx-in-c-apps"></a>Uso con ONNX en C# aplicaciones

Con Azure Machine Learning, puede usar para generar un modelo de Python y convertirlo al formato ONNX ML automatizada. Es compatible con el tiempo de ejecución ONNX C#, por lo que puede usar el modelo creado automáticamente en su C# aplicaciones sin necesidad de volver a codificar o cualquiera de las latencias de red que presentan los extremos REST. Pruebe un ejemplo de este flujo [en este cuaderno de Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>ML automatizada a través de Microsoft

ML automatizada también está disponible en otras soluciones de Microsoft, como:

|Integraciones|DESCRIPCIÓN
|------------|-----------
|ML.NET|Selección del modelo automático y entrenamiento en aplicaciones de .NET con Visual Studio y Visual Studio Code con ML.NET automatizadas Machine Learning (versión preliminar). [Más información](https://docs.microsoft.com/dotnet/machine-learning/automl-overview).
|HDIsnight|Escalar horizontalmente sus trabajos de aprendizaje de ML automatizados en Spark en clústeres de HDInsight en paralelo. [Más información](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md).
|PowerBI|Invocación de los modelos de aprendizaje automático directamente en Power BI (versión preliminar). [Más información](https://docs.microsoft.com/power-bi/service-machine-learning-automated).

## <a name="next-steps"></a>Pasos siguientes

Vea ejemplos y aprenda cómo generar modelos mediante Automated Machine Learning:

+ Siga el [Tutorial: Automatically train a classification model with Azure Automated Machine Learning](tutorial-auto-train-models.md) (Tutorial: Entrenamiento automático de un modelo de clasificación con Azure Machine Learning automatizado)

+ Configurar el experimento de aprendizaje automático:
  + En la interfaz de portal de Azure, [siga estos pasos](how-to-create-portal-experiments.md).
  + Con el SDK de Python, [siga estos pasos](how-to-configure-auto-train.md).

+ Obtenga información sobre cómo auto entrenar con datos de series temporales, [siga estos pasos](how-to-auto-train-forecast.md).

+ Pruebe [ejemplos de Jupyter Notebook](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/)
