---
title: Definición de filtros en Azure Media Services
description: En este tema se describe cómo crear filtros para que su cliente pueda usarlos para el streaming de secciones específicas de una secuencia. Media Services crea manifiestos dinámicos para lograr este streaming selectivo.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 03/20/2019
ms.author: juliako
ms.openlocfilehash: 61b877c322fcd58472990c328beea2e309502bce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60734582"
---
# <a name="define-account-filters-and-asset-filters"></a>Definición de filtros de cuenta y de recurso  

Al entregar su contenido a los clientes (eventos de Streaming en directo o vídeo bajo demanda) el cliente que necesite más flexibilidad que lo que se describe en el archivo de manifiesto del activo predeterminado. Azure Media Services le permite definir filtros de cuenta y filtros de recurso para su contenido. 

Los filtros son reglas del servidor que permitirán a los clientes a hacer cosas como: 

- Reproducir solo una sección de un vídeo (en lugar de reproducir todo el vídeo). Por ejemplo: 
  - Reduzca el manifiesto para mostrar un clip secundario de un evento en directo ("filtrado de vídeos secundarios"), o bien
  - Recorte el inicio de un vídeo ("recorte de un vídeo").
- Entregue únicamente las representaciones y pistas de idioma especificadas que admite el dispositivo que se usa para la reproducción del contenido ("filtrado de representaciones"). 
- Ajuste la ventana de presentación (DVR) para ofrecer una longitud limitada de la ventana de DVR en el reproductor ("ventana de presentación de ajuste").

Media Services ofrece [manifiestos dinámicos](filters-dynamic-manifest-overview.md) basados en filtros predefinidos. Una vez que defina los filtros, los clientes podrán utilizarlos en la dirección URL de streaming. Se podrían aplicar filtros a protocolos de streaming con velocidad de bits adaptable: formatos Apple HTTP Live Streaming (HLS), MPEG-DASH y Smooth Streaming

En la tabla siguiente se muestran algunos ejemplos de direcciones URL con filtros:

|Protocol|Ejemplo|
|---|---|
|HLS|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=m3u8-aapl,filter=myAccountFilter)`<br/>HLS v3, use: `format=m3u8-aapl-v3`.|
|MPEG DASH|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`https://amsv3account-usw22.streaming.media.azure.net/fecebb23-46f6-490d-8b70-203e86b0df58/bigbuckbunny.ism/manifest(filter=myAssetFilter)`|

## <a name="define-filters"></a>Definición de filtros

Hay dos tipos de filtros de activos: 

* [Filtros de cuenta](https://docs.microsoft.com/rest/api/media/accountfilters) (global): se pueden aplicar a cualquier recurso de la cuenta de Azure Media Services; tienen una duración de la cuenta.
* [Filtros de recursos](https://docs.microsoft.com/rest/api/media/assetfilters) (local): solo se pueden aplicar a un recurso con el que estaba asociado el filtro una vez creado; tienen una duración del recurso. 

Los tipos [Filtro de cuentas](https://docs.microsoft.com/rest/api/media/accountfilters) y [Filtro de recursos](https://docs.microsoft.com/rest/api/media/assetfilters) tienen exactamente las mismas propiedades para definir o describir el filtro. Excepto cuando se crea el **filtro de recursos**, deberá especificar el nombre del recurso con el que desea asociar el filtro.

En función del escenario, se decide qué tipo de filtro es el más adecuado (filtro de recursos o filtro de cuentas). Los filtros de cuenta son adecuados para los perfiles de dispositivos (filtrado de representaciones) donde los filtros de recursos podrían usarse para recortar un recurso específico.

Utilice las siguientes propiedades para describir los filtros. 

|NOMBRE|DESCRIPCIÓN|
|---|---|
|firstQuality|La velocidad de bits de primera calidad del filtro.|
|presentationTimeRange|El intervalo de tiempo de la presentación. Esta propiedad se utiliza para filtrar los puntos de inicio y de fin del manifiesto, la duración de la ventana de presentación y la posición de inicio activa. <br/>Para más información, consulte [PresentationTimeRange](#presentationtimerange).|
|tracks|Las condiciones de selección de pistas. Para más información, consulte [Pistas](#tracks).|

### <a name="presentationtimerange"></a>presentationTimeRange

Utilice esta propiedad con los **filtros de recursos**. No se recomienda establecer la propiedad con los **filtros de cuenta**.

|NOMBRE|DESCRIPCIÓN|
|---|---|
|**endTimestamp**|Se aplica al vídeo bajo demanda (VoD).<br/>Para la presentación de Streaming en vivo, se omite en modo silencioso y aplica cuando la presentación termina y el flujo se convierte en VoD.<br/>Se trata de un valor long que representa un punto final absoluto de la presentación, se redondea al inicio GOP siguiente más cercano. La unidad es la escala de tiempo, por lo que sería un endTimestamp de 1800000000 durante 3 minutos.<br/>Use startTimestamp y endTimestamp para recortar los fragmentos que se incluirán en la lista de reproducción (manifiesto).<br/>Por ejemplo, startTimestamp = 40000000 y endTimestamp = 100000000 utilizando la escala de tiempo de forma predeterminada, se generará una lista de reproducción que contiene fragmentos de entre 4 segundos y 10 segundos de la presentación de vídeo bajo demanda. Si un fragmento sobrepasa el límite, se incluirá todo el fragmento en el manifiesto.|
|**forceEndTimestamp**|Se aplica solo el streaming en vivo.<br/>Indica si la propiedad endTimestamp debe estar presente. Si es true, se debe especificar endTimestamp o se devuelve un código de solicitud incorrecta.<br/>Valores permitidos: false, true.|
|**liveBackoffDuration**|Se aplica solo el streaming en vivo.<br/> Este valor define la posición en vivo más reciente que puede buscar un cliente.<br/>Con esta propiedad, puede retrasar la posición de reproducción en directo y crear un búfer del lado servidor para reproductores.<br/>La unidad para esta propiedad es la escala de tiempo (ver abajo).<br/>El máximo en vivo de una interrupción de la duración es de 300 segundos (3000000000).<br/>Por ejemplo, un valor de 2000000000 significa que el contenido más reciente disponible es de 20 segundos se retrasa desde el borde en tiempo real.|
|**presentationWindowDuration**|Se aplica solo el streaming en vivo.<br/>Utilice presentationWindowDuration para aplicar una ventana deslizante de fragmentos para incluir en una lista de reproducción.<br/>La unidad para esta propiedad es la escala de tiempo (ver abajo).<br/>Por ejemplo, configure la presentaciónWindowDuration=1200000000 para aplicar una ventana deslizante de dos minutos. Los medios que se encuentren a menos de dos minutos del borde en directo se incluirán en la lista de reproducción. Si un fragmento sobrepasa el límite, se incluirá todo el fragmento en la lista de reproducción. La duración de la ventana de presentación mínima es de 60 segundos.|
|**startTimestamp**|Se aplica a vídeo bajo demanda (VoD) o Streaming en vivo.<br/>Se trata de un valor long que representa un punto inicial absoluto de la secuencia. El valor redondea al siguiente inicio del grupo de imágenes más cercano. La unidad es la escala de tiempo, por lo que sería un startTimestamp de 150000000 durante 15 segundos.<br/>Use startTimestamp y endTimestampp para recortar los fragmentos que se incluirán en la lista de reproducción (manifiesto).<br/>Por ejemplo, startTimestamp = 40000000 y endTimestamp = 100000000 utilizando la escala de tiempo de forma predeterminada, se generará una lista de reproducción que contiene fragmentos de entre 4 segundos y 10 segundos de la presentación de vídeo bajo demanda. Si un fragmento sobrepasa el límite, se incluirá todo el fragmento en el manifiesto.|
|**timescale**|Se aplica a todas las marcas de tiempo y las duraciones en un intervalo de tiempo de presentación, especificada como el número de incrementos en un segundo.<br/>Valor predeterminado es 10000000 - diez millones incrementos en un segundo, donde cada incremento sería mucho de 100 nanosegundos.<br/>Por ejemplo, si desea establecer un startTimestamp en 30 segundos, usaría un valor de 300000000 cuando se usa la escala de tiempo predeterminado.|

### <a name="tracks"></a>Pistas

Especifique una lista de condiciones de propiedad de seguimiento de filtro (FilterTrackPropertyConditions) basándose en el que se deben incluir las pistas de la secuencia (vídeo bajo demanda o Streaming en vivo) en el manifiesto creado dinámicamente. Los filtros se combinan mediante una operación lógica **AND** y **OR**.

Las condiciones de la propiedad de la pista del filtro describen tipos de pista, valores (descritos en la siguiente tabla) y operaciones (Equal, NotEqual). 

|NOMBRE|DESCRIPCIÓN|
|---|---|
|**Bitrate**|Utilice la velocidad de bits de la pista para filtrar.<br/><br/>El valor recomendado es un rango de velocidades de bits, en bits por segundo. Por ejemplo, "0-2427000".<br/><br/>Nota: aunque puede utilizar un valor de velocidad de bits específico, como 250000 (bits por segundo), no se recomienda este enfoque, ya que las velocidades de bits exactas pueden fluctuar de un recurso a otro.|
|**FourCC**|Utilice el valor FourCC de la pista para filtrar.<br/><br/>El valor es el primer elemento de formato de códecs, según se especifica en [RFC 6381](https://tools.ietf.org/html/rfc6381). Actualmente se admiten los siguientes códecs: <br/>Para vídeo: "avc1", "hev1", "hvc1"<br/>Para audio: "mp4a", "ec-3"<br/><br/>Para determinar los valores de FourCC de pistas en un recurso, obtener y examine el archivo de manifiesto.|
|**Lenguaje**|Utilice el idioma de la pista para filtrar.<br/><br/>El valor es la etiqueta de un idioma que desea incluir, como se especifica en RFC 5646. Por ejemplo, "en".|
|**Nombre**|Utilice el nombre de la pista para filtrar.|
|**Tipo**|Utilice el tipo de la pista para filtrar.<br/><br/>Se permiten los siguientes valores: "video", "audio" o "text".|

## <a name="associate-filters-with-streaming-locator"></a>Asociar filtros de localizador de Streaming

Puede especificar una lista de filtros de activo o una cuenta que se aplicaría a su localizador de Streaming. El [empaquetador dinámico](dynamic-packaging-overview.md) se aplica a esta lista de filtros junto con los que el cliente se especifica en la dirección URL. Esta combinación se genera un [dyanamic manifiesto](filters-dynamic-manifest-overview.md), que se basa en los filtros en la dirección URL y los filtros que especifique en el localizador de Streaming. Se recomienda usar esta característica si desea aplicar filtros pero no desea exponer los nombres de filtro en la dirección URL.

## <a name="definition-example"></a>Ejemplo de definición

```json
{
  "properties": {
    "presentationTimeRange": {
      "startTimestamp": 0,
      "endTimestamp": 170000000,
      "presentationWindowDuration": 9223372036854776000,
      "liveBackoffDuration": 0,
      "timescale": 10000000,
      "forceEndTimestamp": false
    },
    "firstQuality": {
      "bitrate": 128000
    },
    "tracks": [
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Audio"
          },
          {
            "property": "Language",
            "operation": "NotEqual",
            "value": "en"
          },
          {
            "property": "FourCC",
            "operation": "NotEqual",
            "value": "EC-3"
          }
        ]
      },
      {
        "trackSelections": [
          {
            "property": "Type",
            "operation": "Equal",
            "value": "Video"
          },
          {
            "property": "Bitrate",
            "operation": "Equal",
            "value": "3000000-5000000"
          }
        ]
      }
    ]
  }
}
```

## <a name="next-steps"></a>Pasos siguientes

Los artículos siguientes muestran cómo crear filtros mediante programación.  

- [Creación de filtros con las API REST](filters-dynamic-manifest-rest-howto.md)
- [Creación de filtros con .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Creación de filtros con la CLI](filters-dynamic-manifest-cli-howto.md)

