---
title: Referencia de entidades precompiladas de temperatura de LUIS precompiladas en Azure | Microsoft Docs
titleSuffix: Azure
description: Este artículo contiene información sobre la entidad precompilada de temperatura en Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 3cc16e7ec87775407c4261655d8f680cc0903e81
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321507"
---
# <a name="temperature-entity"></a>Entidad de temperatura
La entidad de temperatura extrae una variedad de tipos de temperatura. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que contengan temperatura en la aplicación. La entidad de temperatura se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-temperature"></a>Tipos de temperatura
La entidad de temperatura se administra desde el repositorio de Github [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L819).

## <a name="resolution-for-prebuilt-temperature-entity"></a>Resolución de la entidad de temperatura precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.temperature**.

```JSON
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