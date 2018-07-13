---
title: 'Referencia de las entidades ordinal de LUIS precompiladas: Azure | Microsoft Docs'
titleSuffix: Azure
description: Este artículo contiene información acerca de la entidad ordinal precompilada en Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 2ff9b083e6cabe455baea3ed777dd6cc00b6fbfe
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321687"
---
# <a name="ordinal-entity"></a>Entidad ordinal
Un número ordinal es una representación numérica de un objeto dentro de un conjunto: `first`, `second`, `third`. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan ordinal a las intenciones de la aplicación. La entidad ordinal se admite en [muchas referencias culturales](luis-reference-prebuilt-entities.md). 

## <a name="types-of-ordinal"></a>Tipos de ordinales
La entidad ordinal se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/English/English-Numbers.yaml#L45).

## <a name="resolution-for-prebuilt-ordinal-entity"></a>Resolución de la entidad ordinal precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.ordinal**.

```JSON
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