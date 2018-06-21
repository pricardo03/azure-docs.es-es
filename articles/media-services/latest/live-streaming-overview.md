---
title: Introducción al streaming en vivo con Azure Media Services | Microsoft Docs
description: En este tema se proporciona información general de streaming en vivo con Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/06/2018
ms.author: juliako
ms.openlocfilehash: b8c9375d8ad915200cbc8b2e1a62979fd1b7d179
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35237106"
---
# <a name="live-streaming-with-azure-media-services-v3"></a>Streaming en vivo con Azure Media Services v3

Cuando se proporcionan eventos de streaming en vivo con Azure Media Services normalmente participan los siguientes componentes:

* Una cámara que se usa para difundir un evento.
* Un codificador de vídeo en vivo convierte las señales de la cámara o de otro dispositivo, como un portátil, en secuencias que se envían al servicio de streaming en vivo de Media Services. Las señales también pueden incluir publicidad con SCTE-35 y señales de anuncios. 
* El servicio de streaming en vivo de Media Services permite ingerir, previsualizar, empaquetar, registrar, cifrar y difundir contenido a los clientes o a una red CDN para una futura distribución.

En este artículo se proporciona una descripción detallada y se incluyen diagramas de los componentes principales que intervienen en el streaming en vivo con Media Services.

## <a name="overview-of-main-components"></a>Información general de los componentes principales

En Media Services, los objetos [LiveEvents](https://docs.microsoft.com/rest/api/media/liveevents) son los responsables de procesar el contenido de streaming en vivo. Un objeto LiveEvent proporciona un punto de conexión de entrada (dirección URL de ingesta) que luego se puede ofrecer a un codificador en directo local. El objeto LiveEvent recibe flujos de entrada en vivo desde el codificador en directo en formato RTMP o Smooth Streaming y los deja a disposición del streaming a través de uno o más objetos [StreamingEndpoints](https://docs.microsoft.com/rest/api/media/streamingendpoints). Un objeto [LiveOutput](https://docs.microsoft.com/en-us/rest/api/media/liveoutputs) permite controlar la publicación, la grabación y la configuración de la ventana DVR del flujo en vivo. El objeto LiveEvent también proporciona un punto de conexión de vista previa (dirección URL de vista previa) que se puede utilizar para obtener una vista previa y validar el flujo antes de un procesamiento y entrega ulteriores. 

Media Services proporciona **empaquetado dinámico** que permite previsualizar y difundir contenido en los formatos de streaming MPEG DASH, HLS y Smooth Streaming sin tener que volver a empaquetar manualmente en dichos formatos. Puede reproducir con cualquier reproductor compatible con HLS, DASH o Smooth. También puede usar [Azure Media Player](http://amp.azure.net/libs/amp/latest/docs/index.html) para probar el flujo.

Media Services permite entregar el contenido cifrado de forma dinámica (**cifrado dinámico**) con Estándar de cifrado avanzado (AES-128) o cualquiera de los tres sistemas de administración de derechos digitales (DRM) principales: Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DMR (PlayReady, Widevine y FairPlay) a los clientes autorizados.

Si lo desea, también puede aplicar el **filtro dinámico**, que puede utilizarse para controlar el número de pistas, formatos y velocidades de bits, que se envían a los reproductores. Media Services también admite la inserción de anuncios.


## <a name="liveevent-types"></a>Tipos de objetos LiveEvent

Un objeto [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) puede ser de uno de estos dos tipos: codificación en directo y paso a través. 

### <a name="live-encoding-with-media-services"></a>Codificación en directo con Media Services

![codificación en directo](./media/live-streaming/live-encoding.png)

Un codificador en directo local envía una secuencia de una sola velocidad de bits al objeto LiveEvent que está habilitado para realizar codificación en directo con Media Services, con uno de los siguientes protocolos: RTP o Smooth Streaming (MP4 fragmentado). Después, el objeto LiveEvent codifica en directo la secuencia entrante de una sola velocidad de bits en una secuencia de vídeo de varias velocidades de bits (adaptable). Cuando se solicita, Media Services entrega la secuencia a los clientes.

Al crear este tipo de objeto LiveEvent, especifique **Básico** (LiveEventEncodingType.Basic).

### <a name="pass-through"></a>Paso a través

![paso a través](./media/live-streaming/pass-through.png)

El paso a través está optimizado para flujos en directo de larga ejecución o para una codificación en directo lineal ininterrumpida con el uso de un codificador en directo local. El codificador local envía contenido **RTMP** o **Smooth Streaming** (MP4 fragmentado) con velocidades de bits múltiples al objeto LiveEvent que está configurado para la entrega de **paso a través**. La entrega de **paso a través** significa que las transmisiones ingeridas pasan a través de objetos **LiveEvent** sin más procesamiento. 

Los objetos LiveEvent de paso a través admiten una resolución de hasta 4K, y el paso a través HEVC cuando se usa con el protocolo de ingesta Smooth Streaming. 

Al crear este tipo de objeto LiveEvent, especifique **None** (LiveEventEncodingType.None).

> [!NOTE]
> El uso de un método de paso a través es la forma más económica de streaming en vivo cuando está realizando varios eventos en un largo período y ya ha invertido en codificadores locales. Consulte los detalles en [Precios de Servicios multimedia](https://azure.microsoft.com/pricing/details/media-services/) .
> 

## <a name="liveevent-types-comparison"></a>Comparación de tipos de objetos LiveEvent 

En la tabla siguiente se comparan las características de dos tipos de objetos LiveEvent.

| Característica | LiveEvent con paso a través | LiveEvent básico |
| --- | --- | --- |
| La entrada de velocidad de bits única se codifica en varias velocidades de bits en la nube |Sin  |Sí |
| Resolución máxima, número de capas |4Kp30  |720p, 6 capas, 30 fps |
| Protocolos de entrada |RTMP, Smooth Streaming |RTMP, Smooth Streaming |
| Precio |Consulte la [página de precios](https://azure.microsoft.com/pricing/details/media-services/) y haga clic en la pestaña "Vídeo en vivo" |Consulte la [página de precios](https://azure.microsoft.com/pricing/details/media-services/) |
| Tiempo de ejecución máximo |24x7 |24x7 |
| Compatibilidad con inserción de tabletas táctiles |Sin  |Sí |
| Compatibilidad con señalización de anuncios mediante API|Sin  |Sí |
| Compatibilidad con señalización de anuncios mediante SCTE-35 en banda|Sí |Sí |
| Títulos CEA 608/708 de paso a través |Sí |Sí |
| Capacidad para la recuperación de breves pausas en la fuente de contribución |Sí |No (LiveEvent comenzará a usar la tableta táctil transcurridos más de 6 segundos sin datos de entrada)|
| Compatibilidad con GOP de entrada no uniformes |Sí |No: la entrada debe ser GOP de 2 s fijos |
| Compatibilidad con la entrada de la velocidad de fotogramas variable |Sí |No: la entrada debe ser una velocidad de fotogramas fija.<br/>Se tolerarán pequeñas variaciones, por ejemplo, durante las escenas con grandes movimientos. Sin embargo, el codificador no puede tener una frecuencia inferior de 10 fotogramas/s. |
| Apagado automático de LiveEvent cuando se pierde la fuente de entrada |Sin  |Después de 12 horas, si no hay ningún objeto LiveEvent en ejecución |

## <a name="liveevent-states"></a>Estados de LiveEvent 

El estado actual de un objeto LiveEvent. Los valores posibles son:

|Estado|DESCRIPCIÓN|
|---|---|
|**Stopped**| Este es el estado inicial del objeto LiveEvent después de su creación (a menos que seleccionara el inicio automático especificado). No se produce ninguna facturación en este estado. En este estado, se pueden actualizar las propiedades del objeto LiveEvent pero no se permite el streaming.|
|**Starting** (iniciándose)| El objeto LiveEvent se está iniciando. No se produce ninguna facturación en este estado. No se permiten actualizaciones ni streaming durante este estado. Si se produce un error, el objeto LiveEvent vuelve al estado Detenido.|
|**Ejecución**| El objeto LiveEvent es capaz de procesar secuencias en directo. Está ahora el uso de facturación. Debe detener el objeto LiveEvent para evitar más facturación.|
|**Stopping** (Deteniéndose)| El objeto LiveEvent se está deteniendo. No se produce facturación en este estado transitorio. No se permiten actualizaciones ni streaming durante este estado.|
|**Eliminando**| El objeto LiveEvent se está eliminando. No se produce facturación en este estado transitorio. No se permiten actualizaciones ni streaming durante este estado.|

## <a name="liveoutput"></a>LiveOutput

Un objeto [LiveOutput](https://docs.microsoft.com/en-us/rest/api/media/liveoutputs) permite controlar la publicación, la grabación y la configuración de la ventana DVR del flujo en vivo. La relación entre LiveEvent y LiveOutput es similar a los medios tradicionales, donde un canal (LiveEvent) tiene un flujo constante de contenido y un programa (LiveOutput) se enfoca en algún evento programado en dicho objeto LiveEvent.
Puede especificar la cantidad de horas que desea conservar el contenido grabado del objeto LiveOutput en la configuración de la propiedad **ArchiveWindowLength**. **ArchiveWindowLength** es una duración de marca de tiempo ISO 8601 de la longitud de la ventana de archivo (grabadora de vídeo digital o DVR). Este valor se puede establecer desde un mínimo de cinco minutos a un máximo de 25 horas. 

**ArchiveWindowLength** también indica el tiempo máximo que los clientes pueden buscar hacia atrás a partir de la posición en vivo actual. Los objetos LiveOutput pueden transmitirse durante la cantidad de tiempo especificada, pero el contenido que escape de esa longitud de ventana se descartará continuamente. El valor de esta propiedad también determina durante cuánto tiempo los manifiestos de cliente pueden crecer.

Cada LiveOutput está asociado con un [recurso](https://docs.microsoft.com/rest/api/media/assets) y registra los datos en un contenedor en la instancia de Azure Storage asociada con la cuenta de Media Services. Para publicar LiveOutput, debe crear un objeto [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators) para el recurso asociado. Contar con este localizador le permitirá crear una dirección URL de streaming que puede proporcionar a sus clientes.

Un objeto LiveEvent es compatible con hasta tres objetos LiveOutput en ejecución simultánea, por lo que puede crear varios archivos de la misma transmisión entrante. Esto le permite publicar y archivar distintas partes de un evento, según sea necesario. Por ejemplo, su necesidad empresarial es difundir una fuente lineal en vivo e ininterrumpida, pero desea crear "grabaciones" de programas durante todo el día para ofrecerlas a los clientes como contenido a petición para que puedan verlas para ponerse al día.  En este escenario, primero cree un objeto LiveOutput principal, con una ventana de archivo corta de una hora o menos, para que los clientes la sintonicen como la secuencia en directo principal. Podría crear un objeto StreamingLocator para este LiveOutput y publicarlo en la aplicación o en el sitio web como la fuente "En vivo".  Mientras la fuente se encuentra en ejecución, puede crear mediante programación un segundo objeto LiveOutput simultáneo al inicio de un programa o 5 minutos antes de proporcionar algunas marcas de recorte más adelante. Se puede detener este segundo objeto LiveOutput cinco minutos después de que finalice el programa o el evento y, a continuación, puede crear un objeto StreamingLocator para publicar este programa como un recurso a petición en el catálogo de la aplicación.  Puede repetir este proceso varias veces para otros límites de programa o resaltar los que desea compartir inmediatamente como a petición, todo esto mientras la fuente "En vivo" del primer objeto LiveOutput continúa con la difusión de la fuente lineal.  Además, puede sacar partido de la compatibilidad del filtro dinámico para recortar el principio y el final del archivo desde el objeto LiveOutput que introdujo para "seguridad de superposición" entre programas y lograr un inicio y fin del contenido más precisos. El contenido archivado también se puede enviar a un objeto [Transform](https://docs.microsoft.com/rest/api/media/transforms) para codificar o encuadrar subclips precisos para que se usen varios formatos de salida como sindicación para otros servicios.

## <a name="streamingendpoint"></a>StreamingEndpoint

Cuando la secuencia fluya al objeto LiveEvent, puede comenzar el evento de streaming mediante la creación de los objetos Asset, LiveOutput y StreamingLocator. Se archivará la secuencia y estará disponible a los usuarios a través del objeto [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/streamingendpoints).

Cuando se crea la cuenta de Media Services, se agrega un punto de conexión de streaming predeterminado a la cuenta en estado Detenido. Para iniciar la transmisión del contenido y aprovechar el empaquetado dinámico y el cifrado dinámico, el punto de conexión de streaming desde el que va a transmitir el contenido debe estar en estado En ejecución.

## <a name="billing"></a>Facturación

Un objeto LiveEvent comienza la facturación tan pronto como su estado realiza la transición a "En ejecución". Para detener la facturación del objeto LiveEvent, debe pararlo.

> [!NOTE]
> Si **LiveEventEncodingType** en el objeto [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) está establecido en Básico, Media Services cerrará automáticamente cualquier objeto LiveEvent que tenga aún el estado "En ejecución" doce horas después de que se pierda la fuente de entrada y si no hay ningún objeto LiveOutput en ejecución. Sin embargo, se le facturará por el tiempo que el objeto LiveEvent haya estado en estado "En ejecución".
>

En la tabla siguiente se muestra cómo los estados de LiveEvent se asocian al modo de facturación.

| Estado de LiveEvent | ¿Es la facturación? |
| --- | --- |
| Iniciando |No (estado transitorio) |
| En ejecución |SÍ |
| Deteniéndose |No (estado transitorio) |
| Stopped |Sin  |

## <a name="next-steps"></a>Pasos siguientes

[Tutorial de Live Streaming](stream-live-tutorial-with-api.md)
