---
title: Entidades creadas previamente de URL
titleSuffix: Azure
description: Este artículo contiene información acerca de la entidad precompilada de dirección URL en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 11/27/2018
ms.author: diberry
ms.openlocfilehash: 3e5f6edf213838345c21598213b4fc2065e66335
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55884144"
---
# <a name="url-prebuilt-entity-for-a-luis-app"></a>Entidad precompilada URL para una aplicación de LUIS
La entidad de dirección URL extrae las direcciones URL con nombres de dominio o direcciones IP. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan direcciones URL de la aplicación. La entidad de la dirección URL se admite solamente en la referencia cultural `en-us`. 

## <a name="types-of-urls"></a>Tipos de direcciones URL
La dirección URL se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml).

## <a name="resolution-for-prebuilt-url-entity"></a>Resolución de la entidad de dirección URL precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.url**.

```json
{
  "query": "http://www.luis.ai is a great cognitive services example of artificial intelligence",
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
      "entity": "http://www.luis.ai",
      "type": "builtin.url",
      "startIndex": 0,
      "endIndex": 17
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes

Información sobre las entidades [ordinal](luis-reference-prebuilt-ordinal.md), [number](luis-reference-prebuilt-number.md) y [temperature](luis-reference-prebuilt-temperature.md).
