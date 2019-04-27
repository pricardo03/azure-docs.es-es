---
title: Puntuaciones de predicción
titleSuffix: Language Understanding - Azure Cognitive Services
description: Una puntuación de predicción indica el grado de confianza que tiene el servicio LUIS API para los resultados de predicción, basándose en una declaración de usuario.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: diberry
ms.openlocfilehash: 383ce4c4248f7e21f745f503c74a29cb613983e2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60813997"
---
# <a name="prediction-scores-indicate-prediction-accuracy-for-intent-and-entities"></a>Las puntuaciones de predicción indican la precisión de la predicción de las intenciones y las entidades.

Una puntuación de predicción indica el grado de confianza tiene LUIS para resultados de predicción, basándose en una declaración de usuario.

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

Todas las predicciones de expresión devuelven una intención de puntuación superior. Esta predicción es una comparación numérica de puntuaciones de predicción. Las puntuaciones más altas 2 pueden tener una diferencia muy pequeña entre ellos. LUIS no indica esta proximidad distinto de devolver la puntuación de superior.  

## <a name="return-prediction-score-for-all-intents"></a>Devolver la puntuación de predicción de todas las intenciones

Un resultado de prueba o punto de conexión puede incluir todas las intenciones. Esta configuración se establece en el [punto de conexión](https://aka.ms/v1-endpoint-api-docs) con el par valor/nombre de cadena de consulta `verbose=true`.

## <a name="review-intents-with-similar-scores"></a>Revisar las intenciones con puntuaciones similares

Revisar la puntuación de todas las intenciones es una buena manera de comprobar que no solo se identifica la intención correcta, sino que la puntuación de la siguiente intención identificada es notable y sistemáticamente inferior en las expresiones.

Si varias intenciones tienen puntuaciones de predicción cercanas, en función del contexto de una expresión, LUIS puede alternar entre las intenciones. Para corregir esta situación, continuar agregando declaraciones para cada intención con una amplia variedad de diferencias contextuales o puede tener la aplicación cliente, como un bot de chat, mediante programación toman decisiones sobre cómo controlar los 2 intentos superiores.

Pueden invertir las intenciones de 2, que se puntúan demasiado-estrechamente, debido a la formación no determinista. La puntuación superior podría convertirse en la segunda superior y la puntuación segunda podría convertirse en la puntuación superior. Para evitar esta situación, agregar grabaciones de voz de ejemplo para cada una de las dos intenciones principales para esa declaración con la elección de palabras y el contexto que diferencia los 2 intentos. Las dos intenciones deben tener aproximadamente el mismo número de expresiones de ejemplo. Una regla general de separación para evitar la inversión debida al aprendizaje sería una diferencia del 15 % en las puntuaciones.

Puede desactivar el aprendizaje no determinista por [con todos los datos de entrenamiento](luis-how-to-train.md#train-with-all-data).

## <a name="differences-with-predictions-between-different-training-sessions"></a>Diferencias con las predicciones entre sesiones de entrenamiento distintos

Al entrenar el mismo modelo en otra aplicación y las puntuaciones no son iguales, esta diferencia es porque no hay cursos no determinista (un elemento de aleatoriedad). En segundo lugar, cualquier superposición de una expresión a más de una intención significa que la intención superior de la misma expresión puede cambiar en función del aprendizaje.

Si su bot de chat requiere una puntuación de LUIS específica para indicar confianza en una intención, debe usar la diferencia de puntuación entre las dos intenciones principales. Esta situación proporciona flexibilidad para las variaciones de entrenamiento.

## <a name="e-exponent-notation"></a>Notación E (exponencial)

Las puntuaciones de predicción pueden usar la notación exponencial, de forma que *aparecen* por encima del intervalo 0-1, como `9.910309E-07`. Esta puntuación es una indicación de un número muy **pequeño**.

|Puntuación de notación E |Puntuación real|
|--|--|
|9,910309E-07|,0000009910309|

## <a name="punctuation"></a>Signos de puntuación

Los signos de puntuación son un token independiente de LUIS. Una declaración que contenga un punto al final frente a una declaración que no contenga un punto al final son dos grabaciones de voz independientes y es posible que obtenga dos predicciones diferentes. Asegúrese de que el modelo controla los signos de puntuación, ya sea en las [expresiones de ejemplo](luis-concept-utterance.md) (que los tengan o no) o en los [patrones](luis-concept-patterns.md), donde es más sencillo pasar por alto los signos de puntuación con la sintaxis especial: `I am applying for the {Job} position[.]`

## <a name="next-steps"></a>Pasos siguientes

Vea [Add entities](luis-how-to-add-entities.md) (Agregar entidades) para obtener más información sobre cómo agregar entidades a la aplicación de LUIS.
