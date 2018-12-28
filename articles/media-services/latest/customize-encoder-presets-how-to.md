---
title: 'Codificación de una transformación personalizada con Media Services v3: Azure | Microsoft Docs'
description: Este tema muestra cómo usar Azure Media Services v3 para codificar una transformación personalizada.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 12/08/2018
ms.author: juliako
ms.openlocfilehash: c62d9132cdd7eb2ebcbecc3c417ad30d368a278a
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53138711"
---
# <a name="how-to-encode-with-a-custom-transform"></a>Procedimiento de codificación con una transformación personalizada

Al codificar con Azure Media Services, puede empezar a trabajar rápidamente con alguno de los valores preestablecidos integrados recomendados basados en los procedimientos recomendados del sector como se muestra en el tutorial [Streaming de archivos](stream-files-tutorial-with-api.md) o puede optar por crear un valor preestablecido destinado a su escenario específico o los requisitos de dispositivos. 

> [!Note]
> En Azure Media Services v3, todas las velocidades de bits de codificación se expresan en bits por segundo. Esto es diferente a los valores preestablecidos de Media Encoder Standard v2 de REST. Por ejemplo, la velocidad de bits en v2 se especificaría como 128, mientras que en v3 sería 128000.

## <a name="download-the-sample"></a>Descarga del ejemplo

Clone un repositorio GitHub que contenga el ejemplo de .NET Core completo en la máquina mediante el siguiente comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
El ejemplo de valor preestablecido personalizado se encuentra en la carpeta [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/).

## <a name="create-a-transform-with-a-custom-preset"></a>Creación de una transformación con un valor preestablecido personalizado 

Al crear una nueva [transformación](https://docs.microsoft.com/rest/api/media/transforms), debe especificar qué desea originar como salida. El parámetro requerido es un objeto **TransformOutput**, como se muestra en el código siguiente. Cada objeto **TransformOutput** contiene un **valor preestablecido**. El **valor preestablecido** describe las instrucciones paso a paso de las operaciones de procesamiento de vídeo o audio que se utilizarán para generar el objeto **TransformOutput** deseado. El siguiente objeto **TransformOutput** crea el códec personalizado y la configuración de salida de la capa.

Al crear una [transformación](https://docs.microsoft.com/rest/api/media/transforms), debe comprobar primero si ya existe una con el método **Get**, tal como se muestra en el código siguiente.  En Media Services v3, los métodos **Get** en las entidades devuelven **null** si la entidad no existe (comprobación sin distinción entre mayúsculas y minúsculas en el nombre).

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Pasos siguientes

[Streaming de archivos](stream-files-tutorial-with-api.md) 
