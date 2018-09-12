---
title: Guía de inicio rápido para aprender a agregar expresiones a una aplicación de LUIS con JavaScript (Azure Cognitive Services) | Microsoft Docs
description: En esta guía de inicio rápido, aprenderá a llamar a una aplicación de LUIS mediante JavaScript.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/24/2018
ms.author: diberry
ms.openlocfilehash: ffc19d12c1d3fbb24c514ac87f298d1a52d23eb8
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43771887"
---
# <a name="quickstart-change-model-using-javascript"></a>Guía de inicio rápido: Cambio del modelo mediante JavaScript

[!include[Quickstart introduction for change model](../../../includes/cognitive-services-luis-qs-endpoint-intro-para.md)]

## <a name="prerequisites"></a>Requisitos previos

[!include[Quickstart prerequisites for changing model](../../../includes/cognitive-services-luis-qs-change-model-prereq.md)]
* [Visual Studio Code](https://code.visualstudio.com/)

[!include[Code is available in LUIS-Samples Github repo](../../../includes/cognitive-services-luis-qs-change-model-luis-repo-note.md)]

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!include[Quickstart explanation of example utterance JSON file](../../../includes/cognitive-services-luis-qs-change-model-json-ex-utt.md)]


## <a name="create-quickstart-code"></a>Creación de código de inicio rápido

Cree `add-utterances.html` y agregue el siguiente código:

   [!code-html[Html code](~/samples-luis/documentation-samples/quickstarts/change-model/javascript/add-utterance.html "Javascript code")]

## <a name="run-code"></a>Ejecución del código

1. Abra el archivo en un explorador.

2. Agregue su identificador de creación de LUIS o identificador de aplicación de LUIS.

3. Modifique la **matriz de expresiones** para agregarla a la aplicación. Se almacenan en la variable utteranceJSON. Cambie estos valores hasta ajustarse a sus necesidades de dominio y expresión. 

    ```json
    // example batch utterances
    var utteranceJSON = [
        {
            "text": "go to Seattle",
            "intentName": "BookFlight",
            "entityLabels": [
                {
                    "entityName": "Location::LocationTo",
                    "startCharIndex": 6,
                    "endCharIndex": 12
                }
            ]
        }
    ,
        {
            "text": "book a flight",
            "intentName": "BookFlight",
            "entityLabels": []
        }
    ];
    ```

4. Haga clic en el botón `Upload utterance`. Los resultados de LUIS se muestran debajo de los botones.

5. Seleccione el botón `Train model` para entrenar su aplicación con estas expresiones nuevas.

6. Seleccione el botón `Train Status` para ver el estado del entrenamiento. 

    ![Add-utterances.html](./media/luis-quickstart-javascript-add-utterance/add-utterance.png)

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando haya terminado con la guía de inicio rápido, quite todos los archivos creados en ella. 

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Integración de LUIS con un bot](luis-csharp-tutorial-build-bot-framework-sample.md)
