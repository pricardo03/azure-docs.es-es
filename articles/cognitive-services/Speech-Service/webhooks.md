---
title: Webhooks - servicios de voz
titlesuffix: Azure Cognitive Services
description: Los Webhooks son ideales para optimizar la solución cuando se trabaja con prolongada de devolución de llamada HTTP procesos como importaciones, adaptación, pruebas de precisión o transcripciones de los archivos de larga ejecución.
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: panosper
ms.custom: seodec18
ms.openlocfilehash: 7b47d4fc3aa4a1a50e441e668a856703c67045ae
ms.sourcegitcommit: 48a41b4b0bb89a8579fc35aa805cea22e2b9922c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/15/2019
ms.locfileid: "59581018"
---
# <a name="webhooks-for-speech-services"></a>Webhooks para servicios de voz

Los Webhooks son como las devoluciones de llamada HTTP que permite que la aplicación aceptar datos de los servicios de voz cuando esté disponible. Uso de webhooks, puede optimizar el uso de nuestras API de REST al eliminar la necesidad de sondear continuamente para una respuesta. En las secciones siguientes, obtendrá información sobre cómo usar webhooks con los servicios de voz.

## <a name="supported-operations"></a>Operaciones compatibles

Los servicios de voz admitir webhooks para todas las operaciones de larga ejecución. Cada una de las operaciones enumeradas a continuación puede desencadenar una devolución de llamada HTTP tras la finalización. 

* DataImportCompletion
* ModelAdaptationCompletion
* AccuracyTestCompletion
* TranscriptionCompletion
* EndpointDeploymentCompletion
* EndpointDataCollectionCompletion

A continuación, vamos a crear un webhook.

## <a name="create-a-webhook"></a>Creación de un webhook

Vamos a crear un webhook para una transcripción sin conexión. El escenario: un usuario tiene un archivo de audio de larga ejecución que les gustaría transcribir de forma asincrónica con la API de transcripción de Batch. 

Se proporcionan los parámetros de configuración para la solicitud como JSON:

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
Todas las solicitudes POST a la API de transcripción de Batch requieren un `name`. El `description` y `properties` parámetros son opcionales.

El `Active` propiedad se usa para conmutar devolviendo la llamada a la dirección URL y desactivar sin tener que eliminar y volver a crear el registro de webhook. Si solo necesita devolver la llamada una vez después de que tiene el proceso completo, a continuación, eliminar el webhook y cambiar el `Active` propiedad en false.

El tipo de evento `TranscriptionCompletion` se proporciona en la matriz de eventos. Volverá a llamar al punto de conexión cuando una transcripción entra en un estado terminal (`Succeeded` o `Failed`). Al devolver la llamada a la dirección URL registrada, la solicitud contendrá una `X-MicrosoftSpeechServices-Event` encabezado que contiene uno de los tipos de eventos registrados. Hay una solicitud por tipo de evento registrado. 

Hay un tipo de evento que no puede suscribirse a. Es el `Ping` tipo de evento. Se envía una solicitud con este tipo a la dirección URL cuando termine de crear un webhook cuando se usa la dirección URL de ping (ver abajo).  

En la configuración, el `url` propiedad es obligatoria. Se envían las solicitudes POST a esta dirección URL. El `secret` se utiliza para crear un hash SHA256 de la carga, con el secreto como una clave HMAC. El hash se establece como el `X-MicrosoftSpeechServices-Signature` encabezado al devolver la llamada a la dirección URL registrada. Este encabezado está codificado en Base64.

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
En este fragmento de código, el `secret` se puede descodificar y validado. También podrá observar que se ha cambiado el tipo de evento de webhook. Actualmente hay un evento por cada transcripción completada. El código vuelve a intentar cinco veces para cada evento (con un una retraso segunda) antes de desistir.

### <a name="other-webhook-operations"></a>Otras operaciones de webhook

Para obtener todas las instancias registrados webhooks: GET https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks

Para obtener un webhook específico: GET https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

Para quitar un webhook específico: DELETE https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id

> [!Note] 
> En el ejemplo anterior, la región es "westus". Esto se debe reemplazar por la región donde ha creado el recurso de servicios de voz en el portal de Azure.

POST https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/ping cuerpo: vacío

Envía una solicitud POST a la dirección URL registrada. La solicitud contiene un `X-MicrosoftSpeechServices-Event` encabezado con un ping de valor. Si el webhook se registró con un secreto, contendrá un `X-MicrosoftSpeechServices-Signature` encabezado con un hash SHA256 de la carga con el secreto como clave HMAC. El hash está codificado en Base64. 

POST https://westus.cris.ai/api/speechtotext/v2.1/transcriptions/hooks/:id/test cuerpo: vacío

Envía una solicitud POST a la dirección URL registrada si una entidad para el tipo de evento suscrito (transcripción) está presente en el sistema y se encuentra en el estado apropiado. La carga se generará desde la última entidad que hubiera invocado el webhook. Si no hay entidades está presente, la entrada de blog responderá con 204. Si se puede realizar una solicitud de prueba, responderá con 200. Es el cuerpo de solicitud de la misma forma que en la solicitud GET de una entidad específica (por ejemplo, transcripción) se ha suscrito el webhook. La solicitud tendrá el `X-MicrosoftSpeechServices-Event` y `X-MicrosoftSpeechServices-Signature` encabezados según lo descrito anteriormente.

### <a name="run-a-test"></a>Ejecutar una prueba

Se puede realizar una prueba rápida mediante el sitio Web https://bin.webhookrelay.com. Desde allí, puede obtener llamada hacer una copia de las direcciones URL para pasar como parámetro a la operación HTTP POST para crear un webhook que se describió anteriormente en el documento.

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
