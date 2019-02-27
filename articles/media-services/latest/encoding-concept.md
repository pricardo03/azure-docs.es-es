---
title: 'Codificación en la nube con Media Services: Azure | Microsoft Docs'
description: Este tema describe el proceso de codificación con Azure Media Services
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/17/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 52e7fdf6de25300d4f78ee9822aca4ad83f646e9
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408432"
---
# <a name="encoding-with-media-services"></a>Codificación con Media Services

Azure Media Services permite codificar los archivos multimedia digitales de alta calidad en formatos que se pueden reproducir en una gran variedad de exploradores y dispositivos. Por ejemplo, puede que quiera transmitir su contenido en los formatos HLS o MPEG DASH de Apple. En este tema se proporcionan instrucciones sobre cómo codificar el contenido con Media Services v3.

Para codificar con Media Services v3, debe crear una [transformación](https://docs.microsoft.com/rest/api/media/transforms) y un [trabajo](https://docs.microsoft.com/rest/api/media/jobs). Una transformación define la receta para la configuración de la codificación y las salidas y el trabajo es una instancia de la receta. Para más información, consulte [Transformaciones y trabajos](transforms-jobs-concept.md)

Al codificar con Media Services, se usan valores preestablecidos para indicar al codificador cómo se deben procesar los archivos multimedia de entrada. Por ejemplo, puede especificar la resolución de vídeo o el número de canales de audio que desea en el contenido codificado. 

Puede empezar a trabajar rápidamente con alguno de los valores preestablecidos integrados recomendados basados en los procedimientos recomendados del sector o puede optar por crear un valor preestablecido destinado a su escenario específico o los requisitos de dispositivos. Para más información, vea [Procedimiento de codificación con una transformación personalizada](customize-encoder-presets-how-to.md). 

A partir de enero de 2019, al codificar con Media Encoder Standard para generar archivos MP4, se genera un archivo .mpi nuevo y se agrega a la salida de activos. Este archivo MPI está diseñado para mejorar el rendimiento de escenarios de streaming y [empaquetado dinámico](dynamic-packaging-overview.md).

> [!NOTE]
> No debe modificar ni quitar el archivo MPI, así como tampoco tener ninguna dependencia en el servicio en la existencia (o no) de este tipo de archivo.

## <a name="built-in-presets"></a>Valores preestablecidos integrados

Media Services admite actualmente los siguientes valores preestablecidos de codificación integrados:  

### <a name="builtinstandardencoderpreset-preset"></a>Valor preestablecido BuiltInStandardEncoderPreset

[BuiltInStandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#builtinstandardencoderpreset) se usa para establecer un valor preestablecido integrado para codificar el vídeo de entrada con Standard Encoder. 

Actualmente se admiten los valores preestablecidos siguientes:

- **EncoderNamedPreset.AdaptiveStreaming** (recomendado). Para más información, consulte [Generación automática de una escala de velocidad de bits](autogen-bitrate-ladder.md).
- **EncoderNamedPreset.AACGoodQualityAudio**: genera un solo archivo MP4 que contiene únicamente audio estéreo codificado a 192 kbps.
- **EncoderNamedPreset.H264MultipleBitrate1080p** genera un conjunto de 8 archivos MP4 alineados con GOP, que abarcan desde 6000 kbps hasta 400 kbps y audio AAC estéreo. La resolución comienza en 1080p y baja hasta 360p.
- **EncoderNamedPreset.H264MultipleBitrate720p** genera un conjunto de 6 archivos MP4 alineados con GOP, que abarcan desde 3400 kbps hasta 400 kbps y audio AAC estéreo. La resolución comienza en 720p y baja hasta 360p.
- **EncoderNamedPreset.H264MultipleBitrateSD** genera un conjunto de 5 archivos MP4 alineados con GOP, que abarcan desde 1600 kbps hasta 400 kbps y audio AAC estéreo. La resolución comienza en 480p y baja hasta 360p.<br/><br/>Para más información, consulte [Carga, codificación y streaming de archivos](stream-files-tutorial-with-api.md).

### <a name="standardencoderpreset-preset"></a>Valor preestablecido StandardEncoderPreset

[StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) describe la configuración que se utilizará al codificar el vídeo de entrada con Standard Encoder. Utilice este valor preestablecido al personalizar los valores preestablecidos de transformación. 

#### <a name="custom-presets"></a>Valores preestablecidos personalizados

Media Services es totalmente compatible con la personalización de todos los valores preestablecidos para que cubran sus necesidades y requisitos de codificación específicos. El valor preestablecido **StandardEncoderPreset** se usa al personalizar los valores preestablecidos de transformación. Para una explicación detallada y un ejemplo, consulte [Personalización de valores preestablecidos del codificador](customize-encoder-presets-how-to.md).

## <a name="scaling-encoding-in-v3"></a>Escalado de la codificación en v3

Actualmente, los clientes deben usar Azure Portal o las API de Media Services v2 para establecer las RU (como se describe en [Escalado del procesamiento multimedia](../previous/media-services-scale-media-processing-overview.md)). 

## <a name="next-steps"></a>Pasos siguientes

* [Transformaciones y trabajos](transforms-jobs-concept.md)
* [Carga, codificación y transmisión con Media Services](stream-files-tutorial-with-api.md)
