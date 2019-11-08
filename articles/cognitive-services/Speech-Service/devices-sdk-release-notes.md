---
title: Documentación de Speech Devices SDK
titleSuffix: Azure Cognitive Services
description: 'Notas de la versión: qué ha cambiado en las versiones más recientes'
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: wellsi
ms.openlocfilehash: ba1db1ccba6a1849756e75c9b9f7078371da5bfb
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73464749"
---
# <a name="release-notes-of-cognitive-services-speech-devices-sdk"></a>Notas de la versión de Speech Devices SDK de Cognitive Services
En las siguientes secciones se indican los cambios en las versiones más recientes.

## <a name="speech-devices-sdk-160"></a>SDK de dispositivos de voz 1.6.0:

*   Compatibilidad con [Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/) en Windows y Linux con una [aplicación de ejemplo](https://aka.ms/sdsdk-download) común
*   Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.6.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).

## <a name="speech-devices-sdk-151"></a>SDK de dispositivos de voz 1.5.1:

*   Incluya [transcripción de conversaciones](conversation-transcription-service.md) en la aplicación de ejemplo.
*   Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.5.1. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).

## <a name="cognitive-services-speech-devices-sdk-150-2019-may-release"></a>SDK de dispositivos de voz 1.5.0 de Cognitive Services: Versión de mayo de 2019

*   SDK de dispositivos de voz es ahora disponibilidad general y ya no está en versión preliminar controlada.
*   Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.5.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).
*   La nueva tecnología de palabra clave proporciona importantes mejoras de calidad; consulte Cambios importantes.
*   Nueva canalización de procesamiento de audio para el reconocimiento mejorado de campo lejano.

**Cambios importantes**

*   debido a la nueva tecnología de palabra clave, todas las palabras clave deben volver a crearse en nuestro portal mejorado de palabras clave. Para quitar completamente las palabras clave antiguas del dispositivo, desinstale la aplicación antigua.
    - adb uninstall com.microsoft.coginitiveservices.speech.samples.sdsdkstarterapp

## <a name="cognitive-services-speech-devices-sdk-140-2019-apr-release"></a>SDK de dispositivos de Voz de Cognitive Services 1.4.0: Versión de abril de 2019

* Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.4.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).

## <a name="cognitive-services-speech-devices-sdk-131-2019-mar-release"></a>SDK de dispositivos de voz 1.3.1 de Cognitive Services: Versión de marzo de 2019

* Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.3.1. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).
*   Administración de palabras clave; consulte Cambios importantes.
*   La aplicación de ejemplo agrega la opción de idioma para el reconocimiento y la traducción de voz.

**Cambios importantes**

*   La [instalación de una palabra clave](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws) se ha simplificado, y ahora es parte de la aplicación (no necesita una instalación independiente en el dispositivo).
*   El reconocimiento de la palabra clave ha cambiado, y se admiten dos eventos.
    - RecognizingKeyword, indica que el resultado de voz contiene texto de palabra clave (sin comprobar).
    - RecognizedKeyword, indica que el reconocimiento de la palabra clave terminó de reconocer la palabra clave especificada.


## <a name="cognitive-services-speech-devices-sdk-110-2018-nov-release"></a>SDK de dispositivos de Voz de Cognitive Services 1.1.0: Versión de noviembre de 2018

* Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.1.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).
* Ha aumentado la precisión del reconocimiento de voz a gran distancia gracias a nuestro algoritmo mejorado de procesamiento de audio.
* La aplicación de ejemplo ahora es compatible con el reconocimiento de voz en chino.

## <a name="cognitive-services-speech-devices-sdk-101-2018-oct-release"></a>SDK de dispositivos de Voz de Cognitive Services 1.0.1: versión de octubre de 2018

* Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 1.0.1. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).
* La precisión del reconocimiento de voz se verá mejorada gracias a nuestro algoritmo mejorado de procesamiento de audio.  
* Se ha corregido un error en la sesión de audio de reconocimiento continuo.

**Cambios importantes**

* Con esta versión se presentan una serie de cambios importantes. Consulte [esta página](https://aka.ms/csspeech/breakingchanges_1_0_0) para obtener detalles relacionados con las API.
* Los archivos de modelo de KWS no son compatibles con Speech Devices SDK 1.0.1. Los archivos existentes de palabras clave se eliminarán después de que los nuevos se escriban en el dispositivo.

## <a name="cognitive-services-speech-devices-sdk-050-2018-aug-release"></a>SDK de dispositivos de Voz de Cognitive Services 0.5.0: versión de agosto de 2018

* Se ha mejorado la precisión del reconocimiento de voz mediante la corrección de un error en el código de procesamiento de audio.
* Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 0.5.0. Para más información, consulte sus [notas de la versión](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release).

## <a name="cognitive-services-speech-devices-sdk-0212733-2018-may-release"></a>SDK de dispositivos de Voz de Cognitive Services 0.2.12733: versión de mayo de 2018

La primera versión preliminar pública de Speech Devices SDK de Cognitive Services.
