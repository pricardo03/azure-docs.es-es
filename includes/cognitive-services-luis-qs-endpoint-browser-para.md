---
title: archivo de inclusión
description: archivo de inclusión
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.subservice: luis
ms.topic: include
ms.custom: include file
ms.date: 08/16/2018
ms.author: diberry
ms.openlocfilehash: 1b70c5e71c585524579171782dfdeda308592b30
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478691"
---
Para entender lo que devuelve un punto de conexión de predicción de LUIS, vea un resultado de predicción en un explorador web. Para consultar una aplicación pública, necesita su propia clave y el id. de la aplicación. El id. de la aplicación de IoT pública, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, se proporciona como parte de la dirección URL en el paso uno.

El formato de la dirección URL para una solicitud de punto de conexión **GET** es:

```JSON
https://<region>.api.cognitive.microsoft.com/luis/v2.0/apps/<appID>?subscription-key=<YOUR-KEY>&q=<user-utterance>
```

1. El punto de conexión de la aplicación de IoT pública tiene este formato: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_KEY>&q=turn on the bedroom light`

    Copie la dirección URL y sustituya la clave por el valor de `<YOUR_KEY>`.

2. Pegue la dirección URL en una ventana del explorador y presione ENTRAR. El explorador muestra el resultado JSON que indica que LUIS detecta la intención `HomeAutomation.TurnOn` como intención principal y la entidad `HomeAutomation.Room` con el valor `bedroom`.

    ```JSON
    {
      "query": "turn on the bedroom light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOn",
        "score": 0.809439957
      },
      "entities": [
        {
          "entity": "bedroom",
          "type": "HomeAutomation.Room",
          "startIndex": 12,
          "endIndex": 18,
          "score": 0.8065475
        }
      ]
    }
    ```

3. Cambie el valor del parámetro `q=` en la dirección URL por `turn off the living room light` y presione ENTRAR. Ahora, el resultado indica que LUIS ha detectado la intención `HomeAutomation.TurnOff` como intención principal y la entidad `HomeAutomation.Room` con el valor `living room`. 

    ```JSON
    {
      "query": "turn off the living room light",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOff",
        "score": 0.984057844
      },
      "entities": [
        {
          "entity": "living room",
          "type": "HomeAutomation.Room",
          "startIndex": 13,
          "endIndex": 23,
          "score": 0.9619945
        }
      ]
    }
    ```
