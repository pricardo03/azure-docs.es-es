---
title: Tutorial para aprender a agregar expresiones a una aplicación de LUIS mediante PHP | Microsoft Docs
description: En este tutorial, aprenderá a llamar a una aplicación de LUIS mediante PHP.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 59150b7ed6782c28f243041be2ed6aa17e69cc01
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36263787"
---
# <a name="tutorial-add-utterances-to-app-using-php"></a>Tutorial: Adición de expresiones a una aplicación mediante PHP 
En este tutorial, va a escribir un programa para agregar una expresión a una intención mediante las API de creación de PHP.

<!-- green checkmark -->
> [!div class="checklist"]
> * Creación de un proyecto de consola de Visual Studio 
> * Adición de un método para llamar a la API de LUIS para agregar una expresión y entrenar la aplicación
> * Adición de un archivo JSON con expresiones de ejemplo para la intención de BookFlight
> * Ejecución de la consola y visualización del estado de entrenamiento de las expresiones

Para más información, consulte la documentación técnica de las API [agregar expresión de ejemplo a intención](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08), [entrenar](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c45) y [estado del entrenamiento](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c46).

Para este artículo necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="prerequisites"></a>requisitos previos

* Versión de [**PHP**](http://php.net/) más reciente.
* Asegúrese de que openssl está disponible como una dependencia de PHP.  
* Su **[clave de creación](luis-concept-keys.md#authoring-key)** de LUIS. Dicha clave se puede encontrar en la configuración de la cuenta en el sitio web de [LUIS](luis-reference-regions.md).
* El [**identificador de aplicación**](./luis-get-started-create-app.md) de LUIS existente. El identificador de aplicación se muestra en el panel de la aplicación. La aplicación de LUIS con las intenciones y las entidades utilizadas en el archivo `utterances.json` debe existir antes de ejecutar el código en `add-utterances.php`. El código de este artículo no crea las intenciones ni las entidades. Solo agrega las expresiones para las intenciones y entidades existentes. 
* El **identificador de versión** dentro de la aplicación que recibe las expresiones. El valor predeterminado es "0.1".
* Cree un archivo nuevo llamado proyecto `add-utterances.php` en VSCode.

> [!NOTE] 
> El archivo `add-utterances.cs` completo y un archivo `utterances.json` de ejemplo están disponibles en el repositorio [**LUIS-Samples** de Github](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/authoring-api-samples/php/).


## <a name="write-the-php-code"></a>Escritura del código PHP

Agregue las dependencias al archivo.

   [!code-php[PHP and LUIS Dependencies](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=10-29 "PHP and LUIS Dependencies")]

Agregue la solicitud GET que se usa para el estado de entrenamiento.

   [!code-php[SendGet](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=31-50 "SendGet")]

Agregue la solicitud POST que se usa para crear expresiones o iniciar el entrenamiento. 

   [!code-php[SendPost](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=52-72 "SendPost")]

Agregue la función `AddUtterances`.

   [!code-php[AddUtterances method](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=74-79 "AddUtterances method")]


Agregue la función `Train`. 

   [!code-php[Train](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=81-88 "Train")]

Agregue la función `Status`.

   [!code-php[Status](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=90-94 "Status")]

Para administrar los argumentos de línea de comandos, agregue el bloque de código principal.

   [!code-php[Main code](~/samples-luis/documentation-samples/authoring-api-samples/php/add-utterances.php?range=96-120 "Main code")]

## <a name="specify-utterances-to-add"></a>Especificación de las expresiones que se van a agregar
Cree y edite el archivo `utterances.json` para especificar la **matriz de expresiones** que desea agregar a la aplicación de LUIS. Las entidades y la intención ya **deben** estar en la aplicación de LUIS.

> [!NOTE]
> La aplicación de LUIS con las intenciones y las entidades utilizadas en el archivo `utterances.json` debe existir antes de ejecutar el código en `add-utterances.php`. El código de este artículo no crea las intenciones ni las entidades. Solo agrega las expresiones para las intenciones y entidades existentes.

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

Ejecute la aplicación desde una línea de comandos con PHP.

Al llamar a `add-utterances.php` usando solo utterance.json como argumento se agregan las expresiones a la aplicación de LUIS, pero no se entrenan.
````
> php add-utterances.php ./utterances.json
````

La llamada a la API de adición de expresiones devuelve el siguiente código JSON. El campo `response` está en este formato para las expresiones que se han agregado. `hasError` es false, lo que indica que la expresión se ha agregado.  

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
Llame a `add-utterance.php` con el argumento `-train` para enviar una solicitud de entrenamiento. 

````
> php add-utterances.php ./utterances.json -train
````

> [!NOTE]
> Las expresiones duplicadas no se vuelven a agregar, pero no provocan un error. `response` contiene el identificador de la expresión original.

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
Llame a la aplicación con el argumento `-status` para comprobar el estado de entrenamiento y mostrar los detalles del estado.

````
> php add-utterances.php -status
````

```
Requested training status.
[
   {
      "modelId": "eb2f117c-e10a-463e-90ea-1a0176660acc",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c1bdfbfc-e110-402e-b0cc-2af4112289fb",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "863023ec-2c96-4d68-9c44-34c1cbde8bc9",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "82702162-73ba-4ae9-a6f6-517b5244c555",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "37121f4c-4853-467f-a9f3-6dfc8cad2763",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "de421482-753e-42f5-a765-ad0a60f50d69",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "80f58a45-86f2-4e18-be3d-b60a2c88312e",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "c9eb9772-3b18-4d5f-a1e6-e0c31f91b390",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "2afec2ff-7c01-4423-bb0e-e5f6935afae8",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   },
   {
      "modelId": "95a81c87-0d7b-4251-8e07-f28d180886a1",
      "details": {
         "statusId": 0,
         "status": "Success",
         "exampleCount": 33,
         "trainingDateTime": "2017-11-20T18:09:11Z"
      }
   }
]
```

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando haya terminado con el tutorial, quite Visual Studio y la aplicación de consola si no los necesita. 

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Creación de una aplicación de LUIS mediante programación](luis-tutorial-node-import-utterances-csv.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website