---
title: Codificación de transformación personalizado con Media Services v3 NET - Azure | Microsoft Docs
description: Este tema muestra cómo usar Azure Media Services v3 para codificar una transformación personalizada mediante. NET.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 03/11/2019
ms.author: juliako
ms.openlocfilehash: 848da2996b71b137c6112225c9bef7e93b457c7d
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57837242"
---
# <a name="how-to-encode-with-a-custom-transform-by-using-net"></a>Cómo codificar con una transformación personalizada mediante el uso de .NET

Al codificar con Azure Media Services, puede empezar a trabajar rápidamente con uno de los valores de preestablecidos integrados recomendados basados en procedimientos recomendados del sector, como se muestra en el [Streaming archivos](stream-files-tutorial-with-api.md) tutorial. También puede crear un valor predeterminado para sus requisitos específicos de escenario o dispositivo de destino personalizado.

## <a name="considerations"></a>Consideraciones

Al crear valores preestablecidos personalizados, se aplican las consideraciones siguientes:

* Todos los valores correspondientes al alto y ancho del contenido de AVC deben ser un múltiplo de 4.
* En Azure Media Services v3, todas las velocidades de bits de codificación son en bits por segundo. Esto es diferente de los valores preestablecidos con nuestras API v2, que usa kilobits por segundo como la unidad. Por ejemplo, si la velocidad de bits en la versión 2 se especificó como 128 (kilobits/segundo), en v3 se establecería en 128000 (bits/segundo).

## <a name="prerequisites"></a>Requisitos previos 

[Cree una cuenta de Media Services](create-account-cli-how-to.md). <br/>Asegúrese de recordar el nombre del grupo de recursos y el nombre de la cuenta de Media Services. 

## <a name="download-the-sample"></a>Descarga del ejemplo

Clone un repositorio GitHub que contenga el ejemplo de .NET Core completo en la máquina mediante el siguiente comando:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials.git
 ```
 
El ejemplo de valor preestablecido personalizado se encuentra en la carpeta [EncodeCustomTransform](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/blob/master/NETCore/EncodeCustomTransform/).

## <a name="create-a-transform-with-a-custom-preset"></a>Creación de una transformación con un valor preestablecido personalizado 

Al crear una nueva [transformación](https://docs.microsoft.com/rest/api/media/transforms), debe especificar qué desea originar como salida. El parámetro requerido es un objeto [TransformOutput](https://docs.microsoft.com/rest/api/media/transforms/createorupdate#transformoutput), como se muestra en el código siguiente. Cada objeto **TransformOutput** contiene un **valor preestablecido**. El **valor preestablecido** describe las instrucciones paso a paso de las operaciones de procesamiento de vídeo o audio que se utilizarán para generar el objeto **TransformOutput** deseado. El siguiente objeto **TransformOutput** crea el códec personalizado y la configuración de salida de la capa.

Al crear una [transformación](https://docs.microsoft.com/rest/api/media/transforms), debe comprobar primero si ya existe una con el método **Get**, tal como se muestra en el código siguiente. En Media Services v3, los métodos **Get** en las entidades devuelven **null** si la entidad no existe (comprobación sin distinción entre mayúsculas y minúsculas en el nombre).

### <a name="example"></a>Ejemplo

El ejemplo siguiente define un conjunto de resultados que se va a generarse cuando se usa esta transformación. En primer lugar agregamos una capa AacAudio para la codificación de audio y dos capas de H264Video para la codificación de vídeo. En las capas de vídeo, asignamos las etiquetas para que se pueden usar en los nombres de archivo de salida. A continuación, queremos que la salida para incluir también las miniaturas. En el ejemplo siguiente se especifican las imágenes en formato PNG, generada al 50% de la resolución de la entrada de vídeo y en tres marcas de tiempo - {25%, 50%, 75} de la longitud de la entrada de vídeo. Por último, especificamos el formato de los archivos de salida: uno para vídeo y audio y otro para las vistas en miniatura. Puesto que tenemos varios H264Layers, tenemos que usar las macros que generan nombres únicos por nivel. Se puede usar un `{Label}` o `{Bitrate}` macro, el ejemplo muestra la primera de ellas.

[!code-csharp[Main](../../../media-services-v3-dotnet-core-tutorials/NETCore/EncodeCustomTransform/MediaV3ConsoleApp/Program.cs#EnsureTransformExists)]

## <a name="next-steps"></a>Pasos siguientes

[Streaming de archivos](stream-files-tutorial-with-api.md) 
