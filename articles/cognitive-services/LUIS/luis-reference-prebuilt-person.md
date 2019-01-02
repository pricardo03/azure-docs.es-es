---
title: Entidad precompilada PersonName
titleSuffix: Azure Cognitive Services
description: Este artículo contiene información sobre la entidad pregenerada personName en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 31dce870e99cbe74e2795a3ba661b0caf92dd48e
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53140241"
---
# <a name="personname-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada PersonName para una aplicación de LUIS
La entidad pregenerada personName detecta los nombres de personas. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan la entidad personName para los propósitos de la aplicación. La entidad personName se admite en las [referencias culturales](luis-reference-prebuilt-entities.md) de inglés y chino.

## <a name="resolution-for-personname-entity"></a>Resolución de la entidad personName
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.personName**.

```json
{
  "query": "Is Jill Jones in Cairo?",
  "topScoringIntent": {
    "intent": "WhereIsEmployee",
    "score": 0.762141049
  }
  "entities": [
    {
      "entity": "Jill Jones",
      "type": "builtin.personName",
      "startIndex": 3,
      "endIndex": 12
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades de [correo electrónico](luis-reference-prebuilt-email.md), [número](luis-reference-prebuilt-number.md) y [ordinal](luis-reference-prebuilt-ordinal.md). 