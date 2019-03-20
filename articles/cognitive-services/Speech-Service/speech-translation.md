---
title: Traducción de voz con servicios de voz de Azure
titlesuffix: Azure Cognitive Services
description: Los servicios de voz le permite agregar-to-end, en tiempo real, varios idioma de traducción de voz a sus aplicaciones, herramientas y los dispositivos. La misma API puede usarse para la traducción de voz a voz y de voz a texto.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 95682612b4b0fdb1baa5038039630e74abddb1a9
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57890482"
---
# <a name="what-is-speech-translation"></a>¿Qué es la traducción de voz?

Traducción de voz de servicios de voz de Azure, habilita la traducción de voz para voz y texto a voz en tiempo real, varios idioma de secuencias de audio. Con el SDK de voz, sus aplicaciones, herramientas y los dispositivos tienen acceso a las transcripciones de origen y las salidas de traducción de audio proporcionado. Se devuelven resultados provisionales de transcripción y traducción cuando se detecta la voz y los resultados finales se pueden convertir en voz sintetizada.

Motor de traducción de Microsoft cuenta con la tecnología dos enfoques diferentes: traducción automática estadística (SMT) y traducción automática neuronal (NMT). SMT usa análisis estadísticos avanzados para calcular las traducciones posibles procedimientos según el contexto de unas pocas palabras. Con NMT, las redes neuronales se utilizan para proporcionar las traducciones más precisas y natural mediante el contexto completo de las oraciones traducir palabras.

En la actualidad, Microsoft utiliza NMT para la traducción a idiomas más populares. Todos los [idiomas disponibles para la traducción de voz a voz](language-support.md#speech-translation) cuentan con la tecnología de NMT. La traducción de voz a texto puede utilizar SMT o NMT, según el par de idiomas. Cuando el idioma de destino es compatible con NMT, la traducción completa es con tecnología NMT. Cuando el idioma de destino no es compatible con NMT, la traducción es un híbrido de NMT y SMT, con el idioma inglés como un "dinámica" entre los dos lenguajes.

## <a name="core-features"></a>Características principales

Estas son las características disponibles a través de las API de REST y Speech SDK:

| Caso de uso | SDK | REST |
|----------|-----|------|
| Traducción de voz a texto con los resultados del reconocimiento. | Sí | Sin  |
| Traducción de voz a voz. | Sí | Sin  |
| Resultados provisionales de reconocimiento y la traducción. | Sí | Sin  |

## <a name="get-started-with-speech-translation"></a>Empezar a trabajar con traducción de voz

Se ofrecen tutoriales rápidos diseñados para tener que ejecutar código en menos de 10 minutos. Esta tabla incluye una lista de tutoriales rápidos de traducción de voz organizadas por lenguaje.

| Guía de inicio rápido | Plataforma | Referencia de API |
|------------|----------|---------------|
| [C#, .NET core](quickstart-translate-speech-dotnetcore-windows.md) |  Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, .NET framework](quickstart-translate-speech-dotnetframework-windows.md) |  Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C#, UWP](quickstart-translate-speech-uwp.md) |  Windows | [Browse](https://aka.ms/csspeech/csharpref) |
| [C++](quickstart-translate-speech-cpp-windows.md) |  Windows | [Browse](https://aka.ms/csspeech/cppref)|
| [Java](quickstart-translate-speech-java-jre.md) |  Windows | [Browse](https://aka.ms/csspeech/javaref) |

## <a name="sample-code"></a>Código de ejemplo

Código de ejemplo de Speech SDK está disponible en GitHub. Estos ejemplos tratan escenarios comunes como leer audio desde un archivo o flujo, continua y solo captura reconocimiento/traducción y trabajar con modelos personalizados.

* [Ejemplos de voz a texto y traducción (SDK)](https://github.com/Azure-Samples/cognitive-services-speech-sdk)

## <a name="migration-guides"></a>Guías de migración

> [!WARNING]
> Translator Speech se retirará el 15 de octubre de 2019.

Si sus aplicaciones, herramientas o productos usa Translator Speech, hemos creado las guías para ayudarle a migrar a los servicios de voz.

* [Migrar de Translator Speech API a los servicios de voz](how-to-migrate-from-translator-speech-api.md)

## <a name="reference-docs"></a>Documentos de referencia

* [Speech SDK](speech-sdk-reference.md)
* [Speech Devices SDK](speech-devices-sdk.md)
* [API REST: Speech-to-text](rest-speech-to-text.md)
* [API REST: Text-to-speech](rest-text-to-speech.md)
* [API REST: Personalización y transcripción de lote](https://westus.cris.ai/swagger/ui/index)

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga una clave de suscripción de servicios de voz de forma gratuita](get-started.md)
* [Obtención de Speech SDK](speech-sdk.md)
