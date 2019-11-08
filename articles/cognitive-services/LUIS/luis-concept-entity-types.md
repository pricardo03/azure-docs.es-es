---
title: 'Tipos de entidades: LUIS'
titleSuffix: Azure Cognitive Services
description: 'Las entidades extraen datos de la expresión. Los tipos de entidad realizan una extracción predecible de datos. Hay dos tipos de entidades: con aprendizaje automático y sin aprendizaje automático. Es importante saber qué tipo de entidad se usa en las expresiones.'
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/25/2019
ms.author: diberry
ms.openlocfilehash: 9dc26e50e1c0f43e816e422f0fee91a246ea04a9
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73487604"
---
# <a name="entities-and-their-purpose-in-luis"></a>Entidades y su finalidad en LUIS

El objetivo principal de las entidades es proporcionar a la aplicación cliente la extracción predecible de los datos. Un propósito _opcional_ y secundario es impulsar la predicción de la intención con los descriptores. 

Existen dos tipos de entidades: 

* Con aprendizaje automático: desde el contexto
* Sin aprendizaje automático: para coincidencias de texto exactas

Comience siempre con una entidad con aprendizaje automático porque proporciona la gama más amplia de opciones de extracción de datos.

## <a name="entity-compared-to-intent"></a>Comparación entre entidades e intenciones

La entidad representa un concepto de datos dentro de la expresión que quiere extraer. 

Considere las tres expresiones siguientes:

|Expresión|Datos extraídos|Explicación|
|--|--|--|
|`Help`|-|No hay nada que extraer.|
|`Send Bob a present`|Bob, regalo|Bob es definitivamente importante para completar la tarea. El regalo puede ser suficiente información o el bot puede necesitar aclarar cuál es el regalo con una pregunta de seguimiento.|
|`Send Bob a box of chocolates.`|Los dos fragmentos importantes de datos, Bob y la caja de chocolate, son importantes para completar la solicitud del usuario.|

Una expresión puede incluir varias o ninguna entidad. _Es posible_ que una aplicación cliente necesite la entidad para realizar su tarea. 

En comparación, se _requiere_ la predicción de la intención para una expresión y representa toda la expresión. LUIS requiere que las expresiones de ejemplo estén contenidas en una intención. Si la intención principal de la expresión no es importante para la aplicación cliente, agregue todas las expresiones a la intención None. 

Si más adelante en el ciclo de vida de la aplicación quiere dividir la expresión, puede hacerlo fácilmente. Esto puede ser para organizar las expresiones mientras se crea, o puede ser para usar la intención prevista en la aplicación cliente. 

No es necesario utilizar la intención prevista en la aplicación cliente, pero se devuelve como parte de la respuesta del punto de conexión de la predicción.

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

## <a name="design-entities-for-decomposition"></a>Diseño de entidades para la descomposición

Comience el diseño de la entidad con una entidad con aprendizaje automático. Esto permite un fácil crecimiento del diseño y cambios de la entidad a lo largo del tiempo. Agregue **subcomponentes** (entidades secundarias) con **restricciones** y **descriptores** para completar el diseño de la entidad. 

El diseño de la descomposición permite a LUIS devolver un grado profundo de resolución de la entidad a la aplicación cliente. Esto permite a la aplicación cliente centrarse en reglas de negocios y dejar a LUIS la resolución de datos.

### <a name="machine-learned-entities-are-primary-data-collections"></a>Las entidades con aprendizaje automático son colecciones de datos principales.

Las entidades con aprendizaje automático constituyen la unidad de datos de nivel superior. Los subcomponentes son entidades secundarias de las entidades con aprendizaje automático. 

Las **restricciones** son entidades que coinciden con el texto exacto y que aplican reglas para identificar y extraer datos. Los **descriptores** son características aplicadas para aumentar la relevancia de las palabras o frases de la predicción.

<a name="composite-entity"></a>
<a name="list-entity"></a>
<a name="patternany-entity"></a>
<a name="prebuilt-entity"></a>
<a name="regular-expression-entity"></a>
<a name="simple-entity"></a>

## <a name="types-of-entities"></a>Tipos de entidades

Elija la entidad en función de cómo se deben extraer los datos y cómo se deben representar una vez que se extraen.

|Tipo de entidad|Propósito|
|--|--|
|[**Con aprendizaje automático**](#composite-entity)|Agrupación primaria de entidades, independientemente del tipo de entidad. Las entidades con aprendizaje automático aprenden del contexto de la expresión. Esto hace que la variación de la colocación de las expresiones en el ejemplo sea importante. |
|[**Lista**](#list-entity)|Lista de elementos y sus sinónimos extraída con **coincidencia de texto exacta**.|
|[**Pattern.any**](#patternany-entity)|Entidad cuyo final es difícil de determinar. |
|[**Creada previamente**](#prebuilt-entity)|Ya entrenado para extraer un tipo específico de datos, como la dirección URL o el correo electrónico. Algunas de estas entidades precompiladas se definen en el proyecto de código abierto [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text). Si su referencia cultural o entidad específica no se admite actualmente, colabore en el proyecto.|
|[**Expresión regular**](#regular-expression-entity)|Usa una expresión regular para la **coincidencia de texto exacta**.|

### <a name="entity-role-defines-context"></a>El rol de la entidad define el contexto.

El rol de una entidad es el alias con nombre basado en el contexto dentro de la expresión. Un ejemplo es una expresión para reservar un vuelo que tiene dos ubicaciones, origen y destino.

`Book a flight from Seattle to Cairo`

Deben extraerse los dos ejemplos de una entidad `location`. La aplicación cliente tiene que conocer el tipo de ubicación de cada una de ellas para completar la compra de entradas. La entidad `location` necesita dos roles de `origin` y `destination`, y ambos deben marcarse en las expresiones de ejemplo. 

Si LUIS encuentra `location` pero no puede determinar el rol, se seguirá devolviendo la entidad de ubicación. La aplicación cliente tendría que hacer un seguimiento con una pregunta para determinar qué tipo de ubicación quería decir el usuario. 

Puede haber varias entidades en una expresión y extraerse sin usar roles. Si el contexto de la frase indica el valor de la entidad, se debe usar un rol.

Si la expresión incluye una lista de ubicaciones, `I want to travel to Seattle, Cairo, and London.`, esta es una lista donde cada elemento no tiene un significado adicional. 

## <a name="if-you-need-more-than-the-maximum-number-of-entities"></a>Si necesita más del número máximo de entidades 

Si necesita más que el límite, póngase en contacto con el soporte técnico. Para ello, recopile información detallada sobre el sistema, vaya al sitio web de [LUIS](luis-reference-regions.md#luis-website) y seleccione **Support** (Soporte). Si la suscripción a Azure incluye servicios de soporte técnico, póngase en contacto con el [soporte técnico de Azure](https://azure.microsoft.com/support/options/). 

## <a name="entity-prediction-status"></a>Estado de predicción de entidades

El portal de LUIS muestra cuando la entidad, en una expresión de ejemplo, tiene una predicción de entidad diferente a la que se ha seleccionado. Esta puntuación diferente se basa en el modelo entrenado actual. 

## <a name="next-steps"></a>Pasos siguientes

Aprenda conceptos sobre las buenas [expresiones](luis-concept-utterance.md). 

Vea [Add entities](luis-how-to-add-entities.md) (Agregar entidades) para obtener más información sobre cómo agregar entidades a la aplicación de LUIS.
