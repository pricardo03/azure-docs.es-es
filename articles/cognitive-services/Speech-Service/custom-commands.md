---
title: 'Comandos personalizados (versión preliminar): servicio de Voz'
titleSuffix: Azure Cognitive Services
description: Información general sobre las características, funcionalidades y restricciones de los comandos personalizados (versión preliminar), una solución para crear asistentes de voz.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: 62210bf480d09ce2a256a44b7554ac53aa06eb0c
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73579701"
---
# <a name="custom-commands-preview"></a>Comandos personalizados (versión preliminar)

[Los asistentes de voz](voice-assistants.md) escuchan a los usuarios y realizan una acción en respuesta, lo que a menudo implica una respuesta. Utilizan [conversión de voz en texto](speech-to-text.md) para transcribir la voz del usuario y, a continuación, realizan una acción en función de su comprensión del lenguaje natural del texto. Esta acción suele incluir una respuesta hablada del asistente generada con la [conversión de texto en voz](text-to-speech.md). Los dispositivos se conectan con los asistentes mediante el objeto `DialogServiceConnector` del SDK de Voz.

**Comandos personalizados (versión preliminar)** es una solución simplificada para crear un asistente de voz. Proporciona una experiencia de creación unificada, un modelo de hospedaje automático y una complejidad relativamente inferior en comparación con otras opciones de creación de asistentes como [Direct Line Speech](direct-line-speech.md). Sin embargo, esta simplificación conlleva una reducción en la flexibilidad. Por lo tanto, los comandos personalizados (versión preliminar) son más adecuados para escenarios de finalización de tareas o de comando y control. Es especialmente adecuado para los dispositivos de Internet de las cosas (IoT) y sin periféricos.

Para una interacción e integración de conversaciones complejas con otras soluciones como la [solución de Virtual Assistant y la plantilla empresarial](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview), se recomienda usar Direct Line Speech.

Los candidatos adecuados para los comandos personalizados (versión preliminar) tienen un vocabulario fijo con conjuntos de variables bien definidos. Por ejemplo, las tareas de automatización del hogar, como el control de un termostato, son ideales.

   ![Ejemplos de escenarios de finalización de tareas](media/voice-assistants/task-completion-examples.png "ejemplos de finalización de tareas")

## <a name="getting-started-with-custom-commands-preview"></a>Introducción a los comandos personalizados (versión preliminar)

El primer paso para usar comandos personalizados (versión preliminar) para realizar un asistente de voz es [obtener una clave de suscripción de voz](get-started.md) y acceder al generador de comandos personalizados (versión preliminar) en [Speech Studio](https://speech.microsoft.com). Desde allí, puede crear una nueva aplicación de comandos personalizados (versión preliminar) y publicarla, tras lo cual una aplicación en el dispositivo podrá comunicarse con ella mediante el SDK de Voz.

   ![Flujo de creación de comandos personalizados (versión preliminar)](media/voice-assistants/custom-commands-flow.png "Flujo de creación de los comandos personalizados (versión preliminar)")

Le ofrecemos inicios rápidos diseñados para que ejecute el código en menos de 10 minutos.

* [Creación de una aplicación de comandos personalizados (versión preliminar)](quickstart-custom-speech-commands-create-new.md)
* [Creación de una aplicación de comandos personalizados (versión preliminar) con parámetros](quickstart-custom-speech-commands-create-parameters.md)
* [Conexión a una aplicación de comandos personalizados (versión preliminar) con el SDK de Voz, C#](quickstart-custom-speech-commands-speech-sdk.md)

## <a name="sample-code"></a>Código de ejemplo

El código de ejemplo para crear un asistente de voz con los comandos personalizados (versión preliminar) está disponible en GitHub.

* [Ejemplos del asistente de voz (SDK)](https://aka.ms/csspeech/samples)

## <a name="customization"></a>Personalización

Los asistentes de voz creados mediante los servicios de voz de Azure pueden usar la amplia variedad de opciones de personalización disponibles para [conversión de voz en texto](speech-to-text.md), [conversión de texto en voz](text-to-speech.md) y [selección de palabra clave personalizada](speech-devices-sdk-create-kws.md).

> [!NOTE]
> Las opciones de personalización varían según el idioma o la configuración regional (consulte los [idiomas admitidos](supported-languages.md)).

## <a name="reference-docs"></a>Documentos de referencia

* [Speech SDK](speech-sdk-reference.md)

## <a name="next-steps"></a>Pasos siguientes

* [Get a Speech Services subscription key for free](get-started.md) (Consiga una clave de suscripción a los servicios de voz gratis)
* [Obtención del SDK de voz](speech-sdk.md)
