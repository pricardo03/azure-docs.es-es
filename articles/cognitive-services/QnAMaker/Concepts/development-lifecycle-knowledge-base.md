---
title: 'Ciclo de vida de una base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker aprende mejor en un ciclo iterativo de cambios en el modelo, ejemplos de expresiones, publicación y recopilación de datos de las consultas de punto de conexión.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/16/2019
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 3f78b8a2566137d596f4ab3f083e1d14289365c3
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59684028"
---
# <a name="knowledge-base-lifecycle-in-qna-maker"></a>Ciclo de vida de una base de conocimiento de QnA Maker
QnA Maker aprende mejor en un ciclo iterativo de cambios en el modelo, ejemplos de expresiones, publicación y recopilación de datos de las consultas de punto de conexión. 

![Ciclo de creación](../media/qnamaker-concepts-lifecycle/kb-lifecycle.png)

## <a name="creating-a-qna-maker-knowledge-base"></a>Creación de una base de conocimiento de QnA Maker
El punto de conexión de la base de conocimiento de QnA Maker proporciona una respuesta mejor para las consultas de usuario basadas en el contenido de la base de conocimiento. Crear una base de conocimiento es una acción que se realiza una vez para configurar un repositorio de contenido de preguntas, respuestas y los metadatos asociados. Para crear una base de datos de conocimiento, se puede rastrear el contenido existente, como páginas de preguntas más frecuentes, manuales de productos o pares de pregunta-respuesta estructurados. Aprenda cómo [crear una base de conocimiento](../How-To/create-knowledge-base.md).

## <a name="testing-and-updating-the-knowledge-base"></a>Probar y actualizar la base de conocimiento

La base de conocimiento está lista para probarla una vez que se llena con el contenido, bien editorialmente o mediante extracción automática. Las pruebas interactivas que pueden realizarse en el portal de QnA Maker a través de la **prueba** panel especificando las consultas comunes de usuario y comprobar que las respuestas se devuelven con la respuesta correcta y la puntuación de confianza suficiente. 

* **Para corregir las puntuaciones de confianza baja**: agregar preguntas alternativas. 
* **Cuando una consulta devuelve incorrectamente el [respuesta predeterminada](confidence-score.md#change-default-answer)**: agregar nuevas respuestas a la pregunta correcta. 

Este bucle ajustado de prueba-actualización continúa hasta que esté satisfecho con los resultados. Vea cómo [probar la base de conocimiento](../How-To/test-knowledge-base.md).

Para artículos de KB de gran tamaño, use pruebas automatizadas con el [generateAnswer API](../how-to/metadata-generateanswer-usage.md#get-answer-predictions-with-the-generateanswer-api) y `isTest` body (propiedad) en las consultas que el `test` knowledge base en lugar de la base de conocimiento publicada. 

```json
{
  "question": "example question",
  "top": 3,
  "userId": "Default",
  "isTest": true
}
```

## <a name="publish-the-knowledge-base"></a>Publicación de una base de conocimiento
Cuando haya terminado de probar la base de conocimiento, puede publicarla. Al publicar, se inserta la versión más reciente de la base de conocimiento probada en un índice de Azure Search dedicado que representa la base de conocimiento **publicada**. También se crea un punto de conexión al que puede llamar en su aplicación o bot de chat.

De esta manera, los cambios realizados en la versión de prueba de la base de conocimiento no afectan a la versión publicada que podría estar activa en una aplicación de producción.

Cada una de estas bases de conocimiento se pueden probar por separado. Mediante las API, puede tener como destino la versión de prueba de la base de conocimiento con `isTest` body (propiedad) en la llamada generateAnswer.

Vea cómo [publicar la base de conocimiento](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base).

## <a name="monitor-usage"></a>Supervisión de uso
Para poder registrar los registros de chat de su servicio, deberá habilitar Application Insights cuando [cree su servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md).

Puede obtener diversos datos de análisis de su uso del servicio. Obtenga más información sobre cómo utilizar Application Insights para obtener [datos de análisis del servicio QnA Maker](../How-To/get-analytics-knowledge-base.md).

Según la información que obtenga de los análisis, realice [las actualizaciones correspondientes en su base de conocimiento](../How-To/edit-knowledge-base.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Puntuación de confianza](./confidence-score.md)

## <a name="see-also"></a>Vea también 

[Base de conocimiento](./knowledge-base.md)
[Información general de QnA Maker](../Overview/overview.md)
