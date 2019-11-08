---
title: Obtención de intención con la llamada a REST en Python
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/18/2019
ms.author: diberry
ms.openlocfilehash: 491d97411cec65d4f747495a6246b4c62d33e973
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73499586"
---
## <a name="prerequisites"></a>Requisitos previos

* [Python 3.6](https://www.python.org/downloads/) o versiones posteriores.
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../includes/get-key-quickstart.md)]

## <a name="get-intent--programmatically"></a>Obtención de la intención mediante programación

Use Python para consultar la [API](https://aka.ms/luis-apim-v3-prediction) GET del punto de conexión de predicción y obtener el resultado de la predicción.

1. Copie uno de los siguientes fragmentos de código en un archivo denominado `predict.py`:

    ```python
    ########### Python 3.6 #############
    import requests
    
    try:
    
        key = 'YOUR-KEY'
        endpoint = 'YOUR-ENDPOINT' # such as 'westus2.api.cognitive.microsoft.com' 
        appId = 'df67dcdb-c37d-46af-88e1-8b97951ca1c2'
        utterance = 'turn on all lights'
    
        headers = {
        }
    
        params ={
            'query': utterance,
            'timezoneOffset': '0',
            'verbose': 'true',
            'show-all-intents': 'true',
            'spellCheck': 'false',
            'staging': 'false',
            'subscription-key': key
        }
    
        r = requests.get(f'https://{endpoint}/luis/prediction/v3.0/apps/{appId}/slots/production/predict',headers=headers, params=params)
        print(r.json())
    
    except Exception as e:
        print(f'{e}')
    ```

1. Reemplace los valores siguientes:

    * `YOUR-KEY` por la clave de inicio.
    * `YOUR-ENDPOINT` por el punto de conexión, por ejemplo, `westus2.api.cognitive.microsoft.com`.

1. Ejecute el siguiente comando de la consola para instalar las dependencias:

    ```console
    pip install requests
    ```

1. Ejecute el script con el siguiente comando de la consola:

    ```console
    python predict.py
    ``` 

1. Revise la respuesta de predicción en formato JSON:

    ```console
    {'query': 'turn on all lights', 'prediction': {'topIntent': 'HomeAutomation.TurnOn', 'intents': {'HomeAutomation.TurnOn': {'score': 0.5375382}, 'None': {'score': 0.08687421}, 'HomeAutomation.TurnOff': {'score': 0.0207554}}, 'entities': {'HomeAutomation.Operation': ['on'], '$instance': {'HomeAutomation.Operation': [{'type': 'HomeAutomation.Operation', 'text': 'on', 'startIndex': 5, 'length': 2, 'score': 0.724984169, 'modelTypeId': -1, 'modelType': 'Unknown', 'recognitionSources': ['model']}]}}}}
    ```

    Respuesta JSON con formato para mejorar la legibilidad: 

    ```JSON
    {
        "query": "turn on all lights",
        "prediction": {
            "topIntent": "HomeAutomation.TurnOn",
            "intents": {
                "HomeAutomation.TurnOn": {
                    "score": 0.5375382
                },
                "None": {
                    "score": 0.08687421
                },
                "HomeAutomation.TurnOff": {
                    "score": 0.0207554
                }
            },
            "entities": {
                "HomeAutomation.Operation": [
                    "on"
                ],
                "$instance": {
                    "HomeAutomation.Operation": [
                        {
                            "type": "HomeAutomation.Operation",
                            "text": "on",
                            "startIndex": 5,
                            "length": 2,
                            "score": 0.724984169,
                            "modelTypeId": -1,
                            "modelType": "Unknown",
                            "recognitionSources": [
                                "model"
                            ]
                        }
                    ]
                }
            }
        }
    }
    ```

## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../includes/starter-key-explanation.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este inicio rápido, elimine el archivo del sistema de archivos. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Adición de expresiones y entrenamiento](../luis-get-started-python-add-utterance.md)