---
title: 'Tipos de entidad en aplicaciones de LUIS: Language Understanding'
titleSuffix: Azure Cognitive Services
description: Agregue entidades (datos clave del dominio de la aplicación) a las aplicaciones de Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 10/19/2018
ms.author: diberry
ms.openlocfilehash: 92b4864f8991380740e6edb498328ce2eea98250
ms.sourcegitcommit: ccdea744097d1ad196b605ffae2d09141d9c0bd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49650118"
---
# <a name="entities-in-luis"></a>Entidades de LUIS

Las entidades son palabras o frases de expresiones que son datos clave en el dominio de la aplicación.

## <a name="entity-compared-to-intent"></a>Comparación entre entidades e intenciones
La entidad representa una palabra o frase dentro de la expresión que quiere extraer. Una expresión puede incluir varias o ninguna entidad. Una entidad representa una clase que incluye una colección de objetos parecidos (lugares, cosas, personas, eventos o conceptos). Las entidades describen información relevante para la intención y a veces son esenciales para que la aplicación lleve a cabo la tarea. Por ejemplo, una aplicación de búsqueda de noticias podría incluir entidades como "tema", "fuente", "palabra clave" o "fecha de publicación", que son datos clave para buscar noticias. En una aplicación de reserva de viajes, "ubicación", "fecha", "aerolínea", "clase de viaje" y "billetes" representan información clave para reservar vuelos (relevante para la intención "Bookflight").

En comparación, la intención representa la predicción de toda la expresión. 

## <a name="entities-represent-data"></a>Las entidades representan datos
Las entidades son los datos que quiere extraer de la expresión. Pueden ser un nombre, una fecha, el nombre de un producto o cualquier grupo de palabras. 

|Expresión|Entidad|Datos|
|--|--|--|
|Comprar 3 billetes a Nueva York|Número creado previamente<br>Location.Destination|3<br>Nueva York|
|Comprar un billete de Nueva York a Londres para el 5 de marzo|Location.Origin<br>Location.Destination<br>Entidad datetimeV2 creada previamente|Nueva York<br>Londres<br>5 de marzo de 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Las entidades son opcionales, pero muy recomendables
Mientras las intenciones son obligatorias, las entidades son opcionales. No es necesario crear entidades para cada concepto en la aplicación, sino solo para los que resulten necesarios para que la aplicación pueda llevar a cabo la operación. 

Si las expresiones no tienen información detallada, el bot deberá continuar (no es necesario agregarlas). A medida que la aplicación crezca, puede ir agregándolas más adelante. 

Si no está seguro de cómo usar la información, agregue algunas entidades comunes creadas previamente, como datetimeV2, ordinal, correo electrónico y número de teléfono.

## <a name="label-for-word-meaning"></a>Etiqueta para el significado de las palabras
Si la elección o la organización de las palabras es la misma pero no significa lo mismo, no la etiquete con la entidad. 

En las siguientes expresiones inglesas, la palabra `fair` es un homógrafo. Es decir, se escribe igual pero tiene un significado diferente:

|Expresión|
|--|
|¿Qué tipo de ferias locales se producen en el área de Seattle este verano?|
|¿La clasificación actual para la revisión de Seattle es razonable?|

Si quiere que una entidad de evento busque todos los datos de eventos, etiquete la palabra `fair` en la primera expresión, pero no en la segunda.

## <a name="entities-are-shared-across-intents"></a>Las entidades se comparten entre las intenciones
Las entidades se comparten entre las intenciones. No pertenecen a ninguna intención. Las intenciones y las entidades pueden asociarse semánticamente, pero no se trata de una relación exclusiva.

En la expresión "Resérvame un billete a París", "París" es una entidad de tipo ubicación. Al reconocer las entidades que se mencionan en la entrada del usuario, LUIS le ayuda a elegir las acciones específicas que se deben llevar a cabo para satisfacer una intención.

## <a name="assign-entities-in-none-intent"></a>Asignar entidades en la intención None
Todas las intenciones, incluida **None** (Ninguno), deben tener entidades etiquetadas. De esta manera, LUIS puede obtener más información sobre la ubicación de las entidades dentro de las expresiones y las palabras que hay alrededor de las entidades. 

## <a name="entity-status-for-predictions"></a>Estado de la entidad para predicciones

Para más información, consulte [Predicciones de estado de entidad](luis-how-to-add-example-utterances.md#entity-status-predictions). 

## <a name="types-of-entities"></a>Tipos de entidades
LUIS ofrece muchos tipos de entidades: entidades creadas previamente, entidades personalizadas de aprendizaje automático y entidades de lista.

| NOMBRE | Se puede etiquetar | DESCRIPCIÓN |
| -- |--|--|
| **Creada previamente** <br/>[Personalizada](#prebuilt)| |  **Definición**<br>Tipos integrados que representan conceptos comunes. <br><br>**Lista**<br/>número de frase clave, ordinal, temperatura, dimensión, moneda, edad, porcentaje, correo electrónico, dirección URL, número de teléfono y frase clave. <br><br>Los nombres de las entidades creadas previamente están reservados. <br><br>Todas las entidades creadas previamente que se agregan a la aplicación se devuelven en la consulta de [punto de conexión](luis-glossary.md#endpoint). Para obtener más información, vea [Entidades creadas previamente](./luis-prebuilt-entities.md). <br/><br/>[Respuesta de ejemplo de entidad](luis-concept-data-extraction.md#prebuilt-entity-data)|
|<!-- added week of 3/21/08 --> **Expresión regular**<br/>[RegEx](#regex)||**Definición**<br>Expresión regular personalizada para el texto con formato de expresiones sin formato. No distingue entre mayúsculas y minúsculas e ignora la variante cultural.  <br><br>Esta entidad es adecuada para las palabras o frases que tienen un formato coherente con cualquier variación que también sea coherente.<br><br>La coincidencia de expresiones regulares se aplica después de las modificaciones de la ortografía. <br><br>Si la expresión regular es demasiado compleja (por ejemplo, uso excesivo de corchetes), no podrá agregar la expresión al modelo. <br><br>**Ejemplo**<br>`kb[0-9]{6,}` coincide con kb123456.<br/><br/>[Guía de inicio rápido](luis-quickstart-intents-regex-entity.md)<br>[Respuesta de ejemplo de entidad](luis-concept-data-extraction.md)|
| **Simple** <br/>[Con aprendizaje automático](#machine-learned) | ✔ | **Definición**<br>Una entidad simple es una entidad genérica que describe un concepto único que se ha aprendido en un contexto de aprendizaje automático. El contexto incluye la elección de palabras, la colocación de las palabras y la longitud de la expresión.<br/><br/>Se trata de una entidad adecuada para las palabras o frases que no tienen un formato coherente pero que indican lo mismo. <br/><br/>[Guía de inicio rápido](luis-quickstart-primary-and-secondary-data.md)<br/>[Respuesta de ejemplo de entidad](luis-concept-data-extraction.md#simple-entity-data)|  
| **Lista** <br/>[Coincidencia exacta](#exact-match)|| **Definición**<br>Las entidades de lista representan un conjunto fijo y cerrado de palabras relacionadas (y sus sinónimos) del sistema. <br><br>Cada entidad de lista puede tener uno o varios formatos. Es útil para un conjunto conocido de variaciones sobre cómo representar el mismo concepto.<br/><br/>LUIS no detecta valores adicionales para las entidades de lista. Use la característica **Recommend** (Recomendar) para ver sugerencias de palabras nuevas en función de la lista actual.<br/><br>Si hay más de una entidad de lista con el mismo valor, se devolverá cada entidad en la consulta de punto de conexión. <br/><br/>[Guía de inicio rápido](luis-quickstart-intent-and-list-entity.md)<br>[Respuesta de ejemplo de entidad](luis-concept-data-extraction.md#list-entity-data)| 
| **Pattern.any** <br/>[Mixto](#mixed) | ✔|**Definición**<br>Patterns.any es un marcador de posición de longitud variable que solo se usa en la expresión de plantilla de un patrón para marcar dónde empieza y acaba la entidad.  <br><br>**Ejemplo**<br>Dada una búsqueda de expresión de libros en función del título, pattern.any extrae el título completo. `Who wrote {BookTitle}[?]` es una expresión de plantilla que usa pattern.any.<br/><br/>[Tutorial](luis-tutorial-pattern.md)<br>[Respuesta de ejemplo de entidad](luis-concept-data-extraction.md#composite-entity-data)|  
| **Compuesto** <br/>[Con aprendizaje automático](#machine-learned) | ✔|**Definición**<br>Una entidad compuesta está formada de otras entidades, como las entidades precompiladas, las entidades simples, regex, listas y jerárquicas. Las entidades independientes forman una entidad completa. <br><br>**Ejemplo**<br>Una entidad compuesta denominada PedidoBilleteAvión puede tener las entidades secundarias creadas previamente `number` y `ToLocation`. <br/><br/>[Tutorial](luis-tutorial-composite-entity.md)<br>[Respuesta de ejemplo de entidad](luis-concept-data-extraction.md#composite-entity-data)|  
| **Jerárquico** <br/>[Con aprendizaje automático](#machine-learned) |✔ | **Definición**<br>Una entidad jerárquica es una categoría de entidades simples de aprendizaje contextual.<br><br>**Ejemplo**<br>Dada una entidad jerárquica de `Location` con los elementos secundarios `ToLocation` y `FromLocation`, cada elemento secundario se puede determinar en función del **contexto** dentro de la expresión. En la expresión, `Book 2 tickets from Seattle to New York`, `ToLocation` y `FromLocation` son contextualmente diferentes teniendo en cuenta las palabras de su alrededor. <br/><br/>**No lo use si**<br>Si busca una entidad que tenga coincidencias de texto exactas para los elementos secundarios independientemente del contexto, debe usar una entidad de lista. Si busca una relación entre elementos primarios y secundarios con otros tipos de entidades, debe usar la entidad compuesta.<br/><br/>[Guía de inicio rápido](luis-quickstart-intent-and-hier-entity.md)<br>[Respuesta de ejemplo de entidad](luis-concept-data-extraction.md#hierarchical-entity-data)|

Las entidades <a name="prebuilt"></a>
**creadas previamente** son entidades personalizadas que proporciona LUIS. Algunas de estas entidades se definen en el proyecto de código abierto [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). Hay muchos [ejemplos](https://github.com/Microsoft/Recognizers-Text/tree/master/Specs) en el directorio /Specs de las referencias culturales admitidas. Si su referencia cultural o entidad específica no se admite actualmente, colabore en el proyecto. 

Las entidades de <a name="machine-learned"></a>
**aprendizaje automático** funcionan mejor cuando se prueban con [consultas de punto de conexión](luis-concept-test.md#endpoint-testing) y con [expresiones de punto de conexión de revisión](luis-how-to-review-endoint-utt.md). 

<a name="regex"></a>
**Las entidades de expresión regular** se definen con una expresión regular proporcionada por el usuario como parte de la definición de la entidad. 

Las entidades de <a name="exact-match"></a>
**coincidencia exacta** usan el texto proporcionado en la entidad para conseguir una coincidencia de texto exacta.

Las entidades <a name="mixed"></a>
**mixtas** usan una combinación de métodos de detección de entidades.

## <a name="entity-limits"></a>Límites de entidad
Consulte los [límites](luis-boundaries.md#model-boundaries) para saber cuántas entidades de cada tipo puede agregar a un modelo.

## <a name="entity-roles"></a>Roles de entidad
Los [roles](luis-concept-roles.md) de entidad se aplican a las entidades personalizadas y a las creadas previamente y solo se usan con patrones. 

## <a name="composite-vs-hierarchical-entities"></a>Entidades compuestas frente a entidades jerárquicas
Las entidades compuestas y las entidades jerárquicas tienen relaciones entre elementos primarios y secundarios, y se aprenden mediante el aprendizaje automático. Gracias al aprendizaje automático, LUIS puede comprender las entidades en función de otros contextos (organización de palabras). Las entidades compuestas son más flexibles porque admiten distintos tipos de entidad como elementos secundarios. Los elementos secundarios de una entidad jerárquica son solo entidades simples. 

|Escriba|Propósito|Ejemplo|
|--|--|--|
|Jerárquico|Elementos primarios y secundarios de entidades simples|Location.Origin=Nueva York<br>Location.Destination=Londres|
|Compuesto|Entidades de elementos primarios y secundarios: creada previamente, lista, simple y jerárquica| number=3<br>list=primera clase<br>prebuilt.datetimeV2=5 de marzo|

## <a name="data-matching-multiple-entities"></a>Datos que coinciden con varias entidades
Si una palabra o frase coincide con más de una entidad, la consulta de punto de conexión devolverá todas las entidades. Si agrega una entidad de número y una entidad datetimeV2 creadas previamente y tiene la expresión `create meeting on 2018/03/12 for lunch with wayne`, LUIS reconocerá todas las entidades y devolverá una matriz de entidades como parte de la respuesta del punto de conexión JSON: 

```JSON
{
  "query": "create meeting on 2018/03/12 for lunch with wayne",
  "topScoringIntent": {
    "intent": "Calendar.Add",
    "score": 0.9333419
  },
  "entities": [
    {
      "entity": "2018/03/12",
      "type": "builtin.datetimeV2.date",
      "startIndex": 18,
      "endIndex": 27,
      "resolution": {
        "values": [
          {
            "timex": "2018-03-12",
            "type": "date",
            "value": "2018-03-12"
          }
        ]
      }
    },
    {
      "entity": "2018",
      "type": "builtin.number",
      "startIndex": 18,
      "endIndex": 21,
      "resolution": {
        "value": "2018"
      }
    },
    {
      "entity": "03/12",
      "type": "builtin.number",
      "startIndex": 23,
      "endIndex": 27,
      "resolution": {
        "value": "0.25"
      }
    }
  ]
}
```

## <a name="data-matching-multiple-list-entities"></a>Datos que coinciden con varias entidades de lista
Si una palabra o frase coincide con más de una entidad de lista, la consulta de punto de conexión devolverá todas las entidades de lista.

Para la consulta `when is the best time to go to red rock?`, si la aplicación tiene la palabra `red` en más de una lista, LUIS reconocerá todas las entidades y devolverá una matriz de entidades como parte de la respuesta del punto de conexión JSON: 

```JSON
{
  "query": "when is the best time to go to red rock?",
  "topScoringIntent": {
    "intent": "Calendar.Find",
    "score": 0.06701678
  },
  "entities": [
    {
      "entity": "red",
      "type": "Colors",
      "startIndex": 31,
      "endIndex": 33,
      "resolution": {
        "values": [
          "Red"
        ]
      }
    },
    {
      "entity": "red rock",
      "type": "Cities",
      "startIndex": 31,
      "endIndex": 38,
      "resolution": {
        "values": [
          "Destinations"
        ]
      }
    }
  ]
}
```

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Si necesita más del número máximo de entidades 

Es posible que tenga que usar entidades jerárquicas y entidades compuestas. Las entidades jerárquicas reflejan la relación entre las entidades que comparten características o son miembros de una categoría. Todas las entidades secundarias son miembros de la categoría de su elemento primario. Por ejemplo, una entidad jerárquica denominada ClaseBilleteAvión podría tener las entidades secundarias ClaseEconómica y PrimeraClase. La jerarquía abarca un solo nivel de profundidad.  

Las entidades compuestas representan partes de un total. Por ejemplo, una entidad compuesta denominada PedidoBilleteAvión podría tener las entidades secundarias Aerolínea, Destino, CiudadSalida, FechaSalida y ClaseBilleteAvión. Debe crear una entidad compuesta a partir de entidades simples existentes, elementos secundarios de entidades jerárquicas o entidades creadas previamente.  

LUIS también proporciona el tipo de entidad de lista que no es de aprendizaje automático, pero permite a la aplicación de LUIS especificar una lista fija de valores. Vea la referencia de [LUIS Boundaries](luis-boundaries.md) (Límites de LUIS) para revisar los límites del tipo de entidad de lista. 

Si ha tenido en cuenta las entidades jerárquicas, compuestas y de lista, y aun así necesita más del límite, póngase en contacto con el soporte técnico. Para ello, recopile información detallada sobre el sistema, vaya al sitio web de [LUIS](luis-reference-regions.md#luis-website) y seleccione **Support** (Soporte). Si la suscripción a Azure incluye servicios de soporte técnico, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/options/). 

## <a name="best-practices"></a>Procedimientos recomendados

Cree una [entidad](luis-concept-entity-types.md) si la aplicación que realiza la llamada o el bot necesita algunos parámetros o datos de la expresión que son necesarios para ejecutar una acción. Una entidad es una palabra o frase de la expresión que necesita extraer (quizás como parámetro para una función). 

Para poder seleccionar el tipo correcto de entidad que va a agregar a la aplicación, debe saber cómo introducen los datos los usuarios. Cada tipo de entidad se encuentra mediante un mecanismo diferente, como el aprendizaje automático, las listas cerradas o las expresiones regulares. Si no está seguro, comience con una entidad simple y etiquete la palabra o frase que representa esos datos en todas las expresiones por todas las intenciones, incluida la intención None (Ninguno).  

Revise las expresiones de punto de conexión de forma periódica para buscar un uso común donde se pueda identificar una entidad como expresión regular o encontrarse con una coincidencia de texto exacta.  

Como parte de la revisión, considere la posibilidad de agregar una lista de frases para agregar una señal a LUIS para las palabras o frases que son relevantes para el dominio pero que no son coincidencias exactas y para las que LUIS no tenga una confianza alta.  

Vea los [procedimientos recomendados](luis-concept-best-practices.md) para obtener más información.

## <a name="next-steps"></a>Pasos siguientes

Aprenda conceptos sobre las buenas [expresiones](luis-concept-utterance.md). 

Vea [Add entities](luis-how-to-add-entities.md) (Agregar entidades) para obtener más información sobre cómo agregar entidades a la aplicación de LUIS.