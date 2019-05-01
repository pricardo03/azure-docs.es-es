---
title: 'Directivas de clave de contenido de Media Services: Azure | Microsoft Docs'
description: En este artículo se explica qué son las directivas de clave de contenido y cómo las usa Azure Media Services.
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
ms.custom: seodec18
ms.openlocfilehash: d9e86c45d535862e0c3d02b3f331bc40ebb7f6c7
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60733052"
---
# <a name="content-key-policies"></a>Directivas de clave de contenido

Con Media Services puede entregar el contenido cifrado de forma dinámica en vivo y a petición con el Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DMR (PlayReady, Widevine y FairPlay) a los clientes autorizados.

Para especificar opciones de cifrado en la secuencia, debe crear la [directiva de clave de contenido](https://docs.microsoft.com/rest/api/media/contentkeypolicies) y asociarla con su **localizador de streaming**. La **directiva de clave de contenido** configura cómo se entrega la clave de contenido a los clientes finales mediante el componente de entrega de claves de Media Services. Puede permitir que Media Services genere automáticamente la clave de contenido. Normalmente, usaría una clave de larga duración y comprobaría la existencia de directivas con Get. Para obtener la clave, debe llamar a un método de acción independiente para conseguir los secretos o credenciales. Vea el ejemplo siguiente.

Las **directivas de clave de contenido** son actualizables. Por ejemplo, puede que quiera actualizar la directiva si necesita hacer una rotación de claves. Puede actualizar la clave de verificación principal y la lista de claves de verificación alternativas en la directiva existente. Puede tardar hasta 15 minutos para que las memorias caché de entrega de claves se actualicen y recojan la directiva actualizada. 

> [!IMPORTANT]
> * Las propiedades de **directivas de clave de contenido** que son de tipo Datetime siempre están en formato UTC.
> * Debería diseñar un conjunto limitado de directivas para su cuenta de Media Service y reutilizarlas con sus localizadores de streaming siempre que se necesiten las mismas opciones. 

## <a name="example"></a>Ejemplo

Para obtener la clave, use **GetPolicyPropertiesWithSecretsAsync**, tal y como se muestra en el ejemplo siguiente.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="filtering-ordering-paging"></a>Filtrado, ordenación, paginación

Consulte [Filtrado, ordenación y paginación de entidades de Media Services](entities-overview.md).

## <a name="next-steps"></a>Pasos siguientes

* [Uso del cifrado dinámico AES-128 y el servicio de entrega de claves](protect-with-aes128.md)
* [Uso del cifrado dinámico de DRM y el servicio de entrega de licencias](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
