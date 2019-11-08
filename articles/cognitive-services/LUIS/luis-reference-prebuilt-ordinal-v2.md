---
title: 'Entidad precompilada Ordinal V2: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información acerca de la entidad precompilada ordinal V2 en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 02bc6657126cb1cf241c2ca4668e62bd49608d4b
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73491253"
---
# <a name="ordinal-v2-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Ordinal V2 para una aplicación de LUIS
Ordinal V2 expande la entidad [Ordinal](luis-reference-prebuilt-ordinal.md) para proporcionar referencias relativas como `next`, `last` y `previous`. Estas no se extraen mediante la entidad precompilada ordinal.

## <a name="resolution-for-prebuilt-ordinal-v2-entity"></a>Resolución de la entidad precompilada ordinal V2

Se devuelven los siguientes objetos entidad para la consulta:

`what is the second to last choice in the list`

#### <a name="v3-responsetabv3"></a>[Respuesta de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ]
}
```

#### <a name="v3-verbose-responsetabv3-verbose"></a>[Respuesta detallada de V3](#tab/V3-verbose)

El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
"entities": {
    "ordinalV2": [
        {
            "offset": -1,
            "relativeTo": "end"
        }
    ],
    "$instance": {
        "ordinalV2": [
            {
                "type": "builtin.ordinalV2.relative",
                "text": "the second to last",
                "startIndex": 8,
                "length": 18,
                "modelTypeId": 2,
                "modelType": "Prebuilt Entity Extractor",
                "recognitionSources": [
                    "model"
                ]
            }
        ]
    }
}
```
#### <a name="v2-responsetabv2"></a>[Respuesta de V2](#tab/V2)

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.ordinalV2**.

```json
"entities": [
    {
        "entity": "the second to last",
        "type": "builtin.ordinalV2.relative",
        "startIndex": 8,
        "endIndex": 25,
        "resolution": {
            "offset": "-1",
            "relativeTo": "end"
        }
    }
]
```
* * * 

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Más información sobre las entidades [percentage](luis-reference-prebuilt-percentage.md), [phonenumber](luis-reference-prebuilt-phonenumber.md) y [temperature](luis-reference-prebuilt-temperature.md). 
