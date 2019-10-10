---
title: 'Entidad precompilada Number: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información de la entidad precompilada de número en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: cb97cc5b0004442e00b970202dd01f76aa971a2a
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677568"
---
# <a name="number-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Number para una aplicación de LUIS
Hay muchas maneras en que los valores numéricos se utilizan para cuantificar, expresar y describir información. En este artículo, se cubren solo algunos de los ejemplos posibles. LUIS interpreta las variaciones en expresiones de usuario y devuelve los valores numéricos coherentes. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan el número de intenciones de la aplicación. 

## <a name="types-of-number"></a>Tipos de número
El número se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml)

## <a name="examples-of-number-resolution"></a>Ejemplos de resolución de números

| Expresión        | Entidad   | Resolución |
| ------------- |:----------------:| --------------:|
| ```one thousand times```  | ```"one thousand"``` |   ```"1000"```      | 
| ```1,000 people```        | ```"1,000"```    |   ```"1000"```      |
| ```1/2 cup```         | ```"1 / 2"```    |    ```"0.5"```      |
|  ```one half the amount```     | ```"one half"```     |    ```"0.5"```      |
| ```one hundred fifty orders``` | ```"one hundred fifty"``` | ```"150"``` |
| ```one hundred and fifty books``` | ```"one hundred and fifty"``` | ```"150"```|
| ```a grade of one point five```| ```"one point five"``` |  ```"1.5"``` |
| ```buy two dozen eggs```    | ```"two dozen"``` | ```"24"``` |


LUIS incluye el valor reconocido de una entidad **`builtin.number`** en el campo `resolution` de la respuesta JSON que devuelve.

## <a name="resolution-for-prebuilt-number"></a>Resolución del número precompilado


#### <a name="v2-prediction-endpoint-responsetabv2"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

En el siguiente ejemplo, se muestra una respuesta JSON de LUIS, que incluye la resolución del valor 24, para la expresión "dos docenas".

```json
{
  "query": "order two dozen eggs",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.105443209
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.105443209
    },
    {
      "intent": "OrderFood",
      "score": 0.9468431361
    },
    {
      "intent": "Help",
      "score": 0.000399122015
    },
  ],
  "entities": [
    {
      "entity": "two dozen",
      "type": "builtin.number",
      "startIndex": 6,
      "endIndex": 14,
      "resolution": {
        "subtype": "integer",
        "value": "24"
      }
    }
  ]
}
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
{
    "query": "order two dozen eggs",
    "prediction": {
        "normalizedQuery": "order two dozen eggs",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.7124502
            }
        },
        "entities": {
            "number": [
                24
            ]
        }
    }
}
```

El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
{
    "query": "order two dozen eggs",
    "prediction": {
        "normalizedQuery": "order two dozen eggs",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.7124502
            }
        },
        "entities": {
            "number": [
                24
            ],
            "$instance": {
                "number": [
                    {
                        "type": "builtin.number",
                        "text": "two dozen",
                        "startIndex": 6,
                        "length": 9,
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

Obtenga información acerca de las entidades [currency](luis-reference-prebuilt-currency.md), [ordinal](luis-reference-prebuilt-ordinal.md) y [percentage](luis-reference-prebuilt-percentage.md). 
