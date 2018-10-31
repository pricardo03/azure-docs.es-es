---
title: Puntuación de predicción, intenciones, entidades, LUIS
titleSuffix: Azure Cognitive Services
description: Una puntuación de predicción indica el grado de confianza que tiene LUIS en los resultados de la predicción.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: diberry
ms.openlocfilehash: 7cc3b6fd5865689df57258dcb2088bbef47b9a90
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637983"
---
# <a name="prediction-score"></a>Puntuación de predicción
Una puntuación de predicción indica el grado de confianza que tiene LUIS en los resultados de la predicción. 

Una puntuación de predicción se encuentra entre cero (0) y uno (1). Un ejemplo de una puntuación de LUIS de gran confianza es 0,99. Un ejemplo de una puntuación de confianza baja es 0,01. 

|Valor de la puntuación|Confianza|
|--|--|
|1|coincidencia clara|
|0,99|confianza alta|
|0,01|confianza baja|
|0|falta de coincidencia clara|

Cuando una expresión da como resultado una puntuación de confianza baja, LUIS lo pone de manifiesto en la página **Intención** del sitio web de [LUIS](luis-reference-regions.md), con el valor de **labeled-intent** identificado destacado en color rojo. 

![Discrepancia en la puntuación](./media/luis-concept-score/score-discrepancy.png)

## <a name="top-scoring-intent"></a>Intención de puntuación superior
Todas las predicciones de expresión devuelven una intención de puntuación superior. Se trata de una comparación numérica de puntuaciones de predicción. Puede haber una diferencia muy pequeña entre las dos puntuaciones superiores. LUIS no indica esta proximidad de otra forma que no sea devolver resultados.  

Si le preocupa la proximidad de las puntuaciones superiores, debería devolver la puntuación de todas las intenciones. Puede agregar expresiones a las dos intenciones que indiquen sus diferencias con la elección de palabras y la organización, o bien puede hacer que la aplicación que llama a LUIS, como un bot de chat, tome decisiones de programación sobre cómo controlar las dos intenciones superiores. 

Dos intenciones con puntuaciones muy cercanas se pueden invertir debido al aprendizaje no determinista. La puntuación superior podría convertirse en la segunda superior y la puntuación segunda podría convertirse en la puntuación superior. Con el fin de evitar esto, agregue expresiones de ejemplo a cada una de las dos intenciones principales para esa expresión con la elección de palabras y el contexto que diferencia las dos intenciones. Las dos intenciones deben tener aproximadamente el mismo número de expresiones de ejemplo. Una regla general de separación para evitar la inversión debida al aprendizaje sería una diferencia del 15 % en las puntuaciones.

## <a name="return-prediction-score-for-all-intents"></a>Devolver la puntuación de predicción de todas las intenciones
Un resultado de prueba o punto de conexión puede incluir todas las intenciones. Esta configuración se establece en el [punto de conexión](https://aka.ms/v1-endpoint-api-docs) con el par valor/nombre de cadena de consulta `verbose=true`. 

## <a name="review-intents-with-similar-scores"></a>Revisar las intenciones con puntuaciones similares
Revisar la puntuación de todas las intenciones es una buena manera de comprobar que no solo se identifica la intención correcta, sino que la puntuación de la siguiente intención identificada es notable y sistemáticamente inferior en las expresiones. 

Si varias intenciones tienen puntuaciones de predicción cercanas, en función del contexto de una expresión, LUIS puede alternar entre las intenciones. Para solucionar este problema, siga agregando expresiones a cada intención con una variedad más amplia de diferencias contextuales.   

## <a name="e-exponent-notation"></a>Notación E (exponencial)

Las puntuaciones de predicción pueden usar la notación exponencial, de forma que *aparecen* por encima del intervalo 0-1, como `9.910309E-07`. Esta puntuación es una indicación de un número muy **pequeño**.

|Puntuación de notación E |Puntuación real|
|--|--|
|9,910309E-07|,0000009910309|

## <a name="differences-with-predictions"></a>Diferencias con las predicciones
Si entrena el mismo modelo en otra aplicación y los resultados no son los mismos, esto es porque hay un elemento de aleatoriedad en el aprendizaje. En segundo lugar, cualquier superposición de una expresión a más de una intención significa que la intención superior de la misma expresión puede cambiar en función del aprendizaje.

Si el bot de chat requiere una puntuación de LUIS específica para indicar confianza en una intención, debe usar en su lugar la diferencia de puntuación entre las dos intenciones superiores. Esto proporciona flexibilidad para las variaciones en el aprendizaje. 

## <a name="punctuation"></a>Signos de puntuación
Los signos de puntuación son un token independiente de LUIS. Una expresión que contiene un punto final frente a una expresión que no lo tiene son dos expresiones distintas y pueden tener predicciones diferentes. Asegúrese de que el modelo controla los signos de puntuación ya sea en las [expresiones de ejemplo](luis-concept-utterance.md) (que tienen y no tienen signos de puntuación) o en los [patrones}(luis-concept-patterns.md), donde es más sencillo pasar por alto los signos de puntuación con la sintaxis especial: `I am applying for the {Job} position[.]`

## <a name="next-steps"></a>Pasos siguientes

Vea [Add entities](luis-how-to-add-entities.md) (Agregar entidades) para obtener más información sobre cómo agregar entidades a la aplicación de LUIS.