---
title: Referencia de entidades precompiladas de número de teléfono de LUIS en Azure | Microsoft Docs
titleSuffix: Azure
description: Este artículo contiene información de la entidad precompilada de número de teléfono en Language Understanding (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: v-geberr
ms.openlocfilehash: 0f72b807b9b0ec110a80d67babb1c45902b8c810
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "36321567"
---
# <a name="phonenumber-entity"></a>Entidad de número de teléfono
La entidad `phonenumber` extrae una variedad de números de teléfono, incluido el código de país. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que a la aplicación. La entidad `phonenumber` se admite solamente en la referencia cultural `en-us`. 

## <a name="types-of-phonenumber"></a>Tipos de número de teléfono
El número de teléfono se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml).

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Resolución de la entidad de número de teléfono precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.phonenumber**.

```JSON
{
  "query": "my mobile is 00 44 161 1234567",
  "topScoringIntent": {
    "intent": "None",
    "score": 0.8448457
  },
  "intents": [
    {
      "intent": "None",
      "score": 0.8448457
    }
  ],
  "entities": [
    {
      "entity": "00 44 161 1234567",
      "type": "builtin.phonenumber",
      "startIndex": 13,
      "endIndex": 29,
      "resolution": {
        "value": "00 44 161 1234567"
      }
    }
  ]
}
```


## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades de [porcentaje](luis-reference-prebuilt-percentage.md), [número de teléfono](luis-reference-prebuilt-number.md) y [temperatura](luis-reference-prebuilt-temperature.md). 