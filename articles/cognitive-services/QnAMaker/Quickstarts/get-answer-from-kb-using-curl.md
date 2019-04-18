---
title: 'Inicio rápido: Uso de cURL para obtener respuesta de la base de conocimiento con QnA Maker'
titleSuffix: Azure Cognitive Services
description: En este artículo de inicio rápido se ofrece orientación sobre cómo obtener una respuesta de la base de conocimiento mediante cURL.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 04/16/2019
ms.author: diberry
ms.openlocfilehash: ad4b40d649d4e4cbc17d6aec5d8bc7308012b927
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59677432"
---
# <a name="quickstart-get-answer-from-knowledge-base-using-curl"></a>Inicio rápido: Obtener respuesta de la base de conocimiento mediante cURL

En este artículo de inicio rápido basado en cURL se ofrece orientación sobre cómo obtener una respuesta de la base de conocimiento.

## <a name="prerequisites"></a>Requisitos previos

* Versión más reciente de [**cURL**](https://curl.haxx.se/).
* Tiene que tener un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md) y una [base de conocimiento con preguntas y respuestas](../Tutorials/create-publish-query-in-portal.md).

## <a name="publish-to-get-endpoint"></a>Publicación para obtener puntos de conexión

Cuando esté listo para generar una respuesta a una pregunta de la base de conocimiento, [publique](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) la base de conocimiento.

## <a name="use-production-endpoint-with-curl"></a>Uso del punto de conexión de producción con cURL

Cuando se publica la base de conocimiento, en la página **Publicar** se muestra la configuración de la solicitud HTTP para generar una respuesta. La pestaña **CURL** muestra la configuración necesaria para generar una respuesta de la herramienta de línea de comandos, [CURL](https://www.getpostman.com).

[![Publicar resultados](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png)](../media/qnamaker-use-to-generate-answer/curl-command-on-publish-page.png#lightbox)

Para generar una respuesta con CURL, complete los pasos siguientes:

1. Copie el texto en la pestaña CURL. 
1. Abra una línea de comandos o terminal y pegue el texto.
1. Edite la pregunta para que sea apropiada para su base de conocimiento. Tenga cuidado de no quitar el JSON contenido rodeando la pregunta.
1. Escriba el comando. 
1. La respuesta incluye la información pertinente acerca de la respuesta. 

    ```bash
    > curl -X POST https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/1111f8c-d01b-4698-a2de-85b0dbf3358c/generateAnswer -H "Authorization: EndpointKey 111841fb-c208-4a72-9412-03b6f3e55ca1" -H "Content-type: application/json" -d "{'question':'How do I programmatically update my Knowledge Base?'}"
    {
      "answers": [
        {
          "questions": [
            "How do I programmatically update my Knowledge Base?"
          ],
          "answer": "You can use our REST APIs to manage your Knowledge Base. See here for details: https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600",
          "score": 100.0,
          "id": 18,
          "source": "Custom Editorial",
          "metadata": [
            {
              "name": "category",
              "value": "api"
            }
          ]
        }
      ]
    }
    ```

## <a name="use-staging-endpoint-with-curl"></a>Uso de un punto de conexión de almacenamiento provisional con cURL

Si desea obtener una respuesta desde el punto de conexión de almacenamiento provisional, utilice el `isTest` body (propiedad).

```json
isTest:true
```

## <a name="next-steps"></a>Pasos siguientes

La página de publicación también proporciona información para [generar una respuesta](get-answer-from-kb-using-postman.md) con Postman. 

> [!div class="nextstepaction"]
> [Usar metadatos al generar una respuesta](../How-to/metadata-generateanswer-usage.md)
