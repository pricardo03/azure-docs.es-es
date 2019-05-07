---
title: Tipos de entidades
titleSuffix: Language Understanding - Azure Cognitive Services
description: 'Entidades extraen datos de la declaración. Tipos de entidad le predecible extracción de datos. Hay dos tipos de entidades: aprendido por el equipo y no ha aprendido la máquina. Es importante saber qué tipo de entidad que está trabajando en las grabaciones de voz.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 7fd9ae3ab1f50dc91118ba11bc357a0f6dc0e771
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141044"
---
# <a name="entity-types-and-their-purposes-in-luis"></a>Tipos de entidad y sus propósitos en LUIS

Entidades extraen datos de la declaración. Tipos de entidad le predecible extracción de datos. Hay dos tipos de entidades: aprendido por el equipo y no ha aprendido la máquina. Es importante saber qué tipo de entidad que está trabajando en las grabaciones de voz. 

## <a name="entity-compared-to-intent"></a>Comparación entre entidades e intenciones

La entidad representa una palabra o frase dentro de la expresión que quiere extraer. Una expresión puede incluir varias o ninguna entidad. Una aplicación cliente que necesite la entidad para realizar su tarea o utilizarlo como una guía de varias opciones para presentar al usuario. 

Una entidad:

* Representa una clase incluida una colección de objetos similares (lugares, cosas, personas, eventos o conceptos). 
* Describe la información pertinente a la intención


Por ejemplo, una aplicación de búsqueda de noticias podría incluir entidades como "tema", "fuente", "palabra clave" o "fecha de publicación", que son datos clave para buscar noticias. En una aplicación de reserva de viajes, "ubicación", "fecha", "aerolínea", "clase de viaje" y "billetes" representan información clave para reservar vuelos (relevante para la intención "reservar vuelo").

En comparación, la intención representa la predicción de toda la expresión. 

## <a name="entities-help-with-data-extraction-only"></a>Las entidades solo ayudan con la extracción de datos

Las entidades se marcan o etiquetan solo con el propósito de extracción de entidades, esta acción no ayuda con la predicción de la intención.

## <a name="entities-represent-data"></a>Las entidades representan datos

Las entidades son los datos que quiere extraer de la expresión. Pueden ser un nombre, una fecha, el nombre de un producto o cualquier grupo de palabras. 

|Expresión|Entidad|Datos|
|--|--|--|
|Comprar 3 billetes a Nueva York|Número creado previamente<br>Location.Destination|3<br>Nueva York|
|Comprar un billete de Nueva York a Londres para el 5 de marzo|Location.Origin<br>Location.Destination<br>Entidad datetimeV2 creada previamente|Nueva York<br>Londres<br>5 de marzo de 2018|

## <a name="entities-are-optional-but-highly-recommended"></a>Las entidades son opcionales, pero muy recomendables

Mientras las intenciones son obligatorias, las entidades son opcionales. No es necesario crear entidades para cada concepto en la aplicación, sino solo para los que resulten necesarios para que la aplicación cliente pueda llevar a cabo la operación. 

Si las expresiones no tienen información detallada, el bot deberá continuar (no es necesario agregarlas). A medida que la aplicación crezca, puede ir agregándolas más adelante. 

Si no está seguro de cómo usar la información, agregue algunas entidades comunes creadas previamente, como [datetimeV2](luis-reference-prebuilt-datetimev2.md), [ordinal](luis-reference-prebuilt-ordinal.md), [correo electrónico](luis-reference-prebuilt-email.md) y [número de teléfono](luis-reference-prebuilt-phonenumber.md).

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

En la expresión "Resérvame un billete a París", "París" es una entidad que se refiere a la ubicación. Al reconocer las entidades que se mencionan en la expresión del usuario, LUIS ayuda a la aplicación cliente a elegir las acciones específicas que se deben llevar a cabo para satisfacer la solicitud del cliente.

## <a name="mark-entities-in-none-intent"></a>Marcar entidades en la intención None

Siempre que sea posible, todas las intenciones, incluida la intención **None** (Ninguno), deben tener entidades marcadas. De esta manera, LUIS puede obtener más información sobre la ubicación de las entidades dentro de las expresiones y las palabras que hay alrededor de las entidades. 

## <a name="entity-status-for-predictions"></a>Estado de la entidad para predicciones

El portal de LUIS le indica si la entidad en una declaración de ejemplo es diferente de la entidad marcada o está demasiado cerca de otra y, por tanto, no es clara. Esto se indica con un subrayado rojo en la declaración de ejemplo. 

Para más información, consulte [Predicciones de estado de entidad](luis-how-to-add-example-utterances.md#entity-status-predictions). 

## <a name="types-of-entities"></a>Tipos de entidades

LUIS ofrece muchos tipos de entidades. Elija la entidad en función de cómo se deben extraer los datos y cómo se deben representar una vez que se extraen.

Las entidades se pueden extraer con aprendizaje automático, lo que permite que LUIS siga aprendiendo cómo aparece la entidad en la declaración. Las entidades se pueden extraer sin aprendizaje automático, haciendo coincidir el texto exacto o una expresión regular. Las entidades se pueden extraer de patrones con una implementación mixta. 

Una vez que se extrae la entidad, sus datos pueden representarse como una sola unidad de información o combinados con otras entidades para formar una unidad de información que puede usar la aplicación cliente.

|Con aprendizaje automático|Se puede marcar|Tutorial|Ejemplo<br>Response|Tipo de entidad|Propósito|
|--|--|--|--|--|--|
|✔|✔|[✔](luis-tutorial-composite-entity.md)|[✔](luis-concept-data-extraction.md#composite-entity-data)|[**Compuesta**](#composite-entity)|Agrupación de entidades, independientemente del tipo de entidad.|
|||[✔](luis-quickstart-intent-and-list-entity.md)|[✔](luis-concept-data-extraction.md#list-entity-data)|[**Lista**](#list-entity)|Lista de elementos y sus sinónimos extraída con coincidencia de texto exacta.|
|Mixta||[✔](luis-tutorial-pattern.md)|[✔](luis-concept-data-extraction.md#patternany-entity-data)|[**Pattern.any**](#patternany-entity)|Entidad cuyo final es difícil de determinar.|
|||[✔](luis-tutorial-prebuilt-intents-entities.md)|[✔](luis-concept-data-extraction.md#prebuilt-entity-data)|[**Creada previamente**](#prebuilt-entity)|Entidad entrenada para extraer distintos tipos de datos.|
|||[✔](luis-quickstart-intents-regex-entity.md)|[✔](luis-concept-data-extraction.md#regular-expression-entity-data)|[**Expresión regular**](#regular-expression-entity)|Usa una expresión regular que coincide con el texto.|
|✔|✔|[✔](luis-quickstart-primary-and-secondary-data.md)|[✔](luis-concept-data-extraction.md#simple-entity-data)|[**Simple**](#simple-entity)|Contiene un único concepto en una palabra o frase.|

Solo entidades ha aprendido la máquina deben marcarse como las declaraciones de ejemplo. Las entidades de aprendizaje automático funcionan mejor cuando se prueban con [consultas de punto de conexión](luis-concept-test.md#endpoint-testing) y con [expresiones de punto de conexión de revisión](luis-how-to-review-endoint-utt.md). 

Las entidades Pattern.Any deben marcarse en los ejemplos de la plantilla de [patrón](luis-how-to-model-intent-pattern.md), y no en los ejemplos de la intención del usuario. 

Las entidades mixtas usan una combinación de métodos de detección de entidades.

## <a name="composite-entity"></a>Entidad compuesta

Una entidad compuesta se compone de otras entidades, como entidades precompiladas, simple, expresión regular y las entidades de la lista. Las entidades independientes forman una entidad completa. 

Esta entidad es la opción ideal cuando los datos:

* Están relacionados entre sí. 
* Se relacionan entre sí en el contexto de la expresión.
* Usan una variedad de tipos de entidad.
* Deben agruparse y la aplicación cliente debe procesarlos como una unidad de información.
* Tienen una variedad de expresiones de usuario que requieren aprendizaje automático.

![entidad compuesta](./media/luis-concept-entities/composite-entity.png)

[Tutorial](luis-tutorial-composite-entity.md)<br>
[Respuesta JSON de ejemplo de entidad](luis-concept-data-extraction.md#composite-entity-data)<br>

## <a name="list-entity"></a>Entidad de lista

Las entidades de lista representan un conjunto fijo y cerrado de palabras relacionadas y sus sinónimos. LUIS no detecta valores adicionales para las entidades de lista. Use la característica **Recommend** (Recomendar) para ver sugerencias de palabras nuevas en función de la lista actual. Si hay más de una entidad de lista con el mismo valor, se devolverá cada entidad en la consulta de punto de conexión. 

La entidad es la opción ideal cuando los datos de texto:

* Son un conjunto conocido.
* El conjunto no excede los [límites](luis-boundaries.md) máximos de LUIS para este tipo de entidad.
* El texto de la expresión es una coincidencia exacta con un sinónimo o el nombre canónico. LUIS no usa la lista más allá de las coincidencias exactas de texto. La lematización, los plurales y otras variaciones no se resuelven con una entidad de lista. Para administrar las variaciones, considere el uso de un [patrón](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) con la sintaxis de texto opcional.

![entidad de lista](./media/luis-concept-entities/list-entity.png)

[Tutorial](luis-quickstart-intent-and-list-entity.md)<br>
[Respuesta JSON de ejemplo de entidad](luis-concept-data-extraction.md#list-entity-data)

## <a name="patternany-entity"></a>Entidad Pattern.any

Pattern.any es un marcador de posición de longitud variable que solo se usa en la expresión de plantilla de un patrón para marcar dónde empieza y acaba la entidad.  

La entidad es la opción ideal cuando:

* El final de la entidad se puede confundir con el resto del texto de la expresión. 
[Tutorial](luis-tutorial-pattern.md)<br>
[Respuesta JSON de ejemplo de entidad](luis-concept-data-extraction.md#patternany-entity-data)

**Ejemplo**  
Dada una aplicación cliente que busca libros en función del título, pattern.any extrae el título completo. `Was {BookTitle} written by an American this year[?]` es una expresión de plantilla que usa pattern.any para este libro. 

En la tabla siguiente, cada fila tiene dos versiones de la declaración. La declaración en la parte superior es cómo verá LUIS la declaración inicialmente, donde no está claro dónde comienza ni termina el título. La declaración en la parte inferior es cómo sabrá LUIS cuál es el título del libro cuando exista un patrón para la extracción. 

|Expresión|
|--|
|¿' Era el hombre que ha confundido con su esposa para un Hat y otras historias clínicas escritos por un American este año?<br>¿El libro **El hombre que confundió a su mujer con un sombrero** lo escribió un autor americano este año?|
|`Was Half Asleep in Frog Pajamas written by an American this year?`<br>`Was **Half Asleep in Frog Pajamas** written by an American this year?`|
|`Was The Particular Sadness of Lemon Cake: A Novel written by an American this year?`<br>`Was **The Particular Sadness of Lemon Cake: A Novel** written by an American this year?`|
|`Was There's A Wocket In My Pocket! written by an American this year?`<br>`Was **There's A Wocket In My Pocket!** written by an American this year?`|

## <a name="prebuilt-entity"></a>Entidad creada previamente

Las entidades creadas previamente son tipos integrados que representan conceptos comunes, como el número de teléfono, la dirección URL y el correo electrónico. Los nombres de las entidades creadas previamente están reservados. [Todas las entidades creadas previamente](luis-prebuilt-entities.md) que se agregan a la aplicación se devuelven en la consulta de un punto de conexión de predicción si se encuentran en la expresión. 

La entidad es la opción ideal cuando:

* Los datos coinciden con un caso de uso común compatible con las entidades creadas previamente para su referencia cultural del idioma. 

Las entidades precompiladas se pueden agregar y quitar en cualquier momento.

![Entidad creada previamente de número](./media/luis-concept-entities/number-entity.png)

[Tutorial](luis-tutorial-prebuilt-intents-entities.md)<br>
[Respuesta JSON de ejemplo de entidad](luis-concept-data-extraction.md#prebuilt-entity-data)

Algunas de estas entidades precompiladas se definen en el proyecto de código abierto [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). Si su referencia cultural o entidad específica no se admite actualmente, colabore en el proyecto. 

### <a name="troubleshooting-prebuilt-entities"></a>Solución de problemas de entidades precompiladas

En el portal de LUIS, si se marca una entidad creada previamente en lugar de una entidad personalizada, tiene algunas opciones de cómo solucionar este problema.

Las entidades creadas previamente agregadas a la aplicación le _siempre_ devolverse, incluso si la declaración debe extraer entidades personalizadas para el mismo texto. 

#### <a name="change-tagged-entity-in-example-utterance"></a>Cambiar entidad etiquetado en la declaración de ejemplo

Si la entidad creada previamente es el mismo texto o símbolos (tokens) como la entidad personalizada, seleccione el texto de la declaración de ejemplo y cambie la declaración etiquetada. 

Si la entidad creada previamente se etiqueta con más texto o símbolos (tokens) de la entidad personalizada, tiene un par de opciones de cómo solucionar este problema:

* [Quite la declaración de ejemplo](#remove-example-utterance-to-fix-tagging) (método)
* [Quitar entidad creada previamente](#remove-prebuilt-entity-to-fix-tagging) (método)

#### <a name="remove-example-utterance-to-fix-tagging"></a>Quite la declaración de ejemplo para corregir el etiquetado 

La primera opción es quitar la declaración de ejemplo. 

1. Elimine la declaración de ejemplo.
1. Reciclaje de la aplicación. 
1. Vuelva a agregar únicamente la palabra o frase que es la entidad, que está marcada como una entidad creada previamente, como una declaración de ejemplo completo. La palabra o frase seguirá disponiendo de la entidad creada previamente marcada. 
1. Seleccione la entidad en la declaración de ejemplo en el **intención** página y cambiar a una entidad personalizada y volver a entrenar. Esto evitará que LUIS marcar este texto exactamente como la entidad creada previamente en las grabaciones de voz de ejemplo que usan ese texto. 
1. Agregue la declaración de ejemplo original completo a la intención. La entidad personalizada debe continuar marcarse en lugar de la entidad creada previamente. Si no se marca la entidad personalizada, deberá agregar más ejemplos de ese texto en las grabaciones de voz.

#### <a name="remove-prebuilt-entity-to-fix-tagging"></a>Quitar entidad creada previamente para corregir el etiquetado

1. Quitar la entidad creada previamente desde la aplicación. 
1. En el **intención** página, marque la entidad personalizada en la declaración de ejemplo.
1. Entrene la aplicación.
1. Agrega la entidad creada previamente a la aplicación y entrenar la aplicación. Esta corrección se da por supuesto que la entidad creada previamente no forma parte de una entidad compuesta.

## <a name="regular-expression-entity"></a>Entidad de expresión regular 

Una expresión regular es mejor para el texto de enunciado sin formato. No distingue entre mayúsculas y minúsculas e ignora la variante cultural.  La coincidencia de expresiones regulares se aplica después de las modificaciones de la ortografía en los caracteres, no en el nivel de token. Si la expresión regular es demasiado compleja (por ejemplo, uso excesivo de corchetes), no podrá agregar la expresión al modelo. Usa el elemento, pero no todos los [.NET Regex](https://docs.microsoft.com/dotnet/standard/base-types/regular-expressions) biblioteca. 

La entidad es la opción ideal cuando:

* Los datos tienen un formato coherente con cualquier variación que también sea coherente.
* La expresión regular no necesita más de 2 niveles de anidamiento. 

![Entidad de expresión regular](./media/luis-concept-entities/regex-entity.png)

[Tutorial](luis-quickstart-intents-regex-entity.md)<br>
[Respuesta JSON de ejemplo de entidad](luis-concept-data-extraction.md#regular-expression-entity-data)<br>

## <a name="simple-entity"></a>Entidad simple 

Una entidad simple es una entidad genérica que describe un concepto único que se ha aprendido en el contexto de aprendizaje automático. Dado que las entidades simples suelen ser nombres, como por ejemplo, nombres de compañías, de productos u otras categorías de nombres, agregue un [lista de frases](luis-concept-feature.md) cuando se use una entidad simple para aumentar la señal de los nombres usados. 

La entidad es la opción ideal cuando:

* Los datos no tienen un formato coherente, pero indican lo mismo. 

![entidad simple](./media/luis-concept-entities/simple-entity.png)

[Tutorial](luis-quickstart-primary-and-secondary-data.md)<br/>
[Respuesta de ejemplo de entidad](luis-concept-data-extraction.md#simple-entity-data)<br/>

## <a name="entity-limits"></a>Límites de entidad

Consulte los [límites](luis-boundaries.md#model-boundaries) para saber cuántas entidades de cada tipo puede agregar a un modelo.

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Si necesita más del número máximo de entidades 

Es posible que deba usar entidades compuestas en combinación con los roles de la entidad.

Las entidades compuestas representan partes de un total. Por ejemplo, una entidad compuesta denominada PedidoBilleteAvión podría tener las entidades secundarias Aerolínea, Destino, CiudadSalida, FechaSalida y ClaseBilleteAvión.

LUIS también proporciona el tipo de entidad de lista que no es de aprendizaje automático, pero permite a la aplicación de LUIS especificar una lista fija de valores. Vea la referencia de [LUIS Boundaries](luis-boundaries.md) (Límites de LUIS) para revisar los límites del tipo de entidad de lista. 

Si se han considerado estas entidades y todavía necesita algo más que el límite, póngase en contacto con soporte técnico. Para ello, recopile información detallada sobre el sistema, vaya al sitio web de [LUIS](luis-reference-regions.md#luis-website) y seleccione **Support** (Soporte). Si la suscripción a Azure incluye servicios de soporte técnico, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/options/). 

## <a name="next-steps"></a>Pasos siguientes

Aprenda conceptos sobre las buenas [expresiones](luis-concept-utterance.md). 

Vea [Add entities](luis-how-to-add-entities.md) (Agregar entidades) para obtener más información sobre cómo agregar entidades a la aplicación de LUIS.
