---
title: Ayuda a la predicción de los patrones
titleSuffix: Language Understanding - Azure Cognitive Services
description: Un patrón permite lograr más precisión en una intención sin proporcionar muchas más expresiones.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 03/05/2019
ms.author: diberry
ms.openlocfilehash: 42ac75b6ed0d4489ccae014b9cfe3b08269c1218
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57547425"
---
# <a name="patterns-improve-prediction-accuracy"></a>Los patrones mejoran la precisión de las predicciones
Los patrones están diseñados para mejorar la precisión cuando varias expresiones son muy parecidas.  Un patrón permite lograr más precisión en una intención sin proporcionar muchas más expresiones. 

## <a name="patterns-solve-low-intent-confidence"></a>Los patrones resuelven la confianza con intención baja
Piense en una aplicación de recursos humanos que informa sobre el organigrama en relación con un empleado. Dados el nombre y la relación del empleado, LUIS devuelve los empleados implicados. Considere un empleado, Tom, con una jefa llamada Alice, y un equipo de subalternos llamados: Michael, Rebecca y Carl.

![Imagen de organigrama](./media/luis-concept-patterns/org-chart.png)

|Grabaciones de voz|Intención pronosticada|Puntuación de intención|
|--|--|--|
|¿Quién es el subalterno de Tom?|GetOrgChart|.30|
|¿Quién es el subalterno de Tom?|GetOrgChart|.30|

Si una aplicación tiene entre 10 y 20 expresiones con distintas longitudes de oraciones, diferente orden de palabras, e incluso diferentes palabras (sinónimos de "subalterno", "administrar", "informe"), LUIS puede devolver una puntuación de confianza baja. Crear un patrón para ayudar a LUIS a comprender la importancia de la orden de palabras. 

Los patrones solucionan las situaciones siguientes: 

* La puntuación de la intención es baja
* La intención correcta no es la puntuación de superior pero demasiado cerca de la puntuación de superior. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Los patrones no son una garantía de intención
Los patrones emplean una combinación de tecnologías de predicción. Establecer una intención para una expresión de plantilla en un patrón no es una garantía de la predicción de la intención, pero es un buen indicio. 

<a name="patterns-do-not-improve-entity-detection"/>

## <a name="patterns-do-not-improve-machine-learned-entity-detection"></a>Patrones no mejoran la detección de la entidad ha aprendido la máquina

Un patrón está pensado principalmente para ayudar a la predicción de intenciones y roles. La entidad pattern.any se usa para extraer las entidades de forma libre. Mientras que los patrones utilizan las entidades, un patrón no ayuda a detectar una entidad que ha aprendido la máquina.  

No espere ver una predicción de entidad mejorada si contrae varias expresiones en un patrón único. Para que las entidades simples se activen, tendrá que agregar expresiones o usar entidades de la lista, de lo contrario no se activará el patrón.

## <a name="patterns-use-entity-roles"></a>Los patrones usan roles de entidad
Si dos o más entidades de un patrón están relacionadas por el contexto, los patrones usan los [roles](luis-concept-roles.md) de entidad para extraer información contextual sobre las entidades. Esto es equivalente a los elementos secundarios de entidades jerárquicas, pero **solo** está disponible en los patrones. 

## <a name="prediction-scores-with-and-without-patterns"></a>Puntuaciones de predicción con y sin patrones
Dadas suficientes expresiones de ejemplo, LUIS podría aumentar la confianza de la predicción sin patrones. Los patrones aumentan la puntuación de confianza sin tener que proporcionar tantas expresiones.  

## <a name="pattern-matching"></a>Coincidencia de patrones
La coincidencia con un patrón se basa en primer lugar en la detección de entidades dentro del patrón, para posteriormente validar el resto de las palabras y el orden de las palabras del patrón. Para que un patrón coincida, se necesitan las entidades en el patrón. El patrón se aplica a nivel de token, no a nivel de carácter. 

## <a name="pattern-syntax"></a>Sintaxis del patrón
La sintaxis del patrón es una plantilla para una expresión. La plantilla debe contener las palabras y entidades que quiere que coincidan, así como las palabras y los signos de puntuación que quiere omitir. **No** es una expresión regular. 

Las entidades en los patrones aparecen entre llaves, `{}`. Los patrones pueden incluir entidades y entidades con roles. [Pattern.Any](luis-concept-entity-types.md#patternany-entity) es una entidad solo se usan en patrones. 

Sintaxis de patrón es compatible con la sintaxis siguiente:

|Función|Sintaxis|Nivel de anidamiento|Ejemplo|
|--|--|--|--|
|Entidad| {} -entre llaves|2|¿Dónde está el formulario {entity-name}?|
|opcional|[] - corchetes<BR><BR>Hay un límite de 3 niveles de anidamiento de cualquier combinación de opcionales y agrupación |2|El signo de interrogación es opcional [?]|
|agrupación|() - paréntesis|2|es (un \| b).|
|o| \| -vertical de la barra (canalización)<br><br>Hay un límite de 2 en las barras verticales (o) en un grupo |-|Donde es el formulario ({nombre de forma corta} &#x7c; {nombre de formulario largo} &#x7c; {número de formulario})| 
|principio o final de utterance (dictado)|^: símbolo de intercalación|-|^ empezar la declaración<br>se realiza la declaración ^<br>^ strict coincidencia literal de utterance todo con la entidad {number} ^|

## <a name="nesting-syntax-in-patterns"></a>Sintaxis de anidamiento en patrones

El **opcional** sintaxis con corchetes, pueden ser de dos niveles anidados. Por ejemplo: `[[this]is] a new form`. En este ejemplo se permite para las declaraciones siguientes: 

|Ejemplo de declaración opcionales anidados|Explicación|
|--|--|
|se trata de un nuevo formulario|coincide con todas las palabras en el patrón|
|es un nuevo formulario|coincide con la palabra opcional externa y palabras que no es opcional en el patrón|
|un nuevo formulario|sólo palabras coincidencias necesarias|

El **agrupación** sintaxis con paréntesis, pueden ser de dos niveles anidados. Por ejemplo: `(({Entity1.RoleName1} | {Entity1.RoleName2} ) | {Entity2} )`. Esto permite que cualquiera de las tres entidades que se debe coincidir. 

Si Entity1 es una ubicación con funciones como origen (Seattle) y de destino (Cairo) y 2 de la entidad es un nombre conocido de creación de una entidad de la lista (RedWest-C), las declaraciones siguientes se asignarían a este patrón:

|Ejemplo de agrupación anidadas utterance (dictado)|Explicación|
|--|--|
|RedWest-C|coincide con la entidad de grupo externo|
|Seattle|coincide con una de las entidades de agrupación interno|
|El Cairo|coincide con una de las entidades de agrupación interno|

## <a name="nesting-limits-for-groups-with-optional-syntax"></a>Límites de anidamiento de grupos con una sintaxis opcional

Una combinación de **agrupación** con **opcional** sintaxis tiene un límite de 3 niveles de anidamiento.

|Permitida|Ejemplo|
|--|--|
|Sí|([(test1 &#x7c; test2)] &#x7c; test3)|
|Sin |( [ ( [ test1 ] &#x7c; test2 ) ] &#x7c; test3 )|

## <a name="nesting-limits-for-groups-with-or-ing-syntax"></a>Límites de anidamiento para grupos con sintaxis o ing

Una combinación de **agrupación** con **o ing** sintaxis tiene un límite de 2 barras verticales.

|Permitida|Ejemplo|
|--|--|
|Sí|(test1 &#x7c; test2 &#x7c; (test3 &#x7c; test4))|
|Sin |(test1 &#x7c; test2 &#x7c; test3 &#x7c; (test4 &#x7c; test5)) |

## <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Sintaxis para agregar una entidad a una plantilla de patrón
Para agregar una entidad a la plantilla de patrón, incluya el nombre de la entidad entre llaves, por ejemplo, `Who does {Employee} manage?`. 

|Patrón con entidad|
|--|
|`Who does {Employee} manage?`|

## <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Sintaxis para agregar una entidad y un rol a una plantilla de patrón
Un rol de entidad se representa como `{entity:role}`, donde el nombre de la entidad va seguido por un signo de dos puntos y, a continuación, el nombre del rol. Para agregar una entidad con un rol a la plantilla de patrón, incluya el nombre de la entidad y el nombre del rol entre llaves, por ejemplo, `Book a ticket from {Location:Origin} to {Location:Destination}`. 

|Patrón con roles de entidad|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

## <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Sintaxis para agregar pattern.any a una plantilla de patrón
La entidad pattern.any permite agregar una entidad de longitud variable al patrón. Mientras se siga la plantilla de patrón, pattern.any puede tener cualquier longitud. 

Para agregar una entidad **pattern.any** a la plantilla de patrón, incluya dicha entidad entre llaves, por ejemplo, `How much does {Booktitle} cost and what format is it available in?`.  

|Patrón con entidad Pattern.any|
|--|
|`How much does {Booktitle} cost and what format is it available in?`|

|Títulos de libros en el patrón|
|--|
|¿Cuánto cuesta **robar este libro** y en qué formato está disponible?|
|¿Cuánto cuesta **preguntar** y en qué formato está disponible?|
|¿Cuánto cuesta **El curioso incidente del perro en la noche** y en qué formato está disponible?| 

Las palabras del título del libro no resultan confusas para LUIS ya sabe que LUIS donde finaliza el título del libro, en función de la entidad Pattern.any.

## <a name="explicit-lists"></a>Listas explícitas

crear un [lista explícita](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) a través de la API de creación para permitir la excepción cuando:

* El patrón contiene un [Pattern.any](luis-concept-entity-types.md#patternany-entity)
* Y esa sintaxis patrón permite la posibilidad de una extracción de entidades correcto basándose en la declaración. 

Por ejemplo, suponga que tiene un patrón que contiene tanto la sintaxis opcional, `[]`, como la sintaxis de la entidad, `{}`, combinadas para extraer datos de forma incorrecta.

Considere el patrón `[find] email about {subject} [from {person}]'.

En las siguientes expresiones, las entidades **subject** y **person** se extraen de forma correcta e incorrecta:

|Expresión|Entidad|Extracción correcta|
|--|--|:--:|
|email about dogs from Chris|subject=dogs<br>person=Chris|✔|
|email about the man from La Mancha|subject=the man<br>person=La Mancha|X|

En la tabla anterior, debe ser el sujeto `the man from La Mancha` (un título de libro), pero dado que el asunto incluye la palabra opcional `from`, el título se predice incorrectamente. 

Para corregir esta excepción al patrón, agregue `the man from la mancha` como una coincidencia de lista explícita de la entidad {subject} mediante la [API de creación de lista explícita](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8).

## <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Sintaxis para marcar texto opcional en una expresión de plantilla
Marque texto opcional en la expresión mediante la sintaxis de corchetes de expresión regular, `[]`. El texto opcional solo puede anidar dos corchetes.

|Patrón con texto opcional|Significado|
|--|--|
|`[find] email about {subject} [from {person}]`|`find` y `from {person}` son opcionales|
|' Puede ayudarme [?]|El signo de puntuación es opcional|

Signos de puntuación (`?`, `!`, `.`) se deben omitir y deberá omitir usando la sintaxis de corchete en patrones. 

## <a name="pattern-only-apps"></a>Aplicaciones solo patrón
Puede crear una aplicación con las intenciones que no tengan ninguna grabaciones de voz de ejemplo, siempre y cuando hay un patrón para cada intento. Para una aplicación solo patrón, el patrón no debe contener entidades ha aprendido la máquina porque requieren grabaciones de voz de ejemplo. 

## <a name="best-practices"></a>Procedimientos recomendados
Obtenga información sobre los [procedimientos recomendados](luis-concept-best-practices.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprenda cómo implementar patrones en este tutorial](luis-tutorial-pattern.md)
