---
title: Referencia de API texto a voz (REST) - servicios de voz
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar la API de REST de texto a voz. En este artículo, obtendrá más información sobre las opciones de autorización y de consulta y sobre cómo estructurar una solicitud y recibir una respuesta.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/26/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 71710cd940aad3a56dae6c19d4d52a5b141b3d80
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60480548"
---
# <a name="text-to-speech-rest-api"></a>Text-to-speech REST API

Los servicios de voz permiten [convertir texto en voz sintetizada](#convert-text-to-speech) y [obtener una lista de admitidos voces](#get-a-list-of-voices) para una región con un conjunto de API de REST. Cada punto de conexión disponible se asocia con una región. Se requiere una clave de suscripción para el punto de conexión o región que se va a usar.

Text to Speech REST API admite voces neuronales y de texto a voz estándar, y cada una de ellas admite un idioma y un dialecto específicos, que se identifican mediante la configuración regional.

* Para ver una lista completa de voces, consulte [compatibilidad con idiomas](language-support.md#text-to-speech).
* Para obtener información acerca de la disponibilidad regional, consulte las [regiones](regions.md#text-to-speech).

> [!IMPORTANT]
> Los costes varían para voces estándar, personalizadas y neuronales. Para obtener más información, consulte el apartado [Precios](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

Antes de utilizar esta API, entender:

* La API REST de texto a voz requiere un encabezado de autorización. Esto significa que tiene que completar un intercambio de tokens para acceder al servicio. Para más información, consulte [Autenticación](#authentication).

[!INCLUDE [](../../../includes/cognitive-services-speech-service-rest-auth.md)]

## <a name="get-a-list-of-voices"></a>Obtener una lista de las voces

El `voices/list` punto de conexión le permite obtener una lista completa de las voces de una región o punto de conexión concreto.

### <a name="regions-and-endpoints"></a>Regiones y puntos de conexión

| Region | Punto de conexión |
|--------|----------|
| Este de Australia | https://australiaeast.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Sur de Brasil | https://brazilsouth.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Centro de Canadá | https://canadacentral.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Centro de EE. UU. | https://centralus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Este de Asia | https://eastasia.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Este de EE. UU. | https://eastus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Este de EE. UU. 2 | https://eastus2.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Centro de Francia | https://francecentral.tts.speech.microsoft.com/cognitiveservices/voices/list |
| India central | https://centralindia.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Este de Japón | https://japaneast.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Corea Central | https://koreacentral.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Centro-Norte de EE. UU | https://northcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Norte de Europa | https://northeurope.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Centro-Sur de EE. UU | https://southcentralus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Sudeste asiático | https://southeastasia.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Sur del Reino Unido 2 | https://uksouth.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Europa occidental | https://westeurope.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Oeste de EE. UU. | https://westus.tts.speech.microsoft.com/cognitiveservices/voices/list |
| Oeste de EE. UU. 2 | https://westus2.tts.speech.microsoft.com/cognitiveservices/voices/list |

### <a name="request-headers"></a>Encabezados de solicitud

Esta tabla enumeran los encabezados necesarios y opcionales para las solicitudes de texto a voz.

| Encabezado | DESCRIPCIÓN | Obligatorio u opcional |
|--------|-------------|---------------------|
| `Authorization` | Un token de autorización precedido por la palabra `Bearer`. Para más información, consulte [Autenticación](#authentication). | Obligatorio |

### <a name="request-body"></a>Cuerpo de la solicitud

No es necesario para un cuerpo `GET` las solicitudes a este punto de conexión.

### <a name="sample-request"></a>Solicitud de ejemplo

Esta solicitud solo requiere un encabezado de autorización.

```http
GET /cognitiveservices/voices/list HTTP/1.1

Host: westus.tts.speech.microsoft.com
Authorization: Bearer [Base64 access_token]
```

### <a name="sample-response"></a>Respuesta de muestra

Esta respuesta se ha truncado para ilustrar la estructura de una respuesta.

> [!NOTE]
> Disponibilidad de voz varía según la región/punto de conexión.

```json
[
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-EG, Hoda)",
        "ShortName": "ar-EG-Hoda",
        "Gender": "Female",
        "Locale": "ar-EG"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ar-SA, Naayf)",
        "ShortName": "ar-SA-Naayf",
        "Gender": "Male",
        "Locale": "ar-SA"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (bg-BG, Ivan)",
        "ShortName": "bg-BG-Ivan",
        "Gender": "Male",
        "Locale": "bg-BG"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (ca-ES, HerenaRUS)",
        "ShortName": "ca-ES-HerenaRUS",
        "Gender": "Female",
        "Locale": "ca-ES"
    },
    {
        "Name": "Microsoft Server Speech Text to Speech Voice (cs-CZ, Jakub)",
        "ShortName": "cs-CZ-Jakub",
        "Gender": "Male",
        "Locale": "cs-CZ"
    },

    ...

]
```

### <a name="http-status-codes"></a>Códigos de estado HTTP

El estado HTTP de cada respuesta indica estados de corrección o error comunes.

| Código de estado HTTP | DESCRIPCIÓN | Posible motivo |
|------------------|-------------|-----------------|
| 200 | OK | La solicitud es correcta. |
| 400 | Bad Request | Falta un parámetro requerido, está vacío o es nulo. O bien, el valor pasado a un parámetro obligatorio u opcional no es válido. Un problema común es que el encabezado sea demasiado largo. |
| 401 | No autorizado | La solicitud no está autenticada. Asegúrese de que la clave de suscripción o el token sean válidos y de la región correcta. |
| 429 | Demasiadas solicitudes | Ha superado la cuota o la tasa de solicitudes permitidas para su suscripción. |
| 502 | Puerta de enlace incorrecta | Problema de red o de servidor. Podría indicar también encabezados no válidos. |


## <a name="convert-text-to-speech"></a>Conversión de texto a voz

El `v1` punto de conexión le permite convertir texto a voz usando [lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md).

### <a name="regions-and-endpoints"></a>Regiones y puntos de conexión

Estas regiones son compatibles con la conversión de texto a voz mediante la API REST. Asegúrese de que selecciona el punto de conexión que coincida con la región de su suscripción.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

### <a name="request-headers"></a>Encabezados de solicitud

Esta tabla enumeran los encabezados necesarios y opcionales para las solicitudes de texto a voz.

| Encabezado | DESCRIPCIÓN | Obligatorio u opcional |
|--------|-------------|---------------------|
| `Authorization` | Un token de autorización precedido por la palabra `Bearer`. Para más información, consulte [Autenticación](#authentication). | Obligatorio |
| `Content-Type` | Especifica el tipo de contenido para el texto proporcionado. Valor aceptable: `application/ssml+xml`. | Obligatorio |
| `X-Microsoft-OutputFormat` | Especifica el formato de salida del audio. Para obtener una lista completa de los valores aceptados, consulte [salidas de audio](#audio-outputs). | Obligatorio |
| `User-Agent` | Nombre de la aplicación. El valor proporcionado debe tener menos de 255 caracteres. | Obligatorio |

### <a name="audio-outputs"></a>Salidas de audio

Esta es una lista de formatos de audio admitidos que se envían en cada solicitud como encabezado `X-Microsoft-OutputFormat`. Cada uno de ellos incorpora una velocidad de bits y el tipo de codificación. Los servicios de voz admite KHz 16, 24 KHz y salidas de audio 8 KHz.

|||
|-|-|
| `raw-16khz-16bit-mono-pcm` | `raw-8khz-8bit-mono-mulaw` |
| `riff-8khz-8bit-mono-alaw` | `riff-8khz-8bit-mono-mulaw` |
| `riff-16khz-16bit-mono-pcm` | `audio-16khz-128kbitrate-mono-mp3` |
| `audio-16khz-64kbitrate-mono-mp3` | `audio-16khz-32kbitrate-mono-mp3` |
| `raw-24khz-16bit-mono-pcm` | `riff-24khz-16bit-mono-pcm` |
| `audio-24khz-160kbitrate-mono-mp3` | `audio-24khz-96kbitrate-mono-mp3` |
| `audio-24khz-48kbitrate-mono-mp3` | |

> [!NOTE]
> Si la voz y el formato de salida seleccionados tienen velocidades de bits diferentes, se vuelve a muestrear el audio según sea necesario. Pero las voces de 24 kHz no admiten los formatos de salida `audio-16khz-16kbps-mono-siren` y `riff-16khz-16kbps-mono-siren`.

### <a name="request-body"></a>Cuerpo de la solicitud

El cuerpo de cada solicitud `POST` se envía como [lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md). SSML le permite elegir la voz y el idioma de la voz sintetizada que devuelve el servicio de texto a voz. Para ver una lista completa de voces compatibles, consulte [compatibilidad con idiomas](language-support.md#text-to-speech).

> [!NOTE]
> Si usa una voz personalizada, el cuerpo de una solicitud puede enviarse como texto sin formato (ASCII o UTF-8).

### <a name="sample-request"></a>Solicitud de ejemplo

Esta solicitud HTTP utiliza SSML para especificar el idioma y la voz. El cuerpo no puede superar los 1000 caracteres.

```http
POST /cognitiveservices/v1 HTTP/1.1

X-Microsoft-OutputFormat: raw-16khz-16bit-mono-pcm
Content-Type: application/ssml+xml
Host: westus.tts.speech.microsoft.com
Content-Length: 225
Authorization: Bearer [Base64 access_token]

<speak version='1.0' xml:lang='en-US'><voice xml:lang='en-US' xml:gender='Female'
    name='Microsoft Server Speech Text to Speech Voice (en-US, ZiraRUS)'>
        Microsoft Speech Service Text-to-Speech API
</voice></speak>
```

Consulte nuestras guías de inicio rápido para ver ejemplos específicos del idioma:

* [.NET Core, C#](quickstart-dotnet-text-to-speech.md)
* [Python](quickstart-python-text-to-speech.md)
* [Node.js](quickstart-nodejs-text-to-speech.md)

### <a name="http-status-codes"></a>Códigos de estado HTTP

El estado HTTP de cada respuesta indica estados de corrección o error comunes.

| Código de estado HTTP | DESCRIPCIÓN | Posible motivo |
|------------------|-------------|-----------------|
| 200 | OK | La solicitud es correcta; el cuerpo de la respuesta es un archivo de audio. |
| 400 | Bad Request | Falta un parámetro requerido, está vacío o es nulo. O bien, el valor pasado a un parámetro obligatorio u opcional no es válido. Un problema común es que el encabezado sea demasiado largo. |
| 401 | No autorizado | La solicitud no está autenticada. Asegúrese de que la clave de suscripción o el token sean válidos y de la región correcta. |
| 413 | Entidad de solicitud demasiado larga | La entrada de SSML tiene más de 1024 caracteres. |
| 429 | Demasiadas solicitudes | Ha superado la cuota o la tasa de solicitudes permitidas para su suscripción. |
| 502 | Puerta de enlace incorrecta | Problema de red o de servidor. Podría indicar también encabezados no válidos. |

Si el estado HTTP es `200 OK`, el cuerpo de la respuesta contiene un archivo de audio en el formato solicitado. Este archivo se puede reproducir mientras se transfiere o guardarse en un búfer o en un archivo.

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
