---
title: Códec Stream comprime el audio con el SDK de voz - servicios de voz
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo hacer streaming de audio comprimido a los servicios de voz de Azure con el SDK de voz. Disponible para C++, C#y Java para Linux.
services: cognitive-services
author: amitkumarshukla
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: amishu
ms.openlocfilehash: 41a55eca321cbe1bfa23a889b8e3ce7c701ce769
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65468054"
---
# <a name="using-codec-compressed-audio-input-with-the-speech-sdk"></a>Mediante el códec comprime la entrada de audio con el SDK de voz

El SDK de voz **Stream de entrada de Audio comprimido** API proporciona una manera de transmitir audio comprimido en el servicio de voz mediante PullStream o PushStream.

> [!IMPORTANT]
> Solo se admite la transmisión por secuencias de audio comprimido para C++, C#y Java en Linux (Ubuntu 16.04, 18.04 Ubuntu, Debian 9).

Para wav/PCM consulte la documentación principal de voz.  Fuera de PCM/wav, se admiten los siguientes formatos de entrada de códec comprimido:

- MP3
- OPUS/OGG

## <a name="prerequisites-to-using-codec-compressed-audio-input"></a>Requisitos previos para usar el códec comprimen de entrada de audio

Instalar estas dependencias adicionales para usar la entrada de audio comprimida con el SDK de voz para Linux:

```sh
sudo apt install libgstreamer1.0-0 gstreamer1.0-plugins-base gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly
```

## <a name="example-code-using-codec-compressed-audio-input"></a>Código de ejemplo con el códec comprimido de entrada de audio

Para transmitir en un formato de audio comprimido a los servicios de voz, crear `PullAudioInputStream` o `PushAudioInputStream`. A continuación, cree un `AudioConfig` desde una instancia de la clase stream, especificar el formato de compresión de la secuencia.

Supongamos que tiene una clase de flujo de entrada llamada `myPushStream` y usas OPUS/OGG. El código puede tener este aspecto:

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
