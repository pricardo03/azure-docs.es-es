---
title: Entidad precompilada Percentage
titleSuffix: Azure
description: Este artículo contiene información sobre la entidad precompilada de porcentaje en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 3cdf8844ae614e301e1477a039d949b055034ba8
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55208421"
---
# <a name="percentage-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Percentage para una aplicación de LUIS
Los números de porcentaje pueden aparecer como fracciones, `3 1/2`, o como porcentaje, `2%`. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan la entidad de porcentaje para las intenciones de la aplicación. La entidad de porcentaje se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-percentage"></a>Tipos de porcentaje
El porcentaje se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L114).

## <a name="resolution-for-prebuilt-percentage-entity"></a>Resolución para la entidad de porcentaje precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.percentage**.

```json
{
  "query": "set a trigger when my stock goes up 2%",
  "topScoringIntent": {
    "intent": "SetTrigger",
    "score": 0.971157849
  },
  "intents": [
    {
      "intent": "SetTrigger",
      "score": 0.971157849
    }
  ],
  "entities": [
    {
      "entity": "2%",
      "type": "builtin.percentage",
      "startIndex": 36,
      "endIndex": 37,
      "resolution": {
        "value": "2%"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Información sobre las entidades [ordinal](luis-reference-prebuilt-ordinal.md), [number](luis-reference-prebuilt-number.md) y [temperature](luis-reference-prebuilt-temperature.md). 
