---
title: Qué es el aprendizaje automático automatizado / ML automatizado
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo Azure Machine Learning puede elegir de forma automática un algoritmo y generar un modelo a partir de él para ahorrar tiempo, mediante los parámetros y criterios que proporcione con el fin de seleccionar el mejor algoritmo para el modelo.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
author: cartacioS
ms.author: sacartac
ms.date: 11/04/2019
ms.openlocfilehash: 2ef3cbc83cd67647709a53fee2c32b444c5d86f4
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2020
ms.locfileid: "77526134"
---
# <a name="what-is-automated-machine-learning"></a>Descripción del aprendizaje automático

El aprendizaje automático automatizado, también denominado ML automatizado, es el proceso de automatizar las tareas lentas e iterativas del desarrollo de modelos de Machine Learning. Permite que los desarrolladores, analistas y científicos de datos creen modelos de aprendizaje automático con un escalado, eficiencia y productividad altos, al mismo tiempo que mantiene la calidad del modelo. El aprendizaje automático se basa en un adelanto de nuestra [división de investigación de Microsoft](https://arxiv.org/abs/1705.05355).

El desarrollo de modelos de Machine Learning tradicional consume muchos recursos, que requieren un conocimiento del dominio y tiempo significativos para generar y comparar docenas de modelos. Aplique aprendizaje automático automatizado cuando quiera que Azure Machine Learning entrene y ajuste un modelo automáticamente con la métrica de destino que especifique. A continuación, el servicio recorre en iteración los algoritmos de aprendizaje automático que corresponden con las selecciones de características, de forma que cada iteración genera un modelo con una puntuación de entrenamiento. Cuanto mayor sea la puntuación, mejor se "ajustará" el modelo a sus datos.

Gracias al aprendizaje automático automatizado, reducirá el tiempo necesario para obtener modelos de aprendizaje automático listos para producción con gran eficiencia y facilidad.

## <a name="when-to-use-automated-ml"></a>Cuándo usar aprendizaje automático automatizado

El aprendizaje automático automatizado democratiza el proceso de desarrollo del modelo de Machine Learning y permite que los usuarios (independientemente de su experiencia en ciencia de datos) identifiquen una canalización de aprendizaje automático completa para cualquier problema.

Los científicos de datos, analistas y desarrolladores de todos los sectores pueden usar el aprendizaje automático para:

+ Implementar soluciones de aprendizaje automático sin conocimientos amplios de programación.
+ Ahorrar tiempo y recursos.
+ Aprovechar los procedimientos recomendados de la ciencia de datos.
+ Proporcionar soluciones ágiles a los problemas.

En la tabla siguiente se enumeran los casos de uso comunes de aprendizaje automático automatizado. 

clasificación| Previsión de series temporales | Regresión
---|---|---
[Detección de fraudes](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-credit-card-fraud/auto-ml-classification-credit-card-fraud.ipynb)|[Previsión de ventas](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-orange-juice-sales/auto-ml-forecasting-orange-juice-sales.ipynb)|[Predicción del rendimiento de la CPU](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/regression-hardware-performance-explanation-and-featurization/auto-ml-regression-hardware-performance-explanation-and-featurization.ipynb)
|[Predicción de marketing](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb)|[Previsión de la demanda](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb)|
|[Clasificación de datos de grupos de discusión](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-text-dnn/auto-ml-classification-text-dnn.ipynb)|[Previsión de producción de bebidas](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-beer-remote/auto-ml-forecasting-beer-remote.ipynb)|

## <a name="how-automated-ml-works"></a>Funcionamiento del aprendizaje automático

Si usa **Azure Machine Learning**, puede diseñar y ejecutar sus experimentos de entrenamiento de Machine Learning automatizado mediante los siguientes pasos:

1. **Identifique el problema de Machine Learning** que quiere solucionar: clasificación, previsión o regresión.

1. **Especifique el origen y el formato de los datos de entrenamiento etiquetados**: matrices de Numpy o dataframes de Pandas.

1. **Configure el destino de proceso para el entrenamiento del modelo**, puede ser [un equipo local, los procesos de Azure Machine Learning, las máquinas virtuales remotas o Azure Databricks](how-to-set-up-training-targets.md).  Obtenga información sobre el entrenamiento automático [en recursos remotos](how-to-auto-train-remote.md).

1. **Configure los parámetros de aprendizaje de automático automatizado** que determinan el número de iteraciones en diferentes modelos, las configuraciones de hiperparámetros, la caracterización y preprocesamiento de datos y las métricas que se deben observar para seleccionar al mejor modelo.  Puede configurar los valores para el experimento de entrenamiento automático en [Azure Machine Learning Studio](https://ml.azure.com) o [con el SDK](how-to-configure-auto-train.md). 

    [!INCLUDE [aml-applies-to-enterprise-sku](../../includes/aml-applies-to-enterprise-sku-inline.md)]

1. **Envíe la ejecución del entrenamiento.**

  ![Aprendizaje automático automatizado](./media/concept-automated-ml/automl-concept-diagram2.png)

Durante el entrenamiento, Azure Machine Learning crea una serie de canalizaciones en paralelo que prueban distintos parámetros y algoritmos. Se detendrá una vez que logre los criterios de salida definidos en el experimento.

También puede inspeccionar la información de ejecución registrada, que [contiene las métricas](how-to-understand-automated-ml.md) que se recopilan durante la ejecución. La ejecución del entrenamiento genera un objeto serializado de Python (archivo `.pkl`) que contiene el modelo y el preprocesamiento de los datos.

Aunque se automatiza la creación del modelo, también puede [conocer las características importantes o pertinentes](how-to-configure-auto-train.md#explain) de los modelos generados.

> [!VIDEO https://www.microsoft.com/videoplayer/embed/RE2Xc9t]

<a name="preprocess"></a>

## <a name="preprocessing"></a>Preprocessing (Preprocesamiento)

En todos los experimentos de aprendizaje automático automatizado, los datos se preprocesan mediante los métodos predeterminados y, opcionalmente, a través de opciones avanzadas de preprocesamiento.

> [!NOTE]
> Los pasos previos al procesamiento del aprendizaje automático (normalización de características, control de los datos que faltan, conversión de valores de texto a numéricos, etc.) se convierten en parte del modelo subyacente. Cuando se utiliza el modelo para las predicciones, se aplican automáticamente a los datos de entrada los mismos pasos previos al procesamiento que se aplican durante el entrenamiento.

### <a name="automatic-preprocessing-standard"></a>Preprocesamiento automático (estándar)

En todos los experimentos de aprendizaje automático automatizado, los datos se escalan y se normalizan automáticamente para ayudar a que los algoritmos funcionen bien.  Durante el entrenamiento del modelo, se aplicará una de las siguientes técnicas de escalado o normalización para todos los modelos.

|Escalado&nbsp;y&nbsp;normalización| Descripción |
| ------------- | ------------- |
| [StandardScaleWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)  | Estandariza las características al quitar la media y realizar un escalado según la varianza de la unidad.  |
| [MinMaxScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)  | Transforma las características al escalar cada una según el mínimo y máximo de esa columna.  |
| [MaxAbsScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MaxAbsScaler.html#sklearn.preprocessing.MaxAbsScaler) |Escala cada característica según su valor absoluto máximo. |
| [RobustScalar](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.RobustScaler.html) |Escala las características según su rango intercuartil. |
| [PCA](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.PCA.html) |Reducción de dimensionalidad lineal mediante la descomposición en valores singulares de los datos a fin de proyectarlos en un espacio dimensional inferior. |
| [TruncatedSVDWrapper](https://scikit-learn.org/stable/modules/generated/sklearn.decomposition.TruncatedSVD.html) |Este convertidor realiza la reducción de dimensionalidad lineal por medio de la descomposición en valores singulares truncados (SVD). Al contrario que PCA, este programa de estimación no centra los datos antes de calcular la descomposición en valores singulares, lo que significa que puede trabajar con matrices scipy.sparse de forma eficaz. |
| [SparseNormalizer](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html) | Cada muestra (es decir, cada fila de la matriz de datos) con al menos un componente distinto de cero vuelve a escalarse independientemente de las demás muestras, de modo que su norma vectorial (l1 o l2) sea igual a uno. |

### <a name="advanced-preprocessing-optional-featurization"></a>Preprocesamiento avanzado: caracterización opcional

También están disponibles operaciones de preprocesamiento o caracterización avanzadas adicionales, como los límites de protección de datos, la codificación y las transformaciones. [Más información sobre qué caracterización se incluye](how-to-create-portal-experiments.md#featurization). Para habilitar esta configuración, realice lo siguiente:

+ Azure Machine Learning Studio: Habilite **Automatic featurization** (Características automáticas) en la sección **View additional configuration** (Ver configuración adicional) siguiendo [estos pasos](how-to-create-portal-experiments.md#create-and-run-experiment).

+ SDK de Python: Especifique `"feauturization": 'auto' / 'off' / 'FeaturizationConfig'` para la [clase `AutoMLConfig`](/python/api/azureml-train-automl-client/azureml.train.automl.automlconfig.automlconfig). 

## <a name="prevent-over-fitting"></a>Prevención del sobreajuste

El sobreajuste en el aprendizaje automático se produce cuando un modelo se ajusta demasiado bien a los datos de entrenamiento y, como resultado, no puede predecir con precisión sobre datos de prueba no vistos. En otras palabras, el modelo ha memorizado simplemente patrones y ruido específicos de los datos de entrenamiento, pero no es lo suficientemente flexible como para realizar predicciones sobre datos reales. En los casos más notorios, un modelo sobreajustado supondrá que las combinaciones de valores de características que se han visto durante el entrenamiento siempre darán exactamente la misma salida para el destino. 

La mejor manera de evitar el sobreajuste es seguir los procedimientos recomendados de ML, entre los que se incluyen:

* Usar más datos de entrenamiento y eliminar el sesgo estadístico
* Evitar pérdidas de destino
* Usar menos características
* **Regularización y optimización de hiperparámetros**
* **Limitaciones de la complejidad del modelo**
* **Validación cruzada**

En el contexto del aprendizaje automático automatizado, los tres primeros elementos anteriores son **procedimientos recomendados que usted implementa**. Los tres últimos elementos en negrita son **procedimientos recomendados que el aprendizaje automático automatizado implementa** de forma predeterminada para protegerse frente al sobreajuste. En configuraciones distintas al aprendizaje automático automatizado, es conveniente seguir los seis procedimientos recomendados para evitar modelos con sobreajuste.

### <a name="best-practices-you-implement"></a>Procedimientos recomendados que usted implementa

El uso de **más datos** es la forma más sencilla y óptima de evitar el sobreajuste y, como ventaja adicional, suele aumentar la precisión. Cuando se usan más datos, resulta más difícil para el modelo memorizar patrones exactos y se ve obligado a obtener soluciones que son más flexibles para acomodar más condiciones. También es importante reconocer el **sesgo estadístico**, para asegurarse de que los datos de entrenamiento no incluyen patrones aislados que no existen en los datos de predicción en directo. Este escenario puede ser difícil de resolver, ya que es posible que no haya sobreajuste entre los conjuntos de entrenamiento y pruebas, pero puede haber sobreajuste cuando se comparan con los datos de pruebas en directo.

La pérdida de destino es un problema similar, en el que es posible que no vea sobreajuste entre los conjuntos de entrenamiento y pruebas, sino que aparece en tiempo de predicción. La pérdida de destino se produce cuando el modelo "hace trampa" durante el entrenamiento al tener acceso a datos que normalmente no debería tener en el momento de la predicción. Por ejemplo, si el problema es predecir el lunes cuál será el precio el viernes, pero una de sus características incluye accidentalmente datos del jueves, que serían datos que el modelo no tendrá en el tiempo de predicción, ya que no puede ver el futuro. La pérdida de destino es un error sencillo de evitar, pero a menudo se caracteriza por una precisión anormalmente alta para el problema. Si intenta predecir el precio de las acciones y ha entrenado un modelo con una precisión del 95 %, es probable que se produzcan pérdidas de destino en alguna parte de las características.

La eliminación de características también puede ayudar con el sobreajuste, ya que impide que el modelo tenga demasiados campos para memorizar patrones específicos, lo que hace que sea más flexible. Puede ser difícil de medir cuantitativamente, pero si puede eliminar características y conservar la misma precisión, probablemente haya hecho que el modelo sea más flexible y haya reducido el riesgo de sobreajuste.

### <a name="best-practices-automated-ml-implements"></a>Procedimientos recomendados que implementa el aprendizaje automático automatizado

La regularización es el proceso de minimizar una función de costo para penalizar modelos complejos y sobreajustados. Hay diferentes tipos de funciones de regularización, pero en general todas penalizan el tamaño del coeficiente del modelo, la varianza y la complejidad. El aprendizaje automático automatizado usa L1 (Lasso), L2 (Ridge) y ElasticNet (L1 y L2 simultáneamente) en combinaciones diferentes con diferentes configuraciones de hiperparámetros del modelo que controlan el sobreajuste. En términos simples, el aprendizaje automático automatizado variará cuánto se regula un modelo y elegirá el mejor resultado.

El aprendizaje automático automatizado también implementa limitaciones de complejidad del modelo explícitas para evitar el sobreajuste. En la mayoría de los casos, esta implementación se aplica específicamente a árboles de decisión o algoritmos de bosque, donde la profundidad máxima del árbol individual está limitada y el número total de árboles usados en el bosque o las técnicas conjunto están limitados.

La validación cruzada (CV) es el proceso de tomar muchos subconjuntos de los datos de entrenamiento completos y entrenar un modelo en cada subconjunto. La idea es que un modelo podría tener "suerte" y tener una gran precisión con un subconjunto, pero al usar muchos subconjuntos el modelo no alcanzará esta alta precisión cada vez. Al realizar la CV, se proporciona un conjunto de datos de exclusión de la validación, se especifican los pliegues de la CV (número de subconjuntos) y el aprendizaje automático automatizado entrena el modelo y ajusta los hiperparámetros para minimizar el error en el conjunto de validación. Un plegamiento de la CV podría estar sobreajustado, pero si se usan muchos de ellos, se reduce la probabilidad de que el modelo final esté sobreajustado. La contrapartida es que la CV genera tiempos de entrenamiento más largos y, por lo tanto, un costo mayor, porque en lugar de entrenar un modelo una vez, se entrena una vez por cada *n* subconjuntos de CV.

> [!NOTE]
> La validación cruzada no está habilitada de forma predeterminada; se debe configurar en la configuración de aprendizaje automático automatizado. Sin embargo, una vez configurada la CV y proporcionado un conjunto de datos de validación, el proceso es automático.

### <a name="identifying-over-fitting"></a>Identificar el sobreajuste

Examine los siguientes modelos entrenados y sus correspondientes precisiones de entrenamiento y prueba.

| Modelo | Precisión del entrenamiento | Precisión de la prueba |
|-------|----------------|---------------|
| Un | 99,9 % | 95 % |
| B | 87 % | 87 % |
| C | 99,9 % | 45 % |

Si observa el modelo **A**, hay una idea equivocada habitual de que si la precisión de la prueba en los datos no vistos es inferior a la precisión del entrenamiento, el modelo está sobreajustado. Sin embargo, la precisión de la prueba siempre debe ser menor que la precisión del entrenamiento y la distinción entre el sobreajuste y el ajuste adecuado se convierte en *cuánto* menos preciso. 

Al comparar los modelos **A** y **B**, el modelo **A** es un modelo mejor porque tiene mayor precisión en la prueba y, aunque la precisión de la prueba es ligeramente inferior al 95 %, no es una diferencia significativa que sugiera que existe sobreajuste. No elegiría el modelo **B** simplemente porque las precisiones del entrenamiento y de la prueba están más próximas.

El modelo **C** representa un caso claro de sobreajuste; la precisión del entrenamiento es muy alta, pero la precisión de la prueba no es tan alta. Esta distinción es subjetiva, pero proviene del conocimiento del problema y los datos y de las magnitudes de error que son aceptables. 

## <a name="time-series-forecasting"></a>Previsión de series temporales

La creación de previsiones es una parte integral de cualquier empresa, ya sea a petición de los ingresos, inventarios, ventas o clientes. Puede usar el aprendizaje automático automatizado para combinar las técnicas y enfoques y obtener una previsión recomendada y de alta calidad de series temporales.

Un experimento automatizado de series temporales se trata como un problema de regresión multivariante. Los valores de series temporales anteriores se "dinamizan" para convertirse en dimensiones adicionales para el regresor junto con otros indicadores. Este enfoque, a diferencia de los métodos clásicos de series temporales, tiene la ventaja de incorporar de forma natural varias variables contextuales y su relación entre sí durante el entrenamiento. El aprendizaje automático automatizado aprende un modelo único (a menudo, internamente bifurcado) para todos los elementos en el conjunto de datos y horizontes de predicción. Por tanto, hay más datos disponibles para calcular los parámetros del modelo, y se hace posible la generalización hasta series totalmente nuevas.

Obtenga más información y vea un ejemplo de [aprendizaje automático automatizado para la predicción de series temporales](how-to-auto-train-forecast.md). O bien, consulte el [cuaderno de demanda energética](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/forecasting-energy-demand/auto-ml-forecasting-energy-demand.ipynb) para obtener ejemplos de código detallados de la configuración de predicciones avanzada, que incluye:

* detección y caracterización de festividades
* aprendizaje de series temporales y DNN (ARIMA, Prophet, ForecastTCN)
* compatibilidad con gran cantidad de modelos a través de la agrupación
* validación cruzada de origen variable
* retardos configurables
* características de agregado en periodos acumulados

## <a name="ensemble"></a> Modelos de conjunto

El aprendizaje automático automatizado admite modelos de conjunto, que están habilitados de forma predeterminada. El aprendizaje de conjunto mejora los resultados del aprendizaje automático y su rendimiento predictivo mediante la combinación de varios modelos en lugar de usar modelos únicos. Las iteraciones de conjunto aparecen como las iteraciones finales de la ejecución. El aprendizaje automático automatizado usa los métodos de conjunto de votaciones y apilamiento para combinar modelos:

* **Votación**: realiza la predicción en función de la media ponderada de las probabilidades de clase predichas (para tareas de clasificación) o de los destinos de regresión predichos (para tareas de regresión).
* **Apilamiento**: el apilamiento combina modelos heterogéneos y entrena un metamodelo basado en la salida de los modelos individuales. Los metamodelos predeterminados actuales son LogisticRegression para las tareas de clasificación y ElasticNet para las tareas de regresión y previsión.

El [algoritmo de selección de conjunto de Caruana](http://www.niculescu-mizil.org/papers/shotgun.icml04.revised.rev2.pdf) con inicialización de conjunto ordenado se utiliza para decidir qué modelos se van a utilizar en el conjunto. En un nivel alto, este algoritmo inicializa el conjunto con hasta cinco modelos con las mejores puntuaciones individuales y comprueba que estos modelos se encuentran en un umbral del 5 % de la mejor puntuación para evitar un conjunto inicial deficiente. A continuación, para cada iteración de conjunto, se agrega un nuevo modelo al conjunto existente y se calcula la puntuación resultante. Si un nuevo modelo ha mejorado la puntuación de conjunto existente, el conjunto se actualiza para incluir el nuevo modelo.

Consulte el [procedimiento](how-to-configure-auto-train.md#ensemble) para cambiar la configuración del conjunto predeterminado en el aprendizaje automático automatizado.

## <a name="imbalance"></a> Datos desequilibrados

Los datos desequilibrados suelen encontrarse en los datos de los escenarios de clasificación de aprendizaje automático y hacen referencia a los datos que contienen una relación desproporcionada de observaciones en cada clase. Este desequilibrio puede dar lugar a un efecto positivo percibido de manera falsa de la precisión de un modelo, ya que los datos de entrada tienen un sesgo hacia una clase, lo que da lugar a que el modelo entrenado imite ese sesgo. 

Como parte de su objetivo de simplificar el flujo de trabajo del aprendizaje automático, el ML automatizado tiene funcionalidades integradas que ayudan a tratar con datos desequilibrados como, por ejemplo: 

- Una **columna de peso**: el ML automatizado admite una columna de peso como entrada, lo que provoca que las filas de los datos se puedan subir o bajar, lo que hace que una clase sea más o menos "importante".

- Los algoritmos que usa el ML automatizado pueden controlar correctamente el desequilibrio de hasta 20:1, lo que significa que la clase más común puede tener 20 veces más filas en los datos que la clase menos común.

### <a name="identify-models-with-imbalanced-data"></a>Identificación de modelos con datos desequilibrados

Como la precisión de los algoritmos de clasificación se evalúan habitualmente, comprobar la puntuación de precisión de un modelo es una buena manera de identificar si los datos desequilibrados lo afectaron. ¿Tiene una precisión realmente alta o realmente baja para ciertas clases?

Además, las ejecuciones de ML automatizado generan automáticamente los gráficos siguientes, que pueden ayudarlo a entender la corrección de las clasificaciones del modelo e identificar los modelos potencialmente afectados por los datos desequilibrados.

Gráfico| Descripción
---|---
[Matriz de confusión](how-to-understand-automated-ml.md#confusion-matrix)| Evalúa las etiquetas clasificadas correctamente con respecto a las etiquetas reales de los datos. 
[Precisión-recuperación](how-to-understand-automated-ml.md#precision-recall-chart)| Evalúa la relación de las etiquetas correctas con respecto a la relación de las instancias de etiquetas encontradas de los datos. 
[Curvas ROC](how-to-understand-automated-ml.md#roc)| Evalúa la relación de etiquetas correctas con respecto a la relación de etiquetas de falsos positivos.

### <a name="handle-imbalanced-data"></a>Manejo de datos desequilibrados 

Las técnicas siguientes son otras opciones para controlar los datos desequilibrados fuera del ML automatizado. 

- Vuelva a muestrear para equilibrar el desequilibrio de clases, ya sea mediante el muestreo ascendente de las clases más pequeñas o el muestreo descendente de las clases más grandes. Estos métodos requieren conocimientos para procesar y analizar.

- Use una métrica de rendimiento que se ocupe mejor de los datos desequilibrados. Por ejemplo, la puntuación F1 es un promedio ponderado de precisión y recuperación. La precisión mide la exactitud de un clasificador: la precisión baja indica un gran número de falsos positivos, mientras que la recuperación mide la integridad de un clasificador: la recuperación baja indica un número elevado de falsos negativos. 

## <a name="use-with-onnx-in-c-apps"></a>Uso con ONNX en aplicaciones de C#

Con Azure Machine Learning, puede usar Machine Learning para generar un modelo de Python y convertirlo al formato ONNX. El entorno de ejecución de ONNX es compatible con C#, por lo que puede usar el modelo creado automáticamente en sus aplicaciones de C# sin necesidad de volver a codificar o experimentar alguna de las latencias de red que presentan los puntos de conexión REST. Pruebe un ejemplo de este flujo [en este cuaderno de Jupyter](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/classification-bank-marketing-all-features/auto-ml-classification-bank-marketing-all-features.ipynb).

## <a name="automated-ml-in-azure-machine-learning"></a>Aprendizaje automático automatizado en Azure Machine Learning

Azure Machine Learning ofrece dos experiencias para trabajar con aprendizaje automático automatizado

* Para clientes con experiencia en código, [SDK de Python de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) 

* Para clientes con poca experiencia en código o ninguna, Azure Machine Learning Studio en [https://ml.azure.com](https://ml.azure.com/)  

A continuación se resumen las funcionalidades de aprendizaje automático automatizado de alto nivel que se admiten en cada experiencia.

<a name="parity"></a>

### <a name="experiment-settings"></a>Configuración del experimento 

Los valores siguientes le permiten configurar el experimento de aprendizaje automático automatizado. 

| | SDK de Python| Studio
----|:----:|:----:
Divide los datos en conjuntos de entrenamiento y validación| ✓|✓
Admite tareas de Machine Learning: clasificación, regresión y predicción| ✓| ✓
Optimiza en función de la métrica principal| ✓| ✓
Admite el proceso de AML como destino de proceso | ✓|✓
Configura el horizonte de previsión, los intervalos de destino y el período acumulado|✓|✓
Establece criterios de salida |✓|✓ 
Establece el número de iteraciones simultáneas| ✓|✓
Quitar columnas| ✓|✓
Bloquea algoritmos|✓|✓
Validación cruzada |✓|✓
Admite el aprendizaje en clústeres de Azure Databricks| ✓|
Muestra nombres de características con ingeniería|✓|
Resumen de características| ✓|
Caracterización de fiestas|✓|
Nivel de detalle de los archivos de registro| ✓|

### <a name="model-settings"></a>Configuración del modelo

Estos valores se pueden aplicar al mejor modelo como resultado del experimento de aprendizaje automático automatizado.

||SDK de Python|Studio
----|:----:|:----:
Registro del mejor modelo| ✓|✓
Implementación del mejor modelo| ✓| ✓
Explicación del mejor modelo| ✓|✓
Habilita los modelos del conjunto de votación y el conjunto de pila| ✓|✓
Muestra el mejor modelo en función de una métrica que no es la principal|✓|Habilita o deshabilita la compatibilidad con el modelo de ONNX|✓|
Prueba del modelo | ✓| |

### <a name="run-control-settings"></a>Ejecuta la configuración de un control

Estos valores le permiten revisar y controlar las ejecuciones de los experimentos y sus ejecuciones secundarias. 

||SDK de Python| Studio
----|:----:|:----:
Ejecuta una tabla de resumen| ✓|✓
Cancela la ejecución| ✓|✓
Cancela la ejecución secundaria| ✓| ✓
Obtiene límites de protección| ✓|✓
Pone en pausa la ejecución| ✓| 
Reanuda la ejecución| ✓| 

## <a name="next-steps"></a>Pasos siguientes

Vea ejemplos y aprenda cómo generar modelos mediante aprendizaje automático automatizado:

+ Lea el [Tutorial: entrenamiento automático de un modelo de regresión con Azure Machine Learning](tutorial-auto-train-models.md).

+ Configure el experimento de entrenamiento automático:
  + En Azure Machine Learning Studio, [use estos pasos](how-to-create-portal-experiments.md).
  + Con el SDK de Python, [siga estos pasos](how-to-configure-auto-train.md).

+ Obtenga información sobre cómo realizar entrenamientos automáticos con datos de series temporales [con estos pasos](how-to-auto-train-forecast.md).

+ Pruebe los [ejemplos de Jupyter Notebook para aprendizaje automático automatizado](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/automated-machine-learning/).

* El aprendizaje automático automatizado también está disponible en otras soluciones de Microsoft como [ML.NET](https://docs.microsoft.com/dotnet/machine-learning/automl-overview), [HDInsight](../hdinsight/spark/apache-spark-run-machine-learning-automl.md), [Power BI](https://docs.microsoft.com/power-bi/service-machine-learning-automated) y [SQL Server](https://cloudblogs.microsoft.com/sqlserver/2019/01/09/how-to-automate-machine-learning-on-sql-server-2019-big-data-clusters/)
