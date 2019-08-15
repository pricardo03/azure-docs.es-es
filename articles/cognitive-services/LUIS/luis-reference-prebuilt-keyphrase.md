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
ms.date: 05/07/2019
ms.author: diberry
ms.openlocfilehash: e33b5c766781bc49310dfcae55c3d390a032b522
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933518"
---
# <a name="keyphrase-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada KeyPhrase para una aplicación de LUIS
keyPhrase extrae una variedad de frases clave de una expresión. No es necesario agregar a la aplicación expresiones de ejemplo que contengan keyPhrase. La entidad keyPhrase es compatible con [muchas referencias culturales](luis-language-support.md#languages-supported) como parte de las características de [análisis de texto](../text-analytics/overview.md). 

## <a name="resolution-for-prebuilt-keyphrase-entity"></a>Resolución de la entidad keyPhrase precompilada

### <a name="api-version-2x"></a>Versión de API 2.x

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
