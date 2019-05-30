---
title: Localizadores de streaming en Azure Media Services | Microsoft Docs
description: En este artículo se explica qué son los localizadores de streaming y cómo los usa Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/26/2019
ms.author: juliako
ms.openlocfilehash: 5897b7df2460257784c40eb974c473573ec4003d
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299169"
---
# <a name="streaming-locators"></a>Localizadores de streaming

Para que los vídeos en el recurso de salida estén disponibles para los clientes para reproducción, tiene que crear un objeto [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) y, luego, generar direcciones URL de streaming. Para crear una dirección URL, debe concatenar el nombre de host del punto de conexión de Streaming y la ruta de acceso del localizador de Streaming. Para un ejemplo de.NET, consulte [Obtención de un objeto StreamingLocator](stream-files-tutorial-with-api.md#get-a-streaming-locator).

El proceso de creación de un objeto **StreamingLocator** se denomina publicación. De forma predeterminada, el objeto **StreamingLocator** es válido inmediatamente después de realizar las llamadas a la API y dura hasta que se elimina, salvo que configure las horas de inicio y de finalización opcionales. 

Al crear un **localizador de Streaming**, debe especificar un **activos** nombre y un **directiva Streaming** nombre. Para obtener más información, vea los temas siguientes:

* [Recursos](assets-concept.md)
* [Directivas de streaming](streaming-policy-concept.md)
* [Directivas de claves de contenido](content-key-policy-concept.md)

También puede especificar la hora inicial y final en el localizador de Streaming, que solo permitirá al usuario reproducir el contenido entre estos tiempos (por ejemplo, entre 5/1/2019 para 5/5/2019).  

## <a name="considerations"></a>Consideraciones

* **Localizadores de streaming** no son actualizables. 
* Las propiedades de objetos **StreamingLocator** que son del tipo Datetime siempre están en formato UTC.
* Debería diseñar un conjunto limitado de directivas para su cuenta de Media Service y reutilizarlas con los objetos StreamingLocator siempre que se necesiten las mismas opciones. Para más información, consulte [Cuotas y limitaciones](limits-quotas-constraints.md).

## <a name="create-streaming-locators"></a>Crear los localizadores de Streaming  

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

### <a name="encrypted"></a>Cifrados 

Si necesita cifrar el contenido con el cifrado CENC, establezca la directiva de 'Predefined_MultiDrmCencStreaming'. El cifrado de Widevine se aplicará a una secuencia de guiones y PlayReady a Smooth. La clave se entregará a un cliente de reproducción en función de las licencias DRM configuradas.

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

Si también desea cifrar su transmisión HLS con CBCS (FairPlay), utilice 'Predefined_MultiDrmStreaming'.

## <a name="associate-filters-with-streaming-locators"></a>Asociar filtros a los localizadores de Streaming

Consulte [filtros: asociar con localizadores de Streaming](filters-concept.md#associating-filters-with-streaming-locator).

## <a name="filter-order-page-streaming-locator-entities"></a>Filtro, por orden, las entidades de localizador de Streaming de página

Consulte [Filtrado, ordenación y paginación de entidades de Media Services](entities-overview.md).

## <a name="list-streaming-locators-by-asset-name"></a>Localizadores de Streaming de lista por nombre de recurso

Para obtener los localizadores de Streaming según el nombre de recurso asociado, use las siguientes operaciones:

|Lenguaje|API|
|---|---|
|REST|[liststreaminglocators](https://docs.microsoft.com/rest/api/media/assets/liststreaminglocators)|
|CLI|[az ams asset list-streaming-locators](https://docs.microsoft.com/cli/azure/ams/asset?view=azure-cli-latest#az-ams-asset-list-streaming-locators)|
|.NET|[ListStreamingLocators](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.assetsoperationsextensions.liststreaminglocators?view=azure-dotnet#Microsoft_Azure_Management_Media_AssetsOperationsExtensions_ListStreamingLocators_Microsoft_Azure_Management_Media_IAssetsOperations_System_String_System_String_System_String_)|
|Java|[AssetStreamingLocator](https://docs.microsoft.com/java/api/com.microsoft.azure.management.mediaservices.v2018_07_01.assetstreaminglocator?view=azure-java-stable)|
|Node.js|[listStreamingLocators](https://docs.microsoft.com/javascript/api/azure-arm-mediaservices/assets?view=azure-node-latest#liststreaminglocators-string--string--string--object-)|

## <a name="also-see"></a>Consulte también:

* [Recursos](assets-concept.md)
* [Directivas de streaming](streaming-policy-concept.md)
* [Directivas de claves de contenido](content-key-policy-concept.md)

## <a name="next-steps"></a>Pasos siguientes

[Tutorial: Carga, codificación y transmisión en secuencias de videos mediante .NET](stream-files-tutorial-with-api.md)
