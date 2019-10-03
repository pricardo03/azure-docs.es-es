---
title: 'Guía de inicio rápido: Uso de Postman para obtener respuesta de la base de conocimiento con QnA Maker'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se ofrece orientación sobre cómo obtener una respuesta de la base de conocimiento mediante Postman.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 01/01/2019
ms.author: diberry
ms.openlocfilehash: 3df2703ba24091d0d21f06ae55aca389837e93d9
ms.sourcegitcommit: 4f3f502447ca8ea9b932b8b7402ce557f21ebe5a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71803045"
---
# <a name="quickstart-get-an-answer-from-knowledge-base-using-postman"></a>Guía de inicio rápido: Obtener una respuesta de la base de conocimiento mediante Postman

En esta guía de inicio rápido basada en Postman se ofrece orientación sobre cómo obtener una respuesta de una base de conocimiento.

## <a name="prerequisites"></a>Requisitos previos

* Última versión de [**Postman**](https://www.getpostman.com/).
* Debe tener un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md) y una [base de conocimiento con preguntas y respuestas](../Tutorials/create-publish-query-in-portal.md). 

## <a name="publish-to-get-endpoint"></a>Publicación para obtener puntos de conexión

Cuando esté listo para generar una respuesta a una pregunta de la base de conocimiento, [publique](../Quickstarts/create-publish-knowledge-base.md#publish-the-knowledge-base) la base de conocimiento.

## <a name="use-production-endpoint-with-postman"></a>Use el punto de conexión de producción con Postman

Cuando se publica la base de conocimiento, en la página **Publicar** se muestra la configuración de la solicitud HTTP para generar una respuesta. La vista predeterminada muestra la configuración necesaria para generar una respuesta de [Postman](https://www.getpostman.com).

Los números amarillos de la siguiente imagen indican qué pares de nombre-valor usar en los pasos siguientes.

[![Publicar resultados](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png)](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png#lightbox)

Para generar una respuesta con Postman, complete los pasos siguientes:

1. Abra Postman. Si se le pide que elija un bloque de creación, seleccione el bloque de creación **Basic Request** (Solicitud básica). Establezca el **nombre de la solicitud** como `Generate QnA Maker answer`y la **colección** como `Generate QnA Maker answers`. Si no desea guardar en una colección, seleccione el botón **Cancelar**.
1. En el área de trabajo, seleccione el método HTTP de **POST**.

    [![Establecimiento del método POST en Postman](../media/qnamaker-quickstart-get-answer-with-postman/postman-select-post-method.png)](../media/qnamaker-quickstart-get-answer-with-postman/postman-select-post-method.png#lightbox)

1. Para la dirección URL, concatene el valor HOST (el punto 2 de la imagen) y el valor POST (el punto 1 de la imagen) para crear la dirección URL completa. Una dirección URL de ejemplo completa se parece a esta: 

    `https://qnamaker-f0.azurewebsites.net/qnamaker/knowledgebases/e1115f8c-d01b-4698-a2ed-85b0dbf3348c/generateAnswer`

    [![Establecimiento de la URL completa en Postman](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png#lightbox)

1. Seleccione la pestaña **Encabezados** en la dirección URL y, después, seleccione **Edición en masa**. 

1. Copie los encabezados (los puntos 3 y 4 de la imagen) en el área de texto.

    [![En Postman, establezca los encabezados](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png#lightbox).

1. Seleccione la pestaña **Cuerpo**.
1. Seleccione el formato **sin procesar** y escriba el código JSON (el punto 5 de la imagen) que representa la pregunta.

    `{"question":"How do I programmatically update my Knowledge Base?"}`

    [![En Postman, establezca el valor JSON del cuerpo](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png#lightbox).

1. Seleccione el botón **Enviar**.
1. La respuesta contiene la propia respuesta con otra información que puede resultar importante para la aplicación cliente. 

    [![En Postman, establezca el valor JSON del cuerpo](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png)](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png#lightbox).

## <a name="use-staging-endpoint"></a>Uso de un punto de conexión de almacenamiento provisional

Si quiere obtener una respuesta del punto de conexión de almacenamiento provisional, anexe la dirección URL con el parámetro de cuerpo `isTest`.

## <a name="next-steps"></a>Pasos siguientes

La página de publicación también proporciona información para [generar una respuesta](get-answer-from-kb-using-curl.md) con cURL. 

> [!div class="nextstepaction"]
> [Usar metadatos al generar una respuesta](../How-to/metadata-generateanswer-usage.md)
