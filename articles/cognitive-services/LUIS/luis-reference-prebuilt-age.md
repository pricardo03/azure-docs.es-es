---
title: Entidad precompilada Age
titleSuffix: Azure
description: Este artículo contiene información acerca de la entidad precompilada de edad en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: c901a384d7629a187a2a4fbd4295a1a68615abdb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60712804"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Age para una aplicación de LUIS
La entidad precompilada de edad captura el valor de edad tanto numéricamente como en términos de días, semanas, meses y años. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que contengan la edad en las intenciones de la aplicación. La entidad de edad se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Tipos de edad
La edad se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3).

## <a name="resolution-for-prebuilt-age-entity"></a>Resolución de la entidad de edad precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.age**.

```json
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
