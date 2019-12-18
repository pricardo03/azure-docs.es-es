---
title: Valor preestablecido experimental para la codificación según el contenido - Azure | Microsoft Docs
description: En este artículo se describe la codificación según el contenido en Microsoft Azure Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/05/2019
ms.author: juliako
ms.custom: ''
ms.openlocfilehash: 9389466b6291542563c068706479bf981c5880da
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896138"
---
# <a name="experimental-preset-for-content-aware-encoding"></a>Valor preestablecido experimental para la codificación según el contenido

Para poder preparar el contenido para la entrega mediante [streaming con velocidad de bits adaptable](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), el vídeo debe codificarse en varias velocidades de bits (de alta a baja). Para garantizar una degradación correcta de la calidad, la resolución del vídeo disminuye a medida que lo hace la velocidad de bits. Esto tiene como resultado la denominada “escala de codificación”, que consiste en una tabla de resoluciones y velocidades de bits. Vea los [valores preestablecidos de codificación integrados](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#encodernamedpreset) de Media Services.

## <a name="overview"></a>Información general

El interés por encontrar un enfoque alternativo al uso de un solo valor preestablecido que se adaptara a todos los vídeos aumentó después de que Netflix publicara su [blog](https://medium.com/netflix-techblog/per-title-encode-optimization-7e99442b62a2) en diciembre de 2015. Desde entonces, se han publicado varias soluciones para la codificación según el contenido en Marketplace. Para obtener información general sobre este tema, vea [este artículo](https://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Buyers-Guide-to-Per-Title-Encoding-130676.aspx). La idea es tener en cuenta el contenido para personalizar o ajustar la escala de codificación en función de la complejidad de cada vídeo. A cada resolución le corresponde una velocidad de bits más allá de la cual no se percibe un aumento en la calidad: el codificador funciona en este valor de velocidad de bits óptimo. El siguiente nivel de optimización consiste en seleccionar las resoluciones en función del contenido: por ejemplo, un vídeo de una presentación de PowerPoint no saca ningún provecho al bajar de 720p. Más adelante, el codificador puede encargarse de optimizar la configuración de cada captura dentro del vídeo. Netflix describió [este enfoque](https://medium.com/netflix-techblog/optimized-shot-based-encodes-now-streaming-4b9464204830) en 2018.

A principios de 2017, Microsoft lanzó el valor preestablecido de [streaming adaptable](autogen-bitrate-ladder.md) para solucionar el problema de la variabilidad en la calidad y la resolución de los vídeos de origen. El contenido de nuestros clientes era muy variado: algunos vídeos tenían una resolución de 1080p, otros 720p, unos cuantos tenían una resolución de SD y otros una resolución inferior. Además, no todo el contenido de origen estaba en formato mezzanine de alta calidad y procedía de estudios de televisión o cinematográficos. El valor preestablecido de streaming adaptable soluciona estos problemas garantizando que la escala de velocidad de bits nunca supere la resolución o la velocidad de bits media del archivo mezzanine de entrada.

El valor preestablecido experimental de codificación en función del contenido amplía este mecanismo incorporando una lógica personalizada que permite al codificador buscar el valor óptimo de velocidad de bits para una resolución determinada, sin requerir un análisis de cálculo exhaustivo. Como resultado, este nuevo valor preestablecido genera una salida con una velocidad de bits inferior a la del valor preestablecido de streaming adaptable, pero una calidad superior. Consulte los siguientes gráficos de ejemplo en los que se muestra la comparación usando métricas de calidad como [PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio) y [VMAF](https://en.wikipedia.org/wiki/Video_Multimethod_Assessment_Fusion). El origen se creó mediante la concatenación de clips cortos de capturas de gran complejidad de películas y programas de TV con la finalidad de destacar el codificador. Por definición, este valor predeterminado genera resultados que varían de contenido en contenido. Esto también significa que, para cierto contenido, puede ser que no se produzca una reducción significativa de la velocidad de bits ni una mejora en la calidad.

![Curva de distorsión de la velocidad (RD) mediante PSNR](media/cae-experimental/msrv1.png)

**Figura 1: Curva de distorsión de la velocidad (RD) con la métrica de PSNR para un origen de gran complejidad**

![Curva de distorsión de la velocidad (RD) mediante VMAF](media/cae-experimental/msrv2.png)

**Figura 2: Curva de distorsión de la velocidad (RD) con la métrica de VMAF para un origen de gran complejidad**

Actualmente, el valor preestablecido está optimizado para vídeos de origen de gran complejidad y alta calidad (películas, programas de TV). Se está trabajando para adaptarlo a contenido de baja complejidad (por ejemplo, presentaciones de PowerPoint), así como a vídeos de menor calidad. Este valor preestablecido también usa el mismo conjunto de resoluciones que el valor preestablecido de streaming adaptable. Microsoft está trabajando en métodos para seleccionar el conjunto mínimo de resoluciones en función del contenido. A continuación encontrará los resultados de otra categoría de contenido de origen, en la que el codificador pudo determinar que la entrada era de baja calidad (muchos artefactos de compresión debido a una baja velocidad de bits). Tenga en cuenta que, con el valor preestablecido experimental, el codificador decidió generar solo una capa de salida a una velocidad de bits lo suficientemente baja para que la mayoría de los clientes pudieran reproducir la transmisión sin sufrir un estancamiento.

![Curva de RD mediante PSNR](media/cae-experimental/msrv3.png)

**Figura 3: Curva de RD mediante PSNR para entradas de baja calidad (a 1080p)**

![Curva de RD mediante VMAF](media/cae-experimental/msrv4.png)

**Ilustración 4: Curva de RD mediante VMAF para entradas de baja calidad (a 1080p)**

## <a name="use-the-experimental-preset"></a>Utilice el valor preestablecido experimental

Puede crear transformaciones que usen este valor preestablecido como se indica a continuación. Si usa un tutorial [como este](stream-files-tutorial-with-api.md), puede actualizar el código de la forma siguiente:

```csharp
TransformOutput[] output = new TransformOutput[]
{
   new TransformOutput
   {
      // The preset for the Transform is set to one of Media Services built-in sample presets.
      // You can customize the encoding settings by changing this to use "StandardEncoderPreset" class.
      Preset = new BuiltInStandardEncoderPreset()
      {
         // This sample uses the new experimental preset for content-aware encoding
         PresetName = EncoderNamedPreset.ContentAwareEncodingExperimental
      }
   }
};
```

> [!NOTE]
> El prefijo “experimental” se usa aquí para indicar que los algoritmos subyacentes siguen evolucionando. Es muy probable que, con el tiempo, se produzcan cambios en la lógica utilizada para generar escalas de velocidad de bits con el objetivo de converger en un algoritmo que sea estable y se adapte a una amplia variedad de condiciones de entrada. Los trabajos de codificación en los que se use este valor preestablecido se seguirán facturando en función de los minutos de salida y el recurso de salida se podrá entregar desde nuestros puntos de conexión de streaming en protocolos, como DASH y HLS.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha obtenido información sobre esta nueva opción de optimización de vídeos, le invitamos a probarla. Puede enviarnos comentarios usando los vínculos que encontrará al final de este artículo o poniéndose en contacto directamente con nosotros a través de <amsved@microsoft.com>.
