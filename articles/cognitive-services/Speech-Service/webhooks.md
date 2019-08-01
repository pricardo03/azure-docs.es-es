---
title: 'Webhooks: Servicios de voz'
titlesuffix: Azure Cognitive Services
description: Los webhooks son devoluciones de llamada HTTP idóneos para optimizar la solución cuando se trabaja con procesos de larga duración como importaciones, adaptación, pruebas de precisión o transcripciones de archivos de larga duración.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: fbe6fe25b5ff0cd5148e3bba22dec4648399510d
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072307"
---
# <a name="webhooks-for-speech-services"></a>Webhooks en Servicios de voz

Los webhooks son como las devoluciones de llamada HTTP que permiten que la aplicación acepte datos de Servicios de voz cuando estén disponibles. Mediante los webhooks, puede optimizar el uso de la API REST eliminando la necesidad de realizar sondeos continuamente en busca de una respuesta. En las secciones siguientes, aprenderá a usar webhooks con Servicios de voz.

## <a name="supported-operations"></a>Operaciones compatibles

Servicios de voz admiten webhooks para todas las operaciones de larga duración. Cada una de las operaciones enumeradas a continuación puede desencadenar una devolución de llamada HTTP tras la finalización. 

* DataImportCompletion
* ModelAdaptationCompletion
* AccuracyTestCompletion
* TranscriptionCompletion
* EndpointDeploymentCompletion
* EndpointDataCollectionCompletion

A continuación, vamos a crear un webhook.

## <a name="create-a-webhook"></a>Creación de un webhook

Vamos a crear un webhook para una transcripción sin conexión. El escenario: un usuario tiene un archivo de audio de larga duración que le gustaría transcribir de forma asincrónica con la API Batch Transcription. 

Los webhooks se pueden crear mediante una solicitud POST a https://\<region\>.cris.ai/api/speechtotext/v2.1/transcriptions/hooks.

Los parámetros de configuración de la solicitud se proporcionan como JSON:

```json
{
  "configuration": {
    "url": "https://your.callback.url/goes/here",
    "secret": "<my_secret>"
  },
  "events": [
    "TranscriptionCompletion"
  ],
  "active": true,
  "name": "TranscriptionCompletionWebHook",
  "description": "This is a Webhook created to trigger an HTTP POST request when my audio file transcription is completed.",
  "properties": {
      "Active" : "True"
  }

}
```
Todas las solicitudes POST a la API Batch Transcription requieren un `name`. Los parámetros `description` y `properties` son opcionales.

La propiedad `Active` se usa para activar y desactivar la devolución de llamada a la dirección URL sin tener que eliminar y volver a crear el registro del webhook. Si solo necesita devolver la llamada una vez después de que el proceso se haya completado, elimine el webhook y cambie la propiedad `Active` a false.

El tipo de evento `TranscriptionCompletion` se proporciona en la matriz de eventos. Este devolverá la llamada al punto de conexión cuando una transcripción alcance un estado terminal (`Succeeded` o `Failed`). Al devolver la llamada a la dirección URL registrada, la solicitud contendrá un encabezado `X-MicrosoftSpeechServices-Event` que incluye uno de los tipos de eventos registrados. Hay una solicitud por cada tipo de evento registrado. 

Hay un tipo de evento al que no puede suscribirse. Es el tipo de evento `Ping`. Una solicitud con este tipo se envía a la dirección URL cuando se termina de crear un webhook al usar la dirección URL de ping (como se indica a continuación).  

En la configuración, se requiere la propiedad `url`. Las solicitudes POST se envían a esta dirección URL. El `secret` se utiliza para crear un hash SHA256 de la carga, con el secreto como una clave de HMAC. El hash se establece como el encabezado `X-MicrosoftSpeechServices-Signature` al devolver la llamada a la dirección URL registrada. Este encabezado está codificado en Base64.

Este ejemplo muestra cómo validar una carga mediante C#:

```csharp

private const string EventTypeHeaderName = "X-MicrosoftSpeechServices-Event";
private const string SignatureHeaderName = "X-MicrosoftSpeechServices-Signature";

[HttpPost]
public async Task<IActionResult> PostAsync([FromHeader(Name = EventTypeHeaderName)]WebHookEventType eventTypeHeader, [FromHeader(Name = SignatureHeaderName)]string signature)
{
    string body = string.Empty;
    using (var streamReader = new StreamReader(this.Request.Body))
    {
        body = await streamReader.ReadToEndAsync().ConfigureAwait(false);
        var secretBytes = Encoding.UTF8.GetBytes("my_secret");
        using (var hmacsha256 = new HMACSHA256(secretBytes))
        {
            var contentBytes = Encoding.UTF8.GetBytes(body);
            var contentHash = hmacsha256.ComputeHash(contentBytes);
            var storedHash = Convert.FromBase64String(signature);
            var validated = contentHash.SequenceEqual(storedHash);
        }
    }
 
    switch (eventTypeHeader)
    {
        case WebHookEventType.Ping:
            // Do your ping event related stuff here (or ignore this event)
            break;
        case WebHookEventType.TranscriptionCompletion:
            // Do your subscription related stuff here.
            break;
        default:
            break;
    }
 
    return this.Ok();
}

```
En este fragmento de código, `secret` se descodifica y valida. También observará que se ha cambiado el tipo de evento de webhook. Actualmente hay un evento por cada transcripción completada. El código vuelve a intentarse cinco veces para cada evento (con un retraso de un segundo) antes de desistir.

### <a name="other-webhook-operations"></a>Otras operaciones de webhook

Para obtener todos los webhooks registrados: GET https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks

Para obtener un webhook específico: GET https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

Para eliminar un webhook específico: DELETE https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

> [!Note] 
> En el ejemplo anterior, la región es "westus". Esta se debe reemplazar por la región en la que haya creado el recurso de Servicios de voz en Azure Portal.

POST https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/ping Body: empty

Envía una solicitud POST a la dirección URL registrada. La solicitud contiene un encabezado `X-MicrosoftSpeechServices-Event` con un ping de valor. Si el webhook se registró con un secreto, contendrá un encabezado `X-MicrosoftSpeechServices-Signature` con un hash de SHA256 de la carga con el secreto como clave de HMAC. El hash está codificado en Base64. 

POST https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/test Body: empty

Envía una solicitud POST a la dirección URL registrada si una entidad del tipo de evento suscrito (transcripción) está presente en el sistema y se encuentra en el estado apropiado. La carga se generará desde la última entidad que hubiera invocado el webhook. Si no hay entidades presentes, la solicitud POST responderá con 204. Si se puede realizar una solicitud de prueba, responderá con 200. El cuerpo de la solicitud tiene la misma forma que el de la solicitud GET para una entidad específica a la que el webhook se ha suscrito (por ejemplo, una transcripción). La solicitud tendrá los encabezados `X-MicrosoftSpeechServices-Event` y `X-MicrosoftSpeechServices-Signature` como se indicó anteriormente.

### <a name="run-a-test"></a>Ejecución de una prueba

Se puede realizar una prueba rápida mediante el sitio web https://bin.webhookrelay.com. Desde allí, puede obtener las direcciones URL de devolución de llamada para pasarlas como parámetro a la operación HTTP POST para crear un webhook como se indicó anteriormente en el documento.

Haga clic en "Create Bucket" (Crear cubo) y siga las instrucciones en pantalla para obtener un enlace. A continuación, utilice la información proporcionada en esta página para registrar el enlace con el servicio de Voz. La carga de un mensaje de retransmisión, en respuesta a la finalización de una transcripción, tiene el aspecto siguiente:

```json
{
    "results": [],
    "recordingsUrls": [
        "my recording URL"
    ],
    "models": [
        {
            "modelKind": "AcousticAndLanguage",
            "datasets": [],
            "id": "a09c8c8b-1090-443c-895c-3b1cf442dec4",
            "createdDateTime": "2019-03-26T12:48:46Z",
            "lastActionDateTime": "2019-03-26T14:04:47Z",
            "status": "Succeeded",
            "locale": "en-US",
            "name": "v4.13 Unified",
            "description": "Unified",
            "properties": {
                "Purpose": "OnlineTranscription,BatchTranscription,LanguageAdaptation",
                "ModelClass": "unified-v4"
            }
        }
    ],
    "statusMessage": "None.",
    "id": "d41615e1-a60e-444b-b063-129649810b3a",
    "createdDateTime": "2019-04-16T09:35:51Z",
    "lastActionDateTime": "2019-04-16T09:38:09Z",
    "status": "Succeeded",
    "locale": "en-US",
    "name": "Simple transcription",
    "description": "Simple transcription description",
    "properties": {
        "PunctuationMode": "DictatedAndAutomatic",
        "ProfanityFilterMode": "Masked",
        "AddWordLevelTimestamps": "True",
        "AddSentiment": "True",
        "Duration": "00:00:02"
    }
}
```
El mensaje contiene la dirección URL de la grabación y los modelos que se usan para transcribir esa grabación.

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
