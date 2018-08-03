---
title: Ejemplo para el Reconocimiento de la intención | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Aquí se presenta un ejemplo para el reconocimiento de la intención.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: 3e9afc990d6bfa73eb045e7ed76dfd194df309c6
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213213"
---
# <a name="sample-for-intent-recognition"></a>Ejemplo para el reconocimiento de la intención

Primero es necesario que obtenga una clave de suscripción. A diferencia de otros servicios compatibles con el SDK de Cognitive Services Speech, los servicios de reconocimiento de la intención requieren una clave de suscripción específica. [Aquí](https://www.luis.ai) puede encontrar información adicional acerca de la tecnología de reconocimiento de la intención, así como información acerca de cómo adquirir una clave de suscripción. Reemplace su propia clave de suscripción de Language Understanding, la [región de su suscripción](regions.md) y el AppId del modelo de la intención en los lugares correspondientes de los ejemplos.

## <a name="top-level-declarations"></a>Declaraciones de nivel superior

Para todos los ejemplos siguientes deben aplicarse las siguientes declaraciones de nivel superior:

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

[!code-cpp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#toplevel)]

[!code-java[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#toplevel)]

## <a name="intent-recognition-using-microphone"></a>Reconocimiento de la intención con micrófono

El siguiente fragmento de código muestra cómo reconocer la intención en una entrada de micrófono en el idioma predeterminado (`en-US`).

[!code-csharp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

[!code-cpp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentRecognitionWithMicrophone)]

[!code-java[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithMicrophone)]

## <a name="intent-recognition-using-microphone-in-a-specified-language"></a>Reconocimiento de la intención con micrófono en un idioma especificado

El siguiente fragmento de código muestra cómo reconocer la intención en una entrada de micrófono en un idioma concreto, en este caso en alemán (`de-de`).

[!code-csharp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithLanguage)]

[!code-cpp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentRecognitionWithLanguage)]

[!code-java[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentRecognitionWithLanguage)]

## <a name="intent-recognition-from-a-file-using-events"></a>Reconocimiento de intención desde un archivo mediante eventos

El fragmento de código muestra cómo reconocer la intención en el idioma predeterminado (`en-US`) de manera continua. Este código permite el acceso a información adicional, como los resultados intermedios. La entrada se toma de un archivo de audio, el formato compatible es WAV/PCM de un solo canal (mono) con una frecuencia de muestreo de 16 KHz.

[!code-csharp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

[!code-cpp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/cpp/windows/console/samples/intent_recognition_samples.cpp#IntentContinuousRecognitionWithFile)]

[!code-java[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/java/jre/console/src/com/microsoft/cognitiveservices/speech/samples/console/IntentRecognitionSamples.java#IntentContinuousRecognitionWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]

## <a name="next-steps"></a>Pasos siguientes

- [Reconocimiento de voz](./speech-to-text-sample.md)

- [Traducción](./translation.md)
