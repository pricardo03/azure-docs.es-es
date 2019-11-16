---
title: Cómo elegir algoritmos
titleSuffix: ML Studio (classic) - Azure
description: Cómo elegir algoritmos de Azure Machine Learning Studio (clásico) para el aprendizaje supervisado y no supervisado en experimentos de agrupación en clústeres, clasificación o regresión.
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: conceptual
author: xiaoharper
ms.author: amlstudiodocs
ms.custom: previous-ms.author=pakalra, previous-author=pakalra
ms.date: 03/04/2019
ms.openlocfilehash: c69b4c15397dc13f36a707f932c2464a4ff94ca7
ms.sourcegitcommit: 35715a7df8e476286e3fee954818ae1278cef1fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73838465"
---
# <a name="how-to-choose-algorithms-for-azure-machine-learning-studio-classic"></a>Cómo elegir algoritmos para Azure Machine Learning Studio (clásico)

[!INCLUDE [Designer notice](../../../includes/designer-notice.md)]

La respuesta a la pregunta "¿qué algoritmo de aprendizaje automático debería usar?" siempre es "Depende". Depende del tamaño, la calidad y la naturaleza de los datos. Depende de qué desea hacer con la respuesta. Depende de cómo se hayan traducido los cálculos del algoritmo en instrucciones para el equipo que está usando. Y también depende del tiempo de que disponga. Ni siquiera los científicos con más experiencia en datos pueden determinar qué algoritmo funcionará mejor antes de probarlos.

Machine Learning Studio (clásico) ofrece los algoritmos mas innovadores, como árboles de decisión incrementados escalables, sistemas de recomendaciones bayesianas, redes neuronales profundas y selvas de decisión, que se han desarrollado en Microsoft Research. También se incluyen paquetes de aprendizaje automático escalables de código abierto, como Vowpal Wabbit. La versión clásica de Machine Learning Studio admite algoritmos de aprendizaje automático para clasificación, regresión y agrupación en clústeres y binarias y multiclase. Consulte la lista completa de [módulos de Machine Learning](/azure/machine-learning/studio-module-reference/index).
La documentación proporciona alguna información acerca de cada algoritmos, y se ajustan los parámetros para optimizar el algoritmo para su uso.  


## <a name="the-machine-learning-algorithm-cheat-sheet"></a>Hoja de referencia rápida de algoritmos de Machine Learning

La **[Hoja de referencia rápida de algoritmos de aprendizaje automático de Microsoft Azure Machine Learning Studio](../algorithm-cheat-sheet.md)** ayuda a elegir el algoritmo de aprendizaje automático adecuado para sus soluciones de análisis predictivo de la biblioteca de algoritmos de Azure Machine Learning.
En este artículo se explica cómo usarla.

> [!NOTE]
> Para descargar la hoja de referencia rápida y usarla con las instrucciones de este artículo, consulte la [hoja de referencia rápida de algoritmos de aprendizaje automático](../algorithm-cheat-sheet.md).
> 
> 

Estas recomendaciones son una recopilación de los comentarios y las sugerencias de muchos científicos de datos y expertos en aprendizaje automático. No estuvimos de acuerdo en todo, pero intentamos combinar las opiniones para llegar a un consenso general. La mayoría de los argumentos de desacuerdo comienzan con "Depende...".


> [!TIP]
> Para descargar una introducción infográfica fácil de comprender sobre conceptos básicos de aprendizaje automático para obtener información acerca de los algoritmos populares utilizados para responder a preguntas comunes sobre el aprendizaje automático, consulte [Conceptos básicos de aprendizaje automático con ejemplos de algoritmos](basics-infographic-with-algorithm-examples.md).

## <a name="flavors-of-machine-learning"></a>Variantes del aprendizaje automático

### <a name="supervised"></a>Supervisado

Los algoritmos de aprendizaje supervisado hacen predicciones basadas en un conjunto de ejemplos. Por ejemplo, los precios históricos de las acciones pueden usarse para hacer estimaciones de los precios futuros. Cada ejemplo usado para el entrenamiento se etiqueta con el valor de interés; en este caso, el precio de las acciones. Un algoritmo de aprendizaje supervisado busca patrones en esas etiquetas de valor. Puede usar cualquier información que pueda ser relevante, como el día de la semana, la temporada, datos financieros de la empresa, el tipo de sector o la presencia de eventos geopolíticos perjudiciales, y cada algoritmo busca tipos diferentes de patrones. Una vez que el algoritmo encuentra el mejor patrón posible, lo usa para hacer predicciones de datos de prueba sin etiquetar; en este caso, los precios futuros.

El aprendizaje supervisado es un tipo conocido y útil de aprendizaje automático. Con una única excepción: todos los módulos de la versión clásica de Azure Machine Learning Studio son algoritmos de aprendizaje supervisado. Hay varios tipos específicos de aprendizaje supervisado representados en Azure Machine Learning Studio (clásico): la clasificación, la regresión y la detección de anomalías.

* **Clasificación**. Cuando los datos se usan para predecir una categoría, el aprendizaje supervisado también se denomina clasificación. Esto ocurre cuando se asigna una imagen, como una foto de un 'gato' o un 'perro'. Cuando hay solo dos opciones, se denomina clasificación **de dos clases** o **binomial**. Cuando hay más categorías, como cuando se predice el ganador del torneo March Madness de la NCAA, este problema se conoce como **clasificación multiclase**.
* **Regresión**. Cuando se predice un valor, como el precio de las acciones, el aprendizaje supervisado se denomina regresión.
* **Detección de anomalías**. A veces, el objetivo es identificar puntos de datos que simplemente no son habituales. En la detección de fraudes, por ejemplo, los patrones de gasto de tarjeta de crédito muy poco habituales son sospechosos. Las posibles variaciones son tan numerosas y los ejemplos de formación son tan pocos, que no es posible saber de qué actividad fraudulenta se trata. El enfoque que toma la detección de anomalías es simplemente aprender qué puede considerarse como actividad normal (haciendo uso de las transacciones no fraudulentas del historial) e identificar todo lo que sea significativamente diferente.

### <a name="unsupervised"></a>Sin supervisar

En el aprendizaje sin supervisar, los puntos de datos no tienen etiquetas asociadas a ellos. En su lugar, el objetivo de un algoritmo de aprendizaje sin supervisar es organizar los datos de alguna manera o describir su estructura. Esto puede significar agruparlos en clústeres o buscar diferentes maneras de examinar datos complejos para que parezcan más simples o más organizados.

### <a name="reinforcement-learning"></a>Aprendizaje de refuerzo

En el aprendizaje de refuerzo, el algoritmo elige una acción en respuesta a cada punto de datos. El algoritmo de aprendizaje también recibe una señal de recompensa un poco más adelante, que indica cómo de buena fue la decisión.
Según esto, el algoritmo modifica su estrategia para lograr la mayor recompensa. Actualmente no hay ningún módulo de algoritmos de aprendizaje de refuerzo en Azure Machine Learning Studio (clásico). El aprendizaje de refuerzo es común en robótica, donde el conjunto de lecturas del sensor en un punto en el tiempo es un punto de datos, y el algoritmo debe elegir la siguiente acción del robot. También resulta perfecto para las aplicaciones de Internet de las cosas.

## <a name="considerations-when-choosing-an-algorithm"></a>Consideraciones al elegir un algoritmo

### <a name="accuracy"></a>Precisión

No siempre es necesario obtener la respuesta más precisa posible.
A veces, una aproximación ya es útil, según para qué se la desee usar. Si es así, puede reducir el tiempo de procesamiento de forma considerable al usar métodos más aproximados. Otra ventaja de los métodos más aproximados es que tienden naturalmente a evitar el sobreajuste.

### <a name="training-time"></a>Tiempo de entrenamiento

La cantidad de minutos u horas necesarios para entrenar un modelo varía mucho según el algoritmo. A menudo, el tiempo de entrenamiento depende de la precisión (generalmente, uno determina al otro). Además, algunos algoritmos son más sensibles a la cantidad de puntos de datos que otros.
Si el tiempo es limitado, esto puede determinar la elección del algoritmo, especialmente cuando el conjunto de datos es grande.

### <a name="linearity"></a>Linealidad

Muchos algoritmos de aprendizaje automático hacen uso de la linealidad. Los algoritmos de clasificación lineal suponen que las clases pueden estar separadas mediante una línea recta (o su análogo de mayores dimensiones). Entre ellos, se encuentran la regresión logística y las máquinas de vectores de soporte (como las que se implementan en Azure Machine Learning Studio [clásico]).
Los algoritmos de regresión lineal suponen que las tendencias de datos siguen una línea recta. Estas suposiciones no son incorrectas para algunos problemas, pero en otros disminuyen la precisión.

![Límite de clase no lineal](./media/algorithm-choice/image1.png)

***Límite de clase no lineal*** *: la dependencia de un algoritmo de clasificación lineal se traduciría en baja precisión*.

![Datos con tendencia no lineal](./media/algorithm-choice/image2.png)

***Datos con tendencia no lineal*** *: el uso de un método de regresión lineal generaría errores mucho mayores que los necesarios*

A pesar de los riesgos, los algoritmos lineales son muy populares como primera línea de ataque. Tienden a ser algorítmicamente sencillos y rápidos para entrenar.

### <a name="number-of-parameters"></a>Cantidad de parámetros

Los parámetros son los botones que un científico de datos activa al configurar un algoritmo. Son números que afectan al comportamiento del algoritmo, como la tolerancia a errores o la cantidad de iteraciones, o bien opciones de variantes de comportamiento del algoritmo. El tiempo de entrenamiento y la precisión del algoritmo a veces pueden ser muy sensibles y requerir solo la configuración correcta. Normalmente, los algoritmos con parámetros de números grandes requieren la mayor cantidad de pruebas y errores posible para encontrar una buena combinación.

También puede haber un bloque de módulos de [barrido de parámetros](algorithm-parameters-optimize.md) en la versión clásica de Azure Machine Learning Studio que prueba automáticamente todas las combinaciones de parámetros en cualquier granularidad que se elija. Aunque esta es una excelente manera de asegurarse de que se ha distribuido el espacio de parámetros, el tiempo necesario para entrenar un modelo aumenta de manera exponencial con la cantidad de parámetros.

La ventaja es que tener muchos parámetros normalmente indica que un algoritmo tiene mayor flexibilidad. Por lo general, puede conseguir una precisión muy elevada, siempre y cuando se encuentre la combinación correcta de configuraciones de parámetros.

### <a name="number-of-features"></a>Cantidad de características

Para ciertos tipos de datos, la cantidad de características puede ser muy grande en comparación con la cantidad de puntos de datos. Este suele ser el caso de la genética o los datos textuales. Una gran cantidad de características puede trabar algunos algoritmos de aprendizaje y provocar que el tiempo de entrenamiento sea demasiado largo. Las máquinas de vectores de soporte son especialmente adecuadas para este caso (ver abajo).

### <a name="special-cases"></a>Casos especiales

Algunos algoritmos de aprendizaje hacen determinadas suposiciones sobre la estructura de los datos o los resultados deseados. Si encuentra uno que se ajuste a sus necesidades, este puede ofrecerle resultados más útiles, predicciones más precisas o tiempos de entrenamiento más cortos.

| **Algoritmo** | **Precisión** | **Tiempo de entrenamiento** | **Linealidad** | **Parámetros** | **Notas** |
| --- |:---:|:---:|:---:|:---:| --- |
| **Clasificación multiclase** | | | | | |
| [regresión logística](/azure/machine-learning/studio-module-reference/two-class-logistic-regression) | |● |● |5 | |
| [bosque de decisión](/azure/machine-learning/studio-module-reference/two-class-decision-forest) |● |○ | |6 | |
| [selva de decisión](/azure/machine-learning/studio-module-reference/two-class-decision-jungle) |● |○ | |6 |Uso de memoria bajo |
| [árbol de decisión impulsado](/azure/machine-learning/studio-module-reference/two-class-boosted-decision-tree) |● |○ | |6 |Uso de memoria grande |
| [red neuronal](/azure/machine-learning/studio-module-reference/two-class-neural-network) |● | | |9 |[La personalización adicional es posible](azure-ml-netsharp-reference-guide.md) |
| [perceptrón promedio](/azure/machine-learning/studio-module-reference/two-class-averaged-perceptron) |○ |○ |● |4 | |
| [máquina de vectores de soporte](/azure/machine-learning/studio-module-reference/two-class-support-vector-machine) | |○ |● |5 |Útil para conjuntos de características de gran tamaño |
| [máquina de vectores de soporte localmente profunda](/azure/machine-learning/studio-module-reference/two-class-locally-deep-support-vector-machine) |○ | | |8 |Útil para conjuntos de características de gran tamaño |
| [máquina del punto de Bayes](/azure/machine-learning/studio-module-reference/two-class-bayes-point-machine) | |○ |● |3 | |
| **Clasificación multiclase** | | | | | |
| [regresión logística](/azure/machine-learning/studio-module-reference/multiclass-logistic-regression) | |● |● |5 | |
| [bosque de decisión](/azure/machine-learning/studio-module-reference/multiclass-decision-forest) |● |○ | |6 | |
| [selva de decisión](/azure/machine-learning/studio-module-reference/multiclass-decision-jungle) |● |○ | |6 |Uso de memoria bajo |
| [red neuronal](/azure/machine-learning/studio-module-reference/multiclass-neural-network) |● | | |9 |[La personalización adicional es posible](azure-ml-netsharp-reference-guide.md) |
| [uno frente a todos](/azure/machine-learning/studio-module-reference/one-vs-all-multiclass) |- |- |- |- |Vea las propiedades del método de dos clases seleccionado |
| **Regresión** | | | | | |
| [lineal](/azure/machine-learning/studio-module-reference/linear-regression) | |● |● |4 | |
| [Bayesiano lineal](/azure/machine-learning/studio-module-reference/bayesian-linear-regression) | |○ |● |2 | |
| [bosque de decisión](/azure/machine-learning/studio-module-reference/decision-forest-regression) |● |○ | |6 | |
| [árbol de decisión impulsado](/azure/machine-learning/studio-module-reference/boosted-decision-tree-regression) |● |○ | |5 |Uso de memoria grande |
| [cuantil de bosque rápido](/azure/machine-learning/studio-module-reference/fast-forest-quantile-regression) |● |○ | |9 |Distribuciones en lugar de predicciones de puntos |
| [red neuronal](/azure/machine-learning/studio-module-reference/neural-network-regression) |● | | |9 |[La personalización adicional es posible](azure-ml-netsharp-reference-guide.md) |
| [Poisson](/azure/machine-learning/studio-module-reference/poisson-regression) | | |● |5 |Logarítmico lineal técnicamente. Para la predicción de recuentos |
| [ordinal](/azure/machine-learning/studio-module-reference/ordinal-regression) | | | |0 |Para la predicción de ordenación de clasificaciones |
| **Detección de anomalías** | | | | | |
| [máquina de vectores de soporte](/azure/machine-learning/studio-module-reference/one-class-support-vector-machine) |○ |○ | |2 |Especialmente útil para conjuntos de características de gran tamaño |
| [Detección de anomalías basada en PCA](/azure/machine-learning/studio-module-reference/pca-based-anomaly-detection) | |○ |● |3 | |
| [K-Means](/azure/machine-learning/studio-module-reference/k-means-clustering) | |○ |● |4 |Un algoritmo de agrupación en clústeres |

**Propiedades de algoritmo:**

**●** : muestra una precisión excelente, tiempos de entrenamiento breves y uso de linealidad

**○**: muestra una precisión buena y tiempos de entrenamiento moderado

## <a name="algorithm-notes"></a>Notas sobre los algoritmos

### <a name="linear-regression"></a>Regresión lineal

Como se mencionó anteriormente, la [regresión lineal](/azure/machine-learning/studio-module-reference/linear-regression) ajusta una línea (o plano o hiperplano) al conjunto de datos. Es potente, simple y rápida, pero puede ser demasiado simple para algunos problemas.

![Datos con tendencia lineal](./media/algorithm-choice/image3.png)

***Datos con tendencia lineal***

### <a name="logistic-regression"></a>Regresión logística

Aunque incluye la palabra "regresión" en el nombre, la regresión logística es en realidad una herramienta eficaz para la clasificación [de dos clases](/azure/machine-learning/studio-module-reference/two-class-logistic-regression) y [multiclase](/azure/machine-learning/studio-module-reference/multiclass-logistic-regression). Es rápida y sencilla. El hecho de que use una curva con forma de S en lugar de una línea recta la hace ideal para dividir los datos en grupos. La regresión logística proporciona límites de clase lineal. Por lo tanto, cuando la use, asegúrese de que la aproximación lineal le es útil.

![Regresión logística a datos de dos clases con una sola característica](./media/algorithm-choice/image4.png)

***Regresión logística a datos de dos clases con una sola característica*** *: el límite de clase es el punto en el que la curva logística tiene la misma distancia hacia ambas clases*

### <a name="trees-forests-and-jungles"></a>Árboles, bosques y selvas

Los bosques de decisión ([regresión](/azure/machine-learning/studio-module-reference/decision-forest-regression), [dos clases](/azure/machine-learning/studio-module-reference/two-class-decision-forest) y [multiclase](/azure/machine-learning/studio-module-reference/multiclass-decision-forest)), las selvas de decisión ([dos clases](/azure/machine-learning/studio-module-reference/two-class-decision-jungle) y [multiclase](/azure/machine-learning/studio-module-reference/multiclass-decision-jungle)) y los árboles de decisión impulsados ([regresión](/azure/machine-learning/studio-module-reference/boosted-decision-tree-regression) y [dos clases](/azure/machine-learning/studio-module-reference/two-class-boosted-decision-tree)) se basan en árboles de decisión, un concepto fundamental del aprendizaje automático. Hay muchas variantes de árboles de decisión, pero todas ellas hacen lo mismo: subdividir el espacio de características en regiones con casi siempre la misma etiqueta. Estas pueden ser regiones de la misma categoría o de un valor constante, según si se está realizando una clasificación o una regresión.

![El árbol de decisión subdivide un espacio de características](./media/algorithm-choice/image5.png)

***Un árbol de decisión subdivide un espacio de características en regiones de valores más o menos uniformes***

Ya que un espacio de características se puede subdividir en regiones arbitrariamente pequeñas, es fácil imaginar dividirlo lo suficiente como para tener un punto de datos por región. Se trata de un ejemplo extremo de sobreajuste. Para evitar esto, se genera un conjunto grande de árboles con precisión matemática para garantizar que los árboles no se correlacionen. El resultado promedio de este "bosque de decisión" es un árbol que evita el sobreajuste. Los bosques de decisión pueden llegar a usar demasiada memoria. Las selvas de decisión son una variante que consume menos memoria a costa de un tiempo de entrenamiento ligeramente más prolongado.

Los árboles de decisión impulsados evitan el sobreajuste al limitar la cantidad de veces que pueden subdividir y la cantidad de puntos de datos que se permiten en cada región. El algoritmo genera una secuencia de árboles, cada uno de los cuales aprende a compensar el error que deja el árbol anterior. El resultado es un lector muy preciso que tiende a usar una gran cantidad de memoria. Para obtener la descripción técnica completa, vea el [documento original de Friedman](https://www-stat.stanford.edu/~jhf/ftp/trebst.pdf).

[La regresión rápida de bosque por cuantiles](/azure/machine-learning/studio-module-reference/fast-forest-quantile-regression) es una variación de los árboles de decisión para los casos especiales en los que se desea conocer no solo el valor típico (medio) de los datos dentro de una región, sino también su distribución en forma de cuantiles.

### <a name="neural-networks-and-perceptrons"></a>Redes neuronales y perceptrones

Las redes neuronales son algoritmos de aprendizaje inspirados en el cerebro que abarcan problemas [multiclase](/azure/machine-learning/studio-module-reference/multiclass-neural-network), [de dos clases](/azure/machine-learning/studio-module-reference/two-class-neural-network) y [de regresión](/azure/machine-learning/studio-module-reference/neural-network-regression). Vienen en una variedad infinita, pero las redes neuronales de la versión clásica de Azure Machine Learning Studio tienen todas la forma de gráficos acíclicos dirigidos. Esto significa que las características de entrada se adelantan (nunca se atrasan) en una secuencia de capas antes convertirse en salidas. En cada capa, las entradas se ponderan en varias combinaciones, se suman y se pasan a la siguiente capa. Esta combinación de cálculos sencillos da como resultado la capacidad de aprender límites de clase y tendencias de datos sofisticados, casi como por arte de magia. Las redes de varias capas de este tipo realizan el "aprendizaje profundo" que hace posibles los informes de tecnología y la ciencia ficción.

Sin embargo, el alto rendimiento no es gratuito. Las redes neuronales pueden tardar mucho tiempo para entrenarse, especialmente para grandes conjuntos de datos con muchas características. También tienen más parámetros que la mayoría de los algoritmos, lo que implica que el barrido de parámetros alargue mucho el tiempo de entrenamiento.
Para los que quieren obtener resultados óptimos y [especificar su propia estructura de red](azure-ml-netsharp-reference-guide.md), las posibilidades son inagotables.

![Límites aprendidos por las redes neuronales](./media/algorithm-choice/image6.png)

***Los límites que aprenden las redes neuronales pueden ser complejos e irregulares***

El [perceptrón promedio de dos clases](/azure/machine-learning/studio-module-reference/two-class-averaged-perceptron) es la respuesta de las redes neuronales para acortar en gran medida los tiempos de entrenamiento. Este perceptrón usa una estructura de red que proporciona límites de clase lineal. Es casi primitivo según los estándares actuales, pero tiene un largo historial de trabajo sólido y es lo suficientemente pequeño como para aprender rápidamente.

### <a name="svms"></a>SVM

Las máquinas de vectores de soporte (SVM) buscan el límite que separa las clases con el mayor margen posible. Cuando no se pueden separar bien las dos clases, los algoritmos buscan el mejor límite que pueden. Tal como está escrito en Azure Machine Learning Studio (clásico), la [SVM de dos clases](/azure/machine-learning/studio-module-reference/two-class-support-vector-machine) hace esto solo con una línea recta (en el idioma de SVM, usa un kernel lineal).
Gracias a que hace esta aproximación lineal, se puede ejecutar con bastante rapidez. Donde realmente se destaca es con datos de muchas características, como texto o datos genómicos. En estos casos, las SVM pueden separar las clases más rápidamente y con menos sobreajuste que con la mayoría de los otros algoritmos, además de que requieren solo una pequeña cantidad de memoria.

![Límite de clase de máquina de vectores de soporte](./media/algorithm-choice/image7.png)

***Un límite de clase de máquina de vectores de soporte maximiza el margen que separa las dos clases***

Como otro producto de Microsoft Research, la [SVM de dos clases localmente profunda](/azure/machine-learning/studio-module-reference/two-class-locally-deep-support-vector-machine) es una variante no lineal de SVM que mantiene casi toda la velocidad y la eficiencia de memoria de la versión lineal. Es ideal para los casos en los que el enfoque lineal no proporciona respuestas suficientemente precisas. Para mantener su velocidad, los desarrolladores dividieron el problema en un grupo de pequeños problemas de SVM lineales. Lea la [descripción completa](http://proceedings.mlr.press/v28/jose13.html) para obtener detalles sobre cómo lograron este truco.

Mediante una extensión inteligente de SVM no lineales, la [SVM de una clase](/azure/machine-learning/studio-module-reference/one-class-support-vector-machine) dibuja un límite que demarca con precisión todo el conjunto de datos. Es útil para la detección de anomalías. Los puntos de datos nuevos que quedan fuera de ese límite son tan poco habituales que requieren atención.

### <a name="bayesian-methods"></a>Métodos bayesianos

Los métodos bayesianos tienen una calidad muy deseable: evitan el sobreajuste. Para ello, hacen algunas suposiciones anticipadas sobre la posible distribución de la respuesta. Otra característica de este enfoque es que tienen muy pocos parámetros. La versión clásica de Azure Machine Learning Studio tiene dos algoritmos bayesianos tanto para la clasificación ([automática de puntos de Bayes de dos clases](/azure/machine-learning/studio-module-reference/two-class-bayes-point-machine)) como para la regresión ([regresión lineal bayesiana](/azure/machine-learning/studio-module-reference/bayesian-linear-regression)).
Tenga en cuenta que se asume que los datos se pueden dividir o encajan en una línea recta.

Como nota histórica, se desarrollaron las máquinas de puntos de Bayes en Microsoft Research. Presentan un trabajo teórico excepcional. Al alumno interesado se le dirige al [artículo original de JMLR](http://jmlr.org/papers/volume1/herbrich01a/herbrich01a.pdf) y a un [blog revelador de Chris Bishop](https://blogs.technet.com/b/machinelearning/archive/2014/10/30/embracing-uncertainty-probabilistic-inference.aspx).

### <a name="specialized-algorithms"></a>Algoritmos especializados
Si tiene un objetivo muy específico, puede que sea su día de suerte. Dentro de la colección de Azure Machine Learning Studio (clásico), existen algoritmos que se especializan en:

- predicción de rangos ([regresión ordinal](/azure/machine-learning/studio-module-reference/ordinal-regression)),
- predicción de totales ([regresión de Poisson](/azure/machine-learning/studio-module-reference/poisson-regression)),
- detección de anomalías (uno basado en el [análisis de componentes principales](/azure/machine-learning/studio-module-reference/pca-based-anomaly-detection) y otro en [máquinas de vectores de soporte](/azure/machine-learning/studio-module-reference/one-class-support-vector-machine))
- agrupación en clústeres ([K-Means](/azure/machine-learning/studio-module-reference/k-means-clustering))

![Detección de anomalías basada en PCA](./media/algorithm-choice/image8.png)

***Detección de anomalías basada en PCA*** *: la inmensa mayoría de los datos entra en una distribución típica; los puntos que se desvían mucho de esa distribución son sospechosos*

![Conjunto de datos agrupados mediante K-Means](./media/algorithm-choice/image9.png)

***Un conjunto de datos se agrupa en 5 clústeres mediante K-Means***

También hay un [clasificador multiclase uno contra todos](/azure/machine-learning/studio-module-reference/one-vs-all-multiclass)que divide el problema de clasificación de clase N en problemas de clasificación de dos clases N-1. La precisión, el tiempo de entrenamiento y las propiedades de linealidad se determinan con los clasificadores de dos clases que se usan.

![Clasificadores de dos clases combinados para formar un clasificador de tres clases](./media/algorithm-choice/image10.png)

***Un par de clasificadores de dos clases se combinan para formar un clasificador de tres clases***

La versión clásica de Azure Machine Learning Studio también incluye acceso a un marco de Machine Learning eficaz con el título [Vowpal Wabbit](/azure/machine-learning/studio-module-reference/train-vowpal-wabbit-version-7-4-model).
VW es un reto a la categorización, ya que puede aprender problemas tanto de clasificación como de regresión, e incluso puede aprender a partir de datos parcialmente etiquetados. Puede configurarlo para usar algoritmos de aprendizaje, funciones de pérdida y algoritmos de optimización. Está totalmente diseñado para ser eficiente, paralelo y extremadamente rápido. Administra enormes conjuntos de características con mucha facilidad.
VW, iniciado y liderado por el propio John Langford de Microsoft Research, es una entrada de Fórmula Uno en un campo de algoritmos de coches de línea. No todos los problemas se adaptan a VW, pero si el suyo lo hace, es posible que valga la pena que aumente la curva de aprendizaje en esa interfaz. También está disponible como [código fuente abierto independiente](https://github.com/JohnLangford/vowpal_wabbit) en varios idiomas.

## <a name="next-steps"></a>Pasos siguientes

* Para descargar una introducción infográfica fácil de comprender sobre conceptos básicos de aprendizaje automático para obtener información acerca de los algoritmos populares utilizados para responder a preguntas comunes sobre el aprendizaje automático, consulte [Conceptos básicos de aprendizaje automático con ejemplos de algoritmos](basics-infographic-with-algorithm-examples.md).

* Para ver una lista por categoría de todos los algoritmos disponibles de aprendizaje automático en Machine Learning Studio (clásico), consulte [Inicializar modelo](/azure/machine-learning/studio-module-reference/machine-learning-initialize-model) en la Ayuda de módulos y algoritmos de Machine Learning Studio (clásico).

* Para ver una lista completa de todos los algoritmos de la versión clásica de Machine Learning Studio, consulte [Lista de la A a la Z de módulos de Machine Learning Studio (clásico)](/azure/machine-learning/studio-module-reference/a-z-module-list) en la Ayuda de módulos y algoritmos de Machine Learning Studio (clásico).
