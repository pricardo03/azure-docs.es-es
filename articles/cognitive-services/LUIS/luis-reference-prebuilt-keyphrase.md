---
title: Entidad precompilada KeyPhrase
titleSuffix: Azure
description: Este artículo contiene información sobre la entidad keyPhrase precompilada en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/26/2018
ms.author: diberry
ms.openlocfilehash: a2bb486de0999ad61cf2fdfdfb57dd91d376f597
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55206604"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada KeyPhrase para una aplicación de LUIS
keyPhrase extrae una variedad de frases clave de una expresión. No es necesario agregar a la aplicación expresiones de ejemplo que contengan keyPhrase. La entidad keyPhrase es compatible con [muchas referencias culturales](luis-language-support.md#languages-supported) como parte de las características de [análisis de texto](../text-analytics/overview.md). 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Resolución de la entidad keyPhrase precompilada
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

## <a name="next-steps"></a>Pasos siguientes

Obtenga información acerca de las entidades [porcentaje](luis-reference-prebuilt-percentage.md), [número](luis-reference-prebuilt-number.md) y [edad](luis-reference-prebuilt-age.md).
