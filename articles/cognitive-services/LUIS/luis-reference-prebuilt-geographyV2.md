---
title: 'Entidad precompilada de GeographyV2: referencia de LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información sobre la entidad GeographyV2 de Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 3559bc02944f88f486104d4d9553f0c45a1f1754
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46983419"
---
# <a name="geographyv2-entity"></a>Entidad GeographyV2
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

```JSON
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