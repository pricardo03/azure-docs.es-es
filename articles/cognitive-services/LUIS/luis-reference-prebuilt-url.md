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
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: b05b4f8638eedc0830c887da59c0c22706a1c4ce
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933383"
---
# <a name="url-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada URL para una aplicación de LUIS
La entidad de dirección URL extrae las direcciones URL con nombres de dominio o direcciones IP. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan direcciones URL de la aplicación. La entidad de la dirección URL se admite solamente en la referencia cultural `en-us`. 

## <a name="types-of-urls"></a>Tipos de direcciones URL
La dirección URL se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml).

## <a name="resolution-for-prebuilt-url-entity"></a>Resolución de la entidad de dirección URL precompilada

### <a name="api-version-2x"></a>Versión de API 2.x

En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.url**.

```json
{
  "query": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.781975448
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.781975448
    }
  ],
  "entities": [
    {
      "entity": "https://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

### <a name="preview-api-version-3x"></a>Versión preliminar de API 3.x

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
{
    "query": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
    "prediction": {
        "normalizedQuery": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.421936184
            }
        },
        "entities": {
            "url": [
                "https://www.luis.ai"
            ]
        }
    }
}
```

El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
{
    "query": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
    "prediction": {
        "normalizedQuery": "https://www.luis.ai is a great cognitive services example of artificial intelligence",
        "topIntent": "None",
        "intents": {
            "None": {
                "score": 0.421936184
            }
        },
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
                        "length": 19,
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

Información sobre las entidades [ordinal](luis-reference-prebuilt-ordinal.md), [number](luis-reference-prebuilt-number.md) y [temperature](luis-reference-prebuilt-temperature.md).
