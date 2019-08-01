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
ms.date: 05/28/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: a597ab3519f4ba1696e111622541bcab89488558
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66425423"
---
# <a name="content-key-policies"></a>Directivas de clave de contenido

Con Media Services puede entregar el contenido cifrado de forma dinámica en vivo y a petición con el Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DMR (PlayReady, Widevine y FairPlay) a los clientes autorizados. 

Para especificar opciones de cifrado en la secuencia, debe crear la [directiva de streaming](streaming-policy-concept.md) y asociarla con su [localizador de streaming](streaming-locators-concept.md). La [directiva de clave de contenido](https://docs.microsoft.com/rest/api/media/contentkeypolicies) se crea para configurar el modo en que la clave de contenido (que proporciona acceso seguro a sus [recursos](assets-concept.md)) se entrega a los clientes finales. Debe establecer los requisitos (restricciones) en la directiva de clave de contenido que se deben cumplir para que las claves con la configuración especificada se entreguen a los clientes. Esta directiva de clave de contenido no es necesaria para el streaming o la descarga sin cifrar. 

Por lo general, se asocia la **directiva de clave de contenido** con el [localizador de streaming](streaming-locators-concept.md). Como alternativa, puede especificar la directiva de clave de contenido dentro de una [directiva de streaming](streaming-policy-concept.md) (al crear una directiva de streaming personalizada para escenarios avanzados). 

Se recomienda permitir que Media Services genere automáticamente las claves de contenido. Normalmente, se usaría una clave de larga duración y se comprobaría la existencia de directivas con **Get**. Para obtener la clave, debe llamar a un método de acción independiente para conseguir los secretos o credenciales. Vea el ejemplo siguiente.

Las **directivas de clave de contenido** son actualizables. Puede tardar hasta 15 minutos para que las memorias caché de entrega de claves se actualicen y recojan la directiva actualizada. 

> [!IMPORTANT]
> * Las propiedades de **directivas de clave de contenido** que son de tipo Datetime siempre están en formato UTC.
> * Debería diseñar un conjunto limitado de directivas para su cuenta de Media Service y reutilizarlas con sus localizadores de streaming siempre que se necesiten las mismas opciones. Para más información, consulte [Cuotas y limitaciones](limits-quotas-constraints.md).

### <a name="example"></a>Ejemplo

Para obtener la clave, use **GetPolicyPropertiesWithSecretsAsync**, tal y como se muestra en el ejemplo [Obtención de una clave de firma de la directiva existente](get-content-key-policy-dotnet-howto.md#get-contentkeypolicy-with-secrets).

## <a name="filtering-ordering-paging"></a>Filtrado, ordenación, paginación

Consulte [Filtrado, ordenación y paginación de entidades de Media Services](entities-overview.md).

## <a name="next-steps"></a>Pasos siguientes

* [Uso del cifrado dinámico AES-128 y el servicio de entrega de claves](protect-with-aes128.md)
* [Uso del cifrado dinámico de DRM y el servicio de entrega de licencias](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
