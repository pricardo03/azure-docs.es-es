---
title: 'Direct Line Speech: servicio de voz'
titleSuffix: Azure Cognitive Services
description: Introducción a las características, las funcionalidades y las restricciones de los asistentes de voz mediante Direct Line Speech con el kit de desarrollo de software (SDK) de Voz.
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: travisw
ms.openlocfilehash: cec3131d791d591375fd87d1c080294c9034a815
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74806225"
---
# <a name="about-direct-line-speech"></a>Acerca de Direct Line Speech

[Los asistentes de voz](voice-assistants.md) escuchan a los usuarios y realizan una acción en respuesta, lo que a menudo implica una respuesta. Utilizan [conversión de voz en texto](speech-to-text.md) para transcribir la voz del usuario y, a continuación, realizan una acción en función de su comprensión del lenguaje natural del texto. Esta acción suele incluir una respuesta hablada del asistente generada con la [conversión de texto en voz](text-to-speech.md). Los dispositivos se conectan con los asistentes mediante el objeto `DialogServiceConnector` del SDK de Voz.

**Direct Line Speech** es una solución sólida y completa para crear un asistente de voz flexible y extensible, con la tecnología de Bot Framework y su canal de Direct Line Speech, que está optimizado para la interacción de conversaciones con bots.

Direct Line Speech ofrece los niveles más altos de personalización y sofisticación para los asistentes de voz. Es adecuado para escenarios de conversaciones no estructuradas, naturales o híbridas con la finalización de tareas o el uso de comandos y controles. Este alto grado de flexibilidad implica una mayor complejidad y los escenarios que se limitan a tareas bien definidas mediante la entrada de lenguaje natural quizá prefieran considerar los [comandos personalizados (versión preliminar)](custom-commands.md) para una experiencia de solución simplificada.

## <a name="getting-started-with-direct-line-speech"></a>Introducción a Direct Line Speech

Los primeros pasos para crear un asistente de voz con Direct Line Speech son [obtener una clave de suscripción de voz](get-started.md), crear un nuevo bot asociado a esa suscripción y conectar el bot al canal de Direct Line Speech.

   ![Diagrama conceptual del flujo de servicio de orquestación de Direct Line Speech](media/voice-assistants/overview-directlinespeech.png "El flujo del canal de voz")

Para obtener una guía paso a paso completa sobre la creación de un asistente de voz sencillo mediante Direct Line Speech, consulte [el tutorial para habilitar el bot con voz mediante el SDK de voz y el canal de Direct Line Speech](tutorial-voice-enable-your-bot-speech-sdk.md).

También se ofrecen inicios rápidos diseñados para que ejecute el código en menos de 10 minutos. Esta tabla incluye una lista de inicios rápidos para asistente de voz ordenados por idioma.

| Guía de inicio rápido | Plataforma | Referencia de API |
|------------|----------|---------------|
| C#, UWP | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| Java | Windows, macOS, Linux | [Browse](https://aka.ms/csspeech/javaref) |
| Java | Android | [Browse](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Código de ejemplo

El código de ejemplo para crear un asistente de voz está disponible en GitHub. Estos ejemplos abarcan la aplicación cliente para conectarse al asistente en varios lenguajes de programación conocidos.

* [Ejemplos del asistente de voz (SDK)](https://aka.ms/csspeech/samples)
* [Tutorial: Habilitación del asistente de voz mediante el SDK de Voz, C#](tutorial-voice-enable-your-bot-speech-sdk.md)

## <a name="customization"></a>Personalización

Los asistentes de voz creados mediante el servicio de voz pueden usar la amplia variedad de opciones de personalización disponibles para la [conversión de voz en texto](speech-to-text.md), la [conversión de texto en voz](text-to-speech.md) y la [selección de palabras claves personalizadas](speech-devices-sdk-create-kws.md).

> [!NOTE]
> Las opciones de personalización varían según el idioma o la configuración regional (consulte los [idiomas admitidos](supported-languages.md)).

Direct Line Speech y su funcionalidad asociada para los asistentes de voz constituyen un complemento perfecto para la [solución Virtual Assistant y la plantilla empresarial](https://docs.microsoft.com/azure/bot-service/bot-builder-enterprise-template-overview). Aunque Direct Line Speech puede funcionar con cualquier bot compatible, estos recursos ofrecen una línea de base reutilizable para experiencias conversacionales de alta calidad, así como habilidades y modelos complementarios comunes para comenzar rápidamente.

## <a name="reference-docs"></a>Documentos de referencia

* [Speech SDK](speech-sdk-reference.md)
* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga una clave de suscripción gratuita a los servicios de Voz](get-started.md)
* [Obtención del SDK de voz](speech-sdk.md)
* [Creación e implementación de un bot básico](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Obtención de la solución Virtual Assistant y la plantilla empresarial](https://github.com/Microsoft/AI)
