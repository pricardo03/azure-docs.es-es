---
title: Obtener el SDK de dispositivos de voz
titleSuffix: Azure Cognitive Services
description: El servicio de Voz funciona con una amplia variedad de dispositivos y orígenes de audio. Ahora puede llevar las aplicaciones de voz al siguiente nivel con hardware y software coincidente. En este artículo aprenderá a acceder al SDK de dispositivos de voz para comenzar a desarrollar.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: b9a0890000cda0b3663ac29bee61fc1c702f6254
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65410690"
---
# <a name="get-the-cognitive-services-speech-devices-sdk"></a>Obtener el SDK de dispositivos de voz de Cognitive Services

El SDK de dispositivos de voz es una biblioteca preajustada diseñada para funcionar con kits de desarrollo específicos integrados y diversas configuraciones de matrices de micrófonos.

## <a name="choose-a-development-kit"></a>Selección de un kit de desarrollo

|Dispositivos|Especificación|DESCRIPCIÓN|Escenarios|
|--|--|--|--|
|[Roobo Smart Audio Dev Kit](https://ddk.roobo.com)</br>[Configuración](speech-devices-sdk-roobo-v1.md) / [Inicio rápido](speech-devices-sdk-android-quickstart.md)![Roobo Smart Audio Dev Kit](media/speech-devices-sdk/device-roobo-v1.jpg)|Matriz de 7 micrófonos, ARM SOC, Wi-Fi, salida de audio, E/S. </br>Android|El primer SDK de dispositivos de voz en adaptar la matriz de micrófonos de Microsoft y el SDK de procesamiento frontal para el desarrollo de escenarios de transcripción y voz de alta calidad|Transcripción de conversaciones, altavoz inteligente, agente por voz, ponible|
|[Azure Kinect DK](https://azure.microsoft.com/services/kinect-dk/)![Azure Kinect DK](media/speech-devices-sdk/device-azure-kinect-dk.jpg)|Matriz de 7 micrófonos, cámaras RGB y de profundidad </br>Windows/Linux|Un kit de desarrollo con sensores avanzados de inteligencia artificial (AI) para la creación de modernos modelos de voz y visión artificial. Combina una de las mejores matrices de micrófonos espaciales y cámaras de profundidad con una cámara de vídeo y un sensor de orientación, todo ello en un pequeño dispositivo con varios modos, opciones y SDK para dar cabida a una amplia variedad de tipos de proceso.|Transcripción de conversaciones, robótica, edificios inteligentes|
|Roobo Smart Audio Dev Kit 2![Roobo Smart Audio Dev Kit 2](media/speech-devices-sdk/device-roobo-v2.jpg)|Matriz de 7 micrófonos, ARM SOC, Bluetooth, E/S. </br>Linux|El SDK de dispositivos de voz de segunda generación que proporciona un sistema operativo alternativo y más características en un diseño de referencia rentable.|Transcripción de conversaciones, altavoz inteligente, agente por voz, ponible|
|Placa de desarrollo URbetter T11![URbetter DDK](media/speech-devices-sdk/device-urbetter.jpg)|Matriz de 7 micrófonos, ARM SOC, Wi-Fi, Ethernet, HDMI, cámara USB. </br>Linux|Un SDK de dispositivos de voz de nivel industrial que se adapta a la matriz de micrófonos de Microsoft y es compatible con E/S extendida como HDMI/Ethernet y más periféricos USB|Transcripción de conversaciones, educación, hospitales, robots, caja OTT, agente por voz, servicio de comidas rápidas|

## <a name="download-the-speech-devices-sdk"></a>Descargar el SDK de dispositivos de voz

Descargue el [SDK de dispositivos de voz](https://aka.ms/sdsdk-download).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Introducción al SDK de dispositivos de voz](https://aka.ms/sdsdk-quickstart)
