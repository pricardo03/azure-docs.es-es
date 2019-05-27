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
ms.date: 05/22/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 68778cea51144ec33efd4d5843a51b489ea17ca4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155735"
---
# <a name="content-key-policies"></a>Directivas de clave de contenido

Con Media Services puede entregar el contenido cifrado de forma dinámica en vivo y a petición con el Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DMR (PlayReady, Widevine y FairPlay) a los clientes autorizados.

Para especificar opciones de cifrado en el flujo, deberá crear un [directiva Streaming](streaming-policy-concept.md) y asócielo con su [localizador de Streaming](streaming-locators-concept.md). Deberá crear un [directiva de clave de contenido](https://docs.microsoft.com/rest/api/media/contentkeypolicies) para configurar cómo la clave de contenido (que proporciona acceso seguro a su [activos](assets-concept.md)) se entrega para los clientes finales. El **directiva de clave de contenido** también está asociado a su **localizador de Streaming**. Deberá establecer los requisitos (restricciones) en la directiva de clave de contenido que se deben cumplir en orden para las claves con la configuración especificada que se entregará a los clientes. 

Se recomienda permitir que Media Services para generar automáticamente claves de contenido. Normalmente, usaría una clave de larga duración y comprobar la existencia de las directivas con **obtener**. Para obtener la clave, debe llamar a un método de acción independiente para conseguir los secretos o credenciales. Vea el ejemplo siguiente.

Las **directivas de clave de contenido** son actualizables. Por ejemplo, puede que quiera actualizar la directiva si necesita hacer una rotación de claves. Puede actualizar la clave de verificación principal y la lista de claves de verificación alternativas en la directiva existente. Puede tardar hasta 15 minutos para que las memorias caché de entrega de claves se actualicen y recojan la directiva actualizada. 

> [!IMPORTANT]
> * Las propiedades de **directivas de clave de contenido** que son de tipo Datetime siempre están en formato UTC.
> * Debería diseñar un conjunto limitado de directivas para su cuenta de Media Service y reutilizarlas con sus localizadores de streaming siempre que se necesiten las mismas opciones. Para más información, consulte [Cuotas y limitaciones](limits-quotas-constraints.md).

## <a name="example"></a>Ejemplo

Para obtener la clave, use **GetPolicyPropertiesWithSecretsAsync**, tal y como se muestra en el ejemplo siguiente.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="filtering-ordering-paging"></a>Filtrado, ordenación, paginación

Consulte [Filtrado, ordenación y paginación de entidades de Media Services](entities-overview.md).

## <a name="next-steps"></a>Pasos siguientes

* [Uso del cifrado dinámico AES-128 y el servicio de entrega de claves](protect-with-aes128.md)
* [Uso del cifrado dinámico de DRM y el servicio de entrega de licencias](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
