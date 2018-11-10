---
title: 'Puntuación de confianza: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Explicación de la puntuación de confianza
services: cognitive-services
author: tulasim88
manager: pchoudh
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 09/27/2018
ms.author: tulasim
ms.openlocfilehash: 46b6b789b2bab7e647e10a61939bd16b5ea8726d
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209716"
---
# <a name="confidence-score"></a>Puntuación de confianza
Cuando una consulta de usuario se compara con una base de conocimientos (KB), QnA Maker devuelve respuestas pertinentes, junto con una puntuación de confianza. Esta puntuación indica el grado de confianza que se tiene en que la respuesta sea la coincidencia correcta para la consulta de usuario en cuestión. 

La puntuación de confianza es un número entre 0 y 100. Una puntuación de 100 es probablemente una coincidencia exacta, mientras que una puntuación de 0 significa que no se ha encontrado ninguna respuesta coincidente. Cuanto mayor sea la puntuación, mayor será la confianza en la respuesta. Para una consulta determinada, podrían devolverse varias respuestas. En ese caso, las respuestas se devuelven en orden de mayor a menor puntuación de confianza.

En el ejemplo siguiente, puede ver una entidad QnA con dos preguntas. 


![Ejemplo de un par de QnA](../media/qnamaker-concepts-confidencescore/ranker-example-qna.png)

En el ejemplo anterior, se pueden esperar puntuaciones como el intervalo de puntuación que se muestra a continuación para los distintos tipos de consultas de usuario:


![Intervalo de puntuación del clasificador](../media/qnamaker-concepts-confidencescore/ranker-score-range.png)


En la tabla siguiente se indica la confianza típico asociada a un resultado determinado.

|Valor de la puntuación|Significado de la puntuación|Consulta de ejemplo|
|--|--|--|
|90-100|Coincidencia casi exacta de la consulta del usuario y una pregunta de KB.|"Mis cambios no se actualizan en Knowledge Base después de la publicación".|
|>70|Confianza alta. Normalmente, una buena respuesta que responde por completo a la consulta del usuario.|"He publicado mi KB pero no se actualiza".|
|50-70|Confianza media. Normalmente, una respuesta bastante buena que debería responder el propósito principal de la consulta del usuario.|"¿Debo guardar mis actualizaciones antes de publicar mi KB?"|
|30-50|Confianza baja. Normalmente, una respuesta relacionada que responde en parte a la intención del usuario.|"¿Qué hace Guardar y entrenar?"|
|<30|Confianza muy baja. Normalmente, no responde a la consulta del usuario, pero contiene algunas palabras o frases coincidentes. |"¿Donde puedo agregar sinónimos a mi KB?"|
|0|Ninguna coincidencia, por lo que no se devuelve una respuesta.|"¿Cuánto cuesta el servicio?"|

## <a name="choose-a-score-threshold"></a>Seleccionar un umbral de puntuación
En la tabla anterior se muestran las puntuaciones que se pueden esperar en la mayoría de KB. Aun así, como cada KB es diferente y contiene tipos de palabras, intenciones y objetivos distintos, se recomienda que pruebe y seleccione el umbral que mejor funcione en su caso. El umbral predeterminado y recomendado que debería funcionar para la mayoría de KB es **50**.

Al elegir el umbral, tenga en cuenta el equilibrio entre los valores de Precisión y Cobertura, y ajuste el umbral según sus requisitos.

- Si la **Precisión** es más importante en su caso, aumente el umbral. De este modo, cada vez que se devuelva una respuesta, el resultado será mucho más confiable y probablemente responderá a lo que buscan los usuarios. En este caso, podrían quedar más preguntas sin responder. *Por ejemplo:* si establece el umbral en **70**, podría perder la oportunidad de responder a algunos ejemplos ambiguos, como "¿Qué es Guardar y entrenar?".

- Si la **Cobertura** (o coincidencia) es más importante y quiere responder a tantas preguntas como sea posible, incluso aunque solo haya una relación parcial con la pregunta del usuario, reduzca el umbral. Esto significa que podría haber más casos en los que el resultado no responde a la consulta en sí del usuario, pero proporciona otra respuesta en cierto modo relacionada. *Por ejemplo:* si establece el umbral en **30**, podría proporcionar respuestas no muy relacionadas, como en el ejemplo anterior, para consultas del tipo "¿Dónde puedo editar mi KB?".


## <a name="improve-confidence-scores"></a>Mejorar las puntuaciones de confianza
Para mejorar la puntuación de confianza de una respuesta concreta a una consulta de usuario, puede agregar la consulta del usuario a la base de conocimiento como una pregunta alternativa en la respuesta.


## <a name="similar-confidence-scores"></a>Puntuaciones de confianza similares
Si varias respuestas tienen una puntuación de confianza similar, es probable que la consulta fuera demasiado genérica y, por tanto, coincidiera con la misma probabilidad con varias respuestas. Intente estructurar mejor sus preguntas y respuestas para que todas las entidades QnA tengan una intención distinta.


## <a name="confidence-score-differences"></a>Diferencias en las puntuaciones de confianza
La puntuación de confianza de una respuesta puede cambiar de manera apenas perceptible entre la prueba y la versión publicada de la base de conocimiento, aunque el contenido sea el mismo. Esto se debe a que el contenido de la prueba y la base de conocimiento publicada se encuentran en distintos índices de Azure Search.
Vea aquí cómo se lleva a cabo la operación de [publicación](../How-To/publish-knowledge-base.md).


## <a name="no-match-found"></a>No se encontraron coincidencias
Si el clasificador no encuentra ninguna buena coincidencia, se devuelve una puntuación de confianza de 0,0 o "None" (Ninguna) y la respuesta predeterminada no es "No good match found in the KB" (No se encontró ninguna buena coincidencia en KB)". Puede invalidar esta respuesta predeterminada en el código de aplicación o bot mediante una llamada al punto de conexión. Como alternativa, también puede establecer la respuesta de invalidación en Azure, lo que cambiará el valor predeterminado para todas las knowledge base implementada en un determinado servicio QnA Maker.

### <a name="change-default-answer"></a>Cambio de la respuesta predeterminada

1. Vaya a [Azure Portal](https://portal.azure.com) y navegue hasta el grupo de recursos que representa el servicio QnA Maker que creó.

2. Haga clic para abrir **App Service**.

    ![Acceder a App Service](../media/qnamaker-concepts-confidencescore/set-default-response.png)

3. Haga clic en **Configuración de la aplicación** y edite el campo **DefaultAnswer** a la respuesta predeterminada deseada. Haga clic en **Save**(Guardar).

    ![Cambiar la respuesta predeterminada](../media/qnamaker-concepts-confidencescore/change-response.png)

4. Reiniciar App Service

    ![Reinicio de App Service en QnA Maker](../media/qnamaker-faq/qnamaker-appservice-restart.png)


## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Orígenes de datos admitidos](./data-sources-supported.md)
## <a name="see-also"></a>Otras referencias 
[Introducción de QnA Maker](../Overview/overview.md)
