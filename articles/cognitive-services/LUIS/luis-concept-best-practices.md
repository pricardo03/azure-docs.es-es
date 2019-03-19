---
title: Procedimientos recomendados
titleSuffix: Language Understanding - Azure Cognitive Services
description: Obtenga información sobre los procedimientos recomendados de LUIS para conseguir los mejores resultados del modelo de la aplicación de LUIS.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: diberry
ms.openlocfilehash: d7547ecf387e070efe788ba5f8582076388f422a
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/28/2019
ms.locfileid: "56984210"
---
# <a name="best-practices-for-building-a-language-understanding-app-with-cognitive-services"></a>Procedimientos recomendados para compilar una aplicación de reconocimiento de lenguaje con Cognitive Services
Use el proceso de creación de aplicaciones para compilar la aplicación de LUIS. 

* Compilar el modelo de lenguaje
* Agregar algunas expresiones de ejemplo de entrenamiento (10-15 por intención)
* Publicar 
* Probar desde el punto de conexión 
* Agregar características

Una vez que la aplicación esté [publicada](luis-how-to-publish-app.md), use el ciclo de creación de agregar características, publicar y probar desde el punto de conexión. No comience el siguiente ciclo de creación al agregar más expresiones de ejemplo, ya que, de esta forma, LUIS no aprende el modelo con expresiones reales del usuario. 

Para que LUIS sea eficaz en su trabajo de aprendizaje, no expanda las expresiones hasta que el conjunto actual de expresiones de ejemplo y punto de conexión devuelva resultados seguros y con predicciones altas. Mejore las puntuaciones mediante un [aprendizaje activo](luis-concept-review-endpoint-utterances.md), [patrones](luis-concept-patterns.md) y [listas de frases](luis-concept-feature.md). 

## <a name="do-and-dont"></a>Qué se debe hacer y qué no
En la lista siguiente, se incluyen los procedimientos recomendados para las aplicaciones de LUIS:

|Sí|No|
|--|--|
|[Definir diferentes intenciones](#do-define-distinct-intents) |[Agregar varias expresiones de ejemplo a las intenciones](#dont-add-many-example-utterances-to-intents) |
|[Buscar un punto óptimo entre ser demasiado genérico y demasiado específico para cada intención](#do-find-sweet-spot-for-intents)|[Usar LUIS como una plataforma de aprendizaje](#dont-use-luis-as-a-training-platform)|
|[Compilar la aplicación de forma iterativa](#do-build-the-app-iteratively)|[Agregar varias expresiones de ejemplo del mismo formato, omitiendo otros formatos](#dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats)|
|[Agregar listas de frases y patrones en iteraciones posteriores](#do-add-phrase-lists-and-patterns-in-later-iterations)|[Combinar la definición de intenciones y entidades](#dont-mix-the-definition-of-intents-and-entities)|
|[Equilibrar las expresiones en todas las intenciones](#balance-your-utterances-across-all-intents), excepto en la intención None.<br>[Agregar expresiones de ejemplo a la intención None](#do-add-example-utterances-to-none-intent)|[Crear listas de frases con todos los valores posibles](#dont-create-phrase-lists-with-all-the-possible-values)|
|[Aprovechar la característica de sugerencia para un aprendizaje activo](#do-leverage-the-suggest-feature-for-active-learning)|[Agregar demasiados patrones](#dont-add-many-patterns)|
|[Supervisar el rendimiento de la aplicación](#do-monitor-the-performance-of-your-app)|[Entrenar y publicar al agregar cada expresión de ejemplo](#dont-train-and-publish-with-every-single-example-utterance)|
|[Usar versiones para cada iteración de la aplicación](#do-use-versions-for-each-app-iteration)||

## <a name="do-define-distinct-intents"></a>Definir diferentes intenciones
Asegúrese de que el vocabulario de cada intención sea solo para esa intención y no se superponga con otra intención. Por ejemplo, si quiere que una aplicación controle preparativos de viaje como vuelos y hoteles, puede elegir que estas áreas de asuntos sean intenciones independientes o que tengan una misma intención con entidades de datos específicos dentro de la expresión.

Si el vocabulario entre dos intenciones es el mismo, combine la intención y use entidades. 

Considere las siguientes expresiones de ejemplo:

|Expresiones de ejemplo|
|--|
|Reservar un vuelo|
|Reservar un hotel|

"Book a flight" ("Reservar un vuelo") y "Book a hotel" ("Reservar un hotel") usan el mismo vocabulario: "book a" (“reservar un”). Este formato es el mismo, por lo que debería tratarse de la misma intención con las diferentes palabras extraídas de las entidades "flight" (“vuelo”) y "hotel". 

Para obtener más información:
* Concepto: [Conceptos sobre intenciones en una aplicación de LUIS](luis-concept-intent.md)
* Tutorial: [Compilación de una aplicación de LUIS para determinar las intenciones del usuario](luis-quickstart-intents-only.md)
* Procedimientos para: [Adición de intenciones para determinar la intención de las expresiones del usuario](luis-how-to-add-intents.md)


## <a name="do-find-sweet-spot-for-intents"></a>Buscar un punto óptimo para las intenciones
Use datos de predicción de LUIS para determinar si las intenciones se superponen. Las intenciones que se superponen confunden a LUIS. El resultado es que la intención con mayor puntuación es demasiado cercana a otra intención. Dado que LUIS no usa exactamente la misma ruta a través de los datos para el aprendizaje todas las veces, una intención que se superpone tiene una posibilidad de ser la primera o segunda en el aprendizaje. Quiere que la puntuación de la expresión de cada intención esté más alejada para que esto no suceda. Una buena distinción de las intenciones debería dar como resultado cada vez la intención superior que se esperaba. 
 
## <a name="do-build-the-app-iteratively"></a>Compilar la aplicación de forma iterativa
Mantenga un conjunto de expresiones que no se use como [expresiones de ejemplo](luis-concept-utterance.md) o de punto de conexión. Siga mejorando la aplicación del conjunto de pruebas. Adapte el conjunto de pruebas para que refleje expresiones de usuarios reales. Use este conjunto de pruebas para evaluar cada iteración o versión de la aplicación. 

Los desarrolladores deben tener tres conjuntos de datos. El primero incluye las expresiones de ejemplo para compilar el modelo. El segundo es para probar el modelo en el punto de conexión. El tercero incluye los datos de prueba ocultos usados en las [pruebas por lotes](luis-how-to-batch-test.md). Este último conjunto no se usa en el aprendizaje de la aplicación ni se envía al punto de conexión.  

Para obtener más información:
* Concepto: [Ciclo de creación de una aplicación de LUIS](luis-concept-app-iteration.md)

## <a name="do-add-phrase-lists-and-patterns-in-later-iterations"></a>Agregar listas de frases y patrones en iteraciones posteriores

Un procedimiento recomendado es no aplicar estas prácticas antes de que la aplicación se haya probado. Debe entender cómo se comporta la aplicación antes de agregar listas de frases y patrones porque estas características son mayor peso que grabaciones de voz de ejemplo y distorsionará confianza. 

Una vez comprenda cómo se comporta sin estas características, agregue cada una de ellas a la aplicación según corresponda. No es necesario agregar estas características con cada [iteración](luis-concept-app-iteration.md) ni cambiarlas con cada versión. 

No existe ningún inconveniente en agregarlas al principio del diseño del modelo, pero es más fácil ver cómo resulta cada cambio de la característica después de que el modelo se haya probado con expresiones. 

Una práctica recomendada consiste en probar a través del [punto de conexión](luis-get-started-create-app.md#query-the-endpoint-with-a-different-utterance) para obtener la ventaja adicional del [aprendizaje activo](luis-concept-review-endpoint-utterances.md). El [panel prueba interactiva](luis-interactive-test.md) también es una metodología de pruebas válida. 
 

### <a name="phrase-lists"></a>Listas de frases

Las [listas de frases](luis-concept-feature.md) permiten definir diccionarios de palabras relacionadas con el dominio de la aplicación. Inicie la lista de frases con unas pocas palabras y después use la característica de sugerencia para que LUIS conozca más palabras en el vocabulario específicas de la aplicación. Una lista de frases mejora la detección de intención y la clasificación de entidad incrementando la señal asociada con palabras o frases que son importantes para la aplicación. 

No agregue cada palabra al vocabulario, puesto que la lista de frases no es una coincidencia exacta. 

Para obtener más información:
* Concepto: [Características de lista de frases en una aplicación de LUIS](luis-concept-feature.md)
* Procedimiento: [Uso de listas de frases para aumentar la señal de la lista de palabras](luis-how-to-add-features.md)

### <a name="patterns"></a>Patrones

Las expresiones de usuarios reales del punto de conexión, muy similares entre sí, pueden revelar patrones de elección y ubicación de las palabras. La característica del [patrón](luis-concept-patterns.md) toma esta elección y ubicación de las palabras junto con expresiones regulares para mejorar la precisión de la predicción. Una expresión regular en el patrón permite palabras y puntuación que pretende ignorar, mientras que sigue coincidiendo con el patrón. 

Use una [sintaxis opcional](luis-concept-patterns.md) del patrón para la puntuación, para que esta se pueda omitir. Use la [lista explícita](luis-concept-patterns.md#explicit-lists) para compensar los problemas de sintaxis de pattern.any. 

Para obtener más información:
* Concepto: [Los patrones mejoran la precisión de las predicciones](luis-concept-patterns.md)
* Procedimiento: [Cómo agregar patrones para mejorar la precisión de las predicciones](luis-how-to-model-intent-pattern.md)

## <a name="balance-your-utterances-across-all-intents"></a>Equilibrio de las expresiones en todas las intenciones

Para que las predicciones de LUIS sean precisas, la cantidad de expresiones de ejemplo en cada intención (excepto para la intención None), debe ser relativamente igual. 

Si tiene una intención con 100 expresiones de ejemplo y una intención con 20 expresiones de ejemplo, la intención de las 100 expresiones tendrá una mayor tasa de predicción.  

## <a name="do-add-example-utterances-to-none-intent"></a>Agregar expresiones de ejemplo a la intención None

Esta es la intención de reserva, que indica todo lo que está fuera de la aplicación. Agregue una expresión de ejemplo en la intención None por cada 10 expresiones de ejemplo del resto de la aplicación de LUIS.

Para obtener más información:
* Concepto: [Comprender cuáles son las expresiones correctas para la aplicación de LUIS](luis-concept-utterance.md)

## <a name="do-leverage-the-suggest-feature-for-active-learning"></a>Aprovechar la característica de sugerencia para un aprendizaje activo

Use la **revisión de las expresiones del punto de conexión** del [aprendizaje activo](luis-how-to-review-endoint-utt.md) de forma habitual, en lugar de agregar más expresiones de ejemplo a las intenciones. Dado que la aplicación recibe de forma constante expresiones de punto de conexión, esta lista crece y cambia.

Para obtener más información:
* Concepto: [Conceptos para habilitar el aprendizaje activo mediante la revisión de expresiones de punto de conexión](luis-concept-review-endpoint-utterances.md)
* Tutorial: [Tutorial: Corrección de predicciones poco seguras mediante la revisión de las expresiones del punto de conexión](luis-tutorial-review-endpoint-utterances.md)
* Procedimiento: [Cómo revisar las expresiones del punto de conexión en el portal de LUIS](luis-how-to-review-endoint-utt.md)

## <a name="do-monitor-the-performance-of-your-app"></a>Supervisar el rendimiento de la aplicación

Supervise la precisión de las predicciones mediante un conjunto de [pruebas de lote](luis-concept-batch-test.md). 

## <a name="dont-add-many-example-utterances-to-intents"></a>Agregar varias expresiones de ejemplo a las intenciones

Una vez publicada la aplicación, agregue solo expresiones de aprendizaje activo en el proceso iterativo. Si las expresiones son demasiado similares, agregue un patrón. 

## <a name="dont-use-luis-as-a-training-platform"></a>No usar LUIS como una plataforma de aprendizaje

LUIS es específico de un dominio de modelo de lenguaje. Esto no está pensado para que funcione como una plataforma general de aprendizaje de lenguaje natural. 

## <a name="dont-add-many-example-utterances-of-the-same-format-ignoring-other-formats"></a>No agregar varias expresiones de ejemplo del mismo formato, omitiendo otros formatos

LUIS espera variaciones en las expresiones de una intención. Las expresiones pueden variar y conservar el mismo significado general. Las variaciones pueden incluir la longitud de la expresión, así como la elección y ubicación de las palabras. 

|No usar el mismo formato|Usar un formato diferente|
|--|--|
|Comprar un billete a Seattle<br>Comprar un billete a París<br>Comprar un billete a Orlando|Comprar 1 billete a Seattle<br>Reservar dos asientos en el vuelo nocturno a París el próximo lunes<br>Quiero adquirir 3 billetes a Orlando para las vacaciones de primavera|

La segunda columna usa diferentes verbos (comprar, reservar y adquirir), distintas cantidades (1, dos y 3) y diferentes colocaciones de las palabras, pero todas tienen la misma intención de comprar billetes de avión para viajar. 

## <a name="dont-mix-the-definition-of-intents-and-entities"></a>No combinar la definición de intenciones y entidades

Cree una intención para cualquier acción que llevará a cabo el bot. Use entidades como parámetros que hacen posible esa acción. 

Para un bot de chat que reservará vuelos, cree una intención **BookFlight**. No cree una intención para cada compañía aérea o destino. Use esos datos como [entidades](luis-concept-entity-types.md) y márquelos en las expresiones de ejemplo. 

## <a name="dont-create-phrase-lists-with-all-the-possible-values"></a>No crear listas de frases con todos los valores posibles

Proporcione algunos ejemplos en las [listas de frases](luis-concept-feature.md), pero no todas las palabras. LUIS generaliza y tiene en cuenta el contexto. 

## <a name="dont-add-many-patterns"></a>No agregar muchos patrones

No agregue demasiados [patrones](luis-concept-patterns.md). LUIS está pensado para aprender rápidamente con pocos ejemplos. No sobrecargue el sistema innecesariamente.

## <a name="dont-train-and-publish-with-every-single-example-utterance"></a>No entrenar y publicar con cada expresión de ejemplo

Agregue 10 o 15 expresiones antes de entrenar y publicar. De esta forma, puede ver el impacto en la precisión de la predicción. Si agrega una única expresión, puede que no vea ningún impacto en la puntuación. 

## <a name="do-use-versions-for-each-app-iteration"></a>Uso de versiones para cada iteración de la aplicación

Cada ciclo de creación debe estar en una nueva [versión](luis-concept-version.md), clonada de una versión existente. LUIS no tiene ningún límite para las versiones. El nombre de una versión se usa como parte de la ruta de la API, por lo que es importante elegir los caracteres permitidos en una dirección URL y usar un máximo de 10 caracteres. Desarrolle una estrategia de nombres para las versiones para mantenerlas organizadas. 

Para obtener más información:
* Concepto: [Comprender cómo y cuándo usar una versión de LUIS](luis-concept-version.md)
* Procedimiento: [Uso de versiones para realizar ediciones y pruebas sin afectar a las aplicaciones de almacenamiento provisional o producción](luis-how-to-manage-versions.md)


## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [planear la aplicación](luis-how-plan-your-app.md) en la aplicación de LUIS.
