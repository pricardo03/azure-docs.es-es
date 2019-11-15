---
title: 'Base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Una base de conocimiento de QnA Maker consta de un conjunto de pares de preguntas y respuestas, así como metadatos opcionales asociados a cada par de pregunta y respuesta.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 355556e98300ecad6aa3141f0f4ab14b834cd91e
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2019
ms.locfileid: "73794894"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>¿Qué es una base de conocimiento de QnA Maker?

Una base de conocimiento de QnA Maker consta de un conjunto de pares de preguntas y respuestas, así como metadatos opcionales asociados a cada par de pregunta y respuesta.

## <a name="key-knowledge-base-concepts"></a>Conceptos clave de bases de conocimiento

* **Preguntas**: una pregunta contiene el texto que mejor representa la consulta de un usuario. 
* **Respuestas**: una respuesta es la respuesta que se devuelve cuando una consulta de usuario coincide con la pregunta asociada.  
* **Metadatos**: los metadatos son etiquetas asociadas a un par de QnA y se representan como pares de clave-valor. Las etiquetas de metadatos se usan para filtrar los pares de QnA y limitar el conjunto sobre el que se realiza la coincidencia de la consulta.

Una única QnA, representada por un identificador numérico de QnA, tiene varias variantes de una pregunta (preguntas alternativas), todas asignadas a una sola respuesta. Además, cada uno de estos pares puede tener varios campos de metadatos asociados: una clave y un valor.

![Bases de conocimiento de QnA Maker](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Formato del contenido de la base de conocimiento

Al introducir contenido enriquecido en una base de conocimiento, QnA Maker intenta convertir el contenido en Markdown. Lea [este blog](https://aka.ms/qnamaker-docs-markdown-support) para conocer los formatos de Markdown comprensibles para la mayoría de los clientes de chat.

Los campos de metadatos constan de pares de clave-valor separados por dos puntos, como Producto:Trituradora. Tanto la clave como el valor deben ser de solo texto. La clave de metadatos no debe contener espacios en blanco. Los metadatos admiten solo un valor por clave.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Cómo QnA Maker procesa una consulta de usuario para seleccionar la mejor respuesta

La base de conocimiento entrenada y [publicada](/azure/cognitive-services/qnamaker/quickstarts/create-publish-knowledge-base#publish-the-knowledge-base) de QnA Maker recibe una consulta de usuario, ya sea desde un bot o de otra aplicación cliente, en [GenerateAnswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage). En el diagrama siguiente se muestra el proceso de recepción de la consulta de usuario.

![El proceso de clasificación de una consulta de usuario](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

### <a name="ranker-process"></a>Proceso del clasificador

El proceso se explica en la tabla siguiente.

|Paso|Propósito|
|--|--|
|1|La aplicación cliente envía la consulta de usuario a [GenerateAnswer API](/azure/cognitive-services/qnamaker/how-to/metadata-generateanswer-usage).|
|2|QnA Maker preprocesa la consulta de usuario con la detección del idioma, los correctores ortográficos y los separadores de palabras.|
|3|Este preprocesamiento se realiza para modificar la consulta de usuario y obtener los mejores resultados de la búsqueda.|
|4|Esta consulta modificada se envía al índice de Azure Cognitive Search, que recibe el número `top` de resultados. Si no se encuentra la respuesta correcta en estos resultados, aumente levemente el valor de `top`. Por lo general, un valor de 10 para `top` funciona en el 90 % de las consultas.|
|5|QnA Maker aplica la caracterización avanzada para determinar la corrección de los resultados de búsqueda capturados para la consulta de usuario. |
|6|El modelo de clasificador entrenado usa la puntuación de característica (del paso 5) para clasificar los resultados de Azure Cognitive Search.|
|7|Los resultados nuevos se devuelven a la aplicación cliente en el orden de clasificación.|
|||

Las características que se usan incluyen, entre otros, la semántica en el nivel de palabra, la importancia en el nivel de término en un conjunto y los modelos de semántica de aprendizaje profundo para determinar la similitud e importancia entre dos cadenas de texto.

## <a name="http-request-and-response-with-endpoint"></a>Solicitud y respuesta HTTP con punto de conexión
Al publicar la base de conocimiento, el servicio crea un punto de conexión HTTP basado en REST que se puede integrar en una aplicación, normalmente un bot de chat. 

### <a name="the-user-query-request-to-generate-an-answer"></a>Solicitud de consulta de usuario para generar una respuesta

Una consulta de usuario es la pregunta que el usuario final formula en la base de conocimiento, como por ejemplo `How do I add a collaborator to my app?`. A menudo la consulta está en un formato de lenguaje natural o se usan unas pocas palabras clave que representan la pregunta, como por ejemplo `help with collaborators`. La consulta se envía a la base de conocimiento desde una solicitud HTTP en la aplicación cliente.

```json
{
    "question": "qna maker and luis",
    "top": 6,
    "isTest": true,
    "scoreThreshold": 20,
    "strictFilters": [
    {
        "name": "category",
        "value": "api"
    }],
    "userId": "sd53lsY="
}
```

Puede controlar la respuesta estableciendo propiedades como [scoreThreshold](./confidence-score.md#choose-a-score-threshold), [top](../how-to/improve-knowledge-base.md#use-the-top-property-in-the-generateanswer-request-to-get-several-matching-answers) y [strictFilters](../how-to/metadata-generateanswer-usage.md#filter-results-with-strictfilters-for-metadata-tags).

Use [contexto de conversación](../how-to/metadata-generateanswer-usage.md#use-question-and-answer-results-to-keep-conversation-context) con la [función de varios turnos](../how-to/multiturn-conversation.md) para mantener la conversación en marcha con el fin de refinar las preguntas y respuestas y encontrar la respuesta correcta y final.

### <a name="the-response-from-a-call-to-generate-an-answer"></a>Respuesta de una llamada para generar una respuesta

La respuesta HTTP es la respuesta que se recupera de la base de conocimientos, en función de la coincidencia más alta para una consulta de usuario dada. La respuesta incluye la respuesta y la puntuación de predicción. Si ha solicitado más de una respuesta principal, con la propiedad `top`, obtendrá más de una respuesta principal, cada una con una puntuación. 

```json
{
    "answers": [
        {
            "questions": [
                "What is the closing time?"
            ],
            "answer": "10.30 PM",
            "score": 100,
            "id": 1,
            "source": "Editorial",
            "metadata": [
                {
                    "name": "restaurant",
                    "value": "paradise"
                },
                {
                    "name": "location",
                    "value": "secunderabad"
                }
            ]
        }
    ]
}
```

### <a name="test-and-production-knowledge-base"></a>Base de conocimiento de prueba y producción
Una base de conocimiento es el repositorio de preguntas y respuestas que se crea, se mantiene y se usa en QnA Maker. Cada nivel de QnA Maker se puede usar para varias bases de conocimiento.

Una base de conocimiento tiene dos estados: *prueba* y *publicada*.

La *base de conocimiento de prueba* es la versión que se edita, se guarda y se prueba para conocer la precisión e integridad de las respuestas. Los cambios realizados en la base de conocimientos de prueba no afectan al usuario final de la aplicación o bot de chat. La base de conocimiento de prueba se conoce como `test` en la solicitud HTTP. 

La *base de conocimiento publicada* es la versión que se utiliza en la aplicación o en el bot de chat. La acción de publicar una base de conocimiento coloca el contenido de la base de conocimientos de prueba en la versión publicada de la base de conocimiento. Dado que la base de conocimiento publicada es la versión que la aplicación usa mediante el punto de conexión, asegúrese de que el contenido es correcto y que se ha probado profusamente. La base de conocimiento publicada se conoce como `prod` en la solicitud HTTP.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ciclo de vida de desarrollo de una base de conocimiento](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Otras referencias

[Introducción de QnA Maker](../Overview/overview.md)

Cree y edite la base de conocimiento con: 
* [API DE REST](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/knowledgebase)
* [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.knowledgebase?view=azure-dotnet)

Genere una respuesta con: 
* [API DE REST](https://docs.microsoft.com/rest/api/cognitiveservices/qnamakerruntime/runtime/generateanswer)
* [SDK de .NET](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.knowledge.qnamaker.runtime?view=azure-dotnet)
