---
title: 'Tipo de entidad de lista: LUIS'
titleSuffix: Azure Cognitive Services
description: Las entidades de lista representan un conjunto fijo y cerrado de palabras relacionadas y sus sinónimos. LUIS no detecta valores adicionales para las entidades de lista. Use la característica Recommend (Recomendar) para ver sugerencias de palabras nuevas en función de la lista actual.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: ca9f8b570ee28b1913c8ec81c66a5b70827c04d6
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68559959"
---
# <a name="list-entity"></a>Entidad de lista 

Las entidades de lista representan un conjunto fijo y cerrado de palabras relacionadas y sus sinónimos. LUIS no detecta valores adicionales para las entidades de lista. Use la característica **Recommend** (Recomendar) para ver sugerencias de palabras nuevas en función de la lista actual. Si hay más de una entidad de lista con el mismo valor, se devolverá cada entidad en la consulta de punto de conexión. 

Una entidad de lista no está relacionada con el aprendizaje automático. Es una coincidencia de texto exacta. LUIS marca todas las coincidencias de un elemento de cualquier lista como una entidad en la respuesta. 

**La entidad es la opción ideal cuando los datos de texto:**

* Son un conjunto conocido.
* No cambian a menudo. Si necesita cambiar la lista a menudo o desea que la lista se expanda automáticamente, es mejor una entidad sencilla potenciada con una lista de frases. 
* El conjunto no excede los [límites](luis-boundaries.md) máximos de LUIS para este tipo de entidad.
* El texto de la expresión es una coincidencia exacta con un sinónimo o el nombre canónico. LUIS no usa la lista más allá de las coincidencias exactas de texto. La coincidencia aproximada, la falta de distinción entre mayúsculas y minúsculas, la lematización, los plurales y otras variaciones no se resuelven con una entidad de lista. Para administrar las variaciones, considere el uso de un [patrón](luis-concept-patterns.md#syntax-to-mark-optional-text-in-a-template-utterance) con la sintaxis de texto opcional.

![entidad de lista](./media/luis-concept-entities/list-entity.png)

## <a name="example-json"></a>Ejemplo de JSON

Imagine que la aplicación tiene una lista, denominada `Cities`, que permite variaciones de nombres de ciudades, incluidos la ciudad del aeropuerto (Sea-tac), el código del aeropuerto (SEA), el código postal (98101) y el código de área telefónica (206).

|Elemento de lista|Sinónimos del elemento|
|---|---|
|`Seattle`|`sea-tac`, `sea`, `98101`, `206`, `+1` |
|`Paris`|`cdg`, `roissy`, `ory`, `75001`, `1`, `+33`|

`book 2 tickets to paris`

En la expresión anterior, la palabra `paris` se asigna al elemento paris como parte de la entidad de lista `Cities`. La entidad de lista coincide con el nombre normalizado del elemento y con los sinónimos del elemento.

```JSON
"entities": [
  {
    "entity": "paris",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 22,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

Otra expresión de ejemplo, con un sinónimo de París:

`book 2 tickets to roissy`

```JSON
"entities": [
  {
    "entity": "roissy",
    "type": "Cities",
    "startIndex": 18,
    "endIndex": 23,
    "resolution": {
      "values": [
        "Paris"
      ]
    }
  }
]
```

|Objeto de datos|Nombre de entidad|Valor|
|--|--|--|
|Entidad simple|`Customer`|`bob jones`|

## <a name="next-steps"></a>Pasos siguientes

En este [tutorial](luis-quickstart-intent-and-list-entity.md), aprenderá a usar una **entidad de lista** para extraer las coincidencias exactas del texto de una lista de elementos conocidos. 
