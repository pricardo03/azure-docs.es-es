---
title: 'Inicio rápido: Reconocimiento de la voz a través de un archivo de audio en C++ (macOS): servicio Voz'
titleSuffix: Azure Cognitive Services
description: Aprenda a reconocer la voz en C++ para macOS mediante el SDK de Voz.
services: cognitive-services
author: wolfma61
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 07/05/2019
ms.author: wolfma
ms.openlocfilehash: a3a651286fec9328c6d1835f68a4651212b5bd8c
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506375"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha configurado el entorno de desarrollo](../../../../quickstarts/setup-platform.md?tabs=macos)
> * [Ha creado un proyecto de ejemplo vacío](../../../../quickstarts/create-project.md?tabs=macos)

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Cree un archivo de código fuente C++ denominado `helloworld.cpp` y pegue el siguiente código en él.

   ````C++

    // Creates an instance of a speech config with specified subscription key and service region.
    // Replace with your own subscription key and service region (e.g., "westus").
    auto config = SpeechConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");

    // Creates a speech recognizer using a WAV file. The default language is "en-us".
    // Replace with your own audio file name.
    auto audioInput = AudioConfig::FromWavFileInput("whatstheweatherlike.wav");
    auto recognizer = SpeechRecognizer::FromConfig(config, audioInput);
    cout << "Recognizing first result...\n";

    // Starts speech recognition, and returns after a single utterance is recognized. The end of a
    // single utterance is determined by listening for silence at the end or until a maximum of 15
    // seconds of audio is processed.  The task returns the recognition text as result.
    // Note: Since RecognizeOnceAsync() returns only a single utterance, it is suitable only for single
    // shot recognition like command or query.
    // For long-running multi-utterance recognition, use StartContinuousRecognitionAsync() instead.
    auto result = recognizer->RecognizeOnceAsync().get();

    // Checks result.
    if (result->Reason == ResultReason::RecognizedSpeech)
    {
        cout << "RECOGNIZED: Text=" << result->Text << std::endl;
    }
    else if (result->Reason == ResultReason::NoMatch)
    {
        cout << "NOMATCH: Speech could not be recognized." << std::endl;
    }
    else if (result->Reason == ResultReason::Canceled)
    {
        auto cancellation = CancellationDetails::FromResult(result);
        cout << "CANCELED: Reason=" << (int)cancellation->Reason << std::endl;

        if (cancellation->Reason == CancellationReason::Error)
        {
            cout << "CANCELED: ErrorCode=" << (int)cancellation->ErrorCode << std::endl;
            cout << "CANCELED: ErrorDetails=" << cancellation->ErrorDetails << std::endl;
            cout << "CANCELED: Did you update the subscription info?" << std::endl;
        }
    }

   ````

1. En este nuevo archivo, reemplace la cadena `YourSubscriptionKey` por su clave de suscripción de servicios de voz.

1. Reemplace la cadena `YourServiceRegion` por la [región](~/articles/cognitive-services/Speech-Service/regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Reemplace la cadena `whatstheweatherlike.wav` por su propio nombre de archivo.


## <a name="build-the-app"></a>Compilación de la aplicación

> [!NOTE]
> Asegúrese de introducir los siguientes comandos como una _única línea de comandos_. La forma más fácil de hacerlo es copiar el comando usando el botón **Copiar** junto a cada comando, y luego pegarlo en el símbolo del shell.

* Ejecute el siguiente comando para compilar la aplicación.

  ```sh
  g++ helloworld.cpp -o helloworld --std=c++14 -F${SPEECHSDK_ROOT} -framework MicrosoftCognitiveServicesSpeech
  ```

## <a name="run-the-app"></a>Ejecución de la aplicación

1. Configuración de la ruta de acceso de la biblioteca del cargador para que apunte a la biblioteca del SDK de Voz.

    ```sh
    export DYLD_FRAMEWORK_PATH="$DYLD_FRAMEWORK_PATH:$SPEECHSDK_ROOT"
    ```

1. Ejecute la aplicación.

   ```sh
   ./helloworld
   ```

1. El archivo de audio se transmite a los servicios de voz y la primera expresión del archivo se transcribe a texto, que aparece en la misma ventana.

   ```text
   Recognizing first result...
   We recognized: What's the weather like?
   ```

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]