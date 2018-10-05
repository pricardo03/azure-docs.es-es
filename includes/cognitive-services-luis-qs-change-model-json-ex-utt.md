---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: a1b0afce31d7202c38b049addf546350ff347719
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47044192"
---
El archivo de expresiones de ejemplo, **utterances.json**, sigue un formato específico. 

El campo `text` contiene el texto de la expresión de ejemplo. El campo `intentName` debe corresponder al nombre de una intención existente en la aplicación de LUIS. El campo `entityLabels` es obligatorio. Si no desea etiquetar ninguna entidad, proporcione una matriz vacía.

Si la matriz entityLabels no está vacía, `startCharIndex` y `endCharIndex` tienen que marcar la entidad a la que se hace referencia en el campo `entityName`. El índice está basado en cero, lo que significa que 6 en el ejemplo superior hace referencia a la "S" de Seattle y no al espacio antes de la S mayúscula. Si comienza o termina la etiqueta en un espacio en el texto, se produce un error en la llamada de API para agregar las expresiones.

```JSON
[
  {
    "text": "go to Seattle today",
    "intentName": "BookFlight",
    "entityLabels": [
      {
        "entityName": "Location::LocationTo",
        "startCharIndex": 6,
        "endCharIndex": 12
      }
    ]
  },
  {
    "text": "purple dogs are difficult to work with",
    "intentName": "BookFlight",
    "entityLabels": []
  }
]
```