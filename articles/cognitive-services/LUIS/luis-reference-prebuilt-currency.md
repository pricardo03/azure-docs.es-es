---
title: Entidad precompilada Currency
titleSuffix: Azure
description: Este artículo contiene información de la entidad precompilada de la moneda en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: 0ba2c3229a806cd45e8ef27ac1dd8a258057d0ff
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206092"
---
# <a name="currency-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Currency para una aplicación de LUIS
La entidad de la moneda precompilada detecta la moneda en muchas denominaciones y países, independientemente de la referencia cultural de la aplicación de LUIS. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan la moneda para las intenciones de la aplicación. La entidad de la moneda se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-currency"></a>Tipos de moneda
La moneda se administra desde el repositorio de GitHub [Recognizers-text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-NumbersWithUnit.yaml#L26)

## <a name="resolution-for-currency-entity"></a>Resolución de la entidad de moneda
En el siguiente ejemplo se muestra la resolución de la entidad **builtin.currency**.

```json
{
  "query": "search for items under $10.99",
  "topScoringIntent": {
    "intent": "SearchForItems",
    "score": 0.926173568
  },
  "intents": [
    {
      "intent": "SearchForItems",
      "score": 0.926173568
    },
    {
      "intent": "None",
      "score": 0.07376878
    }
  ],
  "entities": [
    {
      "entity": "$10.99",
      "type": "builtin.currency",
      "startIndex": 23,
      "endIndex": 28,
      "resolution": {
        "unit": "Dollar",
        "value": "10.99"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades [datetimeV2](luis-reference-prebuilt-datetimev2.md), [dimension](luis-reference-prebuilt-dimension.md) y [email](luis-reference-prebuilt-email.md). 
