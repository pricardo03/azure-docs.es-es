---
title: 'Azure Content Moderator: Trabajos de moderación y revisiones con intervención humana | Microsoft Docs'
description: Aplique la revisión humana a la moderación asistida por máquina para obtener mejores resultados.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 1/21/2018
ms.author: sajagtap
ms.openlocfilehash: 35b3cdaa410712c3fd08d3df4ebe4c83e3955d50
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380270"
---
# <a name="moderation-jobs-and-reviews"></a>Trabajos de moderación y revisiones

Combine la moderación asistida por máquina con las capacidades de revisión humanas usando [Review API](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c5) de Azure Content Moderator para obtener los mejores resultados para su negocio.

Review API ofrece los siguientes métodos para incluir supervisión humana en el proceso de moderación de contenido:

* Se usan operaciones `Job` para iniciar la moderación asistida por máquina y la creación de la revisión humana como un paso.
* Las operaciones `Review` se utilizan para la creación de la revisión humana, fuera del paso de moderación.
* Las operaciones `Workflow` se utilizan para administrar los flujos de trabajo que automatizan el análisis con umbrales para la creación de la revisión.

Las operaciones `Job` y `Review` aceptan los puntos de conexión de devolución de llamada para recibir el estado y los resultados.

En este artículo se tratan las operaciones `Job` y `Review`. Lea la [introducción a los flujos de trabajo](workflow-api.md) para más información sobre cómo crear, editar y obtener las definiciones de flujo de trabajo.

## <a name="job-operations"></a>Operaciones de trabajos

### <a name="start-a-job"></a>Iniciar un trabajo
Use la operación `Job.Create` para iniciar un trabajo de moderación y de creación de revisión humana. Content Moderator analiza el contenido y evalúa el flujo de trabajo designado. Según los resultados del flujo de trabajo, crea revisiones u omite el paso. También envía las etiquetas post-moderation y post-review al punto de conexión de devolución de llamada.

Las entradas incluyen la siguiente información:

- El identificador del equipo de revisión.
- El contenido que se va a moderar.
- El nombre del flujo de trabajo. (El valor predeterminado es el flujo de trabajo "default").
- El punto de devolución de llamada de API para las notificaciones.
 
La respuesta siguiente muestra el identificador del trabajo que se inició. Puede usar este identificador para obtener el estado del trabajo y recibir información detallada.

    {
        "JobId": "2018014caceddebfe9446fab29056fd8d31ffe"
    }

### <a name="get-job-status"></a>Obtención de estado del trabajo

Use la operación `Job.Get` y el identificador de trabajo para obtener los detalles de un trabajo completado o en ejecución. La operación devuelve resultados inmediatamente aunque el trabajo de moderación se ejecute de forma asincrónica. Los resultados se devuelven a través del punto de conexión de devolución de llamada.

Las entradas incluyen la siguiente información:

- El identificador del equipo de revisión: el identificador de trabajo devuelto por la operación anterior

La respuesta incluye la siguiente información:

- El identificador de la revisión que se creó. (Use este identificador para obtener los resultados de la revisión final).
- El estado del trabajo (completado o en curso): las etiquetas de moderación asignadas (pares clave-valor).
- El informe de ejecución del trabajo.
 
 
        {
            "Id": "2018014caceddebfe9446fab29056fd8d31ffe",
            "TeamName": "some team name",
            "Status": "Complete",
            "WorkflowId": "OCR",
            "Type": "Image",
            "CallBackEndpoint": "",
            "ReviewId": "201801i28fc0f7cbf424447846e509af853ea54",
            "ResultMetaData":[
            {
            "Key": "hasText",
            "Value": "True"
            },
            {
            "Key": "ocrText",
            "Value": "IF WE DID \r\nALL \r\nTHE THINGS \r\nWE ARE \r\nCAPABLE \r\nOF DOING, \r\nWE WOULD \r\nLITERALLY \r\nASTOUND \r\nOURSELVE \r\n"
            }
            ],
            "JobExecutionReport": [
            {
                "Ts": "2018-01-07T00:38:29.3238715",
                "Msg": "Posted results to the Callbackendpoint: https://requestb.in/vxke1mvx"
                },
                {
                "Ts": "2018-01-07T00:38:29.2928416",
                "Msg": "Job marked completed and job content has been removed"
                },
                {
                "Ts": "2018-01-07T00:38:29.0856472",
                "Msg": "Execution Complete"
                },
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
 
![Revisión de imagen para moderadores humanos](images/ocr-sample-image.PNG)

## <a name="review-operations"></a>Operaciones de revisión

### <a name="create-reviews"></a>Creación de revisiones

Use la operación `Review.Create` para crear las revisiones humanas. Las puede moderar en algún otro lugar o usar la lógica personalizada para asignar las etiquetas de moderación.

Las entradas a esta operación incluyen:

- El contenido que se va a revisar.
- Las etiquetas asignadas (pares clave-valor) para su revisión por moderadores humanos.

La respuesta siguiente muestra el identificador de revisión:

    [
        "201712i46950138c61a4740b118a43cac33f434",
    ]


### <a name="get-review-status"></a>Obtención del estado de la revisión
Use la operación `Review.Get` para obtener los resultados una vez completada una revisión humana de la imagen moderada. Recibirá las notificaciones a través del punto de conexión de devolución de llamada que proporcionó. 

La operación devuelve dos conjuntos de etiquetas: 

* Las etiquetas asignadas por el servicio de moderación
* Las etiquetas una vez que se completó la revisión humana

Las entradas incluyen como mínimo:

- El nombre del equipo de revisión.
- El identificador de revisión devuelto por la operación anterior

La respuesta incluye la siguiente información:

- El estado de revisión
- Las etiquetas (pares clave-valor) que confirmó el revisor humano
- Las etiquetas (pares clave-valor) asignadas por el servicio de moderación

Puede ver las etiquetas asignadas por el revisor (**reviewerResultTags**) y las etiquetas iniciales (**metadata**) en el siguiente ejemplo de respuesta:

    {
        "reviewId": "201712i46950138c61a4740b118a43cac33f434",
        "subTeam": "public",
        "status": "Complete",
        "reviewerResultTags": [
        {
            "key": "a",
            "value": "False"
        },
        {
            "key": "r",
            "value": "True"
        },
        {
            "key": "sc",
            "value": "True"
        }
        ],
        "createdBy": "{teamname}",
        "metadata": [
        {
            "key": "sc",
            "value": "true"
        }
        ],
        "type": "Image",
        "content": "https://reviewcontentprod.blob.core.windows.net/{teamname}/IMG_201712i46950138c61a4740b118a43cac33f434",
        "contentId": "0",
        "callbackEndpoint": "{callbackUrl}"
    }

## <a name="next-steps"></a>Pasos siguientes

* Pruebe la [consola de Job API](try-review-api-job.md) y use los ejemplos de código de la API REST. Consulte también el [inicio rápido de .NET de trabajos](moderation-jobs-quickstart-dotnet.md) si está familiarizado con C# y Visual Studio. 
* Para las revisiones, empiece a trabajar con la [consola de Review API](try-review-api-review.md) y use los ejemplos de código de la API REST. A continuación, consulte [Reviews .NET quickstart](moderation-reviews-quickstart-dotnet.md) (Inicio rápido de las revisiones con .NET).
* Para revisiones de vídeos, use [Video review quickstart](video-reviews-quickstart-dotnet.md) (Guía de inicio rápido de revisión de vídeos) y aprenda a [agregar transcripciones a la revisión del vídeo](video-transcript-reviews-quickstart-dotnet.md).
