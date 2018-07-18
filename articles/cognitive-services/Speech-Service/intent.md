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
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 38f7f038a803546adb83245519efc5de0c0d1599
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37045017"
---
# <a name="sample-for-intent-recognition"></a>Ejemplo para el reconocimiento de la intención

> [!NOTE]
> Para obtener instrucciones para descargar este ejemplo y otros., consulte los [Ejemplos de SDK de voz](samples.md).

> [!NOTE]
> Primero es necesario que obtenga una clave de suscripción. A diferencia de otros servicios compatibles con el SDK de Cognitive Services Speech, los servicios de reconocimiento de la intención requieren una clave de suscripción específica. [Aquí](https://www.luis.ai) puede encontrar información adicional acerca de la tecnología de reconocimiento de la intención, así como información acerca de cómo adquirir una clave de suscripción. Coloque su propia clave de suscripción, la región del servicio y el AppId de su modelo de intención en el lugar adecuado en los ejemplos.

> [!NOTE]
> Para todos los ejemplos siguientes deben aplicarse las siguientes declaraciones de nivel superior:
>
> [!code-cpp[](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#toplevel)]
>
> - - -

## <a name="intent-recognition-using-microphone"></a>Reconocimiento de la intención con micrófono

El siguiente fragmento de código muestra cómo reconocer la intención en una entrada de micrófono en el idioma predeterminado (`en-US`).

[!code-cpp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithMicrophone)]

- - -

## <a name="intent-recognition-using-microphone-in-a-specified-language"></a>Reconocimiento de la intención con micrófono en un idioma especificado

El siguiente fragmento de código muestra cómo reconocer la intención en una entrada de micrófono en un idioma concreto, en este caso en alemán (`de-de`).

[!code-cpp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithLanguage)]

- - -

## <a name="intent-recognition-from-a-file"></a>Reconocimiento de la intención en un archivo

El fragmento de código siguiente reconoce la intención en un archivo de audio en el idioma predeterminado (`en-US`), el formato permitido es WAV/PCM de canal único (mono) con una frecuencia de muestreo de 16 KHz.

[!include[Sample Audio](includes/sample-audio.md)]

[!code-cpp[Intent Recognition From a File](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentRecognitionWithFile)]

- - -

## <a name="intent-recognition-using-events"></a>Reconocimiento de la intención con eventos

El fragmento de código muestra cómo reconocer la intención de forma continua. Este código permite el acceso a información adicional, como los resultados intermedios. 

[!code-cpp[Intent Recognition Using Events](~/samples-cognitive-services-speech-sdk/Windows/cxx_samples/intent_recognition_samples.cpp#IntentContinuousRecognitionUsingEvents)]

- - -

## <a name="sample-source-code"></a>Código fuente de ejemplo

Para obtener el conjunto más reciente de ejemplos, consulte el [repositorio de GitHub de ejemplos del SDK de Cognitive Services Speech](https://aka.ms/csspeech/samples).

## <a name="next-steps"></a>Pasos siguientes

- [Reconocimiento de voz](./speech-to-text-sample.md)

- [Traducción](./translation.md)
