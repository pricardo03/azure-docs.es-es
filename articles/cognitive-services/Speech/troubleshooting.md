---
title: Solución de problemas de Bing Speech | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Solucione los problemas que aparecen al usar Bing Speech.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ms.openlocfilehash: 2da6795e938f1a0c0ed0495fb169007126501eea
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2018
ms.locfileid: "53753667"
---
# <a name="troubleshooting-bing-speech"></a>Solución de problemas de Bing Speech

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

## <a name="error-http-403-forbidden"></a>Error `HTTP 403 Forbidden`

Cuando se utiliza la API de reconocimiento de voz, devuelve un error `HTTP 403 Forbidden`.

### <a name="cause"></a>Causa

Este error suele deberse a problemas de autenticación. El servicio rechaza las solicitudes de conexión sin un encabezado `Ocp-Apim-Subscription-Key` o `Authorization` válido con una respuesta `HTTP 403 Forbidden`.

Si usa una clave de suscripción para la autenticación, el motivo puede ser uno de los siguientes:

- Falta la clave de suscripción o no es válida.
- Se ha excedido la cuota de utilización de la clave de suscripción.
- No se ha establecido el campo `Ocp-Apim-Subscription-Key` en el encabezado de la solicitud, cuando se llama la API de REST.

Si usa el token de autorización para la autenticación, los siguientes motivos pueden ser la causa del error.

- Falta el encabezado `Authorization` en la solicitud cuando se usa REST.
- El token de autorización especificado en el encabezado de autorización no es válido.
- El token de autorización ha expirado. El token de acceso tiene una expiración de 10 minutos.

Para obtener más información sobre la autenticación, consulte la página [Authentication](How-to/how-to-authentication.md) (Autenticación).

### <a name="troubleshooting-steps"></a>Pasos para solucionar problemas

#### <a name="verify-that-your-subscription-key-is-valid"></a>Comprobar que la clave de la suscripción sea válida

Puede ejecutar el comando siguiente para la comprobación. Tenga en cuenta que debe reemplazar *YOUR_SUBSCRIPTION_KEY* por su propia clave de suscripción. Si la clave de la suscripción es válida, recibirá en la respuesta el token de autorización como Token web JSON (JWT). De lo contrario, recibirá un error en la respuesta.

> [!NOTE]
> Reemplace `YOUR_SUBSCRIPTION_KEY` por su propia clave de suscripción.

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

El ejemplo usa curl en Linux con Bash. Si no está disponible en la plataforma, puede que deba instalar curl. El ejemplo también debe funcionar en Cygwin en Windows, Git Bash, zsh y otros shells.

```
curl -v -X POST "https://api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY"
```
---

Asegúrese de que usa la misma clave de suscripción en la aplicación o en la solicitud de REST, tal como se usa arriba.

#### <a name="verify-the-authorization-token"></a>Comprobar el token de autorización

Este paso solo es necesario si usa el token de autorización para la autenticación. Ejecute el siguiente comando para comprobar que el token de autorización sigue siendo válido. El comando realiza una solicitud POST al servicio y espera un mensaje de respuesta del servicio. Si sigue recibiendo el error `403 Forbidden` de HTTP, vuelva a comprobar que el token de acceso esté establecido correctamente y no haya expirado.

> [!IMPORTANT]
> El token tiene una expiración de 10 minutos.
> [!NOTE]
> Reemplace `YOUR_AUDIO_FILE` por la ruta de acceso del archivo de audio grabado previamente y `YOUR_ACCESS_TOKEN` por el token de autorización devuelto en el paso anterior.

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

---

## <a name="error-http-400-bad-request"></a>Error `HTTP 400 Bad Request`

El motivo suele ser que el cuerpo de la solicitud contiene datos de audio no válidos. Actualmente solo se admite el archivo WAV.

## <a name="error-http-408-request-timeout"></a>Error `HTTP 408 Request Timeout`

Lo más probable es que el error sea debido a que no se han enviado datos de audio al servicio y el servicio devuelve este error después del tiempo de expiración. Para la API de REST, los datos de audio se deben colocar en el cuerpo de la solicitud.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>El elemento `RecognitionStatus` de la respuesta es `InitialSilenceTimeout`

Por lo general, los datos de audio son el motivo del problema. Por ejemplo,

- El audio contiene un tiempo de silencio largo al principio. El servicio detendrá el reconocimiento después de uno segundos y devuelve `InitialSilenceTimeout`.
- El audio usa un formato de códec no admitido, que hace que los datos de audio se traten como silencio.
