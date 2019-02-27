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
ms.date: 02/12/2019
ms.author: juliako
ms.openlocfilehash: 09de372ffdb48c00fde9a43c07f8f8b574462d1f
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2019
ms.locfileid: "56372963"
---
# <a name="define-account-filters-and-asset-filters"></a>Definición de filtros de cuenta y de recurso  

Al entregar su contenido a los clientes (streaming de eventos en directo o vídeo bajo demanda), es posible que el cliente necesite más flexibilidad que la descrita en el archivo de manifiesto del recurso predeterminado. Azure Media Services le permite definir filtros de cuenta y filtros de recurso para su contenido. 

Los filtros son reglas del servidor que permitirán a los clientes a hacer cosas como: 

- Reproducir solo una sección de un vídeo (en lugar de reproducir todo el vídeo). Por ejemplo: 
  - Reduzca el manifiesto para mostrar un clip secundario de un evento en directo ("filtrado de vídeos secundarios"), o bien
  - Recorte el inicio de un vídeo ("recorte de un vídeo").
- Entregue únicamente las representaciones y pistas de idioma especificadas que admite el dispositivo que se usa para la reproducción del contenido ("filtrado de representaciones"). 
- Ajuste la ventana de presentación (DVR) para ofrecer una longitud limitada de la ventana de DVR en el reproductor ("ventana de presentación de ajuste").

Media Services ofrece [manifiestos dinámicos](filters-dynamic-manifest-overview.md) basados en filtros predefinidos. Una vez que defina los filtros, los clientes podrán utilizarlos en la dirección URL de streaming. Se podrían aplicar filtros a protocolos de streaming con velocidad de bits adaptable: formatos Apple HTTP Live Streaming (HLS), MPEG-DASH y Smooth Streaming

En la tabla siguiente se muestran algunos ejemplos de direcciones URL con filtros:

|Protocolo|Ejemplo|
|---|---|
|HLS V4|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|HLS V3|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,filter=myAccountFilter)`|
|MPEG DASH|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=myAssetFilter)`|

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
|presentationTimeRange|El intervalo de tiempo de la presentación. Esta propiedad se utiliza para filtrar los puntos de inicio y de fin del manifiesto, la duración de la ventana de presentación y la posición de inicio activa. <br/>Para más información, consulte [PresentationTimeRange](#PresentationTimeRange).|
|tracks|Las condiciones de selección de pistas. Para más información, consulte [Pistas](#tracks).|

### <a name="presentationtimerange"></a>PresentationTimeRange

Utilice esta propiedad con los **filtros de recursos**. No se recomienda establecer la propiedad con los **filtros de cuenta**.

|NOMBRE|DESCRIPCIÓN|
|---|---|
|**endTimestamp**|El límite de tiempo de finalización absoluto. Se aplica al vídeo bajo demanda (VoD). Para la presentación en directo, se ignora silenciosamente y se aplica cuando la presentación termina y la secuencia se convierte en VoD.<br/><br/>El valor representa un punto final absoluto de la secuencia. Se redondea al siguiente inicio del grupo de imágenes más cercano.<br/><br/>Use StartTimestamp y EndTimestamp para recortar la lista de reproducción (manifiesto). Por ejemplo, StartTimestamp=40000000 y EndTimestamp = 100000000 generarán una lista de reproducción que contiene medios entre StartTimestamp y EndTimestamp. Si un fragmento sobrepasa el límite, se incluirá todo el fragmento en el manifiesto.<br/><br/>También, consulte la definición de **forceEndTimestamp** que sigue.|
|**forceEndTimestamp**|Se aplica a los filtros activos.<br/><br/>**forceEndTimestamp** es un booleano que indica si **endTimestamp** se estableció o no en un valor válido. <br/><br/>Si el valor es **true**, debe especificarse el valor **endTimestamp**. Si no se especifica, se devuelve una solicitud incorrecta.<br/><br/>Si, por ejemplo, desea definir un filtro que comience a los cinco minutos de la entrada de vídeo y que dure hasta el final de la secuencia, debe establecer **forceEndTimestamp** en false y omitir la configuración de **endTimestamp**.|
|**liveBackoffDuration**|Solo se aplica los filtros activos. La propiedad se utiliza para definir la posición de reproducción en vivo. Con esta regla, se puede retrasar la posición de reproducción en directo y crear un búfer en el servidor para los reproductores. LiveBackoffDuration está relacionado con la posición activa. La duración de retroceso en directo máxima es de 300 segundos.|
|**presentationWindowDuration**|Se aplica a los filtros activos. Utilice **presentationWindowDuration** para aplicar una ventana deslizante a la lista de reproducción. Por ejemplo, configure la presentaciónWindowDuration=1200000000 para aplicar una ventana deslizante de dos minutos. Los medios que se encuentren a menos de dos minutos del borde en directo se incluirán en la lista de reproducción. Si un fragmento sobrepasa el límite, se incluirá todo el fragmento en la lista de reproducción. La duración de la ventana de presentación mínima es de 60 segundos.|
|**startTimestamp**|Se aplica al VoD o al streaming en vivo. El valor representa un punto inicial absoluto de la secuencia. El valor redondea al siguiente inicio del grupo de imágenes más cercano.<br/><br/>Use **StartTimestamp** y **EndTimestamp** para recortar la lista de reproducción (manifiesto). Por ejemplo, startTimestamp=40000000 y endTimestamp = 100000000 generarán una lista de reproducción que contiene medios entre StartTimestamp y EndTimestamp. Si un fragmento sobrepasa el límite, se incluirá todo el fragmento en el manifiesto.|
|**timescale**|Se aplica al VoD o al streaming en vivo. La escala temporal utilizada por las marcas de tiempo y duraciones especificadas anteriormente. La escala temporal predeterminada es 10000000. Se puede usar una escala temporal alternativa. El valor predeterminado es 10000000 HNS (cien nanosegundos).|

### <a name="tracks"></a>Pistas

Se especifica una lista de condiciones de propiedad de pista de filtro (FilterTrackPropertyConditions) en función de las pistas de la secuencia (vídeo en directo o bajo demanda) que deben incluirse en el manifiesto creado dinámicamente. Los filtros se combinan mediante una operación lógica **AND** y **OR**.

Las condiciones de la propiedad de la pista del filtro describen tipos de pista, valores (descritos en la siguiente tabla) y operaciones (Equal, NotEqual). 

|NOMBRE|DESCRIPCIÓN|
|---|---|
|**Bitrate**|Utilice la velocidad de bits de la pista para filtrar.<br/><br/>El valor recomendado es un rango de velocidades de bits, en bits por segundo. Por ejemplo, "0-2427000".<br/><br/>Nota: aunque puede utilizar un valor de velocidad de bits específico, como 250000 (bits por segundo), no se recomienda este enfoque, ya que las velocidades de bits exactas pueden fluctuar de un recurso a otro.|
|**FourCC**|Utilice el valor FourCC de la pista para filtrar.<br/><br/>El valor es el primer elemento de formato de códecs, según se especifica en [RFC 6381](https://tools.ietf.org/html/rfc6381). Actualmente se admiten los siguientes códecs: <br/>Para vídeo: "avc1", "hev1", "hvc1"<br/>Para audio: "mp4a", "ec-3"<br/><br/>Para determinar los valores de FourCC para las pistas de un recurso, [obtenga y examine el archivo de manifiesto](#get-and-examine-manifest-files).|
|**Lenguaje**|Utilice el idioma de la pista para filtrar.<br/><br/>El valor es la etiqueta de un idioma que desea incluir, como se especifica en RFC 5646. Por ejemplo, "en".|
|**Nombre**|Utilice el nombre de la pista para filtrar.|
|**Tipo**|Utilice el tipo de la pista para filtrar.<br/><br/>Se permiten los siguientes valores: "video", "audio" o "text".|

## <a name="example"></a>Ejemplo

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

