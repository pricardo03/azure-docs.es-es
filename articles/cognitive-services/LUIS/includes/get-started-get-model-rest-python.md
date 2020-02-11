---
title: Obtención del modelo con la llamada de REST en C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 01/31/2020
ms.author: diberry
ms.openlocfilehash: a21834cec456ded82c15dc916cc4991a196ea22c
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2020
ms.locfileid: "76966964"
---
## <a name="prerequisites"></a>Prerequisites

* Azure Language Understanding: creación de una clave de 32 caracteres y de la dirección URL del punto de conexión de creación. Cree con [Azure Portal](../luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) o con la [CLI de Azure](../luis-how-to-azure-subscription.md#create-resources-in-azure-cli).
* Importe la aplicación [TravelAgent](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) desde el repositorio de GitHub cognitive-services-language-understanding.
* El identificador de LUIS de la aplicación TravelAgent importada. El identificador de aplicación se muestra en el panel de la aplicación.
* El identificador de versión dentro de la aplicación que recibe las expresiones. El id. predeterminado es "0.1".
* [Python 3.6](https://www.python.org/downloads/) o versiones posteriores.
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]


## <a name="change-model-programmatically"></a>Cambio de modelo mediante programación

Use Go para agregar una [API](https://aka.ms/luis-apim-v3-authoring) de entidad de aprendizaje automático a la aplicación.

1. Cree un nuevo archivo llamado `model.py`. Agregue el siguiente código:

    ```python
    ########### Python 3.6 #############
    import requests

    # 32 character Authoring key
    LUIS_authoringKey  = "YOUR-KEY"

    LUIS_APP_ID = "YOUR-APP-ID"

    # Authoring endpoint, example: your-resource-name.api.cognitive.microsoft.com
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

1. Reemplace los valores a partir de `YOUR-` por sus propios valores.

    |Information|Propósito|
    |--|--|
    |`YOUR-KEY`|La clave de creación de 32 caracteres.|
    |`YOUR-ENDPOINT`| El punto de conexión de la dirección URL de creación. Por ejemplo, `replace-with-your-resource-name.api.cognitive.microsoft.com`. El nombre del recurso se establece al crear el recurso.|
    |`YOUR-APP-ID`| El identificador de la aplicación de LUIS. |

    Las claves y recursos asignados son visibles en el portal de LUIS, en la sección Administrar de la página de **recursos de Azure**. El identificador de la aplicación está disponible en la misma sección Administrar, en la página **Configuración de la aplicación**.

1. Con un símbolo del sistema en el mismo directorio que donde creó el archivo, escriba el siguiente comando para ejecutar el archivo:

    ```console
    python model.py
    ```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este inicio rápido, elimine el archivo del sistema de archivos.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados para una aplicación](../luis-concept-best-practices.md)
