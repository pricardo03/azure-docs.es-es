---
title: Entidades precompiladas Dimension
titleSuffix: Azure
description: Este artículo contiene información sobre la entidad precompilada de dimensión en Language Understanding (LUIS).
services: cognitive-services
ms.custom: seodec18
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 02/28/2019
ms.author: diberry
ms.openlocfilehash: 9099cdbb91e41998065d953b9d48b3b501df7c10
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60712770"
---
# <a name="dimension-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Dimension para una aplicación de LUIS
La entidad de dimensión precompilada detecta varios tipos de dimensiones, con independencia de la referencia cultural de la aplicación LUIS. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que contengan dimensiones en las intenciones de la aplicación. La entidad de dimensión se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-dimension"></a>Tipos de dimensión

La dimensión se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml).


## <a name="resolution-for-dimension-entity"></a>Resolución de la entidad de dimensión
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.ordinal**.

```json
{
  "query": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.762141049
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.762141049
    }
  ],
  "entities": [
    {
      "entity": "10 1/2 miles",
      "type": "builtin.dimension",
      "startIndex": 19,
      "endIndex": 30,
      "resolution": {
        "unit": "Mile",
        "value": "10.5"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades de [correo electrónico](luis-reference-prebuilt-email.md), [número](luis-reference-prebuilt-number.md) y [ordinal](luis-reference-prebuilt-ordinal.md). 
