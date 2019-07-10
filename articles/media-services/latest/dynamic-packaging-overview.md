---
title: Introducción al empaquetado dinámico de Azure Media Services | Microsoft Docs
description: El artículo proporciona información general sobre el empaquetado dinámico en Azure Media Services.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 06/03/2019
ms.author: juliako
ms.openlocfilehash: 4836ec4bb66bbf8ced921dd1095665d004f8a28b
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542577"
---
# <a name="dynamic-packaging"></a>Empaquetado dinámico

Microsoft Azure Media Services puede usarse para proporcionar varios formatos de archivo de origen multimedia, formatos de streaming multimedia y formatos de protección de contenido a diversas tecnologías cliente (por ejemplo, iOS y XBOX). Estos clientes entienden distintos protocolos. Por ejemplo, iOS requiere un formato HTTP Live Streaming (HLS) y Xbox requiere Smooth Streaming. Si tiene un conjunto de archivos MP4 (ISO Base Media 14496-12) de velocidad de bits adaptable (múltiples velocidades de bits), o un conjunto de archivos Smooth Streaming de velocidad de bits adaptable que desea ofrecer a los clientes que entienden HLS, MPEG DASH o Smooth Streaming, puede aprovechar el *empaquetado dinámico*. El empaquetado es independiente de la resolución del vídeo, se admiten SD/HD/UHD-4K.

En Media Services, un [punto de conexión de streaming](streaming-endpoint-concept.md) representa un empaquetado dinámico (just-in-time) y el servicio de origen que puede entregar directamente el contenido en directo y a petición a una aplicación de reproducción de cliente, mediante uno de los protocolos de streaming de multimedia (HLS o DASH). El empaquetado dinámico es una característica incluida en todos los **puntos de conexión de streaming** (estándar o premium). 

Para sacar partido del empaquetado dinámico, tiene que tener un **Recurso** con un conjunto de archivos MP4 de velocidad de bits adaptable, y los archivos de configuración de streaming que el empaquetado dinámico de Media Services necesita. Una manera de obtener los archivos consiste en codificar su archivo intermedio (origen) con Media Services. Para que los vídeos en el recurso codificado estén disponibles para que los clientes puedan reproducirlos, tiene que crear un **localizador de streaming** y generar direcciones URL de streaming. Después, según el formato especificado en el manifiesto del cliente de streaming (HLS, DASH o Smooth), recibirá la transmisión en el protocolo que haya elegido.

Como resultado, solo tendrá que almacenar y pagar los archivos en formato de almacenamiento único y Media Services creará y proporcionará la respuesta adecuada en función de las solicitudes de un cliente. 

En Media Services el empaquetado dinámico se utiliza para el streaming en directo o a petición. 

> [!NOTE]
> Actualmente, no puede usar Azure Portal para administrar recursos de v3. Use la [API REST](https://aka.ms/ams-v3-rest-ref), la [CLI](https://aka.ms/ams-v3-cli-ref) o uno de los [SDK](media-services-apis-overview.md#sdks) admitidos.

## <a name="on-demand-streaming-workflow"></a>Flujo de trabajo de streaming a petición

Este es un flujo de trabajo común para el streaming a petición con el empaquetado dinámico de Media Services:

1. Cargar un archivo de entrada o archivo de origen (llamado archivo *intermedio*). Algunos ejemplos son un archivo MP4, MOV o MXF. 
1. Codificar el archivo intermedio en conjuntos MP4 de velocidad de bits adaptable H.264. 
1. Publicación del recurso que contiene el conjunto de MP4 de velocidad de bits adaptable. Publicar mediante la creación de un localizador de streaming.
1. Generar direcciones URL que tienen como destino diferentes formatos (HLS, MPEG-DASH y Smooth Streaming). El punto de conexión de streaming se encarga de atender el manifiesto correcto y las solicitudes de los diferentes formatos.

En el siguiente diagrama se muestra el flujo de trabajo para streaming a petición con empaquetado dinámico:

![Diagrama de un flujo de trabajo para streaming a petición con empaquetado dinámico](./media/dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="live-streaming-workflow"></a>Flujo de trabajo de streaming en vivo

Un evento en directo puede ser de uno de estos dos tipos: paso a través y codificación en directo. 

Este es un flujo de trabajo común para el streaming en vivo con el empaquetado dinámico:

1. Cree un [evento en directo](live-events-outputs-concept.md).
1. Obtenga la dirección URL de ingesta y configure el codificador local a fin de usar la dirección URL para enviar la fuente de contribución.
1. Obtenga la dirección URL de versión preliminar y úsela para verificar que la entrada del codificador se está recibiendo.
1. Cree un recurso nuevo.
1. Cree una salida en directo y use el nombre del recurso que ha creado.<br />La salida en directo archivará la secuencia en el recurso.
1. Cree un localizador de streaming con los tipos de directiva de streaming integrados.<br />Si va a cifrar el contenido, consulte [Introducción a la protección de contenido](content-protection-overview.md).
1. Enumere las rutas de acceso en el localizador de streaming para recuperar las direcciones URL que se van a usar.
1. Obtenga el nombre de host del punto de conexión de streaming desde el que quiere realizar el streaming.
1. Generar direcciones URL que tienen como destino diferentes formatos (HLS, MPEG-DASH y Smooth Streaming). El punto de conexión de streaming se encarga de atender el manifiesto correcto y las solicitudes de los diferentes formatos.

Este diagrama se muestra el flujo de trabajo para el streaming en vivo con empaquetado dinámico:

![Diagrama de un flujo de trabajo para codificación de paso a través con empaquetado dinámico](./media/live-streaming/pass-through.svg)

Para más información acerca del streaming en vivo en Media Services v3, consulte [Introducción al streaming en vivo](live-streaming-overview.md).

## <a name="delivery-protocols"></a>Protocolos de entrega

Puede usar estos protocolos de entrega para el contenido de empaquetado dinámico de Media Services:

|Protocolo|Ejemplo|
|---|---|
|HLS V4 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl)`|
|HLS V3 |`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-aapl-v3)`|
|HLS CMAF| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=m3u8-cmaf)`|
|MPEG-DASH CSF| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-csf)` |
|MPEG-DASH CMAF|`https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest(format=mpd-time-cmaf)` |
|Smooth Streaming| `https://amsv3account-usw22.streaming.media.azure.net/21b17732-0112-4d76-b526-763dcd843449/ignite.ism/manifest`|

## <a name="encode-to-adaptive-bitrate-mp4s"></a>Codificación en archivos MP4s de velocidad de bits adaptable

Los artículos siguientes muestran ejemplos de [cómo codificar un vídeo con Media Services](encoding-concept.md):

* [Codificación desde una dirección URL de HTTPS con valores preestablecidos integrados](job-input-from-http-how-to.md)
* [Codificación de un archivo local con valores preestablecidos integrados](job-input-from-local-file-how-to.md)
* [Compilación de un valor preestablecido personalizado para sus requisitos específicos de escenario o dispositivo](customize-encoder-presets-how-to.md)

Consulte la lista de [códecs y formatos](media-encoder-standard-formats.md) de Media Encoder Standard.

## <a name="video-codecs"></a>Códecs de vídeo

El empaquetado dinámico admite los siguientes códecs de vídeo:
* Archivos MP4, que contienen vídeo codificado con [H.264](https://en.m.wikipedia.org/wiki/H.264/MPEG-4_AVC) (MPEG-4 AVC o AVC1) o [H.265](https://en.m.wikipedia.org/wiki/High_Efficiency_Video_Coding) (HEVC, hev1, o hvc1).

## <a name="audio-codecs"></a>Códecs de audio

El empaquetado dinámico admite los siguientes protocolos de audio:
* Archivos MP4
* Varias pistas de audio

El empaquetado dinámico no admite archivos que contienen audio [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) (es un códec heredado).

### <a name="mp4-files"></a>Archivos MP4

El empaquetado dinámico admite archivos MP4, que contienen audio que está codificado con los siguientes protocolos: 

* [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, o HE-AAC v2)
* [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus) (Enhanced AC-3 o E-AC3)
* Dolby Atmos<br />
   El streaming de contenido Dolby Atmos es compatible con estándares como el protocolo MPEG-DASH con formato de streaming común (CSF) o formato de aplicación multimedia común (CMAF) MP4 fragmentado, y mediante HTTP Live Streaming (HLS) con CMAF.

* [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29)<br />
   Los códecs de DTS admitidos por los formatos de empaquetado DASH-CSF, DASH-CMAF, HLS-M2TS y HLS-CMAF son:  

    * DTS Digital Surround (dtsc)
    * DTS-HD alta resolución y DTS HD Master Audio (dtsh)
    * DTS Express (dtse)
    * DTS-HD Lossless (sin núcleo) (dtsl)

### <a name="multiple-audio-tracks"></a>Varias pistas de audio

El empaquetado dinámico admite varias pistas de audio para la salida HLS (versión 4 o posterior) para recursos de streaming que tienen varias pistas de audio con varios códecs y lenguajes.

## <a name="dynamic-encryption"></a>Cifrado dinámico

Puede usar el *cifrado dinámico* para cifrar de forma dinámica el contenido en directo o a petición con AES-128 o cualquiera de los tres principales sistemas de administración de derechos digitales (DRM): Microsoft PlayReady, Google Widevine y Apple FairPlay. Media Services también proporciona un servicio para entregar claves AES y licencias de DRM a clientes autorizados. Para más información, consulte [Cifrado dinámico](content-protection-overview.md).

## <a name="manifest-examples"></a>Ejemplos de manifiestos 
 
En el empaquetado dinámico de Media Services, los manifiestos de cliente de streaming para HLS, MPEG-DASH y Smooth Streaming, se generarán dinámicamente basándose en el selector de formato en la dirección URL. Para más información, consulte [Protocolos de entrega](#delivery-protocols). 

Un archivo de manifiesto incluye metadatos de streaming como: el tipo de pista (audio, vídeo o texto), el nombre de la pista, la hora inicial y final, la velocidad de bits (calidades), los idiomas de pista, la ventana de presentación (ventana deslizante de duración fija), el códec de vídeo (FourCC). También indica al reproductor que recupere el siguiente fragmento ofreciendo información acerca de los próximos fragmentos de vídeo reproducibles que están disponibles y su ubicación. Los fragmentos (o segmentos) son "fragmentos" reales de un contenido de vídeo.

### <a name="hls"></a>HLS

Este es un ejemplo de un archivo de manifiesto HLS, también llamado lista de reproducción maestra HLS: 

```
#EXTM3U
#EXT-X-VERSION:4
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536608,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(381048)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536608,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(381048)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884544,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721495)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884544,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721495)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327398,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1154816)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327398,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1154816)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2413312,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2217354)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2413312,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2217354)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805760,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579827)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805760,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579827)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

### <a name="mpeg-dash"></a>MPEG-DASH

Este es un ejemplo de un archivo de manifiesto MPEG-DASH, también llamado descripción de presentación multimedia (MPD) MPEG-DASH:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<MPD xmlns="urn:mpeg:dash:schema:mpd:2011" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" profiles="urn:mpeg:dash:profile:isoff-live:2011" type="static" mediaPresentationDuration="PT1M10.315S" minBufferTime="PT7S">
   <Period>
      <AdaptationSet id="1" group="5" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="audio" mimeType="audio/mp4" codecs="mp4a.40.2" lang="en">
         <SegmentTemplate timescale="10000000" media="QualityLevels($Bandwidth$)/Fragments(aac_eng_2_128041_2_1=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(aac_eng_2_128041_2_1=i,format=mpd-time-csf)">
            <SegmentTimeline>
               <S d="60160000" r="10" />
               <S d="41386666" />
            </SegmentTimeline>
         </SegmentTemplate>
         <Representation id="5_A_aac_eng_2_128041_2_1_1" bandwidth="128041" audioSamplingRate="48000" />
      </AdaptationSet>
      <AdaptationSet id="2" group="1" profiles="ccff" bitstreamSwitching="false" segmentAlignment="true" contentType="video" mimeType="video/mp4" codecs="avc1.640020" maxWidth="1280" maxHeight="720" startWithSAP="1">
         <SegmentTemplate timescale="10000000" media="QualityLevels($Bandwidth$)/Fragments(video=$Time$,format=mpd-time-csf)" initialization="QualityLevels($Bandwidth$)/Fragments(video=i,format=mpd-time-csf)">
            <SegmentTimeline>
               <S d="60060000" r="10" />
               <S d="42375666" />
            </SegmentTimeline>
         </SegmentTemplate>
         <Representation id="1_V_video_1" bandwidth="3579827" width="1280" height="720" />
         <Representation id="1_V_video_2" bandwidth="2217354" codecs="avc1.64001F" width="960" height="540" />
         <Representation id="1_V_video_3" bandwidth="1154816" codecs="avc1.64001E" width="640" height="360" />
         <Representation id="1_V_video_4" bandwidth="721495" codecs="avc1.640015" width="480" height="270" />
         <Representation id="1_V_video_5" bandwidth="381048" codecs="avc1.64000D" width="320" height="180" />
      </AdaptationSet>
   </Period>
</MPD>
```
### <a name="smooth-streaming"></a>Smooth Streaming

Este es un ejemplo de un archivo de manifiesto de Smooth Streaming:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<SmoothStreamingMedia MajorVersion="2" MinorVersion="2" Duration="703146666" TimeScale="10000000">
   <StreamIndex Chunks="12" Type="audio" Url="QualityLevels({bitrate})/Fragments(aac_eng_2_128041_2_1={start time})" QualityLevels="1" Language="eng" Name="aac_eng_2_128041_2_1">
      <QualityLevel AudioTag="255" Index="0" BitsPerSample="16" Bitrate="128041" FourCC="AACL" CodecPrivateData="1190" Channels="2" PacketSize="4" SamplingRate="48000" />
      <c t="0" d="60160000" r="11" />
      <c d="41386666" />
   </StreamIndex>
   <StreamIndex Chunks="12" Type="video" Url="QualityLevels({bitrate})/Fragments(video={start time})" QualityLevels="5">
      <QualityLevel Index="0" Bitrate="3579827" FourCC="H264" MaxWidth="1280" MaxHeight="720" CodecPrivateData="0000000167640020ACD9405005BB011000003E90000EA600F18319600000000168EBECB22C" />
      <QualityLevel Index="1" Bitrate="2217354" FourCC="H264" MaxWidth="960" MaxHeight="540" CodecPrivateData="000000016764001FACD940F0117EF01100000303E90000EA600F1831960000000168EBECB22C" />
      <QualityLevel Index="2" Bitrate="1154816" FourCC="H264" MaxWidth="640" MaxHeight="360" CodecPrivateData="000000016764001EACD940A02FF9701100000303E90000EA600F162D960000000168EBECB22C" />
      <QualityLevel Index="3" Bitrate="721495" FourCC="H264" MaxWidth="480" MaxHeight="270" CodecPrivateData="0000000167640015ACD941E08FEB011000003E90000EA600F162D9600000000168EBECB22C" />
      <QualityLevel Index="4" Bitrate="381048" FourCC="H264" MaxWidth="320" MaxHeight="180" CodecPrivateData="000000016764000DACD941419F9F011000003E90000EA600F14299600000000168EBECB22C" />
      <c t="0" d="60060000" r="11" />
      <c d="42375666" />
   </StreamIndex>
</SmoothStreamingMedia>
```

## <a name="dynamic-manifest"></a>Manifiesto dinámico

Para controlar el número de pistas, formatos, velocidades de bits y ventanas de tiempo de presentación que se envían a los reproductores, puede usar el filtro dinámico con el empaquetador dinámico de Media Services. Para más información, consulte [Manifiestos de un filtrado previo con el empaquetador dinámico](filters-dynamic-manifest-overview.md).

## <a name="more-information"></a>Más información

Consulte [Comunidad de Azure Media Services](media-services-community.md) para ver diferentes formas de formular preguntas, enviar comentarios y obtener actualizaciones de Media Services.

## <a name="next-steps"></a>Pasos siguientes

Aprenda cómo [cargar, codificar y realizar streaming de vídeos](stream-files-tutorial-with-api.md).

