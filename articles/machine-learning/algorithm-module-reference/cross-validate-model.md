---
title: 'Modelo de validación cruzada: referencia para los módulos'
titleSuffix: Azure Machine Learning service
description: Aprenda a usar el módulo Cross-Validate Model (Modelo de validación cruzada) del servicio Azure Machine Learning para realizar una validación cruzada de las estimaciones de parámetros de los modelos de clasificación o regresión mediante la partición de los datos.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/10/2019
ms.openlocfilehash: a5eea61ee8284010531e80e17bf1110ab470d04c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510289"
---
# <a name="cross-validate-model"></a>Modelo de validación cruzada

En este artículo se describe cómo usar el módulo **Cross-Validate Model** (Modelo de validación cruzada) del diseñador de Azure Machine Learning (versión preliminar). La *validación cruzada* es una técnica importante que se usa a menudo en el aprendizaje automático para evaluar la variabilidad de un conjunto de datos y la confiabilidad de cualquier modelo entrenado con esos datos.  

El módulo **Cross Validate Model** (Modelo de validación cruzada) toma como entrada un conjunto de datos con etiquetas, junto con un modelo de clasificación o regresión no entrenado. Divide el conjunto de datos en varios subconjuntos (*plegamientos*), crea un modelo en cada plegamiento y, a continuación, devuelve un conjunto de estadísticas de precisión para cada plegamiento. Al comparar las estadísticas de precisión de todos los plegamientos, puede interpretar la calidad del conjunto de datos y comprender si el modelo es susceptible a variaciones de los datos.  

La validación cruzada también devuelve probabilidades y resultados previstos para el conjunto de datos, de modo que pueda evaluar la confiabilidad de las predicciones.  

### <a name="how-cross-validation-works"></a>Funcionamiento de la validación cruzada

1. La validación cruzada divide aleatoriamente los datos de entrenamiento en varias particiones, también denominadas *plegamientos*. 

    + El algoritmo se establece de forma predeterminada en 10 plegamientos si no ha particionado previamente el conjunto de datos. 
    + Para dividir el conjunto de datos en un número diferente de plegamientos, puede usar el módulo de [partición y ejemplo](partition-and-sample.md) e indicar cuántos plegamientos quiere usar.  

2.  El módulo reserva los datos del plegamiento 1 para la validación (se denomina a veces *plegamiento de datos de exclusión*) y usa los plegamientos restantes para entrenar un modelo. 

    Por ejemplo, al crear cinco plegamientos, el módulo generaría cinco modelos durante la validación cruzada, con cada uno de los modelos entrenado con 4/5 de los datos y probado con el 1/5 restante.  

3.  Durante las pruebas del modelo para cada plegamiento, se evalúan varias estadísticas de precisión. Las estadísticas que se usan dependen del tipo de modelo que se está evaluando. Se usan estadísticas diferentes para evaluar los modelos de clasificación y los modelos de regresión.  

4.  Cuando el proceso de compilación y evaluación se completa para todos los plegamientos, el **modelo de validación cruzada** genera un conjunto de métricas de rendimiento y resultados puntuados de todos los datos. Debe revisar estas métricas para ver si un solo plegamiento tiene una precisión especialmente alta o baja. 

### <a name="advantages-of-cross-validation"></a>Ventajas de la validación cruzada

Una forma diferente y común de evaluar un modelo es dividir los datos en un conjunto de entrenamiento y prueba mediante [Dividir datos](split-data.md) y, a continuación, validar el modelo en los datos de entrenamiento. Sin embargo, la validación cruzada ofrece algunas ventajas:  

-   La validación cruzada utiliza más datos de prueba.

     La validación cruzada mide el rendimiento del modelo con los parámetros especificados en un espacio de datos mayor. Es decir, la validación cruzada utiliza todo el conjunto de datos de entrenamiento para el entrenamiento y la evaluación, en lugar de una parte. Por el contrario, si valida un modelo usando los datos generados de una división aleatoria, se suele evaluar el modelo solo con un 30 % o menos de los datos disponibles.  

     Sin embargo, dado que la validación cruzada entrena y valida el modelo varias veces con un conjunto de datos mayor, es mucho más intensivo a nivel computacional y tarda mucho más tiempo que la validación con una división aleatoria.  

-   La validación cruzada evalúa el conjunto de datos y el modelo.

     La validación cruzada no solo mide la precisión de un modelo, sino que también ofrece alguna idea sobre lo representativo que es el conjunto de datos y el grado de sensibilidad del modelo a las variaciones en los datos.  

## <a name="how-to-use-cross-validate-model"></a>Cómo usar el modelo de validación cruzada

La validación cruzada puede tardar mucho tiempo en ejecutarse si el conjunto de caracteres es grande.  Por lo tanto, puede usar el módulo **Cross Validate Model** (Modelo de validación cruzada) en la fase inicial de compilación y prueba del modelo, para evaluar la calidad de los parámetros del modelo (suponiendo que el tiempo de cálculo sea tolerable) y, a continuación, entrenar y evaluar el modelo mediante los parámetros establecidos con los módulos de [entrenamiento del modelo](train-model.md) y [evaluación del modelo](evaluate-model.md).

En este escenario, se entrena y se prueba el modelo con el módulo **Cross Validate Model** (Modelo de validación cruzada).

1. Agregue el módulo **Cross Validate Model** (Modelo de validación cruzada) a la canalización. Puede encontrarlo en el diseñador de Azure Machine Learning, en la categoría **Model Scoring & Evaluation** (Puntuación y evaluación del modelo). 

2. Conecte la salida de cualquier modelo de **clasificación** o **regresión**. 

    Por ejemplo, si usa una **Two Class Bayes Point Machine** para la clasificación, configure el modelo con los parámetros que quiera y, a continuación, arrastre un conector del puerto del **modelo no entrenado** del clasificador al puerto coincidente del módulo **Cross Validate Model** (Modelo de validación cruzada). 

    > [!TIP] 
    > No es necesario entrenar el modelo porque el **modelo de validación cruzada** lo entrena automáticamente como parte de la evaluación.  
3.  En el puerto del **conjunto de datos** del **modelo de validación cruzada**, conecte cualquier conjunto de datos de entrenamiento etiquetado.  

4.  En el panel **Propiedades** del módulo **Cross Validate Model** (Modelo de validación cruzada), haga clic en **Launch column selector** (Iniciar el selector de columnas) y elija la columna única que contiene la etiqueta de clase o el valor de predicción. 

5. Establezca un valor para el parámetro **Valor de inicialización aleatorio** si quiere poder repetir los resultados de la validación cruzada en ejecuciones posteriores de los mismos datos.  

6.  Ejecución de la canalización

7. Consulte la sección [Resultados](#results) para obtener una descripción de los informes.

    Para obtener una copia del modelo para su reutilización posterior, haga clic con el botón derecho en la salida del módulo que contiene el algoritmo (por ejemplo, **Two Class Bayes Point Machine**) y haga clic en **Save as Trained Model** (Guardar como modelo entrenado).

## <a name="results"></a>Results

Una vez completadas todas las iteraciones, el módulo **Cross Validate Model** (Modelo de validación cruzada) crea puntuaciones para todo el conjunto de datos, así como métricas de rendimiento que puede usar para evaluar la calidad del modelo.

### <a name="scored-results"></a>Resultados puntuados

La primera salida del módulo proporciona los datos de origen de cada fila, junto con algunos valores de predicción y las probabilidades relacionadas. 

Para ver estos resultados, en la canalización, haga clic con el botón derecho en el módulo **Cross Validate Model** (Modelo de validación cruzada), seleccione **Scored results** (Resultados puntuados) y haga clic en **Visualizar**.

| Nuevo nombre de columna      | DESCRIPCIÓN                              |
| -------------------- | ---------------------------------------- |
| Etiquetas puntuadas        | Esta columna se agrega al final del conjunto de datos y contiene el valor de predicción de cada fila. |
| Probabilidades puntuadas | Esta columna se agrega al final del conjunto de datos e indica la probabilidad estimada del valor en **Etiquetas puntuadas**. |
| Número de plegamientos          | Indica el índice de base 0 del plegamiento al que se asignó cada fila de datos durante la validación cruzada. |

 ### <a name="evaluation-results"></a>Evaluation results

El segundo informe se agrupa por plegamientos. Recuerde que, durante la ejecución, el módulo **Cross Validate Model** (Modelo de validación cruzada) divide aleatoriamente los datos de entrenamiento en *n* plegamientos (de forma predeterminada, 10). En cada iteración sobre el conjunto de datos, el módulo **Cross Validate Model** (Modelo de validación cruzada) usa un plegamiento como un conjunto de datos de validación y los *n-1* plegamientos restantes para entrenar un modelo. Cada uno de los *n* modelos se prueba en comparación con los datos de todos los demás plegamientos.

En este informe, los plegamientos se enumeran por valor de índice en orden ascendente.  Para ordenar por cualquier otra columna, puede guardar los resultados como un conjunto de datos.

Para ver estos resultados, en la canalización, haga clic con el botón derecho en el módulo **Cross Validate Model** (Modelo de validación cruzada), seleccione **Scored results** (Resultados puntuados) y haga clic en **Visualizar**.


|Nombre de la columna| DESCRIPCIÓN|
|----|----|
|Número de plegamiento| Identificador de cada plegamiento. Si ha creado 5 plegamientos, debería haber 5 subconjuntos de datos, numerados de 0 a 4.
|Número de ejemplos del plegamiento|Número de filas asignadas a cada plegamiento. Deben ser aproximadamente iguales. |


Además, se incluyen las siguientes métricas para cada plegamiento, dependiendo del tipo de modelo que se esté evaluando. 

+ **Modelos de clasificación**: precisión, recuperación, puntuación F, AUC y precisión  

+ **Modelos de regresión**: error absoluto medio, error cuadrático medio raíz, error absoluto relativo, error cuadrado relativo y coeficiente de determinación


## <a name="technical-notes"></a>Notas técnicas  

+ Se recomienda normalizar los conjuntos de datos antes de usarlos para la validación cruzada. 

+ Dado que el módulo **Cross Validate Model** (Modelo de validación cruzada) entrena y valida el modelo varias veces, es mucho más intensivo a nivel computacional y tarda más tiempo en completarse que si se valida el modelo con un conjunto de datos dividido aleatoriamente. 

+ No es necesario dividir el conjunto de datos en conjuntos de entrenamiento y de prueba cuando se usa la validación cruzada para medir la precisión del modelo. 


## <a name="next-steps"></a>Pasos siguientes

Consulte el [conjunto de módulos disponibles](module-reference.md) para Azure Machine Learning Service. 

