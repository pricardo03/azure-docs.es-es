---
title: 'Transcripción de conversaciones de varios participantes con el SDK de voz: servicio de Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a usar la transcripción de conversaciones con el SDK de voz. Disponible para C++, C# y Java.
services: cognitive-services
author: jhakulin
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: jhakulin
ms.openlocfilehash: 5ad912b1cee5495e18a5eb2da4d981eadd74dd7d
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70066430"
---
# <a name="transcribe-multi-participant-conversations-with-the-speech-sdk"></a>Transcripción de conversaciones de varios participantes con el SDK de voz

La API **ConversationTranscriber** del SDK de voz le permite transcribir reuniones y conversaciones con la posibilidad de agregar, quitar e identificar participantes haciendo streaming de audio a Servicios de voz mediante `PullStream` o `PushStream`.

## <a name="limitations"></a>Limitaciones

* Se admite el transcriptor de conversaciones para C++, C# y Java en Windows, Linux y Android.
* ROOBO DevKit es el entorno de hardware que se admite para crear transcripciones de conversaciones, ya que proporciona una matriz circular de varios micrófonos que se puede utilizar eficazmente para la identificación del hablante. [Para más información, consulte Speech Devices SDK](speech-devices-sdk.md).
* La compatibilidad del SDK de voz con la transcripción de conversaciones se limita a secuencias en modo de extracción e inserción de audio con ocho canales de audio de PCM de 16 bits y 16 kHz. Actualmente, solo se admiten estos kits para la captura de audio de ocho canales:
   * [ROOBO Smart audio circular 7-MIC DK](https://ddk.roobo.com/)
   * [Azure Kinect DK](https://azure.microsoft.com/en-in/services/kinect-dk/).
* Transcripción de conversaciones solo está disponible actualmente para los idiomas "en-US" y "zh-CN" en las siguientes regiones: centralus y eastasia.

## <a name="prerequisites"></a>Requisitos previos

* [Aprenda a usar la conversión de voz a texto con el SDK de voz.](quickstart-csharp-dotnet-windows.md)
* [Obtenga su suscripción de prueba a Voz.](https://azure.microsoft.com/try/cognitive-services/)
* Se requiere el SDK de voz versión 1.5.1 o posterior.

## <a name="create-voice-signatures-for-participants"></a>Creación de firmas de voz para los participantes

El primer paso es crear firmas de voz para los participantes de la conversación. La creación de firmas de voz es necesaria para una identificación eficaz del hablante.

### <a name="requirements-for-input-wave-file"></a>Requisitos del archivo WAVE de entrada

* El archivo WAVE de entrada para la creación de firmas de voz debe aparecer en las muestras de 16 bits, con frecuencia de muestreo de 16 kHz y un formato de canal único (Mono).
* La longitud recomendada para cada muestra de audio está entre 30 segundos y dos minutos.

El ejemplo siguiente muestra dos maneras diferentes de crear la firma de voz [mediante la API REST](https://aka.ms/cts/signaturegenservice) de C#:

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

## <a name="transcribing-conversations"></a>Transcripción de conversaciones

Para transcribir conversaciones con varios participantes, cree el objeto `ConversationTranscriber` que está asociado al objeto `AudioConfig` creado para la sesión de conversación y haga streaming del audio mediante `PullAudioInputStream` o `PushAudioInputStream`.

Supongamos que tiene una clase ConversationTranscriber denominada `MyConversationTranscriber`. El aspecto del código sería el siguiente:

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
