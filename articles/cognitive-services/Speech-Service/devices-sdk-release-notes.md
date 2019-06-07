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
ms.date: 05/22/2019
ms.author: wellsi
ms.openlocfilehash: 1c91cde45a6a420376af36f70487adf7fe0ee83a
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66751813"
---
# <a name="release-notes-of-cognitive-services-speech-devices-sdk"></a>Notas de la versión de Speech Devices SDK de Cognitive Services
En las siguientes secciones se indican los cambios en las versiones más recientes.

## <a name="speech-devices-sdk-151"></a>Dispositivos de Speech SDK 1.5.1:

*   Incluir [conversación transcripción](conversation-transcription-service.md) en la aplicación de ejemplo.
*   Actualiza el [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) componente a la versión 1.5.1. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).

## <a name="cognitive-services-speech-devices-sdk-150-2019-may-release"></a>Dispositivos de cognitive Services Speech SDK 1.5.0: Versión de mayo de 2019

*   SDK de dispositivos de voz es ahora disponibilidad general y ya no está en versión preliminar controlada.
*   Actualiza el [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) componente a la versión 1.5.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew).
*   Nueva tecnología de word wake ofrece mejoras de calidad importantes, vea cambios importantes.
*   Nueva canalización de procesamiento de audio para el reconocimiento de lejos campo mejorado.

**Cambios importantes**

*   Debido a la nueva tecnología de word wake todas las palabras de reactivación deben volver a crearse en nuestro portal de word wake mejorada. Para quitar completamente antiguas palabras clave del dispositivo desinstalación la aplicación antigua.
    - adb uninstall com.microsoft.coginitiveservices.speech.samples.sdsdkstarterapp

## <a name="cognitive-services-speech-devices-sdk-140-2019-apr-release"></a>Dispositivos de cognitive Services Speech SDK 1.4.0: Versión de abril de 2019 

* Actualiza el [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) componente a la versión 1.4.0. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew). 

## <a name="cognitive-services-speech-devices-sdk-131-2019-mar-release"></a>Dispositivos de cognitive Services Speech SDK 1.3.1: Versión de marzo de 2019 

* Actualiza el [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) componente a la versión 1.3.1. Para más información, consulte sus [notas de la versión](https://aka.ms/csspeech/whatsnew). 
*   Word wake actualizada de control, vea cambios importantes.
*   Aplicación de ejemplo agrega la opción de lenguaje para el reconocimiento de voz y traducción.

**Cambios importantes** 

*   [Instalación de una palabra wake](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws) ha sido simplificado, ahora es parte de la aplicación y no necesita una instalación independiente en el dispositivo.
*   Ha cambiado el reconocimiento de word de reactivación y se admiten dos eventos.
    - RecognizingKeyword, indica el resultado de voz contiene texto de palabra clave (sin comprobar).
    - RecognizedKeyword, indica que el reconocimiento de esa palabra clave completado reconocer la palabra clave dada.


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
* Los archivos de modelo de KWS no son compatibles con Speech Devices SDK 1.0.1. Los archivos existentes de Wake Word se eliminarán después de que los nuevos archivos de Wake Word se escriban en el dispositivo. 

## <a name="cognitive-services-speech-devices-sdk-050-2018-aug-release"></a>SDK de dispositivos de Voz de Cognitive Services 0.5.0: versión de agosto de 2018

* Se ha mejorado la precisión del reconocimiento de voz mediante la corrección de un error en el código de procesamiento de audio.
* Se ha actualizado el componente [Speech SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) a la versión 0.5.0. Para más información, consulte sus [notas de la versión](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release).

## <a name="cognitive-services-speech-devices-sdk-0212733-2018-may-release"></a>SDK de dispositivos de Voz de Cognitive Services 0.2.12733: versión de mayo de 2018

La primera versión preliminar pública de Speech Devices SDK de Cognitive Services.
