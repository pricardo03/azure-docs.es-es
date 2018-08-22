---
title: Examen de la salida de Azure Video Indexer | Microsoft Docs
description: En este tema se examina la salida de Video Indexer.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cflower
ms.service: cognitive-services
ms.topic: article
ms.date: 05/30/2018
ms.author: juliako
ms.openlocfilehash: 7f61833d70cea3a6d176f5a08eb0db1c30e70e86
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "35383114"
---
# <a name="examine-the-video-indexer-output-produced-by-v1-api"></a>Examen de la salida de Video Indexer producida por la API v1

> [!Note]
> La API v1 de Video Indexer está ya en desuso y se eliminará el 1 de agosto de 2018. Debe empezar a utilizar las API v2 de Video Indexer para evitar interrupciones.
>
> Para desarrollar con Video Indexer API V2, consulte las instrucciones que encontrará [aquí](https://api-portal.videoindexer.ai/). 

Cuando se llama a **Get Breakdowns** API y el estado de la respuesta es correcto, se obtiene una salida JSON detallada como contenido de la respuesta. El contenido JSON contiene información detallada del vídeo especificado, con transcripciones, OCR y personas, entre otros. Los detalles incluyen palabras clave (temas), caras y bloques. Cada bloque incluye intervalos de tiempo, líneas de transcripción, líneas de OCR, opiniones, caras y miniaturas de bloque.

Puede usar **Get Breakdowns** API para obtener el análisis completo de un vídeo como contenido JSON.  
 
    GET https://videobreakdown.azure-api.net/Breakdowns/Api/Partner/Breakdowns/63c6d532ff HTTP/1.1
    Host: videobreakdown.azure-api.net
    Ocp-Apim-Subscription-Key: ••••••••••••••••••••••••••••••••
    
Para examinar visualmente la información detallada del vídeo, presione el botón **Reproducir** que hay sobre el vídeo, en el portal de Video Indexer. Para más información, consulte [Visualización y edición de la información detallada de un vídeo](video-indexer-view-edit.md).

![Información detallada](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

En este artículo se examina el contenido JSON que devuelve **Get Breakdowns** API. Puede resultar útil examinar el artículo en que se explican los [conceptos](video-indexer-concepts.md).

> [!NOTE]
> Todos los tokens de acceso en Video Indexer expiran en una hora.

## <a name="root-elements"></a>Elementos raíz

Atributo | Descripción
---|---
id|Identificador de este vídeo. Por ejemplo, 63c6d532ff.
partición|Partición lógica que el usuario puede especificar en la carga para buscarla más adelante.
Nombre|Nombre del vídeo. Por ejemplo, Azure Monitor.
description|Descripción del vídeo. Por ejemplo, "John Kemnetz se une a Scott Hanselman para mostrar cómo desbloquear el potencial de los datos de supervisión de Azure con Azure Monitor".
userName|Autor del vídeo. Por ejemplo, Channel9 Videos.
createTime |Hora de creación. Por ejemplo, 2017-03-31T16:36:41.4504249+00:00.
privacyMode|El vídeo puede tener uno de los siguientes modos: **Privado**, **Público**. **Público**: el vídeo es visible para todos los usuarios de la cuenta y cualquiera que tenga un vínculo al vídeo. **Privado**: el vídeo es visible para todos los usuarios de la cuenta.
isOwned|Es true si el usuario actual es el dueño del vídeo. En caso contrario, es false.  
isBase|Es true si el desglose se basa en un vídeo de origen. Es false si el desglose es de una lista de reproducción que procede de otro análisis.
durationInSeconds|Duración del vídeo.
summarizedInsights|Contiene la sección [summarizedInsights](#summarizedInsights) con información detallada.
breakdowns|Puede contener uno o varios [breakdowns](#breakdowns).
social|Contiene un elemento **social** que describe el número de Me gusta y las visualizaciones del vídeo.

## <a name="summarizedinsights"></a>summarizedInsights

En esta sección se muestra el resumen de la información detallada.

Atributo | Descripción
---|---
name|Nombre del vídeo. Por ejemplo, Azure Monitor.
shortId|Identificador del vídeo. Por ejemplo, 63c6d532ff.
privacyMode|El desglose puede tener uno de los siguientes modos: **Privado**, **Público**. **Público**: el vídeo es visible para todos los usuarios de la cuenta y cualquiera que tenga un vínculo al vídeo. **Privado**: el vídeo es visible para todos los usuarios de la cuenta.
duration|Duración que describe el momento del tiempo en el que se ha producido la información. La duración se mide en segundos.
thumbnailUrl|Dirección URL completa de la miniatura del vídeo. Por ejemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...". Tenga en cuenta que si el vídeo es privado, la dirección URL contiene un token de acceso de una hora. Una vez transcurrida, dejará de ser válida y deberá obtener el análisis de nuevo con otra dirección URL, o bien llamar a GetAccessToken para obtener un nuevo token de acceso y construir manualmente la dirección URL completa ("https://www.videoindexer.ai/api/Thumbnail/[shortId]/[ThumbnailId]?accessToken=[accessToken]").
faces|Puede contener uno o varios [faces](#faces).
topics|Puede contener uno o varios [topics](#topics).
sentiments|Puede contener uno o varios [sentiments](#sentiments).
audioEffects| Puede contener uno o varios [audioEffects](#audioEffects).
brands| Puede contener uno o varios [brands](#brands).
Estadísticas|Para más información, consulte [Statistics](#Statistics).
.
### <a name="statistics"></a>Estadísticas

|NOMBRE|Descripción|
|---|---|
|CorrespondenceCount|Número de correspondencias en el vídeo.|
|WordCount|Número de palabras por orador.|
|SpeakerNumberOfFragments|Cantidad de fragmentos que el orador tiene en un vídeo.|
|SpeakerLongestMonolog|Monólogo más largo del orador. Si el orador tiene períodos de silencio dentro del monólogo, se incluyen. Los silencios al principio y al final del monólogo se eliminan.| 
|SpeakerTalkToListenRatio|El cálculo se basa en el tiempo invertido en el monólogo del orador (sin los silencios intermedios) dividido por el tiempo total del vídeo. El tiempo se redondea a tres decimales.|

## <a name="breakdowns"></a>breakdowns

En esta sección se muestra la información detallada.

Atributo | Descripción
---|---
id|Identificador del análisis. Por ejemplo, 63c6d532ff.
state|El estado de procesamiento del identificador de análisis concreto. Puede ser uno de los siguientes: Uploaded, Processing, Processed, Failed.
processingProgress|Avance. Por ejemplo, 10 %.
externalId| Puede establecer externalId durante la carga. Por ejemplo, "4f9c3500-eca7-4ab3-987e-a745017af698". Más adelante puede buscar vídeos con este externalId.
externalUrl|Puede establecer externalUrl durante la carga. 
metadata|Puede establecer metadata durante la carga. 
insights|Puede contener uno o varios [insights](#insights).
thumbnailUrl|Dirección URL completa de la miniatura del vídeo. Por ejemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO..". Tenga en cuenta que si el vídeo es privado, la dirección URL contiene un token de acceso de una hora. Una vez transcurrida, dejará de ser válida y deberá obtener el análisis de nuevo con otra dirección URL, o bien llamar a GetAccessToken para obtener un nuevo token de acceso y construir manualmente la dirección URL completa ("https://www.videoindexer.ai/api/Thumbnail/[shortId]/[ThumbnailId]?accessToken=[accessToken]").
publishedUrl|Dirección URL publicada. Por ejemplo, "https://BreakdownMedia.azureedge.net:443/d5e5232d-48e2-4fbc-9893-0ea6335da563/Azure%20Monitor%20%20Azure%20Friday.ism/manifest".
viewToken|Token de portador.
sourceLanguage|Idioma de origen. Se admiten los siguientes: chino, inglés, francés, alemán, italiano, japonés, portugués, ruso, español.
language|Idioma de la transcripción.

## <a name="insights"></a>insights

Atributo | DESCRIPCIÓN 
---|---
transcriptBlocks|Puede contener uno o varios [transcriptBlocks](#transcriptBlocks).
topics|Puede contener uno o varios [topics](#topics).
faces|Puede contener uno o varios [faces](#faces).
participants|Puede contener uno o varios [participants](#participants).
contentModeration|Puede contener un [contentModeration](#contentModeration).
audioEffectsCategories|Puede contener uno o varios [audioEffectsCategories](#audioEffectsCategories).

## <a name="faces"></a>faces

### <a name="summarizedinsights"></a>summarizedInsights

**faces** que aparecen bajo **summarizedInsights**, muestran un resumen de cada rostro que se encuentra en el vídeo.

Atributo | Descripción 
---|---
id|Identificador de una persona. Por ejemplo, 11775.
shortId|Identificador breve. Como una lista de reproducción puede proceder de varios análisis, se necesita este identificador para buscar el original de cada rostro.  
Nombre|Si se reconoce el rostro, se agrega el nombre de la persona. Por ejemplo, "Scott Hanselman". Si no se reconoce, se agrega "Unknown #". 
description|Si se reconoce el rostro, la descripción se rellena en función de la API de búsqueda de Bing. En caso contrario, la descripción es **null**.
título|Si se reconoce el rostro, la descripción se rellena en función de la API de búsqueda de Bing. En caso contrario, el título es **null**.
thumbnailFullUrl|Dirección URL completa de la miniatura del rostro. Por ejemplo, "https://www.videoindexer.ai/api/Thumbnail/3a9e38d72e/d1f5fac5-e8ae-40d9-a04a-6b2928fb5d10?accessToken=eyJ0eXAiOiJKV1QiLCJhbGciO...". Tenga en cuenta que si el vídeo es privado, la dirección URL contiene un token de acceso de una hora. Una vez transcurrida, dejará de ser válida y deberá obtener el análisis de nuevo con otra dirección URL, o bien llamar a GetAccessToken para obtener un nuevo token de acceso y construir manualmente la dirección URL completa ("https://www.videoindexer.ai/api/Thumbnail/[shortId]/[ThumbnailId]?accessToken=[accessToken]").
appearances|Puede contener uno o varios dimensiones [appearances](#appearances).
seenDuration|Tiempo durante el que se ha visto el rostro (en segundos).
seenDurationRatio|Presencia en relación con la duración del vídeo (0-1).

### <a name="breakdown-insights"></a>breakdown insights

**faces** que aparecen bajo **breakdowns**, describe los detalles de cada rostro que aparece en el vídeo.

Atributo | Descripción 
---|---
id|Identificador de una persona. Por ejemplo, 11775.
bingId|
Nombre|Si se reconoce el rostro, se agrega el nombre de la persona. Por ejemplo, "Scott Hanselman". Si no se reconoce, se agrega "Unknown #". 
thumbnailId|Por ejemplo, 616468f0-1636-4efa-94e7-262f2e575059.
description|Si se reconoce el rostro, la descripción se rellena en función de la API de búsqueda de Bing. En caso contrario, la descripción es **null**.
título|Si se reconoce el rostro, la descripción se rellena en función de la API de búsqueda de Bing. En caso contrario, el título es **null**.
imageUrl|Esta dirección URL señala a una imagen tomada del vídeo original.  
confidence|Puntuación de confianza (mayor es mejor).
knownPersonId|Identificador de una persona conocida (por ejemplo, un famoso). Si no se conoce la persona, el identificador contiene ceros. Por ejemplo, e3eaff5f-ee1b-4eac-80ce-ebac47aadf64.

## <a name="topics"></a>topics

### <a name="summarizedinsights"></a>summarizedInsights

**topics** que aparecen bajo **summarizedInsights**, muestran un resumen de cada tema que se encuentra en el vídeo.

Atributo | Descripción 
---|---
Nombre|El nombre del tema (por ejemplo, "Azure"). 
appearances|Puede contener uno o varios [appearances](#appearances).
isTranscript|Es true si se encuentra en una transcripción. Es false si se encuentra en OCR.

### <a name="breakdown-insights"></a>breakdown insights

**topics** que aparecen bajo **breakdowns**, describen los detalles de cada tema que aparece en el vídeo.

|Atributo | Descripción |
|---|---|
|id|Identificador único del tema.|
|Nombre|Nombre del tema.|
|stem|Actualmente este atributo no se usa.|
|words|Actualmente este atributo no se usa.|
|rank|Puntuación por pertinencia (mayor es mejor).|

## <a name="sentiments"></a>sentiments

Atributo | DESCRIPCIÓN
---|---
sentimentKey| Actualmente se admiten las siguientes opiniones: Positive, Neutral, Negative. 
appearances|Puede contener uno o varios [appearances](#appearances)|.
seenDurationRatio|Presencia en relación con la duración del vídeo (0-1).

## <a name="audioeffects"></a>audioEffects

Atributo | DESCRIPCIÓN 
---|---
audioEffectKey| Los valores válidos son: Speech, Silence, HandClaps.
appearances|Puede contener uno o varios [appearances](#appearances)
seenDurationRatio|Presencia en relación con la duración del vídeo (0-1).
seenDuration|Duración del efecto de audio (en segundos).

## <a name="appearances"></a>appearances

Atributo | DESCRIPCIÓN 
---|---
startTime| Valor de tiempo.
endTime|Valor de tiempo.
startSeconds| Valor de tiempo.
endSeconds| Valor de tiempo.

## <a name="participants"></a>participants

Atributo | Descripción 
---|---
id|Id. del participante.
Nombre|Nombre del participante. Por ejemplo, Speaker #1.
pictureUrl|El atributo **pictureUrl** se reserva para su uso en el futuro.

## <a name="contentmoderation"></a>contentModeration

Atributo | DESCRIPCIÓN 
---|---
adultClassifierValue|Nivel de confianza que el vídeo tiene en cuanto a contenido para adultos.
racyClassifierValue|Nivel de confianza que el vídeo tiene en cuanto a contenido subido de tono.
bannedWordsCount|Número de términos obscenos. 
bannedWordsRatio|Proporción de obscenidades respecto al total de palabras.
reviewRecommended|Valor booleano que indica si el vídeo se debe revisar manualmente.
isAdult|Valores booleanos que indican si el vídeo se considera para adultos tras la revisión manual.

## <a name="audioeffectscategories"></a>audioEffectsCategories

Atributo | DESCRIPCIÓN 
---|---
Tipo|Identificador de la categoría.
key|Uno de los siguientes: Speech, Silence, HandClaps. 

## <a name="transcriptblocks"></a>transcriptBlocks

Atributo | Descripción
---|---
id|Identificador del bloque
lines|Puede contener uno o varios [lines](#lines).
sentimentIds|El atributo **sentimentIds** se reserva para su uso en el futuro.
thumbnailIds|El atributo **thumbnailIds** se reserva para su uso en el futuro.
opinión|La opinión del bloque (0-1, de negativa a positiva).
faces|Puede contener uno o varios [faces](#faces).
ocrs|Puede contener uno o varios [ocrs](#ocrs).
audioEffectInstances|Puede contener uno o varios [audioEffectInstances](#audioEffectInstances).
scenes|Puede contener uno o varios [scenes](#scenes).
annotations|Puede contener cero o más [annotations](#annotations).

## <a name="ocrs"></a>ocrs

Describe en qué momento del vídeo se encontró el texto. 

Atributo | DESCRIPCIÓN 
---|---
timeRange|Intervalo de tiempo del vídeo original.
adjustedTimeRange|Intervalo de tiempo relativo a la lista de reproducción actual. Como se puede crear una lista de reproducción a partir de diferentes líneas de vídeos distintos, puede grabar un vídeo de una hora y utilizar solo una línea de él, por ejemplo, de 10:00 a 10:15. En ese caso, tendrá una lista de reproducción con 1 línea, donde el intervalo de tiempo es 10:00-10:15, pero adjustedTimeRange es 00:00-00:15.
lines|Puede contener uno o varios [lines](#lines).

## <a name="lines"></a>lines

### <a name="transcriptblocks"></a>transcriptBlocks

**lines** que aparecen bajo **transcriptBlocks**, describe las líneas de transcripción que se encuentran en el video.

Atributo | Descripción 
---|---
id| Identificador de la línea.
timeRange|Intervalo de tiempo del vídeo original.
adjustedTimeRange|Intervalo de tiempo relativo a la lista de reproducción actual. Como se puede crear una lista de reproducción a partir de diferentes líneas de vídeos distintos, puede grabar un vídeo de una hora y utilizar solo una línea de él, por ejemplo, de 10:00 a 10:15. En ese caso, tendrá una lista de reproducción con 1 línea, donde el intervalo de tiempo es 10:00-10:15, pero adjustedTimeRange es 00:00-00:15.
participantID| Identificador del orador de esta línea.
text| Transcripción.
isIncluded| En los análisis de base siempre es true. En las listas de reproducción, las líneas incluidas en el vídeo original se establecen en isIncluded = true. Las demás líneas son false.

### <a name="ocrs"></a>ocrs

**lines** que aparecen bajo **ocrs**, describe las líneas de OCR que se encuentran en el vídeo.

Atributo | Descripción 
---|---
id|Identificador de OCR.
width|Actualmente este atributo no se usa.
height|Actualmente este atributo no se usa.
language|Idioma de OCR.
textData|Texto OCR.
confidence|Puntuación de confianza (mayor es mejor).

## <a name="scenes"></a>scenes

Atributo | Descripción 
---|---
id|Identificador de la escena.
timeRange|Contiene un **timeRange**.
keyFrame|Hora del fotograma clave.
shots|Puede contener uno o varios [shots](#shots).

## <a name="shots"></a>shots

Atributo | Descripción 
---|---
id||Identificador de la toma.
timeRange|Contiene un **timeRange**.
keyFrame|Hora del fotograma clave.

## <a name="audioeffectinstances"></a>audioEffectInstances

Atributo | DESCRIPCIÓN 
---|---
Tipo|El índice del evento audio: Laughter = 1, HandClaps = 2, Music = 3, Speech = 4, Silence = 5.
ranges|Puede contener uno o varios [ranges](#ranges).

## <a name="ranges"></a>ranges

**ranges** que aparecen bajo **audioEffectInstances**, describe los efectos de audio en esos intervalos.

Atributo | DESCRIPCIÓN 
---|---
timeRange|Intervalo de tiempo del vídeo original.
adjustedTimeRange|Intervalo de tiempo relativo a la lista de reproducción actual. Como se puede crear una lista de reproducción a partir de diferentes líneas de vídeos distintos, puede grabar un vídeo de una hora y utilizar solo una línea de él, por ejemplo, de 10:00 a 10:15. En ese caso, tendrá una lista de reproducción con 1 línea, donde el intervalo de tiempo es 10:00-10:15, pero adjustedTimeRange es 00:00-00:15.

## <a name="annotations"></a>annotations

Devuelve etiquetas en función de los objetos reconocibles, los seres vivos, los paisajes, las acciones y los modelos visuales.

|Atributo|Descripción|
|---|---|
|id|Identificador de la anotación.|
|NOMBRE|Nombre de la anotación (por ejemplo, Person, Athletic game, Black Frames).|
|Appearances|Puede contener uno o varios appearances.|

## <a name="brands"></a>brands

Nombres de empresas y marcas de productos detectados en la transcripción de voz a texto o de OCR de vídeo. No incluye el reconocimiento visual de marcas ni la detección de logotipos.

Atributo | Descripción
---|---
id | Identificador de una marca.
Nombre | Nombre de la marca de Bing o una marca personalizada.  
wikiId | Sufijo de la dirección URL de la wikipedia de la marca. Por ejemplo, "Target_Corporation" es el sufijo de [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
wikiUrl | Dirección url de la wikipedia de la marca, si existe. Por ejemplo, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
confidence | Valor de la confianza del detector de marcas de Video Indexer (0-1).
description | Descripción de la marca extraída de Wikipedia. 
appearances | Puede contener uno o varios appearances.
seenDuration | Presencia en relación con la duración del vídeo (0-1).

## <a name="next-steps"></a>Pasos siguientes

Para información sobre cómo crear un análisis propio, consulte [View and edit Video Indexer insights](video-indexer-view-edit.md) (Visualización y edición de conclusiones de Video Indexer).

Para más información acerca de cómo insertar widgets en su aplicación, consulte [Embed Video Indexer widgets into your applications](video-indexer-embed-widgets.md) (Inserción de widgets de Video Indexer en sus aplicaciones). 

## <a name="see-also"></a>Otras referencias

[Video Indexer API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

[Introducción a Video Indexer](video-indexer-overview.md)

