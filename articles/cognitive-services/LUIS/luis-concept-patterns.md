---
title: Cómo los patrones aumentan la precisión de las predicciones
titleSuffix: Azure Cognitive Services
description: Los patrones están diseñados para mejorar la precisión cuando varias expresiones son muy parecidas. Un patrón permite lograr más precisión en una intención sin proporcionar muchas más expresiones.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 09c869bf28b804d8fabe331c4a9c2d222accc1e5
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300377"
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

Si una aplicación tiene entre 10 y 20 expresiones con distintas longitudes de oraciones, diferente orden de palabras, e incluso diferentes palabras (sinónimos de "subalterno", "administrar", "informe"), LUIS puede devolver una puntuación de confianza baja. Para ayudar a LUIS a comprender la importancia del orden de las palabras, cree un patrón. 

Los patrones solucionan las situaciones siguientes: 

* Cuando la puntuación de la intención es baja
* Cuando la intención correcta no es la puntuación más alta, pero se acerca demasiado a ella. 

## <a name="patterns-are-not-a-guarantee-of-intent"></a>Los patrones no son una garantía de intención
Los patrones emplean una combinación de tecnologías de predicción. Establecer una intención para una expresión de plantilla en un patrón no es una garantía de la predicción de la intención, pero es un buen indicio. 

## <a name="patterns-do-not-improve-entity-detection"></a>Los patrones no mejoran la detección de entidades
Aunque los patrones requieren entidades, no ayudan a detectar la entidad. La única finalidad de un patrón es ayudar a la predicción con intenciones y roles.  

## <a name="patterns-use-entity-roles"></a>Los patrones usan roles de entidad
Si dos o más entidades de un patrón están relacionadas por el contexto, los patrones usan los [roles](luis-concept-roles.md) de entidad para extraer información contextual sobre las entidades. Esto es equivalente a los elementos secundarios de entidades jerárquicas, pero **solo** está disponible en los patrones. 

## <a name="prediction-scores-with-and-without-patterns"></a>Puntuaciones de predicción con y sin patrones
Dadas suficientes expresiones de ejemplo, LUIS podría aumentar la confianza de la predicción sin patrones. Los patrones aumentan la puntuación de confianza sin tener que proporcionar tantas expresiones.  

## <a name="pattern-matching"></a>Coincidencia de patrones
La coincidencia con un patrón se basa en primer lugar en la detección de entidades dentro del patrón, para posteriormente validar el resto de las palabras y el orden de las palabras del patrón. Para que un patrón coincida, se necesitan las entidades en el patrón. 

## <a name="pattern-syntax"></a>Sintaxis del patrón
La sintaxis del patrón es una plantilla para una expresión. La plantilla debe contener las palabras y entidades que quiere que coincidan, así como las palabras y los signos de puntuación que quiere omitir. **No** es una expresión regular. 

Las entidades en los patrones aparecen entre llaves, `{}`. Los patrones pueden incluir entidades y entidades con roles. Pattern.any es una entidad que solo se usa en los patrones. En las siguientes secciones se explica la sintaxis.

### <a name="syntax-to-add-an-entity-to-a-pattern-template"></a>Sintaxis para agregar una entidad a una plantilla de patrón
Para agregar una entidad a la plantilla de patrón, incluya el nombre de la entidad entre llaves, por ejemplo, `Who does {Employee} manage?`. 

|Patrón con entidad|
|--|
|`Who does {Employee} manage?`|

### <a name="syntax-to-add-an-entity-and-role-to-a-pattern-template"></a>Sintaxis para agregar una entidad y un rol a una plantilla de patrón
Un rol de entidad se representa como `{entity:role}`, donde el nombre de la entidad va seguido por un signo de dos puntos y, a continuación, el nombre del rol. Para agregar una entidad con un rol a la plantilla de patrón, incluya el nombre de la entidad y el nombre del rol entre llaves, por ejemplo, `Book a ticket from {Location:Origin} to {Location:Destination}`. 

|Patrón con roles de entidad|
|--|
|`Book a ticket from {Location:Origin} to {Location:Destination}`|

### <a name="syntax-to-add-a-patternany-to-pattern-template"></a>Sintaxis para agregar pattern.any a una plantilla de patrón
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

En estos ejemplos de títulos de libros, las palabras contextuales del título del libro no resultan confusas para LUIS. LUIS sabe que el título del libro finaliza porque está en un patrón y marcado con una entidad pattern.any.

### <a name="explicit-lists"></a>Listas explícitas
Si el patrón contiene una entidad pattern.any, y la sintaxis del patrón permite la posibilidad de una extracción incorrecta de entidades basada en la expresión, cree una [lista explícita](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8) mediante la API de creación para permitir la excepción. 

Por ejemplo, suponga que tiene un patrón que contiene tanto la sintaxis opcional, `[]`, como la sintaxis de la entidad, `{}`, combinadas para extraer datos de forma incorrecta.

Considere el patrón `[find] email about {subject} [from {person}]'. En las siguientes expresiones, las entidades **subject** y **person** se extraen de forma correcta e incorrecta:

|Expresión|Entidad|Extracción correcta|
|--|--|:--:|
|email about dogs from Chris|subject=dogs<br>person=Chris|✔|
|email about the man from La Mancha|subject=the man<br>person=La Mancha|X|

En la tabla anterior, en la expresión `email about the man from La Mancha`, el asunto debería ser `the man from La Mancha` (el título de un libro), pero como el asunto incluye la palabra opcional `from`, el título se predice de forma incorrecta. 

Para corregir esta excepción al patrón, agregue `the man from la mancha` como una coincidencia de lista explícita de la entidad {subject} mediante la [API de creación de lista explícita](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5ade550bd5b81c209ce2e5a8).

### <a name="syntax-to-mark-optional-text-in-a-template-utterance"></a>Sintaxis para marcar texto opcional en una expresión de plantilla
Marque texto opcional en la expresión mediante la sintaxis de corchetes de expresión regular, `[]`. El texto opcional solo puede anidar dos corchetes.

|Patrón con texto opcional|
|--|
|`[find] email about {subject} [from {person}]`|

Los signos de puntuación como `.`, `!` y `?` se pueden omitir mediante los corchetes. Para omitir estos signos, cada signo debe estar en un patrón aparte. La sintaxis opcional no admite actualmente la omisión de un elemento de una lista de varios elementos.

## <a name="patterns-only"></a>Solo patrones
LUIS permite una aplicación sin ninguna expresión de ejemplo en la intención. Este uso solo se permite si se usan patrones. Los patrones requieren al menos una entidad en cada patrón. En aplicaciones de un único patrón, el patrón no debe contener entidades de aprendizaje automático porque estas no requieren expresiones de ejemplo. 

## <a name="best-practices"></a>Procedimientos recomendados
Obtenga información sobre los [procedimientos recomendados](luis-concept-best-practices.md).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aprenda cómo implementar patrones en este tutorial](luis-tutorial-pattern.md)