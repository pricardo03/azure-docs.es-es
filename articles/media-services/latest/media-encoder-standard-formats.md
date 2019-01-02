---
title: 'Códecs y formatos de Media Encoder Standard: Azure'
description: En este tema se ofrece información general sobre los códecs y formatos de Estándar de codificador multimedia.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2018
ms.author: juliako;anilmur
ms.openlocfilehash: 22603de40e2ebafb1354377898f5a0432e4f6129
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2018
ms.locfileid: "53388591"
---
# <a name="media-encoder-standard-formats-and-codecs"></a>Códecs y formatos de Media Encoder Standard

Este artículo contiene una lista de los formatos de archivo de importación y exportación más comunes que puede usar con [StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset). Para obtener información sobre cómo crear valores preestablecidos personalizados mediante **StandardEncoderPreset**, consulte [Creación de una transformación con un valor preestablecido personalizado](customize-encoder-presets-how-to.md).

## <a name="input-containerfile-formats"></a>Formatos de archivo/contenedor de entrada

| Formatos de archivo (extensiones de archivo) | Compatible |
| --- | --- | --- | --- |
| FLV (con códecs H.264 y AAC) (.flv) |SÍ |
| MXF    (.mxf) |SÍ |
| GXF    (.gxf) |SÍ |
| MPEG2-PS, MPEG2-TS, 3GP (.ts, .ps, .3gp, .3gpp, .mpg) |SÍ |
| Windows Media Video (WMV)/ASF (.wmv, .asf) |SÍ |
| AVI (sin comprimir de 8 bits/10 bits) (.avi) |SÍ |
| MP4 (.mp4, .m4a, .m4v)/ISMV (.isma, .ismv) |SÍ |
| [Microsoft Digital Video Recording(DVR-MS)](https://msdn.microsoft.com/library/windows/desktop/dd692984) (.dvr-ms) |SÍ |
| Matroska/WebM (.mkv) |SÍ |
| WAVE/WAV (.wav) |SÍ |
| QuickTime (.mov) |SÍ |

> [!NOTE]
> La lista anterior contiene las extensiones de archivo más habituales. El Codificador multimedia estándar admite muchos más (por ejemplo: .m2ts, .mpeg2video, .qt). Si intenta codificar un archivo y recibe un mensaje de error que indica que no se admite el formato, notifíquelo [aquí](https://feedback.azure.com/forums/169396-media-services/category/144411-encoding-and-processing/).
> 
> 

### <a name="audio-formats-in-input-containers"></a>Formatos de audio en contenedores de entrada
Media Encoder Standard admite el transporte de los siguientes formatos de audio en contenedores de entrada:

* Archivos MXF, GXF y QuickTime que tengan pistas de audio con muestras de estéreo entrelazado o 5.1

o

* Archivos MXF, GXF y QuickTime donde el audio se transporte como pistas PCM independientes, pero la asignación de canal (a estéreo o 5.1) se pueda deducir de los metadatos del archivo

## <a name="input-video-codecs"></a>Códecs de vídeo de entrada
| Códecs de vídeo de entrada | Compatible |
| --- | --- | --- | --- |
| AVC 8 bits/10 bits, hasta 4:2:2, incluido AVCIntra |8 bits: 4:2:0 y 4:2:2 |
| Avid DNxHD (en MXF) |SÍ |
| DVCPro/DVCProHD (en MXF) |SÍ |
| Vídeo digital (DV) (en archivos AVI) |SÍ |
| JPEG 2000 |SÍ |
| MPEG-2 (hasta 422 Perfil y Nivel alto; incluidas variantes como XDCAM, XDCAM HD, XDCAM IMX, CableLabs® y D10) |Hasta 422 Perfil |
| MPEG-1 |SÍ |
| VC-1/WMV9 |SÍ |
| Canopus HQ/HQX |Sin  |
| MPEG-4, parte 2 |SÍ |
| [Theora](https://en.wikipedia.org/wiki/Theora) |SÍ |
| YUV420 sin comprimir o intermedio |SÍ |
| Apple ProRes 422 |SÍ |
| Apple ProRes 422 LT |SÍ |
| Apple ProRes 422 HQ |SÍ |
| Apple ProRes Proxy |SÍ |
| Apple ProRes 4444 |SÍ |
| Apple ProRes 4444 XQ |SÍ |
| HEVC/H.265| Perfil Main|

## <a name="input-audio-codecs"></a>Códecs de audio de entrada
| Códecs de audio de entrada | Compatible |
| --- | --- | --- | --- |
| AAC (AAC-LC, AAC-HE y AAC-HEv2; hasta 5.1) |SÍ |
| MPEG Layer 2 |SÍ |
| MP3 (MPEG-1 Audio Layer 3) |SÍ |
| Windows Media Audio |SÍ |
| WAV/PCM |SÍ |
| [FLAC](https://en.wikipedia.org/wiki/FLAC)</a> |SÍ |
| [Opus](https://go.microsoft.com/fwlink/?LinkId=822667) |SÍ |
| [Vorbis](https://en.wikipedia.org/wiki/Vorbis)</a> |SÍ |
| AMR (velocidad múltiple adaptable) |SÍ |
| AES (SMPTE 331M y 302M, AES3-2003) |Sin  |
| Dolby® E |Sin  |
| Dolby® Digital (AC3) |Sin  |
| Dolby® Digital Plus (E-AC3) |Sin  |

## <a name="output-formats-and-codecs"></a>Códecs y formatos de salida
La siguiente tabla enumera los códecs y los formatos de archivo compatibles para exportación.

| Formato de archivo | Códec de vídeo | Códec de audio |
| --- | --- | --- |
| MP4  <br/><br/>(incluidos los contenedores de MP4 de velocidad de bits múltiple) |H.264 (perfil alto, perfil principal y perfil de base de línea) |AAC-LC, HE-AAC v1, HE-AAC v2 |
| MPEG2-TS |H.264 (perfil alto, perfil principal y perfil de base de línea) |AAC-LC, HE-AAC v1, HE-AAC v2 |

## <a name="next-steps"></a>Pasos siguientes

[Creación de una transformación con un valor preestablecido personalizado](customize-encoder-presets-how-to.md)
