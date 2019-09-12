---
title: 'Conceptos de Azure Media Services: eventos en vivo y salidas en vivo | Microsoft Docs'
description: En este artículo se proporciona información general de conceptos de streaming en vivo con Azure Media Services v3.
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
ms.date: 08/26/2019
ms.author: juliako
ms.openlocfilehash: c81c2de180a2c5734f3896d4b6843f2ccccdf45f
ms.sourcegitcommit: 2aefdf92db8950ff02c94d8b0535bf4096021b11
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/03/2019
ms.locfileid: "70231202"
---
# <a name="live-events-and-live-outputs"></a>Eventos en directo y salidas en vivo

Azure Media Services permite entregar eventos en directo a sus clientes en la nube de Azure. Para configurar los eventos de streaming en vivo en Media Services v3, debe comprender los conceptos examinados en este artículo:

> [!TIP]
> En el caso de los clientes que migran desde las API de Media Services v2, la entidad **Live Event** reemplaza a **Channel** en v2 y **Live Output** reemplaza a **Program**.

## <a name="live-events"></a>Eventos en vivo

Los objetos [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) son responsables de la ingesta y el procesamiento de las fuentes de vídeo en directo. Al crear un evento en directo, se crea un punto de conexión de entrada primario y secundario que puede usar para enviar una señal en directo desde un codificador remoto. El codificador en directo remoto envía la fuente de contribución a ese punto de conexión de entrada mediante el protocolo de entrada [RTMP](https://www.adobe.com/devnet/rtmp.html) o [Smooth Streaming](https://msdn.microsoft.com/library/ff469518.aspx) (MP4 fragmentado). Con el protocolo de ingesta de RTMP, el contenido se puede enviar sin cifrar (`rtmp://`) o cifrado de forma segura en la conexión (`rtmps://`). Para el protocolo de introducción Smooth Streaming, los esquemas de dirección URL admitidos son `http://` o `https://`.  

## <a name="live-event-types"></a>Tipos de objetos LiveEvent

Un [objeto LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) puede ser de uno de estos dos tipos: paso a través y codificación en directo. Los tipos se establecen durante la creación mediante [LiveEventEncodingType](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventencodingtype):

* **LiveEventEncodingType.None**: un codificador en directo local envía una secuencia de velocidad de bits múltiple. Las secuencias ingeridas pasan por el evento en directo sin más procesamiento. 
* **LiveEventEncodingType.Standard**: un codificador en directo local envía una secuencia única de velocidad de bits al evento en directo y Media Services crea varias secuencias de velocidad de bits. Si la fuente de contribución tiene una resolución de 720p o más, el valor preestablecido **Default720p** codificará un conjunto de seis pares de velocidad de bits-resolución.
* **LiveEventEncodingType.Premium1080p**: un codificador en directo local envía una única secuencia de velocidad de bits al evento en directo y Media Services crea varias secuencias de velocidad de bits. El valor preestablecido Default1080p especifica el conjunto de salida de pares de resolución-velocidad de bits. 

### <a name="pass-through"></a>Paso a través

![paso a través](./media/live-streaming/pass-through.svg)

Cuando se utiliza el **objeto LiveEvent** de paso a través, se confía en el codificador en directo local para generar una secuencia de vídeo con múltiples velocidades de bits y enviarla como fuente de contribución al objeto LiveEvent (mediante el protocolo RTMP o MP4 fragmentado). El objeto LiveEvent lleva a cabo las secuencias de vídeo entrantes sin ningún otro procesamiento. Este tipo de objeto LiveEvent de paso a través está optimizado para eventos en directo de larga duración o para el streaming en vivo ininterrumpidamente. Al crear este tipo de objeto LiveEvent, especifique None (LiveEventEncodingType.None).

Puede enviar la fuente de contribución a resoluciones de hasta 4K y a una velocidad de fotogramas de 60 fotogramas/segundo, con códecs de vídeo H.264/AVC o H.265/HEVC y códecs de audio AAC (AAC-LC, HE-AACv1 o HE-AACv2).  Consulte el artículo [Comparación de tipos de objetos LiveEvent](live-event-types-comparison.md) para obtener más detalles.

> [!NOTE]
> El uso de un método de paso a través es la forma más económica de streaming en vivo cuando está realizando varios eventos en un largo período y ya ha invertido en codificadores locales. Consulte los detalles en [Precios de Servicios multimedia](https://azure.microsoft.com/pricing/details/media-services/) .
> 

Consulte un ejemplo de código .NET en [MediaV3LiveApp](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/Live/MediaV3LiveApp/Program.cs#L126).

### <a name="live-encoding"></a>Live Encoding  

![codificación en directo](./media/live-streaming/live-encoding.svg)

Si utiliza la codificación en directo con Media Services, deberá configurar el codificador en directo local para que envíe un vídeo con una única velocidad de bits como fuente de contribución al objeto LiveEvent (mediante el protocolo RTMP o Mp4 fragmentado). Configuraría entonces un evento en directo para que codifique esa secuencia de velocidad de bits única entrante como una [secuencia de vídeo de velocidad de bits múltiple](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming) y permita que la salida esté disponible para la entrega para la reproducción de los dispositivos mediante protocolos como MPEG-DASH, HLS y Smooth Streaming.

Cuando se usa la codificación en directo, puede enviar la fuente de contribución solo con resoluciones de hasta 1080p y a una velocidad de 30 fotogramas/segundo, con el códec de vídeo H.264/AVC y el códec de audio AAC (AAC-LC, HE-AACv1 o HE-AACv2). Tenga en cuenta que los eventos en directo de paso a través pueden admitir resoluciones de hasta 4 K a 60 fotogramas por segundo. Consulte el artículo [Comparación de tipos de objetos LiveEvent](live-event-types-comparison.md) para obtener más detalles.

Las resoluciones y velocidades de bits contenidas en la salida del codificador en directo vienen determinadas por el valor preestablecido. Si usa un codificador en directo **Standard** (LiveEventEncodingType.Standard), el valor preestablecido *Default720p* especifica un conjunto de seis pares de resolución-velocidad de bits, que van desde 720p a 3,5 Mbps hasta 192p a 200 kbps. De lo contrario, si usa un codificador en directo **Premium1080p** (LiveEventEncodingType.Premium1080p), el valor preestablecido *Default1080p* especifica un conjunto de seis pares de resolución-velocidad de bits, que van desde 1080p a 3,5 Mbps hasta 180p a 200 kbps. Para más información, consulte [Valores preestablecidos del sistema](live-event-types-comparison.md#system-presets).

> [!NOTE]
> Si necesita personalizar el valor preestablecido de codificación en directo, abra una incidencia de soporte técnico a través de Azure Portal. Debe especificar la tabla de resoluciones y velocidades de bits deseadas. Compruebe que solo haya una capa a 720p (si se solicita un valor preestablecido para un codificador en directo Standard) o a 1080p (si se solicita un valor preestablecido para un codificador en directo Premium1080p) y, al menos, seis capas.

## <a name="live-event-creation-options"></a>Opciones de creación de objetos LiveEvent

Al crear un objeto LiveEvent, puede especificar las siguientes opciones:

* El protocolo de streaming para el objeto LiveEvent (actualmente, se admiten los protocolos RTMP y Smooth Streaming).<br/>No se puede cambiar la opción de protocolo mientras estén en ejecución el objeto LiveEvent o sus objetos LiveOutput asociados. Si necesita diferentes protocolos, debe crear un objeto LiveEvent independiente para cada protocolo de streaming.  
* Restricciones de IP en la ingesta y vista previa. Puede definir las direcciones IP a las que se permite ingerir un vídeo en este objeto LiveEvent. Las direcciones IP permitidas se pueden especificar como una dirección IP única (por ejemplo, 10.0.0.1), un intervalo IP que usa una dirección IP y una máscara de subred CIDR (por ejemplo, 10.0.0.1/22) o un intervalo de IP que usa una máscara de subred decimal con puntos; por ejemplo, 10.0.0.1(255.255.252.0).<br/>Si no se especifica ninguna dirección IP y no hay ninguna definición de regla, no se permitirá ninguna dirección IP. Para permitir las direcciones IP, cree una regla y establezca 0.0.0.0/0.<br/>Las direcciones IP deben estar en uno de los siguientes formatos: dirección IpV4 con 4 números, intervalo de direcciones CIDR.
* Al crear el evento, puede especificar que se inicie automáticamente. <br/>Cuando el inicio automático está establecido en true, el evento en directo se inicia después de la creación. La facturación comienza en cuanto el objeto LiveEvent empieza a ejecutarse. Debe llamar explícitamente a Stop en el recurso del objeto LiveEvent para evitar que continúe la facturación. De lo contrario, puede iniciar el evento cuando esté listo para iniciar el streaming. 

    Para más información, consulte [Estados y facturación de LiveEvent](live-event-states-billing.md).

## <a name="live-event-ingest-urls"></a>Direcciones URL de ingesta de objetos LiveEvent

Una vez que se crea el objeto LiveEvent, puede ingerir las direcciones URL que proporcionará al codificador en directo. El codificador en directo usa estas direcciones URL para introducir una secuencia en vivo. Para más información, consulte [Recommended live streaming encoders](recommended-on-premises-live-encoders.md) (Codificadores de streaming en vivo recomendados). 

Puede usar direcciones URL que sean mnemónicas o no mnemónicas. 

> [!NOTE] 
> Para que una dirección URL de ingesta sea predictiva, establezca el modo "mnemónica".

* Dirección URL no mnemónica

    Dirección URL no mnemónica es el modo predeterminado en Media Services v3. Posiblemente, puede obtener el objeto LiveEvent rápidamente, pero solo conocerá la dirección URL de ingesta cuando se haya iniciado el evento en directo. La dirección URL cambiará si detiene o inicia el objeto LiveEvent. <br/>El modo no mnemónico es útil en aquellos casos en los que un usuario final quiere hacer streaming mediante una aplicación y esta quiere obtener un objeto LiveEvent lo antes posible, y en los que tener una dirección URL de ingesta dinámica no es un problema.
    
    Si no es necesario que la aplicación cliente genere previamente una dirección URL de ingesta para crear el evento en directo, permita que Media Services genere automáticamente el token de acceso para el evento en directo.
* Dirección URL mnemónica

    El modo mnemónico lo prefieren los grandes operadores de difusión multimedia que usan codificadores de difusión y no quieren volver a configurarlos cuando empiezan un objeto LiveEvent. Prefieren una dirección URL de ingesta predictiva que no cambie con el tiempo.
    
    Para especificar este modo, establezca `vanityUrl` en `true` en tiempo de creación (el valor predeterminado es `false`). También debe pasar su propio token de acceso (`LiveEventInput.accessToken`) en tiempo de creación. El valor del token se especifica para evitar un token aleatorio en la dirección URL. El token de acceso debe ser una cadena GUID válida (con o sin guiones). Una vez establecido el modo, no puede actualizarse.

    El token de acceso debe ser único en su centro de datos. Si la aplicación necesita usar una dirección URL mnemónica, se recomienda crear siempre una instancia de GUID para el token de acceso (en lugar de reutilizar el GUID existente). 

    Use las siguientes API para habilitar la dirección URL mnemónica y establezca el token de acceso en un GUID válido (por ejemplo, `"accessToken": "1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`).  
    
    |Idioma|Habilitar la dirección URL mnemónica|Establecer un token de acceso|
    |---|---|---|
    |REST|[properties.vanityUrl](https://docs.microsoft.com/rest/api/media/liveevents/create#liveevent)|[LiveEventInput.accessToken](https://docs.microsoft.com/rest/api/media/liveevents/create#liveeventinput)|
    |CLI|[--vanity-url](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#az-ams-live-event-create)|[--access-token](https://docs.microsoft.com/cli/azure/ams/live-event?view=azure-cli-latest#optional-parameters)|
    |.NET|[LiveEvent.VanityUrl](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveevent.vanityurl?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEvent_VanityUrl)|[LiveEventInput.AccessToken](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.media.models.liveeventinput.accesstoken?view=azure-dotnet#Microsoft_Azure_Management_Media_Models_LiveEventInput_AccessToken)|
    
### <a name="live-ingest-url-naming-rules"></a>Reglas de nomenclatura de direcciones URL de ingesta en directo

* La cadena *random* que aparece a continuación es un número hexadecimal de 128 bits (que se compone de 32 caracteres del 0 al 9 y de la "a" a la "f").
* *el token de acceso*: la cadena GUID válida establecida al usar el modo mnemónico. Por ejemplo, `"1fce2e4b-fb15-4718-8adc-68c6eb4c26a7"`.
* *nombre de secuencia*: indica el nombre de la secuencia de una conexión específica. El valor del nombre de secuencia normalmente lo agrega el codificador en directo que use. Puede configurar el codificador en directo para usar cualquier nombre para describir la conexión, por ejemplo: "vídeo1_audio1", "vídeo2_audio1", "secuencia".

#### <a name="non-vanity-url"></a>Dirección URL no mnemónica

##### <a name="rtmp"></a>RTMP

`rtmp://<random 128bit hex string>.channel.media.azure.net:1935/live/<auto-generated access token>/<stream name>`<br/>
`rtmp://<random 128bit hex string>.channel.media.azure.net:1936/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2935/live/<auto-generated access token>/<stream name>`<br/>
`rtmps://<random 128bit hex string>.channel.media.azure.net:2936/live/<auto-generated access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<random 128bit hex string>.channel.media.azure.net/<auto-generated access token>/ingest.isml/streams(<stream name>)`<br/>

#### <a name="vanity-url"></a>Dirección URL mnemónica

##### <a name="rtmp"></a>RTMP

`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1935/live/<your access token>/<stream name>`<br/>
`rtmp://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:1936/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2935/live/<your access token>/<stream name>`<br/>
`rtmps://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net:2936/live/<your access token>/<stream name>`<br/>

##### <a name="smooth-streaming"></a>Smooth Streaming

`http://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>
`https://<live event name>-<ams account name>-<region abbrev name>.channel.media.azure.net/<your access token>/ingest.isml/streams(<stream name>)`<br/>

## <a name="live-event-preview-url"></a>Dirección URL de vista previa de objeto LiveEvent

Cuando el objeto LiveEvent comienza a recibir la fuente de contribución, puede usar el punto de conexión de vista previa para obtener una vista previa y validar que está recibiendo el streaming en vivo antes de continuar con la publicación. Después de haber comprobado que la secuencia de vista previa es buena, puede usar el evento en directo para que el streaming en vivo esté disponible para su entrega mediante uno o más puntos de conexión de streaming (creados previamente). Para ello, cree un objeto [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) en el objeto LiveEvent. 

> [!IMPORTANT]
> Asegúrese de que el vídeo fluye a la dirección URL de vista previa antes de continuar.

## <a name="live-event-long-running-operations"></a>Operaciones de larga duración de eventos en directo

Para más información, consulte [Operaciones de larga duración](media-services-apis-overview.md#long-running-operations).

## <a name="live-outputs"></a>Salidas en vivo

Una vez que la secuencia fluye en el objeto LiveEvent, puede comenzar el evento de streaming mediante la creación de un [recurso](https://docs.microsoft.com/rest/api/media/assets), un [objeto LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) y un [objeto StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators). El objeto LiveOutput archivará la secuencia y la pondrá a disposición de los usuarios a través del [punto de conexión de streaming](https://docs.microsoft.com/rest/api/media/streamingendpoints).  

Para obtener información detallada sobre las salidas en directo, consulte [Uso de una DVR en la nube](live-event-cloud-dvr.md).

## <a name="ask-questions-give-feedback-get-updates"></a>Formule preguntas, realice comentarios y obtenga actualizaciones

Consulte el artículo [Comunidad de Azure Media Services](media-services-community.md) para ver diferentes formas de formular preguntas, enviar comentarios y obtener actualizaciones de Media Services.

## <a name="next-steps"></a>Pasos siguientes

[Tutorial de Live Streaming](stream-live-tutorial-with-api.md)
