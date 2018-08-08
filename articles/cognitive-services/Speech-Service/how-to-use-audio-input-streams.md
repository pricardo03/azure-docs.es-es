---
title: Conceptos de AudioInputStream
description: Información general sobre las funciones de AudioInputStream API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: fmegen
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 06/07/2018
ms.author: fmegen
ms.openlocfilehash: b3e12fbc616c8d67b557102c6094467e119a23f1
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39281912"
---
# <a name="about-the-audio-input-stream-api"></a>Acerca de la API de flujo de entrada de audio

La API **Transmisión de entrada de audio** proporciona una manera para transmitir secuencias de audio a los identificadores en lugar de usar el micrófono o las API de archivo de entrada.

## <a name="api-overview"></a>Introducción a la API

En la API se usan dos componentes, `AudioInputStream` (los datos de audio sin formato) y `AudioInputStreamFormat`.

`AudioInputStreamFormat` define el formato de los datos de audio. Se puede comparar con la estructura `WAVEFORMAT` estándar de los archivos de sonido de Windows.

  - `FormatTag`

    El formato del audio. En la actualidad, el SDK de Voz solo admite `format 1` (PCM: little endian).

  - `Channels`

    El número de canales. El servicio de voz actual solo admite material de audio de un único canal (mono).

  - `SamplesPerSec`

    La frecuencia de muestreo. Una grabación de micrófono típica tiene 16 000 muestras por segundo.

  - `AvgBytesPerSec`

    El promedio de bytes por segundo, calculados como `SamplesPerSec * Channels * ceil(BitsPerSample, 8)`. El promedio de bytes por segundo puede ser diferente para las secuencias de audio que usan velocidades de bits variables.

  - `BlockAlign`

    El tamaño de un fotograma, calculado como `Channels * ceil(wBitsPerSample, 8)`. Debido al relleno, es posible que el valor real sea mayor que este valor.

  - `BitsPerSample`

    Los bits por muestra. Una secuencia de audio típica usa 16 bits por muestra (calidad de CD).

El adaptador de secuencia personalizado reemplazará la clase base `AudioInputStream`. Este adaptador debe implementar estas funciones:

   - `GetFormat()`

     Se llama a esta función para obtener el formato de la secuencia de audio. Obtiene un puntero al búfer de AudioInputStreamFormat.

   - `Read()`

     Se llama a esta función para obtener los datos de la secuencia de audio. Un parámetro es un puntero al búfer en el que se van a copiar los datos de audio. El segundo parámetro es el tamaño del búfer. La función devuelve el número de bytes copiados en el búfer. Un valor devuelto de `0` indica el final de la secuencia.

   - `Close()`

     Se llama a esta función para cerrar la secuencia de audio.

## <a name="usage-examples"></a>Ejemplos de uso

Por lo general, cuando se usan secuencias de entrada de audio se siguen estos pasos:

  - Identificar el formato de la secuencia de audio. El formato debe ser compatible con el SDK y el servicio de voz. Actualmente se admite la configuración siguiente:

    Una etiqueta de formato de audio (PCM), un canal, 16 000 muestras por segundo, 32 000 bytes por segundo, alineación de dos bloques (de 16 bits incluido el relleno para una muestra), 16 bits por muestra

  - Asegúrese de que el código puede proporcionar los datos de audio RAW según las especificaciones indicadas anteriormente. Si los datos del origen de audio no coinciden con los formatos admitidos, el audio se debe transcodificar al formato requerido.

  - Derive la clase de secuencia de entrada de audio personalizada de `AudioInputStream`. Implemente la operación `GetFormat()`, `Read()` y `Close()`. La firma exacta de la función depende del idioma, pero el código tendrá un aspecto similar a este ejemplo de código:

    ```
     public class ContosoAudioStream : AudioInputStream {
        ContosoConfig config;

        public ContosoAudioStream(const ContosoConfig& config) {
            this.config = config;
        }

        public void GetFormat(AudioInputStreamFormat& format) {
            // returns format data to the caller.
            // e.g. format.FormatTag = config.XXX;
            // ...
        }

        public size_t Read(byte *buffer, size_t size) {
            // returns audio data to the caller.
            // e.g. return read(config.YYY, buffer, size);
        }

        public void Close() {
            // close and cleanup resources.
        }
     };
    ```

  - Use la secuencia de entrada de audio:

    ```
    var contosoStream = new ContosoAudioStream(contosoConfig);

    var factory = SpeechFactory.FromSubscription(...);
    var recognizer = CreateSpeechRecognizerWithStream(contosoStream);

    // run stream through recognizer
    var result = await recognizer.RecognizeAsync();

    var text = result.GetText();

    // In some languages you need to delete the stream explicitly.
    // delete contosoStream;
    ```

  - En algunos idiomas, se debe eliminar `contosoStream` de forma explícita una vez completado el reconocimiento. No se puede liberar el AudioStream antes de leer la entrada completa. En un escenario en el que se use `StopContinuousRecognitionAsync` y `StopContinuousRecognitionAsync` se necesita un concepto que se muestra en este ejemplo:

    ```
    var contosoStream = new ContosoAudioStream(contosoConfig);

    var factory = SpeechFactory.FromSubscription(...);
    var recognizer = CreateSpeechRecognizerWithStream(contosoStream);

    // run stream through recognizer
    await recognizer.StartContinuousRecognitionAsync();

    // ERROR: do not delete the contosoStream before ending recognition!
    // delete contosoStream;

    await recognizer.StopContinuousRecognitionAsync();

    // OK: Safe to delete the contosoStream.
    // delete contosoStream;
    ```

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
