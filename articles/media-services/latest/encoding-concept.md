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
ms.date: 04/21/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: ed798995807f4037f0127b08e25e04bdd0340d42
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "62103618"
---
# <a name="encoding-with-media-services"></a>Codificación con Media Services

Azure Media Services le permite codificar los archivos multimedia digitales de alta calidad en archivos MP4 de velocidad de bits adaptable por lo que se puede reproducir el contenido en una amplia variedad de exploradores y dispositivos. Un trabajo de codificación correcta de Media Services crea una salida de un recurso con un conjunto de velocidad de bits adaptativa MP4s y archivos de configuración de transmisión por secuencias. Los archivos de configuración incluyen .ism, .ismc, .mpi y otros archivos que no se deben modificar. Una vez que se realiza el trabajo de codificación, puede sacar partido de [empaquetado dinámico](dynamic-packaging-overview.md) e iniciar la transmisión por secuencias.

Para crear vídeos en la salida activos disponibles para los clientes para la reproducción, tiene que crear un **localizador de Streaming** y generar direcciones URL de streaming. A continuación, según el formato especificado en el manifiesto, los clientes reciban la transmisión en el protocolo elegido.

El siguiente diagrama muestra el streaming a petición con el flujo de trabajo de empaquetado dinámico.

![Empaquetado dinámico](./media/dynamic-packaging-overview/media-services-dynamic-packaging.svg)

En este tema se proporcionan instrucciones sobre cómo codificar el contenido con Media Services v3.

## <a name="transforms-and-jobs"></a>Transformaciones y trabajos

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

- **EncoderNamedPreset.AACGoodQualityAudio**: genera un solo archivo MP4 que contiene únicamente audio estéreo codificado a 192 kbps.
- **EncoderNamedPreset.AdaptiveStreaming** (recomendado). Para más información, consulte [Generación automática de una escala de velocidad de bits](autogen-bitrate-ladder.md).
- **EncoderNamedPreset.ContentAwareEncodingExperimental** -expone un ajuste preestablecido experimental para la codificación según el contenido. Dado que cualquier contenido de entrada, el servicio intenta determinar automáticamente el número óptimo de capas, velocidad de bits adecuado y configuración de resolución para la entrega mediante transmisión por secuencias adaptativa. Los algoritmos subyacentes continuará evolucionando con el tiempo. La salida contendrá archivos MP4 con vídeo y audio intercalados. Para obtener más información, consulte [Experimental valor preestablecido de codificación compatible con contenido](cae-experimental.md).
- **EncoderNamedPreset.H264MultipleBitrate1080p** genera un conjunto de 8 archivos MP4 alineados con GOP, que abarcan desde 6000 kbps hasta 400 kbps y audio AAC estéreo. La resolución comienza en 1080p y baja hasta 360p.
- **EncoderNamedPreset.H264MultipleBitrate720p** genera un conjunto de 6 archivos MP4 alineados con GOP, que abarcan desde 3400 kbps hasta 400 kbps y audio AAC estéreo. La resolución comienza en 720p y baja hasta 360p.
- **EncoderNamedPreset.H264MultipleBitrateSD** genera un conjunto de 5 archivos MP4 alineados con GOP, que abarcan desde 1600 kbps hasta 400 kbps y audio AAC estéreo. La resolución comienza en 480p y baja hasta 360p.
- **EncoderNamedPreset.H264SingleBitrate1080p** -genera un archivo MP4, donde el vídeo codificado con códecs H.264 a 6750 kbps y un alto de imagen de 1080 píxeles y el audio estéreo codificado con el códec AAC-LC a 64 kbps.
- **EncoderNamedPreset.H264SingleBitrate720p** -genera un archivo MP4, donde el vídeo codificado con códecs H.264 a 4500 kbps y un alto de imagen de 720 píxeles y el audio estéreo codificado con el códec AAC-LC a 64 kbps.
- **EncoderNamedPreset.H264SingleBitrateSD** -genera un archivo MP4, donde el vídeo codificado con códecs H.264 a 2200 kbps y un alto de 480 píxeles de la imagen y el audio estéreo codificado con el códec AAC-LC a 64 kbps.

Para ver la lista más actualizada de los valores preestablecidos, consulte [valores preestablecidos integrados que se utilizará para codificar vídeos](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset).

Para ver cómo se usan los valores preestablecidos, consulte [cargando, codificación y streaming de archivos](stream-files-tutorial-with-api.md).

### <a name="standardencoderpreset-preset"></a>Valor preestablecido StandardEncoderPreset

[StandardEncoderPreset](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#standardencoderpreset) describe la configuración que se utilizará al codificar el vídeo de entrada con Standard Encoder. Utilice este valor preestablecido al personalizar los valores preestablecidos de transformación. 

#### <a name="considerations"></a>Consideraciones

Al crear valores preestablecidos personalizados, se aplican las consideraciones siguientes:

- Todos los valores correspondientes al alto y ancho del contenido de AVC deben ser un múltiplo de 4.
- En Azure Media Services v3, todas las velocidades de bits de codificación son en bits por segundo. Esto es diferente de los valores preestablecidos con nuestras API v2, que usa kilobits por segundo como la unidad. Por ejemplo, si la velocidad de bits en la versión 2 se especificó como 128 (kilobits/segundo), en v3 se establecería en 128000 (bits/segundo).

#### <a name="examples"></a>Ejemplos

Media Services es totalmente compatible con la personalización de todos los valores preestablecidos para que cubran sus necesidades y requisitos de codificación específicos. Para obtener ejemplos que muestran cómo personalizar los valores preestablecidos del codificador, vea:

- [Personalizar valores preestablecidos con .NET](customize-encoder-presets-how-to.md)
- [Personalizar valores preestablecidos con CLI](custom-preset-cli-howto.md)
- [Personalizar valores preestablecidos con REST](custom-preset-rest-howto.md)

## <a name="scaling-encoding-in-v3"></a>Escalado de la codificación en v3

Para escalar el procesamiento multimedia, consulte [escala con CLI](media-reserved-units-cli-how-to.md).

## <a name="provide-feedback"></a>Envío de comentarios

Consulte el artículo [Comunidad de Azure Media Services](media-services-community.md) para ver diferentes formas de formular preguntas, enviar comentarios y obtener actualizaciones de Media Services.

## <a name="next-steps"></a>Pasos siguientes

* [Codificar desde una dirección URL de HTTPS con los valores preestablecidos integrados](job-input-from-http-how-to.md)
* [Codificar un archivo local con los valores preestablecidos integrados](job-input-from-local-file-how-to.md)
* [Crear un valor predeterminado para sus requisitos específicos de escenario o dispositivo de destino personalizado](customize-encoder-presets-how-to.md)
* [Carga, codificación y transmisión con Media Services](stream-files-tutorial-with-api.md)
