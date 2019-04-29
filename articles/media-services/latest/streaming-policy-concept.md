---
title: Directivas de streaming en Azure Media Services | Microsoft Docs
description: En este artículo se explica qué son las directivas de streaming y cómo las usa Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/03/2019
ms.author: juliako
ms.openlocfilehash: 10600d8f3ff4e08b8d90f28ec15d3cb0c56bcae0
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61230903"
---
# <a name="streaming-policies"></a>Directivas de streaming

En Azure Media Services v3, las [directivas de streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies) permiten definir los protocolos de streaming y las opciones de cifrado de los [localizadores de streaming](streaming-locators-concept.md). Puede usar una de las directivas de streaming predefinidas o crear una directiva personalizada. Las directivas de streaming predefinidas que están disponibles actualmente son: "Predefined_DownloadOnly", "Predefined_ClearStreamingOnly", "Predefined_DownloadAndClearStreaming", "Predefined_ClearKey", "Predefined_MultiDrmCencStreaming" y "Predefined_ MultiDrmStreaming".

> [!IMPORTANT]
> * Las propiedades de **directivas de streaming** que son del tipo Datetime siempre están en formato UTC.
> * Debería diseñar un conjunto limitado de directivas para su cuenta de Media Service y reutilizarlas con sus localizadores de streaming siempre que se necesiten las mismas opciones. 

## <a name="examples"></a>Ejemplos

### <a name="not-encrypted"></a>No cifrado

Si quiere transmitir el archivo sin cifrar, establezca la directiva de streaming sin cifrar predefinida en "Predefined_ClearStreamingOnly" (en. NET, puede usar PredefinedStreamingPolicy.ClearStreamingOnly).

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = PredefinedStreamingPolicy.ClearStreamingOnly
    });
```

### <a name="encrypted"></a>Cifrados 

Si necesita cifrar el contenido con cifrado de sobre y cifrado común, establezca la directiva en "Predefined_MultiDrmCencStreaming". Esta directiva indica que desea que se generen y establezcan dos claves de contenido (sobre y CENC) en el localizador. De esta forma, se aplican los cifrados de sobre, PlayReady y Widevine (la clave se entrega al cliente de reproducción en función de las licencias DRM configuradas).

```csharp
StreamingLocator locator = await client.StreamingLocators.CreateAsync(
    resourceGroup,
    accountName,
    locatorName,
    new StreamingLocator
    {
        AssetName = assetName,
        StreamingPolicyName = "Predefined_MultiDrmCencStreaming",
        DefaultContentKeyPolicyName = contentPolicyName
    });
```

Si también quiere cifrar su transmisión con CBCS (FairPlay), use "Predefined_MultiDrmStreaming".

## <a name="filtering-ordering-paging"></a>Filtrado, ordenación, paginación

Consulte [Filtrado, ordenación y paginación de entidades de Media Services](entities-overview.md).

## <a name="next-steps"></a>Pasos siguientes

* [Streaming de un archivo](stream-files-dotnet-quickstart.md)
* [Uso del cifrado dinámico AES-128 y el servicio de entrega de claves](protect-with-aes128.md)
* [Uso del cifrado dinámico de DRM y el servicio de entrega de licencias](protect-with-drm.md)
