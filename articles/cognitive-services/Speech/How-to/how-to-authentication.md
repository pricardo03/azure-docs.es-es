---
title: Autenticación en Bing Speech | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Solicitud de autenticación para usar Bing Speech API
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 163ec61dc534255470626468195dcab28b3e3250
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55221834"
---
# <a name="authenticate-to-the-speech-api"></a>Autenticación en Speech API

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Bing Speech permite autenticarse con:

- Una clave de suscripción.
- Un token de autorización.

## <a name="use-a-subscription-key"></a>Uso de una clave suscripción

Para utilizar el servicio Voz, primero debe suscribirse a Speech API, que forma parte de Cognitive Services (antes Project Oxford). Puede obtener las claves de la suscripción de prueba gratuita en la página [Suscripción a Cognitive Services](https://azure.microsoft.com/try/cognitive-services/). Después de seleccionar Speech API, seleccione **Obtener clave de API** para obtener la clave. Devuelve una clave principal y una clave secundaria. Las dos claves están asociadas a la misma cuota, por lo que puede usar cualquiera de las claves.

Para un uso a largo plazo o una cuota mayor, suscríbase para obtener una [cuenta de Azure](https://azure.microsoft.com/free/).

Para usar Speech API REST, debe pasar la clave de suscripción en el campo `Ocp-Apim-Subscription-Key` del encabezado de la solicitud.

NOMBRE| Formato| DESCRIPCIÓN
----|-------|------------
Ocp-Apim-Subscription-Key | ASCII | YOUR_SUBSCRIPTION_KEY

El siguiente es un ejemplo de un encabezado de solicitud:

```HTTP
POST https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US&format=detailed HTTP/1.1
Accept: application/json;text/xml
Content-Type: audio/wav; codec=audio/pcm; samplerate=16000
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: speech.platform.bing.com
Transfer-Encoding: chunked
Expect: 100-continue
```

> [!IMPORTANT]
> Si usa [bibliotecas cliente](../GetStarted/GetStartedClientLibraries.md) en su aplicación, compruebe que puede obtener el token de autorización con su clave de suscripción, tal y como se describe en la sección siguiente. Las bibliotecas cliente utilizan la clave de suscripción para obtener un token de autorización y, a continuación, usan el token para la autenticación.

## <a name="use-an-authorization-token"></a>Uso de un token de autorización

También puede usar un token de autorización para la autenticación como prueba de autenticación o autorización. Para obtener este token, primero debe obtener una clave de suscripción de Speech API, tal y como se describe en la [sección anterior](#use-a-subscription-key).

### <a name="get-an-authorization-token"></a>Obtención de un token de autorización

Cuando tenga una clave de una suscripción válida, envíe una solicitud POST al servicio de token de Cognitive Services. En la respuesta, recibirá el token de autorización como JSON Web Token (JWT).

> [!NOTE]
> El token tiene un plazo de expiración de 10 minutos. Para renovar el token, consulte la sección siguiente.

El URI del servicio de token se encuentra aquí:

```
https://api.cognitive.microsoft.com/sts/v1.0/issueToken
```

El ejemplo de código siguiente muestra cómo obtener un token de acceso. Reemplace `YOUR_SUBSCRIPTION_KEY` por su propia clave de suscripción:

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell
$FetchTokenHeader = @{
  'Content-type'='application/x-www-form-urlencoded';
  'Content-Length'= '0';
  'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
}

$OAuthToken = Invoke-RestMethod -Method POST -Uri https://api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader

# show the token received
$OAuthToken

```

# <a name="curltabcurl"></a>[curl](#tab/curl)

El ejemplo usa curl en Linux con Bash. Si no está disponible en la plataforma, puede que deba instalar curl. El ejemplo también funciona en Cygwin en Windows, Git Bash, zsh y otros shells.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
    /*
     * This class demonstrates how to get a valid access token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = "https://api.cognitive.microsoft.com/sts/v1.0";
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
                uriBuilder.Path += "/issueToken";

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```

---

El siguiente es un ejemplo de solicitud POST:

```HTTP
POST https://api.cognitive.microsoft.com/sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
Connection: Keep-Alive
```

Si no puede obtener un token de autorización en el servicio de token, compruebe si la clave de suscripción sigue siendo válida. Si utiliza la clave de una versión de evaluación gratuita, vaya a la página de [suscripción de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/), haga clic en "Iniciar sesión" para iniciar sesión con la cuenta que utilizó para solicitar la clave de la versión de evaluación gratuita y compruebe si la clave de la suscripción ha expirado o supera la cuota.

### <a name="use-an-authorization-token-in-a-request"></a>Uso de un token de autorización en una solicitud

Cada vez que se llama a Speech API, es necesario pasar el token de autorización en el encabezado `Authorization`. El encabezado `Authorization` debe contener un token de acceso JWT.

En el ejemplo siguiente se muestra cómo usar un token de autorización cuando se llama a Speech API.

> [!NOTE]
> Reemplace `YOUR_AUDIO_FILE` por la ruta de acceso al archivo de audio grabado previamente. Reemplace `YOUR_ACCESS_TOKEN` por el token de autorización que obtuvo en el paso anterior [Obtención de un token de autorización](#get-an-authorization-token).

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```Powershell

$SpeechServiceURI =
'https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed'

# $OAuthToken is the authrization token returned by the token service.
$RecoRequestHeader = @{
  'Authorization' = 'Bearer '+ $OAuthToken;
  'Transfer-Encoding' = 'chunked'
  'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
}

# Read audio into byte array
$audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")

$RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes

# Show the result
$RecoResponse

```

# <a name="curltabcurl"></a>[curl](#tab/curl)

```
curl -v -X POST "https://speech.platform.bing.com/speech/recognition/interactive/cognitiveservices/v1?language=en-us&format=detailed" -H "Transfer-Encoding: chunked" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
```

# <a name="ctabcsharp"></a>[C#](#tab/csharp)

```cs
HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = @"speech.platform.bing.com";
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Authorization"] = "Bearer " + token;

// Send an audio file by 1024 byte chunks
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

---

### <a name="renew-an-authorization-token"></a>Renovación de un token de autorización

El token de autorización expira después de un determinado tiempo (actualmente, 10 minutos). Debe renovar el token de autorización antes de que expire.

El código siguiente es una implementación de ejemplo en C# de cómo renovar el token de autorización:

```cs
    /*
     * This class demonstrates how to get a valid O-auth token.
     */
    public class Authentication
    {
        public static readonly string FetchTokenUri = "https://api.cognitive.microsoft.com/sts/v1.0";
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
                uriBuilder.Path += "/issueToken";

                var result = await client.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
                Console.WriteLine("Token Uri: {0}", uriBuilder.Uri.AbsoluteUri);
                return await result.Content.ReadAsStringAsync();
            }
        }
    }
```
