---
title: Acerca de Speech to Text | Microsoft Docs
description: Información general de las funcionalidades de Speech to Text API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 2ed00377db80849a8355ccc895db12d006bea642
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39069934"
---
# <a name="about-the-speech-to-text-api"></a>Acerca de Speech to Text API

**Speech to Text** API *transcribe* secuencias de audio en texto que la aplicación puede mostrar al usuario o actuar como si fuera una entrada de comandos. Las API pueden usarse con una biblioteca cliente del SDK (para las plataformas e idiomas admitidos) o una API REST.

**Speech to Text** API ofrece las siguientes características:

- Tecnología avanzada de reconocimiento de voz de Microsoft (la misma que usan Cortana, Office y otros productos de Microsoft).

- Reconocimiento continuo en tiempo real. **Speech to Text** permite a los usuarios transcribir audio en texto en tiempo real. También admite la recepción de los resultados intermedios de las palabras que reconoció hasta ahora. El servicio reconoce automáticamente el final del habla. Los usuarios también pueden elegir opciones de formato adicionales, incluidos el uso de mayúsculas y signos de puntuación, el enmascaramiento de palabras soeces y la normalización de texto inverso.

- Optimización de los resultados de **Speech to Text** para escenarios interactivos, de conversación y de dictado. 

- Compatibilidad con muchos idiomas hablados y dialectos. Para obtener la lista completa de los idiomas admitidos en cada modo de reconocimiento, consulte [Supported languages](supported-languages.md#speech-to-text) (Idiomas admitidos).

- Modelos acústicos y lingüísticos personalizados, que le permiten adaptar la aplicación al vocabulario de dominio especializado, al entorno de habla y a la forma de hablar de los usuarios.

- Comprensión del lenguaje natural A través de la integración con [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), puede derivar las intenciones y las entidades de voz. Los usuarios no tienen que conocer el vocabulario de la aplicación, pero pueden describir lo que quieren en sus propias palabras.

## <a name="api-capabilities"></a>Funcionalidades de la API

Algunas funcionalidades de **Speech to Text** API no están disponibles a través de REST. En la tabla siguiente se resumen las funcionalidades de cada método de acceso a la API.

| Caso de uso | REST | SDK |
|-----|-----|-----|----|
| Transcripción de una expresión corta, como un comando (longitud < 15 s); sin resultados provisionales | SÍ | SÍ |
| Transcripción de una expresión más larga (> 15 s) | Sin  | SÍ |
| Transcripción de secuencias de audio con resultados provisionales opcionales | Sin  | SÍ |
| Reconocimiento de las intenciones del orador a través de LUIS | No\* | SÍ |

\* *Las entidades e intenciones de LUIS pueden derivarse mediante una suscripción independiente de LUIS. Con esta suscripción, el SDK puede llamar a LUIS y proporcionar resultados de la entidad y la intención, así como transcripciones de voz. Con la API REST, puede llamar a LUIS usted mismo para deducir las entidades y las intenciones con su suscripción a LUIS.*

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Vea cómo funciona el reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
