---
title: 'Guía de inicio rápido: Actualización de la base de conocimiento con REST y Node.js en QnA Maker'
titleSuffix: Azure Cognitive Services
description: Esta guía de inicio rápido le lleva por la actualización mediante programación de una base de conocimiento (KB) existente de QnA Maker.  Este código JSON le permite actualizar una base de conocimiento mediante la incorporación de nuevos orígenes de datos y el cambio o la eliminación de orígenes de datos.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 10/02/2018
ms.author: diberry
ms.openlocfilehash: 3bbc55b3bb064b2cf4b140a395e99209b71a5ce1
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816246"
---
# <a name="quickstart-update-a-qna-maker-knowledge-base-in-nodejs"></a>Guía de inicio rápido: Actualización de una base de conocimiento de QnA Maker en Node.js

Esta guía de inicio rápido le lleva por la actualización mediante programación de una base de conocimiento (KB) existente de QnA Maker.  Este código JSON le permite actualizar una base de conocimiento mediante la incorporación de nuevos orígenes de datos y el cambio o la eliminación de orígenes de datos.

Esta API es equivalente a editar y luego usar el botón **Save and train** (Guardar y entrenar) del portal de QnA Maker.

En esta guía de inicio rápido se llama a las QnA Maker API:
* [Actualización](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da7600): el modelo de la base de conocimiento se define en el código JSON enviado en el cuerpo de la solicitud de API. 
* [Obtención de los detalles de la operación](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)

## <a name="prerequisites"></a>Requisitos previos

* [Node.js 6+](https://nodejs.org/en/download/)
* Debe tener un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Para recuperar la clave, seleccione **Claves** en **Administración de recursos** en el panel. 
* Identificador de la base de conocimiento (KB) de QnA Maker encontrado en la dirección URL en el parámetro de cadena de consulta de KBID, como se muestra a continuación.

    ![Identificador de base de conocimiento de QnA Maker](../media/qnamaker-quickstart-kb/qna-maker-id.png)

Si aún no tiene una base de conocimiento, puede crear una de ejemplo para usar en esta guía de inicio rápido: [Creación de una nueva base de conocimiento](create-new-kb-nodejs.md).

[!INCLUDE [Code is available in Azure-Samples Github repo](../../../../includes/cognitive-services-qnamaker-nodejs-repo-note.md)]

## <a name="create-a-knowledge-base-nodejs-file"></a>Creación de un archivo de base de conocimiento de Node.js

Cree un archivo llamado `update-knowledge-base.js`.

## <a name="add-the-required-dependencies"></a>Incorporación de las dependencias necesarias

En la parte superior de `update-knowledge-base.js`, agregue las líneas siguientes para agregar las dependencias necesarias al proyecto:

[!code-nodejs[Add the dependencies](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=1-4 "Add the dependencies")]

## <a name="add-required-constants"></a>Incorporación de las constantes necesarias
Después de las dependencias necesarias anteriores, agregue las constantes necesarias para acceder a QnA Maker. Sustituya el valor de la variable `subscriptionKey` por su propia clave de QnA Maker. 

[!code-nodejs[Add required constants](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=10-17 "Add required constants")]

## <a name="add-knowledge-base-id"></a>Incorporación del identificador de la base de conocimiento

Después de las constantes anteriores, agregue el identificador de la base de conocimiento y agréguelo a la ruta de acceso:

[!code-nodejs[Add knowledge base ID](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=19-23 "Add knowledge base ID")]

## <a name="add-the-kb-update-model-definition"></a>Incorporación de la definición del modelo de actualización de la base de conocimiento

Después de las constantes, agregue la siguiente definición de actualización de la base de conocimiento. La definición de la actualización tiene tres secciones:

* agregar
* update
* delete

Cada sección puede usarse en la misma solicitud única a la API. 

[!code-nodejs[Add the KB update definition](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=25-53 "Add the KB update definition")]


## <a name="add-supporting-functions"></a>Incorporación de funciones auxiliares

A continuación, agregue las siguientes funciones auxiliares.

1. Agregue la siguiente función para imprimir el código JSON en un formato legible:

   [!code-nodejs[Add supporting functions, step 1](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=55-58 "Add supporting functions, step 1")]

2. Agregue las siguientes funciones para administrar la respuesta HTTP y obtener el estado de la operación de creación:

   [!code-nodejs[Add supporting functions, step 2](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=60-82 "Add supporting functions, step 2")]

## <a name="add-patch-request-to-update-kb"></a>Incorporación de la solicitud PATCH para actualizar la base de conocimientos

Agregue las siguientes funciones para realizar una solicitud HTTP PATCH a fin de actualizar la base de conocimiento. `Ocp-Apim-Subscription-Key` es la clave del servicio QnA Maker, usada para la autenticación.

[!code-nodejs[Add PATCH request to update KB](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=84-111 "Add PATCH request to update KB")]

Esta llamada API devuelve una respuesta JSON que incluye el identificador de operación. El identificador de operación es necesario para solicitar el estado si la operación no se completa.

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-10-02T01:23:00Z",
  "lastActionTimestamp": "2018-10-02T01:23:00Z",
  "userId": "335c3841df0b42cdb00f53a49d51a89c",
  "operationId": "e7be3897-88ff-44e5-a06c-01df0e05b78c"
}
```

## <a name="add-get-request-to-determine-operation-status"></a>Incorporación de la solicitud GET para determinar el estado de la operación

Compruebe el estado de la operación.
    
[!code-nodejs[Add GET request to determine operation status](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=113-137 "Add GET request to determine operation status")]

Esta llamada API devuelve una respuesta JSON que incluye el estado de la operación: 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

Repita la llamada hasta obtener éxito o error: 

```JSON
{
  "operationState": "Succeeded",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:23:08Z",
  "resourceLocation": "/knowledgebases/XXX7892b-10cf-47e2-a3ae-e40683adb714",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "177e12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-updatekb-method"></a>Incorporación del método update_kb

El método siguiente actualiza la base de conocimiento y repite las comprobaciones sobre el estado. Dado que la creación de la base de conocimiento puede tardar algún tiempo, deberá repetir las llamadas para comprobar el estado hasta que sea correcto o se produzca un error.

[!code-nodejs[Add update_kb method](~/samples-qnamaker-nodejs/documentation-samples/quickstarts/update-knowledge-base/update-knowledge-base.js?range=139-169 "Add update_kb method")]

## <a name="run-the-program"></a>Ejecución del programa

Escriba el siguiente comando en una línea de comandos para ejecutar el programa. La solicitud se envía a QnA Maker API para actualizar la base de conocimiento y, luego, se sondean los resultados cada 30 segundos. Cada respuesta se imprime en la ventana de consola.

```bash
node update-knowledge-base.js
```

Una vez actualizada la base de conocimiento, puede verla en el portal de QnA Maker en la página [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (Mis bases de conocimiento). 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de QnA Maker (V4) REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)