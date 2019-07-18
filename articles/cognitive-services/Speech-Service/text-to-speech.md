---
title: Texto a voz con los servicios de voz de Azure
titleSuffix: Azure Cognitive Services
description: La opción de texto a voz de los servicios de voz de Azure es un servicio que permite que sus aplicaciones, herramientas o dispositivos conviertan el texto en una voz sintetizada natural similar a la humana. Puede elegir entre voces estándar y neuronales, o puede crear su propia voz personalizada única para su producto o marca. Tiene más de 75 voces estándar disponibles en más de 45 idiomas y configuraciones regionales y 5 voces neuronales que están disponibles en 4 idiomas y configuraciones regionales.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/24/2019
ms.author: erhopf
ms.openlocfilehash: da7259585ad66ac9b58131ce834d82e7b3d4bcf2
ms.sourcegitcommit: c63e5031aed4992d5adf45639addcef07c166224
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67466958"
---
# <a name="what-is-text-to-speech"></a>¿Qué es el texto a voz?

La opción de texto a voz de los servicios de voz de Azure es un servicio que permite que sus aplicaciones, herramientas o dispositivos conviertan el texto en una voz sintetizada natural similar a la humana. Puede elegir entre voces estándar y neuronales, o puede crear su propia voz personalizada única para su producto o marca. Tiene más de 75 voces estándar disponibles en más de 45 idiomas y configuraciones regionales y 5 voces neuronales que están disponibles en 4 idiomas y configuraciones regionales. Para ver una lista completa, consulte los [idiomas admitidos](language-support.md#text-to-speech).

La tecnología de texto a voz permite a los creadores de contenido interactuar con sus usuarios de diferentes maneras. La conversión de texto a voz puede mejorar la accesibilidad al ofrecer a los usuarios la opción de interactuar con el contenido de manera audible. Si el usuario tiene dificultades visuales, problemas de aprendizaje o requiere información de navegación mientras conduce, el texto a voz puede mejorar una experiencia ya existente. Asimismo, el texto a voz también es un complemento valioso para los robots de voz y los asistentes virtuales.


Al aprovechar el lenguaje de marcado de síntesis de voz (SSML), que es un lenguaje de marcado basado en XML, los desarrolladores que usan el servicio de texto a voz pueden especificar cómo se convierte el texto de entrada en una voz sintetizada. Con SSML, puede ajustar el tono, la pronunciación, la velocidad del habla, el volumen y muchas cosas más. Para obtener más información, consulte [SSML](#speech-synthesis-markup-language-ssml).

### <a name="standard-voices"></a>Voces estándar

Las voces estándar se crean mediante técnicas de Síntesis paramétrica estadística y de Síntesis de concatenación. Estas voces son realmente inteligibles y suenan muy naturales. Puede habilitar fácilmente sus aplicaciones para que hablen en más de 45 idiomas, con una amplia gama de opciones de voz. Estas voces proporcionan una alta precisión de pronunciación, admiten abreviaturas, expanden acrónimos, interpretan la fecha y la hora, son polifónicas y ofrecen muchas cosas más. Use la voz estándar para mejorar la accesibilidad de sus aplicaciones y servicios al permitir que los usuarios interactúen con su contenido de manera audible.

### <a name="neural-voices"></a>Voces neuronales

Las voces neuronales usan redes neuronales profundas para superar los límites de los sistemas tradicionales de texto a voz y así poder hacer coincidir los patrones de acentuación y entonación en el lenguaje hablado y sintetizar las unidades del habla en una voz del equipo. La conversión de texto a voz estándar divide la prosodia en análisis lingüísticos separados y pasos de predicción acústica que se rigen por modelos independientes, lo que puede resultar en una síntesis de voz que se oye amortiguada. La capacidad neuronal se encarga de la predicción de la prosodia y la síntesis de voz simultáneamente, lo que resulta en una voz más fluida y natural.

Las voces neuronales se pueden usar para que las interacciones con los bots de chat y los asistentes virtuales sean más naturales y atractivas, para convertir textos digitales, como los libros electrónicos, en audiolibros y para mejorar los sistemas de navegación de los automóviles. Gracias a su prosodia natural similar a la humana y a la clara articulación de las palabras, las voces neuronales reducen considerablemente la fatiga al escucharlas que suele aparecer cuando los usuarios interactúan con sistemas de inteligencia artificial.

Las voces neuronales admiten diferentes estilos, como el neutral y el alegre. Por ejemplo, la voz Jessa (en-US) puede hablar alegremente, lo que está optimizado para tener una conversación cálida y feliz. Puede ajustar la salida de voz, como el tono, el timbre y la velocidad mediante el [Lenguaje de marcado de síntesis de voz](speech-synthesis-markup.md). Hay una lista completa de voces disponibles en [Idiomas admitidos](language-support.md#text-to-speech).

Para obtener más información sobre los beneficios de las voces neuronales, consulte [Microsoft’s new neural text-to-speech service helps machines speak like people](https://azure.microsoft.com/blog/microsoft-s-new-neural-text-to-speech-service-helps-machines-speak-like-people/) (El nuevo servicio neuronal de conversión de texto a voz de Microsoft ayuda a las máquinas a hablar como personas).

### <a name="custom-voices"></a>Voces personalizadas

La personalización de la voz le permite crear una voz reconocible y única para su marca. Para crear su fuente de voz, haga que un estudio grabe y cargue los scripts asociados como datos de aprendizaje. A continuación, el servicio crea un modelo de voz único ajustado a la grabación. Asimismo, puede usar esta fuente de voz personalizada para sintetizar la voz. Para obtener más información, consulte las [voces personalizadas](how-to-customize-voice-font.md).

## <a name="speech-synthesis-markup-language-ssml"></a>Lenguaje de marcado de síntesis de voz (SSML)

El lenguaje de marcado de síntesis de voz (SSML) es un lenguaje de marcado basado en XML que permite a los desarrolladores especificar cómo se convierte el texto de entrada en una voz sintetizada mediante el servicio de texto a voz. En comparación con el texto sin formato, SSML permite a los desarrolladores ajustar el tono, la pronunciación, la velocidad del habla, el volumen y muchas cosas más en la salida de texto a voz. La puntuación normal, como hacer una pausa después de un punto o usar la entonación correcta cuando una oración termina con un signo de interrogación, se administra automáticamente.

Todas las entradas de texto enviadas al servicio de texto a voz deben estar estructuradas como SSML. Para obtener más información, consulte [Speech Synthesis Markup Language](speech-synthesis-markup.md) (Lenguaje de marcado de síntesis de voz).

### <a name="pricing-note"></a>Nota de precios

Al usar el servicio de texto a voz, se le facturará por cada carácter que se convierte a voz, incluida la puntuación. Si bien el documento SSML en sí no es facturable, los elementos opcionales que se usan para ajustar el modo de convertir el texto a voz, como los fonemas y el tono, se cuentan como caracteres facturables. Aquí tiene una lista de lo que se puede facturar:

* El texto que se ha pasado al servicio de texto a voz en el cuerpo SSML de la solicitud.
* Todas las marcas en el campo de texto del cuerpo de la solicitud que están en formato SSML, excepto las etiquetas `<speak>` y `<voice>`.
* Letras, puntuación, espacios, tabulaciones, marcas y todos los caracteres de espacios en blanco.
* Cada punto de código que se define en Unicode

Para obtener más información, consulte [Precios](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

> [!IMPORTANT]
> Cada carácter en chino, japonés y coreano se cuenta como dos caracteres para la facturación.

## <a name="core-features"></a>Características principales

En esta tabla se enumeran las características principales de texto a voz:

| Caso de uso | SDK | REST |
|----------|-----|------|
| Convertir texto a voz. | Sí | Sí |
| Carga de conjuntos de datos para la adaptación de voces. | Sin | Sí\* |
| Creación y administración de modelos de fuentes de voz. | Sin | Sí\* |
| Creación y administración de implementaciones de fuentes de voz. | Sin | Sí\* |
| Creación y administración de pruebas de fuentes de voz. | Sin | Sí\* |
| Administración de suscripciones. | Sin | Sí\* |

\**Estos servicios están disponibles al usar el punto de conexión cris.ai. Consulte la [referencia de Swagger](https://westus.cris.ai/swagger/ui/index). Estas API de administración y aprendizaje de voz personalizadas implementan una limitación que permite 25 solicitudes por 5 segundos, mientras que la API de síntesis de voz implementa una limitación que permite 200 solicitudes por segundo como el valor más alto. Cuando se produzca una limitación, recibirá una notificación a través de los encabezados de los mensajes.*

## <a name="get-started-with-text-to-speech"></a>Empezar a trabajar con texto a voz

Le ofrecemos inicios rápidos diseñados para que ejecute el código en menos de 10 minutos. Esta tabla incluye una lista de inicios rápidos de texto a voz ordenados en función del idioma.

### <a name="sdk-quickstarts"></a>Guías de inicio rápido de SDK

| Inicio rápido (SDK) | Plataforma | Referencia de API |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-text-to-speech-dotnetcore.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](quickstart-text-to-speech-dotnet-windows.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](quickstart-text-to-speech-csharp-uwp.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, Unity](quickstart-text-to-speech-csharp-unity.md) | Windows, Android | [Browse](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-text-to-speech-cpp-windows.md) | Windows | [Browse](https://aka.ms/csspeech/cppref) |
| [C++](quickstart-text-to-speech-cpp-linux.md) | Linux | [Browse](https://aka.ms/csspeech/cppref) |

### <a name="rest-quickstarts"></a>Inicios rápidos de REST

| Inicio rápido (REST) | Plataforma | Referencia de API |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-dotnet-text-to-speech.md) | Windows, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Node.js](quickstart-nodejs-text-to-speech.md) | Window, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Python](quickstart-python-text-to-speech.md) | Window, macOS, Linux | [Browse](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |

## <a name="sample-code"></a>Código de ejemplo

El ejemplo de código para texto a voz está disponible en GitHub. Estos ejemplos tratan la conversión de texto a voz en los lenguajes de programación más populares.

* [Ejemplos de conversión de texto a voz (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
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
