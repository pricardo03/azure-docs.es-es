---
title: 'Entidades precompiladas URL: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información acerca de la entidad precompilada de dirección URL en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 10/04/2019
ms.author: diberry
ms.openlocfilehash: 12831ede2b9d9251f2e02fa396ee7d2fb2d61240
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499492"
---
# <a name="url-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada URL para una aplicación de LUIS
La entidad de dirección URL extrae las direcciones URL con nombres de dominio o direcciones IP. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan direcciones URL de la aplicación. La entidad de la dirección URL se admite solamente en la referencia cultural `en-us`. 

## <a name="types-of-urls"></a>Tipos de direcciones URL
La dirección URL se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml).

## <a name="resolution-for-prebuilt-url-entity"></a>Resolución de la entidad de dirección URL precompilada

Se devuelven los siguientes objetos entidad para la consulta:

`https://www.luis.ai is a great cognitive services example of artificial intelligence`

#### <a name="v3-responsetabv3"></a>[Respuesta de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
"entities": {
    "url": [
        "https://www.luis.ai"
    ]
}
```
#### <a name="v3-verbose-responsetabv3-verbose"></a>[Respuesta detallada de V3](#tab/V3-verbose)

El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
"entities": {
    "url": [
        "https://www.luis.ai"
    ],
    "$instance": {
        "url": [
            {
                "type": "builtin.url",
                "text": "https://www.luis.ai",
                "startIndex": 0,
                "length": 17,
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

En el ejemplo siguiente se muestra la resolución de https://www.luis.ai, un excelente ejemplo de Cognitive Services de inteligencia artificial.

```json
"entities": [
    {
        "entity": "https://www.luis.ai",
        "type": "builtin.url",
        "startIndex": 0,
        "endIndex": 17
    }
]
```

* * * 

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Información sobre las entidades [ordinal](luis-reference-prebuilt-ordinal.md), [number](luis-reference-prebuilt-number.md) y [temperature](luis-reference-prebuilt-temperature.md).
