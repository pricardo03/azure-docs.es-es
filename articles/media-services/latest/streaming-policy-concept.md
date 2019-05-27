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
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: 510899e44e4ea4a90e21473ee6af546744c2be2a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66120233"
---
# <a name="streaming-policies"></a>Directivas de streaming

En Azure Media Services v3, las [directivas de streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies) permiten definir los protocolos de streaming y las opciones de cifrado de los [localizadores de streaming](streaming-locators-concept.md). Media Services v3 proporciona que algunas directivas de transmisión por secuencias de predefinidas para que se pueden usar directamente para la versión de prueba o producción. 

Directivas de transmisión por secuencias predefinidas disponibles actualmente:<br/>"Predefined_DownloadOnly", "Predefined_ClearStreamingOnly", "Predefined_DownloadAndClearStreaming", "Predefined_ClearKey", "Predefined_MultiDrmCencStreaming" y "Predefined_ MultiDrmStreaming".

Si tiene requisitos especiales (por ejemplo, si desea especificar diferentes protocolos, debe usar un servicio de entrega de claves personalizados, o debe usar una pista de audio claro), puede crear una directiva personalizada de transmisión por secuencias. 

 
> [!IMPORTANT]
> * Las propiedades de **directivas de streaming** que son del tipo Datetime siempre están en formato UTC.
> * Debería diseñar un conjunto limitado de directivas para su cuenta de Media Service y reutilizarlas con los objetos StreamingLocator siempre que se necesiten las mismas opciones. Para más información, consulte [Cuotas y limitaciones](limits-quotas-constraints.md).

## <a name="decision-tree"></a>Árbol de decisión

El árbol de decisión siguiente le ayudará a elegir una directiva predefinida de transmisión por secuencias para su escenario.

Haga clic en la imagen para verla a tamaño completo.  <br/>
<a href="./media/streaming-policy/large.png" target="_blank"><img src="./media/streaming-policy/small.png"></a> 

## <a name="examples"></a>Ejemplos

### <a name="not-encrypted"></a>No cifrado

Si va a transmitir el archivo en-the-clear (sin cifrar), establecer la directiva de transmisión por secuencias clara predefinida: a 'Predefined_ClearStreamingOnly' (en. NET, puede usar la enumeración PredefinedStreamingPolicy.ClearStreamingOnly).

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

### <a name="encrypted"></a>Cifrada 

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
