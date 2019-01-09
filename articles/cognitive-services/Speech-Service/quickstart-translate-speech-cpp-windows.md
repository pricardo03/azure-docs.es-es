---
title: 'Inicio rápido: Traducción de voz, C++ (Windows): servicios de Voz'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, creará una sencilla aplicación de C++ para capturar la voz del usuario, traducirla a otro idioma y mostrar el texto en la línea de comandos. Esta guía está diseñada para usuarios de Windows.
services: cognitive-services
author: wolfma61
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 12/13/2018
ms.author: erhopf
ms.openlocfilehash: 1531ad879b3345cfa089792220e7a2044570a6f8
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53729652"
---
# <a name="quickstart-translate-speech-with-the-speech-sdk-for-c"></a>Inicio rápido: Traducción de voz con el SDK de Voz para C++

En este inicio rápido, creará una sencilla aplicación de C++ que captura la voz del usuario procedente del micrófono del equipo, traduce el contenido de voz y transcribe el texto traducido en la línea de comandos en tiempo real. La aplicación está diseñada para ejecutarse en Windows de 64 bits y se ha creado con el [paquete NuGet del SDK de Voz](https://aka.ms/csspeech/nuget) y Microsoft Visual Studio 2017.

Para obtener una lista completa de los idiomas disponibles para la traducción de voz, consulte la [compatibilidad con idiomas](language-support.md).

## <a name="prerequisites"></a>Requisitos previos

Esta guía de inicio rápido requiere:

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)
* Una clave de suscripción de Azure para el servicio Voz. [Obtenga una gratis](get-started.md).

## <a name="create-a-visual-studio-project"></a>Creación de un proyecto de Visual Studio

[!INCLUDE [](../../../includes/cognitive-services-speech-service-quickstart-cpp-create-proj.md)]

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Abra el archivo de origen *helloworld.cpp*. Reemplace todo el código que aparece debajo de la instrucción include inicial (`#include "stdafx.h"` o `#include "pch.h"`) por lo siguiente:

    ```cpp
    #include "pch.h"
    #include <iostream>
    #include <vector>
    #include <speechapi_cxx.h>

    using namespace std;
    using namespace Microsoft::CognitiveServices::Speech;
    using namespace Microsoft::CognitiveServices::Speech::Translation;

    // Translation with microphone input.
    void TranslationWithMicrophone()
    {
        // Creates an instance of a speech translation config with specified subscription key and service region.
        // Replace with your own subscription key and service region (e.g., "westus").
        auto config = SpeechTranslationConfig::FromSubscription("YourSubscriptionKey", "YourServiceRegion");

        // Sets source and target languages
        // Replace with the languages of your choice.
        auto fromLanguage = "en-US";
        config->SetSpeechRecognitionLanguage(fromLanguage);
        config->AddTargetLanguage("de");
        config->AddTargetLanguage("fr");

        // Creates a translation recognizer using microphone as audio input.
        auto recognizer = TranslationRecognizer::FromConfig(config);
        cout << "Say something...\n";

        // Starts translation. RecognizeOnceAsync() returns when the first utterance has been recognized,
        // so it is suitable only for single shot recognition like command or query. For long-running
        // recognition, use StartContinuousRecognitionAsync() instead.
        auto result = recognizer->RecognizeOnceAsync().get();

        // Checks result.
        if (result->Reason == ResultReason::TranslatedSpeech)
        {
            cout << "RECOGNIZED: Text=" << result->Text << std::endl
                << "  Language=" << fromLanguage << std::endl;

            for (const auto& it : result->Translations)
            {
                cout << "TRANSLATED into '" << it.first.c_str() << "': " << it.second.c_str() << std::endl;
            }
        }
        else if (result->Reason == ResultReason::RecognizedSpeech)
        {
            cout << "RECOGNIZED: Text=" << result->Text << " (text could not be translated)" << std::endl;
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
    }

    int wmain()
    {
        TranslationWithMicrophone();
        cout << "Please press a key to continue.\n";
        cin.get();
        return 0;
    }
    ```

1. En el mismo archivo, reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](regions.md) asociada a sus suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

1. Compile la aplicación. En la barra de menús, elija **Compilar** > **Compilar solución**. El código se debería compilar sin errores ahora.

   ![Captura de pantalla de la aplicación de Visual Studio, con la opción Compilar solución resaltada](media/sdk/qs-cpp-windows-06-build.png)

1. Inicie la aplicación. En la barra de menús, elija **Depurar** > **Iniciar depuración** o bien presione **F5**.

   ![Captura de pantalla de la aplicación de Visual Studio, con la opción Iniciar depuración resaltada](media/sdk/qs-cpp-windows-07-start-debugging.png)

1. Aparece una ventana de consola que le pide decir algo. Diga una oración o frase en inglés. Lo que diga se transmitirá al servicio Voz, se traducirá y se transcribirá como texto que aparecerá en la misma ventana.

   ![Captura de pantalla de la salida de la consola después de una traducción correcta](media/sdk/qs-translate-cpp-windows-output.png)

## <a name="next-steps"></a>Pasos siguientes

Se pueden encontrar ejemplos adicionales como, por ejemplo, leer voz desde un archivo de audio y proporcionar una salida del texto traducido como voz sintetizada, en GitHub.

> [!div class="nextstepaction"]
> [Exploración de ejemplos de C++ en GitHub](https://aka.ms/csspeech/samples)

## <a name="see-also"></a>Otras referencias

- [Personalización de modelos acústicos](how-to-customize-acoustic-models.md)
- [Personalización de modelos de lenguaje](how-to-customize-language-model.md)
