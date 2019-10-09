---
title: 'Entidades precompiladas Phonenumber: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información de la entidad precompilada de número de teléfono en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 9e237b09bd2b0ecc3e91e10697f6c025f73057da
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677435"
---
# <a name="phone-number-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada de número de teléfono para una aplicación de LUIS
La entidad `phonenumber` extrae una variedad de números de teléfono, incluido el código de país. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que a la aplicación. La entidad `phonenumber` se admite solamente en la referencia cultural `en-us`. 

## <a name="types-of-a-phone-number"></a>Tipos de un número de teléfono
`Phonenumber` se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml).

## <a name="resolution-for-this-prebuilt-entity"></a>Resolución para esta entidad precompilada

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.phonenumber**.

```json
{
  "query": "my mobile is 1 (800) 642-7676",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
        "entity": "1 (800) 642-7676",
        "type": "builtin.phonenumber",
        "startIndex": 13,
        "endIndex": 28,
        "resolution": {
            "score": "1",
            "value": "1 (800) 642-7676"
        }
    }
  ]
}
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
{
    "query": "my mobile is 1 (800) 642-7676",
    "prediction": {
        "normalizedQuery": "my mobile is 1 (800) 642-7676",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.592748761
            }
        },
        "entities": {
            "phonenumber": [
                "1 (800) 642-7676"
            ]
        }
    }
}
```

El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
{
    "query": "my mobile is 1 (800) 642-7676",
    "prediction": {
        "normalizedQuery": "my mobile is 1 (800) 642-7676",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.592748761
            }
        },
        "entities": {
            "phonenumber": [
                "1 (800) 642-7676"
            ],
            "$instance": {
                "phonenumber": [
                    {
                        "type": "builtin.phonenumber",
                        "text": "1 (800) 642-7676",
                        "startIndex": 13,
                        "length": 16,
                        "score": 1,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

* * * 

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Obtenga información acerca de las entidades de [porcentaje](luis-reference-prebuilt-percentage.md), [número de teléfono](luis-reference-prebuilt-number.md) y [temperatura](luis-reference-prebuilt-temperature.md). 
