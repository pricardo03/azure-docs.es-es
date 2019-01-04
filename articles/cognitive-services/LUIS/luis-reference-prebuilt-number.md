---
title: Entidad precompilada Number
titleSuffix: Azure
description: Este artículo contiene información de la entidad precompilada de número en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: bd42fbb85468ab652741573731515e499fe453da
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53165680"
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
        "value": "24"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades [currency](luis-reference-prebuilt-currency.md), [ordinal](luis-reference-prebuilt-ordinal.md) y [percentage](luis-reference-prebuilt-percentage.md). 