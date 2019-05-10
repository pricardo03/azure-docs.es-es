---
title: Personalizado voz primero virtuales asistentes (versión preliminar) - servicios de voz
titleSuffix: Azure Cognitive Services
description: Información general de las características, funcionalidades y restricciones para los asistentes virtuales de voz en primer lugar personalizados mediante el canal de voz de línea directa en el Bot Framework y la Cognitive Services Speech Software Development Kit (SDK).
services: cognitive-services
author: trrwilson
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: travisw
ms.custom: ''
ms.openlocfilehash: 1344de526564ab623a51eb903951b6a2e866048d
ms.sourcegitcommit: bb85a238f7dbe1ef2b1acf1b6d368d2abdc89f10
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2019
ms.locfileid: "65523489"
---
# <a name="about-custom-voice-first-virtual-assistants-preview"></a>Acerca de vista previa personalizado asistentes virtuales para la voz en primer lugar

Los asistentes virtuales personalizadas que utilizan los servicios de voz de Azure permiten a los desarrolladores crear interfaces de conversación naturales, similares a la humana, para sus aplicaciones y experiencias. El canal de voz Direct Line de Bot Framework mejora estas funcionalidades porque proporciona un punto de entrada coordinado y organizado a un bot compatible que permite la interacción de entrada y salida de voz con baja latencia y alta confiabilidad. Estos bots puede utilizar Microsoft Language Understanding (LUIS) para la interacción del lenguaje natural. Voz de línea directa se tiene acceso a los dispositivos mediante el Kit de desarrollo de Software (SDK) de voz.

   ![Diagrama conceptual del flujo de servicio de orquestación de voz de línea directa](media/voice-first-virtual-assistants/overview.png "flujo el canal de voz")

Voz de línea directa y su funcionalidad asociada para los asistentes virtuales primero de voz personalizados son un complemento ideal para la [solución Ayudante Virtual](https://docs.microsoft.com/azure/bot-service/bot-builder-virtual-assistant-introduction) y [Enterprise Templates](https://docs.microsoft.com/azure/bot-service/bot-builder-virtual-assistant-introduction). Aunque voz de línea directa puede funcionar con cualquier bot compatible, estos recursos ofrecen una línea base reutilizable para modelos para comenzar rápidamente, así como aptitudes auxiliares comunes y conversacionales experiencias de alta calidad.

## <a name="core-features"></a>Características principales

| Categoría | Características |
|----------|----------|
|[Word de reactivación personalizado](speech-devices-sdk-create-kws.md) | Puede permitir que los usuarios iniciar conversaciones con los bots con una palabra clave personalizada, como "¡Hola Contoso". Esta tarea se realiza con un motor de word de reactivación personalizado en el SDK de voz, que puede configurarse con una palabra de reactivación personalizado [que puede generar aquí](speech-devices-sdk-create-kws.md). El canal de voz de línea directa incluye la comprobación de word de activación del servicio que mejora la precisión de la activación de word de reactivación en comparación con el dispositivo por sí solo.
|[Voz a texto](speech-to-text.md) | El canal de voz de línea directa incluye la transcripción de audio en tiempo real en usando el texto reconocido [texto a voz](speech-to-text.md) desde servicios de voz de Azure. Este texto está disponible para su bot y la aplicación cliente como que se transcribe.
|[Texto a voz](text-to-speech.md) | Respuestas textuales desde su bot se sintetizarlo mediante [texto a voz](text-to-speech.md) desde servicios de voz de Azure. Esta síntesis, a continuación, estará disponible para la aplicación cliente como una secuencia de audio. Microsoft ofrece la capacidad de crear su propia voz TTS neuronal personalizada y de alta calidad que ofrece una voz a su marca, para obtener más información [póngase en contacto con nosotros](mailto:mstts@microsoft.com).
|[Voz de línea directa](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech) | Como un canal dentro de Bot Framework, voz de línea directa permite una conexión sin problemas y sin problemas entre la aplicación cliente, un bot compatible y las capacidades de servicios de voz de Azure. Para obtener más información sobre cómo configurar su bot para utilizar el canal de voz de línea directa, vea [su página en la documentación de Bot Framework](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-directlinespeech).

## <a name="sample-code"></a>Código de ejemplo

Código de ejemplo para crear a un asistente de voz en primer lugar virtual está disponible en GitHub. Estos ejemplos tratan la aplicación cliente para conectarse a su bot en varios lenguajes de programación.

* [Ejemplos de Asistente de voz en primer lugar virtual (SDK)](https://aka.ms/csspeech/samples)
* [Inicio rápido: voz primero asistentes virtuales (C#)](quickstart-virtual-assistant-csharp-uwp.md)
* [Inicio rápido: primer de voz virtuales asistentes (Java)](quickstart-virtual-assistant-java-jre.md)

## <a name="customization"></a>Personalización

Voz primero asistentes virtuales creadas mediante Azure Speech Service pueden usar la amplia gama de opciones de personalización disponibles para [texto a voz](speech-to-text.md), [texto a voz](text-to-speech.md), y [palabra clave personalizada selección](speech-devices-sdk-create-kws.md).

> [!NOTE]
> Opciones de personalización varían según el idioma o configuración regional (vea [idiomas admitidos](supported-languages.md)).

## <a name="reference-docs"></a>Documentos de referencia

* [Speech SDK](speech-sdk-reference.md)
* [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0)

## <a name="next-steps"></a>Pasos siguientes

* [Get a Speech Services subscription key for free](get-started.md) (Consiga una clave de suscripción a los servicios de voz gratis)
* [Obtención de Speech SDK](speech-sdk.md)
* [Creación e implementación de un bot básico](https://docs.microsoft.com/azure/bot-service/bot-builder-tutorial-basic-deploy?view=azure-bot-service-4.0)
* [Obtener la solución Ayudante Virtual y plantilla de la empresa](https://github.com/Microsoft/AI)
