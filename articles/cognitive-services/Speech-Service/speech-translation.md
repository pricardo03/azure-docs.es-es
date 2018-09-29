---
title: Acerca de la traducción de voz
description: Información general sobre las funcionalidades de Translator Speech
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: f80c0f3cdc114b53c002266820e8d9b8773acc5d
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47432196"
---
# <a name="about-the-speech-translation-api"></a>Introducción a Translator Speech API

Microsoft Speech API le permite agregar a sus aplicaciones, herramientas y dispositivos una traducción de voz de varios idiomas de un extremo a otro en tiempo real. La misma API puede usarse para la traducción de voz a voz y de voz a texto.

Con Microsoft Translator Speech API, las aplicaciones cliente transmiten audio de voz al servicio y reciben de nuevo una secuencia de resultados. Estos resultados incluyen el texto reconocido en el idioma de origen y su traducción en el idioma de destino. Se pueden proporcionar traducciones provisionales hasta que se complete una expresión, momento en el que se proporciona una traducción final.

También puede prepararse una versión de audio sintetizada de la traducción final, habilitando la auténtica traducción de voz a voz.

Translator Speech API utiliza un protocolo WebSocket para proporcionar un canal de comunicación dúplex completo entre el cliente y el servidor. Sin embargo, no es necesario lidiar con WebSocket; el SDK de Speech lo hace por usted.

Translator Speech API emplea las mismas tecnologías que ofrecen varios servicios y productos de Microsoft. Miles de empresas en todo el mundo ya usan este servicio en sus aplicaciones y flujos de trabajo.

## <a name="about-the-technology"></a>Acerca de la tecnología

En el motor de traducción subyacente de Microsoft existen dos enfoques diferentes: traducción automática estadística (SMT) y traducción automática neuronal (NMT). El segundo, un enfoque de inteligencia artificial que utiliza redes neuronales, es el enfoque más moderno para la traducción automática. NMT proporciona mejores traducciones; no es solo más preciso, sino que es más fluido y natural. El motivo principal para esta fluidez es que NMT usa el contexto completo de una oración para traducir palabras.

Actualmente, Microsoft ha migrado a NMT para los idiomas más populares, dejando el uso de SMT solo para los idiomas que se utilizan con menos frecuencia. Todos los [idiomas disponibles para la traducción de voz a voz](language-support.md#speech-translation) cuentan con la tecnología de NMT. La traducción de voz a texto puede utilizar SMT o NMT, según el par de idiomas. Si el idioma de destino es compatible con NMT, la traducción completa ser realiza con dicha tecnología. Si el idioma de destino no es compatible con NMT, la traducción es un híbrido de NMT y SMT, usando el inglés como un "eje" entre los dos idiomas.

Las diferencias entre los modelos son internas para el motor de traducción. Los usuarios finales solo observan la calidad de la traducción mejorada, especialmente en los casos de árabe, chino y japonés.

> [!NOTE]
> ¿Le interesa obtener más información sobre la tecnología del motor de traducción de Microsoft? Consulte [Traducción automática](https://www.microsoft.com/en-us/translator/mt.aspx).

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Conozca cómo traducir voz en C#](how-to-translate-speech-csharp.md)
* [Conozca cómo traducir voz en C++](how-to-translate-speech-cpp.md)
* [Conozca cómo traducir voz en Java](how-to-translate-speech-java.md)
