---
title: Entidad precompilada Temperature
titleSuffix: Azure
description: Este artículo contiene información sobre la entidad precompilada de temperatura en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/04/2019
ms.author: diberry
ms.openlocfilehash: 6a6600a0294eaef9f3b8ee6e7f6f021eb5c1e60b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60709702"
---
# <a name="temperature-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Temperature para una aplicación de LUIS
La entidad de temperatura extrae una variedad de tipos de temperatura. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que contengan temperatura en la aplicación. La entidad de temperatura se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Tipos de temperatura
La temperatura se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819).

## <a name="resolution-for-prebuilt-temperature-entity"></a>Resolución de la entidad de temperatura precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.temperature**.

```json
{
  "query": "set the temperature to 30 degrees",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.85310787
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.85310787
    }
  ],
  "entities": [
    {
      "entity": "30 degrees",
      "type": "builtin.temperature",
      "startIndex": 23,
      "endIndex": 32,
      "resolution": {
        "unit": "Degree",
        "value": "30"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades [porcentaje](luis-reference-prebuilt-percentage.md), [número](luis-reference-prebuilt-number.md) y [edad](luis-reference-prebuilt-age.md). 
