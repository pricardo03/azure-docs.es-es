---
title: 'Azure Cognitive Services: ¿Qué es Language Understanding (LUIS)? | Microsoft Docs'
description: Language Understanding (LUIS) es un servicio de API basado en la nube que aplica inteligencia de aprendizaje automático personalizado a una conversación o un texto de lenguaje natural de un usuario para predecir el significado global y extraer información pertinente y detallada. Una aplicación cliente para LUIS es cualquier aplicación conversacional que se comunique con un usuario en lenguaje natural para completar una tarea. Entre estas aplicaciones cliente se encuentran, por ejemplo, aplicaciones de redes sociales, bots de chat y aplicaciones de escritorio habilitadas para voz.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: overview
ms.date: 10/06/2018
ms.author: diberry
ms.openlocfilehash: 28580a29c2ffaadfa3b3ea26cb28f103d883d576
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2018
ms.locfileid: "49637279"
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

<!--

### Example of JSON endpoint response

The minimum JSON endpoint response contains the query utterance, and the top scoring intent. It can also extract data such as the following **Contact Type** entity. 

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
-->

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>

## <a name="natural-language-processing"></a>Procesamiento de lenguaje natural

Una aplicación de LUIS contiene un modelo de lenguaje natural específico de un dominio. Puede iniciar la aplicación de LUIS con un modelo de dominio creado previamente, crear el suyo propio o combinar partes de un dominio creado previamente con su propia información personalizada.

* **Modelo creado previamente** LUIS tiene muchos modelos de dominio creados previamente, entre los que se incluyen intenciones, expresiones y entidades creadas previamente. Puede usar las entidades creadas previamente sin tener que utilizar las intenciones y expresiones del modelo precompilado. Los [modelos de dominio creados previamente](luis-how-to-use-prebuilt-domains.md) incluyen todo el diseño y suponen una excelente manera de empezar a usar rápidamente LUIS.

* **Entidades personalizadas** LUIS ofrece varias maneras de identificar sus propias intenciones y entidades personalizadas, como las entidades de aprendizaje automático, las entidades específicas o literales y una combinación de aprendizaje automático y literal.

## <a name="build-the-luis-model"></a>Creación del modelo de LUIS
Cree el modelo con las API de [creación](https://aka.ms/luis-authoring-apis) o con el portal de LUIS.

El modelo de LUIS comienza con categorías de intenciones del usuario llamadas **[intenciones](luis-concept-intent.md)**. Cada intención necesita ejemplos de **[expresiones](luis-concept-utterance.md)** del usuario. Cada expresión puede ofrecer una gran variedad de datos que se deben extraer con **[entidades](luis-concept-entity-types.md)**. 

|Expresión de usuario de ejemplo|Intención|Entidades|
|-----------|-----------|-----------|
|"¿Reservar un vuelo a __Seattle__?"|BookFlight|Seattle|
|"¿Cuando __abre__ su tienda?"|StoreHoursAndLocation|open|
|"Programar una reunión a la __1 p.m.__ con __Bob__ de distribución"|ScheduleMeeting|1 p.m., Bob|

<!--
## What is a natural language model?

A model begins with a list of general user intentions, called _intents_, such as "Book Flight" or "Contact Help Desk." You provide user's example text, called _example utterances_ for the intents. Then mark significant words or phrases in the utterance, called _entities_.


A model includes:

* **[intents](#intents)**: categories of user intentions (overall intended action or result)
* **[entities](#entities)**: specific types of data in utterances such as number, email, or name contained in text
* **[example utterances](#example-utterances)**: example text a user might enter in the client application

### Intents 

An [intent](luis-how-to-add-intents.md), short for _intention_, is a purpose or goal expressed in a user's utterance, such as booking a flight, paying a bill, or finding a news article. You create an intent for each action. A LUIS travel app may define an intent named "BookFlight." Each prediction query includes the top scored intent. 

The client application can use the top scoring intent to trigger an action. For example, when "BookFlight" intent is returned from LUIS, a client application could trigger an API call to an external service for booking a plane ticket.

### Entities

An [entity](luis-how-to-add-entities.md) represents detailed information found within the utterance that is relevant to the user's request. For example, in the utterance "Book a ticket to Paris",  a single ticket is requested, and "Paris" is a location. Two entities are found "a ticket" indicating a single ticket and "Paris" indicating the destination. 

After LUIS returns the entities found in the user’s utterance, the client application can use the list of entities as parameters to trigger an action. For example, booking a flight requires entities like the travel destination, date, and airline.
-->
<!--
### Example utterances

An example [utterance](luis-how-to-add-example-utterances.md) is text input from the user that the client application needs to understand. It may be a sentence, like "Book a ticket to Paris", or a fragment of a sentence, like "Booking" or "Paris flight." Utterances aren't always well-formed, and there can be many utterance variations for a particular intent. Add 10 to 20 example utterances to each intent and mark entities in every utterance.

|Example user utterance|Intent|Entities|
|-----------|-----------|-----------|
|"Book a flight to __Seattle__?"|BookFlight|Seattle|
|"When does your store __open__?"|StoreHoursAndLocation|open|
|"Schedule a meeting at __1pm__ with __Bob__ in Distribution"|ScheduleMeeting|1pm, Bob|
-->
## <a name="query-prediction-endpoint"></a>Consulta de un punto de conexión de predicción

Después de que el modelo está creado y publicado en el punto de conexión, la aplicación cliente envía expresiones a la API del [punto de conexión](https://aka.ms/luis-endpoint-apis) de predicción publicado. La API aplica el modelo al texto para su análisis. La API responde con los resultados de la predicción en formato JSON.  

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
<!--
### Active learning

In the [active learning](luis-how-to-review-endoint-utt.md) process, LUIS allows you to adapt the LUIS app to real-world utterances by selecting utterances it received at the endpoint for your review. You can accept or correct the endpoint prediction, retrain, and republish. LUIS learns quickly with this iterative process, taking the minimum amount of your time and effort. 

### Phrase lists 

LUIS provides [phrases lists](luis-concept-feature.md) so you can indicate important words or phrases of the model. LUIS uses these lists to add additional significance to those words and phrases that would otherwise not be found in the model.

### Patterns 

Patterns allow you to simplify an intent's utterance collection into common [templates](luis-concept-patterns.md) of word choice and word order. This allows LUIS to learn quicker by needing fewer example utterances for the intents. Patterns are a hybrid system of regular expressions and machine-learned expressions. 
-->
<a name="using-luis"></a>
<!--
## Authoring and accessing models
Author LUIS from the [authoring](https://aka.ms/luis-authoring-apis) APIs or from the LUIS portal. Query the published prediction endpoint of the model from the [endpoint](https://aka.ms/luis-endpoint-apis) APIs.
-->

## <a name="development-lifecycle"></a>Ciclo de vida del desarrollo
LUIS proporciona herramientas, control de versiones y colaboración con otros autores de LUIS para integrarse en el ciclo de vida de desarrollo completo en el nivel de la aplicación cliente y del modelo de lenguaje. 

## <a name="implementing-luis"></a>Implementación de LUIS
LUIS, como una API REST, se puede usar con cualquier producto, servicio o marco de trabajo que realice una solicitud HTTP. La lista siguiente contiene los principales productos y servicios de Microsoft que se utilizan con LUIS.

La principal aplicación cliente para LUIS es:
* [Bot de aplicación web](https://docs.microsoft.com/azure/bot-service/?view=azure-bot-service-3.0) para crear rápidamente un bot de chat habilitado para LUIS para hablar con un usuario mediante entrada de texto. Usa [Bot Framework][bot-framework] versión [3.x](https://github.com/Microsoft/BotBuilder) o [4.x](https://github.com/Microsoft/botbuilder-dotnet) para una experiencia de bot completa.

Herramientas para usar de forma rápida y sencilla LUIS con un bot:
* La [CLI de LUIS](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUIS): paquete NPM que proporciona creación y predicción mediante una herramienta independiente de la línea de comandos o como importación. 
* [LUISGen](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/LUISGen) LUISGen es una herramienta para generar código fuente de C# y de TypeScript fuertemente tipado a partir de un modelo de LUIS exportado.
* [Dispatch](https://aka.ms/dispatch-tool) permite que una aplicación primaria utilice varias aplicaciones de LUIS y QnA Maker mediante el uso de un modelo de distribuidor.
* [LUDown](https://github.com/Microsoft/botbuilder-tools/tree/master/packages/Ludown) LUDown es una herramienta de la línea de comandos que ayuda a administrar los modelos de lenguaje para el bot.

Otros servicios de Cognitive Services que utilizan LUIS:
* [QnA Maker][qnamaker] permite la combinación de varios tipos de texto en una base de conocimiento de pregunta y respuesta.
* [Bing Spell Check API](../bing-spell-check/proof-text.md) proporciona corrección de texto antes de la predicción. 
* [Speech Service](../Speech-Service/overview.md) convierte solicitudes de lenguaje hablado en texto. 
* [Conversation Learner](https://docs.microsoft.com/azure/cognitive-services/labs/conversation-learner/overview) le permite crear conversaciones de bot más rápido con LUIS.
* [Project Personality Chat](https://docs.microsoft.com/azure/cognitive-services/project-personality-chat/overview) para controlar conversaciones de bot.

## <a name="next-steps"></a>Pasos siguientes

Cree una nueva aplicación de LUIS con un dominio [creado previamente](luis-get-started-create-app.md) o [personalizado](luis-quickstart-intents-only.md). [Consulte el punto de conexión de predicción](luis-get-started-cs-get-intent.md) de una aplicación de IoT pública.

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/
