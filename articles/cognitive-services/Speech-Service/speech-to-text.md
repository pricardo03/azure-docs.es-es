---
title: Voz a texto con Servicios de voz de Azure
titleSuffix: Azure Cognitive Services
description: La conversión de voz a texto de Servicios de voz de Azure, también conocida simplemente como voz a texto, permite la transcripción en tiempo real de secuencias de audio a texto que las aplicaciones, herramientas o dispositivos pueden usar, mostrar o actuar como ante una entrada de comandos. Este servicio funciona con la misma tecnología de reconocimiento que Microsoft utiliza para los productos de Cortana y Office, y funciona sin problemas con la traducción y el servicio de texto a voz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: fcb09720d188150736dbe7da0c925f5762ff6a10
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67072315"
---
# <a name="what-is-speech-to-text"></a>¿Qué es la conversión de voz a texto?

La conversión de voz a texto de Servicios de voz de Azure, también conocida simplemente como voz a texto, permite la transcripción en tiempo real de secuencias de audio a texto que las aplicaciones, herramientas o dispositivos pueden usar, mostrar o actuar como ante una entrada de comandos. Este servicio funciona con la misma tecnología de reconocimiento que Microsoft utiliza para los productos de Cortana y Office, y funciona sin problemas con la traducción y el servicio de texto a voz.  Si desea obtener una lista completa de los idiomas disponibles para la conversión de voz a texto, consulte [Idiomas admitidos](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#speech-to-text).

De forma predeterminada, el servicio de voz a texto utiliza el modelo de lenguaje universal. Este modelo se entrenó con datos propiedad de Microsoft y se implementa en la nube. Resulta óptimo para escenarios de conversación y dictado. Si usa voz a texto para el reconocimiento y la transcripción en un entorno único, puede crear y entrenar modelos acústicos, de lenguaje y pronunciación personalizados para dirigir el sonido ambiental o vocabulario específico del sector. 

Puede capturar audio desde un micrófono fácilmente, leer de una secuencia o acceder a archivos de audio desde el almacenamiento con el SDK de voz y las API REST de Speech Services. El SDK de voz admite WAV/PCM de 16 bits, 16 kHz u 8 kHz y audio de un solo canal para el reconocimiento de voz. Los formatos de audio adicionales se admiten mediante el [punto de conexión de REST de voz a texto](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) o el [servicio de transcripción por lotes](https://docs.microsoft.com/azure/cognitive-services/speech-service/batch-transcription#supported-formats).

## <a name="core-features"></a>Características principales

Estas son las características disponibles en el SDK de voz y las API REST de Speech Services:

| Caso de uso | SDK | REST |
|----------|-----|------|
| Transcripción de expresiones de voz (<15 segundos). Solo es compatible con el resultado final de la transcripción. | Sí | Sí |
| Transcripción continua de expresiones de voz largas y streaming de audio (>15 segundos). Es compatible con resultados de transcripción intermedios y finales. | Sí | Sin |
| Derivación de intenciones a partir de resultados de reconocimiento con [LUIS](https://docs.microsoft.com/azure/cognitive-services/luis/what-is-luis). | Sí | No\* |
| Transcripción por lotes de archivos de audio de forma asincrónica. | Sin | Sí\** |
| Creación y administración de modelos de voz. | Sin | Sí\** |
| Creación y administración de implementaciones de modelos personalizados. | Sin | Sí\** |
| Creación de pruebas de precisión para medir la exactitud de la base de referencia del modelo frente a los modelos personalizados. | Sin | Sí\** |
| Administración de suscripciones. | Sin | Sí\** |

\* *Las entidades e intenciones de LUIS pueden derivarse mediante una suscripción independiente de LUIS. Con esta suscripción, el SDK puede llamar a LUIS y proporcionar resultados de la entidad y la intención. Con la API REST, puede llamar a LUIS usted mismo para deducir las entidades y las intenciones con su suscripción a LUIS.*

\** *Estos servicios están disponibles mediante el punto de conexión cris.ai. Consulte la [referencia de Swagger](https://westus.cris.ai/swagger/ui/index).*

## <a name="get-started-with-speech-to-text"></a>Introducción a la conversión de voz a texto

Ofrecemos guías de inicio rápido en los lenguajes de programación más populares, cuyo diseño individual le permite ejecutar código en menos de 10 minutos. Esta tabla incluye una lista completa de inicios rápidos del SDK de voz ordenados por idioma.

| Guía de inicio rápido | Plataforma | Referencia de API |
|------------|----------|---------------|
| [C#, .NET Core](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-csharp-dotnetcore-windows) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-csharp-dotnet-windows) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-csharp-uwp) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C++](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-cpp-windows) | Windows | [Browse](https://aka.ms/csspeech/cppref)|
| [C++](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-cpp-linux) | Linux | [Browse](https://aka.ms/csspeech/cppref) |
| [Java](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-java-android) | Android | [Browse](https://aka.ms/csspeech/javaref) |
| [Java](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-java-jre) | Windows, Linux, macOS | [Browse](https://aka.ms/csspeech/javaref) |
| [JavaScript, Explorador](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-js-browser) | Browser, Windows, Linux, macOS | [Browse](https://aka.ms/AA434tv) |
| [JavaScript, Node.js](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-js-node) | Windows, Linux, macOS | [Browse](https://aka.ms/AA434tv) |
| [Objective-C](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-objectivec-ios) | iOS | [Browse](https://aka.ms/csspeech/objectivecref) |
| [Python](https://docs.microsoft.com/azure/cognitive-services/speech-service/quickstart-python) | Windows, Linux, macOS | [Browse](https://aka.ms/AA434tr)  |

Si prefiere usar el servicio REST de voz a texto, consulte [API REST](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis).

## <a name="tutorials-and-sample-code"></a>Tutoriales y ejemplo de código

Una vez que haya tenido la oportunidad de usar los servicios de voz, pruebe nuestro tutorial, que le enseña a reconocer intenciones a partir de contenido de voz mediante el SDK de voz y LUIS.

* [Tutorial: Reconocimiento de intenciones a partir de contenido de voz con el SDK de voz y LUIS, C#](how-to-recognize-intents-from-speech-csharp.md)

Hay un ejemplo de código para el SDK de voz disponible en GitHub. En estos ejemplos se tratan escenarios comunes como la lectura de audio de un archivo o flujo, el reconocimiento continuo y de una sola emisión, y el trabajo con modelos personalizados.

* [Ejemplos de conversión de voz a texto (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Ejemplos de transcripción de Azure Batch (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)

## <a name="customization"></a>Personalización

Además del modelo universal que usa Servicios de voz, se pueden crear modelos acústicos, de idiomas y de pronunciación personalizados específicos para su experiencia. Esta es una lista de las opciones de personalización:

| Modelo | DESCRIPCIÓN |
|-------|-------------|
| [Modelo acústico](how-to-customize-acoustic-models.md) | La creación de un modelo acústico personalizado resulta útil si las aplicaciones, herramientas o dispositivos se emplean en un entorno concreto como en un automóvil o en una planta de producción, cada uno con unas condiciones de grabación específicas. Los ejemplos incluyen el habla con acento, ruidos de fondo específicos o el uso de un micrófono específico para la grabación. |
| [Modelo de lenguaje](how-to-customize-language-model.md) | Cree un modelo de lenguaje personalizado para mejorar la transcripción de gramática y vocabulario específicos del sector, como terminología médica o jerga de TI. |
| [Modelo de pronunciación](how-to-customize-pronunciation.md) | Con un modelo de pronunciación personalizado, puede definir el formato fonético y mostrar una palabra o un término. Es útil para controlar términos personalizados, como nombres de producto o acrónimos. Basta con un archivo de pronunciación (un archivo .txt simple). |

> [!NOTE]
> Las opciones de personalización varían según el idioma o la configuración regional (consulte los [idiomas admitidos](supported-languages.md)).

## <a name="migration-guides"></a>Guías de migración

> [!WARNING]
> Bing Speech se retirará el 15 de octubre de 2019.

Si sus aplicaciones, herramientas o productos usan Bing Speech API o Custom Speech, hemos creado guías para que le ayuden a migrar a Servicios de voz.

* [Migración de Bing Speech a Servicios de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-bing-speech)
* [Migración de Custom Speech a Servicios de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-migrate-from-custom-speech-service)

## <a name="reference-docs"></a>Documentos de referencia

* [Speech SDK](speech-sdk-reference.md)
* [Speech Devices SDK](speech-devices-sdk.md)
* [API REST: Speech-to-text](rest-speech-to-text.md) (API de REST: Voz a texto)
* [API REST: Text-to-speech](rest-text-to-speech.md) (API de REST: Texto a voz)
* [API REST: Batch transcription and customization](https://westus.cris.ai/swagger/ui/index) (API de REST: Transcripción y personalización de Azure Batch)

## <a name="next-steps"></a>Pasos siguientes

* [Get a Speech Services subscription key for free](get-started.md) (Consiga una clave de suscripción a los servicios de voz gratis)
* [Obtención del SDK de voz](speech-sdk.md)
