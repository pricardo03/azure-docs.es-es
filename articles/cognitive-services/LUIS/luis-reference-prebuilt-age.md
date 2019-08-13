---
title: 'Entidad precompilada Age: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información acerca de la entidad precompilada de edad en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 77290790342c1159eca049108e1c9fadd5d4bbf3
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68560293"
---
# <a name="age-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Age para una aplicación de LUIS
La entidad precompilada de edad captura el valor de edad tanto numéricamente como en términos de días, semanas, meses y años. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que contengan la edad en las intenciones de la aplicación. La entidad de edad se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-age"></a>Tipos de edad
La edad se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L3).

## <a name="resolution-for-prebuilt-age-entity"></a>Resolución de la entidad de edad precompilada

### <a name="api-version-2x"></a>Versión de API 2.x

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.age**.

```json
{
  "query": "A 90 day old utilities bill is quite late.",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8236133
  },
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

### <a name="preview-api-version-3x"></a>Versión preliminar de API 3.x

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
{
    "query": "A 90 day old utilities bill is quite late.",
    "prediction": {
        "normalizedQuery": "a 90 day old utilities bill is quite late.",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.558252
            }
        },
        "entities": {
            "age": [
                {
                    "number": 90,
                    "unit": "Day"
                }
            ]
        }
    }
}
```

El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
{
    "query": "A 90 day old utilities bill is quite late.",
    "prediction": {
        "normalizedQuery": "a 90 day old utilities bill is quite late.",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.558252
            }
        },
        "entities": {
            "age": [
                {
                    "number": 90,
                    "unit": "Day"
                }
            ],
            "$instance": {
                "age": [
                    {
                        "type": "builtin.age",
                        "text": "90 day old",
                        "startIndex": 2,
                        "length": 10,
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

Obtenga información acerca de las entidades [moneda](luis-reference-prebuilt-currency.md), [datetimeV2](luis-reference-prebuilt-datetimev2.md) y [dimensión](luis-reference-prebuilt-dimension.md). 
