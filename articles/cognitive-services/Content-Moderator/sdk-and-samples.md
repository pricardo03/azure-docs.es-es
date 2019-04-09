---
title: 'SDK y ejemplos: Content Moderator, Python, Java, Node.js y .NET'
titlesuffix: Azure Cognitive Services
description: Obtención de SDK y ejemplos para Content Moderator
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: sample
ms.date: 02/27/2018
ms.author: sajagtap
ms.openlocfilehash: fd71a48372bcdb459bb3b7509e9a9c5dba529555
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58756609"
---
# <a name="content-moderator-sdks-and-samples"></a>SDK y ejemplos de Content Moderator

## <a name="sdks-for-python-java-nodejs-and-net"></a>SDK para Python, Java, Node.js y .NET

- [SDK de Content Moderator para Python](https://pypi.python.org/pypi/azure-cognitiveservices-vision-contentmoderator)
- [SDK de Content Moderator para Java](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-cognitiveservices-contentmoderator%22)
- [SDK de Content Moderator para Node.js](https://www.npmjs.com/package/azure-cognitiveservices-contentmoderator)
- [SDK de Content Moderator para .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/)

## <a name="net-sdk-samples"></a>Ejemplos de SDK de .NET

En la lista siguiente se incluyen vínculos a ejemplos de código compilados mediante el SDK de Azure Content Moderator para .NET.

- **Biblioteca auxiliar**: [cree un cliente de Content Moderator para usar en otros ejemplos](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ModeratorHelper/Clients.cs). Consulte el [inicio rápido](content-moderator-helper-quickstart-dotnet.md).

### <a name="moderation"></a>Moderación 
- **Moderación de imágenes**: [evalúe una imagen en busca de contenido explícito y para adultos, texto y caras](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageModeration/Program.cs). Consulte el [inicio rápido](image-moderation-quickstart-dotnet.md).
- **Imágenes personalizadas**: [modere con listas de imágenes personalizadas](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageListManagement/Program.cs). Consulte el [inicio rápido](image-lists-quickstart-dotnet.md).

> [!NOTE]
> Hay un límite máximo de **5 listas de imágenes** y cada una de ellas **no debe superar las 10 000 imágenes**.
>

- **Moderación de texto**: [Pantalla de texto para blasfemias y datos personales](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TextModeration/Program.cs). Consulte el [inicio rápido](text-moderation-quickstart-dotnet.md).
- **Términos personalizados**: [modere con listas de términos personalizados](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/TermListManagement/Program.cs). Consulte el [inicio rápido](term-lists-quickstart-dotnet.md).

> [!NOTE]
> Hay un límite máximo de **5 listas de términos** y cada lista **no debe superar los 10 000 términos**.
>

- **Moderación de vídeo**: [examine un vídeo para buscar contenido explícito y para adultos y obtener resultados](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoModeration/Program.cs). Consulte el [inicio rápido](video-moderation-api.md).

### <a name="review"></a>Revisión
- **Trabajos con imágenes**: [inicie un trabajo de moderación que examine y cree revisiones](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageJobs/Program.cs). Consulte el [inicio rápido](moderation-jobs-quickstart-dotnet.md).
- **Revisiones de imágenes**: [cree revisiones con intervención humana](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/ImageReviews/Program.cs). Consulte el [inicio rápido](moderation-reviews-quickstart-dotnet.md).
- **Revisiones de vídeos**: [cree revisiones de vídeo con intervención humana](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoReviews/Program.cs). Consulte el [inicio rápido](video-reviews-quickstart-dotnet.md).
- **Revisiones de transcripciones de vídeos**: [cree revisiones de transcripciones de vídeos con intervención humana](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/ContentModerator/VideoTranscriptReviews/Program.cs). Consulte el [inicio rápido](video-reviews-quickstart-dotnet.md).

Consulte todos los ejemplos de .NET en los [ejemplos de .NET para Content Moderator en GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator).

## <a name="rest-api-samples-in-c"></a>Ejemplos de API REST en C#

- [Moderación de imágenes](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageModeration)
- [Moderación de texto](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/TextModeration)
- [Moderación de vídeo](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/VideoModeration)
- [Revisiones de imágenes](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageReviews)
- [Trabajos de imágenes](https://github.com/MicrosoftContentModerator/ContentModerator-API-Samples/tree/master/ImageJob)

Para ver tutoriales de estos ejemplos, consulte el [seminario web a petición](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html).

## <a name="tutorials"></a>Tutoriales
- [Moderación de catálogo de comercio electrónico](https://github.com/MicrosoftContentModerator/samples-eCommerceCatalogModeration). Consulte el [tutorial](ecommerce-retail-catalog-moderation.md).
- [Moderación de contenido de Facebook](https://github.com/MicrosoftContentModerator/samples-fbPageModeration). Consulte el [tutorial](facebook-post-moderation.md).
- [Moderación de contenido de vídeo y transcripción y solución de revisión](https://github.com/MicrosoftContentModerator/VideoReviewConsoleApp). Consulte el [tutorial](video-transcript-moderation-review-tutorial-dotnet.md)

## <a name="on-demand-webinars"></a>Seminarios web a petición
- [Moderación de contenido a escala asistida por máquina con Content Moderator](https://info.microsoft.com/cognitive-services-content-moderator-ondemand.html)
