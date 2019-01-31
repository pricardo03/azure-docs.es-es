---
title: Entidad precompilada Temperature
titleSuffix: Azure
description: Este artículo contiene información sobre la entidad precompilada de temperatura en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 921f2bb2c03a24d5ed269452cdc7f14e7a40c73b
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207996"
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
