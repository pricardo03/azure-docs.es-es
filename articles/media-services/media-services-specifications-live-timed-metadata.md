---
title: 'Azure Media Services: Señalización de metadatos con tiempo en streaming en vivo | Microsoft Docs'
description: En esta especificación se describen los métodos de señalización de los metadatos con tiempo cuando se llevan a cabo la ingesta y el streaming en Azure Media Services. Esto incluye compatibilidad con las señales genéricas de los metadatos con tiempo (ID3), así como con la señalización SCTE-35 para una inserción de anuncios y la señalización de condiciones de inserción.
services: media-services
documentationcenter: ''
author: johndeu
manager: femila
editor: johndeu
ms.assetid: 265b94b1-0fb8-493a-90ec-a4244f51ce85
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/22/2019
ms.author: johndeu
ms.openlocfilehash: df2a86dd1292f58511765e842ee97daddcff4e3e
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70102933"
---
# <a name="signaling-timed-metadata-in-live-streaming"></a>Señalización de metadatos con tiempo en streaming en vivo 

Última actualización: 22 de agosto de 2019

### <a name="conformance-notation"></a>Notación de cumplimiento

Las palabras clave "DEBE", "NO DEBE", "SE REQUIERE", "TENDRÁ QUE", "NO DEBERÁ", "DEBERÍA", "NO DEBERÍA", "RECOMENDADO", "PUEDE" y "OPCIONAL" de este documento se deben interpretar como se describe en RFC 2119

## <a name="1-introduction"></a>1. Introducción 

Para señalizar la inserción de anuncios o eventos de metadatos personalizados en un reproductor de cliente, los medios de difusión a menudo usan metadatos con tiempo insertados en el vídeo. Para habilitar estos escenarios, Media Services proporciona compatibilidad con el transporte de metadatos desde el punto de ingesta del canal de streaming en vivo a la aplicación cliente.
Esta especificación describe distintos modos que admite Media Services para los metadatos con tiempo en señales de streaming en vivo.

1. La señalización [SCTE-35] que cumple con los estándares descritos por [SCTE-35], [SCTE-214-1], [SCTE-214-3] y [RFC8216].

2. La señalización [SCTE-35] que cumple con la especificación heredada [Adobe-Primetime] para la señalización de anuncios de RTMP.
   
3. Un modo de señalización genérico de los metadatos con tiempo para los mensajes que **NO** son de tipo [SCTE-35] y podrían incluir [ID3v2] u otros esquemas personalizados definidos por el desarrollador de la aplicación.

## <a name="11-terms-used"></a>1.1 Términos empleados

| Término              | Definición |
|-------------------|------------|
| Pausa publicitaria          | Ubicación o momento dado en el que se puede programar la entrega de uno o más anuncios; es lo mismo que la disponibilidad y la oportunidad de ubicación. |
| Servicio de decisión publicitaria| Servicio externo que decide qué anuncios y duraciones se mostrarán al usuario. Normalmente, los servicios se proporcionan a través de un asociado y están fuera del ámbito de este documento.|
| Pila               | Indicación de tiempo y parámetros de la próxima pausa de anuncios. Tenga en cuenta que las pilas pueden indicar un cambio pendiente en una pausa publicitaria, en el siguiente anuncio de una pausa publicitaria y en una pausa publicitaria del contenido principal. |
| Empaquetador          | El "punto de conexión de streaming" de Azure Media Services proporciona funcionalidades de empaquetado dinámico para DASH y HLS y se conoce como "empaquetador" en el sector multimedia. 
| Tiempo de presentación | Hora en que se presenta un evento en un visor. La hora representa el momento en la escala de tiempo de los elementos multimedia en que un visor verá el evento. Por ejemplo, el tiempo de presentación de un mensaje de comando SCTE-35 splice_info() es splice_time(). |
| Hora de llegada      | La hora a la que llega un mensaje de evento. Este tiempo normalmente es distinto del tiempo de presentación del evento, ya que los mensajes de eventos se envían antes del tiempo de presentación del evento.                                     |
| Pista dispersa      | pista de elementos multimedia que no es continua y que está sincronizada con una pista principal o de control.                                                                                                                                    |
| Origen            | El servicio Azure Media Streaming                                                                                                                                                                                                |
| Receptor del canal      | El servicio Azure Media Live Streaming Service                                                                                                                                                                                           |
| HLS               | Protocolo Apple HTTP Live Streaming                                                                                                                                                                                               |
| DASH              | Streaming adaptable dinámico a través de HTTP                                                                                                                                                                                             |
| Smooth            | Protocolo de Smooth Streaming                                                                                                                                                                                                        |
| MPEG2-TS          | Secuencias de transporte MPEG-2                                                                                                                                                                                                         |
| RTMP              | Protocolo multimedia en tiempo real                                                                                                                                                                                                    |
| uimsbf            | Entero sin signo, el bit más significativo aparece en primer lugar.                                                                                                                                                                                    |

---

## <a name="12-normative-references"></a>1.2 Referencias de la normativa

Los siguientes documentos contienen disposiciones, que, mediante la referencia de este texto, constituyen disposiciones de este documento. Todos los documentos están sujetos a revisión por parte de organismos de normalización, y animamos a los lectores que investiguen la posibilidad de aplicar las ediciones más recientes de los documentos que se enumeran a continuación. También queremos recordar a los lectores que las ediciones más recientes de los documentos a los que se hace referencia podrían no ser compatibles con esta versión de la especificación de metadatos con tiempo para Azure Media Services.


|Estándar  |Definición  |
|---------|---------|
|[Adobe-Primetime] | [Especificación de señalización de inserción del programa digital PrimeTime 1.2](https://www.adobe.com/content/dam/acom/en/devnet/primetime/PrimetimeDigitalProgramInsertionSignalingSpecification.pdf) |
|[Adobe-Flash-AS] | [Referencia del lenguaje ActionScript de FLASH](https://help.adobe.com/archive/en_US/as2/flashlite_2.x_3.x_aslr.pdf) |
| [AMF0]            | ["Formato del mensaje de acción AMF0"](https://download.macromedia.com/pub/labs/amf/amf0_spec_121207.pdf) |
| [DASH-IF-IOP]     | Guía de interoperabilidad del foro del sector de DASH versión 4.2: [https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html](https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html) |
| [HLS-TMD]         | Metadatos con tiempo para HTTP Live Streaming: [https://developer.apple.com/streaming](https://developer.apple.com/streaming) |
| [CMAF-ID3]         | [Metadatos con tiempo en el formato de aplicación multimedia común (CMAF)](https://aomediacodec.github.io/av1-id3/)
| [ID3v2]           | Versión de etiqueta ID3 2.4.0: [http://id3.org/id3v2.4.0-structure](http://id3.org/id3v2.4.0-structure) |
| [ISO-14496-12]    | ISO/IEC 14496-12; parte 12: formato de archivo multimedia base ISO, cuarta edición, 15/07/2012  |
| [MPEGDASH]        | Tecnologías de la información; streaming adaptable dinámico a través de HTTP (DASH); parte 1: descripción de presentación de elementos multimedia y formatos de segmento. Mayo de 2014. Publicado. Dirección URL: https://www.iso.org/standard/65274.html |
| [MPEGCMAF]        | Tecnologías de la información; formato de la aplicación multimedia (MPEG-A); parte 19: Formato de aplicación multimedia común (CMAF) para los elementos multimedia segmentados. Enero de 2018. Publicado. Dirección URL: https://www.iso.org/standard/71975.html |
| [MPEGCENC]        | Tecnologías de la información; tecnologías de sistemas MPEG; parte 7: cifrado común en archivos del formato de archivo multimedia base ISO. Febrero de 2016. Publicado. Dirección URL: https://www.iso.org/standard/68042.html |
| [MS-SSTR]         | ["Protocolo Microsoft Smooth Streaming", 15 de mayo de 2014](https://docs.microsoft.com/openspecs/windows_protocols/ms-sstr/8383f27f-7efe-4c60-832a-387274457251) |
| [MS-SSTR-Ingest]  | [Especificación de la introducción en directo de MP4 fragmentado de Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-fmp4-live-ingest-overview) |
| [RFC8216]         | R. Pantos, Ed.; W. May. HTTP Live Streaming. Agosto de 2017. Informativo. [https://tools.ietf.org/html/rfc8216](https://tools.ietf.org/html/rfc8216) |
| [RFC4648]         |Codificaciones de datos Base 16, base 32 y Base 64: [https://tools.ietf.org/html/rfc4648](https://tools.ietf.org/html/rfc4648) |
| [RTMP]            |["Protocolo de mensajería en tiempo real de Adobe", 21 de diciembre de 2012](https://www.adobe.com/devnet/rtmp.html)  |
| [SCTE-35-2019]    | SCTE 35: 2019; mensaje de pila de inserción del programa digital para cable: https://www.scte.org/SCTEDocs/Standards/ANSI_SCTE%2035%202019r1.pdf  |
| [SCTE-214-1]      | SCTE 214-1 2016; MPEG-DASH para servicios de cable basados en IP, parte 1: restricciones y extensiones de MPD |
| [SCTE-214-3]      | SCTE 214-3 2015: MPEG-DASH para servicios de cable basados en IP, parte 3: perfil DASH/FF |
| [SCTE-224]        | SCTE 224 2018r1: interfaz de programación y notificación de eventos |
| [SCTE-250]        | API de administración de señalización y eventos (ESAM) |

---


## <a name="2-timed-metadata-ingest"></a>2. Ingesta de metadatos con tiempo

Azure Media Services admite metadatos en banda en tiempo real para los protocolos [RTMP] y Smooth Streaming [MS-SSTR-Ingest]. Los metadatos en tiempo real se pueden usar para definir eventos personalizados, con sus propios esquemas personalizados únicos (JSON, binario, XML) así como otros formatos definidos por el sector como ID3 o SCTE-35, para la señalización de anuncios en un flujo de difusión. 

En este artículo se proporcionan los detalles sobre cómo enviar señales personalizadas de metadatos con tiempo mediante los protocolos de ingesta admitidos por Azure Media Services. En este artículo también se explica cómo los manifiestos de HLS, DASH y Smooth Streaming se decoran con las señales de metadatos con tiempo, y cómo se transfieren en banda cuando el contenido se entrega mediante CMAF (fragmentos de MP4) o segmentos de secuencia de transporte (TS) para HLS. 

Entre los usos más habituales de los metadatos con tiempo se incluyen:

 - Señales de anuncios SCTE-35 para desencadenar pausas comerciales durante un evento en directo o una difusión lineal.
 - Metadatos de ID3 personalizados que pueden desencadenar eventos en una aplicación cliente (explorador, iOS o Android).
 - Metadatos JSON, binarios o XML con definición personalizada para desencadenar eventos en una aplicación cliente.
 - Datos de telemetría de un codificador en directo, cámara IP o dron
 - Eventos de una cámara IP como movimiento, detección de caras, etc.
 - Información de posición geográfica procedente de una cámara deportiva, dron o dispositivo en movimiento
 - Letras de canciones
 - Límites de programa en una fuente lineal de contenidos en directo
 - Imágenes o metadatos ampliados que se mostrarán en una fuente de contenidos en directo.
 - Marcadores deportivos o información de relojes de partidos
 - Paquetes de publicidad interactiva que se mostrarán junto con el vídeo en el explorador.
 - Cuestionarios o encuestas.
  
Los eventos en directo y el empaquetador de Azure Media Services tienen la capacidad de recibir estas señales de metadatos con tiempo y convertirlas en un flujo de metadatos que puede llegar a las aplicaciones cliente mediante protocolos basados en estándares como HLS y DASH.


## <a name="21-rtmp-timed-metadata"></a>2.1 Metadatos con tiempo de RTMP

El protocolo [RTMP] permite el envío de las señales de los metadatos con tiempo en distintos escenarios, incluidos los metadatos personalizados y las señales de anuncios SCTE-35. 

Las señales de anuncios (mensajes de indicación) se envían como mensajes de indicación [AMF0] insertados en el flujo [RTMP]. Los mensajes de pila se pueden enviar algo antes de que se deba producir el evento real o la señal de inserción del anuncio [SCTE35]. Para que este escenario sea viable, la marca de tiempo real de la presentación se envía en el mensaje de indicación. Para más información, consulte [AMF0].

Los siguientes comandos de [AMF0] son compatibles con Azure Media Services para la ingesta de RTMP:

- **onUserDataEvent**: se usa para metadatos personalizados o metadatos con tiempo [ID3v2]
- **onAdCue**: se usa principalmente para indicar una oportunidad de colocación de anuncios en un streaming en vivo. Se admiten dos formas de indicación, un modo simple y un modo "SCTE-35". 
- **onCuePoint**: compatible con determinados codificadores de hardware locales, como el codificador Elemental Live, para señalizar mensajes [SCTE35]. 
  

En las tablas siguientes se describe el formato de la carga del mensaje AMF que Media Services ingerirá para los modos de mensaje "simple" y [SCTE35].

El nombre del mensaje [AMF0] se puede utilizar para diferenciar varias secuencias de eventos del mismo tipo.  En el caso de los mensajes [SCTE-35] y el modo "simple", el nombre del mensaje de AMF DEBE ser "onAdCue", según sea necesario en la especificación [Adobe-Primetime].  Azure Media Services DEBE omitir los campos que no se enumeran a continuación en la ingesta.

## <a name="211-rtmp-with-custom-metadata-using-onuserdataevent"></a>2.1.1 RTMP con metadatos personalizados mediante "onUserDataEvent"

Si desea proporcionar fuentes de metadatos personalizados desde el codificador de nivel superior, la cámara IP, el dron o el dispositivo que use el protocolo RTMP, utilice el tipo de comando de mensaje de datos [AMF0] "onUserDataEvent".

El comando **"onUserDataEvent"** de mensaje de datos DEBE llevar una carga de mensaje con la siguiente definición para que Media Services la capture y empaquete en un formato de archivo en banda junto con los manifiestos de HLS, DASH y Smooth Streaming.
Es recomendable enviar mensajes de metadatos con tiempo con una frecuencia inferior a 0,5 segundos (500 ms) porque podrían producirse problemas de estabilidad con el streaming en vivo. Cada mensaje podría agregar metadatos de varios fotogramas si es necesario proporcionar metadatos en el nivel de fotogramas. Si va a enviar flujos de múltiples velocidades de bits, se recomienda que proporcione también los metadatos a una velocidad de bits única para reducir el ancho de banda y evitar las interferencias con el procesamiento de vídeo y audio. 

La carga de **"onUserDataEvent"** debe ser un mensaje con formato XML de EventStream de [MPEGDASH]. Esto facilita el paso de esquemas personalizados y definidos que se pueden transmitir en cargas de "emsg" en banda para el contenido de CMAF [MPEGCMAF] que se entrega mediante los protocolos HLS o DASH. Cada mensaje de EventStream de DASH contiene un identificador schemeIdUri que funciona como un identificador de esquema de mensajes URN y que define la carga del mensaje. Algunos esquemas como "https://aomedia.org/emsg/ID3" para [ID3v2] o **urn:scte:scte35:2013:bin** para [SCTE-35] están normalizados por los consorcios del sector para mejorar la interoperabilidad. Cualquier proveedor de aplicaciones puede definir su propio esquema personalizado mediante una dirección URL bajo su control (dominio en propiedad) y puede proporcionar una especificación en esa dirección URL si así lo decide. Si un reproductor tiene un controlador para el esquema definido, ese es el único componente que debe entender la carga y el protocolo.

El esquema de la carga XML de EventStream de [MPEG-DASH] se define como (extracto de DASH ISO-IEC-23009-1-3rd Edition). Tenga en cuenta que en este momento solo se admite un "EventType" por "EventStream". Solo se procesará el primer elemento **Event** si se proporcionan varios eventos en **EventStream**.

```xml
  <!-- Event Stream -->
  <xs:complexType name="EventStreamType">
    <xs:sequence>
      <xs:element name="Event" type="EventType" minOccurs="0" maxOccurs="unbounded"/>
      <xs:any namespace="##other" processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute ref="xlink:href"/>
    <xs:attribute ref="xlink:actuate" default="onRequest"/>
    <xs:attribute name="schemeIdUri" type="xs:anyURI" use="required"/>
    <xs:attribute name="value" type="xs:string"/>
    <xs:attribute name="timescale" type="xs:unsignedInt"/>
  </xs:complexType>
  <!-- Event  -->
  <xs:complexType name="EventType">
    <xs:sequence>
      <xs:any namespace="##other" processContents="lax" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="presentationTime" type="xs:unsignedLong" default="0"/>
    <xs:attribute name="duration" type="xs:unsignedLong"/>
    <xs:attribute name="id" type="xs:unsignedInt"/>
    <xs:attribute name="contentEncoding" type="ContentEncodingType"/>
    <xs:attribute name="messageData" type="xs:string"/>
    <xs:anyAttribute namespace="##other" processContents="lax"/>
  </xs:complexType>
```


### <a name="example-xml-event-stream-with-id3-schema-id-and-base64-encoded-data-payload"></a>Flujo de eventos XML de ejemplo con el identificador de esquema ID3 y carga de datos con codificación Base64  
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <EventStream schemeIdUri="https://aomedia.org/emsg/ID3">
         <Event contentEncoding="Base64">
          -- base64 encoded ID3v2 full payload here per [CMAF-TMD] --
         </Event>
   <EventStream>
```

### <a name="example-event-stream-with-custom-schema-id-and-base64-encoded-binary-data"></a>Flujo de eventos de ejemplo con el identificador de esquema personalizado y datos binarios con codificación Base64  
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <EventStream schemeIdUri="urn:example.org:custom:binary">
         <Event contentEncoding="Base64">
          -- base64 encoded custom binary data message --
         </Event>
   <EventStream>
```

### <a name="example-event-stream-with-custom-schema-id-and-custom-json"></a>Ejemplo de flujo de eventos con identificador de esquema y JSON personalizados  
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <EventStream schemeIdUri="urn:example.org:custom:JSON">
         <Event>
          [
            {"key1" : "value1"},
            {"key2" : "value2"}
          ]
         </Event>
   <EventStream>
```

### <a name="built-in-supported-scheme-id-uris"></a>Identificadores URI admitidos e integrados de identificador de esquema
| URI de identificador de esquema                 |  DESCRIPCIÓN                                             |
|-------------------------------|----------------------------------------------------------|
| https:\//aomedia.org/emsg/ID3   | Describe cómo se pueden transferir los metadatos [ID3v2] como metadatos con tiempo en un MP4 fragmentado compatible con CMAF [MPEGCMAF]. Para más información, consulte [Metadatos con tiempo en el formato de aplicación multimedia común (CMAF)](https://aomediacodec.github.io/av1-id3/). |

### <a name="event-processing-and-manifest-signaling"></a>Procesamiento de eventos y señalización del manifiesto

Al recibir un evento **"onUserDataEvent"** válido, Azure Media Services buscará una carga XML válida que se corresponde con el tipo de EventStreamType (definido en [MPEGDASH]), analizará esta carga y la convertirá en un cuadro versión 1 de "emsg" de un fragmento de MP4 [MPEGCMAF] para el almacenamiento en el archivo y en la transmisión en directo al empaquetador de Media Services.   El empaquetador detectará el cuadro de "emsg" en el streaming en vivo y realizará una de las siguientes acciones:

- (a) Lo "empaquetará dinámicamente" en segmentos de TS para su entrega a clientes de HLS conforme a la especificación para metadatos con tiempo de HLS [HLS-TMD].
- (b) Lo pasará para su entrega en fragmentos de CMAF a través de HLS o DASH. 
- (c) Lo convertirá en una señal de pista dispersa para su entrega a través de Smooth Streaming [MS-SSTR].

Además del CMAF en formato "emsg" en banda o los paquetes TS PES para HLS, los manifiestos para DASH (MPD) y Smooth Streaming contendrán una referencia a los flujos de eventos en banda (también conocidos como pista de flujos dispersa en Smooth Streaming). 

Los eventos individuales o sus cargas de datos NO se muestran directamente en la salida de los manifiestos de HLS, DASH o Smooth. 

### <a name="additional-informational-constraints-and-defaults-for-onuserdataevent-events"></a>Restricciones informativas adicionales y valores predeterminados para eventos de onUserDataEvent

- Si la escala temporal no está establecida en el elemento de EventStream, se usará de forma predeterminada la escala temporal de RTMP 1 kHz.
- La entrega de un mensaje de onUserDataEvent se limita a una vez cada 500 ms como máximo. Si envía eventos con una frecuencia mayor, esto puede afectar al ancho de banda y a la estabilidad de la fuente de contenidos en directo.

## <a name="212-rtmp-ad-cue-signaling-with-oncuepoint"></a>2.1.2 Señalización de indicación de anuncios de RTMP con "onCuePoint"

Azure Media Services puede escuchar varios tipos de mensajes [AMF0], así como responder a ellos, que se pueden usar para señalizar varios metadatos sincronizados en tiempo real en el streaming en vivo.  La especificación [Adobe-PrimeTime] define dos tipos de pilas denominados "simple" y "SCTE-35". En el modo "simple", Media Services admite un único mensaje de pila de AMF denominado "onAdCue" con una carga que coincide con la tabla siguiente definida para la señal "modo simple".  

En la sección siguiente se muestra la carga del "modo simple" de RTMP, que se puede usar para establecer una señal de anuncio básica "spliceOut" que se llevará a cabo en el manifiesto de cliente para HLS, DASH y Microsoft Smooth Streaming. Esto resulta muy útil en los escenarios en los que el cliente no tiene ningún sistema de inserción o ninguna implementación de señalización de anuncio basada en SCTE-35, y usa un codificador local básico para enviar el mensaje de pila a través de una API. Normalmente, el codificador local admitirá una API basada en REST para desencadenar esta señal, que también "insertará la condición" de la secuencia de vídeo mediante la inserción de un fotograma de IDR en el vídeo y el inicio de un nuevo GOP.

## <a name="213--rtmp-ad-cue-signaling-with-oncuepoint---simple-mode"></a>2.1.3 Señalización de indicación de anuncios de RTMP con "onCuePoint": modo sencillo

| Nombre del campo | Tipo de campo | ¿Necesario? | Descripciones                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| cue        | Cadena     | Obligatorio | El mensaje de evento.  Debe ser "SpliceOut" para designar una inserción de modo sencillo.                                              |
| id         | Cadena     | Obligatorio | Un identificador único que describe la inserción o el segmento. Identifica la instancia del mensaje                            |
| duration   | Number     | Obligatorio | Duración de la inserción. Las unidades son fracciones de segundos.                                                                |
| elapsed    | Number     | Opcional | Cuando se repite la señal para que sea compatible con la sintonización, este campo indicará la cantidad de tiempo de la presentación que ha transcurrido desde que comenzó la inserción. Las unidades son fracciones de segundos. Cuando se usa el modo simple, este valor no debe superar la duración original de la inserción.                                                  |
| time       | Number     | Obligatorio | Debe ser el tiempo de la inserción, en el tiempo de presentación. Las unidades son fracciones de segundos.                                     |

---
 
## <a name="214-rtmp-ad-cue-signaling-with-oncuepoint---scte-35-mode"></a>2.1.4 Señalización de indicación de anuncios de RTMP con "onCuePoint": modo SCTE-35

Cuando trabaje con un flujo de trabajo de producción de difusión más avanzado que requiera que el mensaje de carga SCTE-35 completo se transmita al manifiesto HLS o DASH, lo mejor es usar el modo "SCTE-35" de la especificación [Adobe-PrimeTime].  Este modo admite señales SCTE-35 en banda que se envían directamente a un codificador en directo local, que codifica las señales en el flujo de RTMP mediante el "modo SCTE-35" determinado en la especificación [Adobe-PrimeTime]. 

Normalmente, los mensajes SCTE-35 solo pueden aparecer en entradas de secuencia de transporte MPEG-2 (TS) en un codificador local. Póngase en contacto con el fabricante del codificador para obtener más información sobre cómo configurar una ingesta de secuencia de transporte que contenga SCTE-35 y habilitarla para el paso a través de RTMP en el modo SCTE-35 de Adobe.

En este escenario, se DEBE enviar la siguiente carga desde el codificador local mediante el tipo de mensaje **"onAdCue"** [AMF0].

| Nombre del campo | Tipo de campo | ¿Necesario? | Descripciones                                                                                                             |
|------------|------------|----------|--------------------------------------------------------------------------------------------------------------------------|
| cue        | Cadena     | Obligatorio | El mensaje de evento.  En el caso de los mensajes [SCTE-35], DEBE ser la sección binaria splice_info_section() [RFC4648] con codificación Base 64 para que los mensajes se envíen a los clientes HLS, Smooth y Dash.                                              |
| Tipo       | Cadena     | Obligatorio | Un URN o una dirección URL que identifiquen el esquema del mensaje. En el caso de los mensajes [SCTE-35], **DEBE** ser **"scte35"** para que los mensajes se envíen a los clientes HLS, Smooth y Dash conforme a [Adobe-Primetime]. Opcionalmente, el URN "urn:scte:scte35:2013:bin" también se puede usar para señalizar un mensaje [SCTE-35]. |
| id         | Cadena     | Obligatorio | Un identificador único que describe la inserción o el segmento. Identifica la instancia del mensaje.  Los mensajes con una semántica equivalente deben tener el mismo valor.|
| duration   | Number     | Obligatorio | La duración del evento o segmento de ad-splice, si se conoce. Si no se conoce, el valor **DEBERÍA** ser 0.                                                                 |
| elapsed    | Number     | Opcional | Cuando se repite la señal de anuncio de [SCTE-35] para realizar la sintonización, este campo indicará la cantidad de tiempo de la presentación que ha transcurrido desde que comenzó la inserción. Las unidades son fracciones de segundos. En el modo [SCTE-35], este valor puede superar la duración original especificada de la inserción o segmentación.                                                  |
| time       | Number     | Obligatorio | El tiempo de presentación del evento o inserción con ad-splice.  El tiempo de presentación y la duración se **DEBEN** alinear con los puntos de acceso de secuencias (SAP) del tipo 1 o 2, como se definió en [ISO-14496-12] en el anexo I. En el caso de la salida de HLS, el tiempo y la duración se **DEBEN** alinear con los límites de los segmentos. El tiempo de presentación y la duración de los diferentes mensajes de eventos del mismo flujo de eventos NO DEBE solaparse. Las unidades son fracciones de segundos.

---
## <a name="215-rtmp-ad-signaling-with-oncuepoint-for-elemental-live"></a>2.1.5 Señalización de anuncios de RTMP con "onCuePoint" para Elemental Live

El codificador local de Elemental Live es compatible con los marcadores de anuncios de la señal de RTMP. Actualmente, Azure Media Services solo admite el tipo de marcador de anuncio "onCuePoint "para RTMP.  Puede habilitarse en la configuración del grupo de RTMP de Adobe en la configuración del codificador Elemental MediaLive o en la API estableciendo el valor de "**ad_markers**" en "onCuePoint".  Consulte la documentación sobre Elemental Live para ver información detallada. La habilitación de esta característica en el grupo de RTMP pasará las señales SCTE-35 a las salidas de RTMP de Adobe para que las procese Azure Media Services.

El tipo de mensaje "onCuePoint" se define en [Adobe-Flash-AS] y tiene la siguiente estructura de carga cuando se envía desde la salida de RTMP de Elemental Live.


| Propiedad  |DESCRIPCIÓN  |
|---------|---------|
|  Nombre     | El nombre DEBE ser "**scte35**" y estar definido por Elemental Live. |
|time     |  Tiempo en segundos en el que se produjo el punto de pila en el archivo de vídeo durante la escala de tiempo |
| Tipo     | El tipo de punto de pila debe establecerse en "**evento**". |
| parameters | Una matriz asociativa de cadenas de pares de nombre y valor que contiene la información del mensaje SCTE-35, incluidos el identificador y la duración. Estos valores se analizan mediante Azure Media Services y se incluyen en la etiqueta de decoración del manifiesto.  |


Cuando se utiliza este modo del marcador de anuncio, la salida del manifiesto HLS es similar al modo "simple" de Adobe. 

### <a name="216-cancellation-and-updates"></a>2.1.6 Cancelación y actualizaciones

Los mensajes se pueden cancelar o actualizar mediante el envío de varios mensajes con el mismo tiempo de presentación y el mismo identificador. El tiempo de presentación y el identificador identifican de forma exclusiva el evento. El último mensaje recibido durante un tiempo de presentación específico que cumple las restricciones de cuña anterior es el mensaje sobre el que se actúa. El evento actualizado reemplaza a todos los mensajes recibidos con anterioridad. La restricción de cuña anterior es de cuatro segundos. Se actuará sobre aquellos mensajes recibidos al menos cuatro segundos antes del tiempo de presentación.

## <a name="22-fragmented-mp4-ingest-smooth-streaming"></a>2.2 Ingesta de MP4 fragmentado (Smooth Streaming)

Consulte [MS-SSTR-Ingest] para conocer los requisitos sobre la ingesta de secuencias en vivo. Las secciones siguientes proporcionan detalles sobre la ingesta de metadatos de presentación con tiempo.  Los metadatos de presentación con tiempo se ingieren como una pista dispersa, lo cual se define en el cuadro Live Server Manifest Box (consulte MS-SSTR) y en Movie Box (‘moov’).  

Cada fragmento disperso consta de un cuadro Movie Fragment Box (‘moof’) y un cuadro Media Data Box (‘mdat’), en el que el cuadro ‘mdat’ es el mensaje binario.

Con el fin de lograr la inserción exacta de fotogramas de los anuncios, el codificador DEBE dividir el fragmento en el momento de la presentación en el que es necesario insertar la pila.  DEBE crearse un nuevo fragmento que comience con un marco de IDR recién creado, o con puntos de acceso de secuencia (SAP) del tipo 1 o 2, tal como se define en el anexo I de [ISO-14496-12]. Esto permite que Azure Media Packager genere correctamente un manifiesto HLS y un manifiesto de varios períodos de DASH en el que el nuevo período comience en el momento de la presentación condicionado por la inserción precisa de fotogramas.

### <a name="221-live-server-manifest-box"></a>2.2.1 Cuadro Live Server Manifest Box

La pista dispersa se **DEBE** declarar en el cuadro Live Server Manifest Box con una entrada **\<textstream\>** y **DEBE** tener el siguiente conjunto de atributos:

| **Nombre del atributo** | **Tipo de campo** | **¿Necesario?** | **Descripción**                                                                                                                                                                                                                                                 |
|--------------------|----------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| systemBitrate      | Number         | Obligatorio      | **DEBE** ser "0", lo cual indica una pista con una velocidad de bits desconocida y variable.                                                                                                                                                                                                 |
| parentTrackName    | Cadena         | Obligatorio      | **DEBE** ser el nombre de la pista principal, con la que se alinean en la escala de tiempo los códigos de tiempo de las pistas dispersas. La pista principal no puede ser una pista dispersa.                                                                                                                    |
| manifestOutput     | Boolean        | Obligatorio      | **DEBE** ser "true" para indicar que la pista dispersa se insertará en el manifiesto de cliente de Smooth.                                                                                                                                                               |
| Subtype            | Cadena         | Obligatorio      | **DEBE** ser el código de cuatro caracteres "DATA".                                                                                                                                                                                                                        |
| Esquema             | Cadena         | Obligatorio      | **DEBE** ser un URN o una dirección URL que identifiquen el esquema del mensaje. En el caso de los mensajes [SCTE-35], este **DEBE** ser "urn:scte:scte35:2013:bin", para que los mensajes se envíen a los clientes HLS, Smooth y Dash conforme a [SCTE-35]. |
| trackName          | Cadena         | Obligatorio      | **DEBE** ser el nombre de la pista dispersa. trackName se puede utilizar para diferenciar varios flujos de eventos con el mismo esquema. Cada flujo de eventos único **DEBE** tener un nombre de pista único.                                                                           |
| timescale          | Number         | Opcional      | **DEBE** ser la escala de tiempo de la pista principal.                                                                                                                                                                                                                      |

---

### <a name="222-movie-box"></a>2.2.2 Cuadro Movie Box

El cuadro Movie Box (‘moov’) sigue al cuadro Live Server Manifest Box como parte del encabezado de flujo de una pista dispersa.

El cuadro "moov" **DEBE** contener un cuadro **TrackHeaderBox ("tkhd")** tal y como se define en [ISO-14496-12] con las siguientes restricciones:

| **Nombre del campo** | **Tipo de campo**          | **¿Necesario?** | **Descripción**                                                                                                |
|----------------|-------------------------|---------------|----------------------------------------------------------------------------------------------------------------|
| duration       | Entero sin signo de 64 bits | Obligatorio      | **DEBE** ser 0, ya que el cuadro de la pista tiene cero ejemplos y la duración total de los ejemplos del cuadro de la pista es 0. |

---

El cuadro "moov" **DEBE** contener un cuadro **HandlerBox ("hdlr")** tal y como se define en [ISO-14496-12] con las siguientes restricciones:

| **Nombre del campo** | **Tipo de campo**          | **¿Necesario?** | **Descripción**   |
|----------------|-------------------------|---------------|-------------------|
| handler_type   | Entero sin signo de 32 bits | Obligatorio      | **DEBE** ser "meta". |

---

El cuadro "stsd" **DEBE** contener un cuadro MetaDataSampleEntry con un nombre de codificación que se define en [ISO-14496-12].  Por ejemplo, para los mensajes de SCTE-35 el nombre de codificación **DEBE** ser "scte".

### <a name="223-movie-fragment-box-and-media-data-box"></a>2.2.3 Cuadros Movie Fragment Box y Media Data Box

Los fragmentos de pistas dispersas constan de un cuadro Movie Fragment Box (‘moof’) y uno Media Data Box (‘mdat’).

> [!NOTE]
> Con el fin de lograr la inserción exacta de fotogramas de los anuncios, el codificador DEBE dividir el fragmento en el momento de la presentación en el que es necesario insertar la pila.  DEBE crearse un nuevo fragmento que comience con un marco de IDR recién creado, o con puntos de acceso de secuencia (SAP) de tipo 1 o 2, tal como se define en el anexo I de la [ISO-14496-12].
> 

El cuadro MovieFragmentBox ("moof") **DEBE** contener un cuadro **TrackFragmentExtendedHeaderBox ("uuid")** , tal y como se define en [MS-SSTR] con los siguientes campos:

| **Nombre del campo**         | **Tipo de campo**          | **¿Necesario?** | **Descripción**                                                                               |
|------------------------|-------------------------|---------------|-----------------------------------------------------------------------------------------------|
| fragment_absolute_time | Entero sin signo de 64 bits | Obligatorio      | **DEBE** ser la hora de llegada del evento. Este valor alinea el mensaje con la pista principal.   |
| fragment_duration      | Entero sin signo de 64 bits | Obligatorio      | **DEBE** ser la duración del evento. La duración puede ser cero para indicar que es desconocida. |

---


El cuadro MediaDataBox ("mdat") **DEBE** tener el formato siguiente:

| **Nombre del campo**          | **Tipo de campo**                   | **¿Necesario?** | **Descripción**                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|-------------------------|----------------------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| version                 | Entero sin signo de 32 bits (uimsbf) | Obligatorio      | Determina el formato del contenido del cuadro 'mdat'. Se ignorarán las versiones no reconocidas. Actualmente, la única versión admitida es: 1.                                                                                                                                                                                                                                                                                                                                                      |
| id                      | Entero sin signo de 32 bits (uimsbf) | Obligatorio      | Identifica la instancia del mensaje. Los mensajes con una semántica equivalente deben tener el mismo valor, es decir, será suficiente con el procesamiento de cualquier cuadro de mensaje de evento con el mismo identificador.                                                                                                                                                                                                                                                                                                            |
| presentation_time_delta | Entero sin signo de 32 bits (uimsbf) | Obligatorio      | La suma del valor de fragment_absolute_time, especificado en TrackFragmentExtendedHeaderBox, y el de presentation_time_delta **DEBE** ser el tiempo de presentación del evento. El tiempo de presentación y la duración se **DEBEN** alinear con los puntos de acceso de secuencias (SAP) del tipo 1 o 2, como se definió en [ISO-14496-12] en el anexo I. En el caso de la salida de HLS, el tiempo y la duración se **DEBEN** alinear con los límites de los segmentos. El tiempo de presentación y la duración de los diferentes mensajes de eventos de la misma secuencia de eventos **NO DEBE** superponerse. |
| message                 | byte array                       | Obligatorio      | El mensaje de evento. En el caso de los mensajes [SCTE-35], el mensaje es la sección binaria splice_info_section(). En el caso de los mensajes [SCTE-35], este **DEBE** ser la sección splice_info_section() para que los mensajes se envíen a los clientes HLS, Smooth y Dash conforme a [SCTE-35]. En el caso de los mensajes [SCTE-35], la sección binaria splice_info_section() es la carga del cuadro "mdat" y **NO** está codificada en Base 64.                                                            |

---


### <a name="224-cancellation-and-updates"></a>2.2.4 Cancelación y actualizaciones

Los mensajes se pueden cancelar o actualizar mediante el envío de varios mensajes con el mismo tiempo de presentación y el mismo identificador.  El tiempo de presentación y el identificador identifican de forma exclusiva el evento. El último mensaje recibido durante un tiempo de presentación específico que cumple las restricciones de cuña anterior es el mensaje sobre el que se actúa. El mensaje actualizado reemplaza a todos los mensajes recibidos con anterioridad.  La restricción de cuña anterior es de cuatro segundos. Se actuará sobre aquellos mensajes recibidos al menos cuatro segundos antes del tiempo de presentación. 


## <a name="3-timed-metadata-delivery"></a>3 Entrega de metadatos con tiempo

Los datos de los flujos de eventos son opacos para Media Services. Media Services simplemente pasa tres fragmentos de información entre el punto de conexión de ingesta y el punto de conexión del cliente. Las siguientes propiedades se entregan al cliente, conforme a lo que se especifica en [SCTE-35] o en el protocolo de streaming del cliente:

1.  Esquema: un URN o URL que identifica el esquema del mensaje.
2.  Tiempo de presentación: el tiempo de presentación del evento en la escala de tiempo de los elementos multimedia.
3.  Duración: la duración del evento.
4.  ID: el identificador único opcional para el evento.
5.  Mensaje: los datos del evento.

## <a name="31-microsoft-smooth-streaming-manifest"></a>3.1 Manifiesto de Microsoft Smooth Streaming  

Consulte el tema de administración de pistas dispersas [MS-SSTR] para obtener más información sobre cómo dar formato a una pista de mensaje dispersa. En el caso de los mensajes [SCTE35], Smooth Streaming producirá la sección splice_info_section () con codificación Base 64 en un fragmento disperso.
El valor de StreamIndex **DEBE** tener un subtipo "DATA", y el valor de CustomAttributes **DEBE** contener un atributo con Name = "Schema" y Value = "urn:scte:scte35:2013:bin".

#### <a name="smooth-client-manifest-example-showing-base64-encoded-scte35-splice_info_section"></a>Ejemplo del manifiesto de cliente de Smooth en el que se muestra la sección splice_info_section() de [SCTE35] codificada en Base 64
~~~ xml
<?xml version=”1.0” encoding=”utf-8”?>
<SmoothStreamingMedia MajorVersion=”2” MinorVersion=”0” TimeScale=”10000000” IsLive=”true” Duration=”0”
  LookAheadFragmentCount=”2” DVRWindowLength=”6000000000”>

  <StreamIndex Type=”video” Name=”video” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(video={start time})”>
    <QualityLevel Index=”0” Bitrate=”230000”
      CodecPrivateData=”250000010FC3460B50878A0B5821FF878780490800704704DC0000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <QualityLevel Index=”1” Bitrate=”305000”
      CodecPrivateData=”250000010FC3480B50878A0B5821FF87878049080894E4A7640000010E5A67F840” FourCC=”WVC1”
      MaxWidth=”364” MaxHeight=”272”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”audio” Name=”audio” Subtype=”” Chunks=”0” TimeScale=”10000000”
    Url=”QualityLevels({bitrate})/Fragments(audio={start time})”>
    <QualityLevel Index=”0” Bitrate=”96000” CodecPrivateData=”1000030000000000000000000000E00042C0”
      FourCC=”WMAP” AudioTag=”354” Channels=”2” SamplingRate=”44100” BitsPerSample=”16” PacketSize=”4459”/>
    <c t=”0” d=”20000000” r=”300” />
  </StreamIndex>
  <StreamIndex Type=”text” Name=”scte35-sparse-stream” Subtype=”DATA” Chunks=”0” TimeScale=”10000000”
    ParentStreamIndex=”video” ManifestOutput=”true” 
    Url=”QualityLevels({bitrate})/Fragments(captions={start time})”>
    <QualityLevel Index=”0” Bitrate=”0” CodecPrivateData=”” FourCC=””>
      <CustomAttributes>
        <Attribute Name=”Scheme” Value=”urn:scte:scte35:2013:bin”/>
      </CustomAttributes>
    </QualityLevel>
    <!-- The following <c> and <f> fragments contains the base64-encoded [SCTE35] splice_info_section() message -->
    <c t=”600000000” d=”300000000”>    <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTUwMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
    <c t=”1200000000” d=”400000000”>      <f>PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0idXRmLTgiPz48QWNxdWlyZWRTaWduYWwgeG1sbnM9InVybjpjYWJsZWxhYnM6bWQ6eHNkOnNpZ25hbGluZzozLjAiIGFjcXVpc2l0aW9uUG9pbnRJZGVudGl0eT0iRVNQTl9FYXN0X0FjcXVpc2l0aW9uX1BvaW50XzEiIGFjcXVpc2l0aW9uU2lnbmFsSUQ9IjRBNkE5NEVFLTYyRkExMUUxQjFDQTg4MkY0ODI0MDE5QiIgYWNxdWlzaXRpb25UaW1lPSIyMDEyLTA5LTE4VDEwOjE0OjI2WiI+PFVUQ1BvaW50IHV0Y1BvaW50PSIyMDEyLTA5LTE4VDEwOjE0OjM0WiIvPjxTQ1RFMzVQb2ludERlc2NyaXB0b3Igc3BsaWNlQ29tbWFuZFR5cGU9IjUiPjxTcGxpY2VJbnNlcnQgc3BsaWNlRXZlbnRJRD0iMzQ0NTY4NjkxIiBvdXRPZk5ldHdvcmtJbmRpY2F0b3I9InRydWUiIHVuaXF1ZVByb2dyYW1JRD0iNTUzNTUiIGR1cmF0aW9uPSJQVDFNMFMiIGF2YWlsTnVtPSIxIiBhdmFpbHNFeHBlY3RlZD0iMTAiLz48L1NDVEUzNVBvaW50RGVzY3JpcHRvcj48U3RyZWFtVGltZXM+PFN0cmVhbVRpbWUgdGltZVR5cGU9IkhTUyIgdGltZVZhbHVlPSI1MTYyMDAwMDAwMDAiLz48L1N0cmVhbVRpbWVzPjwvQWNxdWlyZWRTaWduYWw+</f>
    </c>
  </StreamIndex>
</SmoothStreamingMedia>
~~~

## <a name="32-apple-hls-manifest-decoration"></a>3.2 Decoración del manifiesto HLS de Apple

Azure Media Services admite las siguientes etiquetas del manifiesto HLS para señalizar información de disponibilidad de los anuncios durante un evento en directo o a petición. 

- EXT-X-DATERANGE tal y como se define en el protocolo HLS de Apple [RFC8216]
- EXT-X-CUE tal y como se define en [Adobe-PrimeTime]: este modo se considera "heredado". Los clientes deben adoptar la etiqueta EXT-X-DATERANGE cuando sea posible.

La salida de los datos para cada etiqueta variará en función del modo de señal de ingesta que se use. Por ejemplo, la ingesta de RTMP con el modo simple de Adobe no contiene la carga con codificación Base 64 completa de SCTE-35.

## <a name="321-apple-hls-with-adobe-primetime-ext-x-daterange-recommended"></a>3.2.1 Protocolo HLS de Apple con la etiqueta EXT-X-DATERANGE de Adobe PrimeTime (recomendado)

La especificación de HTTP Live Streaming de Apple [RFC8216] permite la señalización de mensajes [SCTE-35]. Los mensajes se insertan en la lista de reproducción de segmentos en una etiqueta EXT-X-DATERANGE por sección [RFC8216] titulada "Mapping SCTE-35 into EXT-X-DATERANGE".  El nivel de aplicación cliente puede analizar la lista de reproducción M3U y procesar las etiquetas M3U, o bien recibir los eventos a través del marco de reproducción de Apple.  

El enfoque **RECOMENDADO** en Azure Media Services (API de la versión 3) es seguir [RFC8216] y usar la etiqueta EXT-X_DATERANGE para la decoración de disponibilidad de anuncios de [SCTE35] en el manifiesto.

## <a name="322-apple-hls-with-adobe-primetime-ext-x-cue-legacy"></a>3.2.2 Protocolo HLS de Apple con la etiqueta EXT-X-CUE de Adobe PrimeTime (heredado)

También hay una implementación "heredada" que se proporciona en Azure Media Services (API de la versión 2 y 3) que usa la etiqueta EXT-X-CUE tal como se define en el "modo SCTE-35" de [Adobe-PrimeTime]. En este modo, Azure Media Services insertará la sección splice_info_section() de [SCTE-35] codificada en Base 64 en la etiqueta EXT-X-CUE.  

La etiqueta EXT-X-CUE "heredada" se define como se indica a continuación y también se le puede hacer referencia en normativas de la especificación de [Adobe-PrimeTime]. Solo se debe usar para la señalización de SCTE35 heredada cuando sea necesario; en caso contrario, la etiqueta recomendada se define en [RFC8216] como EXT-X-DATERANGE. 

| **Nombre del atributo** | **Tipo**                      | **¿Necesario?**                             | **Descripción**                                                                                                                                                                                                                                                                      |
|--------------------|-------------------------------|-------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| CUE                | cadena entrecomillada                 | Obligatorio                                  | Mensaje codificado como una cadena en base 64 tal y como se describe en [RFC4648]. En el caso de los mensajes [SCTE-35], es la sección splice_info_section() codificada en base 64.                                                                                                |
| TIPO               | cadena entrecomillada                 | Obligatorio                                  | Un URN o una dirección URL que identifiquen el esquema del mensaje. En el caso de los mensajes [SCTE-35], el tipo tiene el valor especial "scte35".                                                                                                                                |
| id                 | cadena entrecomillada                 | Obligatorio                                  | Un identificador único para el evento. Si no se especifica el identificador cuando se ingiere el mensaje, Azure Media Services generará un identificador exclusivo.                                                                                                                                          |
| DURATION           | número de punto flotante decimal | Obligatorio                                  | Duración del evento. Si no se conoce, el valor **DEBERÍA** ser 0. Las unidades son fracciones de segundos.                                                                                                                                                                                           |
| ELAPSED            | número de punto flotante decimal | Opcional, pero obligatorio para la ventana deslizante | Cuando se repite la señal para que admita una ventana de presentación deslizante, este campo **DEBE** ser la cantidad de tiempo de la presentación que ha transcurrido desde que comenzó el evento. Las unidades son fracciones de segundos. Este valor puede superar la duración original especificada de la inserción o segmentación. |
| TIME               | número de punto flotante decimal | Obligatorio                                  | El tiempo de presentación del evento. Las unidades son fracciones de segundos.                                                                                                                                                                                                                    |


El nivel de aplicación del reproductor HLS usará TYPE para identificar el formato del mensaje, descodificar el mensaje, aplicar las conversiones de tiempo necesarias y procesar el evento.  Los eventos se sincronizan en la lista de reproducción de segmentos de la pista principal, según la marca de tiempo del evento.  Se insertan antes del segmento más cercano (etiqueta #EXTINF).

### <a name="323-hls-segment-playlist-example-using-legacy-adobe-primetime-ext-x-cue"></a>3.2.3 Ejemplo de lista de reproducción del segmento HLS con la etiqueta EXT-X-CUE de Adobe PrimeTime "heredada"

En el ejemplo siguiente se muestra la decoración del manifiesto HLS mediante la etiqueta EXT-X-CUE de Adobe PrimeTime.  El parámetro "CUE" contiene la carga splice_info de SCTE-35 con codificación Base 64 completa que indica que esta señal llegó usando RTMP en el modo de señal de SCTE-35 de Adobe, o llegó a través del modo de señal de SCTE-35 de Smooth Streaming. 

~~~
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-ALLOW-CACHE:NO
#EXT-X-MEDIA-SEQUENCE:346
#EXT-X-TARGETDURATION:6
#EXT-X-I-FRAMES-ONLY
#EXT-X-PROGRAM-DATE-TIME:2018-12-13T15:54:19.462Z
#EXTINF:4.000000,no-desc
KeyFrames(video_track=15447164594627600,format=m3u8-aapl)
#EXTINF:6.000000,no-desc
KeyFrames(video_track=15447164634627600,format=m3u8-aapl)
#EXT-X-CUE:ID="1026",TYPE="scte35",DURATION=30.000000,TIME=1544716520.022760,CUE="/DAlAAAAAAAAAP/wFAUAAAQCf+//KRjAfP4AKTLgAAAAAAAAVYsh2w=="
#EXTINF:6.000000,no-desc
KeyFrames(video_track=15447165474627600,format=m3u8-aapl)
~~~

### <a name="324-hls-message-handling-for-legacy-adobe-primetime-ext-x-cue"></a>3.2.4 Administración de mensajes de HLS para la etiqueta EXT-X-CUE de Adobe PrimeTime "heredada"

Los eventos se señalizan en la lista de reproducción de segmentos de cada vídeo y pista de audio. La posición de la etiqueta EXT-X-CUE siempre **DEBE** estar inmediatamente antes del primer segmento HLS (fuera de la inserción o al inicio del segmento) o inmediatamente después del último segmento HLS (dentro de la inserción o al final del segmento) al que hacen referencia sus atributos TIME y DURATION, como requiera [Adobe-Primetime].

Cuando se habilita una ventana de presentación deslizante, la etiqueta EXT-X-CUE se **DEBE** repetir con la suficiente frecuencia para que la inserción o segmento estén siempre descritos por completo en la lista de reproducción de segmentos, y se **DEBE** usar el atributo ELAPSED para indicar la cantidad de tiempo que la inserción o segmento han estado activos, como requiera [Adobe-Primetime].

Cuando se habilita una ventana de presentación deslizante, se quitan las etiquetas EXT-X-CUE de la lista de reproducción de segmentos si el tiempo de los elementos multimedia al que hacen referencia se ha extendido más allá de la ventana de presentación deslizante.

## <a name="33-dash-manifest-decoration-mpd"></a>3.3 Decoración del manifiesto DASH (MPD)

[MPEGDASH] proporciona tres maneras de señalizar eventos:

1.  Eventos señalizados en el elemento EventStream de MPD
2.  Eventos señalizados en banda mediante el cuadro de mensajes de evento ("emsg")
3.  Una combinación de 1 y 2

Los eventos señalizados en el elemento EventStream de MPD resultan útiles para el streaming de vídeo bajo demanda, ya que los clientes tienen acceso a todos los eventos justo después de descargar MPD. También son útiles para la señalización de SSAI, en que el proveedor de SSAI de bajada debe analizar las señales de un manifiesto de MPD de varios períodos e insertar contenido de anuncios de forma dinámica.  La solución en banda ("emsg") resulta útil para el streaming en vivo en el que no es necesario que los clientes descarguen de nuevo MPD, o bien no se produce ninguna manipulación del manifiesto de SSAI entre el cliente y el origen. 

El comportamiento predeterminado de Azure Media Services para DASH consiste en señalizar ambos en el elemento EventStream de MPD y en banda mediante el cuadro de mensajes de evento (' eMsg ').

Los mensajes de pila que se ingieren a través de [RTMP] o [MS-SSTR-Ingest] se asignan a los eventos DASH mediante los cuadros "eMsg" en banda o en los elementos EventStreams de MPD. 

La señalización en banda de SCTE-35 para DASH sigue la definición y los requisitos definidos en [SCTE-214-3] y también en la sección 13.12.2 ("SCTE35 Events") de [DASH-IF-IOP]. 

En el caso del transporte en banda de [SCTE-35], el cuadro de mensajes de evento ("eMsg") usa schemeId = "urn:scte:scte35:2013:bin". En el caso de la decoración del manifiesto MPD, el valor de schemeId del elemento EventStream usa "urn:scte:scte35:2014:xml+bin".  Este formato es una representación XML del evento que incluye una salida binaria codificada en Base 64 del mensaje de SCTE-35 completo que llegó en el momento de la ingesta. 

Las definiciones de referencia normativa de transporte de los mensajes de pila de [SCTE-35] en DASH están disponibles en la sección 6.7.4 de [SCTE-214-1] (MPD) y la sección 7.3.2 de [SCTE-214-3] (transporte de mensajes de pila de SCTE 35).

### <a name="331-mpeg-dash-mpd-eventstream-signaling"></a>3.3.1 Señalización del elemento EventStream de MPEG-DASH (MPD)

La decoración del manifiesto (MPD) de los eventos se señalizará en MPD mediante el elemento EventStream que aparece en el elemento Period. El valor de schemeId que se usa es "urn:scte:scte35:2014:xml+bin".

> [!NOTE]
> Por motivos de brevedad, [SCTE-35] permite el uso de la sección codificada en Base 64 en el elemento Signal.Binary (en lugar del elemento Signal.SpliceInfoSection) como alternativa al transporte de un mensaje de pila completamente analizado.
> Azure Media Services usa este enfoque "xml+bin" para realizar señalizaciones en el manifiesto de MPD.
> Este método recomendado también se usa en [DASH-IF-IOP]. Consulte la sección titulada [Secuencias de eventos de inserción de anuncios" de las instrucciones de DASH IF IOP](https://dashif-documents.azurewebsites.net/DASH-IF-IOP/master/DASH-IF-IOP.html#ads-insertion-event-streams).
> 

El elemento EventStream tiene los siguientes atributos:

| **Nombre del atributo** | **Tipo**                | **¿Necesario?** | **Descripción**                                                                                                                                                                                                                                                                                   |
|--------------------|-------------------------|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri      | string                  | Obligatorio      | Identifica el esquema del mensaje. El esquema se establece en el valor del atributo Scheme en el cuadro de Live Server Manifest box. El valor **DEBE** ser un URN o una dirección URL que identifique el esquema del mensaje. El valor de schemeId de salida compatible debe ser "urn:scte:scte35:2014:xml+bin" según la sección 6.7.4 de [SCTE-214-1] (MPD), ya que el servicio solo admite "xml+bin" en este momento por motivos de brevedad en MPD.  |
| value              | string                  | Opcional      | Un valor de cadena adicional que usan los propietarios del esquema para personalizar la semántica del mensaje. Para diferenciar entre varias secuencias de eventos con el mismo esquema, se **DEBE** establecer el valor en el nombre de la secuencia de eventos (trackName para [MS-SSTR-Ingest] o el nombre del mensaje de AMF para la ingesta de [RTMP]). |
| Timescale          | Entero sin signo de 32 bits | Obligatorio      | Escala de tiempo, en tics por segundo.                                                                                                                                                                                                       |


### <a name="332-example-mpeg-dash-manifest-mpd-signaling-of-scte-35-using-eventstream"></a>3.3.2 Ejemplo de señalización del manifiesto MPEG-DASH (MPD) de SCTE-35 mediante EventStream

~~~ xml
<!-- Example MPD using xml+bin style signaling per [SCTE-214-1] -->
  <EventStream schemeIdUri="urn:scte:scte35:2014:xml+bin" value="scte35_track_001_000" timescale="10000000">
        <Event presentationTime="15447165200227600" duration="300000000" id="1026">
            <scte35:Signal>
                <scte35:Binary>
                    /DAlAAAAAAAAAP/wFAUAAAQDf+//KaeGwP4AKTLgAAAAAAAAn75a3g==
                </scte35:Binary>
            </scte35:Signal>
        </Event>
        <Event presentationTime="15447166250227600" duration="300000000" id="1027">
           <scte35:Signal>
                <scte35:Binary>
                    /DAlAAAAAAAAAP/wFAUAAAQDf+//KaeGwP4AKTLgAAAAAAAAn75a3g==
                </scte35:Binary>
            </scte35:Signal>
        </Event>
    </EventStream>
~~~

> [!IMPORTANT]
> Tenga en cuenta que presentationTime hace referencia al tiempo de presentación del evento de [SCTE-35] convertido para ser relativo a la hora de inicio del período y no a la hora de llegada del mensaje.
> [MPEGDASH] define Event@presentationTime como especifica el tiempo de presentación del evento relativo al inicio del período.
> El valor del tiempo de presentación en segundos es la división del valor de este atributo y el valor del atributo EventStream@timescale.
> Si no existe, el valor de la hora de presentación es 0.


### <a name="333-mpeg-dash-in-band-event-message-box-signaling"></a>3.3.3 Señalización del cuadro de mensajes de evento en banda de MPEG-DASH

Un flujo de eventos en banda requiere que el MPD tenga el elemento InbandEventStream en el nivel del conjunto de adaptación.  Este elemento tiene un atributo schemeIdUri obligatorio y un atributo opcional de escala de tiempo que también aparecen en el cuadro de mensajes de evento ('emsg').  Los cuadros de mensajes de evento con los identificadores de esquema que no estén definidos en MPD no **DEBEN** existir.

En el caso del transporte en banda de[SCTE-35 ], las señales **DEBEN** usar el valor de schemeId = "urn:scte:scte35:2013:bin".
Las definiciones normativas del transporte de los mensajes en banda de [SCTE-35] se definen en la sección 7.3.2 de [SCTE-214-3] (transporte de mensajes de pila de SCTE 35).

En los siguientes detalles se describen los valores específicos que el cliente debe esperar en "emsg" en cumplimiento con [SCTE-214-3]:

| **Nombre del campo**          | **Tipo de campo**          | **¿Necesario?** | **Descripción**                                                                                                                                                                                                                                                                                                                                                    |
|-------------------------|-------------------------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| scheme_id_uri           | string                  | Obligatorio      | Identifica el esquema del mensaje. El esquema se establece en el valor del atributo Scheme en el cuadro de Live Server Manifest box. El valor **DEBE** ser un URN que identifique el esquema del mensaje. En el caso de los mensajes de [SCTE-35], **DEBE** ser "urn:scte:scte35:2013:bin" conforme a [SCTE-214-3]. |
| Valor                   | string                  | Obligatorio      | Un valor de cadena adicional que usan los propietarios del esquema para personalizar la semántica del mensaje. Para diferenciar entre varios flujos de eventos con el mismo esquema, se establecerá el valor en el nombre del flujo de eventos (trackName para la ingesta con Smooth o el nombre del mensaje de AMF para la ingesta de RTMP).                                                                  |
| Timescale               | Entero sin signo de 32 bits | Obligatorio      | La escala de tiempo, en tics por segundo, de los campos de tiempo y duración del cuadro 'emsg'.                                                                                                                                                                                                                                                                        |
| Presentation_time_delta | Entero sin signo de 32 bits | Obligatorio      | El diferencial de tiempo de presentación del elemento multimedia entre el tiempo de presentación del evento y el tiempo de presentación más temprano de este segmento. El tiempo de presentación y la duración se **DEBERÍAN** alinear con los puntos de acceso de secuencias (SAP) de tipo 1 o 2, tal como se define en el Anexo I de [ISO-14496-12].                                                                                            |
| event_duration          | Entero sin signo de 32 bits | Obligatorio      | La duración del evento, o 0xFFFFFFFF para indicar una duración desconocida.                                                                                                                                                                                                                                                                                          |
| Id                      | Entero sin signo de 32 bits | Obligatorio      | Identifica la instancia del mensaje. Los mensajes con una semántica equivalente deben tener el mismo valor. Si no se especifica el identificador cuando se ingiere el mensaje, Azure Media Services generará un identificador exclusivo.                                                                                                                                                    |
| Message_data            | byte array              | Obligatorio      | El mensaje de evento. En el caso de los mensajes [SCTE-35], los datos del mensaje son la sección binaria splice_info_section() conforme a [SCTE-214-3]. |

### <a name="334-dash-message-handling"></a>3.3.4 Administración de mensajes DASH

Los eventos se señalizan en banda, en el cuadro 'emsg', para pistas de audio y de vídeo.  La señalización se produce en todas las solicitudes de segmento en los que presentation_time_delta es de 15 segundos o menos. 

Cuando se habilita una ventana de presentación deslizante, los mensajes de evento se eliminan del MPD si la suma del tiempo y la duración del mensaje de evento es inferior al tiempo de los datos del elemento multimedia en el manifiesto.  En otras palabras, los mensajes de evento se eliminarán del manifiesto si el tiempo del elemento multimedia al que hacen referencia se ha extendido más allá de la ventana de presentación deslizante.

## <a name="4-scte-35-ingest-implementation-guidance-for-encoder-vendors"></a>4. Guía de implementación de ingesta de SCTE-35 para proveedores de codificadores

Las siguientes instrucciones tratan problemas comunes que pueden afectar a la implementación del proveedor del codificador de esta especificación.  Las instrucciones siguientes se han recopilado a partir de comentarios reales de los asociados a fin de simplificar la implementación de esta especificación para otros usuarios. 

Los mensajes de [SCTE-35] se ingieren en formato binario mediante el esquema **"urn:scte:scte35:2013:bin"** para [MS-SSTR-Ingest] y el tipo **"scte35"** para la ingesta de [RTMP]. Para facilitar la conversión de la temporización de [SCTE-35], que se basa en las marcas de tiempo de presentación (PTS) de flujos de transporte MPEG-2, el tiempo de presentación del evento proporciona una asignación entre PTS (pts_time + pts_adjustment de splice_time()) y la escala de tiempo del elemento multimedia (el campo fragment_absolute_time para la ingesta con Smooth y el campo time para la ingesta con RTMP). La asignación es necesaria ya que el valor PTS de 33 bits cambia cada 26,5 horas aproximadamente.

En la ingesta de Smooth Streaming, [MS-SSTR-Ingest] requiere que el cuadro de datos multimedia ("mdat") **CONTENGA** la sección **splice_info_section()** definida en [SCTE-35]. 

Para la ingesta de RTMP, se establece el atributo de pila del mensaje de AMF en la sección **splice_info_section()** codificada en base 64 definida en [SCTE-35].  

Cuando los mensajes tienen el formato descrito anteriormente, se envían a los clientes HLS, Smooth y Dash según se define anteriormente.  

Al probar la implementación con la plataforma de Azure Media Services, empiece a realizar pruebas con un elemento de LiveEvent "de paso a través" en primer lugar, antes de pasar a las pruebas con un elemento de LiveEvent de codificación.

---

## <a name="change-history"></a>Historial de cambios

| Date     | Cambios                                                                            |
|----------|------------------------------------------------------------------------------------|
| 02/07/19  | Se ha revisado la ingesta de RTMP para la compatibilidad con SCTE35 y se ha agregado "onCuePoint" de RTMP para Elemental Live. | 
| 22/08/19 | Actualización para agregar OnUserDataEvent a RTMP para metadatos personalizados.                         |

## <a name="next-steps"></a>Pasos siguientes
Ver las rutas de aprendizaje de Media Services

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
