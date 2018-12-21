---
title: 'Guía de inicio rápido: Uso de Postman para obtener respuesta de la base de conocimiento con QnA Maker'
titlesuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se ofrece orientación sobre cómo obtener una respuesta de la base de conocimiento mediante Postman.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 12/11/2018
ms.author: diberry
ms.openlocfilehash: 476e982bdddd41c1daf06c3a673d964ce2a85f98
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53270899"
---
# <a name="quickstart-get-an-answer-from-knowledge-base-using-postman"></a>Guía de inicio rápido: Obtener una respuesta de la base de conocimiento mediante Postman

En esta guía de inicio rápido basada en Postman se ofrece orientación sobre cómo obtener una respuesta de una base de conocimiento.

## <a name="prerequisites"></a>Requisitos previos

* Última versión de [**Postman**](https://www.getpostman.com/).
* Debe tener un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md) y una [base de conocimiento con preguntas y respuestas](../Tutorials/create-publish-query-in-portal.md). 

## <a name="publish-to-get-endpoint"></a>Publicación para obtener puntos de conexión

Cuando esté listo para generar una respuesta a una pregunta de la base de conocimiento, [publique](../How-to/publish-knowledge-base.md) la base de conocimiento.

## <a name="use-production-endpoint-with-postman"></a>Use el punto de conexión de producción con Postman

Cuando se publica la base de conocimiento, en la página **Publicar** se muestra la configuración de la solicitud HTTP para generar una respuesta. La vista predeterminada muestra la configuración necesaria para generar una respuesta de [Postman](https://www.getpostman.com).

[![Publicar los resultados](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png)](../media/qnamaker-quickstart-get-answer-with-postman/publish-settings.png#lightbox)

Para generar una respuesta con Postman, complete los pasos siguientes:

1. Abra Postman. 
1. Seleccione el bloque de creación para crear una solicitud básica.
1. Establezca el **nombre de la solicitud** como `Generate QnA Maker answer`y la **colección** como `Generate QnA Maker answers`.
1. En el área de trabajo, seleccione el método HTTP de **POST**.
1. Para la dirección URL, concatene el valor HOST y el valor Post para crear la dirección URL completa. 

    [![En Postman, establezca el método para Post y la dirección URL completa](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-method-and-url.png#lightbox).

1. Seleccione la pestaña **Encabezados** en la dirección URL y, después, seleccione **Edición en masa**. 
1. Copie los encabezados en el área de texto.

    [![En Postman, establezca los encabezados](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-headers.png#lightbox).

1. Seleccione la pestaña **Cuerpo**.
1. Seleccione el formato **sin procesar** y escriba el código JSON que representa la pregunta.

    [![En Postman, establezca el valor JSON del cuerpo](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png)](../media/qnamaker-quickstart-get-answer-with-postman/set-postman-body-json-value.png#lightbox).

1. Seleccione el botón **Enviar**.
1. La respuesta contiene la propia respuesta con otra información que puede resultar importante para la aplicación cliente. 

    [![En Postman, establezca el valor JSON del cuerpo](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png)](../media/qnamaker-quickstart-get-answer-with-postman/receive-postman-response.png#lightbox).

## <a name="use-staging-endpoint-with-curl"></a>Uso de punto de conexión de almacenamiento provisional con cURL

Si desea obtener una respuesta desde el punto de conexión de almacenamiento provisional, utilice el parámetro booleano querystring `isTest` con el valor de `true`.

`isTest=true`

## <a name="next-steps"></a>Pasos siguientes

La página de publicación también proporciona información para [generar una respuesta](get-answer-from-kb-using-curl.md) con cURL. 

> [!div class="nextstepaction"]
> [Usar metadatos al generar una respuesta](../How-to/metadata-generateanswer-usage.md)