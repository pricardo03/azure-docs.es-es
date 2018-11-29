---
title: 'Tutorial 3: Datos coincidentes con expresiones regulares, extracción de datos con formato correcto'
titleSuffix: Azure Cognitive Services
description: Extraiga datos con formato de forma coherente desde una expresión con la entidad de expresión regular.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: 5e8345deb629d293d9673819893181e652d5dbb9
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52423425"
---
# <a name="tutorial-3-extract-well-formatted-data"></a>Tutorial 3: Extracción de datos con formato correcto
En este tutorial, modifique la aplicación de recursos humanos para extraer datos con formato de forma coherente desde una expresión con la entidad **Expresión regular**.

El propósito de una entidad es extraer datos importantes contenidos en la expresión. El uso de la aplicación de la entidad de expresión regular es extraer los números del formulario de Recursos humanos con formato desde una expresión. Aunque la intención de la expresión siempre se determina con el aprendizaje automático, este tipo específico de entidad no se aprende de manera automática. 

**Las expresiones de ejemplo incluyen:**

```
Where is HRF-123456?
Who authored HRF-123234?
HRF-456098 is published in French?
HRF-456098
HRF-456098 date?
HRF-456098 title?
```
 
Una expresión regular es una buena opción para este tipo de datos cuando:

* los datos tienen el formato correcto.

**En este tutorial, aprenderá lo siguiente:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Usar la aplicación del tutorial existente
> * Adición de la intención FindForm
> * Incorporación de entidades de expresiones regulares 
> * Train
> * Publicar
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Usar una aplicación existente
Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Si no tiene la aplicación HumanResources del tutorial anterior, siga estos pasos:

1. Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-prebuilts-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `regex`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL. 

## <a name="findform-intent"></a>Intención FindForm

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Haga clic en **Create new intent** (Crear intención). 

3. Escriba `FindForm` en el cuadro de diálogo emergente y seleccione **Done** (Listo). 

    ![Captura de pantalla del cuadro de diálogo de creación de nueva intención con las utilidades en el cuadro de búsqueda](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

4. Agregue expresiones de ejemplo a la intención.

    |Expresiones de ejemplo|
    |--|
    |¿Cuál es la dirección URL de hrf-123456?|
    |¿Dónde está hrf-345678?|
    |¿Cuándo se ha actualizado hrf-456098?|
    |¿John Smith ha actualizado hrf 234639 la semana pasada?|
    |¿Cuántas versiones de hrf 345123 existen?|
    |¿Quién tiene que autorizar el formulario hrf-123456?|
    |¿Cuántas personas tienen que cerrar la sesión en hrf 345678?|
    |¿fecha de hrf 234123?|
    |¿autor de hrf 546234?|
    |¿título de hrf 456234?|

    [ ![Captura de pantalla de la página Intent (Intención) con nuevas expresiones resaltadas](./media/luis-quickstart-intents-regex-entity/findform-intent.png) ](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    La aplicación tiene una entidad de número creada previamente agregada del tutorial anterior, por lo que cada número de formulario está etiquetado. Esto puede ser suficiente para la aplicación cliente, pero el número no se etiquetará con el tipo de número. La creación de una nueva entidad con un nombre apropiado permite a la aplicación cliente procesar la entidad apropiadamente cuando se devuelve desde LUIS.

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="regular-expression-entity"></a>Entidad de expresión regular 
La entidad de expresión regular para que coincida con el número de formulario es `hrf-[0-9]{6}`. Esta expresión regular coincide con los caracteres literales `hrf-` pero omite variantes de mayúsculas y minúsculas y la referencia cultural. Coincide exactamente con dígitos de 0 a 9, para 6 dígitos exactamente.

HRF significa `human resources form`.

LUIS tokeniza la expresión cuando esta se agrega a una intención. La tokenización para estas expresiones agrega espacios antes y después del guion, `Where is HRF - 123456?` La expresión regular se aplica a la expresión en su forma sin formato, antes de que se tokenice. Debido a que se aplica al formulario _sin formato_, la expresión regular no tiene que tratar con límites de palabras. 

Cree una entidad de expresión regular para decirle a LUIS qué es un formato de número HRF en los siguientes pasos:

1. Seleccione **Entities** (Entidades) en el panel izquierdo.

2. Seleccione **Create new entity** (Crear nueva entidad) en la página Entities (Entidades). 

3. En el cuadro de diálogo emergente, introduzca el nombre de la nueva entidad `HRF-number`, seleccione **RegEx** como tipo de entidad, especifique `hrf-[0-9]{6}` como el valor **Regex** y, después, seleccione **Done** (Listo).

    ![Captura de pantalla del cuadro de diálogo emergente que establece las propiedades de la nueva entidad](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

4. Seleccione **Intents** (Intenciones) en el menú de la izquierda, después la intención **FindForm** para ver la expresión regular etiquetada en la expresiones. 

    [![Captura de pantalla de la expresión de etiqueta con la entidad existente y el patrón de la expresión regular](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Debido a que la entidad no es una entidad que se aprende automáticamente, la etiqueta se aplica a las expresiones y se muestra en el sitio web de LUIS tan pronto como se crea.

## <a name="train"></a>Train

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obtención de intenciones y entidades del punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la dirección y escriba `When were HRF-123456 and hrf-234567 published in the last year?`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `FindForm` con los dos números de formulario `HRF-123456` y `hrf-234567`.

    ```JSON
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9993477
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9993477
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0206110049
        },
        {
          "intent": "GetJobInformation",
          "score": 0.00533067342
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.004215215
        },
        {
          "intent": "Utilities.Help",
          "score": 0.00209096959
        },
        {
          "intent": "None",
          "score": 0.0017655947
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.00109490135
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.0005704638
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.000525338168
        }
      ],
      "entities": [
        {
          "entity": "last year",
          "type": "builtin.datetimeV2.daterange",
          "startIndex": 53,
          "endIndex": 61,
          "resolution": {
            "values": [
              {
                "timex": "2017",
                "type": "daterange",
                "start": "2017-01-01",
                "end": "2018-01-01"
              }
            ]
          }
        },
        {
          "entity": "hrf-123456",
          "type": "HRF-number",
          "startIndex": 10,
          "endIndex": 19
        },
        {
          "entity": "hrf-234567",
          "type": "HRF-number",
          "startIndex": 25,
          "endIndex": 34
        },
        {
          "entity": "-123456",
          "type": "builtin.number",
          "startIndex": 13,
          "endIndex": 19,
          "resolution": {
            "value": "-123456"
          }
        },
        {
          "entity": "-234567",
          "type": "builtin.number",
          "startIndex": 28,
          "endIndex": 34,
          "resolution": {
            "value": "-234567"
          }
        }
      ]
    }
    ```

    Los números en la expresión se devuelven dos veces, una como la nueva entidad `hrf-number` y otra como una entidad creada previamente, `number`. Una expresión puede tener más de una entidad, y más de una del mismo tipo de entidad, como muestra este ejemplo. Mediante el uso de una entidad de expresión regular, LUIS extrae datos con nombre, lo que es más útil desde el punto de vista de programación para la aplicación cliente que recibe la respuesta de JSON.


## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes
Este tutorial se creó una nueva intención, se agregaron expresiones de ejemplo y luego se creó una entidad de expresión regular para extraer los datos con formato correcto en las expresiones. Después del aprendizaje y de la publicación de la aplicación, una consulta al punto de conexión identificó la intención y devolvió los datos extraídos.

> [!div class="nextstepaction"]
> [Obtenga información acerca de la entidad de la lista](luis-quickstart-intent-and-list-entity.md)

