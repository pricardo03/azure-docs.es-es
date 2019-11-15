---
title: 'Tipo de entidad simple: LUIS'
titleSuffix: Azure Cognitive Services
description: Una entidad simple describe un concepto único desde el contexto del aprendizaje automático. Agregue una lista de frases al usar una entidad simple para mejorar los resultados.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 09/29/2019
ms.author: diberry
ms.openlocfilehash: 27eb4d827c1488b0d051505e62286fd66d6a286b
ms.sourcegitcommit: 609d4bdb0467fd0af40e14a86eb40b9d03669ea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73671517"
---
# <a name="simple-entity"></a>Entidad simple 

Una entidad simple es una entidad genérica que describe un concepto único que se ha aprendido en el contexto de aprendizaje automático. Dado que las entidades simples suelen ser nombres, como por ejemplo, nombres de compañías, de productos u otras categorías de nombres, agregue un [lista de frases](luis-concept-feature.md) cuando se use una entidad simple para aumentar la señal de los nombres usados. 

**La entidad es la opción ideal cuando:**

* Los datos no tienen un formato coherente, pero indican lo mismo. 

![entidad simple](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>Ejemplo de JSON

`Bob Jones wants 3 meatball pho`

En la expresión anterior, `Bob Jones` se etiqueta como una entidad `Customer` simple.

Los datos devueltos desde el punto de conexión incluyen el nombre de la entidad, el texto de la expresión que se ha detectado, la ubicación del texto detectado y la puntuación:

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

Este es el valor JSON si se establece `verbose=false` en la cadena de consulta:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ]
}```

This is the JSON if `verbose=true` is set in the query string:

```json
"entities": {
    "Customer": [
        "Bob Jones"
    ],
    "$instance": {
        "Customer": [
            {
                "type": "Customer",
                "text": "Bob Jones",
                "startIndex": 0,
                "length": 9,
                "score": 0.9339134,
                "modelTypeId": 1,
                "modelType": "Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```

* * * 

|Objeto de datos|Nombre de entidad|Valor|
|--|--|--|
|Entidad simple|`Customer`|`bob jones`|

## <a name="next-steps"></a>Pasos siguientes

En este [tutorial](luis-quickstart-primary-and-secondary-data.md) se extraen datos aprendidos automáticamente de un nombre de empleo de una expresión con la **entidad Simple**. Para aumentar la precisión de extracción, agregue una [lista de frases](luis-concept-feature.md) de términos específicos para la entidad simple.