---
title: Ejecutar trabajos de moderación de contenido en Azure Content Moderator | Microsoft Docs
description: Obtenga información acerca de cómo ejecutar trabajos de moderación de contenido en la consola de API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/03/2017
ms.author: sajagtap
ms.openlocfilehash: 6f741be1001ae70d5fdbf6f374204aaad1601abe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380151"
---
# <a name="start-a-moderation-job-from-the-api-console"></a>Iniciar un trabajo de moderación en la consola de API

Use las [operaciones de trabajo](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) de la API de revisión para iniciar trabajos de moderación de contenido de un extremo a otro para el contenido de imágenes o texto en Azure Content Moderator. 

El trabajo de moderación examina el contenido con Content Moderator Image Moderation API o Text Moderation API. A continuación, el trabajo de moderación utiliza flujos de trabajo (definidos en la herramienta de revisión) para generar revisiones en la herramienta de revisión. 

Después de que un moderador humano revise las etiquetas asignadas automáticamente y los datos de predicción y de que envíe una decisión de moderación final, la API de revisión envía toda la información al punto de conexión de API.

## <a name="prerequisites"></a>requisitos previos

Navegue hasta la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/). Suscríbase, si aún no lo ha hecho. En la herramienta de revisión, [defina un flujo de trabajo personalizado](Review-Tool-User-Guide/Workflows.md) para utilizarlo en la operación `Job`.

## <a name="use-the-api-console"></a>Usar la consola de API
Para probar la API mediante la consola en línea, debe especificar algunos valores en la consola:
    
- `teamName`: use el campo `Id` de la pantalla de credenciales de la herramienta de revisión. 
- `ContentId`: esta cadena se pasa a la API y se devuelve a través de la devolución de llamada. **ContentId** es útil para asociar identificadores internos o metadatos con los resultados de un trabajo de moderación.- `Workflowname`: el nombre del [flujo de trabajo creado](Review-Tool-User-Guide/Workflows.md) en la sección anterior.
- `Ocp-Apim-Subscription-Key`: ubicada en la pestaña **Configuración**. Para más información, consulte [Información general](overview.md).

El acceso a la consola de API se realiza desde la ventana **Credenciales**.

### <a name="navigate-to-the-api-reference"></a>Navegar hasta la referencia de API
En la ventana **Credenciales**, seleccione [Referencia de la API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5).

  Se abre la página `Job.Create`.

### <a name="select-your-region"></a>Seleccionar la región
En **Open API testing console** (Abrir consola de prueba de API), seleccione la región que mejor describa su ubicación.
  ![Selección de la región en la página Job - Create (Trabajo: crear)](images/test-drive-job-1.png)

  Se abre la consola de API `Job.Create`. 

### <a name="enter-parameters"></a>Escribir parámetros

Especifique valores para los parámetros de consulta necesarios y su clave de suscripción. En el cuadro **Cuerpo de la solicitud**, especifique la ubicación de la información que desea examinar. En este ejemplo, se usa esta [imagen de ejemplo](https://moderatorsampleimages.blob.core.windows.net/samples/sample6.png).

  ![Parámetros de consulta de la consola Job - Create (Trabajo: crear), encabezados y cuadro Cuerpo de la solicitud](images/job-api-console-inputs.PNG)

### <a name="submit-your-request"></a>Enviar la solicitud
Seleccione **Enviar**. Se crea una id. de trabajo. Cópiela para usarla en los pasos siguientes.

  `"JobId": "2018014caceddebfe9446fab29056fd8d31ffe"`

### <a name="open-the-get-job-details-page"></a>Abrir la página de detalles Obtener trabajo
Seleccione **Obtener** y, a continuación, abra la API seleccionando el botón que coincida con su región.

  ![Resultados de Obtener de la consola Job - Create (Trabajo: crear)](images/test-drive-job-4.png)

### <a name="review-the-response"></a>Revisar la respuesta

Especifique valores para **teamName** y **JobID**. Escriba la clave de suscripción y, a continuación, seleccione **Enviar**. En la respuesta siguiente se muestran los detalles y el estado del trabajo de ejemplo.

```
    {
        "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
        "TeamName": "some team name",
        "Status": "InProgress",
        "WorkflowId": "OCR",
        "Type": "Image",
        "CallBackEndpoint": "",
        "ReviewId": "",
        "ResultMetaData": [],
        "JobExecutionReport": 
        [
            {
            "Ts": "2018-01-07T00:38:26.7714671",
                "Msg": "Successfully got hasText response from Moderator"
            },
            {
                "Ts": "2018-01-07T00:38:26.4181346",
                "Msg": "Getting hasText from Moderator"
            },
            {
                "Ts": "2018-01-07T00:38:25.5122828",
                "Msg": "Starting Execution - Try 1"
            }
        ]
    }
```

## <a name="navigate-to-the-review-tool"></a>Navegar hasta la herramienta de revisión
En el panel de Content Moderator, seleccione **Revisar** > **Imagen**. Aparece la imagen que ha examinado, lista para revisión humana.

  ![Imagen de tres ciclistas de la herramienta de revisión](images/ocr-sample-image.PNG)

## <a name="next-steps"></a>Pasos siguientes

Usar la API de REST en el código o empiece en [Jobs .NET quickstart](moderation-jobs-quickstart-dotnet.md) (Inicio rápido de trabajos con .NET) para integrar la aplicación.
