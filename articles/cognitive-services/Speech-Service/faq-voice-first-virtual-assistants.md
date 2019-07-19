---
title: Preguntas frecuentes sobre Direct Line Speech
titleSuffix: Azure Cognitive Services
description: Obtenga respuestas a las preguntas más habituales sobre los asistentes virtuales por voz mediante el canal Direct Line Speech.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: travisw
ms.openlocfilehash: 2c669f00ae65667f85976aca218ce51d630159ee
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67465500"
---
# <a name="voice-first-virtual-assistants-preview-frequently-asked-questions"></a>Asistentes virtuales por voz (versión preliminar): Preguntas más frecuentes

Si no encuentra respuestas a sus preguntas en este documento, [consulte otras opciones de soporte técnico](support.md).

## <a name="general"></a>General

**P: ¿Qué es Direct Line Speech?**

**R:** El elemento `DialogServiceConnector` del SDK de voz proporciona comunicación asincrónica bidireccional con los bots que están conectados al canal Direct Line Speech en Bot Framework. Este canal proporciona acceso coordinado a las opciones de conversión de voz a texto y de texto a voz de los servicios de Voz de Azure que permiten que los bots se conviertan en verdaderas experiencias conversacionales de entrada y salida de voz. Gracias a la compatibilidad con las palabras de reactivación y la comprobación de palabras de reactivación, esta solución permite crear asistentes virtuales por voz enormemente personalizables para su marca o producto.

**P: ¿Cómo empiezo?**

**R:** La mejor manera de comenzar a crear un asistente virtual por voz es [crear un bot de Bot Framework básico](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0). A continuación, conecte el bot al [canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech).

## <a name="debugging"></a>Depuración

**P: Se produce un error 401 durante la conexión y nada funciona. Sé que mi clave de suscripción de voz es válida. ¿Qué está ocurriendo?**

**R:** En la versión preliminar, Direct Line Speech tiene limitaciones específicas sobre las suscripciones que puede usar. Asegúrese de que usa el recurso **Voz** (Microsoft.CognitiveServicesSpeechServices, "Voz") y *no* el recurso **Cognitive Services** (Microsoft.CognitiveServicesAllInOne, "Todos los servicios de Cognitive Services"). Actualmente, solo se admite [un subconjunto de las regiones de los servicios de Voz](regions.md#voice-first-virtual-assistants) para Direct Line Speech.

![Suscripción correcta para Direct Line Speech](media/voice-first-virtual-assistants/faq-supported-subscription.png "Ejemplo de una suscripción de Voz compatible")

**P: Obtengo el texto de reconocimiento de Direct Line Speech, pero veo un error "1011" y no recibo nada de mi bot. ¿Por qué?**

**R:** Este error indica un problema de comunicación entre el bot y Direct Line Speech. Asegúrese de que ha [conectado el canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech), [ha agregado compatibilidad con el protocolo de transmisión](https://aka.ms/botframework/addstreamingprotocolsupport) al bot (con la compatibilidad relacionada con socket web) y, luego, compruebe que su bot responde a las solicitudes entrantes del canal.

**P: Este código todavía no funciona o recibo un error diferente cuando uso DialogServiceConnector. ¿qué debo hacer?**

**R:** El registro basado en archivos proporciona bastantes más detalles y puede ayudarle a acelerar las solicitudes de soporte técnico. Para habilitar esta funcionalidad, consulte el [uso del registro de archivos](how-to-use-logging.md).

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas](troubleshooting.md)
* [Notas de la versión](releasenotes.md)
