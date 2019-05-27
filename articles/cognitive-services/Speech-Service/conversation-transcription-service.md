---
title: Transcripción de la conversación - servicios de voz
titleSuffix: Azure Cognitive Services
description: Transcripción de conversación es una característica avanzada de los servicios de voz que combina el reconocimiento de voz en tiempo real, la identificación del hablante y diarization. Transcripción de conversación es perfecta para transcribir reuniones en persona, con la capacidad para distinguir los altavoces, le permite saber quién ha dicho qué y cuándo, los participantes que permite centrarse en la reunión y rápidamente realizar el seguimiento de los pasos siguientes. Esta característica también mejora la accesibilidad. Con la transcripción, puede participar activamente los participantes con dificultades auditivas.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: a31921e229a4bbfccd6fdd871666aad1eeef3232
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66243875"
---
# <a name="what-is-conversation-transcription"></a>¿Qué es la transcripción de la conversación?

Transcripción de conversación es una característica avanzada de los servicios de voz que combina el reconocimiento de voz en tiempo real, la identificación del hablante y diarization. Transcripción de conversación es perfecta para transcribir reuniones en persona, con la capacidad para distinguir los altavoces, le permite saber quién ha dicho qué y cuándo, los participantes que permite centrarse en la reunión y rápidamente realizar el seguimiento de los pasos siguientes. Esta característica también mejora la accesibilidad. Con la transcripción, puede participar activamente los participantes con dificultades auditivas.   

Transcripción de conversación proporciona reconocimiento preciso con modelos de voz personalizables que puede adaptar para comprender el vocabulario específicos de la empresa y del sector. Además, puede emparejar la transcripción de la conversación con el SDK de dispositivos de voz para optimizar la experiencia de micrófonos varios dispositivos.

>[!NOTE]
> Actualmente, se recomienda la transcripción de conversación para pequeñas reuniones. Si desea ampliar la transcripción de conversación para las reuniones de gran tamaño a escala, póngase en contacto con nosotros.

Este diagrama ilustra el hardware, software y servicios que funcionan junto con la transcripción de la conversación.

![El diagrama de transcripción de conversación de importación](media/scenarios/conversation-transcription-service.png)

>[!IMPORTANT]
> Se requiere una matriz de siete micrófono circular con la configuración de objeto geometry específico. Para obtener detalles de especificación y diseño, vea [micrófono de SDK de dispositivo de Microsoft Speech](https://aka.ms/cts/microphone). Para obtener más información o comprar un kit de desarrollo, consulte [obtener el SDK de dispositivo de Microsoft Speech](https://aka.ms/cts/getsdk).

## <a name="get-started-with-conversation-transcription"></a>Introducción a la transcripción de conversación

Hay tres pasos que debe seguir para empezar a trabajar con la transcripción de la conversación.

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
> Transcripción de la conversación está actualmente disponible en "en-US" y "zh-CN" en las siguientes regiones: `centralus` y `eastasia`.

* [Especificación de REST](https://aka.ms/cts/signaturegenservice)
* [Uso de transcripción de conversación](https://aka.ms/cts/howto)

## <a name="transcribe-and-identify-speakers"></a>Transcribir e identificar oradores

Transcripción de conversación espera secuencias de audio multicanal y perfiles de usuario como entradas para generar transcripciones e identificar los altavoces. Datos de perfil de usuario y de audio se envían al servicio de transcripción de la conversación con el SDK de dispositivos de voz. Como se mencionó anteriormente, una matriz de siete micrófono circular y el SDK de dispositivos de voz deben usar la transcripción de la conversación.

>[!NOTE]
> Para obtener detalles de especificación y diseño, vea [micrófono de SDK de dispositivo de Microsoft Speech](https://aka.ms/cts/microphone). Para obtener más información o comprar un kit de desarrollo, consulte [obtener el SDK de dispositivo de Microsoft Speech](https://aka.ms/cts/getsdk).

Para obtener información sobre cómo usar la transcripción de la conversación con el SDK de dispositivos de voz, consulte [cómo usar la transcripción de conversación](https://aka.ms/cts/howto).


## <a name="quick-start-with-a-sample-app"></a>Inicio rápido de una aplicación de ejemplo

SDK de dispositivo de voz de Microsoft tiene una aplicación de ejemplo de inicio rápido para todos los ejemplos relacionados con el dispositivo. Transcripción de conversación es uno de ellos. Puede encontrarlo en [inicio rápido de android SDK de dispositivo de voz](https://aka.ms/sdsdk-quickstart) con la aplicación de ejemplo y su código fuente para su referencia.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Más información sobre el SDK de dispositivos de voz](speech-devices-sdk.md)
