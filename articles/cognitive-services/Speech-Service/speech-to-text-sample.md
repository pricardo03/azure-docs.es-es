---
title: Ejemplo de voz a texto | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Este es un ejemplo de voz a texto.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 2a1850e6a4f3c8eebd1b947aabe1374bdaab3fc8
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37030257"
---
# <a name="sample-for-speech-to-text"></a>Ejemplo de voz a texto

> [!NOTE]
> Para obtener instrucciones para descargar este ejemplo y otros, consulte el [SDK de ejemplos de voz](samples.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Para todos los ejemplos siguientes, deben aplicarse las siguientes declaraciones de nivel superior:
>
> [!code-csharp[](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#toplevel)]
>
> [!code-cpp[](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#toplevel)]
>
> - - -

## <a name="speech-recognition-using-the-microphone"></a>Reconocimiento de voz mediante el micrófono

El siguiente fragmento de código muestra cómo reconocer la entrada de voz a través del micrófono en el idioma predeterminado (`en-US`).

[!code-csharp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionWithMicrophone)]

[!code-cpp[Speech Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechRecognitionWithMicrophone)]

- - -

## <a name="speech-recognition-from-a-file"></a>Reconocimiento de voz desde un archivo

El fragmento de código siguiente reconoce la entrada de voz desde un archivo de audio en el idioma predeterminado (`en-US`); el formato permitido es WAV/PCM de canal único (mono) con una frecuencia de muestreo de 16 KHz.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-csharp[Speech Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs?name=recognitionFromFile)]

[!code-cpp[Speech Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp?name=SpeechRecognitionWithFile)]

- - -

## <a name="speech-recognition-using-a-customized-model"></a>Reconocimiento de voz mediante un modelo personalizado

[Custom Speech Service (CRIS)](https://www.cris.ai/) permite la personalización del motor de voz a texto de Microsoft para su aplicación. El siguiente fragmento de código muestra cómo reconocer la voz a través de un micrófono con el modelo CRIS; rellene la clave de suscripción de CRIS y su propia identificación de implementación antes de ejecutarlo.

[!code-csharp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionCustomized)]

[!code-cpp[Speech Recognition Using a Customized Model](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechRecognitionUsingCustomizedModel)]

- - -

## <a name="continuous-speech-recognition"></a>Reconocimiento de voz continua

[!code-csharp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/speech_recognition_samples.cs#recognitionContinuous)]

[!code-cpp[Continuous Speech Recognition](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/speech_recognition_samples.cpp#SpeechContinuousRecognitionUsingEvents)]

- - -

## <a name="sample-source-code"></a>Código fuente de ejemplo

La versión más reciente de los ejemplos, e incluso más ejemplos de nivel avanzado, se pueden encontrar en un [repositorio de GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk) dedicado.

## <a name="next-steps"></a>Pasos siguientes

- [Reconocimiento de la intención](./intent.md)

- [Traducción](./translation.md)
