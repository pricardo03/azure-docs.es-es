---
title: 'Base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: Una base de conocimiento de QnA Maker consta de un conjunto de pares de preguntas y respuestas (QnA), y metadatos opcionales asociados a cada par de QnA.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 865525d9c978f3168a4c580dec20f0f91f0a0d16
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074101"
---
# <a name="what-is-a-qna-maker-knowledge-base"></a>¿Qué es una base de conocimiento de QnA Maker?

Una base de conocimiento de QnA Maker consta de un conjunto de pares de preguntas y respuestas (QnA), y metadatos opcionales asociados a cada par de QnA.

## <a name="key-knowledge-base-concepts"></a>Conceptos clave de bases de conocimiento

* **Preguntas**: una pregunta contiene el texto que mejor representa la consulta de un usuario. 
* **Respuestas**: una respuesta es la respuesta que se devuelve cuando una consulta de usuario coincide con la pregunta asociada.  
* **Metadatos**: los metadatos son etiquetas asociadas a un par de QnA y se representan como pares de clave-valor. Los metadatos se usan para filtrar los pares de QnA y limitar el conjunto sobre el que se realiza la comparación de la consulta.

Una única QnA, representada por un identificador numérico de QnA, tiene varias variantes de una pregunta (preguntas alternativas), todas asignadas a una sola respuesta. Además, cada uno de estos pares puede tener varios campos de metadatos asociados.

![Bases de conocimiento de QnA Maker](../media/qnamaker-concepts-knowledgebase/knowledgebase.png) 

## <a name="knowledge-base-content-format"></a>Formato del contenido de la base de conocimiento

Al introducir contenido enriquecido en una base de conocimiento, QnA Maker intenta convertir el contenido en Markdown. Lea [este](https://aka.ms/qnamaker-docs-markdown-support) blog para conocer los formatos de Markdown comprensibles para la mayoría de los clientes de chat.

Los campos de metadatos constan de pares de clave-valor separados por dos puntos **(Producto:Trituradora)**. La clave y el valor deben ser de solo texto. La clave de metadatos no debe contener espacios en blanco.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ciclo de vida de desarrollo de una base de conocimiento](./development-lifecycle-knowledge-base.md)

## <a name="see-also"></a>Otras referencias

[Introducción de QnA Maker](../Overview/overview.md)