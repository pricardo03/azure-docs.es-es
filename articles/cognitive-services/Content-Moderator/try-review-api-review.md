---
title: 'Creación de las revisiones de moderación con consola de API de REST: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Usar las API de revisión de Azure Content Moderator para crear las revisiones de imagen o texto de moderación humana.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/18/2019
ms.author: sajagtap
ms.openlocfilehash: 254269ccedc92b9dfc164cc4665a8a8513682773
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58882028"
---
# <a name="create-human-reviews-rest"></a>Crear revisiones humanas (REST)

[Las revisiones](./review-api.md#reviews) almacenar y mostrar el contenido para que moderadores humanos evaluar. Cuando un usuario completa una revisión, los resultados se envían a un punto de conexión de devolución de llamada especificadas. En esta guía, obtendrá información sobre cómo configurar las revisiones mediante la revisión de las API de REST a través de la consola de API. Una vez que comprenda la estructura de las API, puede portar con facilidad estas llamadas a cualquier plataforma compatible con REST.

## <a name="prerequisites"></a>Requisitos previos

- Iniciar sesión o crear una cuenta de Content Moderator [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/) sitio.

## <a name="create-a-review"></a>Creación de una revisión

Para crear una revisión, vaya a la **[revisar: creación de](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4)** API hacen referencia a página y seleccione el botón de su región clave (puede encontrarlo en la dirección URL del extremo en el **credenciales** página de la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/)). Esto inicia la consola de API, donde puede crear y ejecutar llamadas API de REST con facilidad.

![Revisar: selección de región de Get](images/test-drive-region.png)

### <a name="enter-rest-call-parameters"></a>Escriba los parámetros de llamada REST

Especifique los valores de **teamName**, y **Ocp-Apim-Subscription-Key**:

- **teamName**: El identificador de equipo que creó al configurar su [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/) cuenta (se encuentra en la **Id** campo en pantalla de credenciales de la herramienta de revisión).
- **Ocp-Apim-Subscription-Key**: La clave de Content Moderator. Puede encontrarlo en el **configuración** pestaña de la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com).

### <a name="enter-a-review-definition"></a>Especifique una definición de revisión

Editar el **cuerpo de la solicitud** cuadro para especificar la solicitud JSON con los siguientes campos:

- **Metadatos**: Pares de clave-valor personalizados que se devuelven a su punto de conexión de devolución de llamada. Si la clave es un código corto que se define en el [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com), aparece como una etiqueta.
- **Contenido**: En el caso de contenido de imagen y vídeo, se trata de una cadena de dirección URL que apunte al contenido. Para obtener contenido de texto, esta es la cadena de texto real.
- **ContentId**: Una cadena de identificador personalizado. Esta cadena se pasa a la API y se devuelve a través de la devolución de llamada. Resulta útil para asociar identificadores internos o los metadatos con los resultados de un trabajo de moderación.
- **CallbackEndpoint**: (Opcional) La dirección URL para recibir información de devolución de llamada cuando se complete la revisión.

El cuerpo de solicitud predeterminada muestra ejemplos de los diferentes tipos de revisiones que se puede crear:

```json
[Image]
[
  {
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      }
    ],
    "Type": "Image",
    "Content": "<Content Url>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>"
  }
]
[Text]
[
  {
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      }
    ],
    "Type": "Text",
    "Content": "<Your Text Content>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>"
  }
]
[Video]
[
  {
    "VideoFrames":[
      {
          "Id": "<Frame Id>",
          "Timestamp": "<Frame Timestamp",
          "FrameImage":"<Frame Image URL",
          "Metadata": [
            {
              "Key": "<Key>",
              "Value": "<Value"
            }
          ],
          "ReviewerResultTags": [
          ]
    ], 
    "Metadata": [
      {
        "Key": "string",
        "Value": "string"
      },
      //For encrypted Videos
        {
          "Key": "protectedType",
          "Value": "AES or FairPlay or Widevine or Playready"
        },
        {
          "Key": "authenticationToken",
          "Value": "your viewtoken(In case of Video Indexer AES encryption type, this value is viewtoken from breakdown json)"
        },
      //For FairPlay encrypted type video include certificateUrl as well
        {
          "Key": "certificateUrl",
          "Value": "your certificate url"
        }
    ],
    "Type": "Video",
    "Content": "<Stream Url>",
    "ContentId": "<Your identifier for this content>",
    "CallbackEndpoint": "<Url where you would receive callbacks>",
    [Optional]
    "Timescale": "<Timescale of the video>
  }
]
```

### <a name="submit-your-request"></a>Enviar la solicitud
  
Seleccione **Enviar**. Si la operación se realiza correctamente, el **estado de respuesta** es `200 OK`y el **contenido de la respuesta** cuadro muestra un identificador para la revisión. Copie esta id. para usarla en los pasos siguientes.

![Cuadro de contenido de la repuesta de la consola Review - Create (Revisar: crear) muestra la id. de revisión](images/test-drive-review-2.PNG)

### <a name="examine-the-new-review"></a>Examine la nueva revisión

En el [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com), seleccione **revisión** > **imagen**/**texto** / **Vídeo** (según el contenido que usa). Debe aparecer el contenido que ha cargado, listo para revisión humana.

![Imagen de un balón de fútbol de la herramienta de revisión](images/test-drive-review-5.PNG)

## <a name="get-review-details"></a>Obtener detalles de revisión

Para recuperar los detalles de una revisión existente, vaya a la [revisar: obtener](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c2) API de referencia de página y seleccione el botón de su región (es decir, la región en la que se administra la clave).

![Selección de región para Workflow - Get (Flujo de trabajo: Obtener)](images/test-drive-region.png)

Escriba los parámetros de llamada REST como se muestra en la sección anterior. Este paso, **reviewId** es la cadena de identificador único que recibiste cuando se haya creado la revisión.

![Resultados de Obtener de la consola Review - Create (Revisar: crear)](images/test-drive-review-3.PNG)
  
Seleccione **Enviar**. Si la operación se realiza correctamente, el **estado de respuesta** es `200 OK`y el **contenido de la respuesta** cuadro muestra los detalles de revisión en formato JSON, similar al siguiente:

```json
{  
  "reviewId":"201712i46950138c61a4740b118a43cac33f434",
  "subTeam":"public",
  "status":"Complete",
  "reviewerResultTags":[  
    {  
      "key":"a",
      "value":"False"
    },
    {  
      "key":"r",
      "value":"True"
    },
    {  
      "key":"sc",
      "value":"True"
    }
  ],
  "createdBy":"<teamname>",
  "metadata":[  
    {  
      "key":"sc",
      "value":"true"
    }
  ],
  "type":"Image",
  "content":"https://reviewcontentprod.blob.core.windows.net/<teamname>/IMG_201712i46950138c61a4740b118a43cac33f434",
  "contentId":"0",
  "callbackEndpoint":"<callbackUrl>"
}
```

Tome nota de los siguientes campos en la respuesta:

- **status**
- **reviewerResultTags**: Esto aparece si las etiquetas se han agregado manualmente el equipo de revisión humana (se muestra el **createdBy** campo).
- **metadata**: Esto muestra las etiquetas que inicialmente se agregaron en la revisión, antes de que cambie de equipo de revisión humana.

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido a crear las revisiones de moderación de contenido mediante la API de REST. A continuación, integrar las revisiones en un escenario de moderación de extremo a otro, como el [moderación de comercio electrónico](./ecommerce-retail-catalog-moderation.md) tutorial.