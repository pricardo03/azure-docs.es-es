---
title: 'Acerca del SDK de los dispositivos de voz: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Introducción al SDK de dispositivos de voz. El servicio de voz funciona con una amplia variedad de dispositivos y orígenes de audio. El SDK de dispositivos de voz es una biblioteca pre-ajustada que está emparejada con kits de desarrollo específicos integrados de la matriz del micrófono.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: erhopf
ms.openlocfilehash: 854e7beb2afd8ae838455f77ff448f13d8b3fbea
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77188973"
---
# <a name="about-the-speech-devices-sdk"></a>Introducción a Speech Devices SDK

El [servicio de voz](overview.md) funciona con una amplia variedad de dispositivos y orígenes de audio. Ahora puede llevar las aplicaciones de voz al siguiente nivel con hardware y software coincidente. El SDK de dispositivos de voz es una biblioteca preajustada que está emparejada con kits de desarrollo específicos integrados de la matriz del micrófono.

El SDK de dispositivos de voz puede ayudarle a:

- Probar rápidamente nuevos escenarios de voz.
- Integrar más fácilmente el servicio de voz basado en la nube en el dispositivo.
- Crear una experiencia de usuario excepcional para sus clientes.

El SDK de dispositivos de voz consume el [SDK de voz](speech-sdk.md). Uso de nuestros algoritmos avanzados de procesamiento de audio con la matriz del micrófono del dispositivo para enviar el audio al [servicio de voz](overview.md). Ofrece un [reconocimiento de voz](speech-to-text.md) de campo lejano preciso mediante la supresión de ruido, la cancelación del eco, la formación de haces y la eliminación de la reverberación.

También puede usar el SDK de dispositivos de voz para crear dispositivos de ambiente que tengan su propia [palabra clave personalizada](speech-devices-sdk-create-kws.md). Una palabra clave personalizada proporciona una indicación de inicio de una interacción del usuario única para su marca.

El SDK de dispositivos de voz permite diversos escenarios habilitados para voz, como [asistentes de voz](https://aka.ms/bots/speech/va), sistemas de pedidos de comida para llevar, [transcripción de conversaciones](conversation-transcription-service.md) y altavoces inteligentes. Puede responder a los usuarios con texto, hablarles con una voz predeterminada o [personalizada](how-to-customize-voice-font.md), proporcionar resultados de búsqueda, [traducir](speech-translation.md) a otros idiomas y mucho más. Estamos deseando ver lo que ha compilado.

## <a name="get-the-speech-devices-sdk"></a>Obtener el SDK de dispositivos de voz

### <a name="android"></a>Android

Para los dispositivos Android, descargue la versión más reciente del [SDK para dispositivos de voz de Android](https://aka.ms/sdsdk-download-android).

### <a name="windows"></a>Windows

En el caso de Windows, la aplicación de ejemplo se proporciona como aplicación Java multiplataforma. Descargue la última versión del [SDK para dispositivos de voz de JRE](https://aka.ms/sdsdk-download-JRE).
La aplicación se crea con el paquete del SDK de Voz y el entorno de desarrollo integrado de Java de Eclipse (v4) en Windows de 64 bits. Se ejecuta en un entorno de tiempo de ejecución de Java 8 (JRE) de 64 bits.

### <a name="linux"></a>Linux

En el caso de Linux, la aplicación de ejemplo se proporciona como aplicación Java multiplataforma. Descargue la última versión del [SDK para dispositivos de voz de JRE](https://aka.ms/sdsdk-download-JRE).
La aplicación se crea con el paquete del SDK de Voz y el entorno de desarrollo integrado de Java de Eclipse (v4) en Linux de 64 bits (Ubuntu 16.04, Ubuntu 18.04 y Debian 9). Se ejecuta en un entorno de tiempo de ejecución de Java 8 (JRE) de 64 bits.

Se proporcionan archivos binarios adicionales para ofrecer compatibilidad con dispositivos venideros: [Roobo V2 DDK](https://aka.ms/sdsdk-download-roobov2)y [Urbetter DDK](https://aka.ms/sdsdk-download-urbetter).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Elija el dispositivo de voz](get-speech-devices-sdk.md)
> [!div class="nextstepaction"]
> [Obtenga una clave de suscripción gratuita a los servicios de Voz](get-started.md)
