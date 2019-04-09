---
title: Un experimental preestablecido para la codificación según el contenido - Azure | Microsoft Docs
description: Este artículo describe la codificación según el contenido en Azure Media Services
services: media-services
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/05/2019
ms.author: sethm
ms.custom: ''
ms.openlocfilehash: 3c50502a8b873503ee937914fac5f2d92cb23a2b
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288387"
---
# <a name="experimental-preset-for-content-aware-encoding"></a>Valor preestablecido de codificación compatible con el contenido de experimental

Con el fin de preparar el contenido para la entrega por [streaming de velocidad de bits adaptativa](https://en.wikipedia.org/wiki/Adaptive_bitrate_streaming), vídeo debe codificarse en varias velocidades de bits de (alto a bajo). Para garantizar una degradación correcta de calidad, como se reduce la velocidad de bits por lo que es la resolución del vídeo. Esto da como resultado una escalera de codificación llamada: una tabla de resoluciones y velocidades de bits, como puede ver en algunos de nuestros valores preestablecidos codificación fijos, como [H264MultipleBitrate1080p](../previous/media-services-mes-preset-h264-multiple-bitrate-1080p.md).

## <a name="overview"></a>Información general

Interés en migrar más allá de un enfoque de un valor preestablecido se adapta a todos los vídeos aumenta una vez publicado Netflix sus [blog](https://medium.com/netflix-techblog/per-title-encode-optimization-7e99442b62a2) en diciembre de 2015. Desde entonces, se han publicado varias soluciones para la codificación de contenido basadas en marketplace. consulte [en este artículo](https://www.streamingmedia.com/Articles/Editorial/Featured-Articles/Buyers-Guide-to-Per-Title-Encoding-130676.aspx) para obtener información general. La idea es tener en cuenta el contenido, personalizar o ajustar la escala de codificación a la complejidad del vídeo individual. En cada resolución, hay una velocidad de bits más allá del cual el aumento de calidad no es visión: el codificador funciona en este valor óptimo de la velocidad de bits. El siguiente nivel de optimización consiste en seleccionar las resoluciones en función del contenido: por ejemplo, un vídeo de una presentación de PowerPoint no se beneficia de ir a continuación 720p. Si continúa, el codificador puede encargarse de optimizar la configuración de cada captura dentro del vídeo. Netflix descrito [este enfoque](https://medium.com/netflix-techblog/optimized-shot-based-encodes-now-streaming-4b9464204830) en 2018.

Principios de 2017, Microsoft lanzó la [transmisión por secuencias adaptativa](autogen-bitrate-ladder.md) preestablecido para abordar el problema de la variabilidad de la calidad y la resolución de los vídeos de origen. Nuestros clientes tenían una mezcla diferente de contenido, algunos en 1080p, otros usuarios a 720p y poco a SD y resoluciones más bajas. Además, no todo el contenido de origen era mezzanines de alta calidad de la película o estudios de TV. El Streaming adaptativo direcciones preestablecidas estos problemas asegurándose de que la escalera nunca supera la resolución o la velocidad de bits Media de la entrada intermedio.

El valor preestablecido de codificación compatible con contenido experimental amplía ese mecanismo, incorporando una lógica personalizada que permite que el codificador buscar el valor óptimo de la velocidad de bits para una determinada resolución, pero sin necesidad de análisis exhaustivo de cálculo. El resultado neto es que este valor preestablecido nuevo genera una salida que tiene la menor velocidad de bits que el valor preestablecido de transmisión por secuencias adaptativa, pero con una calidad superior. Consulte los siguientes gráficos de ejemplo que muestran la comparación con las métricas de calidad como [PSNR](https://en.wikipedia.org/wiki/Peak_signal-to-noise_ratio) y [VMAF](https://en.wikipedia.org/wiki/Video_Multimethod_Assessment_Fusion). El origen se creó mediante la concatenación de clips cortos de capturas de gran complejidad de películas y programas de TV, se pretende enfatizar el codificador. Por definición, este valor predeterminado genera resultados de la que varían de contenido para contenido, también significa que para algún contenido, puede no haber reducción significativa de la velocidad de bits o mejora de calidad.

![Curva de distorsión de la velocidad (RD) mediante PSNR](media/cae-experimental/msrv1.png)

**Ilustración 1: Curva de distorsión de la velocidad (RD) con la métrica de PSNR para el origen de gran complejidad**

![Curva de distorsión de la velocidad (RD) mediante VMAF](media/cae-experimental/msrv2.png)

**Ilustración 2: Curva de distorsión de la velocidad (RD) con la métrica de VMAF para el origen de gran complejidad**

El valor predeterminado está optimizado actualmente para el alta complejidad, los vídeos de origen de alta calidad (películas, programas de TV). Trabajo está en curso para adaptarse al contenido de la complejidad baja (por ejemplo, presentaciones de PowerPoint), así como los vídeos de calidad más deficiente. Este valor preestablecido también usa el mismo conjunto de soluciones, como el Streaming adaptable preestablecido. Microsoft está trabajando en los métodos para seleccionar el conjunto mínimo de resoluciones en función del contenido. Son los siguientes resultados en otra categoría de contenido de origen, donde el codificador fue capaz de determinar que la entrada fue de baja calidad (muchos artefactos de compresión debido a la velocidad de bits baja). Tenga en cuenta que, con la instancia experimental, valor predeterminado, el codificador decidido generar solo una capa de salida: en una velocidad de bits lo suficientemente baja para que la mayoría de los clientes podrán reproducir la secuencia sin estancamiento.

![Curva de escritorio remoto mediante PSNR](media/cae-experimental/msrv3.png)

**Ilustración 3: Curva de escritorio remoto mediante PSNR para la entrada de baja calidad (a 1080p)**

![Curva de escritorio remoto mediante VMAF](media/cae-experimental/msrv4.png)

**Ilustración 4: Curva de escritorio remoto mediante VMAF para la entrada de baja calidad (a 1080p)**

## <a name="use-the-experimental-preset"></a>Utilice el valor preestablecido experimental

Puede crear transformaciones que usan esta programación como se indica a continuación. Si usa un tutorial [como este](stream-files-tutorial-with-api.md), puede actualizar el código como sigue:

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
> El prefijo "experimental" se usa aquí para indicar que los algoritmos subyacentes siguen evolucionando. Puede haber y serán los cambios con el tiempo para la lógica utilizada para generar escalas de velocidad de bits, con el objetivo de convergen en un algoritmo que es estable y se adapta a una amplia variedad de condiciones de entrada. Trabajos con este seguirá preestablecido de codificación se minutos de salida en función de facturación, y se puede entregar el recurso de salida desde nuestros puntos de conexión de streaming en protocolos, como DASH y HLS.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido acerca de esta nueva opción de optimización de los vídeos, le invitamos a probarlo. Puede enviarnos comentarios mediante los vínculos al final de este artículo, o ponerse en contacto nosotros más directamente en <amsved@microsoft.com>.
