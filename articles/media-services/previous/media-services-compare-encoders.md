---
title: Comparación de codificadores multimedia a petición de Azure | Microsoft Docs
description: En esta tema se comparan las funcionalidades de codificación de **Media Encoder Standard** y **Flujo de trabajo premium de Media Encoder**.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: a79437c0-4832-423a-bca8-82632b2c47cc
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2018
ms.author: juliako;anilmur
ms.openlocfilehash: c08759f4682c6010c2338ff7aaf61cda92eb0484
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50232094"
---
# <a name="comparison-of-azure-on-demand-media-encoders"></a>Comparación de codificadores multimedia a petición de Azure

En esta tema se comparan las funcionalidades de codificación de **Media Encoder Standard** y **Flujo de trabajo premium de Media Encoder**.

## <a name="video-and-audio-processing-capabilities"></a>Funcionalidades de procesamiento de vídeo y audio

En la tabla siguiente se compara la funcionalidad entre Media Encoder Standard (MES) y el flujo de trabajo de Media Encoder Premium (MEPW). 

|Capacidad|Media Encoder Estándar|Flujo de trabajo del Codificador multimedia|
|---|---|---|
|Aplicación de la lógica condicional durante la codificación<br/>(por ejemplo, si la entrada es HD, entonces, codifique audio 5.1)|Sin |SÍ|
|Subtítulos (CC)|Sin |[Sí](media-services-premium-workflow-encoder-formats.md#closed_captioning)|
|[Corrección de volumen profesional Dolby®](http://www.dolby.com/us/en/technologies/dolby-professional-loudness-solutions.pdf)<br/> con Dialogue Intelligence™|Sin |SÍ|
|Deshacer entrelazado, telecine inverso|Básica|Calidad de difusión|
|Detectar y quitar bordes negros <br/>(formato pillarbox y formato letterbox)|Sin |SÍ|
|Generación de miniaturas|[Sí](media-services-dotnet-generate-thumbnail-with-mes.md)|[Sí](media-services-media-encoder-premium-workflow-tutorials.md#thumbnails_to__multibitrate_MP4)|
|Recorte y unión de vídeos|[Sí](media-services-advanced-encoding-with-mes.md#trim_video)|SÍ|
|Superposiciones de audio o vídeo|[Sí](media-services-advanced-encoding-with-mes.md#overlay)|[Sí](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-1--overlay-an-image-on-top-of-the-video)|
|Superposiciones de gráficos|De orígenes de imagen|De orígenes de imagen y texto|
|Varias pistas de idiomas de audio|Limitado|[Sí](media-services-media-encoder-premium-workflow-multiplefilesinput.md#example-2--multiple-audio-language-encoding)|

## <a id="billing"></a>Medidor de facturación usado por cada codificador
| Nombre de procesador multimedia | Precios aplicables | Notas |
| --- | --- | --- |
| **Media Encoder Estándar** |ENCODER |Las tareas de codificación se cobrarán en función de la duración total, en minutos, de todos los archivos multimedia producidos como salida, según la tarifa especificada [aquí][1], bajo la columna ENCODER. |
| **Flujo de trabajo del Codificador multimedia** |CODIFICADOR PREMIUM |Las tareas de Encoding se cobrarán en función de la duración total, en minutos, de todos los archivos multimedia producidos como salida, según la tarifa especificada [aquí][1], bajo la columna PREMIUM ENCODER. |

## <a name="input-containerfile-formats"></a>Formatos de archivo/contenedor de entrada
| Formatos de archivo/contenedor de entrada | Media Encoder Estándar | Flujo de trabajo del Codificador multimedia |
| --- | --- | --- |
| Adobe® Flash® F4V |SÍ |SÍ |
| MXF/SMPTE 377M |SÍ |SÍ |
| GXF |SÍ |SÍ |
| Secuencias de transporte MPEG-2 |SÍ |SÍ |
| Secuencias de programa MPEG-2 |SÍ |SÍ |
| MPEG-4/MP4 |SÍ |SÍ |
| Windows Media/ASF |SÍ |SÍ |
| AVI (sin comprimir de 8 bits/10 bits) |SÍ |SÍ |
| 3GPP/3GPP2 |SÍ |Sin  |
| Formato de archivo de streaming con velocidad de transmisión adaptable (PIFF 1.3) |SÍ |Sin  |
| [Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) |SÍ |Sin  |
| Matroska/WebM |SÍ |Sin  |
| QuickTime (.mov) |SÍ |Sin  |

## <a name="input-video-codecs"></a>Códecs de vídeo de entrada
| Códecs de vídeo de entrada | Media Encoder Estándar | Flujo de trabajo del Codificador multimedia |
| --- | --- | --- |
| AVC 8 bits/10 bits, hasta 4:2:2, incluido AVCIntra |8 bits: 4:2:0 y 4:2:2 |SÍ |
| Avid DNxHD (en MXF) |SÍ |SÍ |
| DVCPro/DVCProHD (en MXF) |SÍ |SÍ |
| JPEG2000 |SÍ |SÍ |
| MPEG-2 (hasta 422 Perfil y Nivel alto; incluidas variantes como XDCAM, XDCAM HD, XDCAM IMX, CableLabs® y D10) |Hasta 422 Perfil |SÍ |
| MPEG-1 |SÍ |SÍ |
| Windows Media Video/VC-1 |SÍ |SÍ |
| Canopus HQ/HQX |Sin  |Sin  |
| MPEG-4, parte 2 |SÍ |Sin  |
| [Theora](https://en.wikipedia.org/wiki/Theora) |SÍ |Sin  |
| Apple ProRes 422 |SÍ |Sin  |
| Apple ProRes 422 LT |SÍ |Sin  |
| Apple ProRes 422 HQ |SÍ |Sin  |
| Apple ProRes Proxy |SÍ |Sin  |
| Apple ProRes 4444 |SÍ |Sin  |
| Apple ProRes 4444 XQ |SÍ |Sin  |
| HEVC/H.265|Perfil Main|Perfil Main y Main 10|

## <a name="input-audio-codecs"></a>Códecs de audio de entrada
| Códecs de audio de entrada | Media Encoder Estándar | Flujo de trabajo del Codificador multimedia |
| --- | --- | --- |
| AES (SMPTE 331M y 302M, AES3-2003) |Sin  |SÍ |
| Dolby® E |Sin  |SÍ |
| Dolby® Digital (AC3) |Sin  |SÍ |
| Dolby® Digital Plus (E-AC3) |Sin  |SÍ |
| AAC (AAC-LC, AAC-HE y AAC-HEv2; hasta 5.1) |SÍ |SÍ |
| MPEG Layer 2 |SÍ |SÍ |
| MP3 (MPEG-1 Audio Layer 3) |SÍ |SÍ |
| Windows Media Audio |SÍ |SÍ |
| WAV/PCM |SÍ |SÍ |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |SÍ |Sin  |
| [Opus](https://en.wikipedia.org/wiki/Opus_\(audio_format\)) |SÍ |Sin  |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |SÍ |Sin  |

## <a name="output-containerfile-formats"></a>Formatos de archivo/contenedor de salida
| Formatos de archivo/contenedor de salida | Media Encoder Estándar | Flujo de trabajo del Codificador multimedia |
| --- | --- | --- |
| Adobe® Flash® F4V |Sin  |SÍ |
| MXF (OP1a, XDCAM y AS02) |Sin  |SÍ |
| DPP (incluido AS11) |Sin  |SÍ |
| GXF |Sin  |SÍ |
| MPEG-4/MP4 |SÍ |SÍ |
| MPEG-TS |SÍ |SÍ |
| Windows Media/ASF |Sin  |SÍ |
| AVI (sin comprimir de 8 bits/10 bits) |Sin  |SÍ |
| Formato de archivo de streaming con velocidad de transmisión adaptable (PIFF 1.3) |Sin  |SÍ |

## <a name="output-video-codecs"></a>Códecs de vídeo de salida
| Códecs de vídeo de salida | Media Encoder Estándar | Flujo de trabajo del Codificador multimedia |
| --- | --- | --- |
| AVC (H.264; 8 bits; hasta Perfil alto, Nivel 5.2; 4K Ultra HD; AVC Intra) |Solo 8 bits 4:2:0 |SÍ |
| HEVC (H.265; 8 bits y 10 bits;)  |Sin  |SÍ |
| Avid DNxHD (en MXF) |Sin  |SÍ |
| MPEG-2 (hasta 422 Perfil y Nivel alto; incluidas variantes como XDCAM, XDCAM HD, XDCAM IMX, CableLabs® y D10) |Sin  |SÍ |
| MPEG-1 |Sin  |SÍ |
| Windows Media Video/VC-1 |Sin  |SÍ |
| Creación de miniaturas JPEG |SÍ |SÍ |
| Creación de miniaturas PNG |SÍ |SÍ |
| Creación de miniaturas BMP |SÍ |Sin  |

## <a name="output-audio-codecs"></a>Códecs de audio de salida
| Códecs de audio de salida | Media Encoder Estándar | Flujo de trabajo del Codificador multimedia |
| --- | --- | --- |
| AES (SMPTE 331M y 302M, AES3-2003) |Sin  |SÍ |
| Dolby® Digital (AC3) |Sin  |SÍ |
| Dolby® Digital Plus (E-AC3) hasta 7.1 |Sin  |SÍ |
| AAC (AAC-LC, AAC-HE y AAC-HEv2; hasta 5.1) |SÍ |SÍ |
| MPEG Layer 2 |Sin  |SÍ |
| MP3 (MPEG-1 Audio Layer 3) |Sin  |SÍ |
| Windows Media Audio |Sin  |SÍ |

>[!NOTE]
>Si utiliza la codificación en Dolby® Digital (AC3), la salida solo puede escribirse en un archivo ISO MP4.

## <a name="media-services-learning-paths"></a>Rutas de aprendizaje de Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Artículos relacionados
* [Realización de tareas de codificación avanzadas mediante la personalización de valores preestablecidos de Media Encoder Estándar](media-services-custom-mes-presets-with-dotnet.md)
* [Cuotas y limitaciones](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: http://azure.microsoft.com/pricing/details/media-services/
