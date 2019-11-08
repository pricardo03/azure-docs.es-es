---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 10/19/2019
ms.author: diberry
ms.openlocfilehash: ba400beb13d0bd4b29eee0c60617ec7ad128dd77
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505831"
---
Las expresiones del ejemplo siguen un formato específico. 

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
