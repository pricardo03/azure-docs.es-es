---
title: 'API REST de servicios de Voz: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a usar las API REST de voz a texto y texto a voz. En este artículo, obtendrá más información sobre las opciones de autorización y de consulta y sobre cómo estructurar una solicitud y recibir una respuesta.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/13/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 0b38c61f4fe884137204cba6d99d5e383b3259a0
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338897"
---
# <a name="speech-service-rest-apis"></a>API REST del servicio Voz

Como alternativa al [SDK de Voz](speech-sdk.md), el servicio Voz le permite convertir voz a texto y texto a voz con un conjunto de API REST. Cada punto de conexión accesible se asocia con una región. La aplicación requiere una clave de suscripción para el punto de conexión que se va a usar.

Antes de usar las API REST debe saber que:
* Las solicitudes de voz a texto mediante la API REST solo pueden contener 10 segundos de audio grabado.
* La API REST de voz a texto solo devuelve resultados finales. No se proporcionan resultados parciales.
* La API REST de texto a voz requiere un encabezado de autorización. Esto significa que tiene que completar un intercambio de tokens para acceder al servicio. Para más información, consulte [Autenticación](#authentication).

## <a name="authentication"></a>Autenticación

Cada solicitud a la API REST de voz a texto o texto a voz requiere un encabezado de autorización. Esta tabla muestra qué encabezados son compatibles con cada servicio:

| Encabezados de autorización compatibles | Voz a texto | Texto a voz |
|------------------------|----------------|----------------|
| Ocp-Apim-Subscription-Key | SÍ | Sin  |
| Autorización: Portador | SÍ | SÍ |

Cuando se usa el encabezado `Ocp-Apim-Subscription-Key`, solo se le pide que proporcione la clave de suscripción. Por ejemplo: 

```
'Ocp-Apim-Subscription-Key': 'YOUR_SUBSCRIPTION_KEY'
```

Cuando se usa el encabezado `Authorization: Bearer`, se le pide que haga una solicitud al punto de conexión `issueToken`. En esta solicitud, va a intercambiar la clave de suscripción de un token de acceso que es válido durante 10 minutos. En las secciones siguientes aprenderá a obtener, usar y actualizar un token.

### <a name="how-to-get-an-access-token"></a>Obtención de un token de acceso

Para obtener un token de acceso, tiene que realizar una solicitud al punto de conexión `issueToken` mediante `Ocp-Apim-Subscription-Key` y su clave de suscripción.

Se admiten estas regiones y puntos de conexión:

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Use estos ejemplos para crear la solicitud de token de acceso.

#### <a name="http-sample"></a>Ejemplo de HTTP

Este ejemplo es una solicitud HTTP para obtener un token. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Voz. Si la suscripción no está en la región Oeste de EE. UU., reemplace el encabezado `Host` por el nombre de host de la región.

```http
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

El cuerpo de la respuesta contiene el token de acceso en formato Java Web Token (JWT).

#### <a name="powershell-sample"></a>Ejemplo de PowerShell

En este ejemplo se muestra un script sencillo de PowerShell para obtener un token de acceso. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Voz. Asegúrese de usar el punto de conexión correcto para la región que coincida con su suscripción. En este ejemplo la región es Oeste de EE. UU.

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

#### <a name="curl-sample"></a>Ejemplo de cURL

cURL es una herramienta de la línea de comandos disponible en Linux (y en el subsistema Windows para Linux). Este comando cURL muestra cómo obtener un token de acceso. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Voz. Asegúrese de usar el punto de conexión correcto para la región que coincida con su suscripción. En este ejemplo la región es Oeste de EE. UU.

```cli
curl -v -X POST
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" \
 -H "Content-type: application/x-www-form-urlencoded" \
 -H "Content-Length: 0" \
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

#### <a name="c-sample"></a>Ejemplo de C#

Esta clase de C# muestra cómo obtener un token de acceso. Pase la clave de suscripción del servicio Voz al crear una instancia de la clase. Si su suscripción no está en la región Oeste de EE. UU., cambie el valor de `FetchTokenUri` para que coincida con la región de su suscripción.

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

### <a name="how-to-use-an-access-token"></a>Uso de un token de acceso

Se debe enviar el token de acceso al servicio como encabezado `Authorization: Bearer <TOKEN>`. Cada token de acceso tiene una validez de 10 minutos. Puede obtener un nuevo token en cualquier momento. No obstante, para reducir el tráfico de red y la latencia, se recomienda usar el mismo token durante nueve minutos.

Este es un ejemplo de solicitud HTTP a la API REST de texto a voz:

```http
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

### <a name="how-to-renew-an-access-token-using-c"></a>Renovación de un token de acceso mediante C#

Este código de C# es un sustituto directo de la clase presentada anteriormente. La clase `Authentication` obtiene automáticamente un nuevo token de acceso cada nueve minutos mediante un temporizador. Este enfoque garantiza que siempre esté disponible un token válido mientras se ejecuta el programa.

> [!NOTE]
> En lugar de usar un temporizador, puede almacenar una marca de tiempo de cuándo se obtuvo el último token. Luego puede solicitar uno nuevo únicamente si está a punto de expirar. Este enfoque evita solicitar nuevos tokens innecesariamente y puede ser más adecuado en el caso de programas que realizan solicitudes de voz poco frecuentes.

Al igual que antes, asegúrese de que el valor `FetchTokenUri` coincida con su región de suscripción. Pase la clave de suscripción al crear una instancia de la clase.

```cs
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

## <a name="speech-to-text-api"></a>Speech-to-text API

La API REST de voz a texto solo admite expresiones cortas. Las solicitudes pueden contener hasta 10 segundos de audio y durar 14 segundos como máximo. La API REST solo devuelve resultados finales, ni parciales ni provisionales.

Si el envío de un audio más grande es necesario para la aplicación, considere la posibilidad de usar el [SDK de Voz](speech-sdk.md) o la [transcripción por lotes](batch-transcription.md).

### <a name="regions-and-endpoints"></a>Regiones y puntos de conexión

Estas regiones son compatibles con la transcripción de voz a texto mediante la API REST. Asegúrese de que selecciona el punto de conexión que coincida con la región de su suscripción.

[!INCLUDE [](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

### <a name="query-parameters"></a>Parámetros de consulta

Estos parámetros podrían incluirse en la cadena de consulta de la solicitud de REST.

| Parámetro | DESCRIPCIÓN | Obligatorio u opcional |
|-----------|-------------|---------------------|
| `language` | Identifica el idioma hablado que se está reconociendo. Vea [Idiomas admitidos](language-support.md#speech-to-text). | Obligatorio |
| `format` | Especifica el formato del resultado. Los valores aceptados son: `simple` y `detailed`. Los resultados simples incluyen `RecognitionStatus`, `DisplayText`, `Offset` y `Duration`. Las respuestas detalladas incluyen varios resultados con valores de confianza y cuatro representaciones diferentes. La configuración predeterminada es `simple`. | Opcional |
| `profanity` | Especifica cómo controlar las palabras soeces en los resultados del reconocimiento. Los valores aceptados son `masked`, que reemplaza las palabras soeces con asteriscos, `removed`, que quita todas las palabras soeces del resultado o `raw` que incluye la palabra soez en el resultado. La configuración predeterminada es `masked`. | Opcional |

### <a name="request-headers"></a>Encabezados de solicitud

Esta tabla enumera los encabezados obligatorios y opcionales para las solicitudes de voz a texto.

|Encabezado| DESCRIPCIÓN | Obligatorio u opcional |
|------|-------------|---------------------|
| `Ocp-Apim-Subscription-Key` | Clave de suscripción del servicio Voz. | Se necesita este encabezado, o bien `Authorization`. |
| `Authorization` | Un token de autorización precedido por la palabra `Bearer`. Para más información, consulte [Autenticación](#authentication). | Se necesita este encabezado, o bien `Ocp-Apim-Subscription-Key`. |
| `Content-type` | Describe el formato y el códec de los datos de audio proporcionados. Los valores aceptados son: `audio/wav; codec=audio/pcm; samplerate=16000` y `audio/ogg; codec=audio/pcm; samplerate=16000`. | Obligatorio |
| `Transfer-Encoding` | Especifica que se están enviando datos de audio fragmentados en lugar de un único archivo. Use este encabezado solo si hay fragmentación de los datos de audio. | Opcional |
| `Expect` | Si usa la transferencia fragmentada, envíe `Expect: 100-continue`. El servicio Voz confirma la solicitud inicial y espera datos adicionales.| Obligatorio si se envían datos de audio fragmentados. |
| `Accept` | Si se proporciona, debe ser `application/json`. El servicio Voz proporciona resultados en JSON. Algunas plataformas de solicitud web proporcionan un valor predeterminado incompatible si no se especifica uno, por lo que es recomendable incluir siempre `Accept`. | Opcional pero recomendable. |

### <a name="audio-formats"></a>Formatos de audio

El audio se envía en el cuerpo de la solicitud HTTP `POST`. Debe estar en uno de los formatos de esta tabla:

| Formato | Códec | Bitrate | Velocidad de muestreo |
|--------|-------|---------|-------------|
| WAV | PCM | 16 bits | 16 kHz, mono |
| OGG | OPUS | 16 bits | 16 kHz, mono |

>[!NOTE]
>Se admiten los formatos anteriores a través de la API de REST y WebSocket en el servicio de voz. El [SDK de Voz](speech-sdk.md) actualmente solo admite el formato WAV con el códec PCM.

### <a name="sample-request"></a>Solicitud de ejemplo

Esta es una solicitud HTTP típica. El ejemplo siguiente incluye el nombre de host y los encabezados necesarios. Es importante tener en cuenta que el servicio también espera datos de audio, que no están incluidos en este ejemplo. Como se ha mencionado anteriormente, la fragmentación es recomendable pero no obligatoria.

```HTTP
POST speech/recognition/conversation/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.stt.speech.microsoft.com
Transfer-Encoding: chunked
Expect: 100-continue
```

### <a name="http-status-codes"></a>Códigos de estado HTTP

El estado HTTP de cada respuesta indica estados de corrección o error comunes.

| Código de estado HTTP | DESCRIPCIÓN | Posible motivo |
|------------------|-------------|-----------------|
| 100 | Continuar | Se ha aceptado la solicitud inicial. Continúe con el envío del resto de los datos. (Se usa con la transferencia fragmentada). |
| 200 | OK | La solicitud es correcta; el cuerpo de la respuesta es un objeto JSON. |
| 400 | Solicitud incorrecta | Código de idioma no proporcionado o idioma no compatible; archivo de audio no válido. |
| 401 | No autorizado | Clave de suscripción o token de autorización no válido en la región especificada, o punto de conexión no válido. |
| 403 | Prohibido | Falta la clave de suscripción o el token de autorización. |

### <a name="chunked-transfer"></a>Transferencia fragmentada

La transferencia fragmentada (`Transfer-Encoding: chunked`) puede ayudar a reducir la latencia de reconocimiento, ya que permite que el servicio Voz comience a procesar el archivo de audio mientras se transmite. La API de REST no proporciona resultados parciales ni provisionales. Esta opción está diseñada exclusivamente para mejorar la capacidad de respuesta.

Este ejemplo de código muestra cómo enviar audio en fragmentos. Solo el primer fragmento debe contener el encabezado del archivo de audio. `request` es un objeto HTTPWebRequest conectado al punto de conexión de REST adecuado. `audioFile` es la ruta de acceso a un archivo de audio en disco.

```csharp

    HttpWebRequest request = null;
    request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
    request.SendChunked = true;
    request.Accept = @"application/json;text/xml";
    request.Method = "POST";
    request.ProtocolVersion = HttpVersion.Version11;
    request.Host = host;
    request.ContentType = @"audio/wav; codec=""audio/pcm""; samplerate=16000";
    request.Headers["Ocp-Apim-Subscription-Key"] = args[1];
    request.AllowWriteStreamBuffering = false;

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

### <a name="response-parameters"></a>Parámetros de respuesta

Los resultados se proporcionan como JSON. El formato `simple` incluye los siguientes campos de nivel superior.

| Parámetro | DESCRIPCIÓN  |
|-----------|--------------|
|`RecognitionStatus`|Estado, como `Success`, para un reconocimiento correcto. Vea la tabla siguiente.|
|`DisplayText`|Texto reconocido tras mayúsculas, puntuación, normalización inversa de texto (conversión de texto hablado en formularios más cortos, como 200 para "doscientos" o "Dr. Smith" para "doctor smith") y enmascaramiento de palabras soeces. Solo se presenta en caso de corrección.|
|`Offset`|El tiempo (en unidades de 100 nanosegundos) en el que comienza la voz reconocida en la secuencia de audio.|
|`Duration`|La duración (en unidades de 100 nanosegundos) de la voz reconocida en la secuencia de audio.|

El campo `RecognitionStatus` puede contener estos valores:

| Status | DESCRIPCIÓN |
|--------|-------------|
| `Success` | El reconocimiento es correcto y el campo `DisplayText` está presente. |
| `NoMatch` | Se detectó voz en la secuencia de audio, pero no se encontraron coincidencias de palabras en el idioma de destino. Normalmente significa que el idioma de reconocimiento es un idioma distinto al que habla el usuario. |
| `InitialSilenceTimeout` | El inicio de la secuencia de audio contiene solo silencio y el servicio ha agotado el tiempo de espera de la voz. |
| `BabbleTimeout` | El inicio de la secuencia de audio contiene solo ruido y el servicio ha agotado el tiempo de espera de la voz. |
| `Error` | El servicio de reconocimiento ha detectado un error interno y no ha podido continuar. Vuelva a intentarlo si es posible. |

> [!NOTE]
> Si el audio consta solo de palabras soeces y el parámetro de consulta `profanity` está establecido en `remove`, el servicio no devuelve ningún resultado de voz.

El formato `detailed` incluye los mismos datos que el formato `simple`, junto con `NBest`, una lista de interpretaciones alternativas del mismo resultado de reconocimiento de voz. Estos resultados se clasifican de mayor probabilidad a menor. La primera entrada es la misma que el resultado de reconocimiento principal.  Cuando se usa el formato `detailed`, se proporciona `DisplayText` como `Display` para cada resultado de la lista `NBest`.

Cada objeto de la lista `NBest` incluye:

| Parámetro | DESCRIPCIÓN |
|-----------|-------------|
| `Confidence` | La puntuación de confianza de la entrada de 0,0 (ninguna confianza) a 1,0 (plena confianza) |
| `Lexical` | La forma léxica del texto reconocido: palabras reales reconocidas. |
| `ITN` | El formato de normalización inversa de texto ("canónica") del texto reconocido, con números de teléfono, números, abreviaturas ("doctor smith" a "dr smith") y otras transformaciones aplicadas. |
| `MaskedITN` | Formato ITN con enmascaramiento de palabras soeces aplicado, si se solicita. |
| `Display` | Formato de presentación del texto reconocido, con adición de signos de puntuación y mayúsculas. Este parámetro es el mismo que `DisplayText` que se proporcionó cuando el formato se estableció en `simple`. |

### <a name="sample-responses"></a>Respuestas de ejemplo

La siguiente es una respuesta típica de reconocimiento de `simple`.

```json
{
  "RecognitionStatus": "Success",
  "DisplayText": "Remind me to buy 5 pencils.",
  "Offset": "1236645672289",
  "Duration": "1236645672289"
}
```

La siguiente es una respuesta típica de reconocimiento de `detailed`.

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

## <a name="text-to-speech-api"></a>Text-to-Speech API

Text to Speech REST API admite voces neuronales y de texto a voz estándar, y cada una de ellas admite un idioma y un dialecto específicos, que se identifican mediante la configuración regional.

* Para ver una lista completa de voces, consulte [compatibilidad con idiomas](language-support.md#text-to-speech).
* Para obtener información acerca de la disponibilidad regional, consulte las [regiones](regions.md#text-to-speech).

### <a name="request-headers"></a>Encabezados de solicitud

Esta tabla enumera los encabezados obligatorios y opcionales para las solicitudes de voz a texto.

| Encabezado | DESCRIPCIÓN | Obligatorio u opcional |
|--------|-------------|---------------------|
| `Authorization` | Un token de autorización precedido por la palabra `Bearer`. Para más información, consulte [Autenticación](#authentication). | Obligatorio |
| `Content-Type` | Especifica el tipo de contenido para el texto proporcionado. Valor aceptable: `application/ssml+xml`. | Obligatorio |
| `X-Microsoft-OutputFormat` | Especifica el formato de salida del audio. Para obtener una lista completa de los valores aceptados, consulte [salidas de audio](#audio-outputs). | Obligatorio |
| `User-Agent` | Nombre de la aplicación. Este debe tener menos de 255 caracteres. | Obligatorio |

### <a name="audio-outputs"></a>Salidas de audio

Esta es una lista de formatos de audio admitidos que se envían en cada solicitud como encabezado `X-Microsoft-OutputFormat`. Cada uno de ellos incorpora una velocidad de bits y el tipo de codificación. El servicio Voz admite salidas de audio de 24 KHz y 16 KHz.

|||
|-|-|
| `raw-16khz-16bit-mono-pcm` | `raw-8khz-8bit-mono-mulaw` |
| `riff-8khz-8bit-mono-mulaw` | `riff-16khz-16bit-mono-pcm` |
| `audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3` |
| `audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm` |
| `riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3` |
| `audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3` |

> [!NOTE]
> Si la voz y el formato de salida seleccionados tienen velocidades de bits diferentes, se vuelve a muestrear el audio según sea necesario. Pero las voces de 24 kHz no admiten los formatos de salida `audio-16khz-16kbps-mono-siren` y `riff-16khz-16kbps-mono-siren`.

### <a name="request-body"></a>Cuerpo de la solicitud

Texto que se envía como cuerpo de una solicitud `POST` HTTP. Puede ser texto sin formato (ASCII o UTF-8) o formato SSML ([Lenguaje de marcado de síntesis de voz](speech-synthesis-markup.md)) (UTF-8). Las solicitudes de texto sin formato usan la voz y el idioma predeterminados del servicio Voz. Con SSML puede especificar el idioma y la voz.

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
