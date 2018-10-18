---
title: Creación de una entrada de trabajo de Azure Media Services a partir de una dirección URL de HTTPS | Microsoft Docs
description: En este tema se muestra cómo crear una entrada de trabajo a partir de una dirección URL de HTTP(s).
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 09/24/2018
ms.author: juliako
ms.openlocfilehash: 8b8bfb578b9a7190e93da721531041bb93a9a03c
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47224739"
---
# <a name="create-a-job-input-from-an-https-url"></a>Crear una entrada de trabajo desde una dirección URL de HTTPS

En Media Services v3, cuando se envían trabajos para procesar los vídeos, es necesario indicar a Media Services dónde encontrar el vídeo de entrada. Una de las opciones es especificar una dirección URL de HTTP(s) como una entrada de trabajo (como se muestra en este ejemplo). Tenga en cuenta que, actualmente, AMS v3 no admite la codificación de transferencia fragmentada a través de direcciones URL de HTTPS. Para un ejemplo completo, consulte este [ejemplo de GitHub](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="net-sample"></a>Ejemplo de .NET

El código siguiente muestra cómo crear un trabajo con una entrada de dirección URL HTTPS.

[!code-csharp[Main](../../../media-services-v3-dotnet-quickstarts/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs#SubmitJob)]

## <a name="next-steps"></a>Pasos siguientes

[Cree una entrada de trabajo a partir de un archivo local](job-input-from-local-file-how-to.md).
