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
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: e77cd2290981de62ba7fce7f04174cd0c5ec2af3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65072184"
---
# <a name="dimension-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Dimension para una aplicación de LUIS
La entidad de dimensión precompilada detecta varios tipos de dimensiones, con independencia de la referencia cultural de la aplicación LUIS. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que contengan dimensiones en las intenciones de la aplicación. La entidad de dimensión se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-dimension"></a>Tipos de dimensión

La dimensión se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml).


## <a name="resolution-for-dimension-entity"></a>Resolución de la entidad de dimensión

### <a name="api-version-2x"></a>Versión de API 2.x

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

### <a name="preview-api-version-3x"></a>Versión preliminar de API 3.x

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
{
    "query": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
    "prediction": {
        "normalizedQuery": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.400049
            }
        },
        "entities": {
            "dimension": [
                {
                    "number": 10.5,
                    "unit": "Mile"
                }
            ]
        }
    }
}
```

El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
{
    "query": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
    "prediction": {
        "normalizedQuery": "it takes more than 10 1/2 miles of cable and wire to hook it all up , and 23 computers.",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.400049
            }
        },
        "entities": {
            "dimension": [
                {
                    "number": 10.5,
                    "unit": "Mile"
                }
            ],
            "$instance": {
                "dimension": [
                    {
                        "type": "builtin.dimension",
                        "text": "10 1/2 miles",
                        "startIndex": 19,
                        "length": 12,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades de [correo electrónico](luis-reference-prebuilt-email.md), [número](luis-reference-prebuilt-number.md) y [ordinal](luis-reference-prebuilt-ordinal.md). 
