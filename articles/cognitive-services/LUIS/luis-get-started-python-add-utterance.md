---
title: Tutorial para aprender a agregar expresiones a una aplicación de LUIS mediante Python | Microsoft Docs
description: En este tutorial, aprenderá a llamar a una aplicación de LUIS mediante Python.
services: cognitive-services
author: v-geberr
manager: Kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 3e1ca2ea874b6b39ac8a1b1eaa94fe9ff1894ea3
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36264308"
---
# <a name="tutorial-add-utterances-to-app-using-python"></a>Tutorial: Adición de expresiones a una aplicación mediante Python
En este tutorial, va a escribir un programa para agregar una expresión a una intención mediante las API de creación de Python.

<!-- green checkmark -->
> [!div class="checklist"]
> * Creación de un proyecto de consola de Visual Studio 
> * Adición de un método para llamar a la API de LUIS para agregar una expresión y entrenar la aplicación
> * Adición de un archivo JSON con expresiones de ejemplo para la intención de BookFlight
> * Ejecución de la consola y visualización del estado de entrenamiento de las expresiones

Para más información, consulte la documentación técnica de las API [agregar expresión de ejemplo a intención](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45), [entrenar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) y [estado del entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Para este artículo necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="prerequisites"></a>requisitos previos

* [Python 3.6](https://www.python.org/downloads/) o versiones posteriores.
* **[Recomendado]**  [Visual Studio Code](https://code.visualstudio.com/) para IntelliSense y depuración.
* Su **[clave de creación](luis-concept-keys.md#authoring-key)** de LUIS. Dicha clave se puede encontrar en la configuración de la cuenta en el sitio web de [LUIS](luis-reference-regions.md).
* El [**identificador de aplicación**](./luis-get-started-create-app.md) de LUIS existente. El identificador de aplicación se muestra en el panel de la aplicación. La aplicación de LUIS con las intenciones y las entidades utilizadas en el archivo `utterances.json` debe existir antes de ejecutar el código en `add-utterances.js`. El código de este artículo no crea las intenciones ni las entidades. Solo agrega las expresiones para las intenciones y entidades existentes. 
* El **identificador de versión** dentro de la aplicación que recibe las expresiones. El valor predeterminado es "0.1".
* Cree un archivo nuevo llamado proyecto `add-utterances-3-6.py` en VSCode.

> [!NOTE] 
> El archivo `add-utterances-3-6.py` completo está disponible en el repositorio [**LUIS-Samples** de Github](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/authoring-api-samples/python).


## <a name="write-the-python-code"></a>Escritura del código de Python

1. Copie los siguientes fragmentos de código:

   [!code-python[Console app code that adds an utterance Python 3.6](~/samples-luis/documentation-samples/authoring-api-samples/python/add-utterances-3-6.py)]

## <a name="specify-utterances-to-add"></a>Especificación de las expresiones que se van a agregar
Cree y edite el archivo `utterances.json` para especificar la **matriz de expresiones** que desea agregar a la aplicación de LUIS. Las entidades y la intención ya **deben** estar en la aplicación de LUIS.

> [!NOTE]
> La aplicación de LUIS con las intenciones y las entidades utilizadas en el archivo `utterances.json` debe existir antes de ejecutar el código en `add-utterances.js`. El código de este artículo no crea las intenciones ni las entidades. Solo agrega las expresiones para las intenciones y entidades existentes.

El campo `text` contiene el texto de la expresión. El campo `intentName` debe corresponder al nombre de una intención en la aplicación de LUIS. El campo `entityLabels` es obligatorio. Si no desea etiquetar ninguna entidad, proporcione una lista vacía como se muestra en el ejemplo siguiente:

Si la lista entityLabels no está vacía, `startCharIndex` y `endCharIndex` necesitan marcar la entidad a la que se hace referencia en el campo `entityName`. Ambos índices son recuentos basados en cero, lo que significa que, en el ejemplo de arriba, 6 hace referencia a la "S" de Seattle y no al espacio antes de la S mayúscula.

```json
[
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
    },
    {
        "text": "book a flight",
        "intentName": "BookFlight",
        "entityLabels": []
    }
]
```

## <a name="add-an-utterance-from-the-command-line"></a>Adición de una expresión desde la línea de comandos

Ejecute la aplicación desde una línea de comandos con Python 3.6.

Al llamar a add-utterance sin argumentos, se agrega una expresión a la aplicación sin necesidad de entrenarla.

````
> python add-utterances-3-6.py
````

Este ejemplo crea un archivo `results.json` que contiene los resultados de la llamada a la API de expresiones. El campo `response` está en este formato para las expresiones que se han agregado. `hasError` es false, lo que indica que la expresión se ha agregado.  

```json
    "response": [
        {
            "value": {
                "UtteranceText": "go to seattle",
                "ExampleId": -5123383
            },
            "hasError": false
        },
        {
            "value": {
                "UtteranceText": "book a flight",
                "ExampleId": -169157
            },
            "hasError": false
        }
    ]
```

## <a name="add-an-utterance-and-train-from-the-command-line"></a>Adición y entrenamiento de una expresión desde la línea de comandos
Al llamar a add-utterance con el argumento `-train`, se envía una solicitud de entrenamiento y, posteriormente, se solicita el estado de entrenamiento. El estado se pone en cola inmediatamente después de que comience el entrenamiento. Los detalles sobre el estado se escriben en un archivo.

````
> python add-utterances-3-6.py -train
````

> [!NOTE]
> Las expresiones duplicadas no se vuelven a agregar, pero no provocan un error. `response` contiene el identificador de la expresión original.

Cuando se llama a la muestra con el argumento `-train`, se crea un archivo `training-results.json` que indica que la solicitud para entrenar la aplicación de LUIS se puso en cola correctamente. 

A continuación se muestra el resultado de una solicitud de entrenamiento correcta:
```json
{
    "request": null,
    "response": {
        "statusId": 9,
        "status": "Queued"
    }
}
```

Después de poner en cola la solicitud de entrenamiento, este puede tardar un momento en completarse.

## <a name="get-training-status-from-the-command-line"></a>Obtención del estado de entrenamiento desde la línea de comandos
Llame al ejemplo con el argumento `-status` para comprobar el estado de entrenamiento y escribir los detalles del estado en un archivo.

````
> python add-utterances-3-6.py -status
````
## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando haya terminado con el tutorial, quite Visual Studio y la aplicación de consola si no los necesita. 

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Creación de una aplicación de LUIS mediante programación](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website

