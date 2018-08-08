---
title: Reconocimiento de intenciones a partir de contenido de voz con Speech SDK para C#
titleSuffix: Microsoft Cognitive Services
description: >
  Se muestran diversas maneras de reconocer intenciones a partir de contenido de voz (desde un archivo o un micrófono) con Speech SDK para C#.
services: cognitive-services
author: wolfma61
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 07/16/2018
ms.author: wolfma
ms.openlocfilehash: e14120462145891090f864952dccb26ef2a77fc9
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39331356"
---
# <a name="recognize-intents-from-speech-by-using-the-speech-sdk-for-c"></a>Reconocimiento de intenciones a partir de contenido de voz con Speech SDK para C#

[!include[Selector](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-selector.md)]

[!include[Intro](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-intro.md)]

[!include[Intro - top-level declarations](../../../includes/cognitive-services-speech-service-how-to-toplevel-declarations.md)]

[!code-csharp[Top-level declarations](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#toplevel)]

[!include[Intro - using microphone](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-microphone.md)]

[!code-csharp[Intent Recognition Using Microphone](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithMicrophone)]

[!include[Intro - using microphone and language](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-microphone-language.md)]

[!code-csharp[Intent Recognition Using Microphone In A Specified Language](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentRecognitionWithLanguage)]

[!include[Intro - continuous file](../../../includes/cognitive-services-speech-service-how-to-recognize-intents-from-speech-continuous.md)]

[!code-csharp[Intent Recognition Using Events From a File](~/samples-cognitive-services-speech-sdk/samples/csharp/sharedcontent/console/intent_recognition_samples.cs#intentContinuousRecognitionWithFile)]

[!include[Download the sample](../../../includes/cognitive-services-speech-service-speech-sdk-sample-download-h2.md)]
Busque el código de este artículo en la carpeta `samples/csharp/sharedcontent/console`.

## <a name="next-steps"></a>Pasos siguientes

- [Reconocimiento de voz](how-to-recognize-speech-csharp.md)
- [Traducción de voz](how-to-translate-speech-csharp.md)
