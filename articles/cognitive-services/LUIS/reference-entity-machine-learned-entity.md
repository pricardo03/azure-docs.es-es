---
title: 'Tipo de entidad de aprendizaje automático: LUIS'
titleSuffix: Azure Cognitive Services
description: ''
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 11/11/2019
ms.author: diberry
ms.openlocfilehash: 2289e8ac7744a7b4cbee300e77efda89d29ee2f5
ms.sourcegitcommit: ae8b23ab3488a2bbbf4c7ad49e285352f2d67a68
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74017209"
---
# <a name="machine-learned-entity"></a>Entidad de aprendizaje automático 



**La entidad es la opción ideal cuando los datos de texto:**


![entidad de lista](./media/luis-concept-entities/list-entity.png)

## <a name="example-json"></a>Ejemplo de JSON

Imagine que la aplicación tiene una lista, denominada `Cities`, que permite variaciones de nombres de ciudades, incluidos la ciudad del aeropuerto (Sea-tac), el código del aeropuerto (SEA), el código postal (98101) y el código de área telefónica (206).

|Elemento de lista|Sinónimos del elemento|
|---|---|
|`Seattle`|`sea-tac`, `sea`, `98101`, `206`, `+1` |
|`Paris`|`cdg`, `roissy`, `ory`, `75001`, `1`, `+33`|

`book 2 tickets to paris`

En la expresión anterior, la palabra `paris` se asigna al elemento paris como parte de la entidad de lista `Cities`. La entidad de lista coincide con el nombre normalizado del elemento y con los sinónimos del elemento.

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

```JSON
  "entities": [
    {
      "entity": "paris",
      "type": "Cities",
      "startIndex": 18,
      "endIndex": 22,
      "resolution": {
        "values": [
          "Paris"
        ]
      }
    }
  ]
```

#### <a name="v3-prediction-endpoint-responsetabv3"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)


Este es el valor JSON si `verbose=false` se establece en la cadena de consulta:

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ]
}
```

Este es el valor JSON si `verbose=true` se establece en la cadena de consulta:

```json
"entities": {
    "Cities": [
        [
            "Paris"
        ]
    ],
    "$instance": {
        "Cities": [
            {
                "type": "Cities",
                "text": "paris",
                "startIndex": 18,
                "length": 5,
                "modelTypeId": 5,
                "modelType": "List Entity Extractor",
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
|Entidad de lista|`Cities`|`paris`|


## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre la entidad [lista](reference-entity-list.md) y la entidad [expresión regular](reference-entity-regular-expression.md).