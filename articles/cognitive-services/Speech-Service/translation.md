---
title: Ejemplo para traducción | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Este es un ejemplo de traducción de voz.
services: cognitive-services
author: wolfma61
manager: onano
ms.service: cognitive-services
ms.technology: Speech
ms.topic: article
ms.date: 06/07/2018
ms.author: wolfma
ms.openlocfilehash: 1151628ed695e74e2196c20b08e33fa5eaf33282
ms.sourcegitcommit: 150a40d8ba2beaf9e22b6feff414f8298a8ef868
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37028394"
---
# <a name="sample-for-translation"></a>Ejemplo de traducción

> [!NOTE]
> Para obtener instrucciones para descargar este ejemplo y otros, consulte el [SDK de ejemplos de voz](samples.md).

[!include[Get a Subscription Key](includes/get-subscription-key.md)]

> [!NOTE]
> Para todos los ejemplos siguientes deben aplicarse las siguientes declaraciones de nivel superior:
>
> [!code-csharp [Using Statements](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#toplevel)]
>
> - - -

## <a name="translation-using-the-microphone"></a>Traducción mediante el micrófono

El siguiente fragmento de código muestra cómo traducir la entrada de voz de inglés a alemán, y también obtener la salida de voz del texto traducido. Utiliza el micrófono.

[!code-csharp[Translation Using Microphone](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#TranslationWithMicrophoneAsync)]

- - -

## <a name="translation-using-file-input"></a>Traducción con entrada de archivos

El siguiente fragmento de código muestra cómo traducir la entrada de voz de inglés a alemán y francés.
Utiliza el archivo como entrada.

[!code-csharp[Translation Using File Input](~/samples-cognitive-services-speech-sdk/Windows/csharp_samples/translation_samples.cs#TranslationWithFileAsync)]

- - -

## <a name="sample-source-code"></a>Código fuente de ejemplo

La versión más reciente de los ejemplos, e incluso más ejemplos de nivel avanzado, se pueden encontrar en un [repositorio de GitHub](https://github.com/Azure-Samples/cognitive-services-speech-sdk) dedicado.

## <a name="next-steps"></a>Pasos siguientes

- [Reconocimiento de voz](./speech-to-text-sample.md)

- [Reconocimiento de la intención](./intent.md)
