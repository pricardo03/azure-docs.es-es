---
title: Solución de problemas de Speech SDK de Cognitive Services
description: Solución de problemas del SDK de Voz de Cognitive Services
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: wolfma
ms.openlocfilehash: ff8aba562cfd2d6d54c708ee7fdc4c6ca7185f29
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39284129"
---
# <a name="troubleshooting-speech-services-sdk"></a>Solución de problemas del SDK de servicios de Voz

Este artículo proporciona información para ayudarle a resolver los problemas que pueden surgir al usar el SDK de Voz.

## <a name="error-websocket-upgrade-failed-with-an-authentication-error-403"></a>Error `WebSocket Upgrade failed with an authentication error (403).`

Puede que el punto de conexión sea incorrecto para su región o servicio. Compruebe el URI para asegurarse de que sea correcto. Consulte también la siguiente sección, ya que también podría haber un problema con la clave de la suscripción o el token de autorización.

## <a name="error-http-403-forbidden-or-error-http-401-unauthorized"></a>Error `HTTP 403 Forbidden` o error `HTTP 401 Unauthorized`

Este error suele deberse a problemas de autenticación. Las solicitudes de conexión sin un encabezado `Ocp-Apim-Subscription-Key` o `Authorization` válido se rechazan con el estado 401 o 403.

* Si usa una clave de suscripción para la autenticación, la causa puede ser una de las siguientes:

    - Falta la clave de suscripción o no es válida.
    - Ha superado la cuota de uso de la suscripción.

* Si usa un token de autorización para la autenticación, la causa puede ser una de las siguientes:

    - El token de autorización no es válido.
    - El token de autorización ha expirado.

### <a name="validate-your-subscription-key"></a>Validar la clave de suscripción

Puede comprobar si tiene una clave de suscripción válida ejecutando uno de los comandos siguientes.

> [!NOTE]
> Reemplace `YOUR_SUBSCRIPTION_KEY` y `YOUR_REGION` por su propia clave de la suscripción y región asociada, respectivamente.

* PowerShell

    ```Powershell
    $FetchTokenHeader = @{
      'Content-type'='application/x-www-form-urlencoded'
      'Content-Length'= '0'
      'Ocp-Apim-Subscription-Key' = 'YOUR_SUBSCRIPTION_KEY'
    }
    $OAuthToken = Invoke-RestMethod -Method POST -Uri https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken -Headers $FetchTokenHeader
    $OAuthToken
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.api.cognitive.microsoft.com/sts/v1.0/issueToken" -H "Ocp-Apim-Subscription-Key: YOUR_SUBSCRIPTION_KEY" -H "Content-type: application/x-www-form-urlencoded" -H "Content-Length: 0"
    ```

### <a name="validate-an-authorization-token"></a>Validar un token de autorización

Si usa un token de autorización para la autenticación, ejecute uno de los siguientes comandos para comprobar que el token de autorización sigue siendo válido. Los tokens son válidos durante 10 minutos.

> [!NOTE]
> Reemplace `YOUR_AUDIO_FILE` por la ruta de acceso del archivo de audio grabado previamente, `YOUR_ACCESS_TOKEN` por el token de autorización devuelto en el paso anterior y `YOUR_REGION` por la región correcta.

* PowerShell

    ```Powershell
    $SpeechServiceURI =
    'https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US'
    
    # $OAuthToken is the authorization token returned by the token service.
    $RecoRequestHeader = @{
      'Authorization' = 'Bearer '+ $OAuthToken
      'Transfer-Encoding' = 'chunked'
      'Content-type' = 'audio/wav; codec=audio/pcm; samplerate=16000'
    }
    
    # Read audio into byte array
    $audioBytes = [System.IO.File]::ReadAllBytes("YOUR_AUDIO_FILE")
    
    $RecoResponse = Invoke-RestMethod -Method POST -Uri $SpeechServiceURI -Headers $RecoRequestHeader -Body $audioBytes
    
    # Show the result
    $RecoResponse
    ```

* cURL

    ```
    curl -v -X POST "https://YOUR_REGION.stt.speech.microsoft.com/speech/recognition/interactive/cognitiveservices/v1?language=en-US" -H "Authorization: Bearer YOUR_ACCESS_TOKEN" -H "Transfer-Encoding: chunked" -H "Content-type: audio/wav; codec=audio/pcm; samplerate=16000" --data-binary @YOUR_AUDIO_FILE
    ```

---

## <a name="error-http-400-bad-request"></a>Error `HTTP 400 Bad Request`

El motivo suele darse cuando el cuerpo de la solicitud contiene datos de audio no válidos. Solo se admite el formato `WAV`. Compruebe también los encabezados de la solicitud para asegurarse de que está especificando parámetros `Content-Type` y `Content-Length` correctos.

## <a name="error-http-408-request-timeout"></a>Error `HTTP 408 Request Timeout`

El error es más probable porque no se envía ningún dato de audio al servicio. Este error también podría deberse a problemas de red.

## <a name="the-recognitionstatus-in-the-response-is-initialsilencetimeout"></a>El elemento `RecognitionStatus` de la respuesta es `InitialSilenceTimeout`

Por lo general, los datos de audio son el motivo del problema. Por ejemplo: 

* Se produce un período de silencio prolongado al principio del audio. El servicio detendrá el reconocimiento después de unos segundos y devolverá `InitialSilenceTimeout`.
* El audio usa un formato de códec no admitido, que hace que los datos de audio se traten como silencio.

## <a name="next-steps"></a>Pasos siguientes

* [Notas de la versión](releasenotes.md)

