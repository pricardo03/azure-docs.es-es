---
title: Entidad creada previamente de moneda
titleSuffix: Azure
description: Este artículo contiene información de la entidad precompilada de la moneda en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 7e882a66ae5a090e1fd3a0850ff35281dc4e692d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "65072023"
---
# <a name="currency-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Currency para una aplicación de LUIS
La entidad creada previamente divisa detecta moneda en muchas denominaciones y países o regiones, con independencia de la referencia cultural de la aplicación de LUIS. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan la moneda para las intenciones de la aplicación. La entidad de la moneda se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-currency"></a>Tipos de moneda
La moneda se administra desde el repositorio de GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26)

## <a name="resolution-for-currency-entity"></a>Resolución de la entidad de moneda

### <a name="api-version-2x"></a>Versión de API 2.x

En el siguiente ejemplo se muestra la resolución de la entidad **builtin.currency**.

```json
{
  "query": "search for items under $10.99",
  "topScoringIntent": {
    "intent": "SearchForItems",
    "score": 0.926173568
  },
  "intents": [
    {
      "intent": "SearchForItems",
      "score": 0.926173568
    },
    {
      "intent": "None",
      "score": 0.07376878
    }
  ],
  "entities": [
    {
      "entity": "$10.99",
      "type": "builtin.currency",
      "startIndex": 23,
      "endIndex": 28,
      "resolution": {
        "unit": "Dollar",
        "value": "10.99"
      }
    }
  ]
}
```



### <a name="preview-api-version-3x"></a>Versión de API preliminar 3.x

El siguiente JSON es con el `verbose` parámetro establecido en `false`:

```json
{
    "query": "search for items under $10.99",
    "prediction": {
        "normalizedQuery": "search for items under $10.99",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.605889857
            }
        },
        "entities": {
            "money": [
                {
                    "number": 10.99,
                    "unit": "Dollar"
                }
            ]
        }
    }
}
```

El siguiente JSON es con el `verbose` parámetro establecido en `true`:

```json
{
    "query": "search for items under $10.99",
    "prediction": {
        "normalizedQuery": "search for items under $10.99",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.605889857
            }
        },
        "entities": {
            "money": [
                {
                    "number": 10.99,
                    "unit": "Dollar"
                }
            ],
            "$instance": {
                "money": [
                    {
                        "type": "builtin.currency",
                        "text": "$10.99",
                        "startIndex": 23,
                        "length": 6,
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

Obtenga información acerca de las entidades [datetimeV2](luis-reference-prebuilt-datetimev2.md), [dimension](luis-reference-prebuilt-dimension.md) y [email](luis-reference-prebuilt-email.md). 
