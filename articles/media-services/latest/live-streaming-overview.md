---
title: Introducción al streaming en vivo con Azure Media Services | Microsoft Docs
description: En este artículo se proporciona información general de streaming en vivo con Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/27/2019
ms.author: juliako
ms.openlocfilehash: a3e4821d9deb7ceee815d804f58d0b1ba14925b4
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103571"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Streaming en vivo con Azure Media Services v3

Azure Media Services permite entregar eventos en directo a sus clientes en la nube de Azure. Para transmitir los eventos en directo con Media Services, necesita lo siguiente:  

- Una cámara que se utilice para capturar el evento en directo.<br/>Para obtener ideas para la configuración, consulte [Simple and portable event video gear setup]( https://link.medium.com/KNTtiN6IeT) (Equipo de vídeo para eventos sencillo y portátil).
- Un codificador de vídeo en vivo que convierte las señales de una cámara (u otro dispositivo, como un portátil) en una fuente de contribución que se envía a Media Services. La fuente de contribución puede incluir señales relacionadas con la publicidad, como los marcadores SCTE-35.<br/>Para obtener una lista de los codificadores de streaming en vivo recomendados, consulte [Codificadores de streaming en vivo](recommended-on-premises-live-encoders.md). Vea también este blog: [Live streaming production with OBS](https://link.medium.com/ttuwHpaJeT) (Producción de streaming en vivo con OBS).
- Componentes de Media Services, que permiten ingerir, previsualizar, empaquetar, registrar, cifrar y difundir el evento en directo a los clientes o a una red CDN para una futura distribución.

Con Media Services puede aprovechar el **empaquetado dinámico**, que le permite obtener una vista previa y difundir las secuencias en vivo en los [formatos MPEG DASH, HLS y Smooth Streaming](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) desde la fuente de contribución que envía al servicio. Los espectadores pueden reproducir el streaming en vivo con cualquier reproductor compatible con HLS, DASH o Smooth Streaming. Puede utilizar [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) en las aplicaciones web o móviles para entregar la transmisión en cualquiera de estos protocolos.

Media Services permite entregar el contenido cifrado de forma dinámica (**cifrado dinámico**) con Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DRM a clientes autorizados. Para más información sobre cómo cifrar el contenido con Media Services, consulte [Introducción a la protección de contenido](content-protection-overview.md).

Si lo desea, también puede aplicar el filtro dinámico, que puede utilizarse para controlar el número de pistas, formatos, velocidades de bits y ventanas de tiempo de presentación que se envían a los reproductores. Para obtener más información, consulte [Filtros y manifiestos dinámicos](filters-dynamic-manifest-overview.md).

En este artículo se proporciona información general y una guía del streaming en vivo con Media Services.

## <a name="prerequisites"></a>Requisitos previos

Para conocer el flujo de trabajo de streaming en vivo de Media Services v3, tendrá que examinar y conocer los conceptos siguientes: 

- [Puntos de conexión de streaming](streaming-endpoint-concept.md)
- [Objetos LiveEvent y LiveOutput](live-events-outputs-concept.md)

## <a name="live-streaming-workflow"></a>Flujo de trabajo de streaming en vivo

Estos son los pasos para un flujo de trabajo de streaming en vivo:

1. Vaya a la cuenta de Media Services y asegúrese de que el **punto de conexión de streaming** se ejecuta. 
2. Cree un **evento en directo**. <br/>Al crear el evento, puede especificar que se inicie automáticamente. De lo contrario, puede iniciar el evento cuando esté listo para iniciar el streaming.<br/> Cuando el inicio automático está establecido en true, el evento en directo se iniciará después de la creación. Es decir, la facturación comienza en cuanto el objeto LiveEvent empieza a ejecutarse. Debe llamar explícitamente a Stop en el recurso del objeto LiveEvent para evitar que continúe la facturación. Para más información, consulte [Estados y facturación de LiveEvent](live-event-states-billing.md).
3. Obtenga las direcciones URL de ingesta y configure el codificador local para usar la dirección URL para enviar la fuente de contribución.<br/>Consulte [Codificadores de streaming en vivo recomendados](recommended-on-premises-live-encoders.md).
4. Obtenga la dirección URL de versión preliminar y úsela para verificar que la entrada del codificador se está recibiendo realmente.
5. Cree un nuevo objeto de **recurso**.
6. Cree un objeto de **salida en directo** y use el nombre del recurso que ha creado.<br/>El objeto **LiveOutput** archivará la secuencia en el objeto **Asset**.
7. Cree un objeto **Streaming Locator** con los tipos del objeto **Streaming Policy** integrados.<br/>Si va a cifrar el contenido, consulte [Introducción a la protección de contenido](content-protection-overview.md).
8. Enumere las rutas de acceso en el **localizador de streaming** para recuperar las direcciones URL que se van a usar (estas son deterministas).
9. Obtenga el nombre de host para el **punto de conexión de streaming** desde el que quiere hacer el streaming.
10. Combine la dirección URL del paso 8 con el nombre de host del paso 9 para obtener la dirección URL completa.
11. Si desea que **LiveEvent** deje de estar visible, es preciso que deje de transmitir en secuencias el evento y que elimine el objeto **StreamingLocator**.

## <a name="other-important-articles"></a>Otros artículos importantes

- [Recommended live encoders](recommended-on-premises-live-encoders.md) (Codificadores en directo recomendados)
- [Uso de un DVR en la nube](live-event-cloud-dvr.md)
- [Comparación de tipos de objetos LiveEvent](live-event-types-comparison.md)
- [Estados y facturación](live-event-states-billing.md)
- [Latency](live-event-latency.md)

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial de Live Streaming](stream-live-tutorial-with-api.md)
* [Guía de migración para mover de Media Services v2 a v3](migrate-from-v2-to-v3.md)
