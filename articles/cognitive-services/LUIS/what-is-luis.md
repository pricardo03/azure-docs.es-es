---
title: 'Azure Cognitive Services: ¿Qué es Language Understanding (LUIS)? | Microsoft Docs'
description: Language Understanding (LUIS) es un servicio de API basado en la nube que aplica inteligencia de aprendizaje automático personalizado a una conversación o un texto de lenguaje natural de un usuario para predecir el significado global y extraer información pertinente y detallada.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: overview
ms.date: 06/11/2019
ms.author: diberry
ms.openlocfilehash: 569b33d299f52f0da50d8a8992420754aa85b533
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67062160"
---
# <a name="what-is-language-understanding-luis"></a>¿Qué es Language Understanding (LUIS)?

Language Understanding (LUIS) es un servicio de API basado en la nube que aplica inteligencia de aprendizaje automático personalizado a una conversación o un texto de lenguaje natural de un usuario para predecir el significado global y extraer información pertinente y detallada. 

Una aplicación cliente para LUIS es cualquier aplicación conversacional que se comunique con un usuario en lenguaje natural para completar una tarea. Entre estas aplicaciones cliente se encuentran, por ejemplo, aplicaciones de redes sociales, bots de chat y aplicaciones de escritorio habilitadas para voz.  

![Imagen conceptual de 3 aplicaciones cliente que trabajan con Language Understanding (LUIS) de Cognitive Services](./media/luis-overview/luis-entry-point.png "Conceptual image of 3 client applications working with Cognitive Services Language Understanding (LUIS)")

## <a name="use-luis-in-a-chat-bot"></a>Uso de LUIS en un bot de chat

<a name="Accessing-LUIS"></a>

Una vez que la aplicación de LUIS esté publicada, la aplicación cliente envía expresiones (texto) a la [API][endpoint-apis] del punto de conexión de procesamiento de lenguaje natural de LUIS y recibe los resultados como respuestas en formato JSON. Una aplicación cliente común para LUIS es un bot de chat.


![Imagen conceptual del trabajo de LUIS con el bot de chat para predecir el texto del usuario con reconocimiento de lenguaje natural (NLP)](./media/luis-overview/luis-overview-process-2.png "Conceptual imagery of LUIS working with Chat bot to predict user text with natural language understanding (NLP)")

|Paso|.|
|:--|:--|
|1|La aplicación cliente envía la _expresión_ del usuario (el texto en sus propias palabras), "I want to call my HR rep" ("Quiero llamar a mi representante de recursos humanos") al punto de conexión de LUIS como una solicitud HTTP.|
|2|LUIS aplica el modelo entrenado al texto en lenguaje natural para proporcionar reconocimiento inteligente sobre la entrada del usuario. LUIS devuelve una respuesta con formato JSON con una intención principal, "HRContact" ("Contacto de recursos humanos"). La respuesta en formato JSON del punto de conexión contiene como mínimo la expresión de consulta y la intención con mayor puntuación. También puede extraer datos, como la entidad Tipo de contacto.|
|3|La aplicación cliente usa la respuesta JSON para tomar decisiones acerca de cómo responder a las solicitudes del usuario. Estas decisiones pueden incluir algunos árboles de decisión en el código de Bot Framework y llamadas a otros servicios. |

La aplicación de LUIS proporciona inteligencia para que la aplicación cliente pueda tomar decisiones inteligentes. LUIS no proporciona estas opciones. 

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-processing"></a>Procesamiento de lenguaje natural

Una aplicación de LUIS contiene un modelo de lenguaje natural específico de un dominio. Puede iniciar la aplicación de LUIS con un modelo de dominio creado previamente, crear el suyo propio o combinar partes de un dominio creado previamente con su propia información personalizada.

* **Modelo creado previamente** LUIS tiene muchos modelos de dominio creados previamente, entre los que se incluyen intenciones, expresiones y entidades creadas previamente. Puede usar las entidades creadas previamente sin tener que utilizar las intenciones y expresiones del modelo precompilado. Los [modelos de dominio creados previamente](luis-how-to-use-prebuilt-domains.md) incluyen todo el diseño y suponen una excelente manera de empezar a usar rápidamente LUIS.

* **Entidades personalizadas** LUIS ofrece varias maneras de identificar sus propias intenciones y entidades personalizadas, como las entidades de aprendizaje automático, las entidades específicas o literales y una combinación de aprendizaje automático y literal.

## <a name="build-the-luis-model"></a>Creación del modelo de LUIS
Cree el modelo con las API de [creación](https://go.microsoft.com/fwlink/?linkid=2092087) o con el portal de LUIS.

El modelo de LUIS comienza con categorías de intenciones del usuario llamadas **[intenciones](luis-concept-intent.md)** . Cada intención necesita ejemplos de **[expresiones](luis-concept-utterance.md)** del usuario. Cada expresión puede ofrecer una gran variedad de datos que se deben extraer con **[entidades](luis-concept-entity-types.md)** . 

|Expresión de usuario de ejemplo|Intención|Entidades|
|-----------|-----------|-----------|
|"¿Reservar un vuelo a __Seattle__?"|BookFlight|Seattle|
|"¿Cuando __abre__ su tienda?"|StoreHoursAndLocation|open|
|"Programar una reunión a la __1 p.m.__ con __Bob__ de distribución"|ScheduleMeeting|1 p.m., Bob|

## <a name="query-prediction-endpoint"></a>Consulta de un punto de conexión de predicción

Después de que el modelo está creado y publicado en el punto de conexión, la aplicación cliente envía expresiones a la API del [punto de conexión](https://go.microsoft.com/fwlink/?linkid=2092356) de predicción publicado. La API aplica el modelo al texto para su análisis. La API responde con los resultados de la predicción en formato JSON.  

La respuesta en formato JSON del punto de conexión contiene como mínimo la expresión de consulta y la intención con mayor puntuación. También puede extraer datos como la siguiente entidad **Tipo de contacto**. 

```JSON
{
  "query": "I want to call my HR rep.",
  "topScoringIntent": {
    "intent": "HRContact",
    "score": 0.921233
  },
  "entities": [
    {
      "entity": "call",
      "type": "Contact Type",
      "startIndex": 10,
      "endIndex": 13,
      "score": 0.7615982
    }
  ]
}
```

## <a name="improve-model-prediction"></a>Mejora de la predicción del modelo

Una vez que un modelo de LUIS está publicado y recibe expresiones de usuario reales, LUIS ofrece varios métodos para mejorar la precisión de la predicción: [aprendizaje activo](luis-concept-review-endpoint-utterances.md) de expresiones de punto de conexión, [listas de frases](luis-concept-feature.md) para la inclusión de palabras de dominio y [patrones](luis-concept-patterns.md) para reducir el número de expresiones necesarias.

<a name="using-luis"></a>

## <a name="development-lifecycle"></a>Ciclo de vida del desarrollo
LUIS proporciona herramientas, control de versiones y colaboración con otros autores de LUIS para integrarse en el ciclo de vida de desarrollo completo en el nivel de la aplicación cliente y del modelo de lenguaje. 

## <a name="implementing-luis"></a>Implementación de LUIS
LUIS, como una API REST, se puede usar con cualquier producto, servicio o marco de trabajo que realice una solicitud HTTP. La lista siguiente contiene los principales productos y servicios de Microsoft que se utilizan con LUIS.

La principal aplicación cliente para LUIS es:
* [Bot de aplicación web](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-4.0) para crear rápidamente un bot de chat habilitado para LUIS para hablar con un usuario mediante entrada de texto. Usa [Bot Framework][bot-framework] versión [4.x](https://github.com/Microsoft/botbuilder-dotnet) para una experiencia de bot completa.

Herramientas para usar de forma rápida y sencilla LUIS con un bot:
* [CLI de LUIS](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS): paquete NPM que proporciona creación y predicción mediante una herramienta independiente de la línea de comandos o como importación. 
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen) LUISGen es una herramienta para generar código fuente de C# y de TypeScript fuertemente tipado a partir de un modelo de LUIS exportado.
* [Dispatch](https://aka.ms/dispatch-tool) permite que una aplicación primaria utilice varias aplicaciones de LUIS y QnA Maker mediante el uso de un modelo de distribuidor.
* [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) LUDown es una herramienta de la línea de comandos que ayuda a administrar los modelos de lenguaje para el bot.

Otros servicios de Cognitive Services que utilizan LUIS:
* [QnA Maker][qnamaker] permite la combinación de varios tipos de texto en una base de conocimiento de pregunta y respuesta.
* [Bing Spell Check API](../bing-spell-check/proof-text.md) proporciona corrección de texto antes de la predicción. 
* [Speech Service](../Speech-Service/overview.md) convierte solicitudes de lenguaje hablado en texto. 
* [Conversation Learner](https://docs.microsoft.com/azure/cognitive-services/labs/conversation-learner/overview) le permite crear conversaciones de bot más rápido con LUIS.
* [Project Personality Chat](https://docs.microsoft.com/azure/cognitive-services/project-personality-chat/overview) para controlar conversaciones de bot.

Ejemplos de uso de LUIS:
* Repositorio de GitHub para [Inteligencia artificial conversacional](https://github.com/Microsoft/AI).
* Ejemplos de Azure para [Language Understanding](https://github.com/Azure-Samples/cognitive-services-language-understanding)

## <a name="next-steps"></a>Pasos siguientes

Cree una nueva aplicación de LUIS con un dominio [creado previamente](luis-get-started-create-app.md) o [personalizado](luis-quickstart-intents-only.md). [Consulte el punto de conexión de predicción](luis-get-started-cs-get-intent.md) de una aplicación de IoT pública.

[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://go.microsoft.com/fwlink/?linkid=2092087
[endpoint-apis]: https://go.microsoft.com/fwlink/?linkid=2092356
[qnamaker]: https://qnamaker.ai/
