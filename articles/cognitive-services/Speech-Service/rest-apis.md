---
title: API REST del servicio Speech
description: Referencia de las API REST del servicio Speech.
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: speech
ms.topic: article
ms.date: 05/09/2018
ms.author: erhopf
ms.openlocfilehash: f8b27277cbf3ea6d53a8f02e550beae67fc50741
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167637"
---
# <a name="speech-service-rest-apis"></a>API REST del servicio Speech

Las API de REST del servicio de voz de Azure Cognitive Services son similares a las API que proporciona [Bing Speech API](https://docs.microsoft.com/azure/cognitive-services/Speech). Los puntos de conexión son distintos a los usados por el servicio Bing Speech. Hay puntos de conexión regionales disponibles, y debe usar una clave de suscripción que corresponda al punto de conexión que esté usando.

## <a name="speech-to-text"></a>Speech to Text

Los puntos de conexión de la API de REST de conversión de voz en texto se muestran en la tabla siguiente. Use el que coincida con su región de suscripción. 

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

> [!NOTE]
> Si ha personalizado el modelo acústico o el modelo de lenguaje, o la pronunciación, use en su lugar su punto de conexión personalizado.

Esta API admite solo expresiones cortas. Las solicitudes pueden contener hasta 10 segundos de audio y pueden durar 14 segundos como máximo en general. La API de REST solo devuelve resultados finales, ni parciales ni provisionales. El servicio de voz también tiene una API de [transcripción de lotes](batch-transcription.md) que puede transcribir fragmentos de audio más largos.


### <a name="query-parameters"></a>Parámetros de consulta

Los parámetros siguientes podrían incluirse en la cadena de consulta de la solicitud de REST.

|Nombre de parámetro|Obligatorio/opcional|Significado|
|-|-|-|
|`language`|Obligatorio|Identificador del idioma que se va a reconocer. Vea [Idiomas admitidos](language-support.md#speech-to-text).|
|`format`|Opcional<br>valor predeterminado: `simple`|Formato del resultado, `simple` o `detailed`. Los resultados simples incluyen `RecognitionStatus`, `DisplayText`, `Offset` y duración. Los resultados detallados incluyen varios candidatos con valores de confianza y cuatro representaciones diferentes.|
|`profanity`|Opcional<br>valor predeterminado: `masked`|Cómo controlar las palabras soeces en los resultados del reconocimiento. Puede ser `masked` (sustituye las palabras soeces por asteriscos), `removed` (quita todas las palabras soeces) o `raw` (incluye palabras soeces).

### <a name="request-headers"></a>Encabezados de solicitud

Los siguientes campos se envían en el encabezado de la solicitud HTTP.

|Encabezado|Significado|
|------|-------|
|`Ocp-Apim-Subscription-Key`|Clave de suscripción del servicio de voz. Se debe proporcionar este encabezado o `Authorization`.|
|`Authorization`|Un token de autorización precedido por la palabra `Bearer`. Se debe proporcionar este encabezado o `Ocp-Apim-Subscription-Key`. Consulte [Autenticación](#authentication).|
|`Content-type`|Describe el formato y el códec de los datos de audio. De momento, este valor debe ser `audio/wav; codec=audio/pcm; samplerate=16000`.|
|`Transfer-Encoding`|Opcional. Si se proporciona, debe ser `chunked` para permitir que los datos de audio se envíen en varios fragmentos pequeños en lugar de en un único archivo.|
|`Expect`|Si usa la transferencia fragmentada, envíe `Expect: 100-continue`. El servicio de voz confirma la solicitud inicial y espera datos adicionales.|
|`Accept`|Opcional. Si se proporciona, debe incluir `application/json`, ya que el servicio de voz proporciona los resultados en formato JSON. (Algunos marcos de trabajo de solicitud web proporcionan un valor predeterminado incompatible si no se especifica uno, por lo que es recomendable incluir siempre `Accept`).|

### <a name="audio-format"></a>Formato de audio

El audio se envía en el cuerpo de la solicitud HTTP `PUT`. Debe estar en formato WAV de 16 bits con canal único PCM (mono) a 16 kHz de los siguientes formatos/codificaciones.

* Formato WAV con el códec PCM
* Formato Ogg con el códec OPUS

>[!NOTE]
>Se admiten los formatos anteriores a través de la API de REST y WebSocket en el servicio de voz. El [SDK de Voz](/index.yml) actualmente solo admite el formato WAV con el códec PCM. 

### <a name="chunked-transfer"></a>Transferencia fragmentada

La transferencia fragmentada (`Transfer-Encoding: chunked`) puede ayudar a reducir la latencia de reconocimiento, ya que permite que el servicio de voz comience a procesar el archivo de audio mientras se transmite. La API de REST no proporciona resultados parciales ni provisionales. Esta opción está diseñada exclusivamente para mejorar la capacidad de respuesta.

El siguiente código muestra cómo enviar audio en fragmentos. Solo el primer fragmento debe contener el encabezado del archivo de audio. `request` es un objeto HTTPWebRequest conectado al punto de conexión de REST adecuado. `audioFile` es la ruta de acceso a un archivo de audio en disco.

```csharp
using (fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
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

### <a name="example-request"></a>Solicitud de ejemplo

La siguiente es una solicitud típica.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="http-status"></a>Estado de HTTP.

El estado HTTP de la respuesta indica estados de corrección o error comunes.

Código HTTP|Significado|Posible motivo
-|-|-|
100|Continuar|Se ha aceptado la solicitud inicial. Continúe con el envío del resto de los datos. (Se usa con la transferencia fragmentada).
200|OK|La solicitud es correcta; el cuerpo de la respuesta es un objeto JSON.
400|Solicitud incorrecta|Código de idioma no proporcionado o idioma no compatible; archivo de audio no válido.
401|No autorizado|Clave de suscripción o token de autorización no válido en la región especificada, o punto de conexión no válido.
403|Prohibido|Falta la clave de suscripción o el token de autorización.

### <a name="json-response"></a>Respuesta JSON

Los resultados se devuelven en formato JSON. El formato `simple` incluye solo los siguientes campos de nivel superior.

|Nombre del campo|Contenido|
|-|-|
|`RecognitionStatus`|Estado, como `Success`, para un reconocimiento correcto. Vea la tabla siguiente.|
|`DisplayText`|Texto reconocido tras mayúsculas, puntuación, normalización inversa de texto (conversión de texto hablado en formularios más cortos, como 200 para "doscientos" o "Dr. Smith" para "doctor smith") y enmascaramiento de palabras soeces. Solo se presenta en caso de corrección.|
|`Offset`|El tiempo (en unidades de 100 nanosegundos) en el que comienza la voz reconocida en la secuencia de audio.|
|`Duration`|La duración (en unidades de 100 nanosegundos) de la voz reconocida en la secuencia de audio.|

El campo `RecognitionStatus` puede contener los siguientes valores.

|Valor de estado|DESCRIPCIÓN
|-|-|
| `Success` | El reconocimiento es correcto y el campo DisplayText está presente. |
| `NoMatch` | Se detectó voz en la secuencia de audio, pero no se encontraron coincidencias de palabras en el idioma de destino. Normalmente significa que el idioma de reconocimiento es un idioma distinto al que habla el usuario. |
| `InitialSilenceTimeout` | El inicio de la secuencia de audio contiene solo silencio y el servicio ha agotado el tiempo de espera de la voz. |
| `BabbleTimeout` | El inicio de la secuencia de audio contiene solo ruido y el servicio ha agotado el tiempo de espera de la voz. |
| `Error` | El servicio de reconocimiento ha detectado un error interno y no ha podido continuar. Vuelva a intentarlo si es posible. |

> [!NOTE]
> Si el audio consta solo de palabras soeces y el parámetro de consulta `profanity` está establecido en `remove`, el servicio no devuelve ningún resultado de voz. 


El formato `detailed` incluye los mismos campos que el formato `simple`, además de un campo `NBest`. El campo `NBest` es una lista de interpretaciones alternativas de la misma de voz, clasificadas de la más a la menos probable. La primera entrada es la misma que el resultado de reconocimiento principal. Cada entrada contiene los siguientes campos:

|Nombre del campo|Contenido|
|-|-|
|`Confidence`|La puntuación de confianza de la entrada de 0,0 (ninguna confianza) a 1,0 (plena confianza)
|`Lexical`|La forma léxica del texto reconocido: palabras reales reconocidas.
|`ITN`|El formato de normalización inversa de texto ("canónica") del texto reconocido, con números de teléfono, números, abreviaturas ("doctor smith" a "dr smith") y otras transformaciones aplicadas.
|`MaskedITN`| Formato ITN con enmascaramiento de palabras soeces aplicado, si se solicita.
|`Display`| Formato de presentación del texto reconocido, con adición de signos de puntuación y mayúsculas. Igual que `DisplayText` en el resultado de nivel superior.

### <a name="sample-responses"></a>Respuestas de ejemplo

La siguiente es una respuesta típica de reconocimiento `simple`.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

La siguiente es una respuesta típica de reconocimiento `detailed`.

```json
{
  "RecognitionStatus": "Success",
  "Offset": "1236645672289",
  "Duration": "1236645672289",
  "NBest": [
      {
        "Confidence" : "0.87",
        "Lexical" : "remind me to buy five pencils",
        "ITN" : "remind me to buy 5 pencils",
        "MaskedITN" : "remind me to buy 5 pencils",
        "Display" : "Remind me to buy 5 pencils.",
      },
      {
        "Confidence" : "0.54",
        "Lexical" : "rewind me to buy five pencils",
        "ITN" : "rewind me to buy 5 pencils",
        "MaskedITN" : "rewind me to buy 5 pencils",
        "Display" : "Rewind me to buy 5 pencils.",
      }
  ]
}
```

## <a name="text-to-speech"></a>Texto a voz

Estos son los puntos de conexión de REST de Text to Speech API del servicio de voz. Use el que coincida con su región de suscripción.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

El servicio de voz admite la salida de audio de 24 kHz además de la salida de 16 kHz compatible con Bing Speech. Hay cuatro formatos de salida de 24 kHz disponibles para su uso en el encabezado HTTP `X-Microsoft-OutputFormat`, además de dos voces de 24 kHz, `Jessa24kRUS` y `Guy24kRUS`.

Configuración regional | Idioma   | Sexo | Asignación de nombre de servicio
-------|------------|--------|------------
es-ES  | English (Estados Unidos) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)" 
es-ES  | English (Estados Unidos) | Hombre   | "Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)"

Hay una lista completa de voces disponibles en [Idiomas admitidos](language-support.md#text-to-speech).

### <a name="request-headers"></a>Encabezados de solicitud

Los siguientes campos se envían en el encabezado de la solicitud HTTP.

|Encabezado|Significado|
|------|-------|
|`Authorization`|Un token de autorización precedido por la palabra `Bearer`. Necesario. Consulte [Autenticación](#authentication).|
|`Content-Type`|El tipo de contenido de entrada: `application/ssml+xml`.|
|`X-Microsoft-OutputFormat`|El formato de audio de salida. Vea la tabla siguiente.|
|`User-Agent`|Nombre de la aplicación. Obligatorio; debe contener menos de 255 caracteres.|

Los formatos de salida de audio disponibles (`X-Microsoft-OutputFormat`) incorporan una velocidad de bits y una codificación.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `raw-8khz-8bit-mono-mulaw`
`riff-8khz-8bit-mono-mulaw`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

> [!NOTE]
> Si la voz y el formato de salida seleccionados tienen velocidades de bits diferentes, se vuelve a muestrear el audio según sea necesario. Pero las voces de 24 kHz no admiten los formatos de salida `audio-16khz-16kbps-mono-siren` y `riff-16khz-16kbps-mono-siren`. 

### <a name="request-body"></a>Cuerpo de la solicitud

El texto que se va a convertir en voz se envía como cuerpo de una solicitud HTTP `POST` en texto sin formato (ASCII o UTF-8) o formato [Speech Synthesis Markup Language](speech-synthesis-markup.md) (SSML) (UTF-8). Las solicitudes de texto sin formato usan la voz y el idioma predeterminados del servicio. Envíe SSML para usar otra voz.

### <a name="sample-request"></a>Solicitud de ejemplo

La solicitud HTTP siguiente usa un cuerpo SSML para elegir la voz. El cuerpo no debe superar los 1 000 caracteres.

```xml
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

### <a name="http-response"></a>Respuesta HTTP

El estado HTTP de la respuesta indica estados de corrección o error comunes.

Código HTTP|Significado|Posible motivo
-|-|-|
200|OK|La solicitud es correcta; el cuerpo de la respuesta es un archivo de audio.
400 |Bad Request |Falta un parámetro requerido, está vacío o es nulo. O bien, el valor pasado a un parámetro obligatorio u opcional no es válido. Un problema común es que el encabezado sea demasiado largo.
401|No autorizado |La solicitud no está autenticada. Asegúrese de que la clave de suscripción o el token sean válidos y de la región correcta.
413|Entidad de solicitud demasiado larga|La entrada de SSML tiene más de 1024 caracteres.
429|Demasiadas solicitudes|Ha superado la cuota o la tasa de solicitudes permitidas para su suscripción.
502|Puerta de enlace incorrecta | Problema de red o de servidor. Podría indicar también encabezados no válidos.

Si el estado HTTP es `200 OK`, el cuerpo de la respuesta contiene un archivo de audio en el formato solicitado. Este archivo se puede reproducir mientras se transfiere o guardarse en un búfer o archivo para su posterior reproducción u otro uso.

## <a name="authentication"></a>Autenticación

Para enviar una solicitud a la API de REST del servicio de voz se necesita una clave de suscripción o un token de acceso. En general, es más fácil enviar la clave de suscripción directamente. El servicio de voz luego obtiene el token de acceso automáticamente. Para minimizar el tiempo de respuesta, es posible que quiera usar un token de acceso en su lugar.

Para obtener un token, presente la clave de suscripción a un punto de conexión `issueToken` del servicio de voz regional, como se muestra en la tabla siguiente. Use el que coincida con su región de suscripción.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Cada token de acceso tiene una validez de 10 minutos. Puede obtener un nuevo token en cualquier momento. Si quiere, puede obtener un token justo antes de cada solicitud de API de REST de voz. Para minimizar el tráfico de red y la latencia, se recomienda usar el mismo token durante nueve minutos.

En las secciones siguientes se muestra cómo obtener un token y cómo usarlo en una solicitud.

### <a name="get-a-token-http"></a>Obtener un token: HTTP

El siguiente es un ejemplo de solicitud HTTP para obtener un token. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Speech. Si la suscripción no está en la región Oeste de EE. UU., reemplace el encabezado `Host` por el nombre de host de la región.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

El cuerpo de la respuesta a esta solicitud es el token de acceso en formato Java Web Token (JWT).

### <a name="get-a-token-powershell"></a>Obtener un token: PowerShell

El siguiente script de Windows PowerShell muestra cómo obtener un token de acceso. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Speech. Si la suscripción no está en la región Oeste de EE. UU., cambie el nombre de host del URI proporcionado en consecuencia.

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken
 -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

### <a name="get-a-token-curl"></a>Obtener un token: cURL

cURL es una herramienta de la línea de comandos disponible en Linux (y en el subsistema Windows para Linux). El comando cURL siguiente muestra cómo obtener un token de acceso. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Speech. Si la suscripción no está en la región Oeste de EE. UU., cambie el nombre de host del URI proporcionado en consecuencia.

> [!NOTE]
> El comando se muestra en varias líneas por motivos de legibilidad, pero escríbalo en una única línea en el símbolo del sistema de Shell.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="get-a-token-c"></a>Obtener un token: C#

La clase C# siguiente muestra cómo obtener un token de acceso. Pase la clave de suscripción del servicio de voz al crear una instancia de la clase. Si la suscripción no está en la región Oeste de EE. UU., cambie el nombre de host de `FetchTokenUri` en consecuencia.

```cs
/*
    * This class demonstrates how to get a valid access token.
    */
public class Authentication
{
    public static readonly string FetchTokenUri =
        "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
    private string subscriptionKey;
    private string token;

    public Authentication(string subscriptionKey)
    {
        this.subscriptionKey = subscriptionKey;
        this.token = FetchTokenAsync(FetchTokenUri, subscriptionKey).Result;
    }

    public string GetAccessToken()
    {
        return this.token;
    }

    private async Task<string> FetchTokenAsync(string fetchUri, string subscriptionKey)
    {
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            UriBuilder uriBuilder = new UriBuilder(fetchUri);

            var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
            Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
            return await result.Content.ReadAsStringAsync();
        }
    }
}
```

### <a name="use-a-token"></a>Uso de un token

Para usar un token en una solicitud de API REST, proporciónelo en el encabezado `Authorization`, después de la palabra `Bearer`. Esta es una solicitud de API de REST Text to Speech de ejemplo que contiene un token. Sustituya el token real por `YOUR_ACCESS_TOKEN`. Use el nombre de host correcto en el encabezado `Host`.

```xml
POST /cognitiveservices/v1 HTTP/1.1
Authorization: Bearer YOUR_ACCESS_TOKEN
Host: westus.tts.speech.microsoft.com
Content-type: application/ssml+xml
Content-Length: 199
Connection: Keep-Alive

<speak version='1.0' xmlns="http://www.w3.org/2001/10/synthesis" xml:lang='en-US'>
<voice name='Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)'>
    Hello, world!
</voice></speak>
```

### <a name="renew-authorization"></a>Renovar la autorización

El token de autorización expira al cabo de 10 minutos. Para renovar la autorización, obtenga un nuevo token antes de que expire. Por ejemplo, puede obtener un nuevo token después de nueve minutos.

El siguiente código de C# es un sustituto directo de la clase presentada anteriormente. La clase `Authentication` obtiene automáticamente un nuevo token de acceso cada nueve minutos mediante un temporizador. Este enfoque garantiza que siempre esté disponible un token válido mientras se ejecuta el programa.

> [!NOTE]
> En lugar de usar un temporizador, puede almacenar una marca de tiempo de cuándo se obtuvo el último token. Luego puede solicitar uno nuevo únicamente si está a punto de expirar. Este enfoque evita solicitar nuevos tokens innecesariamente y puede ser más adecuado en el caso de programas que realizan solicitudes de voz poco frecuentes.

Al igual que antes, asegúrese de que el valor `FetchTokenUri` coincida con su región de suscripción. Pase la clave de suscripción al crear una instancia de la clase.

```cs
/*
    * This class demonstrates how to maintain a valid access token.
    */
public class Authentication
{
    public static readonly string FetchTokenUri = 
        "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken";
    private string subscriptionKey;
    private string token;
    private Timer accessTokenRenewer;

    //Access token expires every 10 minutes. Renew it every 9 minutes.
    private const int RefreshTokenDuration = 9;

    public Authentication(string subscriptionKey)
    {
        this.subscriptionKey = subscriptionKey;
        this.token = FetchToken(FetchTokenUri, subscriptionKey).Result;

        // renew the token on set duration.
        accessTokenRenewer = new Timer(new TimerCallback(OnTokenExpiredCallback),
                                        this,
                                        TimeSpan.FromMinutes(RefreshTokenDuration),
                                        TimeSpan.FromMilliseconds(-1));
    }

    public string GetAccessToken()
    {
        return this.token;
    }

    private void RenewAccessToken()
    {
        this.token = FetchToken(FetchTokenUri, this.subscriptionKey).Result;
        Console.WriteLine("Renewed token.");
    }

    private void OnTokenExpiredCallback(object stateInfo)
    {
        try
        {
            RenewAccessToken();
        }
        catch (Exception ex)
        {
            Console.WriteLine(string.Format("Failed renewing access token. Details: {0}", ex.Message));
        }
        finally
        {
            try
            {
                accessTokenRenewer.Change(TimeSpan.FromMinutes(RefreshTokenDuration), TimeSpan.FromMilliseconds(-1));
            }
            catch (Exception ex)
            {
                Console.WriteLine(string.Format("Failed to reschedule the timer to renew access token. Details: {0}", ex.Message));
            }
        }
    }

    private async Task<string> FetchToken(string fetchUri, string subscriptionKey)
    {
        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            UriBuilder uriBuilder = new UriBuilder(fetchUri);

            var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
            Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
            return await result.Content.ReadAsStringAsync();
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)

