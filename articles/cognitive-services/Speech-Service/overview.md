---
title: ¿Qué son los servicios de voz de Azure?
titleSuffix: Azure Cognitive Services
description: Los servicios de voz de Azure son la unificación de voz a texto, texto a voz y traducción de voz en una sola suscripción a Azure. Es fácil agregar voz a sus aplicaciones, herramientas y dispositivos con el SDK de voz, el SDK de dispositivos de voz o las API de REST. Agregue la funcionalidad de voz a un bot de chat existente, convierta texto a voz en una aplicación de traducción o transcriba grandes volúmenes de datos del centro de llamadas.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: overview
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 2e82e033e56af10bd8e676570e9409941753eb38
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65020984"
---
# <a name="what-are-the-speech-services"></a>¿Qué son los servicios Voz?

Los servicios de voz de Azure son la unificación de voz a texto, texto a voz y traducción de voz en una sola suscripción a Azure. Es fácil habilitar voz en sus aplicaciones, herramientas y dispositivos con el [SDK de voz](speech-sdk-reference.md), el [SDK de dispositivos de voz](https://aka.ms/sdsdk-quickstart) o las [API de REST](rest-apis.md).

> [!IMPORTANT]
> Los servicios de voz han reemplazado Bing Speech API, Translator Speech y Custom Speech. Consulte *Guías de procedimientos > Migración* para obtener instrucciones de migración.

Estas características conforman los servicios de voz de Azure. Use los vínculos en esta tabla para obtener más información sobre los casos de uso comunes para cada característica o examinar la referencia de API.

| Servicio | Característica | DESCRIPCIÓN | SDK | REST |
|---------|---------|-------------|-----|------|
| [Voz a texto](speech-to-text.md) | Voz a texto | Voz a texto transcribe secuencias de audio a texto en tiempo real que sus aplicaciones, herramientas o dispositivos pueden usar o mostrar. Use voz a texto con [Language Understanding (LUIS)](https://docs.microsoft.com/azure/cognitive-services/luis/) para derivar las intenciones del usuario a partir de voz transcrita y actuar en los comandos de voz. | [Sí](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Sí](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Batch Transcription](batch-transcription.md) (Transcripción de Azure Batch) | La transcripción de Azure Batch permite la transcripción de voz a texto asincrónica de grandes volúmenes de datos. Este es un servicio basado en REST, que usa el mismo punto de conexión que la personalización y la administración de modelos. | Sin  | [Sí](https://westus.cris.ai/swagger/ui/index) |
| | [Servicio de transcripción de conversaciones](conversation-transcription-service.md) | Permite el reconocimiento de voz en tiempo real, la identificación del hablante y la diarización. Es perfecto para transcribir reuniones en persona con la capacidad de distinguir a los oradores. | Sí | Sin  |
| | [Personalización](#customize-your-speech-experience) | Si usa voz a texto para el reconocimiento y la transcripción en un entorno único, puede crear y entrenar modelos acústicos, de lenguaje y pronunciación personalizados para dirigir el sonido ambiental o vocabulario específico del sector. | Sin  | [Sí](https://westus.cris.ai/swagger/ui/index) |
| [Texto a voz](text-to-speech.md) | Texto a voz | Texto a voz convierte el texto de entrada en voz sintetizada similar a la humana. Elija entre voces estándar y voces neuronales (consulte [Compatibilidad de idioma](language-support.md)). | [Sí](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | [Sí](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| | [Personalización](#customize-your-speech-experience) | Cree fuentes de voz personalizadas únicas para su marca o producto. | Sin  | [Sí](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis) |
| [Traducción de voz](speech-translation.md) | Traducción de voz | La traducción de voz habilita la traducción de voz en varios idiomas en tiempo real en sus aplicaciones, herramientas y dispositivos. Use este servicio para la traducción de voz a voz y voz a texto. | [Sí](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) | Sin  |

## <a name="news-and-updates"></a>Noticias y actualizaciones

Obtenga información sobre las novedades con los servicios de voz de Azure.

* Mayo de 2019 - Speech SDK versión 1.5.0 publicado. Para obtener una lista completa de actualizaciones, mejoras y problemas conocidos, consulte las [Notas de la versión](releasenotes.md).
* Abril de 2019: publicación del SDK de Voz 1.4.0 con compatibilidad con conversión de texto a voz (Beta) para C++, C# y Java en Windows y Linux. Además, el SDK ahora admite formatos de audio MP3 y Opus/Ogg para C++ y C# en Linux. Para obtener una lista completa de actualizaciones, mejoras y problemas conocidos, consulte las [Notas de la versión](releasenotes.md).
* Marzo de 2019: está disponible un nuevo punto de conexión para la conversión de texto a voz (TTS) que devuelve una lista completa de las voces disponibles en una región concreta. Además, ahora se admiten regiones nuevas para TTS. Para más información, consulte la [referencia de Text-to-speech API (REST)](rest-text-to-speech.md).
* Febrero de 2019: Speech SDK 1.3.0 publicado con compatibilidad para [Unity (beta)](quickstart-csharp-unity.md). Se agregó compatibilidad para la clase `AudioInput`, que le permite elegir el origen de streaming para audio. Para obtener una lista completa de mejoras y problemas conocidos, consulte [Notas de la versión](releasenotes.md).
* Diciembre de 2018: Speech SDK 1.2.0 publicado con compatibilidad para [Python](quickstart-python.md) y [Node.js](quickstart-js-node.md), así como Ubuntu 18.04 LTS. Para más información, consulte [Notas de la versión](releasenotes.md).

## <a name="try-speech-services"></a>Prueba de los servicios de voz

Ofrecemos guías de inicio rápido en los lenguajes de programación más populares, cuyo diseño individual le permite ejecutar código en menos de 10 minutos. En esta tabla se incluyen las guías de inicio rápido más populares para cada característica. Use el menú de navegación izquierdo para explorar lenguajes y plataformas adicionales.

| Voz a texto (SDK) | Traducción (SDK) | Texto a voz (REST) | Texto a voz (SDK) |
|-------------------|-------------------|-----------------------|-----------------------|
| [C#, .NET Core (Windows)](quickstart-csharp-dotnet-windows.md) | [Java (Windows, Linux)](quickstart-translate-speech-java-jre.md) | [Python (Windows, Linux, macOS)](quickstart-python-text-to-speech.md) | [C#, .NET Framework (Windows)](quickstart-text-to-speech-dotnet-windows.md) |
| [JavaScript (Explorador)](quickstart-js-browser.md) | [C#, .NET Core (Windows)](quickstart-translate-speech-dotnetcore-windows.md) | [C#, .NET Core (Windows, Linux, macOS)](quickstart-dotnet-text-to-speech.md) | [C++ (Windows)](quickstart-text-to-speech-cpp-windows.md) |
| [Python (Windows, Linux, macOS)](quickstart-python.md) | [C#, .NET Framework (Windows)](quickstart-translate-speech-dotnetframework-windows.md) | [Node.js (Windows, Linux, macOS)](quickstart-nodejs-text-to-speech.md) | [C++ (Linux)](quickstart-text-to-speech-cpp-linux.md) |
| [Java (Windows, Linux)](quickstart-java-jre.md) | [C++ (Windows)](quickstart-translate-speech-cpp-windows.md) | |

Una vez que haya tenido la oportunidad de usar los servicios de voz, pruebe nuestro tutorial, que le enseña a reconocer intenciones a partir de contenido de voz mediante el SDK de voz y LUIS.

* [Tutorial: Reconocimiento de intenciones a partir de contenido de voz con el SDK de voz y LUIS, C#](how-to-recognize-intents-from-speech-csharp.md)

## <a name="get-sample-code"></a>Obtención de código de ejemplo

El código de ejemplo está disponible en GitHub para cada uno de los servicios de voz de Azure. En estos ejemplos se tratan escenarios comunes como la lectura de audio de un archivo o flujo, el reconocimiento continuo y de una sola emisión, y el trabajo con modelos personalizados. Use estos vínculos para ver ejemplos de SDK y REST:

* [Ejemplos de conversión de voz a texto, texto a voz y traducción de voz (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)
* [Ejemplos de transcripción de Azure Batch (REST)](https://github.com/Azure-Samples/cognitive-services-speech-sdk/tree/master/samples/batch)
* [Ejemplos de texto a voz (REST)](https://github.com/Azure-Samples/Cognitive-Speech-TTS)

## <a name="customize-your-speech-experience"></a>Personalización de su experiencia de voz

Los servicios de voz de Azure funcionan bien con los modelos integrados; sin embargo, es posible que desee personalizar y optimizar más la experiencia para su producto o entorno. Las opciones de personalización abarcan desde la optimización de modelos acústicos a fuentes de voz únicas para su marca. Una vez que haya creado un modelo personalizado, podrá usarlo con cualquiera de los servicios de voz de Azure.

| Speech Service | Modelo | DESCRIPCIÓN |
|----------------|-------|-------------|
| Voz a texto | [Modelo acústico](how-to-customize-acoustic-models.md) | Cree un modelo acústico personalizado para las aplicaciones, herramientas o dispositivos usados en entornos concretos como en un automóvil o en una planta de producción, cada uno con unas condiciones de grabación específicas. Los ejemplos incluyen el habla con acento, ruidos de fondo específicos o el uso de un micrófono específico para la grabación. |
| | [Modelo de lenguaje](how-to-customize-language-model.md) | Cree un modelo de lenguaje personalizado para mejorar la transcripción de gramática y vocabulario específicos del campo, como terminología médica o jerga de TI. |
| | [Modelo de pronunciación](how-to-customize-pronunciation.md) | Con un modelo de pronunciación personalizado, puede definir el formato fonético y mostrar una palabra o un término. Es útil para controlar términos personalizados, como nombres de producto o acrónimos. Basta con un archivo de pronunciación (un archivo .txt simple). |
| Text-to-Speech | [Fuente de voz](how-to-customize-voice-font.md) | Las fuentes de voz personalizadas le permiten crear una voz única y reconocible para su marca. Solo toma una pequeña cantidad de datos para empezar a trabajar. Cuantos más datos proporcione, más natural y similar a la humana sonará su fuente de voz. |

## <a name="reference-docs"></a>Documentos de referencia

* [Speech SDK](speech-sdk-reference.md)
* [Speech Devices SDK](speech-devices-sdk.md)
* [API REST: Speech-to-text](rest-speech-to-text.md) (API de REST: Voz a texto)
* [API REST: Text-to-speech](rest-text-to-speech.md) (API de REST: Texto a voz)
* [API REST: Batch transcription and customization](https://westus.cris.ai/swagger/ui/index) (API de REST: Transcripción y personalización de Azure Batch)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Get a Speech Services subscription key for free](get-started.md) (Consiga una clave de suscripción a los servicios de voz gratis)
