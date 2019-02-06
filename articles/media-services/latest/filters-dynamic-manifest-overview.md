---
title: Filtros y manifiestos dinámicos de Azure Media Services | Microsoft Docs
description: En este tema se describe cómo crear filtros para que su cliente pueda usarlos para el streaming de secciones específicas de una secuencia. Media Services crea manifiestos dinámicos par lograr este streaming selectivo.
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
ms.date: 01/24/2019
ms.author: juliako
ms.openlocfilehash: 5b666551ed47852fe8653fff174589acc4bff348
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2019
ms.locfileid: "54912040"
---
# <a name="filters-and-dynamic-manifests"></a>Filtros y manifiestos dinámicos

Al entregar su contenido a los clientes (streaming de eventos en directo o vídeo bajo demanda), es posible que el cliente necesite más flexibilidad que la descrita en el archivo de manifiesto del recurso predeterminado. Azure Media Services le permite definir filtros de cuenta y filtros de recurso para su contenido. 

Los filtros son reglas del servidor que permitirán a los clientes a hacer cosas como: 

- Reproducir solo una sección de un vídeo (en lugar de reproducir todo el vídeo). Por ejemplo: 

    - Reduzca el manifiesto para mostrar un clip secundario de un evento en directo ("filtrado de vídeos secundarios") o
    - Recorte el inicio de un vídeo ("recorte de un vídeo").

- Entregue únicamente las representaciones y pistas de idioma especificadas que admite el dispositivo que se usa para la reproducción del contenido ("filtrado de representaciones"). 
- Ajuste la ventana de presentación (DVR) para ofrecer una longitud limitada de la ventana de DVR en el reproductor ("ventana de presentación de ajuste").

En este tema se describen [conceptos](#concepts) y [muestra las definiciones de filtros](#definitions). También ofrece información sobre [escenarios comunes](#common-scenarios). Al final del artículo, encontrará vínculos que muestran cómo crear filtros mediante programación.  

## <a name="concepts"></a>Conceptos

### <a name="dynamic-manifests"></a>Manifiestos dinámicos

Media Services ofrece **manifiestos dinámicos** basados en [filtros](#filters)predefinidos. Cuando defina los filtros, los clientes podrían usarlos para transmitir una representación específica o clips secundarios del vídeo. Especificarían filtros en la URL de streaming. Se podrían aplicar filtros a protocolos de streaming con velocidad de bits adaptable: formatos Apple HTTP Live Streaming (HLS), MPEG-DASH y Smooth Streaming 

En la tabla siguiente se muestran algunos ejemplos de direcciones URL con filtros:

|Protocolo|Ejemplo|
|---|---|
|HLS V4|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl,filter=myAccountFilter)`|
|HLS V3|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=m3u8-aapl-v3,filter=myAccountFilter)`|
|MPEG DASH|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(format=mpd-time-csf,filter=myAssetFilter)`|
|Smooth Streaming|`http://testendpoint-testaccount.streaming.mediaservices.windows.net/fecebb23-46f6-490d-8b70-203e86b0df58/BigBuckBunny.ism/Manifest(filter=myAssetFilter)`|

> [!NOTE]
> Los manifiestos dinámicos no cambian el recurso y el manifiesto predeterminado para ese recurso. Su cliente puede elegir solicitar una secuencia con o sin filtros. 
> 
> 

### <a name="manifest-files"></a>Archivos de manifiesto

Cuando codifique un activo para transmisión por secuencias de velocidad de bits adaptativa, se crea un archivo de **manifiesto** (lista de reproducción) (el archivo se basa en texto o XML). El archivo de **manifiesto** incluye metadatos de transmisión por secuencias como: el tipo de pista (audio, vídeo o texto), el nombre de la pista, la hora inicial y final, la velocidad de bits (calidades), los idiomas de pista, la ventana de presentación (ventana deslizante de duración fija), el códec de vídeo (FourCC). También indica al reproductor que recupere el siguiente fragmento ofreciendo información sobre los próximos fragmentos de vídeo reproducibles disponibles y su ubicación. Los fragmentos (o segmentos) son "fragmentos" reales de un contenido de vídeo.

Este es un ejemplo de un archivo de manifiesto HLS: 

```
#EXT-X-MEDIA:TYPE=AUDIO,GROUP-ID="audio",NAME="aac_eng_2_128041_2_1",LANGUAGE="eng",DEFAULT=YES,AUTOSELECT=YES,URI="QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)"
#EXT-X-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d,mp4a.40.2",AUDIO="audio"
QualityLevels(380658)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=536209,RESOLUTION=320x180,CODECS="avc1.64000d",URI="QualityLevels(380658)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015,mp4a.40.2",AUDIO="audio"
QualityLevels(721426)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=884474,RESOLUTION=480x270,CODECS="avc1.640015",URI="QualityLevels(721426)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e,mp4a.40.2",AUDIO="audio"
QualityLevels(1155246)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=1327838,RESOLUTION=640x360,CODECS="avc1.64001e",URI="QualityLevels(1155246)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f,mp4a.40.2",AUDIO="audio"
QualityLevels(2218559)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=2414544,RESOLUTION=960x540,CODECS="avc1.64001f",URI="QualityLevels(2218559)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020,mp4a.40.2",AUDIO="audio"
QualityLevels(3579378)/Manifest(video,format=m3u8-aapl)
#EXT-X-I-FRAME-STREAM-INF:BANDWIDTH=3805301,RESOLUTION=1280x720,CODECS="avc1.640020",URI="QualityLevels(3579378)/Manifest(video,format=m3u8-aapl,type=keyframes)"
#EXT-X-STREAM-INF:BANDWIDTH=139017,CODECS="mp4a.40.2",AUDIO="audio"
QualityLevels(128041)/Manifest(aac_eng_2_128041_2_1,format=m3u8-aapl)
```

### <a name="get-and-examine-manifest-files"></a>Obtención y examen de archivos de manifiesto

Se especifica una lista de condiciones de propiedad de pista de filtro en función de las pistas de la secuencia (vídeo en directo o bajo demanda) que deben incluirse en el manifiesto creado dinámicamente. Para obtener y examinar las propiedades de las pistas, primero debe cargar el manifiesto de Smooth Streaming.

El tutorial [Carga, codificación y streaming con .NET](stream-files-tutorial-with-api.md) muestra cómo crear las direcciones URL de transmisión con .NET (consulte la sección sobre [creación de direcciones URL](stream-files-tutorial-with-api.md#get-streaming-urls)). Si ejecuta la aplicación, una de las URLs apunta al manifiesto Smooth Streaming: `https://amsaccount-usw22.streaming.media.azure.net/00000000-0000-0000-0000-0000000000000/ignite.ism/manifest`.<br/>Copie y pegue la dirección URL en la barra de direcciones de un explorador. Se descargará el archivo. Puede abrirlo en el editor de texto que prefiera.

Para el ejemplo de REST, consulte [Carga, codificación y streaming con REST](stream-files-tutorial-with-rest.md#list-paths-and-build-streaming-urls).

### <a name="monitor-the-bitrate-of-a-video-stream"></a>Supervisión de la velocidad de bits de una secuencia de vídeo

Puede utilizar la [página de demostración de Azure Media Player](http://aka.ms/amp) para supervisar la velocidad de bits de una secuencia de vídeo. La página de demostración muestra información de diagnóstico en la pestaña **Diagnósticos**:

![Diagnósticos de Azure Media Player][amp_diagnostics]

## <a name="defining-filters"></a>Definición de filtros

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

### <a name="example"></a>Ejemplo

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

## <a name="rendition-filtering"></a>Filtrado de representaciones

Puede elegir codificar el activo en varios perfiles de codificación (H.264 Baseline, H.264 High, AACL, AACH, Dolby Digital Plus) y varias velocidades de bits de calidad. Sin embargo, no todos los dispositivos cliente serán compatibles con todos los perfiles y velocidades de bits de su activo. Por ejemplo, los dispositivos Android más antiguos solo admiten H.264 Baseline+AACL. El envío de velocidades de bits más altas a un dispositivo que no puede obtener los beneficios, desperdicia el ancho de banda y el cálculo del dispositivo. Dicho dispositivo debe descodificar toda la información ofrecida, solo para reducirla verticalmente para presentación.

Con el manifiesto dinámico, puede crear perfiles de dispositivo como móvil, consola, HD/SD, etc. e incluir las pistas y calidades que quiere que formen parte de cada perfil.

![Ejemplo de filtrado de representaciones][renditions2]

En el ejemplo siguiente, se usó un codificador para codificar un recurso intermedio en siete representaciones de vídeo MP4 ISO (de 180p a 1080p). El recurso codificado puede empaquetarse dinámicamente en cualquiera de los siguientes protocolos de transmisión: HLS, MPEG DASH y Smooth.  En la parte superior del diagrama, se muestra el manifiesto HLS para el activo sin filtros (contiene las siete representaciones).  En la parte inferior izquierda, se muestra el manifiesto HLS al que se aplicó un filtro denominado "ott". El filtro de "ott" especifica la eliminación de todas las velocidades de bits por debajo de 1 Mbps, lo que dio lugar a que se quitaran los dos niveles de calidad inferiores en la respuesta. En la parte inferior derecha se muestra el manifiesto HLS al que se aplicó un filtro denominado "móvil". El filtro "móvil" especifica la eliminación de las representaciones donde la resolución es mayor que 720p, lo que hizo que se quitaran las dos representaciones de 1080p.

![Filtrado de representaciones][renditions1]

## <a name="removing-language-tracks"></a>Quitar pistas de idioma
Los activos pueden incluir varios idiomas de audio como inglés, español, francés, etc. Normalmente, los administradores del SDK del reproductor toman como valor predeterminado la selección de pistas de audio y las pistas de audio disponibles por selección de usuario. Es un desafío desarrollar estos SDK del Reproductor; requiere diferentes implementaciones en marcos de reproductores específicos de dispositivos. Además, en algunas plataformas, las API del reproductor están limitadas y no incluyen la característica de selección de audio, donde los usuarios no pueden seleccionar o cambiar la pista de audio predeterminada. Con los filtros de activo, puede controlar el comportamiento creando filtros que solo incluyen idiomas de audio deseados.

![Filtrado de pistas de idioma][language_filter]

## <a name="trimming-start-of-an-asset"></a>Recorte del inicio de un activo
En la mayoría de los eventos de streaming en directo, los operadores ejecutan algunas pruebas antes del evento real. Por ejemplo, podrían incluir una pizarra como esta antes del inicio del evento: "El programa comenzará momentáneamente". Si el programa se está archivando, los datos de pizarra y de prueba también se archivan y se incluyen en la presentación. Sin embargo, esta información no se debe mostrar a los clientes. Con el manifiesto dinámico, puede crear un filtro de tiempo de inicio y quitar los datos no deseados del manifiesto.

![Inicio de recorte][trim_filter]

## <a name="creating-subclips-views-from-a-live-archive"></a>Crear clips secundarios (vistas) desde un archivo dinámico
Muchos eventos en directo son de larga ejecución y el archivo en directo puede incluir varios eventos. Después de que termine el evento en directo, es posible que los emisores deseen dividir el archivo dinámico en secuencias de inicio y detención de programa lógicas. Después, publique por separado estos programas virtuales sin procesar posteriormente el archivo dinámico y sin crear activos separados (que no se beneficiarán de los fragmentos en caché existentes en las CDN). Entre los ejemplos de estos programas virtuales se encuentran los cuatro cuartos de un partido de baloncesto o fútbol americano, las entradas en el béisbol o los eventos individuales de una tarde de un programa deportivo.

Con el manifiesto dinámico, puede crear filtros mediante las horas inicial y final y crear vistas virtuales encima de su archivo dinámico. 

![Filtro de clips secundarios][subclip_filter]

Activo filtrado:

![Esquí][skiing]

## <a name="adjusting-presentation-window-dvr"></a>Ajustar la ventana de presentación (DVR)
En la actualidad, Azure Media Services ofrecen un archivo circular donde la duración se puede configurar entre 5 minutos y 25 horas. El filtrado de manifiestos se puede usar para crear una ventana de DVR dinámica encima del archivo, sin eliminar medios. Hay muchos escenarios en los que los emisores quieren proporcionar una ventana de DVR limitada que se mueva con el borde dinámico y al mismo tiempo mantener una ventana de archivado más grande. Es posible que un emisor desee usar los datos que están fuera de la ventana de DVR para resaltar clips, o que desee proporcionar diferentes ventanas de DVR para dispositivos diferentes. Por ejemplo, la mayoría de los dispositivos móviles no administran grandes ventanas de DVR (puede tener una ventana de DVR de 2 minutos para dispositivos móviles y 1 hora para clientes de escritorio).

![Ventana de DVR][dvr_filter]

## <a name="adjusting-livebackoff-live-position"></a>Ajustar LiveBackoff (posición en directo)
El filtrado de manifiestos puede usarse para quitar varios segundos del borde directo de un programa activo. El filtrado permite a los emisores ver la presentación en el punto de publicación de vista previa y crear puntos de inserción de anuncios antes de que los visores reciban la transmisión (normalmente con una interrupción de copia anterior a 30 segundos). Los emisores, entonces, pueden insertar estos anuncios en sus marcos de cliente a tiempo para su recepción y procesar la información antes de la oportunidad de anuncio.

Además de la compatibilidad de anuncio, la opción de configuración LiveBackoff se puede usar para ajustar la posición de los visores para que cuando los clientes se desvíen y lleguen al borde directo puedan obtener todavía fragmentos del servidor en lugar de obtener un error HTTP 404 o 412.

![livebackoff_filter][livebackoff_filter]

## <a name="combining-multiple-rules-in-a-single-filter"></a>Combinación de varias reglas en un filtro único
Puede combinar varias reglas de filtrado en un filtro único. Por ejemplo, puede definir una "regla de intervalos" para quitar las pizarras de un archivo dinámico y filtrar además las velocidades de bits disponibles. Al aplicar varias reglas de filtrado, el resultado final es la intersección de todas las reglas.

![varias reglas][multiple-rules]

## <a name="combining-multiple-filters-filter-composition"></a>Combinar múltiples filtros (composición de filtros)

También puede combinar varios filtros en una sola dirección URL. 

En el escenario siguiente se muestra para qué puede resultar útil la combinación de filtros:

1. Necesita filtrar sus calidades de vídeos para dispositivos móviles, como Android o iPAD (con el fin de limitar las calidades de vídeo). Para quitar las calidades no deseadas, debe crear un filtro de cuenta adecuado para los perfiles de dispositivo. Los filtros de cuenta pueden utilizarse para todos sus recursos en la misma cuenta de servicios multimedia sin ninguna asociación adicional. 
2. También desea recortar el tiempo de inicio y finalización de un activo. Para hacerlo, debe crear un filtro de recursos y establecer la hora de inicio y fin. 
3. Quiere combinar ambos filtros (sin combinación tendría que agregar el filtrado de calidad al filtro de recorte, lo cual dificultaría el uso del filtro).

Para combinar filtros, deberá establecer los nombres de filtro de la dirección URL del manifiesto o la lista de reproducción separados por punto y coma. Supongamos que tiene un filtro denominado *MyMobileDevice* que filtra cualidades y que tiene otro denominado *MyStartTime* para establecer una determinada hora de inicio. Puede combinarlos así:

Puede combinar hasta tres filtros. 

Para más información, consulte [este blog](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) .

## <a name="considerations-and-limitations"></a>Consideraciones y limitaciones

- Los valores de **forceEndTimestamp**, **presentationWindowDuration** y **liveBackoffDuration** no deben establecerse para un filtro de vídeo bajo demanda. Sólo se utilizan para escenarios de filtro en vivo. 
- El manifiesto dinámico funciona en los límites GOP (fotogramas clave), por lo que el recorte tiene precisión GOP. 
- Puede usar el mismo nombre de filtro para los filtros de cuentas y recursos. Los filtros de recurso tienen una mayor prioridad e invalidarán los filtros de cuenta.
- Si actualiza un filtro, se pueden tardar hasta 2 minutos en que el extremo de streaming actualice las reglas. Si el contenido se suministró con algunos filtros (y se almacenó en caché en servidores proxy y cachés CDN), la actualización de  estos filtros puede generar errores del reproductor. Se recomienda borrar la memoria caché después de actualizar el filtro. Si esta opción no es posible, piense en usar un filtro diferente.
- Los clientes necesitan descargar manualmente el manifiesto y analizar la marca de hora de inicio exacta y la escala temporal.
    
    - Para determinar las propiedades de las pistas de un recurso, [obtenga y examine el archivo de manifiesto](#get-and-examine-manifest-files).
    - La fórmula para establecer las propiedades de la marca de tiempo del filtro de recursos: <br/>startTimestamp = &lt;hora de inicio del manifiesto&gt; +  &lt;hora de inicio del filtro esperado en segundos&gt;*escala temporal


## <a name="next-steps"></a>Pasos siguientes

Los artículos siguientes muestran cómo crear filtros mediante programación.  

- [Creación de filtros con las API REST](filters-dynamic-manifest-rest-howto.md)
- [Creación de filtros con .NET](filters-dynamic-manifest-dotnet-howto.md)
- [Creación de filtros con la CLI](filters-dynamic-manifest-cli-howto.md)

[renditions1]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter.png
[renditions2]: ./media/filters-dynamic-manifest-overview/media-services-rendition-filter2.png

[rendered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[timeline_trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[timeline_trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png

[multiple-rules]:./media/filters-dynamic-manifest-overview/media-services-multiple-rules-filters.png

[subclip_filter]: ./media/filters-dynamic-manifest-overview/media-services-subclips-filter.png
[trim_event]: ./media/filters-dynamic-manifests/media-services-timeline-trim-event.png
[trim_subclip]: ./media/filters-dynamic-manifests/media-services-timeline-trim-subclip.png
[trim_filter]: ./media/filters-dynamic-manifest-overview/media-services-trim-filter.png
[redered_subclip]: ./media/filters-dynamic-manifests/media-services-rendered-subclip.png
[livebackoff_filter]: ./media/filters-dynamic-manifest-overview/media-services-livebackoff-filter.png
[language_filter]: ./media/filters-dynamic-manifest-overview/media-services-language-filter.png
[dvr_filter]: ./media/filters-dynamic-manifest-overview/media-services-dvr-filter.png
[skiing]: ./media/filters-dynamic-manifest-overview/media-services-skiing.png
[amp_diagnostics]: ./media/filters-dynamic-manifest-overview/amp_diagnostics.png
