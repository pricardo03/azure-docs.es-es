---
title: 'Acerca del SDK de dispositivos de voz: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Introducción al SDK de dispositivos de voz. El servicio de Voz funciona con una amplia variedad de dispositivos y orígenes de audio. Ahora puede llevar las aplicaciones de voz al siguiente nivel con hardware y software coincidente. El SDK de dispositivos de voz es una biblioteca pre-ajustada que está emparejada con kits de desarrollo específicos integrados de la matriz del micrófono.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 341f35d04c7f3f591198acbbf05ac32afe84e95c
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67718543"
---
# <a name="about-the-speech-devices-sdk"></a>Introducción a Speech Devices SDK

Los [servicios de Voz](overview.md) funcionan con una amplia variedad de dispositivos y orígenes de audio. Ahora puede llevar las aplicaciones de voz al siguiente nivel con hardware y software coincidente. El SDK de dispositivos de voz es una biblioteca preajustada que está emparejada con kits de desarrollo específicos integrados de la matriz del micrófono.

El SDK de dispositivos de voz puede ayudarle a:

* Probar rápidamente nuevos escenarios de voz.
* Integrar más fácilmente los servicios de Voz basados en la nube en el dispositivo.
* Crear una experiencia de usuario excepcional para sus clientes.

El SDK de dispositivos de voz consume el [SDK de voz](speech-sdk.md). Emplea el SDK de voz para enviar el audio que se procesa mediante nuestro algoritmo de procesamiento de audio avanzado desde la matriz del micrófono del dispositivo a los [servicios de Voz](overview.md). Usa audio multicanal para proporcionar un [reconocimiento de voz](speech-to-text.md) de campo lejano más preciso mediante la supresión de ruido, la cancelación del eco, la formación de haces y la eliminación de la reverberación.

También puede usar Speech Devices SDK para compilar dispositivos ambientales que tienen su propia [palabra de reactivación personalizada](speech-devices-sdk-create-kws.md), de modo que la indicación que inicia la interacción del usuario es única para su marca.

Speech Devices SDK facilita diversos escenarios habilitados para voz, como [asistentes virtuales por voz](https://aka.ms/bots/speech/va), sistemas de pedidos de comida para llevar, [transcripción de conversaciones](conversation-transcription-service.md) y altavoces inteligentes. Puede responder a los usuarios con texto, hablarles con una voz predeterminada o [personalizada](how-to-customize-voice-font.md), proporcionar resultados de búsqueda, [traducir](speech-translation.md) a otros idiomas y mucho más. Estamos deseando ver lo que ha compilado.

## <a name="get-the-speech-devices-sdk"></a>Obtener el SDK de dispositivos de voz

### <a name="android"></a>Android

En el caso los dispositivos Android, descargue la versión más reciente del [SDK para dispositivos de voz de Android](https://aka.ms/sdsdk-download-android).

### <a name="windows"></a>Windows

En el caso de Windows, la aplicación de ejemplo que se proporciona es una aplicación de Java multiplataforma. Descargue la última versión del [SDK para dispositivos de voz de JRE](https://aka.ms/sdsdk-download-JRE).
La aplicación se crea con el paquete del SDK de Voz y el entorno de desarrollo integrado de Java de Eclipse (v4) en Windows de 64 bits. Se ejecuta en un entorno de tiempo de ejecución de Java 8 (JRE) de 64 bits.

### <a name="linux"></a>Linux

En el caso de Linux, la aplicación de ejemplo que se proporciona es una aplicación de Java multiplataforma. Descargue la última versión del [SDK para dispositivos de voz de JRE](https://aka.ms/sdsdk-download-JRE).
La aplicación se crea con el paquete del SDK de Voz y el entorno de desarrollo integrado de Java de Eclipse (v4) en Linux de 64 bits (Ubuntu 16.04, Ubuntu 18.04 y Debian 9). Se ejecuta en un entorno de tiempo de ejecución de Java 8 (JRE) de 64 bits.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Elija el dispositivo de voz](get-speech-devices-sdk.md)
>
> [!div class="nextstepaction"]
> [Get a Speech Services subscription key for free](get-started.md) (Consiga una clave de suscripción a los servicios de voz gratis)
