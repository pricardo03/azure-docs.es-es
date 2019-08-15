---
title: Referencia de correo electrónico de entidades precompiladas de LUIS
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información acerca de la entidad precompilada de correo electrónico en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: 542173a8756b868603c606307c5b682c68d7db25
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933548"
---
# <a name="email-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Email para una aplicación de LUIS
La extracción de correo electrónico incluye la dirección de correo electrónico completa de un mensaje. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que contengan el correo electrónico en las intenciones de la aplicación. La entidad de correo electrónico se admite solamente en la referencia cultural `en-us`. 

## <a name="resolution-for-prebuilt-email"></a>Resolución del correo electrónico precompilado

### <a name="api-version-2x"></a>Versión de API 2.x

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.email**.

```json
{
  "query": "please send the information to patti.owens@microsoft.com",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.811592042
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.811592042
    }
  ],
  "entities": [
    {
      "entity": "patti.owens@microsoft.com",
      "type": "builtin.email",
      "startIndex": 31,
      "endIndex": 55,
      "resolution": {
        "value": "patti.owens@microsoft.com"
      }
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Versión preliminar de API 3.x

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
{
    "query": "please send the information to patti.owens@microsoft.com",
    "prediction": {
        "normalizedQuery": "please send the information to patti.owens@microsoft.com",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.5023781
            }
        },
        "entities": {
            "email": [
                "patti.owens@microsoft.com"
            ]
        }
    }
}
```


El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
{
    "query": "please send the information to patti.owens@microsoft.com",
    "prediction": {
        "normalizedQuery": "please send the information to patti.owens@microsoft.com",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.5023781
            }
        },
        "entities": {
            "email": [
                "patti.owens@microsoft.com"
            ],
            "$instance": {
                "email": [
                    {
                        "type": "builtin.email",
                        "text": "patti.owens@microsoft.com",
                        "startIndex": 31,
                        "length": 25,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor"
                    }
                ]
            }
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades de [número](luis-reference-prebuilt-number.md), [ordinal](luis-reference-prebuilt-ordinal.md) y [porcentaje](luis-reference-prebuilt-percentage.md). 
