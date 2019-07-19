---
title: Entidad precompilada Ordinal V2
titleSuffix: Language Understanding - Azure Cognitive Services
description: Este artículo contiene información acerca de la entidad precompilada ordinal V2 en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 06/25/2019
ms.author: diberry
ms.openlocfilehash: 862b962f5642e01d7ed8250f49d51a6132447083
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67486130"
---
# <a name="ordinal-v2-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Ordinal V2 para una aplicación de LUIS
Ordinal V2 expande la entidad [Ordinal](luis-reference-prebuilt-ordinal.md) para proporcionar referencias relativas como `next`, `last` y `previous`. Estas no se extraen mediante la entidad precompilada ordinal.

## <a name="resolution-for-prebuilt-ordinal-v2-entity"></a>Resolución de la entidad precompilada ordinal V2

### <a name="api-version-2x"></a>Versión de API 2.x

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.ordinalV2**.

```json
{
    "query": "what is the second to last choice in the list",
    "topScoringIntent": {
        "intent": "None",
        "score": 0.823669851
    },
    "intents": [
        {
            "intent": "None",
            "score": 0.823669851
        }
    ],
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
}
```

### <a name="preview-api-version-3x"></a>Versión preliminar de API 3.x

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
{
    "query": "what is the second to last choice in the list",
    "prediction": {
        "normalizedQuery": "what is the second to last choice in the list",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.823669851
            }
        },
        "entities": {
            "ordinalV2": [
                {
                    "offset": -1,
                    "relativeTo": "end"
                }
            ]
        }
    }
}
```

El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
{
    "query": "what is the second to last choice in the list",
    "prediction": {
        "normalizedQuery": "what is the second to last choice in the list",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.823669851
            }
        },
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
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las entidades [percentage](luis-reference-prebuilt-percentage.md), [phonenumber](luis-reference-prebuilt-phonenumber.md) y [temperature](luis-reference-prebuilt-temperature.md). 
