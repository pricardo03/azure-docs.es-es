---
title: Roles para entidades
titleSuffix: Azure Cognitive Services
description: Los roles son subtipos contextuales con nombre de una entidad que solo se usa en patrones. Por ejemplo, en la expresión `buy a ticket from New York to London`, New York (Nueva York) y London (Londres) son ciudades, pero cada una tiene un significado diferente en la frase. New York (Nueva York) es la ciudad de origen y London (Londres) es la de destino.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/01/2019
ms.author: diberry
ms.openlocfilehash: 318e71b68bbabeeef34c75a412f9fdd5b6db754a
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "65073026"
---
# <a name="entity-roles-for-contextual-subtypes"></a>Roles de entidad para los subtipos contextuales

Funciones que las entidades que haya asignado un nombre subtipos. Un rol se puede usar con cualquier tipo de entidad precompilada o personalizada, y tanto en patrones como en expresiones de ejemplo. 

<a name="example-role-for-entities"></a>
<a name="roles-with-prebuilt-entities"></a>

## <a name="machine-learned-entity-example-of-roles"></a>Ejemplo de entidad ha aprendido la máquina de roles

En la declaración "comprar un billete de **Nueva York** a **Londres**, Nueva York y Londres son ciudades, pero cada uno tiene un significado diferente en la frase. New York (Nueva York) es la ciudad de origen y London (Londres) es la de destino. 

```
buy a ticket from New York to London
```

Los roles dan un nombre a esas diferencias:

|Tipo de entidad|Nombre de entidad|Rol|Propósito|
|--|--|--|--|
|Simple|Ubicación|origen|De dónde sale el avión|
|Simple|Ubicación|de destino|En dónde aterriza el avión|

## <a name="non-machine-learned-entity-example-of-roles"></a>Ejemplo de entidad no se ha aprendido máquina de roles

En la declaración "Programar la reunión de 8 a 9", tanto los números indican un tiempo, pero cada vez que tiene un significado diferente en la declaración. Roles proporcionan el nombre de las diferencias. 

```
Schedule the meeting from 8 to 9
```

|Tipo de entidad|Nombre del rol|Valor|
|--|--|--|
|Entidad datetimeV2 creada previamente|hora de inicio|8|
|Entidad datetimeV2 creada previamente|hora de finalización|9|

## <a name="are-multiple-entities-in-an-utterance-the-same-thing-as-roles"></a>¿Son lo mismo que las funciones de varias entidades en una declaración? 

Varias entidades pueden existir en una declaración y pueden extraerse sin usar roles. Si indica el contexto de la frase con la versión de la entidad tiene un valor, se debe usar un rol. 

### <a name="dont-use-roles-for-duplicates-without-meaning"></a>No utilice los roles para los duplicados sin significado

Si la declaración incluye una lista de ubicaciones, `I want to travel to Seattle, Cairo, and London.`, esta es una lista donde cada elemento no tiene un significado adicional. 

### <a name="use-roles-if-duplicates-indicate-meaning"></a>Usar roles si duplicados indican significado

Si la declaración incluye una lista de ubicaciones con significado, `I want to travel from Seattle, with a layover in Londen, landing in Cairo.`, este significado de origen, regresé y destino debe capturarse con roles.

### <a name="roles-can-indicate-order"></a>Roles pueden indicar el orden

Si la declaración cambia para indicar el orden en que desea extraer, `I want to first start with Seattle, second London, then third Cairo`, puede extraer en un par de formas. Puede etiquetar los tokens que indican el rol, `first start with`, `second`, `third`. También puede usar la entidad creada previamente **Ordinal** y **GeographyV2** entidad creada previamente en una entidad compuesta para capturar la idea de orden y en su lugar. 

## <a name="how-are-roles-used-in-example-utterances"></a>¿Cómo se usan roles en declaraciones de ejemplo?

Cuando una entidad tiene un rol y se marca la entidad en una declaración de ejemplo, tiene la opción de seleccionar solo la entidad o la entidad y la función. 

Las siguientes declaraciones de ejemplo use entidades y roles:

|Vista del token|Vista de entidades|
|--|--|
|Estoy interesado en aprender más sobre **Seattle**|Estoy interesado en aprender más acerca de {Location}|
|Comprar un billete de Seattle a Nueva York|Comprar un billete de {ubicación: origen} a {ubicación: Destination}|

## <a name="how-are-roles-used-in-patterns"></a>¿Cómo se usan los roles en los patrones?
En la expresión de plantilla de un patrón, los roles se usan dentro de la expresión: 

|Patrón con roles de entidad|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Sintaxis de los roles en los patrones
La entidad y el rol se incluyen entre paréntesis, `{}`. La entidad y el rol se separan mediante dos puntos. 

## <a name="entity-roles-versus-collaborator-roles"></a>Roles de entidad frente a los roles de colaborador

Los roles de la entidad se aplican al modelo de datos de la aplicación de LUIS. [Colaborador](luis-concept-collaborator.md) roles se aplican a los niveles de acceso de creación. 

[!INCLUDE [Entity roles in batch testing - currently not supported](../../../includes/cognitive-services-luis-roles-not-supported-in-batch-testing.md)]

## <a name="next-steps"></a>Pasos siguientes

* Use un [tutorial práctico](tutorial-entity-roles.md) mediante roles de la entidad con las entidades no ha aprendido la máquina
* Obtenga información sobre cómo agregar [roles](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity).
