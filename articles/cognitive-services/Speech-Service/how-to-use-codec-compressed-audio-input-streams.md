---
title: 'Streaming de audio comprimido con códec mediante el SDK de voz: Servicio de Voz'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo hacer streaming de audio comprimido a Servicios de voz de Azure con el SDK de voz. Disponible para C++, C# y Java para Linux.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: amishu
ms.openlocfilehash: b29b42dea9522526d49c1bda017a522855946def
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559550"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>Uso de entradas de audio comprimido con códec con el SDK de voz

La API **Compressed Audio Input Stream** del SDK de voz le proporciona una forma de hacer streaming de audio comprimido Speech Services mediante PullStream o PushStream.

> [!IMPORTANT]
> Solo se admite el streaming de audio comprimido para C++, C# y Java para Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9).
> Se requiere el SDK de voz versión 1.4.0 o posterior.

Para wav/PCM consulte la documentación principal de voz.  Aparte de wav/PCM, se admiten los siguientes formatos de entrada de audio comprimido con códec:

- MP3
- OPUS/OGG

## <a name="prerequisites-to-using-codec-compressed-audio-input"></a>Requisitos previos para el uso de entradas de audio comprimido con códec

Instale estas dependencias adicionales para usar la entrada de audio comprimido con el SDK de voz para Linux:

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="example-code-using-codec-compressed-audio-input"></a>Ejemplo de código que usa una entrada de audio comprimido con códec

Para hacer streaming de un formato de audio comprimido a Servicios de voz, cree `PullAudioInputStream` o `PushAudioInputStream`. A continuación, cree un objeto `AudioConfig` a partir de una instancia de la clase de secuencia, especificando el formato de compresión de la secuencia.

Supongamos que tiene una clase de flujo de entrada llamada `myPushStream` y que usa OPUS/OGG. El aspecto del código sería el siguiente:

```csharp
using Microsoft.CognitiveServices.Speech;
using Microsoft.CognitiveServices.Speech.Audio;

var speechConfig = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");

// Create an audio config specifying the compressed audio format and the instance of your input stream class.
var audioFormat = AudioStreamFormat.GetCompressedFormat(AudioStreamContainerFormat.OGG_OPUS);
var audioConfig = AudioConfig.FromStreamInput(myPushStream, audioFormat);

var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

var result = await recognizer.RecognizeOnceAsync();

var text = result.GetText();
```

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
- [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
