---
title: Descripción del aprendizaje automático o AutoML
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo Azure Machine Learning Service puede elegir de forma automática un algoritmo y generar un modelo a partir de él para ahorrar tiempo, mediante los parámetros y criterios que proporcione con el fin de seleccionar el mejor algoritmo para el modelo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: nacharya1
ms.author: nilesha
ms.date: 06/20/2019
ms.custom: seodec18
ms.openlocfilehash: 3fb4d953f575dcf6582f9f1c8c2a725971134159
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68856250"
---
# <a name="what-is-automated-machine-learning"></a>Descripción del aprendizaje automático

El aprendizaje automático automatizado, también denominado autoML, es el proceso de automatizar las tareas lentas y repetitivas del desarrollo de modelos de Machine Learning. Permite que los desarrolladores, analistas y científicos de datos creen modelos de aprendizaje automático con un escalado, eficiencia y productividad altos, al mismo tiempo que mantiene la calidad del modelo.

El desarrollo de modelos de Machine Learning tradicional consume muchos recursos, que requieren un conocimiento del dominio y tiempo significativos para generar y comparar docenas de modelos. Aplique aprendizaje automático automatizado cuando quiera que Azure Machine Learning entrene y ajuste un modelo automáticamente con la métrica de destino que especifique. A continuación, el servicio recorre en iteración los algoritmos de aprendizaje automático que corresponden con las selecciones de características, de forma que cada iteración genera un modelo con una puntuación de entrenamiento. Cuanto mayor sea la puntuación, mejor se "ajustará" el modelo a sus datos.

Gracias al aprendizaje automático automatizado, reducirá el tiempo necesario para obtener modelos de aprendizaje automático listos para producción con gran eficiencia y facilidad.

## <a name="when-to-use-automated-ml"></a>Cuándo usar aprendizaje automático automatizado

El aprendizaje automático automatizado democratiza el proceso de desarrollo del modelo de Machine Learning y permite que los usuarios (independientemente de su experiencia en ciencia de datos) identifiquen una canalización de aprendizaje automático completa para cualquier problema.

Los científicos de datos, analistas y desarrolladores de todos los sectores pueden usar el aprendizaje automático para:

+ Implementar soluciones de aprendizaje automático sin conocimientos amplios de programación.
+ Ahorrar tiempo y recursos.
+ Aprovechar los procedimientos recomendados de la ciencia de datos.
+ Proporcionar soluciones ágiles a los problemas.

## <a name="how-automated-ml-works"></a>Funcionamiento del aprendizaje automático

Si usa **Azure Machine Learning Service**, puede diseñar y ejecutar sus experimentos de entrenamiento de Machine Learning automatizado mediante los siguientes pasos:

1. **Identifique el problema de Machine Learning** que quiere solucionar: clasificación, previsión o regresión.

1. **Especifique el origen y el formato de los datos de entrenamiento etiquetados**: matrices de Numpy o dataframes de Pandas.

1. **Configure el destino de proceso para el entrenamiento del modelo**, puede ser [un equipo local, los procesos de Azure Machine Learning, las máquinas virtuales remotas o Azure Databricks](how-to-set-up-training-targets.md).  Obtenga información sobre el entrenamiento automático [en recursos remotos](how-to-auto-train-remote.md).

1. **Configure los parámetros de aprendizaje de automático automatizado** que determinan el número de iteraciones en diferentes modelos, las configuraciones de hiperparámetros, la caracterización y preprocesamiento de datos y las métricas que se deben observar para seleccionar al mejor modelo.  Puede configurar los valores para el experimento de entrenamiento automático [en Azure Portal](how-to-create-portal-experiments.md) o [con el SDK](how-to-configure-auto-train.md).

1. **Envíe la ejecución del entrenamiento.**

  ![Aprendizaje automático automatizado](./media/how-to-automated-ml/automl-concept-diagram2.png)

Durante el entrenamiento, Azure Machine Learning Service crea una serie de canalizaciones en paralelo que prueban distintos parámetros y algoritmos. Se detendrá una vez que logre los criterios de salida definidos en el experimento.

También puede inspeccionar la información de ejecución registrada, que [contiene las métricas](how-to-understand-automated-ml.md) que se recopilan durante la ejecución. La ejecución del entrenamiento genera un objeto serializado de Python (archivo `.pkl`) que contiene el modelo y el preprocesamiento de los datos.

Aunque se automatiza la creación del modelo, también puede [conocer las características importantes o pertinentes](how-to-configure-auto-train.md#explain) de los modelos generados.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Preprocesamiento

En todos los experimentos de aprendizaje automático automatizado, los datos se preprocesan mediante los métodos predeterminados y, opcionalmente, a través de opciones avanzadas de preprocesamiento.

> [!NOTE]
> Los pasos previos al procesamiento del aprendizaje automático (normalización de características, control de los datos que faltan, conversión de valores de texto a numéricos, etc.) se convierten en parte del modelo subyacente. Cuando se utiliza el modelo para las predicciones, se aplican automáticamente a los datos de entrada los mismos pasos previos al procesamiento que se aplican durante el entrenamiento.

### <a name="automatic-preprocessing-standard"></a>Preprocesamiento automático (estándar)

En todos los experimentos de aprendizaje automático automatizado, los datos se escalan y se normalizan automáticamente para ayudar a que los algoritmos funcionen bien.  Durante el entrenamiento del modelo, se aplicará una de las siguientes técnicas de escalado o normalización para todos los modelos.

|Escalado&nbsp;y&nbsp;normalización| DESCRIPCIÓN |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Estandariza las características al quitar la media y realizar un escalado según la varianza de la unidad.  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Transforma las características al escalar cada una según el mínimo y máximo de esa columna.  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Escala cada característica según su valor absoluto máximo. |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Escala las características según su rango intercuartil. |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Reducción de dimensionalidad lineal mediante la descomposición en valores singulares de los datos a fin de proyectarlos en un espacio dimensional inferior. |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Este convertidor realiza la reducción de dimensionalidad lineal por medio de la descomposición en valores singulares truncados (SVD). Al contrario que PCA, este programa de estimación no centra los datos antes de calcular la descomposición en valores singulares. Es decir que puede trabajar eficazmente con matrices scipy.sparse. |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Cada muestra (es decir, cada fila de la matriz de datos) con al menos un componente distinto de cero vuelve a escalarse independientemente de las demás muestras, de modo que su norma vectorial (l1 o l2) sea igual a uno. |

### <a name="advanced-preprocessing-optional-featurization"></a>Preprocesamiento avanzado: caracterización opcional

También están disponibles operaciones de preprocesamiento o caracterización avanzadas adicionales, como la atribución de valores faltantes, la codificación y las transformaciones. [Más información sobre qué caracterización se incluye](how-to-create-portal-experiments.md#preprocess). Para habilitar esta configuración, realice lo siguiente:

+ Azure Portal: seleccione la casilla de verificación **Preprocess** (preprocesamiento) en **Configuración avanzada** [con estos pasos](how-to-create-portal-experiments.md).

+ SDK de Python: Especifique `"preprocess": True` para la [clase `AutoMLConfig`](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig?view=azure-ml-py).


## <a name="time-series-forecasting"></a>Previsión de series temporales
La creación de previsiones es una parte integral de cualquier empresa, ya sea a petición de los ingresos, inventarios, ventas o clientes. Puede usar el aprendizaje automático automatizado para combinar las técnicas y enfoques y obtener una previsión recomendada y de alta calidad de series temporales.

Un experimento automatizado de series temporales se trata como un problema de regresión multivariante. Los valores de series temporales anteriores se "dinamizan" para convertirse en dimensiones adicionales para el regresor junto con otros indicadores. Este enfoque, a diferencia de los métodos clásicos de series temporales, tiene la ventaja de incorporar de forma natural varias variables contextuales y su relación entre sí durante el entrenamiento. El aprendizaje automático automatizado aprende un modelo único (a menudo, internamente bifurcado) para todos los elementos en el conjunto de datos y horizontes de predicción. Por tanto, hay más datos disponibles para calcular los parámetros del modelo, y se hace posible la generalización hasta series totalmente nuevas.

Obtenga más información y vea un ejemplo de [aprendizaje automático automatizado para la predicción de series temporales](how-to-auto-train-forecast.md).

## <a name="ensemble"></a> Modelos de conjunto

El aprendizaje automático automatizado admite modelos de conjunto, que están habilitados de forma predeterminada. El aprendizaje de conjunto mejora los resultados del aprendizaje automático y su rendimiento predictivo mediante la combinación de varios modelos en lugar de usar modelos únicos. Las iteraciones de conjunto aparecen como las iteraciones finales de la ejecución. El aprendizaje automático automatizado usa los métodos de conjunto de votaciones y apilamiento para combinar modelos:

* **Votación**: realiza la predicción en función de la media ponderada de las probabilidades de clase predichas (para tareas de clasificación) o de los destinos de regresión predichos (para tareas de regresión).
* **Apilamiento**: el apilamiento combina modelos heterogéneos y entrena un metamodelo basado en la salida de los modelos individuales. Los metamodelos predeterminados actuales son LogisticRegression para las tareas de clasificación y ElasticNet para las tareas de regresión y previsión.

El [algoritmo de selección de conjunto de Caruana](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf) con inicialización de conjunto ordenado se utiliza para decidir qué modelos se van a utilizar en el conjunto. En un nivel alto, este algoritmo inicializa el conjunto con hasta cinco modelos con las mejores puntuaciones individuales y comprueba que estos modelos se encuentran en un umbral del 5 % de la mejor puntuación para evitar un conjunto inicial deficiente. A continuación, para cada iteración de conjunto, se agrega un nuevo modelo al conjunto existente y se calcula la puntuación resultante. Si un nuevo modelo ha mejorado la puntuación de conjunto existente, el conjunto se actualiza para incluir el nuevo modelo.

Consulte el [procedimiento](how-to-configure-auto-train.md#ensemble) para cambiar la configuración del conjunto predeterminado en el aprendizaje automático automatizado.

## <a name="use-with-onnx-in-c-apps"></a>Uso con ONNX en aplicaciones de C#

Con Azure Machine Learning, puede usar Machine Learning para generar un modelo de Python y convertirlo al formato ONNX. El entorno de ejecución de ONNX es compatible con C#, por lo que puede usar el modelo creado automáticamente en sus aplicaciones de C# sin necesidad de volver a codificar o experimentar alguna de las latencias de red que presentan los puntos de conexión REST. Pruebe un ejemplo de este flujo [en este cuaderno de Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-with-onnx/auto-ml-classification-with-onnx.ipynb).

## <a name="automated-ml-across-microsoft"></a>Aprendizaje automático automatizado en Microsoft

El aprendizaje automático automatizado también está disponible en otras soluciones de Microsoft, como:

|Integraciones|DESCRIPCIÓN|
|------------|-----------|
|[ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview)|Selección y entrenamiento del modelo automático en aplicaciones de .NET mediante Visual Studio y Visual Studio Code con aprendizaje automático (versión preliminar) de ML.NET.|
|[HDInsight](../../hdinsight/spark/apache-spark-run-machine-learning-automl.md)|Escalado horizontal de trabajos de entrenamiento de aprendizaje automático en Spark en clústeres de HDInsight en paralelo.|
|[Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated)|Invocación de modelos de Machine Learning directamente en Power BI (versión preliminar).|
|[SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)|Creación de nuevos modelos de Machine Learning a través de los datos en clústeres de macrodatos de SQL Server 2019.|

## <a name="next-steps"></a>Pasos siguientes

Vea ejemplos y aprenda cómo generar modelos mediante aprendizaje automático automatizado:

+ Lea el [Tutorial: entrenamiento automático de un modelo de regresión con Azure Machine Learning automatizado](tutorial-auto-train-models.md).

+ Configure el experimento de entrenamiento automático:
  + Desde la interfaz de Azure Portal, [siga los siguientes pasos](how-to-create-portal-experiments.md).
  + Con el SDK de Python, [siga estos pasos](how-to-configure-auto-train.md).

+ Obtenga información sobre cómo realizar entrenamientos automáticos con datos de series temporales [con estos pasos](how-to-auto-train-forecast.md).

+ Pruebe los [ejemplos de Jupyter Notebook para aprendizaje automático automatizado](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/).
