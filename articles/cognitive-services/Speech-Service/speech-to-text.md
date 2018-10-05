---
title: Acerca de Speech to Text
description: Información general de las funcionalidades de Speech to Text API.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/07/2018
ms.author: v-jerkin
ms.openlocfilehash: 7cb0257a7302221f80bb90c0a6c3446cde07290a
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434133"
---
# <a name="about-the-speech-to-text-api"></a>Acerca de Speech to Text API

**Speech to Text** API *transcribe* secuencias de audio en texto que la aplicación puede mostrar al usuario o actuar como si fuera una entrada de comandos. Las API pueden usarse con una biblioteca cliente del SDK (para las plataformas e idiomas admitidos) o una API REST.

**Speech to Text** API ofrece las siguientes características:

- Tecnología avanzada de reconocimiento de voz de Microsoft (la misma que usan Cortana, Office y otros productos de Microsoft).

- Reconocimiento continuo en tiempo real. **Speech to Text** permite a los usuarios transcribir audio en texto en tiempo real. También admite la recepción de los resultados intermedios de las palabras que reconoció hasta ahora. El servicio reconoce automáticamente el final del habla. Los usuarios también pueden elegir opciones de formato adicionales, incluidos el uso de mayúsculas y signos de puntuación, el enmascaramiento de palabras soeces y la normalización de texto inverso.

- Optimización de los resultados de **Speech to Text** para escenarios interactivos, de conversación y de dictado. Los resultados reconocidos se devuelven en formato léxico y de visualización (para los resultados léxicos, consulte DetailedSpeechRecognitionResult en los ejemplos o la API).

- Compatibilidad con muchos idiomas hablados y dialectos. Para obtener la lista completa de los idiomas admitidos en cada modo de reconocimiento, consulte [Supported languages](language-support.md#speech-to-text) (Idiomas admitidos).

- Modelos acústicos y lingüísticos personalizados, que le permiten adaptar la aplicación al vocabulario de dominio especializado, al entorno de habla y a la forma de hablar de los usuarios.

- Comprensión del lenguaje natural A través de la integración con [Language Understanding](https://docs.microsoft.com/azure/cognitive-services/luis/) (LUIS), puede derivar las intenciones y las entidades de voz. Los usuarios no tienen que conocer el vocabulario de la aplicación, pero pueden describir lo que quieren en sus propias palabras.

## <a name="api-capabilities"></a>Funcionalidades de la API

A través de REST hay disponible un lote de las capacidades de la API **Speech to Text**, especialmente en relación con la personalización. En la tabla siguiente se resumen las funcionalidades de cada método de acceso a la API. Para obtener una lista completa de las funcionalidades y detalles de la API, consulte [Swagger](https://swagger/service/11ed9226-335e-4d08-a623-4547014ba2cc#/).

| Caso de uso | REST | SDK |
|-----|-----|-----|----|
| Transcripción de una expresión corta, como un comando (longitud < 15 s); sin resultados provisionales | SÍ | SÍ |
| Transcripción de una expresión más larga (> 15 s) | Sin  | SÍ |
| Transcripción de secuencias de audio con resultados provisionales opcionales | Sin  | SÍ |
| Reconocimiento de las intenciones del orador a través de LUIS | No\* | SÍ |
| Creación de pruebas de precisión | SÍ | Sin  |
| Carga de conjuntos de datos para la adaptación de modelos | SÍ | Sin  |
| Creación y administración de modelos de voz | SÍ | Sin  |
| Creación y administración de implementaciones de modelos | SÍ | Sin  |
| Administrar suscripciones | SÍ | Sin  |
| Creación y administración de implementaciones de modelos | SÍ | Sin  |
| Creación y administración de implementaciones de modelos | SÍ | Sin  |

> [!NOTE]
> La API de REST implementa una medida que limita las solicitudes de API a 25 por cada 5 segundos. Los encabezados de los mensajes le informarán sobre los límites.

\* *Las entidades e intenciones de LUIS pueden derivarse mediante una suscripción independiente de LUIS. Con esta suscripción, el SDK puede llamar a LUIS y proporcionar resultados de la entidad y la intención, así como transcripciones de voz. Con la API REST, puede llamar a LUIS usted mismo para deducir las entidades y las intenciones con su suscripción a LUIS.*

## <a name="next-steps"></a>Pasos siguientes

* [Obtenga su suscripción de prueba a Voz](https://azure.microsoft.com/try/cognitive-services/)
* [Guía de inicio rápido: Reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
* [Reconocimiento de intenciones a partir de contenido de voz en C#](how-to-recognize-intents-from-speech-csharp.md)
