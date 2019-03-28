---
title: Texto a voz con servicios de voz de Azure
titleSuffix: Azure Cognitive Services
description: Texto a voz de servicios de voz de Azure es un servicio basado en REST que permite a las aplicaciones, herramientas o dispositivos convertir texto en voz sintetizada de natural similar a la humana. Elija entre estándares y neuronales voces, o crear su propia voz personalizados únicos para su producto o marca. 75 voces estándares están disponibles en más de 45 idiomas y configuraciones regionales y 5 voces neuronales están disponibles en 4 idiomas y configuraciones regionales.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/19/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 1edc2587ef8680f60082bf6271b73d30184f331b
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521251"
---
# <a name="what-is-text-to-speech"></a>¿Qué es el texto a voz?

Texto a voz de servicios de voz de Azure es un servicio basado en REST que permite a las aplicaciones, herramientas o dispositivos convertir texto en voz sintetizada de natural similar a la humana. Elija entre estándares y neuronales voces, o crear su propia voz personalizados únicos para su producto o marca. 75 voces estándares están disponibles en más de 45 idiomas y configuraciones regionales y 5 voces neuronales están disponibles en 4 idiomas y configuraciones regionales. Para obtener una lista completa, consulte [idiomas admitidos](language-support.md#text-to-speech).

Tecnología de texto a voz permite que los creadores de contenido interactuar con los usuarios de maneras diferentes. Texto a voz puede mejorar la accesibilidad proporcionando a los usuarios con una opción para interactuar con el contenido de forma audible. Si el usuario tiene dificultades visuales, una discapacidad de aprendizaje, o si requiere información de navegación mientras conduce, una experiencia existente puede mejorar el texto a voz. Texto a voz también es un complemento valioso para los bots de voz y asistentes virtuales.

### <a name="neural-voices"></a>Voces neuronales

Las voces neuronales se pueden usar para que las interacciones con los bots de chat y los asistentes virtuales sean más naturales y atractivas, para convertir textos digitales, como los libros electrónicos, en audiolibros y para mejorar los sistemas de navegación de los automóviles. Con la prosodia natural similar a la humana y desactive articulación de palabras, voces neuronales reducen significativamente fatiga de la escucha al interactuar con sistemas de inteligencia artificial. Para más información acerca de las voces neuronales, consulte los [idiomas compatibles](language-support.md#text-to-speech).

### <a name="custom-voices"></a>Voces personalizadas

Personalización de voz le permite crear una voz reconocible, uno de tipo para su marca. Para crear la fuente de voz personalizados, crear una grabación de studio y cargar los scripts asociados como los datos de entrenamiento. A continuación, el servicio crea un modelo de voz único ajustado a la grabación. Puede usar esta fuente de voz personalizados para la síntesis de voz. Para obtener más información, consulte [voces personalizadas](how-to-customize-voice-font.md).

## <a name="core-features"></a>Características principales

Esta tabla enumeran las características principales de texto a voz:

| Caso de uso | SDK | REST |
|----------|-----|------|
| Convertir texto a voz. | Sin  | Sí |
| Cargar conjuntos de datos para la adaptación de voz. | Sin  | Sí\* |
| Crear y administrar modelos de fuente de voz. | Sin  | Sí\* |
| Crear y administrar implementaciones de la fuente de voz. | Sin  | Sí\* |
| Crear y administrar las pruebas de fuente de voz. | Sin  | Sí\* |
| Administrar suscripciones. | Sin  | Sí\* |

\* *Estos servicios están disponibles mediante el punto de conexión cris.ai. Consulte [Swagger referencia](https://westus.cris.ai/swagger/ui/index).*

> [!NOTE]
> El punto de conexión de texto a voz implementa la limitación que limita las solicitudes a 25 cada 5 segundos. Cuando se produce la limitación, se le notificará a través de los encabezados del mensaje.

## <a name="get-started-with-text-to-speech"></a>Empezar a trabajar con texto a voz

Se ofrecen tutoriales rápidos diseñados para tener que ejecutar código en menos de 10 minutos. Esta tabla incluye una lista de tutoriales rápidos de texto a voz organizadas por lenguaje.

| Guía de inicio rápido | Plataforma | Referencia de API |
|------------|----------|---------------|
| [C#, .NET core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Ventana, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Ventana, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Código de ejemplo

Código de ejemplo de texto a voz está disponible en GitHub. Estos ejemplos tratan la conversión de texto a voz en lenguajes de programación más populares.

* [Ejemplos de texto a voz (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="reference-docs"></a>Documentos de referencia

* [Speech SDK](speech-sdk-reference.md)
* [Speech Devices SDK](speech-devices-sdk.md)
* [API REST: Speech-to-text](rest-speech-to-text.md) (API de REST: Voz a texto)
* [API REST: Text-to-speech](rest-text-to-speech.md) (API de REST: Texto a voz)
* [API REST: Batch transcription and customization](https://westus.cris.ai/swagger/ui/index) (API de REST: Transcripción y personalización de Azure Batch)

## <a name="next-steps"></a>Pasos siguientes

* [Obtención de una suscripción de gratuita a servicios de Voz](get-started.md)
* [Creación de fuentes de voz personalizadas](how-to-customize-voice-font.md)
