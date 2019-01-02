---
title: Entidades precompiladas Phonenumber
titleSuffix: Azure
description: Este artículo contiene información de la entidad precompilada de número de teléfono en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 3a538886b0137e1069af1fa680374b749a9b3f92
ms.sourcegitcommit: efcd039e5e3de3149c9de7296c57566e0f88b106
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53165423"
---
# <a name="phonenumber-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada Phonenumber para una aplicación de LUIS
La entidad `phonenumber` extrae una variedad de números de teléfono, incluido el código de país. Dado que esta entidad ya está entrenada, no se necesita agregar expresiones de ejemplo que a la aplicación. La entidad `phonenumber` se admite solamente en la referencia cultural `en-us`. 

## <a name="types-of-phonenumber"></a>Tipos de número de teléfono
El número de teléfono se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-PhoneNumbers.yaml).

## <a name="resolution-for-prebuilt-phonenumber-entity"></a>Resolución de la entidad de número de teléfono precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.phonenumber**.

```json
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