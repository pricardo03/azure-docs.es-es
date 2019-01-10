---
title: Roles para entidades
titleSuffix: Azure Cognitive Services
description: Los roles son subtipos contextuales con nombre de una entidad que solo se usa en patrones. Por ejemplo, en la expresión `buy a ticket from New York to London`, New York (Nueva York) y London (Londres) son ciudades, pero cada una tiene un significado diferente en la frase. New York (Nueva York) es la ciudad de origen y London (Londres) es la de destino.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: diberry
ms.openlocfilehash: cb155486365ffa1beb4657e2d9cc56fcf143b624
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53547752"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Los roles de entidad en los patrones son subtipos contextuales
Los roles son subtipos contextuales con nombre de una entidad que solo se usa en [patrones](luis-concept-patterns.md).

Por ejemplo, en la expresión `buy a ticket from New York to London`, New York (Nueva York) y London (Londres) son ciudades, pero cada una tiene un significado diferente en la frase. New York (Nueva York) es la ciudad de origen y London (Londres) es la de destino. 

Los roles dan un nombre a esas diferencias:

|Entidad|Rol|Propósito|
|--|--|--|
|Ubicación|origin|De dónde sale el avión|
|Ubicación|de destino|En dónde aterriza el avión|
|Entidad datetimeV2 creada previamente|to|fecha de finalización|
|Entidad datetimeV2 creada previamente|De|fecha de inicio|

## <a name="how-are-roles-used-in-patterns"></a>¿Cómo se usan los roles en los patrones?
En la expresión de plantilla de un patrón, los roles se usan dentro de la expresión: 

|Patrón con roles de entidad|
|--|
|`buy a ticket from {Location:origin} to {Location:destination}`|


## <a name="role-syntax-in-patterns"></a>Sintaxis de los roles en los patrones
La entidad y el rol se incluyen entre paréntesis, `{}`. La entidad y el rol se separan mediante dos puntos. 


[!INCLUDE [H2 Roles versus hierarchical entities](../../../includes/cognitive-services-luis-hier-roles.md)] 

## <a name="example-role-for-entities"></a>Rol de ejemplo para las entidades

Un rol es simplemente una selección de ubicación aprendida contextualmente de una entidad dentro de una expresión. Resulta más eficaz cuando la expresión tiene varios tipos de entidad de esa clase. El ejemplo más fácil para cualquier tipo de entidad es distinguir entre una ubicación de origen y una de destino. La ubicación se puede representar en muchos tipos de entidad diferentes. 

Un ejemplo de caso de uso consiste en transferir un empleado de un departamento a otro, donde cada departamento es un elemento de una lista. Por ejemplo:  

`Move [PersonName] from [Department:from] to [Department:to]`. 

En la predicción devuelta, ambas entidades de departamento se devolverán en la respuesta JSON y cada una de ellas incluirá el nombre del rol. 

## <a name="roles-with-prebuilt-entities"></a>Roles con entidades precompiladas

Utilice roles con las entidades creadas previamente para dar sentido a distintas instancias de la entidad creada previamente dentro de una expresión. 

### <a name="roles-with-datetimev2"></a>Roles con datetimeV2

DatetimeV2, la entidad creada previamente, hace un gran trabajo al comprender una gran variedad de fechas y horas distintas en las expresiones. Puede especificar las fechas y los intervalos de fechas de manera diferente a la descripción predeterminada de la entidad creada previamente. 

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo agregar [roles](luis-how-to-add-entities.md#add-a-role-to-pattern-based-entity).
