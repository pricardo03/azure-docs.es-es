---
title: Introducción a Bing Speech Recognition API con REST | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Uso de REST para tener acceso a Speech Recognition API en Microsoft Cognitive Services para convertir el audio hablado en texto.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: fc40f20349e3424dbcf50f5ca0395b06a818566f
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55224061"
---
# <a name="quickstart-use-the-bing-speech-recognition-rest-api"></a>Inicio rápido: Uso de Bing Speech Recognition API REST

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Con el servicio basado en la nube Bing Speech, puede desarrollar aplicaciones utilizando la API REST para convertir audio hablado en texto.

## <a name="prerequisites"></a>Requisitos previos

### <a name="subscribe-to-the-speech-api-and-get-a-free-trial-subscription-key"></a>Suscripción a Speech API y obtención de una clave de suscripción de prueba gratuita

Speech API forma parte de Cognitive Services (anteriormente Project Oxford). Puede obtener las claves de la suscripción de prueba gratuita en la página [Suscripción a Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). Después de seleccionar Speech API, seleccione **Obtener clave de API** para obtener la clave. Devuelve una clave principal y una clave secundaria. Las dos claves están asociadas a la misma cuota, por lo que puede usar cualquiera de las claves.

> [!IMPORTANT]
>* Obtenga una clave suscripción. Para poder acceder a la API REST, debe tener una [clave de suscripción](https://azure.microsoft.com/try/cognitive-services/).
>
>* Use la clave de su suscripción En los siguientes ejemplos de REST, reemplace YOUR_SUBSCRIPTION_KEY por su propia clave de suscripción. 
>
>* En la página [Autenticación](../how-to/how-to-authentication.md), consulte cómo obtener una clave de suscripción.

### <a name="prerecorded-audio-file"></a>Archivo de audio grabado previamente

En este ejemplo, se utiliza un archivo de audio grabado para ilustrar cómo se usa la API REST. Grabe un archivo de audio en la que diga una frase corta. Por ejemplo, "¿Qué tiempo hace hoy?" o "Buscar películas divertidas para ver". La API de reconocimiento de voz también admite la entrada de micrófono externo.

> [!NOTE]
> El ejemplo requiere que el audio se grabe como un archivo WAV con un **solo canal PCM (mono) a 16 KHz**.

## <a name="build-a-recognition-request-and-send-it-to-the-speech-recognition-service"></a>Creación de una solicitud de reconocimiento y envío al servicio de reconocimiento de voz

El siguiente paso para el reconocimiento de voz es enviar una solicitud POST a los puntos de conexión HTTP de Voz con el encabezado de solicitud y el cuerpo correctos.

### <a name="service-uri"></a>URI de servicio

El URI del servicio de reconocimiento de voz se define según los [modos de reconocimiento](../concepts.md#recognition-modes) y los [idiomas de reconocimiento](../concepts.md#recognition-languages):

```HTTP
https://speech.platform.bing.com/speech/recognition/<RECOGNITION_MODE>/cognitiveservices/v1?language=<LANGUAGE_TAG>&format=<OUTPUT_FORMAT>
```

`<RECOGNITION_MODE>` especifica el modo de reconocimiento y debe ser uno de los siguientes valores: `interactive`, `conversation` o `dictation`. Es una ruta de acceso a los recursos necesarios en el URI. Para más información, consulte [Modos de reconocimiento](../concepts.md#recognition-modes).

`<LANGUAGE_TAG>` es un parámetro necesario en la cadena de consulta. Define el lenguaje de destino de la conversión de audio: por ejemplo, `en-US` para inglés (Estados Unidos). Para más información, consulte [Idiomas de reconocimiento](../concepts.md#recognition-languages).

`<OUTPUT_FORMAT>` es un parámetro opcional en la cadena de consulta. Los valores permitidos son `simple` y `detailed`. De forma predeterminada, el servicio devuelve los resultados con el formato `simple`. Para más información, consulte [Formato de salida](../concepts.md#output-format).

En la tabla siguiente se muestran algunos ejemplos de URI del servicio.

| Modo de reconocimiento  | Idioma | Formato de salida | URI de servicio |
|---|---|---|---|
| `interactive` | pt-BR | Valor predeterminado | https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=pt-BR |
| `conversation` | en-US | Detallado |https://speech.platform.bing.com/speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed |
| `dictation` | fr-FR | Simple | https://speech.platform.bing.com/speech/recognition/dictation/cognitiveservices/v1?language=fr-FR&format=simple |

> [!NOTE]
> El URI del servicio solo es necesario cuando la aplicación utiliza las API REST para llamar al servicio de reconocimiento de voz. Si utiliza una de las [bibliotecas cliente](GetStartedClientLibraries.md), normalmente no es necesario saber qué URI se usa. Las bibliotecas cliente pueden usar otros URI de servicio, que solo se aplican a una biblioteca cliente específica. Para más información, consulte la biblioteca cliente de su elección.

### <a name="request-headers"></a>Encabezados de solicitud

Los siguientes campos deben establecerse en el encabezado de la solicitud:

- `Ocp-Apim-Subscription-Key`: cada vez que llame al servicio, debe pasar su clave de suscripción en el encabezado `Ocp-Apim-Subscription-Key`. El servicio Voz también permite pasar tokens de autorización en lugar de claves de suscripción. Para más información, consulte [Autenticación](../How-to/how-to-authentication.md).
- `Content-type`: el campo `Content-type` describe el formato y el códec de la secuencia de audio. Actualmente, solo se admites los archivos WAV con una codificación PCM Mono 16000. El valor de Content-Type de este formato es `audio/wav; codec=audio/pcm; samplerate=16000`.

El campo `Transfer-Encoding` es opcional. Si establece este campo en `chunked`, puede dividir el audio en pequeños fragmentos. Para más información, consulte [Transferencia fragmentada](../How-to/how-to-chunked-transfer.md).

El siguiente es un ejemplo de encabezado de solicitud:

```HTTP
POST https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: speech.platform.bing.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="send-a-request-to-the-service"></a>Envío de una solicitud al servicio

En el ejemplo siguiente se muestra cómo enviar una solicitud de reconocimiento de voz a los puntos de conexión REST de Voz. Utiliza el modo de reconocimiento `interactive`.

> [!NOTE]
> Reemplace `YOUR_AUDIO_FILE` por la ruta de acceso al archivo de audio grabado previamente. Reemplace `YOUR_SUBSCRIPTION_KEY` por su propia clave de suscripción.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell

$SpeechServiceURI =
'https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed'

# $OAuthToken is the authorization token returned by the token service.
$RecoRequestHeader = @{
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY';
  'Transfer-Encoding' = 'chunked';
  'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
}

# Read audio into byte array
$audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

$RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

# Show the result
$RecoResponse

```

# <a name="curltabcurl"></a>[curl](#tab/curl)

El ejemplo usa curl en Linux con Bash. Si no está disponible en la plataforma, puede que deba instalar curl. El ejemplo también funciona en Cygwin en Windows, Git Bash, zsh y otros shells.

> [!NOTE]
> Mantenga `@` antes del nombre de archivo de audio cuando reemplace `YOUR_AUDIO_FILE` por la ruta de acceso a su archivo de audio grabado, porque indica que el valor de `--data-binary` es un nombre de archivo en lugar de datos.

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";

// Send an audio file by 1024 byte chunks
using (FileStream fs = new FileStream(YOUR_AUDIO_FILE, FileMode.Open, FileAccess.Read))
{

    /*
    * Open a request stream and write 1024 byte chunks in the stream one at a time.
    */
    byte[] buffer = null;
    int bytesRead = 0;
    using (Stream requestStream = request.GetRequestStream())
    {
        /*
        * Read 1024 raw bytes from the input audio file.
        */
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        // Flush
        requestStream.Flush();
    }
}
```

---

## <a name="process-the-speech-recognition-response"></a>Procesamiento de la respuesta de reconocimiento de voz

Después de procesar la solicitud, el servicio Voz devuelve los resultados en una respuesta con el formato JSON.

> [!NOTE]
> Si el código anterior devuelve un error, consulte [Solución de problemas](../troubleshooting.md) para buscar la posible causa.

El fragmento de código siguiente muestra un ejemplo de cómo leer la respuesta de la secuencia.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell
# show the response in JSON format
ConvertTo-Json $RecoResponse
```

# <a name="curltabcurl"></a>[curl](#tab/curl)

En este ejemplo, CURL devuelve directamente el mensaje de respuesta en una cadena. Si desea mostrarla en formato JSON, puede usar herramientas adicionales, por ejemplo, jq.

```
curl -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE | jq
```

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
/*
* Get the response from the service.
*/
Console.WriteLine("Response:");
using (WebResponse response = request.GetResponse())
{
    Console.WriteLine(((HttpWebResponse)response).StatusCode);

    using (StreamReader sr = new StreamReader(response.GetResponseStream()))
    {
        responseString = sr.ReadToEnd();
    }

    Console.WriteLine(responseString);
    Console.ReadLine();
}
```

---

El ejemplo siguiente es una respuesta JSON:

```json
OK
{
  "RecognitionStatus": "Success",
  "Offset": 22500000,
  "Duration": 21000000,
  "NBest": [{
    "Confidence": 0.941552162,
    "Lexical": "find a funny movie to watch",
    "ITN": "find a funny movie to watch",
    "MaskedITN": "find a funny movie to watch",
    "Display": "Find a funny movie to watch."
  }]
}
```

## <a name="limitations"></a>Limitaciones

La API REST tiene algunas limitaciones:

- Solo admite secuencias de audio de hasta 15 segundos.
- No admite resultados intermedios durante el reconocimiento. Los usuarios reciben solamente el resultado final del reconocimiento.

Para eliminar estas limitaciones, utilice las [bibliotecas cliente](GetStartedClientLibraries.md) de Voz. O bien, puede trabajar directamente con el [protocolo WebSocket de Voz](../API-Reference-REST/websocketprotocol.md).

## <a name="whats-next"></a>Pasos siguientes

- Para ver cómo se utiliza la API REST en C#, Java, etc., consulte estas [aplicaciones de ejemplo](../samples.md).
- Si localizar y corregir los errores, consulte [Solución de problemas](../troubleshooting.md).
- Para usar características más avanzadas, consulte cómo trabajar con las [bibliotecas cliente](GetStartedClientLibraries.md) de Voz.

### <a name="license"></a>Licencia

Todos los SDK de Cognitive Services y los ejemplos tienen una licencia de MIT. Para más información, consulte [Licencia](https://github.com/Microsoft/Cognitive-Speech-STT-JavaScript/blob/master/LICENSE.md).
