---
title: 'Descripción de cómo se usan los roles en las entidades basadas en patrones: Azure | Microsoft Docs'
description: Obtenga información sobre cómo se usa un rol en una entidad basada en patrones para dar un nombre a un subtipo de entidad contextual.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.technology: luis
ms.topic: article
ms.date: 06/08/2018
ms.author: v-geberr;
ms.openlocfilehash: ab6100e33fb767528b87c6afde4c5ef275fc7c81
ms.sourcegitcommit: 50f82f7682447245bebb229494591eb822a62038
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35383481"
---
# <a name="entity-roles-in-patterns-are-contextual-subtypes"></a>Los roles de entidad en los patrones son subtipos contextuales
Los roles son subtipos contextuales con nombre de una entidad que solo se usa en [patrones](luis-concept-patterns.md).

Por ejemplo, en la expresión `buy a ticket from New York to London`, New York (Nueva York) y London (Londres) son ciudades, pero cada una tiene un significado diferente en la frase. New York (Nueva York) es la ciudad de origen y London (Londres) es la de destino. 

Los roles dan un nombre a esas diferencias:

|Entidad|Rol|Propósito|
|--|--|--|
|Ubicación|de origen|De dónde sale el avión|
|Ubicación|de destino|En dónde aterriza el avión|

## <a name="how-are-roles-used-in-patterns"></a>¿Cómo se usan los roles en los patrones?
En la expresión de plantilla de un patrón, los roles se usan dentro de la expresión: 

```
buy a ticket from {Location:origin} to {Location:destination}
```

## <a name="role-syntax-in-patterns"></a>Sintaxis de los roles en los patrones
La entidad y el rol se incluyen entre paréntesis, `{}`. La entidad y el rol se separan mediante dos puntos. 

## <a name="roles-versus-hierarchical-entities"></a>Roles frente a entidades jerárquicas
Las entidades jerárquicas proporcionan la misma información contextual que los roles, pero solo para las expresiones en **intenciones**. De forma similar, los roles proporcionan la misma información contextual que las entidades jerárquicas, pero solo en los **patrones**.

|Aprendizaje contextual|Se usa en|
|--|--|
|entidades jerárquicas|intenciones|
|roles|patrones|

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo agregar [roles](luis-how-to-add-entities.md#add-role-to-pattern-based-entity).
