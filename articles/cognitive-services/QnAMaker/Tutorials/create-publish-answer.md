---
title: Crear, publicar, responder
titleSuffix: QnA Maker - Azure Cognitive Services
description: Este tutorial basado en REST le guía mediante la creación y publicación de una base de conocimiento mediante programación y de la posterior respuesta a una pregunta desde dicha base de conocimiento.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: tutorial
ms.date: 01/24/2019
ms.author: diberry
ms.openlocfilehash: d209d73d67af96e99589dddcb71b6b50214356ee
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58877285"
---
# <a name="tutorial-using-c-create-knowledge-base-then-answer-question"></a>Tutorial: Uso de C# para crear una base de conocimiento y responder a las preguntas

Este tutorial le guía mediante la creación y publicación de una base de conocimiento mediante programación y de la posterior respuesta a una pregunta de un cliente con dicha base de conocimiento. 

> [!div class="checklist"]
> * Creación de una base de conocimientos 
> * Comprobación del estado de creación
> * Entrenamiento y publicación de la base de conocimiento
> * Obtención de información del punto de conexión
> * Use Curl para consultar la base de conocimiento


En esta guía de inicio rápido se llama a las siguientes API de QnA Maker:

* [Creación de una base de conocimientos](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
* [Obtención de los detalles de la operación](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/operations_getoperationdetails)
* [Obtención de los detalles de la base de conocimientos](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/knowledgebases_getknowledgebasedetails) 
* [Obtención de los puntos de conexión de la base de conocimientos](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/endpointkeys_getendpointkeys)
* [Publicar](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe) 

## <a name="prerequisites"></a>Requisitos previos

* Versión de [**Visual Studio Community Edition**](https://www.visualstudio.com/downloads/) más reciente.
* Debe tener un [servicio QnA Maker](../How-To/set-up-qnamaker-service-azure.md). Para recuperar la clave, seleccione **Claves** en **Administración de recursos** en el panel. 

> [!NOTE] 
> Los archivos de la solución completa están disponibles en el repositorio [**Azure-Samples/cognitive-services-qnamaker-csharp** de GitHub](https://github.com/Azure-Samples/cognitive-services-qnamaker-csharp/tree/master/documentation-samples/tutorials/create-publish-answer-knowledge-base).

## <a name="create-a-knowledge-base-project"></a>Creación de un proyecto de base de conocimiento

[!INCLUDE [Create Visual Studio Project](../../../../includes/cognitive-services-qnamaker-quickstart-csharp-create-project.md)] 

## <a name="add-the-required-dependencies"></a>Incorporación de las dependencias necesarias

En la parte superior de Program.cs, reemplace la instrucción _using_ individual por las líneas siguientes para agregar las dependencias necesarias al proyecto:

[!code-csharp[Add the required dependencies](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=1-11 "Add the required dependencies")]

## <a name="add-a-kbdetails-class"></a>Incorporación de una clase KBDetails
Esta clase KBDetails se agrega dentro de los corchetes de Namespace. Esta clase permite que la biblioteca NewtonSoft deserialice la respuesta JSON en una clase C#.

[!code-csharp[Add a KBDetails class](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=15-26 "Add a KBDetails class")]

## <a name="add-the-required-constants"></a>Incorporación de las constantes necesarias

En la parte superior de la clase Program, agregue las siguientes constantes para acceder a QnA Maker:

[!code-csharp[Add the required constants](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=30-57 "Add the required constants")]

## <a name="add-the-kb-definition"></a>Incorporación de la definición de base de conocimiento

Después de las constantes, agregue la siguiente definición del modelo de la base de conocimiento:

[!code-csharp[Add the KB definition](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=59-85 "Add the KB definition")]

## <a name="add-supporting-functions-and-structures"></a>Incorporación de estructuras y funciones auxiliares
Agregue el siguiente bloque de código dentro de la clase Program:

[!code-csharp[Add supporting functions and structures](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=87-123 "Add supporting functions and structures")]

## <a name="add-a-post-request-to-create-kb"></a>Incorporación de una solicitud POST para crear la base de conocimiento

El código siguiente realiza una solicitud HTTPS a QnA Maker API para crear una base de conocimiento y recibe la respuesta:

[!code-csharp[Add a POST request to create KB](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=124-141 "Add a POST request to create KB")]

Esta llamada API devuelve una respuesta JSON que incluye el identificador de operación. Posteriormente, el programa usa el identificador de la operación para determinar si la base de conocimiento se ha creado correctamente. 

```JSON
{
  "operationState": "NotStarted",
  "createdTimestamp": "2018-09-26T05:19:01Z",
  "lastActionTimestamp": "2018-09-26T05:19:01Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "YYYe12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-get-request-to-determine-creation-status"></a>Incorporación de la solicitud GET para determinar el estado de creación

Compruebe el estado de la operación de creación.

[!code-csharp[Add GET request to determine creation status](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=142-151 "Add GET request to determine creation status")]

Esta llamada API devuelve una respuesta JSON que incluye el estado de la operación: 

```JSON
{
  "operationState": "Running",
  "createdTimestamp": "2018-09-26T05:22:53Z",
  "lastActionTimestamp": "2018-09-26T05:22:53Z",
  "userId": "XXX9549466094e1cb4fd063b646e1ad6",
  "operationId": "YYYe12ff-5d04-4b73-b594-8575f9787963"
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
  "operationId": "YYYe12ff-5d04-4b73-b594-8575f9787963"
}
```

## <a name="add-createkb-method"></a>Incorporación del método CreateKB

El método siguiente encapsula las llamadas para crear la base de conocimiento y comprobar el estado.  El **identificador de la operación** _create_ se devuelve en el campo **Location** del encabezado de la respuesta POST y luego se usa como parte de la ruta en la solicitud GET. Dado que la creación de la base de conocimiento puede tardar algún tiempo, deberá repetir las llamadas para comprobar el estado hasta que sea correcto o se produzca un error. Cuando la operación se realiza correctamente, el identificador de la base de conocimiento se devuelve en **resourceLocation**. 

[!code-csharp[Add GET request to determine creation status](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=152-227 "Add GET request to determine creation status")]

## <a name="add-publish-method"></a>Incorporación de método de publicación

Una vez que la base de conocimiento se cree correctamente, publique la base de conocimiento. Se esperaba una llamada a una API de aprendizaje, algo que no es necesario con esta versión. 

El código siguiente realiza una solicitud HTTPS a QnA Maker API para publicar una base de conocimiento y recibe la respuesta:

[!code-csharp[Add publish method](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=228-259 "Add publish method")]

La llamada API devuelve un estado 204 en caso de publicación correcta sin ningún contenido en el cuerpo de la respuesta. El código de la guía de inicio rápido agrega texto para 204 respuestas para que pueda ver el resultado.

Para cualquier otra respuesta, esa respuesta se devuelve sin modificar.

## <a name="generating-an-answer"></a>Generación de una respuesta
Para acceder a la base de datos de conocimiento para enviar una pregunta y recibir la mejor respuesta, el programa necesita el _host del punto de conexión_ de la base de conocimiento de la API de detalles API y la _clave del punto de conexión principal_ de la API de los puntos de conexión. Estos métodos están en las secciones siguientes junto con el método para generar una respuesta. 

En la tabla siguiente se muestra cómo se usan los datos para construir el identificador URI:

|Generar plantilla de identificador URI de respuesta|
|--|
|https://**HOSTNAME**.azurewebsites.net/qnamaker/knowledgebases/**KBID**/generateAnswer|

El _punto de conexión principal_ se pasa como encabezado para autenticar la solicitud para generar una respuesta:

|Nombre de encabezado|Valor de encabezado|
|--|--|
|Autorización|`Endpoint` + **punto de conexión principal**<br>Ejemplo: `Endpoint xxxxxxx`<br>Observe el espacio entre el texto de `Endpoint` y el valor del punto de conexión principal. 

El cuerpo de la solicitud debe usar el JSON adecuado:

```JSON
{
    question: "What languages does QnA Maker support?"
}
```

## <a name="get-kb-details"></a>Obtención de los detalles de la base de conocimiento
Agregue el método siguiente para obtener los detalles de la base de conocimiento. Estos detalles contienen el nombre de host de la base de conocimiento. El nombre de host es el nombre del servicio web de Azure QnA Maker que especificó al crear el recurso de QnA Maker. 

[!code-csharp[Get KB Details](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=260-273 "Add publish method")]

Esta llamada API devuelve una respuesta JSON: 

```JSON
{
  "id": "ZZZ31e19-cba7-48d1-8594-5c4297ecc9c1",
  "hostName": "https://qnamaker-s0-s.azurewebsites.net",
  "lastAccessedTimestamp": "2018-10-11T18:10:05Z",
  "lastChangedTimestamp": "2018-10-11T18:09:37Z",
  "lastPublishedTimestamp": "2018-10-11T18:10:15Z",
  "name": "QnA Maker FAQ from quickstart",
  "userId": "AAAc3841df0b42cdb00f53a49d51a89c",
  "urls": [
    "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs",
    "https://docs.microsoft.com/bot-framework/resources-bot-framework-faq"
  ],
  "sources": [
    "Custom Editorial"
  ]
}
```

## <a name="get-kb-endpoints"></a>Obtención de puntos de conexión de la base de conocimiento
Agregue el método siguiente para obtener los puntos de conexión principales de QnA Maker. Estos puntos de conexión no están vinculados a la base de conocimiento, son válidos para todas las bases de conocimiento asociadas con las claves de los recursos de QnA Maker desde Azure Portal.  

[!code-csharp[Get KB Endpoints](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=274-289 "Get KB Endpoints")]

Esta llamada API devuelve una respuesta JSON: 

```JSON
{
  "primaryEndpointKey": "AAAb5719-e2f7-4a33-937d-7a3b4736a1be",
  "secondaryEndpointKey": "BBBcba78-c1d2-4166-b98f-c77255aefaba",
  "installedVersion": "4.2.0",
  "lastStableVersion": "4.2.0"
}
```

## <a name="get-an-answer"></a>Obtención de una respuesta
Agregue el método siguiente para obtener una respuesta a la pregunta del usuario. 

[!code-csharp[Get Answer](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=290-315 "Get Answer")]

Esta llamada API devuelve una respuesta JSON: 

```JSON
{
  "answers": [
    {
      "questions": [
        "Does QnA Maker support non-English languages?"
      ],
      "answer": "See more details about [supported languages](https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/overview/languages-supported).\n\n\nIf you have content from multiple languages, be sure to create a separate service for each language.",
      "score": 82.19,
      "id": 11,
      "source": "https://docs.microsoft.com/en-in/azure/cognitive-services/qnamaker/faqs",
      "metadata": []
    }
  ]
}
```

## <a name="main-method"></a>El método main
El método main muestra las llamadas sincrónicas para crear, publicar y generar la respuesta. 

[!code-csharp[Main method](~/samples-qnamaker-csharp/documentation-samples/tutorials/create-publish-answer-knowledge-base/QnaMakerQuickstart/Program.cs?range=316-337 "Main method")]

## <a name="build-and-run-the-program"></a>Compilar y ejecutar el programa

Compile y ejecute el programa. 

Una vez creada la base de conocimiento, puede verla en el portal de QnA Maker en la página [My knowledge bases](https://www.qnamaker.ai/Home/MyServices) (Mis bases de conocimiento). Una vez que sepa cómo usar la API para generar respuestas, puede usarla con cualquier lenguaje o marco de solicitud HTTP. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de QnA Maker (V4) REST API](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)
