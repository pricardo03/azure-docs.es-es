---
title: 'Transcripción de conversaciones: Servicios de voz'
titleSuffix: Azure Cognitive Services
description: Transcripción de conversaciones es una característica avanzada de Servicios de voz que combina el reconocimiento de voz en tiempo real, la identificación del hablante y el registro en diarios. Transcripción de conversaciones es una característica perfecta para transcribir reuniones en persona, con la capacidad de distinguir los hablantes, que le permite saber quién ha dicho qué y cuándo, lo que permite a los participantes centrarse en la reunión y realizar el seguimiento de los pasos siguientes rápidamente. Esta característica también mejora la accesibilidad. Con la transcripción, puede hacer participar activamente a personas con dificultades auditivas.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: a31921e229a4bbfccd6fdd871666aad1eeef3232
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66243875"
---
# <a name="what-is-conversation-transcription"></a>¿Qué es Transcripción de conversaciones?

Transcripción de conversaciones es una característica avanzada de Servicios de voz que combina el reconocimiento de voz en tiempo real, la identificación del hablante y el registro en diarios. Transcripción de conversaciones es una característica perfecta para transcribir reuniones en persona, con la capacidad de distinguir los hablantes, que le permite saber quién ha dicho qué y cuándo, lo que permite a los participantes centrarse en la reunión y realizar el seguimiento de los pasos siguientes rápidamente. Esta característica también mejora la accesibilidad. Con la transcripción, puede hacer participar activamente a personas con dificultades auditivas.   

Transcripción de conversaciones proporciona un reconocimiento preciso con modelos de voz personalizables que puede adaptar para comprender el vocabulario específico de la empresa y del sector. Además, puede emparejar la Transcripción de conversaciones con Speech Devices SDK para optimizar la experiencia para dispositivos con varios micrófonos.

>[!NOTE]
> Actualmente, se recomienda Transcripción de conversaciones para pequeñas reuniones. Si desea ampliar Transcripción de conversaciones para reuniones de gran tamaño a escala, póngase en contacto con nosotros.

Este diagrama ilustra el hardware, el software y los servicios que funcionan con Transcripción de conversaciones.

![Diagrama de importación de Transcripción de conversaciones](media/scenarios/conversation-transcription-service.png)

>[!IMPORTANT]
> Se requiere una matriz circular de siete micrófonos con configuración de geometría específica. Para obtener detalles de la especificación y el diseño, consulte las [recomendaciones de matriz de micrófonos de Microsoft Speech Devices SDK](https://aka.ms/cts/microphone). Para más información o comprar un kit de desarrollo, consulte cómo [obtener Microsoft Speech Devices SDK](https://aka.ms/cts/getsdk).

## <a name="get-started-with-conversation-transcription"></a>Introducción a Transcripción de conversaciones

Hay tres pasos que debe seguir para empezar a trabajar con la Transcripción de conversaciones.

1. Recopilar muestras de voz de los usuarios.
2. Generar perfiles de usuario con las muestras de voz del usuario.
3. Usar el SDK de voz para identificar a los usuarios (hablantes) y transcribir la voz.

## <a name="collect-user-voice-samples"></a>Recopilación de muestras de voz de los usuarios

El primer paso es recopilar grabaciones de audio de cada usuario. La voz del usuario se debe grabar en un entorno tranquilo sin ruido de fondo. La longitud recomendada para cada muestra de audio está entre 30 segundos y dos minutos. Las muestras de audio más largas producirán una mayor precisión al identificar a los hablantes. El audio debe ser un canal mono con una tasa de muestreo de 16 KHz.

Más allá de la guía mencionada anteriormente, cómo se registra y almacena el audio depende de usted, aunque se recomienda una base de datos segura. En la siguiente sección, revisaremos cómo se utiliza este audio para generar perfiles de usuario que se usan con el SDK de voz para reconocer a los hablantes.

## <a name="generate-user-profiles"></a>Generación de perfiles de usuario

A continuación, deberá enviar las grabaciones de audio que ha recopilado al Servicio de generación de firmas para validar el audio y generar perfiles de usuario. El [Servicio de generación de firmas](https://aka.ms/cts/signaturegenservice) es un conjunto de API REST que permite generar y recuperar perfiles de usuario.

Para crear un perfil de usuario, debe usar la API `GenerateVoiceSignature`. Hay disponibles detalles de la especificación y código de ejemplo:

> [!NOTE]
> Transcripción de conversaciones solo está disponible actualmente para "en-US" y "zh-CN" en las siguientes regiones: `centralus` y `eastasia`.

* [Especificación REST](https://aka.ms/cts/signaturegenservice)
* [Uso de Transcripción de conversaciones](https://aka.ms/cts/howto)

## <a name="transcribe-and-identify-speakers"></a>Transcripción e identificación de hablantes

Transcripción de conversaciones espera secuencias de audio multicanal y perfiles de usuario como entradas para generar transcripciones e identificar a los hablantes. Los datos de audio y del perfil de usuario se envían al servicio Transcripción de conversaciones mediante Speech Devices SDK. Como se mencionó anteriormente, se requiere una matriz circular de siete micrófonos y Speech Devices SDK para usar Transcripción de conversaciones.

>[!NOTE]
> Para obtener detalles de la especificación y el diseño, consulte las [recomendaciones de matriz de micrófonos de Microsoft Speech Devices SDK](https://aka.ms/cts/microphone). Para más información o comprar un kit de desarrollo, consulte cómo [obtener Microsoft Speech Devices SDK](https://aka.ms/cts/getsdk).

Para obtener información sobre cómo usar Transcripción de conversaciones con Speech Devices SDK, consulte [Cómo usar Transcripción de conversaciones](https://aka.ms/cts/howto).


## <a name="quick-start-with-a-sample-app"></a>Inicio rápido con una aplicación de ejemplo

Microsoft Speech Devices SDK incluye una aplicación de ejemplo de inicio rápido para todos los ejemplos relacionados con dispositivos. Transcripción de conversaciones es uno de ellos. Puede encontrarlo en el [inicio rápido de Speech Device SDK para Android](https://aka.ms/sdsdk-quickstart) con la aplicación de ejemplo y el código fuente para su referencia.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Conozca Speech Devices SDK](speech-devices-sdk.md).
