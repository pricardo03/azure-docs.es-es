---
title: 'Las revisiones, los flujos de trabajo y conceptos de trabajos: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre las revisiones, los flujos de trabajo y trabajos
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 03/14/2019
ms.author: sajagtap
ms.openlocfilehash: c1d4ef640e2ae072dacba7a665b6689e3224c55c
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756292"
---
# <a name="content-moderation-reviews-workflows-and-jobs"></a>Las revisiones de moderación de contenido, los flujos de trabajo y trabajos

Content Moderator combina la moderación automática con funcionalidades de humanos en bucle para crear un proceso de moderación óptimo para escenarios del mundo real. Lo consigue gracias basadas en la nube [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com). En esta guía, aprenderá los conceptos básicos de la herramienta de revisión: las revisiones, los flujos de trabajo y trabajos.

## <a name="reviews"></a>Revisiones

En una revisión, el contenido se carga en la herramienta de revisión y aparece en el **revisar** ficha. Desde aquí, los usuarios pueden modificar las etiquetas aplicadas y aplicar sus propias etiquetas personalizadas según corresponda. Cuando un usuario envía una revisión, los resultados se envían a un extremo de devolución de llamada especificado y se quitará el contenido del sitio.

![Abra el sitio Web de herramienta de revisión en un explorador, en la ficha de revisión](./Review-Tool-user-Guide/images/image-workflow-review.png)

Consulte la [manual de la herramienta de revisión](./review-tool-user-guide/review-moderated-images.md) para empezar a crear las revisiones o vea el [Guía de la API de REST](./try-review-api-review.md) para aprender a hacerlo mediante programación.

## <a name="workflows"></a>Flujos de trabajo

Un flujo de trabajo es un filtro personalizado basado en la nube para el contenido. Los flujos de trabajo pueden conectarse a una variedad de servicios para filtrar el contenido de maneras diferentes y, a continuación, realice la acción apropiada. Con el conector de Content Moderator, un flujo de trabajo puede aplicar etiquetas de moderación y crear revisiones con el contenido enviado automáticamente.

### <a name="view-workflows"></a>Ver flujos de trabajo

Para ver los flujos de trabajo existentes, vaya a la [herramienta de revisión](https://contentmoderator.cognitive.microsoft.com/) y seleccione **configuración** > **flujos de trabajo**.

![Flujo de trabajo predeterminado](images/default-workflow-listed.PNG)

Los flujos de trabajo se pueden describir por completo como cadenas JSON, lo que hace que sean accesibles mediante programación. Si selecciona el **editar** opción del flujo de trabajo y, a continuación, seleccione el **JSON** pestaña, verá una expresión JSON similar al siguiente:

```json
{
    "Type": "Logic",
    "If": {
        "ConnectorName": "moderator",
        "OutputName": "isAdult",
        "Operator": "eq",
        "Value": "true",
        "Type": "Condition"
        },
    "Then": {
    "Perform": [
    {
        "Name": "createreview",
        "CallbackEndpoint": null,
        "Tags": []
    }
    ],
    "Type": "Actions"
    }
}
```

Consulte la [manual de la herramienta de revisión](./review-tool-user-guide/workflows.md) para empezar a crear y utilizar flujos de trabajo, o vea el [Guía de la API de REST](./try-review-api-workflow.md) para aprender a hacerlo mediante programación.

## <a name="jobs"></a>Trabajos

Un trabajo de moderación actúa como un tipo de contenedor para la funcionalidad de moderación de contenido, los flujos de trabajo y las revisiones. El trabajo examina el contenido mediante la moderación de imágenes de Content Moderator API o la API de moderación de texto y, a continuación, comprueba contra el flujo de trabajo designado. En función de los resultados del flujo de trabajo, puede o no se puede crear una revisión para el contenido en el [herramienta de revisión](./review-tool-user-guide/human-in-the-loop.md). Mientras que las revisiones y los flujos de trabajo se pueden crear y configurar con sus respectivo API, el trabajo de la API le permite obtener un informe detallado de todo el proceso (que se puede enviar a un punto de conexión de devolución de llamada especificado).

Consulte la [Guía de la API de REST](./try-review-api-job.md) para empezar a usar los trabajos.

## <a name="next-steps"></a>Pasos siguientes

* Pruebe la [consola de Job API](try-review-api-job.md) y use los ejemplos de código de la API REST. Consulte también el [inicio rápido de .NET de trabajos](moderation-jobs-quickstart-dotnet.md) si está familiarizado con C# y Visual Studio. 
* Para las revisiones, empiece a trabajar con la [consola de Review API](try-review-api-review.md) y use los ejemplos de código de la API REST. A continuación, consulte [Reviews .NET quickstart](moderation-reviews-quickstart-dotnet.md) (Inicio rápido de las revisiones con .NET).
* Para revisiones de vídeos, use [Video review quickstart](video-reviews-quickstart-dotnet.md) (Guía de inicio rápido de revisión de vídeos) y aprenda a [agregar transcripciones a la revisión del vídeo](video-transcript-reviews-quickstart-dotnet.md).
