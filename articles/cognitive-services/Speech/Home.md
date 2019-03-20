---
title: Servicio Bing Speech de Microsoft | Microsoft Docs
titlesuffix: Azure Cognitive Services
description: Use Microsoft Speech API para agregar acciones de voz a las aplicaciones, incluida la interacción en tiempo real con usuarios.
services: cognitive-services
author: zhouwangzw
manager: wolfma
ms.service: cognitive-services
ms.subservice: bing-speech
ms.topic: article
ms.date: 09/18/2018
ms.author: zhouwang
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: d2c7211831658a18e65e04aa753607f4eb22dac8
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/22/2019
ms.locfileid: "56673195"
---
# <a name="what-is-bing-speech"></a>¿Qué es Bing Speech?

[!INCLUDE [Deprecation note](../../../includes/cognitive-services-bing-speech-api-deprecation-note.md)]

Microsoft Bing Speech API es una API basada en la nube que proporciona a los desarrolladores un mecanismo sencillo para crear en las aplicaciones eficaces características con voz habilitada, como el control de comandos por voz, el diálogo con el usuario mediante una conversación en lenguaje natural y la transcripción y el dictado de voz. Microsoft Speech API admite tanto la conversión de *voz en texto* como la conversión de *texto a voz*.

- **Speech to Text** API convierte la voz humana en texto que puede usarse como entrada o comandos para controlar la aplicación.
- **Text to Speech** API convierte el texto en secuencias de audio que pueden reproducirse para el usuario de la aplicación.

## <a name="speech-to-text-speech-recognition"></a>Conversión de voz a texto (reconocimiento de voz)

Microsoft Speech Recognition API *transcribe* secuencias de audio en texto que la aplicación puede mostrar al usuario o actuar como si fueran entrada de comandos. Proporciona dos maneras de que los desarrolladores agreguen voz a sus aplicaciones: Las API REST **o** bibliotecas cliente basadas en Websocket.

- [API REST](GetStarted/GetStartedREST.md): los desarrolladores pueden usar llamadas HTTP desde sus aplicaciones hasta el servicio para el reconocimiento de voz.
- [Bibliotecas cliente](GetStarted/GetStartedClientLibraries.md): si necesitan características avanzadas, los desarrolladores pueden descargar las bibliotecas cliente de Voz de Microsoft y vincularlas en sus aplicaciones.  Las bibliotecas cliente están disponibles en distintas plataformas (Windows, Android, iOS) con distintos lenguajes (C#, Java, JavaScript, ObjectiveC). A diferencia de las API REST, las bibliotecas cliente utilizan un protocolo basado en Websocket.

| Casos de uso | [API de REST](GetStarted/GetStartedREST.md) | [Bibliotecas cliente](GetStarted/GetStartedClientLibraries.md) |
|-----|-----|-----|
| Convertir un audio hablado corto, por ejemplo, comandos (longitud audio < 15 s) sin resultados provisionales | Sí | Sí |
| Convertir un audio largo (> 15 s) | Sin  | Sí |
| Audio de secuencia con resultados provisionales deseados | Sin  | Sí |
| Comprender el texto convertido desde audio mediante LUIS | Sin  | Sí |

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

- Comience a usar el servicio de texto a voz de Microsoft: [Referencia de Text to Speech API](api-reference-rest/bingvoiceoutput.md). Para obtener la lista completa de idiomas y voces admitidas por la conversión de texto a voz, consulte [Supported Locales and Voice Fonts](api-reference-rest/bingvoiceoutput.md#SupLocales) (Configuraciones regionales y fuentes de voz compatibles).
