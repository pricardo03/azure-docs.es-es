---
title: 'Entidad precompilada KeyPhrase: LUIS'
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información sobre la entidad keyPhrase precompilada en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: e55c0453c117c51e5a8e4986631516d3e61ed10b
ms.sourcegitcommit: 5f0f1accf4b03629fcb5a371d9355a99d54c5a7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/30/2019
ms.locfileid: "71677594"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada KeyPhrase para una aplicación de LUIS
keyPhrase extrae diversas frases clave de una expresión. No es necesario agregar a la aplicación expresiones de ejemplo que contengan keyPhrase. La entidad keyPhrase es compatible con [muchas referencias culturales](luis-language-support.md#languages-supported) como parte de las características de [análisis de texto](../text-analytics/overview.md). 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Resolución de la entidad keyPhrase precompilada

#### <a name="v2-prediction-endpoint-responsetabv2"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

En el ejemplo siguiente se muestra la resolución de la entidad **built.keyPhrase**.

```json
{
  "query": "where is the educational requirements form for the development and engineering group",
  "topScoringIntent": {
    "intent": "GetJobInformation",
    "score": 0.182757929
  },
  "entities": [
    {
      "entity": "development",
      "type": "builtin.keyPhrase",
      "startIndex": 51,
      "endIndex": 61
    },
    {
      "entity": "educational requirements",
      "type": "builtin.keyPhrase",
      "startIndex": 13,
      "endIndex": 36
    }
  ]
}
```
#### <a name="v3-prediction-endpoint-responsetabv3"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

El siguiente JSON es con el parámetro `verbose` establecido en `false`:

```json
{
    "query": "where is the educational requirements form for the development and engineering group",
    "prediction": {
        "normalizedQuery": "where is the educational requirements form for the development and engineering group",
        "topIntent": "GetJobInformation",
        "intents": {
            "GetJobInformation": {
                "score": 0.157861546
            }
        },
        "entities": {
            "keyPhrase": [
                "educational requirements",
                "development"
            ]
        }
    }
}
```

El siguiente JSON es con el parámetro `verbose` establecido en `true`:

```json
{
    "query": "where is the educational requirements form for the development and engineering group",
    "prediction": {
        "normalizedQuery": "where is the educational requirements form for the development and engineering group",
        "topIntent": "GetJobInformation",
        "intents": {
            "GetJobInformation": {
                "score": 0.157861546
            }
        },
        "entities": {
            "keyPhrase": [
                "educational requirements",
                "development"
            ],
            "$instance": {
                "keyPhrase": [
                    {
                        "type": "builtin.keyPhrase",
                        "text": "educational requirements",
                        "startIndex": 13,
                        "length": 24,
                        "modelTypeId": 2,
                        "modelType": "Prebuilt Entity Extractor",
                        "recognitionSources": [
                            "model"
                        ]
                    },
                    {
                        "type": "builtin.keyPhrase",
                        "text": "development",
                        "startIndex": 51,
                        "length": 11,
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
* * * 

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

Obtenga información acerca de las entidades [porcentaje](luis-reference-prebuilt-percentage.md), [número](luis-reference-prebuilt-number.md) y [edad](luis-reference-prebuilt-age.md).
