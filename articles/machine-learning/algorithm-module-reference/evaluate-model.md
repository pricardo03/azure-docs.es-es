---
title: 'Evaluar el modelo: Referencia de módulo'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo usar el módulo Evaluate Model en el servicio de Azure Machine Learning para medir la exactitud de un modelo entrenado.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 40a8247c22da1f7a057e222565ffb2ec4c6b7fb3
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028746"
---
# <a name="evaluate-model-module"></a>Evaluar el módulo de modelo

En este artículo se describe un módulo de la interfaz visual (versión preliminar) para el servicio Azure Machine Learning.

Utilice este módulo para medir la precisión de un modelo entrenado. Proporcionar un conjunto de datos que contiene las puntuaciones generadas a partir de un modelo y el **Evaluate Model** módulo calcula un conjunto de métricas de evaluación estándar del sector.
  
 Las métricas devueltas por **Evaluate Model** dependen del tipo de modelo que va a evaluar:  
  
-   **Modelos de clasificación**    
-   **Modelos de regresión**    



> [!TIP]
> Si está familiarizado con la evaluación de modelos, se recomienda la serie de vídeos mediante la recuperación ante desastres. Stephen Elston, como parte de la [curso de aprendizaje de máquina](https://blogs.technet.microsoft.com/machinelearning/2015/09/08/new-edx-course-data-science-machine-learning-essentials/) de EdX. 


Hay tres formas de usar el **Evaluate Model** módulo:

+ Generar puntuaciones a través de los datos de entrenamiento y evaluación del modelo según estas puntuaciones
+ Generar puntuaciones en el modelo, pero comparar esos resultados a las puntuaciones en un conjunto de pruebas reservado
+ Comparación de puntuaciones para dos modelos diferentes pero relacionadas, con el mismo conjunto de datos

## <a name="use-the-training-data"></a>Usar los datos de entrenamiento

Para evaluar un modelo, debe conectar un conjunto de datos que contiene un conjunto de columnas de entrada y puntuaciones.  Si no hay otros datos están disponibles, puede usar el conjunto de datos original.

1. Conectar el **conjunto de datos puntuado** de salida de la [Score Model](./score-model.md) a la entrada de **Evaluate Model**. 
2. Haga clic en **Evaluate Model** módulo y ejecute el experimento para generar las puntuaciones de la evaluación.

## <a name="use-testing-data"></a>Usar datos de prueba

Un escenario común en el aprendizaje automático consiste en separar el conjunto de datos original en entrenamiento y prueba de conjuntos de datos, mediante el [división](./split-data.md) módulo, o la [partición y ejemplo](./partition-and-sample.md) módulo. 

1. Conectar el **conjunto de datos puntuado** de salida de la [Score Model](score-model.md) a la entrada de **Evaluate Model**. 
2. Conecte la salida del módulo dividir datos que contiene los datos de prueba a la entrada derecha de **Evaluate Model**.
2. Haga clic en **Evaluate Model** módulo y seleccione **ejecutar seleccionado** para generar las puntuaciones de la evaluación.

## <a name="compare-scores-from-two-models"></a>Comparar las puntuaciones de los dos modelos

También se puede conectar un segundo conjunto de puntuaciones para **Evaluate Model**.  Las puntuaciones podrían ser un conjunto compartido de evaluación que conoce los resultados o un conjunto de resultados de un modelo diferente para los mismos datos.

Esta característica es útil porque permite comparar fácilmente los resultados de dos modelos distintos en los mismos datos. O bien, puede comparar las puntuaciones de los dos ejecuciones diferentes con los mismos datos con parámetros diferentes.

1. Conectar el **conjunto de datos puntuado** de salida de la [Score Model](score-model.md) a la entrada de **Evaluate Model**. 
2. Conecte la salida del módulo puntuar modelo para el segundo modelo a la entrada derecha de **Evaluate Model**.
3. Haga clic en **Evaluate Model**y seleccione **ejecutar seleccionado** para generar las puntuaciones de la evaluación.

## <a name="results"></a>Results

Después de ejecutar **Evaluate Model**, haga clic en el módulo y seleccione **resultados de la evaluación** para ver los resultados. Puede:

+ Guardar los resultados como un conjunto de datos, para facilitar el análisis con otras herramientas
+ Generar una visualización en la interfaz

Si se conexión a los conjuntos de datos para las dos entradas de **Evaluate Model**, los resultados contendrán las métricas para ambos conjuntos de datos, o ambos modelos.
El modelo o datos adjuntos al puerto izquierdo aparece en primer lugar en el informe, seguido de las métricas del conjunto de datos, o el modelo conectado en el puerto correcto.  

Por ejemplo, la imagen siguiente representa una comparación de los resultados de dos modelos de agrupación en clústeres que se crearon en los mismos datos, pero con distintos parámetros.  

![AML&#95;Comparing2Models](media/module/aml-comparing2models.png "AML_Comparing2Models")  

Se trata de un modelo de agrupación en clústeres, los resultados de evaluación son diferentes de si compara las puntuaciones de los dos modelos de regresión, o comparar dos modelos de clasificación. Sin embargo, la presentación general es el mismo. 

## <a name="metrics"></a>Métricas

Esta sección describen las métricas devueltas para los tipos específicos de los modelos admitidos para su uso con **Evaluate Model**:

+ [modelos de clasificación](#bkmk_classification)
+ [modelos de regresión](#bkmk_regression)

###  <a name="bkmk_classification"></a> Métricas para los modelos de clasificación

Las siguientes métricas se notifican al evaluar los modelos de clasificación. Si compara los modelos, se clasifican según la métrica que seleccionó para la evaluación.  
  
-   **Precisión** mide la bondad de un modelo de clasificación como la proporción de los resultados verdaderos total de casos.  
  
-   **Precisión** es la proporción de los resultados verdaderos de todos los resultados positivos.  
  
-   **Recuerde** es una fracción de todos los resultados correctos devueltos por el modelo.  
  
-   **Puntuación F** se calcula como el promedio ponderado de precisión y la recuperación entre 0 y 1, donde el valor de puntuación F ideal es 1.  
  
-   **AUC** medidas el área bajo la curva se traza con verdaderos positivos en los positivos y del eje y false en el eje x. Esta métrica es útil porque proporciona un número único que le permite comparar los modelos de diferentes tipos.  
  
- **Promedio de pérdida logarítmica** es una puntuación única que se usa para expresar la penalización de los resultados incorrectos. Se calcula como la diferencia entre dos distribuciones de probabilidad: la true y el que aparece en el modelo.  
  
- **Pérdida logarítmica de entrenamiento** es una puntuación única que representa la ventaja del clasificador sobre una predicción aleatoria. La pérdida de registro mide la incertidumbre del modelo mediante la comparación de las probabilidades que envía a los valores conocidos (datos verdaderos) en las etiquetas. Desea minimizar la pérdida de registro para el modelo como un todo.

##  <a name="bkmk_regression"></a> Métricas para los modelos de regresión
 
Las métricas devueltas para los modelos de regresión están diseñadas generalmente para estimar la cantidad de errores.  Se considera un modelo para ajustar los datos correctamente si la diferencia entre los valores observados y previstos es pequeña. Sin embargo, mirando el patrón de los valores residuales (la diferencia entre un momento previsto y su correspondiente valor real) puede indicarle mucho acerca de la diferencia potencial en el modelo.  
  
 Las siguientes métricas se notifican para evaluar los modelos de regresión. Al comparar los modelos, se clasifican según la métrica que seleccione para su evaluación.  
  
- **Error medio absoluto (MAE)** mide la proximidad de las predicciones son a los resultados reales; por lo tanto, una puntuación menor es mejor.  
  
- **Error cuadrático medio (RMSE)** crea un valor único que resume el error en el modelo. Al elevar la diferencia, la métrica no tiene en cuenta la diferencia entre un exceso de predicción y la predicción incompleta.  
  
- **Error absoluto relativo (RAE)** es la diferencia absoluta relativa entre los valores esperados y reales; relativa porque la diferencia media se divide por la media aritmética.  
  
- **Cuadrático relativo (RSE) error** del mismo modo, normaliza el error cuadrático medio total de los valores previstos dividiendo el total de errores al cuadrado de los valores reales.  
  
- **Significa cero de un Error (MZOE)** indica si la predicción era correcta o no.  En otras palabras: `ZeroOneLoss(x,y) = 1` cuando `x!=y`; de lo contrario `0`.
  
- **Coeficiente de determinación**, que a menudo se conoce como R<sup>2</sup>, representa la eficacia predictiva del modelo como un valor entre 0 y 1. Cero significa que el modelo es aleatorio (se explica nada); 1 significa que hay un ajuste perfecto. Sin embargo, debe tener precaución al interpretar R<sup>2</sup> valores como valores bajos pueden ser completamente normales y los valores altos pueden ser sospechosa.
  

## <a name="next-steps"></a>Pasos siguientes

Consulte la [conjunto de módulos disponibles](module-reference.md) al servicio de Azure Machine Learning. 