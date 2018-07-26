---
title: 'Referencia de dirección URL de entidades de LUIS precompiladas: Azure | Microsoft Docs'
titleSuffix: Azure
description: Este artículo contiene información acerca de la entidad precompilada de dirección URL en Language Understanding (LUIS).
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/20/2018
ms.author: diberry
ms.openlocfilehash: 86989abab1dcf64384b8b26b9484bc508f2ce31f
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/25/2018
ms.locfileid: "39236795"
---
# <a name="url-entity"></a>Entidad de dirección de URL
La entidad de dirección URL extrae las direcciones URL con nombres de dominio o direcciones IP. Dado que esta entidad ya está entrenada, no es necesario agregar expresiones de ejemplo que contengan direcciones URL de la aplicación. La entidad de la dirección URL se admite solamente en la referencia cultural `en-us`. 

## <a name="types-of-urls"></a>Tipos de direcciones URL
La dirección URL se administra desde el repositorio de GitHub [Recognizers-Text](https://github.com/Microsoft/Recognizers-Text/blob/master/Patterns/Base-URL.yaml).

## <a name="resolution-for-prebuilt-url-entity"></a>Resolución de la entidad de dirección URL precompilada
En el siguiente ejemplo, se muestra la resolución de la entidad **builtin.url**.

```JSON
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