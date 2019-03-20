---
title: Introducción y comparación de codificadores multimedia a petición de Azure | Microsoft Docs
description: En este tema se proporciona información general y una comparación de los codificadores multimedia a petición de Azure.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.assetid: e6bfc068-fa46-4d68-b1ce-9092c8f3a3c9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: juliako
ms.openlocfilehash: a1b6fe38087975838d99ca4fe45ee0d0638c56c3
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58187751"
---
# <a name="overview-and-comparison-of-azure-on-demand-media-encoders"></a>Información general y comparación de codificadores multimedia a petición de Azure 

## <a name="encoding-overview"></a>Información general sobre la codificación
Azure Media Services ofrece varias opciones para la codificación de medios en la nube.

Cuando comience con Media Services, es importante comprender la diferencia entre códecs y formatos de archivo.
Los códecs son el software que implementa los algoritmos de compresión/descompresión, mientras que los formatos de archivo son contenedores que contienen el vídeo comprimido.

Media Services proporciona empaquetado dinámico que permite entregar contenido codificado MP4 de velocidad de bits adaptable o Smooth Streaming en formatos de streaming admitidos por Media Services (MPEG-DASH, HLS y Smooth Streaming) sin tener que volver a realizar el empaquetamiento en estos formatos de streaming.

> [!NOTE]
> Cuando se crea la cuenta de AMS, se agrega un punto de conexión de streaming **predeterminado** a la cuenta en estado **Stopped** (Detenido). Para iniciar la transmisión del contenido y aprovechar el empaquetado dinámico y el cifrado dinámico, el punto de conexión de streaming desde el que va a transmitir el contenido debe estar en estado **Running** (En ejecución). 

Media Services admite los siguientes codificadores a petición que se describen en este artículo:

* [Media Encoder Estándar](media-services-encode-asset.md#media-encoder-standard)
* [Flujo de trabajo del Codificador multimedia](media-services-encode-asset.md#media-encoder-premium-workflow)

En este artículo se ofrece una breve introducción a los codificadores multimedia a petición y se proporcionan vínculos a artículos con información más detallada. También se proporciona una comparación de los codificadores.

>[!NOTE]
>De forma predeterminada cada cuenta de Media Services puede tener una tarea de codificación activa a la vez. Puede reservar unidades de codificación que permiten la ejecución simultánea de varias tareas de codificación, una para cada unidad reservada de codificación que ha adquirido. Para obtener información, consulte [Escalado de unidades de codificación](media-services-scale-media-processing-overview.md).

## <a name="media-encoder-standard"></a>Media Encoder Estándar
### <a name="how-to-use"></a>Modo de uso
[Codificación con Codificador multimedia estándar](media-services-dotnet-encode-with-media-encoder-standard.md)

### <a name="formats"></a>Formatos
[Códecs y formatos](media-services-media-encoder-standard-formats.md)

### <a name="presets"></a>Valores preestablecidos
Codificador multimedia estándar se configura mediante uno de los valores preestablecidos descritos [aquí](https://go.microsoft.com/fwlink/?linkid=618336&clcid=0x409).

### <a name="input-and-output-metadata"></a>Metadatos de entrada y salida
[Aquí](media-services-input-metadata-schema.md)se describen los metadatos de entrada de los codificadores.

[Aquí](media-services-output-metadata-schema.md)se describen los metadatos de salida de los codificadores.

### <a name="generate-thumbnails"></a>Generación de miniaturas
Para más información, consulte [Generación de miniaturas](media-services-advanced-encoding-with-mes.md#thumbnails).

### <a name="trim-videos-clipping"></a>Recorte de vídeos
Para más información, consulte [Recorte de un vídeo](media-services-advanced-encoding-with-mes.md#trim_video).

### <a name="create-overlays"></a>Creación de superposiciones
Para más información, consulte [Creación de una superposición](media-services-advanced-encoding-with-mes.md#overlay).

### <a name="see-also"></a>Vea también
[El blog de Media Services](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/)

## <a name="media-encoder-premium-workflow"></a>Flujo de trabajo del Codificador multimedia
### <a name="overview"></a>Información general
[Introducción de la codificación Premium en Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/)

### <a name="how-to-use"></a>Modo de uso
El flujo de trabajo del Codificador multimedia Premium se configura mediante flujos de trabajo complejos. Los archivos de flujo de trabajo pueden crearse y actualizarse con la herramienta [Diseñador de flujo de trabajo](media-services-workflow-designer.md) .

[Uso de la codificación Premium en Azure Media Services](https://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services/)

### <a name="known-issues"></a>Problemas conocidos
Si el vídeo de entrada no contiene subtítulos, el recurso de salida seguirá conteniendo un archivo TTML vacío.


## <a name="media-services-learning-paths"></a>Rutas de aprendizaje de Media Services
[!INCLUDE [media-services-learning-paths-include](../../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Envío de comentarios
[!INCLUDE [media-services-user-voice-include](../../../includes/media-services-user-voice-include.md)]

## <a name="related-articles"></a>Artículos relacionados
* [Realización de tareas de codificación avanzadas mediante la personalización de valores preestablecidos de Media Encoder Estándar](media-services-custom-mes-presets-with-dotnet.md)
* [Cuotas y limitaciones](media-services-quotas-and-limitations.md)

<!--Reference links in article-->
[1]: https://azure.microsoft.com/pricing/details/media-services/
