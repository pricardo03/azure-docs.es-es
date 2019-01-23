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
ms.date: 01/15/2019
ms.author: juliako
ms.openlocfilehash: 91e24fb274c1f9895046e8e2e7d760d02d196ccd
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54354185"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Streaming en vivo con Azure Media Services v3

Azure Media Services permite entregar eventos en directo a sus clientes en la nube de Azure. Para transmitir los eventos en directo con Media Services, necesita lo siguiente:  

- Una cámara que se utilice para capturar el evento en directo.
- Un codificador de vídeo en directo que convierte las señales de la cámara (u otro dispositivo, como un portátil) en una fuente de contribución que se envía a Media Services. La fuente de contribución puede incluir señales relacionadas con la publicidad, como los marcadores SCTE-35.
- Componentes de Media Services, que permiten ingerir, previsualizar, empaquetar, registrar, cifrar y difundir el evento en directo a los clientes o a una red CDN para una futura distribución.

En este artículo se proporciona una introducción general y una guía detalladas, y se incluyen diagramas de los componentes principales que intervienen en el streaming en vivo con Media Services.

## <a name="live-streaming-workflow"></a>Flujo de trabajo de streaming en vivo

Estos son los pasos para un flujo de trabajo de streaming en vivo:

1. Cree un **evento en directo**.
2. Cree un nuevo objeto de **recurso**.
3. Cree un objeto de **salida en directo** y use el nombre del recurso que ha creado.
4. Cree una **directiva de streaming** y la **clave de contenido** si se va a cifrar el contenido con DRM.
5. Si no utiliza DRM, cree un **localizador de streaming** con los tipos de **directiva de streaming** incorporados.
6. Enumere las rutas de acceso en la **directiva de streaming** para recuperar las direcciones URL que se van a utilizar (estas son deterministas).
7. Obtenga el nombre de host para el **punto de conexión de streaming** desde el que quiere realizar la transmisión. 
8. Combine la dirección URL del paso 6 con el nombre de host del paso 7 para obtener la dirección URL completa.
9. Si quiere que su **evento en directo** deje de estar visible, deberá detener la transmisión del evento mediante la eliminación del **localizador de streaming**.

Para más información, consulte un [tutorial de streaming en vivo](stream-live-tutorial-with-api.md) basado en el ejemplo [.NET Core en vivo](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/Live).

## <a name="overview-of-main-components"></a>Información general de los componentes principales

Para entregar secuencias bajo demanda o streaming en vivo con Media Services, debe tener al menos un objeto [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints). Cuando se crea la cuenta de Media Services, se agrega un punto de conexión de streaming **predeterminado** a la cuenta en estado **Detenido**. Debe iniciar el objeto StreamingEndpoint desde el que desea transmitir el contenido a los usuarios. Puede utilizar el objeto **StreamingEndpoint** predeterminado o crear otro objeto **StreamingEndpoint** personalizado con la configuración y los ajustes de la red CDN que desee. Puede decidir habilitar varios StreamingEndpoints, cada uno de ellos dirigido a una red CDN diferente, y proporcionar un nombre de host único para la entrega de contenido. 

En Media Services, los objetos [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) son responsables de la ingesta y el procesamiento de las fuentes de vídeo en directo. Al crear un objeto LiveEvent, se crea un punto de conexión de entrada que puede utilizar para enviar una señal en directo desde un codificador remoto. El codificador en directo remoto envía la fuente de contribución a ese punto de conexión de entrada mediante el protocolo [RTMP](https://www.adobe.com/devnet/rtmp.html) o [Smooth Streaming](https://msdn.microsoft.com/library/ff469518.aspx) (MP4 fragmentado). Para el protocolo de introducción Smooth Streaming, los esquemas de dirección URL admitidos son `http://` o `https://`. Para el protocolo de introducción RTMP, los esquemas de dirección URL admitidos son `rtmp://` o `rtmps://`. Para más información, consulte [Recommended live streaming encoders](recommended-on-premises-live-encoders.md) (Codificadores de streaming en vivo recomendados).

Cuando el objeto **LiveEvent** comience a recibir la fuente de contribución, puede utilizar el punto de conexión de vista previa (dirección URL de vista previa para obtener una vista previa y validar que está recibiendo el streaming en vivo antes de seguir publicándola. Una vez que haya comprobado que la secuencia de vista previa es buena, puede utilizar el objeto LiveEvent para que el streaming en vivo esté disponible para su entrega mediante uno o más objetos **StreamingEndpoints** (creados previamente). Para ello, cree un nuevo objeto [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) en **LiveEvent**. 

El objeto **LiveOutput** es como una grabadora que capta y graba el streaming en vivo en un recurso de su cuenta de Media Services. El contenido grabado persistirá en la cuenta de Azure Storage adjunta a su cuenta, en el contenedor definido por el recurso.  El objeto **LiveOuput** también le permite controlar algunas propiedades de la transmisión en vivo saliente, como la cantidad de la transmisión que se guarda en la grabación del archivo (por ejemplo, la capacidad del DVR en nube). El archivo en disco es una "ventana" circular de archivo que solo contiene la cantidad de contenido que se especifica en la propiedad **archiveWindowLength** de la propiedad **LiveOutput**. El contenido que está fuera de esta ventana se descarta automáticamente del contenedor de almacenamiento y no se puede recuperar. Puede crear varios objetos LiveOutputs (hasta un máximo de tres) en un objeto LiveEvent con diferentes longitudes y configuraciones de archivo.  

Con Media Services puede aprovechar el **empaquetado dinámico**, que le permite obtener una vista previa y difundir las transmisiones en vivo en los formatos [MPEG DASH, HLS y Smooth Streaming](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) desde la fuente de contribución que envía al servicio. Los espectadores pueden reproducir el streaming en vivo con cualquier reproductor compatible con HLS, DASH o Smooth Streaming. Puede utilizar [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) en las aplicaciones web o móviles para entregar la transmisión en cualquiera de estos protocolos.

Media Services permite entregar el contenido cifrado de forma dinámica (**cifrado dinámico**) con Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DRM a clientes autorizados. Para más información sobre cómo cifrar el contenido con Media Services, consulte [Introducción a la protección de contenido](content-protection-overview.md).

Si lo desea, también puede aplicar el filtro dinámico, que puede utilizarse para controlar el número de pistas, formatos, velocidades de bits y ventanas de tiempo de presentación que se envían a los reproductores. Para obtener más información, consulte [Filtros y manifiestos dinámicos](filters-dynamic-manifest-overview.md).

### <a name="new-capabilities-for-live-streaming-in-v3"></a>Nuevas funcionalidades para streaming en vivo en v3

Con la versión 3 de las API de Media Services, se beneficia de las siguientes características nuevas:

- Nuevo modo de latencia baja. Para más información, consulte [latencia](live-event-latency.md).
- Compatibilidad mejorada con RTMP (mayor estabilidad y mejor compatibilidad con codificadores de origen).
- Ingesta segura de RTMPS.<br/>Cuando se crea un objeto LiveEvent, obtiene cuatro direcciones URL de ingesta. Las cuatro direcciones URL de ingesta son casi idénticas, tienen el mismo token de streaming (AppId) y solo se diferencian en componente de número de puerto. Dos de las direcciones URL son principal y de respaldo para RTMPS.   
- Puede transmitir eventos en directo que tengan hasta 24 horas de duración al usar Media Services para transcodificar una fuente de contribución de velocidad de bits única en un flujo de salida que tiene varias velocidades de bits. 

## <a name="liveevent-types"></a>Tipos de objetos LiveEvent

Un objeto [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) puede ser de uno de estos dos tipos: paso a través y codificación en directo. 

### <a name="pass-through"></a>Paso a través

![paso a través](./media/live-streaming/pass-through.png)

Cuando se utiliza el LiveEvent de paso a través, se confía en el codificador en directo local para generar una secuencia de vídeo con múltiples velocidades de bits y enviarla como fuente de contribución a LiveEvent (mediante el protocolo RTMP o MP4 fragmentado). El objeto LiveEvent lleva a cabo las secuencias de vídeo entrantes sin ningún otro procesamiento. Este tipo de objeto LiveEvent está optimizado para eventos en vivo de larga duración o para el streaming en vivo ininterrumpidamente. Al crear este tipo de objeto LiveEvent, especifique None (LiveEventEncodingType.None).

Puede enviar la fuente de contribución a resoluciones de hasta 4K y a una velocidad de fotogramas de 60 fotogramas/segundo, con códecs de vídeo H.264/AVC o H.265/HEVC y códecs de audio AAC (AAC-LC, HE-AACv1 o HE-AACv2).  Consulte el artículo [Comparación de tipos de objetos LiveEvent](live-event-types-comparison.md) para obtener más detalles.

> [!NOTE]
> El uso de un método de paso a través es la forma más económica de streaming en vivo cuando está realizando varios eventos en un largo período y ya ha invertido en codificadores locales. Consulte los detalles en [Precios de Servicios multimedia](https://azure.microsoft.com/pricing/details/media-services/) .
> 

Consulte un ejemplo en vivo en [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live Encoding  

![codificación en directo](./media/live-streaming/live-encoding.png)

Si utiliza la codificación en directo con Media Services, deberá configurar el codificador en directo local para que envíe un vídeo con una única velocidad de bits como fuente de contribución a LiveEvent (mediante el protocolo RTMP o Mp4 fragmentado). LiveEvent codifica ese flujo de una sola tasa de bits entrante en un [flujo de vídeo con múltiples velocidades de bits](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) y hace que esté disponible para que pueda reproducirse en dispositivos mediante protocolos como MPEG-DASH, HLS y Smooth Streaming. Al crear este tipo de objeto LiveEvent, especifique el tipo de codificación como **Estándar** (LiveEventEncodingType.Standard).

Puede enviar la fuente de contribución a una resolución de hasta 1080p a una velocidad de fotogramas de 30 fotogramas/segundo, con códec de vídeo H.264/AVC y códec de audio AAC (AAC-LC, HE-AACv1 o HE-AACv2). Consulte el artículo [Comparación de tipos de objetos LiveEvent](live-event-types-comparison.md) para obtener más detalles.

## <a name="liveevent-types-comparison"></a>Comparación de tipos de objetos LiveEvent

El siguiente artículo contiene una tabla que compara las características de los dos tipos de objetos LiveEvent: [Comparación](live-event-types-comparison.md).

## <a name="liveoutput"></a>LiveOutput

Un objeto [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) le permite controlar las propiedades de la transmisión saliente en vivo, como qué parte de la transmisión se registra (por ejemplo, la capacidad de la DVR en la nube) y si los usuarios pueden empezar a ver la transmisión en vivo. La relación entre un objeto **LiveEvent** y su relación de **LiveOutput** es similar a la difusión de televisión tradicional, en la que un canal (**LiveEvent**) representa un flujo constante de vídeo y una grabación (**LiveOutput**) tiene un ámbito de un segmento de tiempo específico (por ejemplo, noticias vespertinas de 18:30 a 19:00). Puede grabar televisión con una grabadora de vídeo digital (DVR): la característica equivalente en LiveEvents se administra a través de la propiedad ArchiveWindowLength. Se trata de una duración de timespan ISO 8601 (por ejemplo, PTHH:MM:SS), que especifica la capacidad de la DVR y se puede establecer desde un mínimo de 3 minutos hasta un máximo de 25 horas.

> [!NOTE]
> Los objetos **LiveOutput** se inician al crearlos y se detienen cuando se eliminan. Cuando se elimina el objeto **LiveOutput**, no se elimina el **recurso** subyacente ni su contenido. 
>
> Si ha publicado **localizadores de streaming** en el recurso para el objeto **LiveOutput**, el evento (hasta la longitud de ventana DVR) seguirá visible hasta la hora de finalización del **localizador de streaming**  o hasta que elimine el localizador, lo que ocurra primero.   

Para más información, consulte [Uso de DVR de nube](live-event-cloud-dvr.md).

## <a name="streamingendpoint"></a>StreamingEndpoint

Cuando la secuencia fluya al objeto **LiveEvent**, puede comenzar el evento de streaming mediante la creación de los objetos **Asset**, **LiveOutput** y **StreamingLocator**. Se archivará la secuencia y estará disponible a los usuarios a través del objeto [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Cuando se crea la cuenta de Media Services, se agrega un punto de conexión de streaming predeterminado a la cuenta en estado Detenido. Para iniciar la transmisión del contenido y aprovechar el empaquetado dinámico y el cifrado dinámico, el punto de conexión de streaming desde el que va a transmitir el contenido debe estar en estado En ejecución.

## <a name="a-idbilling-liveevent-states-and-billing"></a><a id="billing" />Estados y facturación de LiveEvent

Un objeto LiveEvent comienza la facturación tan pronto como su estado realiza la transición a **En ejecución**. Para detener la facturación del objeto LiveEvent, debe pararlo.

Para más información, consulte [Estados y facturación](live-event-states-billing.md).

## <a name="latency"></a>Latencia

Para más información sobre la latencia de objetos LiveEvent, consulte [Latencia](live-event-latency.md).

## <a name="next-steps"></a>Pasos siguientes

- [Comparación de tipos de objetos LiveEvent](live-event-types-comparison.md)
- [Estados y facturación](live-event-states-billing.md)
- [Latency](live-event-latency.md)
