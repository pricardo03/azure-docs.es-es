---
title: Preguntas más frecuentes sobre los asistentes de voz
titleSuffix: Azure Cognitive Services
description: Obtenga respuestas a las preguntas más habituales sobre los asistentes de voz que usan comandos personalizados (versión preliminar) o el canal de Direct Line Speech.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: 7ad3f932e9a10723d6cc1bae2fc4854c932d4c64
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73506531"
---
# <a name="voice-assistants-frequently-asked-questions"></a>Asistentes de voz: Preguntas más frecuentes

Si no encuentra respuestas a sus preguntas en este documento, [consulte otras opciones de soporte técnico](support.md).

## <a name="general"></a>General

**P: ¿Qué es un asistente de voz?**

**R:** Como Cortana, un asistente de voz es una solución que escucha las expresiones habladas de un usuario, analiza el contenido de esas expresiones en busca de significado, realiza una o varias acciones en respuesta a la intención de la expresión y, a continuación, proporciona una respuesta al usuario que a menudo incluye un componente hablado. Se trata de una experiencia de "conversación" para interactuar con un sistema. Los autores del asistente de voz crean una aplicación en el dispositivo mediante `DialogServiceConnector` en el SDK de Voz para comunicarse con un asistente creado mediante [comandos personalizados (versión preliminar)](custom-commands.md) o el canal de [Direct Line Speech](direct-line-speech.md) de Bot Framework. Estos asistentes pueden usar palabras clave personalizadas, conversaciones personalizadas y voz personalizada para proporcionar una experiencia adaptada a su marca o producto.

**P: ¿Debo usar comandos personalizados (versión preliminar) o Direct Line Speech? ¿Cuál es la diferencia?**

**R:** Los [comandos personalizados (versión preliminar)](custom-commands.md) son un conjunto de herramientas de menor complejidad para crear y hospedar fácilmente un asistente adecuado para escenarios de finalización de tareas. [Direct Line Speech](direct-line-speech.md) proporciona capacidades más enriquecidas y sofisticadas para permitir escenarios de conversación sólidos. Para obtener más información, consulte la [comparación de soluciones para asistentes](voice-assistants.md#comparing-assistant-solutions).

**P: ¿Cómo empiezo?**

**R:** La mejor manera de empezar es la creación de una aplicación de comandos personalizados (versión preliminar) o un bot de Bot Framework.

* [Creación de una aplicación de comandos personalizados (versión preliminar)](quickstart-custom-speech-commands-create-new.md)
* [Creación de un bot de Bot Framework básico](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Conexión de un bot al canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech)

## <a name="debugging"></a>Depuración

**P: Se genera un error 401 durante la conexión y nada funciona. Sé que mi clave de suscripción de voz es válida. ¿Qué está ocurriendo?**

**R:** Cuando administre la suscripción en Azure Portal, asegúrese de que usa el recurso **Voz** (Microsoft.CognitiveServicesSpeechServices, "Voz") y *no* el recurso **Cognitive Services** (Microsoft.CognitiveServicesAllInOne, "Todos los servicios de Cognitive Services"). Consulte también [regiones admitidas del servicio de voz para los asistentes de voz](regions.md#voice-assistants).

![Suscripción correcta para Direct Line Speech](media/voice-assistants/faq-supported-subscription.png "ejemplo de una suscripción de voz compatible")

**P: Obtengo el texto de reconocimiento de `DialogServiceConnector`, pero veo un error "1011" y no recibo nada de mi bot. ¿Por qué?**

**R:** Este error indica un problema de comunicación entre su asistente y el servicio del asistente de voz.

* En el caso de los comandos personalizados (versión preliminar), asegúrese de que la aplicación de comandos personalizados (versión preliminar) esté publicada.
* En el caso de Direct Line Speech, asegúrese de que ha [conectado su bot al canal Direct Line Speech](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech), [ha agregado compatibilidad con el protocolo de transmisión](https://aka.ms/botframework/addstreamingprotocolsupport) al bot (con la compatibilidad relacionada con socket web) y, luego, compruebe que su bot responde a las solicitudes entrantes del canal.

**P: Este código todavía no funciona o recibo un error diferente cuando uso `DialogServiceConnector`. ¿qué debo hacer?**

**R:** El registro basado en archivos proporciona bastantes más detalles y puede ayudarle a acelerar las solicitudes de soporte técnico. Para habilitar esta funcionalidad, consulte el [uso del registro de archivos](how-to-use-logging.md).

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas](troubleshooting.md)
* [Notas de la versión](releasenotes.md)
