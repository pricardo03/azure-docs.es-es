---
title: 'Long Audio API (versión preliminar): servicio de Voz'
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 10/26/2019
ms.author: erhopf
ms.openlocfilehash: 391cddbbd1b69fb7cb5422adbaea2f3378e273bf
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73580161"
---
# <a name="long-audio-api-preview"></a>Long Audio API (versión preliminar)

Long Audio API está diseñada para la síntesis asincrónica de texto de formato largo a voz (por ejemplo: audiolibros). Esta API no devuelve audio sintetizado en tiempo real, sino que, en su lugar, se espera que se sondeen las respuestas y se consuman las salidas a medida que estén disponibles desde el servicio. A diferencia de Text To Speech API, que usa el SDK de Voz, Long Audio API puede crear audio sintetizado de más de diez minutos, lo que resulta idóneo para los editores y las plataformas de contenido de audio.

Ventajas adicionales de Long Audio API:

* La voz sintetizada devuelta por el servicio utiliza voces neuronales, lo que garantiza salidas de audio de alta fidelidad.
* Dado que no se admiten las respuestas en tiempo real, no es necesario implementar un punto de conexión de voz.

## <a name="workflow"></a>Flujo de trabajo

Normalmente, cuando use Long Audio API, enviará un archivo de texto o archivos que se sintetizarán, sondeará el estado y, a continuación, si el estado es correcto, podrá descargar la salida de audio.

El diagrama a continuación proporciona una introducción general del flujo de trabajo.

![Diagrama de flujo de trabajo de Long Audio API](media/long-audio-api/long-audio-api-workflow.png)

## <a name="prepare-content-for-synthesis"></a>Preparación del contenido para la síntesis

Cuando prepare el archivo de texto, asegúrese de lo siguiente:

* Se trata de texto sin formato (.txt) o texto SSML (.txt).
  * En el caso de texto sin formato, cada párrafo se separa al presionar **Entrar**: consulte un [ejemplo de entrada de texto sin formato](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/en-US.txt).
  * En el caso de texto SSML, cada fragmento de SSML se considera un párrafo. Los fragmentos de SSML se deben separar con párrafos distintos: consulte un [ejemplo de entrada de texto SSML](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/SSMLTextInputSample.txt). Para el código de idioma, consulte [Lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md).
* Está codificado como [UTF-8 con marca BOM](https://www.w3.org/International/questions/qa-utf8-bom.en#bom).
* Contiene más de 10 000 caracteres o más de 50 párrafos
* Es un solo archivo, no un archivo ZIP.

## <a name="audio-output-formats"></a>Formatos de salida de audio

Long Audio API admite los siguientes formatos de salida de audio:

> [!NOTE]
> El formato de audio predeterminado es riff-16khz-16bit-mono-pcm.

* riff-8khz-16bit-mono-pcm
* riff-16khz-16bit-mono-pcm
* riff-24khz-16bit-mono-pcm
* riff-48khz-16bit-mono-pcm
* audio-16khz-32kbitrate-mono-mp3
* audio-16khz-64kbitrate-mono-mp3
* audio-16khz-128kbitrate-mono-mp3
* audio-24khz-48kbitrate-mono-mp3
* audio-24khz-96kbitrate-mono-mp3
* audio-24khz-160kbitrate-mono-mp3

## <a name="quickstarts"></a>Guías de inicio rápido

Se ofrecen guías de inicio rápido diseñadas para ayudarle a ejecutar Long Audio API correctamente. Esta tabla incluye una lista de inicios rápidos de Long Audio API en función del idioma.

* [Inicio rápido: Python](https://aka.ms/long-audio-python)

## <a name="sample-code"></a>Código de ejemplo
Hay un ejemplo de código para Long Audio API disponible en GitHub.

* [Código de ejemplo: Python](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/Python)
* [Código de ejemplo: C#](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/CSharp)
* [Código de ejemplo: Java](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/)
