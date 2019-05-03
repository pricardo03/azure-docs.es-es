---
title: Servicio de conversación transcripción - servicios de voz
titleSuffix: Azure Cognitive Services
description: El servicio de transcripción de conversación es una característica avanzada de los servicios de voz que combina el reconocimiento de voz en tiempo real, la identificación del hablante y diarization. Servicio de transcripción de conversación es perfecto para transcribir reuniones en persona, con la capacidad para distinguir los altavoces, le permite saber quién ha dicho qué y cuándo, los participantes que permite centrarse en la reunión y rápidamente realizar el seguimiento de los pasos siguientes. Esta característica también mejora la accesibilidad. Con la transcripción, puede participar activamente los participantes con dificultades auditivas.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 2c3269a9e57c0392e152fedfb640b9fae91a4989
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025940"
---
# <a name="what-is-the-conversation-transcription-service"></a>¿Qué es el servicio de transcripción de la conversación?

El servicio de transcripción de conversación es una característica avanzada de los servicios de voz que combina el reconocimiento de voz en tiempo real, la identificación del hablante y diarization. Servicio de transcripción de conversación es perfecto para transcribir reuniones en persona, con la capacidad para distinguir los altavoces, le permite saber quién ha dicho qué y cuándo, los participantes que permite centrarse en la reunión y rápidamente realizar el seguimiento de los pasos siguientes. Esta característica también mejora la accesibilidad. Con la transcripción, puede participar activamente los participantes con dificultades auditivas.   

Servicio de conversación transcripción ofrece reconocimiento preciso con modelos de voz personalizables que puede adaptar para comprender el vocabulario específicos de la empresa y del sector. Además, puede emparejar el servicio de transcripción de conversación con el SDK de dispositivos de voz para optimizar la experiencia de micrófonos varios dispositivos.

>[!NOTE]
> Actualmente, se recomienda el servicio de transcripción de conversación para pequeñas reuniones. Si desea ampliar el servicio de transcripción de conversación para las reuniones de gran tamaño a escala, póngase en contacto con nosotros.

Este diagrama ilustra el hardware, software y servicios que funcionan junto con el servicio de transcripción de conversación.

![El diagrama de servicio de importación conversación transcripción](media/scenarios/conversation-transcription-service.png)

>[!IMPORTANT]
> Se requiere una matriz de siete micrófono circular con la configuración de objeto geometry específico. Para obtener detalles de especificación y diseño, vea [micrófono de SDK de dispositivo de Microsoft Speech](https://aka.ms/cts/microphone). Para obtener más información o comprar un kit de desarrollo, consulte [obtener el SDK de dispositivo de Microsoft Speech](https://aka.ms/cts/getsdk).

## <a name="get-started-with-conversation-transcription"></a>Introducción a la transcripción de conversación

Hay tres pasos que debe seguir para empezar a trabajar con el servicio de transcripción de conversación.

1. Recopilar ejemplos de voz de los usuarios.
2. Generar perfiles de usuario con los ejemplos de voz del usuario
3. Usar el SDK de voz para identificar a los usuarios (altavoces) y transcribir voz

## <a name="collect-user-voice-samples"></a>Recopilar muestras de voz del usuario

El primer paso es recopilar las grabaciones de audio de cada usuario. Voz del usuario se debe grabar en un entorno tranquilo sin ruido de fondo. La longitud recomendada para cada muestra de sonido es de 30 segundos a dos minutos. Muestras de audio más largo se producirán una mayor precisión al identificar oradores. El audio debe ser un canal mono con una tasa de muestreo de 16.

Más allá de la guía mencionada anteriormente, cómo se registra de audio y almacenados depende de usted, se recomienda una base de datos segura. En la siguiente sección, revisaremos cómo este audio se utiliza para generar perfiles de usuario que se usan con el SDK de voz para reconocer los altavoces.

## <a name="generate-user-profiles"></a>Generar perfiles de usuario

A continuación, deberá enviar las grabaciones de audio que ha recopilado en el servicio de generación de firmas para validar el audio y generar perfiles de usuario. El [servicio de generación de firmas](https://aka.ms/cts/signaturegenservice) es un conjunto de API de REST, que permiten generar y recuperar los perfiles de usuario.

Para crear un perfil de usuario, debe usar el `GenerateVoiceSignature` API. Detalles de especificación y código de ejemplo están disponibles:

> [!NOTE]
> El servicio de transcripción de la conversación está actualmente disponible en las siguientes regiones: `centralus` y `eastasia`.

* [Especificación de REST](https://aka.ms/cts/signaturegenservice)
* [Cómo usar el servicio de conversación transcripción](https://aka.ms/cts/howto)

## <a name="transcribe-and-identify-speakers"></a>Transcribir e identificar oradores

El servicio de transcripción de conversación espera secuencias de audio multicanal y perfiles de usuario como entradas para generar transcripciones e identificar los altavoces. Datos de perfil de usuario y de audio se envían al servicio de transcripción de conversación con el SDK de dispositivos de voz. Como se mencionó anteriormente, una matriz de siete micrófono circular y el SDK de dispositivos de voz deben usar el servicio de transcripción de la conversación.

>[!NOTE]
> Para obtener detalles de especificación y diseño, vea [micrófono de SDK de dispositivo de Microsoft Speech](https://aka.ms/cts/microphone). Para obtener más información o comprar un kit de desarrollo, consulte [obtener el SDK de dispositivo de Microsoft Speech](https://aka.ms/cts/getsdk).

Para obtener información sobre cómo usar el servicio de transcripción de la conversación con el SDK de dispositivos de voz, consulte [cómo usar el servicio de conversación transcripción](https://aka.ms/cts/howto).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre el SDK de dispositivos de voz](speech-devices-sdk.md)
