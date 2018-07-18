---
title: Uso de Text to Speech mediante los servicios Speech | Microsoft Docs
description: Aprenda a usar Text to Speech en el servicio Speech.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 6c358b5a40b1d8e91c2e1af5eb493b13604cf82e
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045065"
---
# <a name="use-text-to-speech-in-speech-service"></a>Uso de "Text to Speech" en el servicio Speech

El servicio Speech proporciona funcionalidad Text to Speech mediante una sencilla solicitud HTTP. El texto que se habla se registra en el punto de conexión adecuado y el servicio devuelve un archivo de audio (`.wav`) que contiene la voz sintetizada. La aplicación puede usar a continuación este audio de la forma deseada.

El cuerpo de la solicitud POST para Text to Speech puede ser texto sin formato (ASCII o UTF8) o un documento [SSML](speech-synthesis-markup.md). Las solicitudes de texto sin formato se hablan con una voz predeterminada. En la mayoría de los casos, querrá usar un cuerpo SSML. La solicitud HTTP debe incluir un token de autorización. 

Aquí se muestran los puntos de conexión regionales de Text to Speech. Use el adecuado para su suscripción.

[!include[](includes/endpoints-text-to-speech.md)]

## <a name="specify-a-voice"></a>Especificación de una voz

Para especificar una voz, use la etiqueta `<voice>` [SSML](speech-synthesis-markup.md). Por ejemplo: 

```xml
<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
  <voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
  </voice>
</speak>
```

Para ver una lista de las voces disponibles y sus nombres, consulte [Voces de Text to Speech](supported-languages.md#text-to-speech).

## <a name="make-a-request"></a>Realización de una solicitud

Una solicitud HTTP Text to Speech se realiza en modo POST con el texto que se va a hablar en el cuerpo de la solicitud. La longitud máxima del cuerpo de la solicitud HTTP es de 1024 caracteres. La solicitud debe tener los siguientes encabezados: 

Encabezado|Valores|Comentarios
-|-|-
|`Content-Type` | `application/ssml+xml` | El formato de texto de entrada.
|`X-Microsoft-OutputFormat`|     `raw-16khz-16bit-mono-pcm`<br>`audio-16khz-16kbps-mono-siren`<br>`riff-16khz-16kbps-mono-siren`<br>`riff-16khz-16bit-mono-pcm`<br>`audio-16khz-128kbitrate-mono-mp3`<br>`audio-16khz-64kbitrate-mono-mp3`<br>`audio-16khz-32kbitrate-mono-mp3`<br>`raw-24khz-16bit-mono-pcm`<br>`riff-24khz-16bit-mono-pcm`<br>`audio-24khz-160kbitrate-mono-mp3`<br>`audio-24khz-96kbitrate-mono-mp3`<br>`audio-24khz-48kbitrate-mono-mp3` | El formato de audio de salida.
|`User-Agent`   |Nombre de la aplicación | El nombre de la aplicación es necesario y debe tener menos de 255 caracteres.
| `Authorization`   | Un token de autorización que se obtienen al presentar su clave de suscripción al servicio de token. Cada token es válido durante diez minutos. Consulte [API REST: Autenticación](rest-apis.md#authentication).

> [!NOTE]
> Si la voz y el formato de salida seleccionados tienen velocidades de bits diferentes, se vuelve a muestrear el audio según sea necesario. Las voces de 24 khz n admiten los formatos de salida `audio-16khz-16kbps-mono-siren` y `riff-16khz-16kbps-mono-siren`. 

A continuación se muestra una solicitud de ejemplo.

```xml
POST /cognitiveservices/v1
HTTP/1.1
Host: westus.tts.speech.microsoft.com
X-Microsoft-OutputFormat: riff-24khz-16bit-mono-pcm
Content-Type: application/ssml+xml
User-Agent: Test TTS application
Authorization: (authorization token)

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice  name='Microsoft Server Speech Text to Speech Voice (en-US, JessaRUS)'>
    Hello, world!
</voice> </speak>
```

El cuerpo de respuesta con un estado de 200 contiene audio en el formato de salida especificado.

```
HTTP/1.1 200 OK
Content-Length: XXX
Content-Type: audio/x-wav

Response audio payload
```

Si se produce un error, se usan los siguientes códigos de estado. El cuerpo de respuesta del error también contiene una descripción del problema.

|Código|DESCRIPCIÓN|Problema|
|-|-|-|
400 |Bad Request |Falta un parámetro requerido, está vacío o es nulo. O bien, el valor pasado a un parámetro obligatorio u opcional no es válido. Un problema común es que el encabezado sea demasiado largo.
401|No autorizado |La solicitud no está autenticada. Asegúrese de que su clave de suscripción o su token sean válidos.
413|Entidad de solicitud demasiado larga|La entrada de SSML tiene más de 1024 caracteres.
|502|Puerta de enlace incorrecta    | Problema de red o de servidor. Podría indicar también encabezados no válidos.

Para más información sobre la API REST de Text to Speech, consulte [API REST](rest-apis.md#text-to-speech).

## <a name="next-steps"></a>Pasos siguientes

- [Obtener su suscripción de prueba a Speech](https://azure.microsoft.com/try/cognitive-services/)
- [Reconocer la voz en C#](quickstart-csharp-windows.md)
