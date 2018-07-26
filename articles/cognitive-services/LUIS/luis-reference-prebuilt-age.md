---
title: Referencia de entidades precompiladas de edad de LUIS en Azure | Microsoft Docs
titleSuffix: Azure
description: Este artículo contiene información acerca de la entidad precompilada de edad en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: f93acb8bece8c66c3ed7197f1c4530011aec3f29
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39237407"
---
# <a name="age-entity"></a>Entidad de edad
La entidad precompilada de edad captura el valor de edad tanto numéricamente como en términos de días, semanas, meses y años. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que contengan la edad en las intenciones de la aplicación. La entidad de edad se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Tipos de edad
La edad se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3).

## <a name="resolution-for-prebuilt-age-entity"></a>Resolución de la entidad de edad precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.age**.

```JSON
{
  "query": "A 90 day old utilities bill is quite late.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8236133
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8236133
    }
  ],
  "entities": [
    {
      "entity": "90 day old",
      "type": "builtin.age",
      "startIndex": 2,
      "endIndex": 11,
      "resolution": {
        "unit": "Day",
        "value": "90"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades [moneda](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md) y [dimensión](luis-reference-prebuilt-dimension.md). 