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
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 51aa33e4ff387a1030dac42bce8d12cf72343b35
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61466738"
---
# <a name="streaming-locators"></a>Localizadores de streaming

Para que los vídeos en el recurso de salida estén disponibles para los clientes para reproducción, tiene que crear un objeto [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) y, luego, generar direcciones URL de streaming. Para un ejemplo de.NET, consulte [Obtención de un objeto StreamingLocator](stream-files-tutorial-with-api.md#get-a-streaming-locator).

El proceso de creación de un objeto **StreamingLocator** se denomina publicación. De forma predeterminada, el objeto **StreamingLocator** es válido inmediatamente después de realizar las llamadas a la API y dura hasta que se elimina, salvo que configure las horas de inicio y de finalización opcionales. 

Al crear un objeto **StreamingLocator**, debe especificar el nombre del [Recurso](https://docs.microsoft.com/rest/api/media/assets) y el nombre de la [Directiva de streaming](https://docs.microsoft.com/rest/api/media/streamingpolicies). Puede usar una de las directivas de streaming predefinidas o crear una directiva personalizada. Las directivas predefinidas que están disponibles actualmente son: "Predefined_DownloadOnly", "Predefined_ClearStreamingOnly", "Predefined_DownloadAndClearStreaming", "Predefined_ClearKey", "Predefined_MultiDrmCencStreaming" y "Predefined_ MultiDrmStreaming". Al usar una directiva de streaming personalizada, debe diseñar un conjunto limitado de dichas directivas para su cuenta de Media Service y reutilizarlas para sus objetos StreamingLocator siempre que se necesiten las mismas opciones y protocolos. 

Si quiere especificar las opciones de cifrado en la secuencia, cree la [directiva de clave de contenido](https://docs.microsoft.com/rest/api/media/contentkeypolicies) configura cómo se entrega la clave de contenido a los clientes finales mediante el componente de entrega de claves de Media Services. Asocie el objeto StreamingLocator con la **directiva de clave de contenido** y la clave de contenido. Puede permitir que Media Services genere automáticamente la clave. El siguiente ejemplo de .NET muestra cómo configurar el cifrado de AES con una restricción de token en Media Services v3: [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted). Las **directivas de clave de contenido** se pueden actualizar, por ejemplo, puede que quiera actualizar la directiva si necesita hacer una rotación de claves. Puede tardar hasta 15 minutos para que las memorias caché de entrega de claves se actualicen y recojan la directiva actualizada. Se recomienda no crear una nueva directiva de clave de contenido para cada objeto StreamingLocator. Debe intentar volver a usar las directivas existentes siempre que se necesiten las mismas opciones.

> [!IMPORTANT]
> * Las propiedades de objetos **StreamingLocator** que son del tipo Datetime siempre están en formato UTC.
> * Debería diseñar un conjunto limitado de directivas para su cuenta de Media Service y reutilizarlas con los objetos StreamingLocator siempre que se necesiten las mismas opciones. 

## <a name="associate-filters-with-streaming-locators"></a>Asociar filtros a los localizadores de Streaming

Puede especificar una lista de [filtros activos o cuenta](filters-concept.md), que se aplicará a su [localizador de Streaming](https://docs.microsoft.com/rest/api/media/streaminglocators/create#request-body). El [empaquetador dinámico](dynamic-packaging-overview.md) se aplica a esta lista de filtros junto con los que el cliente se especifica en la dirección URL. Esta combinación se genera un [dyanamic manifiesto](filters-dynamic-manifest-overview.md), que se basa en los filtros en la dirección URL y los filtros que especifique en el localizador de Streaming. Se recomienda usar esta característica si desea aplicar filtros pero no desea exponer los nombres de filtro en la dirección URL.

## <a name="filter-order-page-streaming-locator-entities"></a>Filtro, por orden, las entidades de localizador de Streaming de página

Consulte [Filtrado, ordenación y paginación de entidades de Media Services](entities-overview.md).

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Carga, codificación y transmisión en secuencias de videos mediante .NET](stream-files-tutorial-with-api.md)
* [Uso del cifrado dinámico de DRM y el servicio de entrega de licencias](protect-with-drm.md)
