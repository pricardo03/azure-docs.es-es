---
title: Acerca de Language Understanding (LUIS) en Azure | Microsoft Docs
description: Obtenga información acerca de cómo usar Language Understanding (LUIS) para emplear la eficacia del aprendizaje automático en sus aplicaciones.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/22/2017
ms.author: v-geberr
ms.openlocfilehash: 1c68c586fb799a540f70804d181aa66b2bda9e97
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2018
ms.locfileid: "37952545"
---
# <a name="what-is-language-understanding-luis"></a>¿Qué es Language Understanding (LUIS)?
Language Understanding (LUIS) es un servicio basado en la nube que aplica aprendizaje automático personalizado a un texto de lenguaje natural y conversacional de un usuario para predecir el significado global y extraer información pertinente y detallada. 

Una aplicación cliente para LUIS puede ser cualquier aplicación conversacional que se comunique con un usuario en lenguaje natural para completar una tarea. Entre estas aplicaciones cliente se encuentran, por ejemplo, aplicaciones de redes sociales, bots de chat y aplicaciones de escritorio habilitadas para voz.  

![Imagen conceptual de 3 aplicaciones que nutren a LUIS con información](./media/luis-overview/luis-entry-point.png)

## <a name="what-is-a-luis-app"></a>¿Qué es una aplicación de LUIS?
Una aplicación de LUIS contiene un modelo de lenguaje natural específico de un dominio que el usuario diseña. Puede iniciar la aplicación de LUIS con un modelo de dominio creado previamente, crear el suyo propio o combinar partes de un dominio creado previamente con su propia información personalizada.

[Los modelos de dominio creados previamente](luis-how-to-use-prebuilt-domains.md) ya incluyen todas estas partes y suponen una excelente manera de empezar a usar rápidamente LUIS.

La aplicación de LUIS también contiene la configuración de la integración, [colaboradores](luis-concept-collaborator.md) y [versiones](luis-concept-version.md).

## <a name="using-a-luis-app"></a>Uso de una aplicación de LUIS
<a name="Accessing-LUIS"></a> Una vez que la aplicación de LUIS esté publicada, la aplicación cliente envía expresiones a la [API del punto de conexión][endpoint-apis] de LUIS y recibe los resultados de la predicción como respuestas en JSON.

En el siguiente diagrama, en primer lugar su bot de chat cliente envía texto de usuario de lo que desea una persona en sus propias palabras a LUIS en una solicitud HTTP. En segundo lugar, LUIS aplica el modelo aprendido al lenguaje natural para dar sentido a la entrada del usuario y devuelve una respuesta en formato JavaScript Object Notation (JSON). En tercer lugar, el bot de chat cliente usa la respuesta JSON para satisfacer las solicitudes del usuario. 

![Imágenes conceptuales de LUIS trabajando con el bot de chat](./media/luis-overview/luis-overview-process-2.png)

### <a name="example-of-json-endpoint-response"></a>Ejemplo de respuesta de punto de conexión de JSON

La respuesta del punto de conexión de JSON contiene como mínimo la expresión de consulta y la intención con mayor puntuación. 

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

<a name="Key-LUIS-concepts"></a>
<a name="what-is-a-luis-model"></a>
## <a name="what-is-a-natural-language-model"></a>¿Qué es un modelo de lenguaje natural?
Un modelo comienza con una lista de intenciones generales del usuario, denominadas _intenciones_, como "Reservar vuelo" o "Llamar al departamento de soporte técnico". Para las intenciones se proporciona texto de ejemplo del usuario, denominado _expresiones de ejemplo_. Luego se marcan las palabras o frases significativas de la expresión, denominadas _entidades_.


Un modelo incluye:

* **[intenciones](#intents)**: categorías de intenciones del usuario (acción pretendida o resultado)
* **[entidades](#entities)**: determinados tipos de datos en expresiones como número, correo electrónico o nombre
* **[expresiones de ejemplo](#example-utterances)**: texto de ejemplo que un usuario proporciona a la aplicación cliente

### <a name="intents"></a>Intenciones 
Una [intención](luis-how-to-add-intents.md) es el p_ropósito_ u objetivo que está presente en la expresión de un usuario, como reservar un vuelo, pagar una factura o buscar un artículo de prensa. Se crea una intención para cada acción. Una aplicación de viajes de LUIS puede definir una intención denominada "BookFlight". La aplicación cliente puede usar la intención con mayor puntuación para desencadenar una acción. Por ejemplo, cuando se devuelve la intención "BookFlight" desde LUIS, la aplicación cliente podría desencadenar una llamada de API a un servicio externo para reservar un billete de avión.

### <a name="entities"></a>Entidades
Un [entidad](luis-how-to-add-entities.md) representa la información detallada que se encuentra en la expresión que es pertinente para la solicitud del usuario. Por ejemplo, en la expresión "Reservar un billete a París", se está solicitando un solo billete, y "París" es una ubicación. Se encuentran dos entidades: "un billete", que representa un solo billete, y "París", que representa el destino. 

Una vez que LUIS devuelve las entidades que se encuentran en la expresión del usuario, la aplicación cliente puede usar la lista de entidades como parámetros para desencadenar una acción. Por ejemplo, reservar un vuelo requiere entidades como el destino del viaje, la fecha y la aerolínea.

LUIS proporciona varias maneras de identificar y clasificar entidades.

* **Entidades creadas previamente** LUIS tiene muchos modelos de dominio precompilados en los que se incluyen intenciones, expresiones y [entidades creadas previamente](luis-prebuilt-entities.md). Puede usar las entidades creadas previamente sin tener que utilizar las intenciones y expresiones del modelo precompilado. Las entidades creadas previamente le ahorran tiempo.

* **Entidades personalizadas** LUIS ofrece varias maneras de identificar sus propias [entidades](luis-concept-entity-types.md) personalizadas, como entidades de aprendizaje automático, entidades específicas o literales y una combinación de aprendizaje automático y literal.

### <a name="example-utterances"></a>Expresiones de ejemplo
Una [expresión](luis-how-to-add-example-utterances.md) de ejemplo es una entrada de texto del usuario que la aplicación cliente debe comprender. Puede ser una frase, como "Reservar un billete a París" o un fragmento de una frase, como "Reservar" o "Vuelo a París". Las expresiones no siempre están bien formadas, y puede haber muchas variaciones de expresiones para una intención determinada. Agregue entre 10 y 20 expresiones de ejemplo a cada intención y marque las entidades de cada una de ellas.

|Expresión de usuario de ejemplo|Intención|Entidades|
|-----------|-----------|-----------|
|"¿Reservar un vuelo a __Seattle__?"|BookFlight|Seattle|
|"¿Cuando __abre__ su tienda?"|StoreHoursAndLocation|open|
|"Programar una reunión a la __1 p.m.__ con __Bob__ de distribución"|ScheduleMeeting|1 p.m., Bob|

## <a name="improve-prediction-accuracy"></a>Mejorar la precisión de la predicción
Una vez que la aplicación de LUIS está publicada y recibe expresiones de usuario reales, LUIS ofrece varios métodos para mejorar la precisión de la predicción: [aprendizaje activo](#active-learning) de expresiones de punto de conexión, [listas de frases](#phrase-lists) para la inclusión de palabras de dominio y [patrones](#patterns) para reducir el número de expresiones necesarias.

### <a name="active-learning"></a>Aprendizaje activo
En el proceso de [aprendizaje activo](luis-how-to-review-endoint-utt.md), LUIS le permite adaptar la aplicación de LUIS a expresiones reales seleccionando las expresiones que recibió en el punto de conexión para su revisión. Puede aceptar o corregir la predicción de punto de conexión, volver a entrenar y volver a publicar. LUIS aprende rápidamente con este proceso iterativo, requiriendo una cantidad mínima de su tiempo y esfuerzo. 

### <a name="phrase-lists"></a>Listas de frases 
LUIS proporciona [listas de frases](luis-concept-feature.md) para que pueda indicar palabras o frases importantes en el dominio modelo. LUIS usa estas listas para agregar una pertinencia adicional a las palabras y frases que, en caso contrario, no se encontrarían en el modelo.

### <a name="patterns"></a>Patrones 
Los patrones le permiten simplificar la recopilación de expresiones de una intención en [plantillas](luis-concept-patterns.md) comunes de elección de las palabras y orden de las palabras. Esto permite que LUIS aprenda más rápido al necesitar menos expresiones de ejemplo para las intenciones. Los patrones son un sistema híbrido de expresiones regulares y expresiones de aprendizaje automático. 

<a name="using-luis"></a>

## <a name="authoring-and-accessing-luis"></a>Creación y acceso a LUIS
Compile su aplicación de LUIS desde el sitio web de LUIS o mediante programación con las API de [creación](https://aka.ms/luis-authoring-apis), o utilice ambos procesos según la necesidad de creación. Acceda a la aplicación de LUIS publicada mediante el [punto de conexión](https://aka.ms/luis-endpoint-apis) de la consulta. 

LUIS ofrece tres sitios web en todo el mundo, accederá a uno u otro dependiendo de su región de creación. La región de creación determina la región de Azure donde puede publicar su aplicación de LUIS.
<!--
|Authoring region|Publishing region(s)|
|--|--|
|[www.luis.ai](https://www.luis.ai)|**U.S.**<br>West US<br>West US 2<br>East US<br>East US 2<br>South Central US<br>West Central US<br><br>**Asia**<br>Southeast Asia<br>East Asia<br><br>**South America**<br>Brazil South |
|[au.luis.ai](https://au.luis.ai)|Australia East|
|[eu.luis.ai](https://eu.luis.ai)|West Europe<br>North Europe|
-->

Obtenga [más](luis-reference-regions.md) información sobre las regiones de creación y publicación.

## <a name="what-technologies-work-with-luis"></a>¿Qué tecnologías funcionan con LUIS?
Varias tecnologías de Microsoft funcionan con LUIS:

* [Bing Spell Check API](../bing-spell-check/proof-text.md) proporciona corrección de texto antes de la predicción. 
* [Bot Framework][bot-framework] permite a un bot de chat comunicarse con un usuario a través de entrada de texto. Seleccione el SDK [3.x](https://github.com/Microsoft/BotBuilder) o [4.x](https://github.com/Microsoft/botbuilder-dotnet) para ofrecer una experiencia de bot completa.
* [QnA Maker][qnamaker] permite la combinación de varios tipos de texto en una base de conocimiento de pregunta y respuesta.
* [Speech](../Speech/home.md) convierte las solicitudes de lengua hablada en texto. Una vez convertidas en texto, LUIS procesa las solicitudes. Vea el [SDK de Speech](https://aka.ms/csspeech) para obtener más información.
* [Text Analytics](../text-analytics/overview.md) proporciona análisis de sentimiento y extracción de los principales datos de una frase.

## <a name="next-steps"></a>Pasos siguientes
Cree una nueva aplicación de LUIS con un dominio [creado previamente](luis-get-started-create-app.md) o [personalizado](luis-quickstart-intents-only.md).

<!-- Reference-style links -->
[bot-framework]: https://docs.microsoft.com/bot-framework/
[flow]: https://docs.microsoft.com/connectors/luis/
[authoring-apis]: https://aka.ms/luis-authoring-api
[endpoint-apis]: https://aka.ms/luis-endpoint-apis
[qnamaker]: https://qnamaker.ai/