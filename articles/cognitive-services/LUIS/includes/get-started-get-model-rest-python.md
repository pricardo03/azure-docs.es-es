---
title: Obtención del modelo con la llamada de REST en C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/18/2019
ms.author: diberry
ms.openlocfilehash: f4d180dd6ad99d5bc00e6970e22b756aa26275da
ms.sourcegitcommit: 2a2af81e79a47510e7dea2efb9a8efb616da41f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76268198"
---
## <a name="prerequisites"></a>Prerequisites

* Clave de inicio.
* Importe la aplicación [TravelAgent](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) desde el repositorio de GitHub cognitive-services-language-understanding.
* El identificador de LUIS de la aplicación TravelAgent importada. El identificador de aplicación se muestra en el panel de la aplicación.
* El identificador de versión dentro de la aplicación que recibe las expresiones. El id. predeterminado es "0.1".
* [Python 3.6](https://www.python.org/downloads/) o versiones posteriores.
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]


## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../includes/get-key-quickstart.md)]


## <a name="change-model-programmatically"></a>Cambio de modelo mediante programación

Use Go para agregar una [API](https://aka.ms/luis-apim-v3-authoring) de entidad de aprendizaje automático a la aplicación. 

1. Cree un nuevo archivo llamado `model.py`. Agregue el siguiente código:

    ```python
    ########### Python 3.6 #############
    import requests
    
    # Starter key
    LUIS_authoringKey  = "YOUR-KEY"
    
    LUIS_APP_ID = "YOUR-APP-ID"
    
    # Authoring endpoint, example: westus.api.cognitive.microsoft.com
    LUIS_ENDPOINT = "YOUR-ENDPOINT"

    # The version number of your LUIS app
    LUIS_APP_VERSION = "0.1"
    
    URI_AddUtterances = f'https://{LUIS_ENDPOINT}/luis/authoring/v3.0-preview/apps/{LUIS_APP_ID}/versions/{LUIS_APP_VERSION}/examples'
    URI_Train = f'https://{LUIS_ENDPOINT}/luis/authoring/v3.0-preview/apps/{LUIS_APP_ID}/versions/{LUIS_APP_VERSION}/train'
    
    HEADERS = {'Ocp-Apim-Subscription-Key': LUIS_authoringKey}
    
    def addUtterances():
        r = requests.post(URI_AddUtterances,headers=HEADERS)
        print(r.json())
    
    def train():
        r = requests.post(URI_Train,headers=HEADERS)
        print(r.json())
    
    def trainStatus():
        r = requests.get(URI_Train,headers=HEADERS)
        print(r.json())
    
    addUtterances()
    train()
    trainStatus()
    ```
1. Reemplace los siguientes valores:

    * `YOUR-KEY` por la clave de inicio.
    * `YOUR-ENDPOINT` por el punto de conexión, por ejemplo, `westus2.api.cognitive.microsoft.com`.
    * `YOUR-APP-ID` por el identificador de la aplicación.

1. Con un símbolo del sistema en el mismo directorio que donde creó el archivo, escriba el siguiente comando para ejecutar el archivo:

    ```console
    python model.py
    ```  

## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../includes/starter-key-explanation.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este inicio rápido, elimine el archivo del sistema de archivos. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados para una aplicación](../luis-concept-best-practices.md)
