---
title: Secuencia de audio comprimido con códec mediante el SDK de Voz en iOS
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo hacer secuencias de audio comprimido a Servicios de Voz de Azure con Speech SDK en iOS.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 09/20/2019
ms.author: chlandsi
ms.openlocfilehash: 1d78c690fd07eb974418f0ea17d71d1f394d863d
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74109571"
---
# <a name="how-to-use-codec-compressed-audio-input-with-the-speech-sdk-on-ios"></a>Procedimientos para: Uso de entradas de audio comprimido con códec con SDK de voz en iOS

La API **Compressed Audio Input Stream** de Speech SDK permite transmitir audio comprimido a Servicios de Voz mediante un flujo de inserción o extracción.

> [!IMPORTANT]
> Se requiere Speech SDK versión 1.7.0 o posterior para la secuencia de audio comprimido en iOS. También se admite para [C++, C# y Java en Linux (Ubuntu 16.04, Ubuntu 18.04, Debian 9) ](how-to-use-codec-compressed-audio-input-streams.md) y [Java en Android.](how-to-use-codec-compressed-audio-input-streams-android.md)

Para wav/PCM consulte la documentación principal de voz. Aparte de wav/PCM, se admiten los siguientes formatos de entrada de audio comprimido con códec:

- MP3
- OPUS/OGG
- FLAC
- ALAW en el contenedor WAV
- MULAW en el contenedor WAV

## <a name="prerequisites"></a>Requisitos previos

El control del audio comprimido se implementa mediante [GStreamer](https://gstreamer.freedesktop.org). Por motivos de concesión de licencias, estas funciones no se pueden enviar con el SDK, pero es necesario que los programadores de aplicaciones compilen una biblioteca de contenedor que contenga estas funciones y que se envíen con las aplicaciones mediante el SDK.

Para compilar esta biblioteca de contenedor, primero descargue e instale el [SDK de GStreamer](https://gstreamer.freedesktop.org/data/pkg/ios/1.16.0/gstreamer-1.0-devel-1.16.0-ios-universal.pkg). A continuación, descargue el proyecto de Xcode para la [biblioteca de contenedor](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/objective-c/ios/compressed-streams/GStreamerWrapper).

Abra el proyecto en Xcode y compílelo para el destino **Generic iOS Device** (dispositivo iOS genérico); no funcionará si se compila para un destino específico.

El paso de compilación generará un conjunto dinámico de marcos con una biblioteca dinámica para todas las arquitecturas necesarias con el nombre de `GStreamerWrapper.framework`.

Este marco debe estar incluido en todas las aplicaciones que usan secuencias de audio comprimidas con Speech SDK.

Aplique la siguiente configuración en el proyecto de Xcode para lograr esto:

1. Copie tanto el `GStreamerWrapper.framework` que acaba de integrar como el marco de Speech SDK de Cognitive Services, que puede descargar [aquí](https://aka.ms/csspeech/iosbinary), en el directorio que contiene el proyecto de ejemplo.
1. Ajuste las rutas de acceso para los marcos en la _Configuración del proyecto_.
   1. En la pestaña **General**, en el encabezado **Binarios incrustados**, agregue la biblioteca del SDK como un marco de trabajo: **Agregar binarios insertados** > **Agregar otros...** > vaya al directorio que eligió y seleccione ambos marcos.
   1. Vaya a la pestaña **Configuración de compilación** y active la configuración **Todos**.
1. Agregue el directorio `$(SRCROOT)/..` a _Rutas de búsqueda de marco_ bajo el encabezado **Rutas de búsqueda**.

## <a name="example-code-using-codec-compressed-audio-input"></a>Ejemplo de código que usa una entrada de audio comprimido con códec

Para transmitir en un formato de audio comprimido a Servicios de Voz, cree `SPXPullAudioInputStream` o `SPXPushAudioInputStream`.

En el siguiente fragmento de código se muestra cómo crear un `SPXAudioConfiguration` a partir de una instancia de `SPXPushAudioInputStream`, especificando MP3 como formato de compresión de la secuencia.

[!code-objectivec[Set up the input stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=66-77&highlight=2-11)]

El siguiente fragmento de código muestra cómo se pueden leer los datos de audio comprimidos de un archivo y bombearlos en `SPXPushAudioInputStream`.

[!code-objectivec[Push compressed audio data into the stream](~/samples-cognitive-services-speech-sdk/samples/objective-c/ios/compressed-streams/CompressedStreamsSample/CompressedStreamsSample/ViewController.m?range=105-151&highlight=19-44)]

## <a name="next-steps"></a>Pasos siguientes

- [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
- [See how to recognize speech in Java](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-java) (Vea cómo funciona el reconocimiento de voz en Java)
