---
title: Tipo de entidad simple
titleSuffix: Language Understanding - Azure Cognitive Services
description: Una entidad simple es una entidad genérica que describe un concepto único que se ha aprendido en el contexto de aprendizaje automático. Dado que las entidades simples suelen ser nombres, como por ejemplo, nombres de compañías, de productos u otras categorías de nombres, agregue un lista de frases cuando se use una entidad simple para aumentar la señal de los nombres usados.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: reference
ms.date: 07/24/2019
ms.author: diberry
ms.openlocfilehash: 3f03b33f685bb5c7c9ba8f2267b8556c5dadade4
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68480136"
---
# <a name="simple-entity"></a>Entidad simple 

Una entidad simple es una entidad genérica que describe un concepto único que se ha aprendido en el contexto de aprendizaje automático. Dado que las entidades simples suelen ser nombres, como por ejemplo, nombres de compañías, de productos u otras categorías de nombres, agregue un [lista de frases](luis-concept-feature.md) cuando se use una entidad simple para aumentar la señal de los nombres usados. 

**La entidad es la opción ideal cuando:**

* Los datos no tienen un formato coherente, pero indican lo mismo. 

![entidad simple](./media/luis-concept-entities/simple-entity.png)

## <a name="example-json"></a>Ejemplo de JSON

`Bob Jones wants 3 meatball pho`

En la expresión anterior, `Bob Jones` se etiqueta como una entidad `Customer` simple.

Los datos devueltos desde el punto de conexión incluyen el nombre de la entidad, el texto de la expresión que se ha detectado, la ubicación del texto detectado y la puntuación:

```JSON
"entities": [
  {
  "entity": "bob jones",
  "type": "Customer",
  "startIndex": 0,
  "endIndex": 8,
  "score": 0.473899543
  }
]
```

|Objeto de datos|Nombre de entidad|Valor|
|--|--|--|
|Entidad simple|`Customer`|`bob jones`|

## <a name="next-steps"></a>Pasos siguientes

En este [tutorial](luis-quickstart-primary-and-secondary-data.md) se extraen datos aprendidos automáticamente de un nombre de empleo de una expresión con la **entidad Simple**. Para aumentar la precisión de extracción, agregue una [lista de frases](luis-concept-feature.md) de términos específicos para la entidad simple.