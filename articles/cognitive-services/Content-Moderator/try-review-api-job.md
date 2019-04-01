---
title: Uso de trabajos de moderación con la consola de API de REST - Content Moderator
titlesuffix: Azure Cognitive Services
description: Use las operaciones de trabajo de la API de revisión para iniciar trabajos de moderación de contenido de un extremo a otro para el contenido de imágenes o texto en Azure Content Moderator.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: article
ms.date: 03/18/2019
ms.author: sajagtap
ms.openlocfilehash: 7827cee2af2dfc0c1fddc407c1d146dc9a66c514
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756094"
---
# <a name="define-and-use-moderation-jobs-rest"></a>Definir y usar trabajos de moderación (REST)

Un trabajo de moderación actúa como un tipo de contenedor para la funcionalidad de moderación de contenido, los flujos de trabajo y las revisiones. Esta guía muestra cómo usar la API de REST de trabajo para iniciar y compruebe los trabajos de moderación de contenido. Una vez que comprenda la estructura de las API, puede portar con facilidad estas llamadas a cualquier plataforma compatible con REST.

## <a name="prerequisites"></a>Requisitos previos

- Iniciar sesión o crear una cuenta de Content Moderator [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/) sitio.
- (Opcional) [Definir un flujo de trabajo personalizado](./Review-Tool-User-Guide/Workflows.md) para usar con el trabajo; también puede usar el flujo de trabajo predeterminada.

## <a name="create-a-job"></a>Creación de un trabajo

Para crear un trabajo de moderación, vaya a la [de trabajo: crear](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) API hacen referencia a página y seleccione el botón de su región clave (puede encontrarlo en la dirección URL del extremo en el **credenciales** página de la [revisión herramienta](https://contentmoderator.cognitive.microsoft.com/)). Esto inicia la consola de API, donde puede crear y ejecutar llamadas API de REST con facilidad.

![Trabajo: Crear página de selección de región](images/test-drive-job-1.png)

### <a name="enter-rest-call-parameters"></a>Escriba los parámetros de llamada REST

Escriba los valores siguientes para construir la llamada REST:

- **teamName**: El identificador de equipo que creó al configurar su [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/) cuenta (se encuentra en la **Id** campo en pantalla de credenciales de la herramienta de revisión).
- **ContentType**: Esto puede ser "Image", "Text" o "Vídeo".
- **ContentId**: Una cadena de identificador personalizado. Esta cadena se pasa a la API y se devuelve a través de la devolución de llamada. Resulta útil para asociar identificadores internos o los metadatos con los resultados de un trabajo de moderación.
- **workflowname**: El nombre del flujo de trabajo que creó anteriormente (o "default" para el flujo de trabajo de forma predeterminada).
- **CallbackEndpoint**: (Opcional) La dirección URL para recibir información de devolución de llamada cuando se complete la revisión.
- **Ocp-Apim-Subscription-Key**: La clave de Content Moderator. Puede encontrarlo en el **configuración** pestaña de la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com).

### <a name="fill-in-the-request-body"></a>Rellene el cuerpo de solicitud

El cuerpo de la llamada REST contiene un campo **ContentValue**. Pegue el contenido de texto sin formato si moderación de texto, o escriba una imagen o una dirección URL de vídeo si está moderación de imagen o vídeo. Puede usar la siguiente dirección URL de imagen de ejemplo: [https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg](https://moderatorsampleimages.blob.core.windows.net/samples/sample2.jpg)

![Parámetros de consulta de la consola Job - Create (Trabajo: crear), encabezados y cuadro Cuerpo de la solicitud](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>Enviar la solicitud

Seleccione **Enviar**. Si la operación se realiza correctamente, el **estado de respuesta** es `200 OK`y el **contenido de la respuesta** cuadro muestra un identificador para el trabajo. Copie esta id. para usarla en los pasos siguientes.

![Cuadro de contenido de la repuesta de la consola Review - Create (Revisar: crear) muestra la id. de revisión](images/test-drive-job-3.PNG)

## <a name="get-job-status"></a>Obtención de estado del trabajo

Para obtener el estado y los detalles de un trabajo completado o en ejecución, vaya a la [de trabajo: obtener](https://westus2.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c3) API de referencia de página y seleccione el botón de su región (es decir, la región en la que se administra la clave).

![Trabajo - seleccionar una región Get](images/test-drive-region.png)

Escriba los parámetros de llamada REST como se muestra en la sección anterior. Este paso, **JobId** es la cadena de identificador único que recibiste cuando se ha creado el trabajo. Seleccione **Enviar**. Si la operación se realiza correctamente, el **estado de respuesta** es `200 OK`y el **contenido de la respuesta** cuadro muestra el trabajo en formato JSON, similar al siguiente:

```json
{  
  "Id":"2018014caceddebfe9446fab29056fd8d31ffe",
  "TeamName":"some team name",
  "Status":"Complete",
  "WorkflowId":"OCR",
  "Type":"Image",
  "CallBackEndpoint":"",
  "ReviewId":"201801i28fc0f7cbf424447846e509af853ea54",
  "ResultMetaData":[  
    {  
      "Key":"hasText",
      "Value":"True"
    },
    {  
      "Key":"ocrText",
      "Value":"IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n"
    }
  ],
  "JobExecutionReport":[  
    {  
      "Ts":"2018-01-07T00:38:29.3238715",
      "Msg":"Posted results to the Callbackendpoint: https://requestb.in/vxke1mvx"
    },
    {  
      "Ts":"2018-01-07T00:38:29.2928416",
      "Msg":"Job marked completed and job content has been removed"
    },
    {  
      "Ts":"2018-01-07T00:38:29.0856472",
      "Msg":"Execution Complete"
    },
    {  
      "Ts":"2018-01-07T00:38:26.7714671",
      "Msg":"Successfully got hasText response from Moderator"
    },
    {  
      "Ts":"2018-01-07T00:38:26.4181346",
      "Msg":"Getting hasText from Moderator"
    },
    {  
      "Ts":"2018-01-07T00:38:25.5122828",
      "Msg":"Starting Execution - Try 1"
    }
  ]
}
```

![Trabajo: obtener respuesta de llamada REST](images/test-drive-job-5.png)

### <a name="examine-the-new-reviews"></a>Examine la nueva review(s)

Si el trabajo contenido dieron lugar a la creación de una revisión, puede verla en el [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com). Seleccione **revisión** > **imagen**/**texto**/**vídeo** (según el contenido se usa). Debe aparecer el contenido, está listo para revisión humana. Después de que un moderador humano revisa las etiquetas asignadas automáticamente y los datos de predicción y envía una decisión final moderación, la API de trabajos envía toda esta información para el punto de conexión de punto de conexión de devolución de llamada designado.

## <a name="next-steps"></a>Pasos siguientes

En esta guía, ha aprendido a crear y consultar los trabajos de moderación de contenido mediante la API de REST. A continuación, integre trabajos en un escenario de moderación de extremo a otro, como el [moderación de comercio electrónico](./ecommerce-retail-catalog-moderation.md) tutorial.