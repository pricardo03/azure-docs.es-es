---
title: Entidad precompilada Ordinal
titleSuffix: Azure
description: Este artículo contiene información acerca de la entidad ordinal precompilada en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: f6762c74dbe6b16791e597464584b91e696220f1
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863457"
---
# <a name="ordinal-prebuilt-entity-for-a-luis-app"></a>entidad precompilada Ordinal para una aplicación de LUIS
Un número ordinal es una representación numérica de un objeto dentro de un conjunto: `first`, `second`, `third`. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan ordinal a las intenciones de la aplicación. La entidad ordinal se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-ordinal"></a>Tipos de ordinales
La entidad ordinal se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45).

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Resolución de la entidad ordinal precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.ordinal**.

```json
{
  "query": "Order the second option",
  "topScoringIntent": {
    "intent": "OrderFood",
    "score": 0.9993253
  },
  "intents": [
    {
      "intent": "OrderFood",
      "score": 0.9993253
    },
    {
      "intent": "None",
      "score": 0.05046708
    }
  ],
  "entities": [
    {
      "entity": "second",
      "type": "builtin.ordinal",
      "startIndex": 10,
      "endIndex": 15,
      "resolution": {
        "value": "2"
      }
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Información sobre las entidades [percentage](luis-reference-prebuilt-percentage.md), [phonenumber](luis-reference-prebuilt-phonenumber.md) y [temperature](luis-reference-prebuilt-temperature.md). 
