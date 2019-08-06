---
title: 'Entidad de expresión regular: LUIS'
titleSuffix: Azure Cognitive Services
description: Extraiga datos con formato de forma coherente desde una expresión con la entidad de expresión regular.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/03/2019
ms.author: diberry
ms.openlocfilehash: 958d880829e9524389a25f029415786231644c04
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563589"
---
# <a name="tutorial-get-well-formatted-data-from-the-utterance"></a>Tutorial: Obtención de datos con formato correcto a partir de la expresión
En este tutorial va a crear una aplicación para extraer datos con formato de forma coherente a partir de una expresión con la entidad de **expresión regular**.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Creación de una nueva aplicación 
> * Agregar intención
> * Incorporación de entidades de expresiones regulares 
> * Train
> * Publicar
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="regular-expression-entities"></a>Entidades de expresión regular

El uso que hace la aplicación de la entidad de expresión regular consiste en extraer números del formulario de Recursos humanos con formato correcto a partir de una expresión. Aunque la intención de la expresión siempre se determina con el aprendizaje automático, este tipo específico de entidad no se aprende de manera automática. 

**Las expresiones de ejemplo incluyen:**

|Expresiones de ejemplo|
|--|
|¿Dónde está el formulario HRF-123456?|
|¿Quién creó el formulario HRF-123234?|
|¿El formulario HRF-456098 se publicó en francés?|
|HRF 456098|
|¿Fecha de HRF 456098?|
 
Una expresión regular es una buena opción para este tipo de datos cuando:

* los datos tienen el formato correcto.


## <a name="create-a-new-app"></a>Creación de una nueva aplicación

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="create-intent-for-finding-form"></a>Creación de una intención para buscar el formulario

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Haga clic en **Create new intent** (Crear intención). 

1. Escriba `FindForm` en el cuadro de diálogo emergente y seleccione **Done** (Listo). 

    ![Captura de pantalla del cuadro de diálogo de creación de nueva intención con las utilidades en el cuadro de búsqueda](./media/luis-quickstart-intents-regex-entity/create-new-intent-ddl.png)

1. Agregue expresiones de ejemplo a la intención.

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

    [![Captura de pantalla de la página Intent (Intención) con nuevas expresiones resaltadas](./media/luis-quickstart-intents-regex-entity/findform-intent.png)](./media/luis-quickstart-intents-regex-entity/findform-intent.png#lightbox)

    [!INCLUDE [Do not use too few utterances](../../../includes/cognitive-services-luis-too-few-example-utterances.md)]  

## <a name="use-the-regular-expression-entity-for-well-formatted-data"></a>Uso de la entidad de expresión regular para datos con formato correcto
La entidad de expresión regular para que coincida con el número de formulario es `hrf-[0-9]{6}`. Esta expresión regular coincide con los caracteres literales `hrf-` pero omite variantes de mayúsculas y minúsculas y la referencia cultural. Coincide exactamente con dígitos de 0 a 9, para 6 dígitos exactamente.

HRF significa `human resources form`.

LUIS tokeniza la expresión cuando esta se agrega a una intención. La tokenización para estas expresiones agrega espacios antes y después del guion, `Where is HRF - 123456?` La expresión regular se aplica a la expresión en su forma sin formato, antes de que se tokenice. Debido a que se aplica al formulario _sin formato_, la expresión regular no tiene que tratar con límites de palabras. 

Cree una entidad de expresión regular para decirle a LUIS qué es un formato de número HRF en los siguientes pasos:

1. Seleccione **Entities** (Entidades) en el panel izquierdo.

1. Seleccione **Create new entity** (Crear nueva entidad) en la página Entities (Entidades). 

1. En el cuadro de diálogo emergente, introduzca el nombre de la nueva entidad `HRF-number`, seleccione **RegEx** como tipo de entidad, especifique `hrf-[0-9]{6}` como el valor **Regex** y, después, seleccione **Done** (Listo).

    ![Captura de pantalla del cuadro de diálogo emergente que establece las propiedades de la nueva entidad](./media/luis-quickstart-intents-regex-entity/create-regex-entity.png)

1. Seleccione **Intents** (Intenciones) en el menú de la izquierda, después la intención **FindForm** para ver la expresión regular etiquetada en la expresiones. 

    [![Captura de pantalla de la expresión de etiqueta con la entidad existente y el patrón de la expresión regular](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png)](./media/luis-quickstart-intents-regex-entity/labeled-utterances-for-entity.png#lightbox)

    Dado que la entidad no es una entidad que se aprende de manera automática, se aplica a las expresiones y se muestra en el sitio web de LUIS en cuanto se crea.

## <a name="add-example-utterances-to-the-none-intent"></a>Incorporación de expresiones de ejemplo a la intención None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-before-testing-or-publishing"></a>Entrenamiento de la aplicación antes de las pruebas o la publicación

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-to-query-from-the-endpoint"></a>Publicación de la aplicación en la consulta desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obtención de intención y predicción de entidad desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la dirección y escriba la siguiente expresión:

    `When were HRF-123456 and hrf-234567 published in the last year?`

    El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `FindForm` con los dos números de formulario `HRF-123456` y `hrf-234567`.

    ```json
    {
      "query": "When were HRF-123456 and hrf-234567 published in the last year?",
      "topScoringIntent": {
        "intent": "FindForm",
        "score": 0.9988884
      },
      "intents": [
        {
          "intent": "FindForm",
          "score": 0.9988884
        },
        {
          "intent": "None",
          "score": 0.00204812363
        }
      ],
      "entities": [
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
        }
      ]
    }
    ```

    Mediante el uso de una entidad de expresión regular, LUIS extrae datos con nombre, lo que es más útil desde el punto de vista de programación para la aplicación cliente que recibe la respuesta de JSON.


## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Información relacionada

* Conceptos de entidad de [expresión regular](luis-concept-entity-types.md#regular-expression-entity)
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)

## <a name="next-steps"></a>Pasos siguientes
Este tutorial se creó una nueva intención, se agregaron expresiones de ejemplo y luego se creó una entidad de expresión regular para extraer los datos con formato correcto en las expresiones. Después del aprendizaje y de la publicación de la aplicación, una consulta al punto de conexión identificó la intención y devolvió los datos extraídos.

> [!div class="nextstepaction"]
> [Obtenga información acerca de la entidad de la lista](luis-quickstart-intent-and-list-entity.md)

