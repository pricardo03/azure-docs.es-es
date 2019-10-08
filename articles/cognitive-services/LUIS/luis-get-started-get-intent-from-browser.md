---
title: 'Inicio rápido: Obtención de la intención con el explorador: LUIS'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, usará una aplicación de LUIS disponible públicamente para determinar la intención de un usuario a partir de texto conversacional en un explorador.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 6282e768ebc51d0d4ec2b15f057727f207a7d81a
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703605"
---
# <a name="quickstart-get-intent-with-a-browser"></a>Inicio rápido: Obtención de la intención con un explorador

Para entender lo que devuelve un punto de conexión de predicción de LUIS, vea un resultado de predicción en un explorador web. 

## <a name="prerequisites"></a>Requisitos previos

Para consultar una aplicación pública, necesita:

* Su propia clave de Language Understanding (LUIS). Si aún no tiene una suscripción para crear una clave, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/).
* El identificador de la aplicación pública: `df67dcdb-c37d-46af-88e1-8b97951ca1c2`. 

## <a name="use-the-browser-to-see-predictions"></a>Uso del explorador para ver las predicciones

1. Abra un explorador web. 
1. Use las direcciones URL completas que se indican a continuación y reemplace `{your-key}` por su propia clave LUIS. Las solicitudes son solicitudes GET e incluyen la autorización, con la clave LUIS, como parámetro de la cadena de consulta.

    #### <a name="v2-prediction-endpoint-requesttabv2"></a>[Solicitud de punto de conexión de predicción de V2](#tab/V2)
    
    El formato de la dirección URL de V2 para una solicitud de punto de conexión **GET** es:
    
    `
    https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key={your-key}&q=turn on all lights
    `
    
    #### <a name="v3-prediction-endpoint-requesttabv3"></a>[Solicitud de punto de conexión de predicción de V3](#tab/V3)
    
    
    El formato de la dirección URL de V3 para una solicitud de punto de conexión (por ranuras) **GET** es:
    
    `
    https://westus.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key={your-key}
    `
    
    * * *

1. Pegue la dirección URL en una ventana del explorador y presione ENTRAR. El explorador muestra el resultado JSON que indica que LUIS detecta la intención `HomeAutomation.TurnOn` como intención principal y la entidad `HomeAutomation.Operation` con el valor `on`.

    #### <a name="v2-prediction-endpoint-responsetabv2"></a>[Respuesta de punto de conexión de predicción de V2](#tab/V2)

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

    #### <a name="v3-prediction-endpoint-responsetabv3"></a>[Respuesta de punto de conexión de predicción de V3](#tab/V3)

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "normalizedQuery": "turn on all lights",
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    * * *

1. Para ver todas las intenciones, agregue el parámetro de cadena de consulta adecuado. 

    #### <a name="v2-prediction-endpointtabv2"></a>[Punto de conexión de predicción de V2](#tab/V2)

    Agregue `verbose=true` al final de la cadena de QueryString para **mostrar todas las intenciones**:

    `
    https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?q=turn on all lights&subscription-key={your-key}&verbose=true
    `

    ```json
    {
        "query": "turn on all lights",
        "topScoringIntent": {
            "intent": "HomeAutomation.TurnOn",
            "score": 0.5375382
        },
        "intents": [
            {
                "intent": "HomeAutomation.TurnOn",
                "score": 0.5375382
            },
            {
                "intent": "None",
                "score": 0.08687421
            },
            {
                "intent": "HomeAutomation.TurnOff",
                "score": 0.0207554
            }
        ],
        "entities": [
            {
                "entity": "on",
                "type": "HomeAutomation.Operation",
                "startIndex": 5,
                "endIndex": 6,
                "score": 0.724984169
            }
        ]
    }
    ```

    #### <a name="v3-prediction-endpointtabv3"></a>[Punto de conexión de predicción de V3](#tab/V3)

    Agregue `show-all-intents=true` al final de la cadena de QueryString para **mostrar todas las intenciones**:

    `
    https://westus.api.cognitive.microsoft.com/luis/v3.0-preview/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2/slots/production/predict?query=turn on all lights&subscription-key={your-key}&show-all-intents=true
    `

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "normalizedQuery": "turn on all lights",
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "HomeAutomation.TurnOff": {
                     "score": 0.0207554
                },
                "None": {
                     "score": 0.08687421
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ]
            }
        }
    }
    ```

    * * * 

## <a name="next-steps"></a>Pasos siguientes

Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).

> [!div class="nextstepaction"]
> [Creación de una aplicación en el portal de LUIS](get-started-portal-build-app.md)