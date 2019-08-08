---
title: Traducción de voz con el servicio de Voz
titleSuffix: Azure Cognitive Services
description: El servicio de Voz le permite agregar a sus aplicaciones, herramientas y dispositivos una traducción de voz completa, de varios idiomas y en tiempo real. La misma API puede usarse para la traducción de voz a voz y de voz a texto.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: cfcefd0b18831163324519b61dbea305f90f44bc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552655"
---
# <a name="what-is-speech-translation"></a>¿Qué es la traducción de voz?

La traducción de voz de Servicios de voz de Azure permite la traducción voz a voz y voz a texto de secuencias de audio en varios idiomas en tiempo real. Con el SDK de voz, sus aplicaciones, herramientas y los dispositivos tienen acceso a las transcripciones de origen y a las salidas de traducción del audio proporcionadas. Se devuelven resultados provisionales de transcripción y traducción cuando se detecta la voz y los resultados finales se pueden convertir en voz sintetizada.

El motor de traducción de Microsoft usa tecnología de dos enfoques diferentes: traducción automática estadística (SMT) y traducción automática neuronal (NMT). SMT usa análisis estadísticos avanzados para estimar las mejores traducciones posibles dado el contexto de unas pocas palabras. Con NMT, las redes neuronales se utilizan para proporcionar traducciones más precisas y naturales mediante el contexto completo de las oraciones para traducir palabras.

En la actualidad, Microsoft utiliza NMT para la traducción a los idiomas más populares. Todos los [idiomas disponibles para la traducción de voz a voz](language-support.md#speech-translation) cuentan con la tecnología de NMT. La traducción de voz a texto puede utilizar SMT o NMT, según el par de idiomas. Si el idioma de destino admite NMT, la traducción completa se realiza con NMT. Si el idioma de destino no admite NMT, la traducción es un híbrido de NMT y SMT, con el idioma inglés como "enlace" entre los dos idiomas.

## <a name="core-features"></a>Características principales

Estas son las características disponibles en el SDK de voz y las API REST de Speech Services:

| Caso de uso | SDK | REST |
|----------|-----|------|
| Traducción de voz a texto con resultados de reconocimiento. | Sí | Sin |
| Traducción de voz a voz. | Sí | Sin |
| Resultados de reconocimiento y traducción provisionales. | Sí | Sin |

## <a name="get-started-with-speech-translation"></a>Introducción a la traducción de voz

Le ofrecemos inicios rápidos diseñados para que ejecute el código en menos de 10 minutos. Esta tabla incluye una lista de inicios rápidos de traducción de voz ordenados por idioma.

| Guía de inicio rápido | Plataforma | Referencia de API |
|------------|----------|---------------|
| [C#, .NET Core](quickstart-translate-speech-dotnetcore-windows.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, .NET Framework](quickstart-translate-speech-dotnetframework-windows.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](quickstart-translate-speech-uwp.md) | Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-translate-speech-cpp-windows.md) | Windows | [Browse](https://aka.ms/csspeech/cppref)|
| [Java](quickstart-translate-speech-java-jre.md) | Windows, Linux, macOS | [Browse](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Código de ejemplo

Hay un ejemplo de código para el SDK de voz disponible en GitHub. En estos ejemplos se tratan escenarios comunes como la lectura de audio de un archivo o secuencia, el reconocimiento o traducción continuos y de una sola emisión, y el trabajo con modelos personalizados.

* [Ejemplos de conversión de voz a texto y de traducción (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>Guías de migración

Si sus aplicaciones, herramientas o productos usan [Translator Speech API](https://docs.microsoft.com/azure/cognitive-services/translator-speech/overview), hemos creado guías que le ayudarán a migrar a los servicios de Voz.

* [Migrar de Translator Speech API a Servicios de voz](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>Documentos de referencia

* [Speech SDK](speech-sdk-reference.md)
* [Speech Devices SDK](speech-devices-sdk.md)
* [API REST: Speech-to-text](rest-speech-to-text.md) (API de REST: Voz a texto)
* [API REST: Text-to-speech](rest-text-to-speech.md) (API de REST: Texto a voz)
* [API REST: Batch transcription and customization](https://westus.cris.ai/swagger/ui/index) (API de REST: Transcripción y personalización de Azure Batch)

## <a name="next-steps"></a>Pasos siguientes

* [Get a Speech Services subscription key for free](get-started.md) (Consiga una clave de suscripción a los servicios de voz gratis)
* [Obtención del SDK de voz](speech-sdk.md)
