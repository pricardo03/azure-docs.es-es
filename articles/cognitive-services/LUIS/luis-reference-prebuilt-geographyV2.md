---
title: Entidad precompilada GeographyV2
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información sobre la entidad GeographyV2 de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 01/23/2019
ms.author: diberry
ms.openlocfilehash: 17f612f2ee6c7d27dcec9f72ed3df1ed418eb3d2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60712626"
---
# <a name="geographyv2-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada GeographyV2 para una aplicación de LUIS
La entidad precompilada GeographyV2 detecta lugares. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan la entidad GeographyV2 a las intenciones de la aplicación. La entidad GeographyV2 se admite en la [referencia cultural](luis-reference-prebuilt-entities.md) de inglés.

## <a name="subtypes"></a>Subtipos
Las ubicaciones geográficas tienen subtipos:

|Subtype|Propósito|
|--|--|
|`poi`|punto de interés|
|`city`|nombre de ciudad|
|`countryRegion`|nombre del país o región|
|`continent`|nombre del continente|
|`state`|nombre de estado o provincia|


## <a name="resolution-for-geographyv2-entity"></a>Resolución de la entidad GeographyV2
En el ejemplo siguiente se muestra la resolución de la entidad **builtin.geographyV2**.

```json
{
    "query": "Carol is visiting the sphinx in gizah egypt in africa before heading to texas",
    "topScoringIntent": {
        "intent": "None",
        "score": 0.8008023
    },
    "intents": [
        {
            "intent": "None",
            "score": 0.8008023
        }
    ],
    "entities": [
        {
            "entity": "the sphinx",
            "type": "builtin.geographyV2.poi",
            "startIndex": 18,
            "endIndex": 27
        },
        {
            "entity": "gizah",
            "type": "builtin.geographyV2.city",
            "startIndex": 32,
            "endIndex": 36
        },
        {
            "entity": "egypt",
            "type": "builtin.geographyV2.countryRegion",
            "startIndex": 38,
            "endIndex": 42
        },
        {
            "entity": "africa",
            "type": "builtin.geographyV2.continent",
            "startIndex": 47,
            "endIndex": 52
        },
        {
            "entity": "texas",
            "type": "builtin.geographyV2.state",
            "startIndex": 72,
            "endIndex": 76
        },
        {
            "entity": "carol",
            "type": "builtin.personName",
            "startIndex": 0,
            "endIndex": 4
        }
    ]
} 
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades de [correo electrónico](luis-reference-prebuilt-email.md), [número](luis-reference-prebuilt-number.md) y [ordinal](luis-reference-prebuilt-ordinal.md). 
