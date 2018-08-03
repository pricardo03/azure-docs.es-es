---
title: API REST del servicio Speech
description: Referencia de las API REST del servicio Speech.
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 05/09/2018
ms.author: v-jerkin
ms.openlocfilehash: 9991e0a1fc54e6aa4a99b8bfbd93abdfe974b01b
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39283076"
---
# <a name="speech-service-rest-apis"></a>API REST del servicio Speech

Las API REST del servicio Speech unificado son similares a las API que proporciona [Speech API](https://docs.microsoft.com/azure/cognitive-services/Speech) (lo que antes se conocía como el servicio Bing Speech). Los puntos de conexión son distintos de los usados por el servicio Speech.

## <a name="speech-to-text"></a>Speech to Text

En Speech to Text API, solo los puntos de conexión usados se diferencian de la API anterior de reconocimiento de voz del servicio Speech. Los nuevos puntos de conexión se muestran en la tabla siguiente. Use el que coincida con su región de suscripción.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-speech-to-text.md)]

Aparte de esto, Speech to Text API es parecida a la [API REST](https://docs.microsoft.com/azure/cognitive-services/speech/getstarted/getstartedrest) de la Speech API anterior.

La API REST Speech to Text solo admite expresiones cortas. Las solicitudes pueden contener hasta 10 segundos de audio y pueden durar 14 segundos como máximo en general. La API REST solo devuelve resultados finales, ni parciales ni provisionales.

> [!NOTE]
> Si ha personalizado el modelo acústico o el modelo de lenguaje, o la pronunciación, use en su lugar su punto de conexión personalizado.

## <a name="text-to-speech"></a>Texto a voz

La nueva Text to Speech API admite salida de audio de 24 KHz. El encabezado `X-Microsoft-OutputFormat` puede contener ahora los siguientes valores.

|||
|-|-|
`raw-16khz-16bit-mono-pcm`         | `audio-16khz-16kbps-mono-siren`
`riff-16khz-16kbps-mono-siren`     | `riff-16khz-16bit-mono-pcm`
`audio-16khz-128kbitrate-mono-mp3` | `audio-16khz-64kbitrate-mono-mp3`
`audio-16khz-32kbitrate-mono-mp3`  | `raw-24khz-16bit-mono-pcm`
`riff-24khz-16bit-mono-pcm`        | `audio-24khz-160kbitrate-mono-mp3`
`audio-24khz-96kbitrate-mono-mp3`  | `audio-24khz-48kbitrate-mono-mp3`

El servicio Speech proporciona ahora dos voces de 24 KHz:

Configuración regional | Idioma   | Sexo | Asignación de nombre de servicio
-------|------------|--------|------------
es-ES  | English (Estados Unidos) | Mujer | "Microsoft Server Speech Text to Speech Voice (en-US, Jessa24kRUS)" 
es-ES  | English (Estados Unidos) | Hombre   | "Microsoft Server Speech Text to Speech Voice (en-US, Guy24kRUS)"

Estos son los puntos de conexión de REST para la Text to Speech API del servicio unificado Speech. Use el que coincida con su región de suscripción.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-text-to-speech.md)]

Tenga en cuenta estas diferencias al consultar la Speech API anterior en la [documentación de API REST](https://docs.microsoft.com/azure/cognitive-services/speech/api-reference-rest/bingvoiceoutput).

## <a name="authentication"></a>Autenticación

Para enviar una solicitud a la API REST del servicio Speech es necesario un token de acceso. Para obtener el token de acceso, se proporciona la clave de suscripción a un punto de conexión `issueToken` del servicio Speech regional, que se muestra en la tabla siguiente. Use el que coincida con su región de suscripción.

[!include[](../../../includes/cognitive-services-speech-service-endpoints-token-service.md)]

Cada token de acceso tiene una validez de 10 minutos. En cualquier momento puede obtener un nuevo token, incluso si quiere, justo antes de cada solicitud de API REST de Speech. No obstante, para reducir el tráfico de red y la latencia, se recomienda usar el mismo token durante nueve minutos.

En las secciones siguientes se muestra cómo obtener un token y cómo usarlo en una solicitud.

### <a name="getting-a-token-http"></a>Obtención de un token: HTTP

La siguiente es una solicitud HTTP de ejemplo para obtener un token. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Speech. Si su suscripción no está en la región Oeste de EE. UU., reemplace el encabezado `Host` por el nombre de host de su región.

```
POST /sts/v1.0/issueToken HTTP/1.1
Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY
Host: westus.api.cognitive.microsoft.com
Content-type: application/x-www-form-urlencoded
Content-Length: 0
```

El cuerpo de la respuesta a esta solicitud es el token de acceso en formato Java Web Token (JWT).

### <a name="getting-a-token-powershell"></a>Obtención de un token: PowerShell

El siguiente script de Windows PowerShell ilustra cómo obtener un token de acceso. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Speech. Si su suscripción no está en la región Oeste de EE. UU., cambie el nombre de host del URI dado en consecuencia.

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

### <a name="getting-a-token-curl"></a>Obtención de un token: cURL

cURL es una herramienta de la línea de comandos disponible en Linux (y en el subsistema Windows para Linux). El comando cURL siguiente ilustra cómo obtener un token de acceso. Reemplace `YOUR_SUBSCRIPTION_KEY` por la clave de suscripción del servicio Speech. Si su suscripción no está en la región Oeste de EE. UU., cambie el nombre de host del URI dado en consecuencia.

> [!NOTE]
> El comando se muestra en varias líneas para mejorar la legibilidad, pero se debe escribir en una única línea en un símbolo de shell.

```
curl -v -X POST 
 "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken" 
 -H "Content-type: application/x-www-form-urlencoded" 
 -H "Content-Length: 0" 
 -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```

### <a name="getting-a-token-c"></a>Obtención de un token: C#

La clase C# siguiente ilustra cómo obtener un token de acceso. Pase su clave de suscripción del servicio Speech al crear una instancia de la clase. Si su suscripción no está en la región Oeste de EE. UU., cambie el nombre de host de `FetchTokenUri` en consecuencia.

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

### <a name="using-a-token"></a>Uso de un token

Para usar un token en una solicitud de API REST, proporciónelo en el encabezado `Authorization`, después de la palabra `Bearer`. Esta, por ejemplo, es una solicitud de REST de Text to Speech que contiene un token. Sustituya el token real por `YOUR_ACCESS_TOKEN` y se el nombre de host correcto en el encabezado `Host`.

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

### <a name="renewing-authorization"></a>Renovación de la autorización

El token de autorización expira al cabo de 10 minutos. Para renovar su autorización, obtenga un nuevo token antes de que expire, por ejemplo, después de nueve minutos. 

El siguiente código de C# es un sustituto directo de la clase presentada anteriormente. La clase `Authentication` obtiene automáticamente un nuevo token de acceso cada nueve minutos mediante un temporizador. Este enfoque garantiza que siempre esté disponible un token válido mientras se ejecuta el programa.

> [!NOTE]
> En lugar de usar un temporizador, podría almacenar una marca de tiempo de cuando el token actual se obtuvo, y luego solicitar uno nuevo solo si el token actual está a punto de expirar. Este enfoque evita tener que solicitar nuevos tokens innecesariamente y puede ser más adecuado en el caso de programas que realizan solicitudes de Speech poco frecuentes.

Al igual que antes, asegúrese de que el valor `FetchTokenUri` coincida con su región de suscripción. Pase su clave de suscripción al crear una instancia de la clase.

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

