---
title: Transcribir varios participantes conversaciones con el SDK de voz - servicios de voz
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar la transcripción de la conversación con el SDK de voz. Disponible para C++, C#y Java.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: jhakulin
ms.openlocfilehash: 80ec606fee30c239d47bca94188d3b9cbb7c82d5
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65604418"
---
# <a name="transcribe-multi-participant-conversations-with-the-speech-sdk"></a>Transcribir varios participantes conversaciones con el SDK de voz

El SDK de voz **ConversationTranscriber** API permite transcribir reuniones y conversaciones con la capacidad de agregar, quitar e identificar los participantes por secuencias de audio para el uso de servicios de voz `PullStream` o `PushStream`.

## <a name="limitations"></a>Limitaciones

* Se admite para la transcripción de conversación C++, C#y Java en Windows, Linux y Android.
* El DevKit ROOBO es el entorno de hardware compatible para crear las transcripciones de conversaciones, ya proporciona eficazmente el circular varios micrófonos que pueden utilizarse para la identificación del hablante. [Para obtener más información, consulte el SDK de dispositivos de voz](speech-devices-sdk.md).
* Compatibilidad con SDK de voz para la transcripción de la conversación está limitado a usar de extracción de audio e insertar secuencias de modo con ocho canales de audio PCM kHz 16 de 16 bits.
* Transcripción de la conversación está actualmente disponible en idiomas "en-US" y "zh-CN" en las siguientes regiones: centralus y este de Asia.

## <a name="prerequisites"></a>Requisitos previos

* [Aprenda a usar texto a voz con el SDK de voz.](quickstart-csharp-dotnet-windows.md)
* [Obtenga su suscripción de prueba de voz.](https://azure.microsoft.com/try/cognitive-services/)
* Speech SDK versión 1.5.1 o posterior es necesario.

## <a name="create-voice-signatures-for-participants"></a>Crear firmas de voz para los participantes

El primer paso es crear las firmas de voz para los participantes de la conversación. Creación de firmas de voz es necesaria para la identificación del hablante eficaz.

### <a name="requirements-for-input-wave-file"></a>Requisitos para el archivo de onda de entrada

* El archivo de onda de audio de entrada para la creación de firmas de voz debe aparecer en los ejemplos de 16 bits, tasa de muestreo de 16 y un formato único canal (Mono).
* La longitud recomendada para cada muestra de sonido es de 30 segundos a dos minutos.

El ejemplo siguiente muestra dos maneras diferentes de crear la firma de voz [con la API de REST]. (https://aka.ms/cts/signaturegenservice) desde C#:

```csharp
class Program
{
    static async Task CreateVoiceSignatureByUsingFormData()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var form = new MultipartFormDataContent();
        var content = new ByteArrayContent(fileBytes);
        form.Add(content, "file", "file");
        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromFormData", form);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static async Task CreateVoiceSignatureByUsingBody()
    {
        var region = "YourServiceRegion";
        byte[] fileBytes = File.ReadAllBytes(@"speakerVoice.wav");
        var content = new ByteArrayContent(fileBytes);

        var client = new HttpClient();
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "YourSubscriptionKey");
        var response = await client.PostAsync($"https://signature.{region}.cts.speech.microsoft.com/api/v1/Signature/GenerateVoiceSignatureFromByteArray", content);
        // A voice signature contains Version, Tag and Data key values from the Signature json structure from the Response body.
        // Voice signature format example: { "Version": <Numeric value>, "Tag": "string", "Data": "string" }
        var jsonData = await response.Content.ReadAsStringAsync();
    }

    static void Main(string[] args)
    {
        CreateVoiceSignatureByUsingFormData().Wait();
        CreateVoiceSignatureByUsingBody().Wait();
    }
}
```

## <a name="transcribing-conversations"></a>Transcribir conversaciones

Para transcribir conversaciones con varios participantes, cree el `ConversationTranscriber` objeto que está asociado el `AudioConfig` objeto creado para la sesión de conversación y el flujo de audio mediante `PullAudioInputStream` o `PushAudioInputStream`.

Supongamos que tiene una clase ConversationTranscriber denominada `MyConversationTranscriber`. El código puede tener este aspecto:

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;
using Microsoft.CognitiveServices.Speech.Conversation;

public class MyConversationTranscriber
{
    public static async Task ConversationWithPullAudioStreamAsync()
    {
        // Creates an instance of a speech config with specified subscription key and service region.
        // Replace with your own subscription key and region.
        var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
        var stopTranscription = new TaskCompletionSource<int>();

        // Create an audio stream from a wav file.
        // Replace with your own audio file name and Helper class which implements AudioConfig using PullAudioInputStreamCallback
        using (var audioInput = Helper.OpenWavFile(@"8channelsOfRecordedPCMAudio.wav"))
        {
            // Creates a conversation transcriber using audio stream input.
            using (var transcriber = new ConversationTranscriber(config, audioInput))
            {
                // Subscribes to events.
                transcriber.Recognizing += (s, e) =>
                {
                    Console.WriteLine($"RECOGNIZING: Text={e.Result.Text}");
                };

                transcriber.Recognized += (s, e) =>
                {
                    if (e.Result.Reason == ResultReason.RecognizedSpeech)
                    {
                        Console.WriteLine($"RECOGNIZED: Text={e.Result.Text}, UserID={e.Result.UserId}");
                    }
                    else if (e.Result.Reason == ResultReason.NoMatch)
                    {
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                    }
                };

                transcriber.Canceled += (s, e) =>
                {
                    Console.WriteLine($"CANCELED: Reason={e.Reason}");

                    if (e.Reason == CancellationReason.Error)
                    {
                        Console.WriteLine($"CANCELED: ErrorCode={e.ErrorCode}");
                        Console.WriteLine($"CANCELED: ErrorDetails={e.ErrorDetails}");
                        Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        stopTranscription.TrySetResult(0);
                    }
                };

                transcriber.SessionStarted += (s, e) =>
                {
                    Console.WriteLine("\nSession started event.");
                };

                transcriber.SessionStopped += (s, e) =>
                {
                    Console.WriteLine("\nSession stopped event.");
                    Console.WriteLine("\nStop recognition.");
                    stopTranscription.TrySetResult(0);
                };

                // Sets a conversation Id.
                transcriber.ConversationId = "AConversationFromTeams";

                // Add participants to the conversation.
                // Create voice signatures using REST API described in the earlier section in this document. 
                // Voice signature needs to be in the following format:
                // { "Version": <Numeric value>, "Tag": "string", "Data": "string" }

                var speakerA = Participant.From("Speaker_A", "en-us", signatureA);
                var speakerB = Participant.From("Speaker_B", "en-us", signatureB);
                var speakerC = Participant.From("SPeaker_C", "en-us", signatureC);
                transcriber.AddParticipant(speakerA);
                transcriber.AddParticipant(speakerB);
                transcriber.AddParticipant(speakerC);

                // Starts transcribing of the conversation. Uses StopTranscribingAsync() to stop transcribing when all participants leave.
                await transcriber.StartTranscribingAsync().ConfigureAwait(false);

                // Waits for completion.
                // Use Task.WaitAny to keep the task rooted.
                Task.WaitAny(new[] { stopTranscription.Task });

                // Stop transcribing the conversation.
                await transcriber.StopTranscribingAsync().ConfigureAwait(false);

                // Ends the conversation.
                await transcriber.EndConversationAsync().ConfigureAwait(false);
            }
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de ejemplos en GitHub](https://aka.ms/csspeech/samples)
