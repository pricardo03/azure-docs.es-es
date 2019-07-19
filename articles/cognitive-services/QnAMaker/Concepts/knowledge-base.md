---
title: 'Base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Una base de conocimiento de QnA Maker consta de un conjunto de pares de preguntas y respuestas (QnA), y metadatos opcionales asociados a cada par de QnA.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: b9562a1686c4de4f4e2ef57a7d91bbf18dce63ef
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67447593"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>¿Qué es una base de conocimiento de QnA Maker?

Una base de conocimiento de QnA Maker consta de un conjunto de pares de preguntas y respuestas (QnA), y metadatos opcionales asociados a cada par de QnA.

## <a name="key-knowledge-base-concepts"></a>Conceptos clave de bases de conocimiento

* **Preguntas**: una pregunta contiene el texto que mejor representa la consulta de un usuario. 
* **Respuestas**: una respuesta es la respuesta que se devuelve cuando una consulta de usuario coincide con la pregunta asociada.  
* **Metadatos**: los metadatos son etiquetas asociadas a un par de QnA y se representan como pares de clave-valor. Las etiquetas de metadatos se usan para filtrar los pares de QnA y limitar el conjunto sobre el que se realiza la coincidencia de la consulta.

Una única QnA, representada por un identificador numérico de QnA, tiene varias variantes de una pregunta (preguntas alternativas), todas asignadas a una sola respuesta. Además, cada uno de estos pares puede tener varios campos de metadatos asociados: una clave y un valor.

![Bases de conocimiento de QnA Maker](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Formato del contenido de la base de conocimiento

Al introducir contenido enriquecido en una base de conocimiento, QnA Maker intenta convertir el contenido en Markdown. Lea [este](https://aka.ms/qnamaker-docs-markdown-support) blog para conocer los formatos de Markdown comprensibles para la mayoría de los clientes de chat.

Los campos de metadatos constan de pares de clave-valor separados por dos puntos **(Producto:Trituradora)** . La clave y el valor deben ser de solo texto. La clave de metadatos no debe contener espacios en blanco. Los metadatos admiten solo un valor por clave.

## <a name="how-qna-maker-processes-a-user-query-to-select-the-best-answer"></a>Cómo QnA Maker procesa una consulta de usuario para seleccionar la mejor respuesta

La base de conocimiento entrenada y [publicada](/quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) de QnA Maker recibe una consulta de usuario, ya sea desde un bot o de otra aplicación cliente, en [GenerateAnswer API](/how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api). En el diagrama siguiente se muestra el proceso de recepción de la consulta de usuario.

![El proceso de clasificación de una consulta de usuario](../media/qnamaker-concepts-knowledgebase/rank-user-query-first-with-azure-search-then-with-qna-maker.png)

El proceso se explica en la tabla siguiente:

|Paso|Propósito|
|--|--|
|1|La aplicación cliente envía la consulta de usuario a [GenerateAnswer API](/how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api).|
|2|Qna Maker preprocesa la consulta de usuario con la detección del idioma, los correctores ortográficos y los separadores de palabras.|
|3|Este preprocesamiento se realiza para modificar la consulta de usuario y obtener mejores resultados de la búsqueda.|
|4|Esta consulta modificada se envía al índice de Azure Search y se recibe el número `top` de resultados. Si no se encuentra la respuesta correcta en estos resultados, aumente levemente el valor de `top`. Por lo general, un valor de 10 para `top` funciona en el 90 % de las consultas.|
|5|QnA Maker aplica la caracterización avanzada para determinar la corrección de los resultados de Azure Search capturados para la consulta de usuario. |
|6|El modelo de clasificador entrenado usa la puntuación de característica (del paso 5) para clasificar los resultados de Azure Search.|
|7|Los resultados nuevos se devuelven a la aplicación cliente en el orden de clasificación.|
|||

Las características que se usan incluyen, entre otros, la semántica en el nivel de palabra, la importancia en el nivel de término en un conjunto y los modelos de semántica de aprendizaje profundo para determinar la similitud e importancia entre dos cadenas de texto.


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ciclo de vida de desarrollo de una base de conocimiento](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Otras referencias

[Introducción de QnA Maker](../Overview/overview.md)
