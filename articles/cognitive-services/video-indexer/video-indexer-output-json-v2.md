---
title: Examen detallado de la salida de Azure Video Indexer producida por la API v2 | Microsoft Docs
description: En este tema se examina la salida de Video Indexer producida por la API v2.
services: cognitive services
documentationcenter: ''
author: juliako
manager: cfowler
ms.service: cognitive-services
ms.topic: article
ms.date: 07/25/2018
ms.author: juliako
ms.openlocfilehash: 9a926eb274e5e4cec721864d1d9c5faee8ec58ef
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618346"
---
# <a name="examine-the-video-indexer-output-produced-by-v2-api"></a>Examen de la salida de Video Indexer producida por la API v2

> [!Note]
> Video Indexer API V1 ha quedado en desuso el 1 de agosto de 2018. A partir de esa fecha, debe usar Video Indexer API V2. <br/>Para desarrollar con Video Indexer API V2, consulte las instrucciones que encontrará [aquí](https://api-portal.videoindexer.ai/). 

Cuando se llama a **Get Video Index** API y el estado de la respuesta es correcto, obtendrá una salida JSON detallada como contenido de la respuesta. El contenido JSON incluye información detallada del vídeo especificado. La información detallada incluye dimensiones como: transcripciones, OCR, rostros, temas o bloques, entre otras. Las dimensiones tienen instancias de intervalos de tiempo que muestran cuándo apareció cada dimensión en el vídeo.  

Para examinar visualmente la información detallada del vídeo, presione el **Reproducir** que hay sobre el vídeo, en el portal de Video Indexer. Para más información, consulte [Visualización y edición de la información detallada de un vídeo](video-indexer-view-edit.md).

![Información detallada](./media/video-indexer-output-json/video-indexer-summarized-insights.png)

En este artículo se examina el contenido JSON devuelto por **Get Video Index** API. 

> [!NOTE]
> Todos los tokens de acceso en Video Indexer expiran en una hora.


## <a name="root-elements"></a>Elementos raíz

|Nombre|Descripción|
|---|---|
|accountId|Identificador de la cuenta de Video Indexer de la lista de reproducción.|
|id|Identificador de la lista de reproducción.|
|name|Nombre de la lista de reproducción.|
|description|Descripción de la lista de reproducción.|
|userName|Nombre del usuario que creó la lista de reproducción.|
|created|Hora de creación de la lista de reproducción.|
|privacyMode|Modo de privacidad de la lista de reproducción (privada/pública).|
|state|Estado de la lista de reproducción (cargada, en proceso, procesada, error, en cuarentena).|
|isOwned|Indica si la lista de reproducción la creó el usuario actual.|
|isEditable|Indica si el usuario actual está autorizado para editar la lista de reproducción.|
|isBase|Indica si la lista de reproducción es una lista de reproducción base (un vídeo) o una lista de reproducción formada por otros vídeos (derivada).|
|durationInSeconds|Duración total de la lista de reproducción.|
|summarizedInsights|Contiene la sección [summarizedInsights](#summarizedinsights) con información detallada.
|videos|Lista de [vídeos](#videos) que forman la lista de reproducción.<br/>Si esta lista de reproducción está compuesta por intervalos de tiempo de otros vídeos (derivados), los vídeos de esta lista contienen los datos únicamente de los intervalos de tiempo incluidos.|

```json
{
  "accountId": "bca61527-1221-bca6-1527-a90000002000",
  "id": "abc3454321",
  "name": "My first video",
  "description": "I am trying VI",
  "userName": "Some name",
  "created": "2018/2/2 18:00:00.000",
  "privacyMode": "Private",
  "state": "Processed",
  "isOwned": true,
  "isEditable": false,
  "isBase": false,
  "durationInSeconds": 120, 
  "summarizedInsights" : { . . . }
  "videos": [{ . . . }]
}
```

## <a name="summarizedinsights"></a>summarizedInsights

En esta sección se muestra el resumen de la información detallada.

|Atributo | Descripción|
|---|---|
|name|Nombre del vídeo. Por ejemplo, Azure Monitor.|
|shortId|Identificador del vídeo. Por ejemplo, 63c6d532ff.|
|privacyMode|El desglose puede tener uno de los siguientes modos: **Privado**, **Público**. **Público**: el vídeo es visible para todos los usuarios de la cuenta y cualquiera que tenga un vínculo al vídeo. **Privado**: el vídeo es visible para todos los usuarios de la cuenta.|
|duration|Duración que describe el momento del tiempo en el que se ha producido la información. La duración se mide en segundos.|
|thumbnailVideoId|Identificador del vídeo del que se tomó la miniatura.
|thumbnailId|Identificador de la miniatura del vídeo. Para obtener la miniatura real, llame a Get-Thumbnail (https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-thumbnail) y pase los valores de thumbnailVideoId y thumbnailId).|
|faces|Puede contener cero o más rostros. Para más información, consulte [faces](#faces).|
|keywords|Puede contener cero o más palabras clave. Para más información, consulte [keywords](#keywords).|
|sentiments|Puede contener cero o más opiniones. Para más información, consulte [sentiments](#sentiments).|
|audioEffects| Puede contener cero o más efectos de audio. Para más información, consulte [audioEffects](#audioeffects).|
|labels| Puede contener cero o más etiquetas. Para más información, consulte [labels](#labels).|
|brands| Puede contener cero o más marcas. Para más información, consulte [brands](#brands).|
|statistics | Para más información, consulte [statistics](#statistics).|

## <a name="videos"></a>videos

|Nombre|Descripción|
|---|---|
|accountId|Identificador de la cuenta de Video Indexer del vídeo.|
|id|Identificador del vídeo.|
|name|Nombre del vídeo.
|state|Estado del vídeo (cargado, en proceso, procesado, error, en cuarentena).|
|processingProgress|Progreso del procesamiento (por ejemplo, 20 %).|
|failureCode|Código de error si no se pudo procesar (por ejemplo, "UnsupportedFileType").|
|failureMessage|Mensaje de error si no se pudo procesar.|
|externalId|Identificador externo del vídeo (si lo especifica el usuario).|
|externalUrl|URL externa del vídeo (si la especifica el usuario).|
|metadata|Metadatos externos del vídeo (si los especifica el usuario).|
|isAdult|Indica si el vídeo se ha revisado manualmente y se ha identificado como un vídeo para adultos.|
|insights|Objeto de información detallada. Para más información, consulte [insights](#insights).|
|thumbnailId|Identificador de la miniatura del vídeo. Para obtener la miniatura real, llame a Get-Thumbnail (https://api-portal.videoindexer.ai/docs/services/operations/operations/Get-thumbnail) y pase el identificador del vídeo y el valor de thumbnailId).|
|publishedUrl|Dirección URL para transmitir el vídeo.|
|publishedUrlProxy|Dirección URL desde la que se transmitirá el vídeo (para dispositivos de Apple).|
|viewToken|Un token de visualización de corta duración para transmitir el vídeo.|
|sourceLanguage|Idioma de origen del vídeo.|
|language|Idioma real del vídeo (traducción).|
|indexingPreset|Valor predeterminado utilizado para indexar el vídeo.|
|streamingPreset|Valor predeterminado utilizado para publicar el vídeo.|
|linguisticModelId|Modelo CRIS utilizado para transcribir el vídeo.|
|statistics | Para más información, consulte [statistics](#statistics).|

```json
{
    "videos": [{
        "accountId": "2cbbed36-1972-4506-9bc7-55367912df2d",
        "id": "142a356aa6",
        "state": "Processed",
        "privacyMode": "Private",
        "processingProgress": "100%",
        "failureCode": "General",
        "failureMessage": "",
        "externalId": null,
        "externalUrl": null,
        "metadata": null,
        "insights": {. . . },
        "thumbnailId": "89d7192c-1dab-4377-9872-473eac723845",
        "publishedUrl": "https://videvmediaservices.streaming.mediaservices.windows.net:443/d88a652d-334b-4a66-a294-3826402100cd/Xamarine.ism/manifest",
        "publishedProxyUrl": null,
        "viewToken": "Bearer=<token>",
        "sourceLanguage": "En-US",
        "language": "En-US",
        "indexingPreset": "Default",
        "linguisticModelId": "00000000-0000-0000-0000-000000000000"
    }],
}
```
### <a name="insights"></a>insights

La información detallada es un conjunto de dimensiones (por ejemplo, líneas de transcripción, rostros, marcas, etc.), en el que cada dimensión es una lista de elementos únicos (por ejemplo, face1, face2, face3) y cada elemento tiene sus propios metadatos y una lista de sus instancias (que son intervalos de tiempo con metadatos opcionales adicionales).

Un rostro podría tener un identificador, un nombre, una miniatura, otros metadatos y una lista de sus instancias temporales (por ejemplo: 00:00:05: 00:00:10, 00:01:00 - 00:02:30 y 00:41:21: 00:41:49). Cada instancia temporal puede tener metadatos adicionales. Por ejemplo, las coordenadas del rectángulo del rostro (20,230,60,60).

|Versión|Versión del código|
|---|---|
|sourceLanguage|Idioma de origen del vídeo (suponiendo que hay un idioma principal). Con el formato de cadena [BCP-47](https://tools.ietf.org/html/bcp47).|
|language|Idioma de la información detallada (traducida del idioma de origen). Con el formato de cadena [BCP-47](https://tools.ietf.org/html/bcp47).|
|transcript|Dimensión [transcript](#transcript).|
|ocr|Dimensión [ocr](#ocr).|
|keywords|Dimensión [keywords](#keywords).|
|blocks|Puede contener uno o más dimensiones [blocks](#blocks).|
|faces|Dimensión [faces](#faces).|
|labels|Dimensión [labels](#labels).|
|shots|Dimensión [shots](#shots).|
|brands|Dimensión [brands](#brands).|
|audioEffects|Dimension [audioEffects](#audioEffects).|
|sentiments|Dimension [sentiments](#sentiments).|
|visualContentModeration|Dimensión [visualContentModeration](#visualcontentmoderation).|
|textualConentModeration|Dimensión [textualConentModeration](#textualconentmoderation).|

Ejemplo:

```json
{
  "version": "0.9.0.0",
  "sourceLanguage": "en-US",
  "language": "es-ES",
  "transcript": ...,
  "ocr": ...,
  "keywords": ...,
  "faces": ...,
  "labels": ...,
  "shots": ...,
  "brands": ...,
  "audioEffects": ...,
  "sentiments": ...,
  "visualContentModeration": ...,
  "textualConentModeration": ...
}
```

#### <a name="blocks"></a>blocks

Atributo | Descripción
---|---
id|Identificador del bloque|
instances|Lista de intervalos de tiempo de este bloque.|

#### <a name="transcript"></a>transcript

|Nombre|Descripción|
|---|---|
|id|Identificador de la línea.|
|text|La transcripción en sí.|
|language|Idioma de la transcripción. Diseñado para admitir la transcripción, donde cada línea puede tener un idioma distinto.|
|instances|Lista de los intervalos de tiempo donde apareció esta línea. Si la instancia es una transcripción, solo tendrá 1 instancia.|

Ejemplo:

```json
"transcript": [
{
    "id": 0,
    "text": "Hi I'm Doug from office.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    }
    ]
},
{
    "id": 1,
    "text": "I have a guest. It's Michelle.",
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:02.7200000",
        "end": "00:00:03.9600000"
    }
    ]
}
] 
```

#### <a name="ocr"></a>ocr

|Nombre|Descripción|
|---|---|
|id|Identificador de la línea OCR.|
|text|Texto OCR.|
|confidence|Confiabilidad del reconocimiento.|
|language|Idioma de OCR.|
|instances|Lista de los intervalos de tiempo donde apareció este OCR (el mismo OCR puede aparecer varias veces).|

```json
"ocr": [
    {
      "id": 0,
      "text": "LIVE FROM NEW YORK",
      "confidence": 0.91,
      "language": "en-US",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:52"
        }
      ]
    },
    {
      "id": 1,
      "text": "NOTICIAS EN VIVO",
      "confidence": 0.9,
      "language": "es-ES",
      "instances": [
        {
          "start": "00:00:26",
          "end": "00:00:28"
        },
        {
          "start": "00:00:32",
          "end": "00:00:38"
        }
      ]
    }
  ],
```

#### <a name="keywords"></a>keywords

|Nombre|Descripción|
|---|---|
|id|Identificador de la palabra clave.|
|text|Texto de la palabra clave.|
|confidence|Confiabilidad del reconocimiento de la palabra clave.|
|language|Idioma de la palabra clave (cuando se traduce).|
|instances|Lista de los intervalos de tiempo donde apareció esta palabra clave (una palabra clave puede aparecer varias veces).|

```json
"keywords": [
{
    "id": 0,
    "text": "office",
    "confidence": 1.6666666666666667,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:00.5100000",
        "end": "00:00:02.7200000"
    },
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    }
    ]
},
{
    "id": 1,
    "text": "icons",
    "confidence": 1.4,
    "language": "en-US",
    "instances": [
    {
        "start": "00:00:03.9600000",
        "end": "00:00:12.2700000"
    },
    {
        "start": "00:00:13.9900000",
        "end": "00:00:15.6100000"
    }
    ]
}
] 

```

#### <a name="faces"></a>faces

|Nombre|Descripción|
|---|---|
|id|Identificador del rostro.|
|name|Nombre del rostro. Puede ser "Unknown #0", una celebridad identificada o una persona capacitada del cliente.|
|confidence|Confidencialidad de la identificación facial.|
|description|Descripción de la celebridad. |
|thumbnalId|Identificador de la miniatura del rostro en cuestión.|
|knownPersonId|Si es alguien conocido, su identificador interno.|
|referenceId|Si es una celebridad de Bing, su identificador de Bing.|
|referenceType|Actualmente, solo Bing.|
|título|Si es una celebridad, su cargo (por ejemplo, "CEO de Microsoft").|
|imageUrl|Si es una celebridad, la dirección URL de su imagen.|
|instances|Instancias de cuándo apareció el rostro en el intervalo de tiempo determinado. Cada instancia también tiene su thumbnailsId. |

```json
"faces": [{
    "id": 2002,
    "name": "Xam 007",
    "confidence": 0.93844,
    "description": null,
    "thumbnailId": "00000000-aee4-4be2-a4d5-d01817c07955",
    "knownPersonId": "8340004b-5cf5-4611-9cc4-3b13cca10634",
    "referenceId": null,
    "title": null,
    "imageUrl": null,
    "instances": [{
        "thumbnailsIds": ["00000000-9f68-4bb2-ab27-3b4d9f2d998e",
        "cef03f24-b0c7-4145-94d4-a84f81bb588c"],
        "adjustedStart": "00:00:07.2400000",
        "adjustedEnd": "00:00:45.6780000",
        "start": "00:00:07.2400000",
        "end": "00:00:45.6780000"
    },
    {
        "thumbnailsIds": ["00000000-51e5-4260-91a5-890fa05c68b0"],
        "adjustedStart": "00:10:23.9570000",
        "adjustedEnd": "00:10:39.2390000",
        "start": "00:10:23.9570000",
        "end": "00:10:39.2390000"
    }]
}]
```

#### <a name="labels"></a>labels

|Nombre|Descripción|
|---|---|
|id|Identificador de la etiqueta.|
|name|Nombre de la etiqueta (por ejemplo, "Equipo", "TV").|
|language|Idioma del nombre de la etiqueta (cuando se traduce). BCP-47|
|instances|Lista de los intervalos de tiempo donde apareció esta etiqueta (una etiqueta puede aparecer varias veces). Cada instancia tiene un campo de confiabilidad. |


```json
"labels": [
    {
      "id": 0,
      "name": "person",
      "language": "en-US",
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 00.0000000",
          "end": "00: 00: 25.6000000"
        },
        {
          "confidence": 1.0,
          "start": "00: 01: 33.8670000",
          "end": "00: 01: 39.2000000"
        }
      ]
    },
    {
      "name": "indoor",
      "language": "en-US",
      "id": 1,
      "instances": [
        {
          "confidence": 1.0,
          "start": "00: 00: 06.4000000",
          "end": "00: 00: 07.4670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 09.6000000",
          "end": "00: 00: 10.6670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 11.7330000",
          "end": "00: 00: 20.2670000"
        },
        {
          "confidence": 1.0,
          "start": "00: 00: 21.3330000",
          "end": "00: 00: 25.6000000"
        }
      ]
    }
  ] 
```

#### <a name="shots"></a>shots

|Nombre|Descripción|
|---|---|
|id|Identificador de la toma.|
|keyFrames|Lista de los fotogramas clave dentro de la toma (cada uno con un identificador y una lista de intervalos de tiempo de instancias). Las instancias de fotogramas clave tienen un campo thumbnailId con el identificador de la miniatura del fotograma clave.|
|instances|Lista de intervalos de tiempo de esta toma (las tomas solo tienen 1 instancia).|

```json
"Shots": [
    {
      "id": 0,
      "keyFrames": [
        {
          "id": 0,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",
              "start": "00: 00: 00.1670000",
              "end": "00: 00: 00.2000000"
            }
          ]
        }
      ],
      "instances": [
        {
       "thumbnailId": "00000000-0000-0000-0000-000000000000",   
          "start": "00: 00: 00.2000000",
          "end": "00: 00: 05.0330000"
        }
      ]
    },
    {
      "id": 1,
      "keyFrames": [
        {
          "id": 1,
          "instances": [
            {
          "thumbnailId": "00000000-0000-0000-0000-000000000000",        
              "start": "00: 00: 05.2670000",
              "end": "00: 00: 05.3000000"
            }
          ]
        }
      ],
      "instances": [
        {
      "thumbnailId": "00000000-0000-0000-0000-000000000000",
          "start": "00: 00: 05.2670000",
          "end": "00: 00: 10.3000000"
        }
      ]
    }
  ]
```

#### <a name="brands"></a>brands

Nombres de empresas y marcas de productos detectados en la transcripción de voz a texto o de OCR de vídeo. No incluye el reconocimiento visual de marcas ni la detección de logotipos.

|Nombre|Descripción|
|---|---|
|id|Identificador de marca.|
|name|Nombre de la marca.|
|referenceId | Sufijo de la dirección URL de la wikipedia de la marca. Por ejemplo, "Target_Corporation" es el sufijo de [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|referenceUrl | Dirección url de la wikipedia de la marca, si existe. Por ejemplo, [https://en.wikipedia.org/wiki/Target_Corporation](https://en.wikipedia.org/wiki/Target_Corporation).
|description|Descripción de la marca.|
|etiquetas|Lista de etiquetas predefinidas asociadas a esta marca.|
|confidence|Valor de la confianza del detector de marcas de Video Indexer (0-1).|
|instances|Lista de intervalos de tiempo de esta marca. Cada instancia tiene un atributo brandType, que indica si esta marca apareció en la transcripción o en el reconocimiento óptico de caracteres.|

```json
"brands": [
{
    "id": 0,
    "name": "MicrosoftExcel",
    "referenceId": "Microsoft_Excel",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft_Excel",
    "referenceType": "Wiki",
    "description": "Microsoft Excel is a sprea..",
    "tags": [],
    "confidence": 0.975,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 00: 31.3000000",
        "end": "00: 00: 39.0600000"
    }
    ]
},
{
    "id": 1,
    "name": "Microsoft",
    "referenceId": "Microsoft",
    "referenceUrl": "http: //en.wikipedia.org/wiki/Microsoft",
    "description": "Microsoft Corporation is...",
    "tags": [
    "competitors",
    "technology"
    ],
    "confidence": 1.0,
    "instances": [
    {
        "brandType": "Transcript",
        "start": "00: 01: 44",
        "end": "00: 01: 45.3670000"
    },
    {
        "brandType": "Ocr",
        "start": "00: 01: 54",
        "end": "00: 02: 45.3670000"
    }
    ]
}
]
```

#### <a name="statistics"></a>statistics

|Nombre|Descripción|
|---|---|
|CorrespondenceCount|Número de correspondencias en el vídeo.|
|WordCount|Número de palabras por orador.|
|SpeakerNumberOfFragments|Cantidad de fragmentos que el orador tiene en un vídeo.|
|SpeakerLongestMonolog|Monólogo más largo del orador. Si el orador tiene períodos de silencio dentro del monólogo, se incluyen. Los silencios al principio y al final del monólogo se eliminan.| 
|SpeakerTalkToListenRatio|El cálculo se basa en el tiempo invertido en el monólogo del orador (sin los silencios intermedios) dividido por el tiempo total del vídeo. El tiempo se redondea a tres decimales.|

#### <a name="audioeffects"></a>audioEffects

|Nombre|Descripción|
|---|---|
|id|Identificador del efecto de audio.|
|Tipo|Tipo de efecto de audio (por ejemplo, aplausos, voz, silencio).|
|instances|Lista de los intervalos de tiempo donde apareció este efecto de audio.|

```json
"audioEffects": [
{
    "id": 0,
    "type": "Clapping",
    "instances": [
    {
        "start": "00:00:00",
        "end": "00:00:03"
    },
    {
        "start": "00:01:13",
        "end": "00:01:21"
    }
    ]
}
]
```

#### <a name="sentiments"></a>sentiments

Las opiniones se agregan según su campo sentimentType (Positive/Neutral/Negative). Por ejemplo, 0-0,1, 0,1-0,2.

|Nombre|Descripción|
|---|---|
|id|Identificador de la opinión.|
|averageScore |Promedio de todas las puntuaciones de todas las instancias de ese tipo de opinión: Positive/Neutral/Negative.|
|instances|Lista de los intervalos de tiempo donde apareció esta opinión.|
|sentimentType |El tipo puede ser "Positive", "Neutral" o "Negative".|

```json
"sentiments": [
{
    "id": 0,
    "averageScore": 0.87,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:23",
        "end": "00:00:41"
    }
    ]
}, {
    "id": 1,
    "averageScore": 0.11,
    "sentimentType": "Positive",
    "instances": [
    {
        "start": "00:00:13",
        "end": "00:00:21"
    }
    ]
}
]
```

#### <a name="visualcontentmoderation"></a>visualContentModeration

El bloque visualContentModeration contiene intervalos de tiempo en los que Video Indexer ha detectado que podría haber contenido para adultos. Si visualContentModeration está vacío, no se ha identificado ningún contenido para adultos.

Los vídeos en los que se encuentre contenido para adultos o subido de tono podrían estar disponibles solo para visualización privada. Los usuarios tienen la opción de enviar una solicitud de revisión humana del contenido, en cuyo caso el atributo IsAdult contendrá el resultado de la revisión humana.

|Nombre|Descripción|
|---|---|
|id|Identificador de la moderación del contenido visual.|
|adultScore|Puntuación de contenido para adultos (del moderador de contenido).|
|racyScore|Puntuación de contenido subido de tono (del moderador de contenido).|
|instances|Lista de intervalos de tiempo donde apareció esta moderación de contenido visual.|

```json
"VisualContentModeration": [
{
    "id": 0,
    "adultScore": 0.00069,
    "racyScore": 0.91129,
    "instances": [
    {
        "start": "00:00:25.4840000",
        "end": "00:00:25.5260000"
    }
    ]
},
{
    "id": 1,
    "adultScore": 0.99231,
    "racyScore": 0.99912,
    "instances": [
    {
        "start": "00:00:35.5360000",
        "end": "00:00:35.5780000"
    }
    ]
}
] 
```

#### <a name="textualconentmoderation"></a>textualConentModeration 

|Nombre|Descripción|
|---|---|
|id|Identificador de la moderación de contenido textual.|
|bannedWordsCount |Número de palabras no permitidas.|
|bannedWordsRatio |Proporción respecto al número total de palabras.|


## <a name="next-steps"></a>Pasos siguientes

[Video Indexer API](https://videobreakdown.portal.azure-api.net/docs/services/582074fb0dc56116504aed75/operations/5857caeb0dc5610f9ce979e4)

Para más información acerca de cómo insertar widgets en su aplicación, consulte [Embed Video Indexer widgets into your applications](video-indexer-embed-widgets.md) (Inserción de widgets de Video Indexer en sus aplicaciones). 

