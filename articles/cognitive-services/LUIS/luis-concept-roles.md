---
title: 'Roles para entidades: LUIS'
titleSuffix: Azure Cognitive Services
description: Los roles son subtipos contextuales con nombre de una entidad que solo se usa en patrones. Por ejemplo, en la expresión `buy a ticket from New York to London`, New York (Nueva York) y London (Londres) son ciudades, pero cada una tiene un significado diferente en la frase. New York (Nueva York) es la ciudad de origen y London (Londres) es la de destino.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/03/2019
ms.author: diberry
ms.openlocfilehash: eead9e0fec8ac4322bc7816de4a4774f8be8129c
ms.sourcegitcommit: 267a9f62af9795698e1958a038feb7ff79e77909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2019
ms.locfileid: "70257967"
---
# <a name="entity-roles-for-contextual-subtypes"></a>Roles de entidad para subtipos contextuales

Los roles permiten que las entidades tengan subtipos con nombre. Un rol se puede usar con cualquier tipo de entidad precompilada o personalizada, y tanto en patrones como en expresiones de ejemplo. 

<a name="example-role-for-entities"></a>
<a name="roles-with-prebuilt-entities"></a>

## <a name="machine-learned-entity-example-of-roles"></a>Ejemplo de entidad de aprendizaje automático de roles

En la expresión "comprar un billete de **Nueva York** a **Londres**", ambas son ciudades, pero cada una tiene un significado diferente en la frase. New York (Nueva York) es la ciudad de origen y London (Londres) es la de destino. 

```
buy a ticket from New York to London
```

Los roles dan un nombre a esas diferencias:

|Tipo de entidad|Nombre de entidad|Role|Propósito|
|--|--|--|--|
|Simple|Location|origin|De dónde sale el avión|
|Simple|Location|de destino|En dónde aterriza el avión|

## <a name="non-machine-learned-entity-example-of-roles"></a>Ejemplo de entidad que no es de aprendizaje automático de roles

En la expresión "Programar la reunión de 8 a 9", ambos números indican una hora, pero cada hora tiene un significado diferente en la expresión. Los roles proporcionan el nombre de las diferencias. 

```
Schedule the meeting from 8 to 9
```

|Tipo de entidad|Nombre de rol|Valor|
|--|--|--|
|Entidad datetimeV2 creada previamente|Starttime|8|
|Entidad datetimeV2 creada previamente|Endtime|9|

## <a name="are-multiple-entities-in-an-utterance-the-same-thing-as-roles"></a>¿Varias entidades en una expresión es lo mismo que los roles? 

Puede haber varias entidades en una expresión y extraerse sin usar roles. Si el contexto de la frase indica que la versión de la entidad tiene un valor, se debe usar un rol. 

### <a name="dont-use-roles-for-duplicates-without-meaning"></a>No usar roles para duplicados sin significado

Si la expresión incluye una lista de ubicaciones, `I want to travel to Seattle, Cairo, and London.`, esta es una lista donde cada elemento no tiene un significado adicional. 

### <a name="use-roles-if-duplicates-indicate-meaning"></a>Usar roles si los duplicados indican significado

Si la expresión incluye una lista de ubicaciones con significado, `I want to travel from Seattle, with a layover in Londen, landing in Cairo.`, este significado de origen, escala y destino debe capturarse con roles.

### <a name="roles-can-indicate-order"></a>Los roles pueden indicar orden

Si la expresión cambia para indicar el orden de extracción, `I want to first start with Seattle, second London, then third Cairo`, puede extraerlo de dos formas. Puede etiquetar los tokens que indican el rol, `first start with`, `second`, `third`. También puede usar las entidades precompiladas **Ordinal** y **GeographyV2** de una entidad compuesta para capturar la idea de orden y lugar. 

## <a name="how-are-roles-used-in-example-utterances"></a>¿Cómo se usan los roles en las expresiones de ejemplo?

Cuando una entidad tiene un rol y la entidad está marcada en una expresión de ejemplo, tiene la opción de seleccionar solo la entidad o la entidad y el rol. 

En las siguientes expresiones de ejemplo se usan entidades y roles:

|Vista del token|Vista de la entidad|
|--|--|
|Estoy interesado en aprender más sobre **Seattle**|Estoy interesado en aprender más sobre {Location}|
|Comprar un billete de Seattle a Nueva York|Comprar un billete de {Location:Origin} a {Location:Destination}|

## <a name="how-are-roles-used-in-patterns"></a>¿Cómo se usan los roles en los patrones?
En la expresión de plantilla de un patrón, los roles se usan dentro de la expresión: 

|Patrón con roles de entidad|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Sintaxis de los roles en los patrones
La entidad y el rol se incluyen entre paréntesis, `{}`. La entidad y el rol se separan mediante dos puntos. 

## <a name="entity-roles-versus-collaborator-roles"></a>Roles de entidad frente a roles de colaborador

Los roles de entidad se aplican al modelo de datos de la aplicación de LUIS. [Los roles de colaborador](luis-concept-keys.md#contributions-from-other-authors) se aplican a los niveles de acceso de creación. 

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="next-steps"></a>Pasos siguientes

* Use un [tutorial práctico](tutorial-entity-roles.md) con roles de entidad sin entidades de aprendizaje automático.
* Obtenga información sobre cómo agregar [roles](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity).
