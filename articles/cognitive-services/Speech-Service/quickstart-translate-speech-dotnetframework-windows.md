---
title: 'Inicio rápido: Traducción de voz, C# (.NET Framework Windows): servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este tutorial, creará una sencilla aplicación de .NET Framework para capturar la voz del usuario, traducirla a otro idioma y mostrar el texto en la línea de comandos. Esta guía está diseñada para usuarios de Windows.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: erhopf
ms.openlocfilehash: 575ec01037c49499dcdef3f915065a38de0bbba1
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53729636"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-net-framework"></a>Inicio rápido: Traducción de voz con el SDK de Voz para .NET Framework

En este inicio rápido, creará una sencilla aplicación de .NET Framework que captura la voz del usuario procedente del micrófono del equipo, traduce el contenido de voz y transcribe el texto traducido en la línea de comandos en tiempo real. La aplicación está diseñada para ejecutarse en Windows de 64 bits y se ha creado con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017.

Para obtener una lista completa de los idiomas disponibles para la traducción de voz, consulte la [compatibilidad con idiomas](language-support.md).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [Create project ](../../../includes/cognitive-services-speech-service-create-speech-project-vs-csharp.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra `Program.cs` y reemplace todo el código que contiene por lo siguiente.

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Microsoft.CognitiveServices.Speech;
    using Microsoft.CognitiveServices.Speech.Translation;

    namespace helloworld
    {
        class Program
        {
            public static async Task TranslationContinuousRecognitionAsync()
            {
                // Creates an instance of a speech translation config with specified subscription key and service region.
                // Replace with your own subscription key and service region (e.g., "westus").
                var config = SpeechTranslationConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

                // Sets source and target languages.
                string fromLanguage = "en-US";
                config.SpeechRecognitionLanguage = fromLanguage;
                config.AddTargetLanguage("de");

                // Sets voice name of synthesis output.
                const string GermanVoice = "de-DE-Hedda";
                config.VoiceName = GermanVoice;
                // Creates a translation recognizer using microphone as audio input.
                using (var recognizer = new TranslationRecognizer(config))
                {
                    // Subscribes to events.
                    recognizer.Recognizing += (s, e) =>
                    {
                        Console.WriteLine($"RECOGNIZING in '{fromLanguage}': Text={e.Result.Text}");
                        foreach (var element in e.Result.Translations)
                        {
                            Console.WriteLine($"    TRANSLATING into '{element.Key}': {element.Value}");
                        }
                    };

                    recognizer.Recognized += (s, e) =>
                    {
                        if (e.Result.Reason == ResultReason.TranslatedSpeech)
                        {
                            Console.WriteLine($"\nFinal result: Reason: {e.Result.Reason.ToString()}, recognized text in {fromLanguage}: {e.Result.Text}.");
                            foreach (var element in e.Result.Translations)
                            {
                                Console.WriteLine($"    TRANSLATING into '{element.Key}': {element.Value}");
                            }
                        }
                    };

                    recognizer.Synthesizing += (s, e) =>
                    {
                        var audio = e.Result.GetAudio();
                        Console.WriteLine(audio.Length != 0
                            ? $"AudioSize: {audio.Length}"
                            : $"AudioSize: {audio.Length} (end of synthesis data)");
                    };

                    recognizer.Canceled += (s, e) =>
                    {
                        Console.WriteLine($"\nRecognition canceled. Reason: {e.Reason}; ErrorDetails: {e.ErrorDetails}");
                    };

                    recognizer.SessionStarted += (s, e) =>
                    {
                        Console.WriteLine("\nSession started event.");
                    };

                    recognizer.SessionStopped += (s, e) =>
                    {
                        Console.WriteLine("\nSession stopped event.");
                    };

                    // Starts continuous recognition. Uses StopContinuousRecognitionAsync() to stop recognition.
                    Console.WriteLine("Say something...");
                    await recognizer.StartContinuousRecognitionAsync().ConfigureAwait(false);

                    do
                    {
                        Console.WriteLine("Press Enter to stop");
                    } while (Console.ReadKey().Key != ConsoleKey.Enter);

                    // Stops continuous recognition.
                    await recognizer.StopContinuousRecognitionAsync().ConfigureAwait(false);
                }
            }

            static void Main(string[] args)
            {
                TranslationContinuousRecognitionAsync().Wait();
            }
        }
    }
    ```

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Además, reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a su suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Generar solución resaltada](media/sdk/qs-csharp-dotnetcore-windows-05-build.png "Compilación correcta")

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

    ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-csharp-dotnetcore-windows-06-start-debugging.png "Iniciar la aplicación en depuración")

1. Aparece una ventana de consola que le pide decir algo. Diga una oración o frase en inglés. Lo que diga se transmitirá al servicio Voz, se traducirá y se transcribirá como texto que aparecerá en la misma ventana.

    ![Captura de pantalla de la salida de la consola después de una traducción correcta](media/sdk/qs-translate-csharp-dotnetcore-windows-output.png "Console output after successful translation")

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales como, por ejemplo, leer voz desde un archivo de audio y proporcionar una salida del texto traducido como voz sintetizada, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C# en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
