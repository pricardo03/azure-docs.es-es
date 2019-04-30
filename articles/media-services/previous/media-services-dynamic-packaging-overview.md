---
title: Introducción al empaquetado dinámico de Azure Media Services | Microsoft Docs
description: El tema proporciona información general de empaquetado dinámico.
author: Juliako
manager: femila
editor: ''
services: media-services
documentationcenter: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: juliako
ms.openlocfilehash: e27b52c96f524f25aab18f45cf72c43884b7640d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61227187"
---
# <a name="dynamic-packaging"></a>Empaquetado dinámico

> [!div class="op_single_selector" title1="Select the version of Media Services that you are using:"]
> * [Versión 3](../latest/dynamic-packaging-overview.md)
> * [Versión 2](media-services-dynamic-packaging-overview.md)

Microsoft Azure Media Services puede usarse para proporcionar varios formatos de archivo de origen multimedia, formatos de streaming multimedia y formatos de protección de contenido a diversas tecnologías cliente (por ejemplo, iOS, XBOX, Silverlight y Windows 8). Estos clientes entienden distintos protocolos. Por ejemplo, iOS requiere un formato HTTP Live Streaming (HLS) V4, y Silverlight y Xbox requieren Smooth Streaming. Si tiene un conjunto de archivos MP4 (ISO Base Media 14496-12) de velocidad de bits adaptable (varias velocidades de bits) o un conjunto de archivos Smooth Streaming de velocidad de bits adaptable que desea ofrecer a los clientes que entienden MPEG DASH, HLS o Smooth Streaming, puede aprovechar el empaquetado dinámico de Media Services.

Con el empaquetado dinámico, necesita es crear un recurso que contenga un conjunto de archivos MP4 de velocidad de bits adaptable o archivos Smooth Streaming de velocidad de bits adaptable. Luego, según el formato especificado en la solicitud de manifiesto o fragmento, el servidor de streaming a petición se asegurará de que reciba la secuencia en el protocolo elegido. Como resultado, solo tendrá que almacenar y pagar los archivos en formato de almacenamiento único y Media Services creará y proporcionará la respuesta adecuada en función de las solicitudes de un cliente.

El diagrama siguiente muestra el flujo de trabajo de codificación y empaquetado estático tradicionales.

![Codificación estática](./media/media-services-dynamic-packaging-overview/media-services-static-packaging.png)

El diagrama siguiente muestra el flujo de trabajo de empaquetado dinámico.

![Codificación dinámica](./media/media-services-dynamic-packaging-overview/media-services-dynamic-packaging.png)

## <a name="common-scenario"></a>Escenario común

1. Cargar un archivo de entrada (llamado archivo intermedio). Por ejemplo, H.264, MP4 o WMV (para obtener la lista de formatos compatibles, vea [Formatos de Media Encoder Standard](media-services-media-encoder-standard-formats.md)).
2. Codificar el archivo intermedio en conjuntos MP4 de velocidad de bits adaptable H.264.
3. Publicar el recurso que contiene el conjunto MP4 de velocidad de bits adaptable mediante la creación del localizador a petición.
4. Compilar las direcciones URL de streaming para obtener acceso al contenido y hacer streaming de este.

## <a name="preparing-assets-for-dynamic-streaming"></a>Preparación de recursos para el streaming dinámico

Para preparar un recurso para el streaming dinámico, tiene las siguientes opciones:

- [Cargar un archivo maestro](media-services-dotnet-upload-files.md).
- [Usar el codificador Media Encoder Standard para producir conjuntos MP4 de velocidad de bits adaptable H.264](media-services-dotnet-encode-with-media-encoder-standard.md).
- [Transmita el contenido](media-services-deliver-content-overview.md).

## <a name="audio-codecs-supported-by-dynamic-packaging"></a>Códecs de audio compatibles con el empaquetado dinámico

Empaquetado dinámico admite archivos MP4 con audio codificado con [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) (AAC-LC, HE-AAC v1, HE-AAC v2), [Dolby Digital Plus](https://en.wikipedia.org/wiki/Dolby_Digital_Plus)(Enhanced AC-3 o E-AC3), Dolby Atmos, o [DTS](https://en.wikipedia.org/wiki/DTS_%28sound_system%29) (DTS Express, LBR DTS, DTS HD, HD DTS sin pérdida de datos). Streaming de contenido Dolby Atmos se admite para estándares como MP4 fragmentado de protocolo de MPEG-DASH con formato de Streaming comunes (CSF) o formato de aplicación comunes de medios (CMAF) y, a través de HTTP Live Streaming (HLS) con CMAF.

> [!NOTE]
> El empaquetado dinámico no admite archivos que contienen audio [Dolby Digital](https://en.wikipedia.org/wiki/Dolby_Digital) (AC3) (es un códec heredado).

## <a name="media-services-learning-paths"></a>Rutas de aprendizaje de Media Services

[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios

[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

