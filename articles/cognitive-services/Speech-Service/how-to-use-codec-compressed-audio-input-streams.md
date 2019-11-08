---
title: 'Streaming de audio comprimido con códec mediante el SDK de voz: Servicio de Voz'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo hacer streaming de audio comprimido a Servicios de voz de Azure con el SDK de voz. Disponible para C++, C#y Java para Linux, Java en Android y Objective-C en iOS.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: amishu
ms.openlocfilehash: 668964c597b8d748220cbeec68e0ba68300cb406
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464348"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>Uso de entradas de audio comprimido con códec con el SDK de voz

La API **Compressed Audio Input Stream** del SDK de voz le proporciona una forma de hacer streaming de audio comprimido Speech Services mediante PullStream o PushStream.

> [!IMPORTANT]
> Solo se admite la secuencia de entrada de audio comprimido para C++, C# y Java para Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9). También se admite para [Java en Android](how-to-use-codec-compressed-audio-input-streams-android.md) y [Objective-C en la plataforma iOS](how-to-use-codec-compressed-audio-input-streams-ios.md).
> Se requiere el SDK de voz versión 1.7.0 o posterior.

Para wav/PCM consulte la documentación principal de voz.  Aparte de wav/PCM, se admiten los siguientes formatos de entrada de audio comprimido con códec:

- MP3
- OPUS/OGG
- FLAC
- ALAW en el contenedor WAV
- MULAW en el contenedor WAV

## <a name="prerequisites"></a>Requisitos previos

El control del audio comprimido se implementa mediante [GStreamer](https://gstreamer.freedesktop.org). Por motivos de licencia, los archivos binarios de Gstreamer no se compilan ni vinculan con el SDK de voz. Por lo tanto, el desarrollador de aplicaciones debe instalar lo siguiente en 18.04, 16.04 y Debian 9 para usar el audio de entrada comprimido.

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
* [See how to recognize speech in Java](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java) (Vea cómo funciona el reconocimiento de voz en Java)
