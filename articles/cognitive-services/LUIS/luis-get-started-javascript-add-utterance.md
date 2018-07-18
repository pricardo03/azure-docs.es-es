---
title: Tutorial para aprender a agregar expresiones a una aplicación de LUIS mediante JavaScript | Microsoft Docs
description: En este tutorial, aprenderá a llamar a una aplicación de LUIS mediante JavaScript.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/18/2017
ms.author: v-geberr
ms.openlocfilehash: b6d021dcfdddb5449aa989c6aa06d7faf326befb
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265466"
---
# <a name="tutorial-add-utterances-to-app-using-javascript"></a>Tutorial: Adición de expresiones a una aplicación mediante JavaScript
En este tutorial, va a escribir un programa para agregar una expresión a una intención mediante las API de creación de Javascript.

<!-- green checkmark -->
> [!div class="checklist"]
> * Creación de un proyecto de consola de Visual Studio 
> * Adición de un método para llamar a la API de LUIS para agregar una expresión y entrenar la aplicación
> * Adición de un archivo JSON con expresiones de ejemplo para la intención de BookFlight
> * Ejecución de la consola y visualización del estado de entrenamiento de las expresiones

Para más información, consulte la documentación técnica de las API [agregar expresión de ejemplo a intención](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [entrenar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) y [estado del entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Para este artículo necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="prerequisites"></a>Requisitos previos
* Su [**clave de creación**](luis-concept-keys.md#authoring-key) de LUIS. 
* El **identificador de aplicación** y el **identificador de versión** de LUIS existentes. 
* Un archivo nuevo denominado `add-utterances.html` en VSCode.

> [!NOTE] 
> El archivo `add-utterances.html` completo está disponible en el repositorio [**LUIS-Samples** de Github](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/javascript/add-utterance.html).


## <a name="write-the-code"></a>Escritura del código
Cree `add-utterances.html` y agregue el siguiente código:

   [!code-javascript[Java Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/javascript/add-utterance.html "Java Dependencies")]

## <a name="view-in-browser"></a>Visualización en un explorador
1. Abra el archivo en un explorador.

2. Agregue el identificador de creación de LUIS, el identificador de aplicación de LUIS y cambie la versión, si no es `0.1`

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
Cuando haya terminado con el tutorial, quite Visual Studio y la aplicación de consola si no los necesita. 

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Integración de LUIS con un bot](luis-csharp-tutorial-build-bot-framework-sample.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website