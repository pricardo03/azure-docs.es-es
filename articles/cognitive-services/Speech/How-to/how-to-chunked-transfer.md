---
title: Cómo realizar la transferencia fragmentada de una secuencia de audio | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Uso de la transferencia fragmentada para enviar una secuencia de audio al servicio Bing Speech
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d9796cf60e2695c21d781131c935d24891401efa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60515003"
---
# <a name="chunked-transfer-encoding"></a>Codificación de la transferencia fragmentada

[!INCLUDE [Deprecation note](../../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Para transcribir la conversión de voz a texto, Microsoft Speech Recognition API permite enviar el audio como un fragmento completo o dividirlo en pequeños fragmentos. Para la transmisión en secuencias de audio de manera eficaz y la reducción de la latencia de la transcripción, se recomienda utilizar la [codificación de la transferencia fragmentada](https://en.wikipedia.org/wiki/Chunked_transfer_encoding) para transmitir en secuencias el audio al servicio. Otras implementaciones pueden dar lugar a que el usuario perciba una mayor latencia. Para más información, consulte la página [Audio Streams](../concepts.md#audio-streams) (Secuencias de audio).

> [!NOTE]
> No se pueden cargar más de 10 segundos de audio en cualquier solicitud y la duración total de la solicitud no puede superar los 14 segundos.
> [!NOTE]
> Solo debe especificar la codificación de la transferencia fragmentada si utiliza la [API de REST](../GetStarted/GetStartedREST.md) para llamar al servicio de voz. No es necesario que las aplicaciones que utilizan [bibliotecas cliente](../GetStarted/GetStartedClientLibraries.md) configuren la codificación de la transferencia fragmentada.

El código siguiente muestra cómo establecer la codificación de la transferencia fragmentada y enviar un archivo de audio fragmentado en fragmentos de 1024 bytes.

```cs

HttpWebRequest request = null;
request = (HttpWebRequest)HttpWebRequest.Create(requestUri);
request.SendChunked = true;
request.Accept = @"application/json;text/xml";
request.Method = "POST";
request.ProtocolVersion = HttpVersion.Version11;
request.Host = @"speech.platform.bing.com";
request.ContentType = @"audio/wav; codec=audio/pcm; samplerate=16000";
request.Headers["Ocp-Apim-Subscription-Key"] = "YOUR_SUBSCRIPTION_KEY";

using (fs = new FileStream(audioFile, FileMode.Open, FileAccess.Read))
{

    /*
    * Open a request stream and write 1024 byte chunks in the stream one at a time.
    */
    byte[] buffer = null;
    int bytesRead = 0;
    using (Stream requestStream = request.GetRequestStream())
    {
        /*
        * Read 1024 raw bytes from the input audio file.
        */
        buffer = new Byte[checked((uint)Math.Min(1024, (int)fs.Length))];
        while ((bytesRead = fs.Read(buffer, 0, buffer.Length)) != 0)
        {
            requestStream.Write(buffer, 0, bytesRead);
        }

        // Flush
        requestStream.Flush();
    }
}
```
