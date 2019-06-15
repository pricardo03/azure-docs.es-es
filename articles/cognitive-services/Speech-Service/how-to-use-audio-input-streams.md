---
title: Conceptos de flujo de entrada de audio del SDK de Voz
titleSuffix: Azure Cognitive Services
description: Una introducción a las funcionalidades de Audio Input Stream API del SDK de Voz.
services: cognitive-services
author: fmegen
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: fmegen
ms.openlocfilehash: f34504dddaa21cc162476d1fdb462e7be5c48629
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60697920"
---
# <a name="about-the-speech-sdk-audio-input-stream-api"></a>Acerca de Audio Input Stream API del SDK de Voz

La **Audio Input Stream API** del SDK de Voz proporciona una manera de transmitir secuencias de audio a los reconocedores en lugar de usar el micrófono o las API de archivo de entrada.

Al usar flujos de entrada de audio, son necesarios los pasos siguientes:

- Identificar el formato de la secuencia de audio. El formato debe ser compatible con el servicio Voz y su SDK. Actualmente, solo se admite la configuración siguiente:

  Muestras de audio en formato PCM, un canal, 16000 muestras por segundo, 32000 bytes por segundo, alineación de dos bloques (de 16 bits incluido el relleno para una muestra), 16 bits por muestra.

  El código correspondiente en el SDK para crear el formato de audio tiene este aspecto:

  ```
  byte channels = 1;
  byte bitsPerSample = 16;
  int samplesPerSecond = 16000;
  var audioFormat = AudioStreamFormat.GetWaveFormatPCM(samplesPerSecond, bitsPerSample, channels);
  ```

- Asegúrese de que el código pueda proporcionar los datos de audio sin procesar de acuerdo con estas especificaciones. Si los datos del origen de audio no coinciden con los formatos admitidos, el audio se debe transcodificar al formato requerido.

- Cree su propia clase de flujo de entrada de audio derivada de `PullAudioInputStreamCallback`. Implemente los miembros `Read()` y `Close()`. La signatura de función exacta depende del lenguaje, pero el código tendrá un aspecto similar al de este ejemplo:

  ```
   public class ContosoAudioStream : PullAudioInputStreamCallback {
      ContosoConfig config;

      public ContosoAudioStream(const ContosoConfig& config) {
          this.config = config;
      }

      public int Read(byte[] buffer, uint size) {
          // returns audio data to the caller.
          // e.g. return read(config.YYY, buffer, size);
      }

      public void Close() {
          // close and cleanup resources.
      }
   };
  ```

- Cree una configuración de audio según el formato y el flujo de entrada del audio. Pase la configuración de voz normal y la configuración de entrada de audio cuando se cree el reconocedor. Por ejemplo:

  ```
  var audioConfig = AudioConfig.FromStreamInput(new ContosoAudioStream(config), audioFormat);

  var speechConfig = SpeechConfig.FromSubscription(...);
  var recognizer = new SpeechRecognizer(speechConfig, audioConfig);

  // run stream through recognizer
  var result = await recognizer.RecognizeOnceAsync();

  var text = result.GetText();
  ```

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
