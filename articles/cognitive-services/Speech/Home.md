---
title: Servicio Bing Speech de Microsoft | Microsoft Docs
description: Use Microsoft Speech API para agregar acciones de voz a las aplicaciones, incluida la interacción en tiempo real con usuarios.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.component: bing-speech
ms.topic: article
ms.date: 09/15/2017
ms.author: zhouwang
ms.openlocfilehash: d58642b95a60d4f1c83dfd969d0c76511dca4653
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43097402"
---
# <a name="microsoft-bing-speech-api-overview"></a>Introducción a Microsoft Bing Speech API

Microsoft Bing Speech API es una API basada en la nube que proporciona a los desarrolladores un mecanismo sencillo para crear en las aplicaciones eficaces características con voz habilitada, como el control de comandos por voz, el diálogo con el usuario mediante una conversación en lenguaje natural y la transcripción y el dictado de voz. Microsoft Speech API admite tanto la conversión de *voz en texto* como la conversión de *texto a voz*.

- **Speech to Text** API convierte la voz humana en texto que puede usarse como entrada o comandos para controlar la aplicación.
- **Text to Speech** API convierte el texto en secuencias de audio que pueden reproducirse para el usuario de la aplicación.

> [!NOTE] 
> En mayo de 2018, lanzamos el nuevo [servicio Voz](../speech-service/overview.md) en versión preliminar pública. Le animamos a que [lo pruebe gratuitamente](../speech-service/get-started.md).

## <a name="speech-to-text-speech-recognition"></a>Conversión de voz a texto (reconocimiento de voz)

Microsoft Speech Recognition API *transcribe* secuencias de audio en texto que la aplicación puede mostrar al usuario o actuar como si fueran entrada de comandos. Proporciona a los desarrolladores dos maneras agregar voz a sus aplicaciones: las API de REST **o** las bibliotecas cliente basadas en Websocket.

- [API de REST](GetStarted/GetStartedREST.md): los desarrolladores pueden usar llamadas HTTP desde sus aplicaciones al servicio para el reconocimiento de voz.
- [Bibliotecas cliente](GetStarted/GetStartedClientLibraries.md): para características avanzadas, los desarrolladores pueden descargar las bibliotecas cliente de voz de Microsoft y vincular sus aplicaciones.  Las bibliotecas cliente están disponibles en distintas plataformas (Windows, Android, iOS) con distintos lenguajes (C#, Java, JavaScript, ObjectiveC). A diferencia de las API REST, las bibliotecas cliente utilizan un protocolo basado en Websocket.

| Casos de uso | [API de REST](GetStarted/GetStartedREST.md) | [Bibliotecas cliente](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Convertir un audio hablado corto, por ejemplo, comandos (longitud audio < 15 s) sin resultados provisionales | SÍ | SÍ |
| Convertir un audio largo (> 15 s) | Sin  | SÍ |
| Audio de secuencia con resultados provisionales deseados | Sin  | SÍ |
| Comprender el texto convertido desde audio mediante LUIS | Sin  | SÍ |

Sea cual sea el enfoque que los desarrolladores elijan (las API de REST o las bibliotecas cliente), el servicio de voz de Microsoft admite lo siguiente:

- Tecnologías avanzadas de reconocimiento de voz de Microsoft que usan Cortana, el dictado de Office, el traductor de Office y otros productos de Microsoft.
- Reconocimiento continuado en tiempo real. Speech Recognition API permite a los usuarios transcribir audio en texto en tiempo real y admite recibir los resultados intermedios de las palabras que se han reconocido hasta ahora. El servicio de voz también admite la detección del fin de voz. Además, los usuarios pueden elegir funcionalidades de formato adicionales, como el uso de mayúsculas y signos de puntuación, el enmascaramiento de palabras soeces y la normalización de texto.
- Admite los resultados del reconocimiento de voz optimizados para escenarios *interactivos*, de *conversación* y de *dictado*. Para los escenarios de usuario que requieren modelos de lenguaje y modelos acústicos personalizados, [Custom Speech Service](../custom-speech-service/cognitive-services-custom-speech-home.md) le permite crear modelos de voz adaptados a la aplicación y los usuarios.
- Admite muchos idiomas hablados en varios dialectos. Para obtener la lista completa de los idiomas admitidos en cada modo de reconocimiento, consulte los [idiomas de reconocimiento](api-reference-rest/supportedlanguages.md).
- Integración con Language Understanding. Además de convertir el audio de entrada en texto, la conversión de *voz en texto* proporciona a las aplicaciones una capacidad adicional para comprender lo que significa el texto. Usa [Language Understanding Intelligent Service (LUIS)](../LUIS/what-is-luis.md) para extraer intenciones y entidades del texto reconocido.

### <a name="next-steps"></a>Pasos siguientes

- Empezar a usar Microsoft Speech Recognition Service con [API de REST](GetStarted/GetStartedREST.md) o [bibliotecas cliente](GetStarted/GetStartedClientLibraries.md).
- Extraer las [aplicaciones de ejemplo](samples.md) en el lenguaje de programación que prefiera.
- Vaya a la sección de referencia para buscar detalles de [Microsoft Speech Protocol](API-Reference-REST/websocketprotocol.md) y referencias de API.

## <a name="text-to-speech-speech-synthesis"></a>Conversión de texto a voz (síntesis de voz)

Las *Text to Speech* API usan REST para convertir el texto estructurado en una secuencia de audio. Las API proporcionan la conversión rápida de texto a voz en distintos idiomas y voces. Además, los usuarios también tienen la capacidad de cambiar las características de audio, como la pronunciación, el volumen, el tono, etc. con etiquetas SSML.

### <a name="next-steps"></a>Pasos siguientes

- Empezar a usar el servicio de conversión de texto a voz de Microsoft: [Referencia de Text to Speech API](api-reference-rest/bingvoiceoutput.md). Para obtener la lista completa de idiomas y voces admitidas por la conversión de texto a voz, consulte [Supported Locales and Voice Fonts](api-reference-rest/bingvoiceoutput.md#SupLocales) (Configuraciones regionales y fuentes de voz compatibles).
