---
title: Preguntas más frecuentes acerca de la voz de línea directa
titleSuffix: Azure Cognitive Services
description: Obtenga respuestas a las preguntas más populares sobre asistentes virtuales voz primero mediante el canal de voz de línea directa.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: travisw
ms.openlocfilehash: 16e4bcbb1514cfd5bbddc22b663d636292095231
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025901"
---
# <a name="voice-first-virtual-assistants-preview-frequently-asked-questions"></a>Voz primero virtuales asistentes para la versión preliminar: Preguntas más frecuentes

Si no se puede encontrar respuestas a sus preguntas en este documento, visite [otras opciones de soporte técnico](support.md).

## <a name="general"></a>General

**P: ¿Qué es la voz de línea directa?**

**R:** El `SpeechBotConnector` de Speech SDK proporciona bidireccional, la comunicación asincrónica a los bots que están conectados al canal de voz de línea directa en Framework Bot. Este canal proporciona acceso coordinado a texto a voz y texto a voz de los servicios de voz de Azure que permiten bots para llegar a ser totalmente voz en, experiencias de conversación de voz. Con la compatibilidad con las palabras de reactivación y reactivar la verificación de Word, esta solución permite crear asistentes virtuales voz primero altamente personalizables para su marca o un producto.

**P: ¿Cómo empiezo?**

**R:** La mejor manera que empiece por crear un asistente de voz en primer lugar virtual es comenzar con [creación de un bot de Bot Framework básico](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0). A continuación, conectar el bot a la [canal de voz de línea directa](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech.md).

## <a name="debugging"></a>Depuración

**P: Se produce un error 401 cuando se conecta y nada funciona. Sé que mi clave de suscripción de voz es válida. ¿Qué pasa?**

**R:** En vista previa, voz de línea directa tiene limitaciones muy específicas en la suscripción utilizada. Asegúrese de que está usando el **voz** recurso (Microsoft.CognitiveServicesSpeechServices, "Voz") y *no* el **Cognitive Services** recurso ( Microsoft.CognitiveServicesAllInOne, "Todos los servicios cognitivos"). Además, tenga en cuenta que solo el **westus2** región se admite actualmente.

![Corrija la suscripción para la voz de línea directa](media/voice-first-virtual-assistants/faq-supported-subscription.png "ejemplo de una suscripción de voz compatible")

**P: Obtengo el texto de reconocimiento de voz de línea directa, pero veo un error de '1011' y nada desde mi bot. ¿Por qué?**

**R:** Este error indica un problema de comunicación entre el bot y voz de línea directa. Asegúrese de que ha [conectado el canal de voz de línea directa](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech.md), [agrega compatibilidad con protocolos de Streaming](https://aka.ms/botframework/addstreamingprotocolsupport) al bot (con el soporte de Socket Web relacionada) y, a continuación, compruebe que su bot responde a las solicitudes entrantes solicitudes desde el canal.

**P: Esto sigue sin funciona u obtengo un error diferente cuando se usa un SpeechBotConnector y no está claro qué debo hacer. ¿Qué *debe* hacer?**

**R:** Basado en archivos de registro proporciona considerablemente más detalles y puede ayudar a acelerar las solicitudes de soporte técnico. Para habilitar esto, consulte [cómo usar el registro de archivo](how-to-use-logging.md).

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas](troubleshooting.md)
* [Notas de la versión](releasenotes.md)
