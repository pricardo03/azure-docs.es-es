---
title: 'Inicio rápido: Reconocimiento de la voz a través de un archivo de audio en C# (.NET): servicio Voz'
titleSuffix: Azure Cognitive Services
description: TBD
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/28/2019
ms.author: erhopf
ms.openlocfilehash: fdb747212914769b8551d9cd12f1fbc8a01245dc
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506351"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha configurado el entorno de desarrollo](../../../../quickstarts/setup-platform.md?tabs=dotnet)
> * [Ha creado un proyecto de ejemplo vacío](../../../../quickstarts/create-project.md?tabs=dotnet)

## <a name="open-your-project-in-visual-studio"></a>Abra el proyecto en Visual Studio.

El primer paso es asegurarse de que tiene el proyecto abierto en Visual Studio.

1. Inicie Visual Studio 2019.
2. Cargue el proyecto y abra `Program.cs`.

## <a name="start-with-some-boilerplate-code"></a>Inicio con código reutilizable

Vamos a agregar código que funcione como el esqueleto del proyecto. Tenga en cuenta que ha creado un método asincrónico llamado `RecognizeSpeechAsync()`.

````C#

using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace helloworld
{
    class Program
    {
        public static async Task RecognizeSpeechAsync()
        {
        }

        static void Main()
        {
            RecognizeSpeechAsync().Wait();
        }
    }
}

````

## <a name="create-a-speech-configuration"></a>Creación de una configuración de Voz

Antes de inicializar un objeto `SpeechRecognizer`, debe crear una configuración que use la clave y la región de suscripción. Inserte este código en el método `RecognizeSpeechAsync()`.

> [!NOTE]
> En este ejemplo se usa el método `FromSubscription()` para compilar la clase `SpeechConfig`. Para ver una lista completa de los métodos disponibles, consulte [Clase SpeechConfig](https://docs.microsoft.com/dotnet/api/microsoft.cognitiveservices.speech.speechconfig?view=azure-dotnet).

````C#
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
````

## <a name="create-an-audio-configuration"></a>Creación de una configuración de audio

Ahora, debe crear un objeto ````AudioConfig```` que apunte al archivo de audio. Este objeto se crea dentro de una instrucción using para garantizar la versión correcta de los recursos no administrados. Inserte este código en el método `RecognizeSpeechAsync()`, justo debajo de la configuración de Voz.

````C#
using (var audioInput = AudioConfig.FromWavFileInput(@"whatstheweatherlike.wav"))
{
}
````

## <a name="initialize-a-speechrecognizer"></a>Inicialización de un objeto SpeechRecognizer

Ahora, se creará el objeto `SpeechRecognizer` con los objetos `SpeechConfig` y `AudioConfig` creados anteriormente. Este objeto se crea también dentro de una instrucción using para garantizar la versión correcta de los recursos no administrados. Inserte este código en el método `RecognizeSpeechAsync()`, dentro de la instrucción using que encapsula el objeto ````AudioConfig````.

````C#
using (var recognizer = new SpeechRecognizer(config, audioInput))
{
}
````

## <a name="recognize-a-phrase"></a>Reconocimiento de una frase

En el objeto `SpeechRecognizer`, va a llamar al método `RecognizeOnceAsync()`. Este método permite que el servicio Voz sepa que solo va a enviar una frase para el reconocimiento y que, una vez que se identifica la frase, se detendrá el reconocimiento de voz.

Dentro de la instrucción using, agregue este código:
````C#
Console.WriteLine("Recognizing first result...");
var result = await recognizer.RecognizeOnceAsync();
````

## <a name="display-the-recognition-results-or-errors"></a>Visualización de los resultados (o errores) del reconocimiento

Cuando el servicio Voz devuelva el resultado del reconocimiento, querrá hacer algo con él. Vamos a hacer algo tan sencillo como imprimir el resultado en la consola.

Dentro de la instrucción using, debajo de `RecognizeOnceAsync()`, agregue este código:
````C#
if (result.Reason == ResultReason.RecognizedSpeech)
{
    Console.WriteLine($"We recognized: {result.Text}");
}
else if (result.Reason == ResultReason.NoMatch)
{
    Console.WriteLine($"NOMATCH: Speech could not be recognized.");
}
else if (result.Reason == ResultReason.Canceled)
{
    var cancellation = CancellationDetails.FromResult(result);
    Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

    if (cancellation.Reason == CancellationReason.Error)
    {
        Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
        Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
        Console.WriteLine($"CANCELED: Did you update the subscription info?");
    }
}
````

## <a name="check-your-code"></a>Comprobación del código

En este momento, el código debe tener esta apariencia:

````C#
//
// Copyright (c) Microsoft. All rights reserved.
// Licensed under the MIT license. See LICENSE.md file in the project root for full license information.
//

using System;
using System.Threading.Tasks;
using Microsoft.CognitiveServices.Speech;

namespace helloworld
{
    class Program
    {
        public static async Task RecognizeSpeechAsync()
        {
            var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

            using (var audioInput = AudioConfig.FromWavFileInput(@"whatstheweatherlike.wav"))
            {
                using (var recognizer = new SpeechRecognizer(config, audioInput))
                {
                    Console.WriteLine("Recognizing first result...");
                    var result = await recognizer.RecognizeOnceAsync();

                    if (result.Reason == ResultReason.RecognizedSpeech)
                    {
                        Console.WriteLine($"We recognized: {result.Text}");
                    }
                    else if (result.Reason == ResultReason.NoMatch)
                    {
                        Console.WriteLine($"NOMATCH: Speech could not be recognized.");
                    }
                    else if (result.Reason == ResultReason.Canceled)
                    {
                        var cancellation = CancellationDetails.FromResult(result);
                        Console.WriteLine($"CANCELED: Reason={cancellation.Reason}");

                        if (cancellation.Reason == CancellationReason.Error)
                        {
                            Console.WriteLine($"CANCELED: ErrorCode={cancellation.ErrorCode}");
                            Console.WriteLine($"CANCELED: ErrorDetails={cancellation.ErrorDetails}");
                            Console.WriteLine($"CANCELED: Did you update the subscription info?");
                        }
                    }
                }
            }
        }

        static void Main()
        {
            RecognizeSpeechAsync().Wait();
        }
    }
}
````

## <a name="build-and-run-your-app"></a>Compilación y ejecución de la aplicación

Ya está listo para compilar la aplicación y probar el reconocimiento de voz con el servicio Voz.

1. **Compile el código**: en la barra de menús de Visual Studio, elija **Compilar** > **Compilar solución**.
2. **Inicie la aplicación**: en la barra de menús, elija **Depurar** > **Iniciar depuración** o presione **F5**.
3. **Inicie el reconocimiento**: el archivo de audio se envía al servicio Voz, se transcribe como texto y se representa en la consola.

   ```text
   Recognizing first result...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]
