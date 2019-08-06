---
title: 'Análisis de opiniones: LUIS'
titleSuffix: Azure Cognitive Services
description: En este tutorial va a crear una aplicación que muestra cómo obtener opiniones positivas, negativas y neutrales de expresiones. Las opiniones se determinan a partir de la expresión completa.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/03/2019
ms.author: diberry
ms.openlocfilehash: ad1dba3c35c0f1fa5640e484319b584731843dd8
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563572"
---
# <a name="tutorial--get-sentiment-of-utterance"></a>Tutorial:  Obtención de opiniones de expresión

En este tutorial va a crear una aplicación que muestra cómo determinar opiniones positivas, negativas y neutrales de expresiones. Las opiniones se determinan a partir de la expresión completa.

**En este tutorial, aprenderá a:**

<!-- green checkmark -->
> [!div class="checklist"]
> * Creación de una nueva aplicación
> * Agregar análisis de opiniones como configuración de publicación
> * Entrenamiento de la aplicación
> * Publicación de aplicación
> * Obtener las opiniones de expresión del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="sentiment-analysis-is-a-publish-setting"></a>Análisis de sentimiento es una opción de publicación

Las expresiones siguientes muestran ejemplos de opiniones:

|Opinión|Score|Expresión|
|:--|:--|:--|
|positiva|0,91 |John W. Smith hizo un gran trabajo en la presentación en París.|
|positiva|0,84 |Los ingenieros de Seattle realizaron un excelente trabajo en el discurso de ventas de Parker.|

El análisis de opiniones es una configuración de publicación que se aplica a cada expresión. No es necesario buscar las palabras que indican la opinión en la expresión y marcarlas. 

Dado que es una configuración de publicación, no lo ve en las páginas de entidades o intenciones. Puede verlo en el panel de [prueba interactiva](luis-interactive-test.md#view-sentiment-results) o cuando se prueba en la dirección URL del punto de conexión. 


## <a name="create-a-new-app"></a>Creación de una nueva aplicación

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]

## <a name="add-personname-prebuilt-entity"></a>Incorporación de la entidad precompilada PersonName 

1. Seleccione **Compilar** en el menú de navegación.

1. Seleccione **Entities** (Entidades) en el menú de navegación izquierdo.

1. Seleccione el botón **Add prebuilt entity** (Agregar entidad creada previamente).

1. Seleccione la siguiente entidad de la lista de entidades precompiladas y luego seleccione **Done** (Listo):

   * **[PersonName](luis-reference-prebuilt-person.md)** 

     ![Captura de pantalla del cuadro de diálogo de la selección de número en entidades creadas previamente](./media/luis-quickstart-intent-and-sentiment-analysis/add-personname-prebuilt-entity.png)

## <a name="create-an-intent-to-determine-employee-feedback"></a>Creación de una intención para determinar los comentarios de los empleados

Agregue una nueva intención para capturar los comentarios de los miembros de la empresa. 

1. Seleccione **Intents** (Intenciones) en el panel izquierdo.

1. Haga clic en **Create new intent** (Crear intención).

1. Escriba el nombre de la intención nueva `EmployeeFeedback`.

    ![Cuadro de diálogo Create new intent (Crear nueva intención) con EmployeeFeedback como nombre](./media/luis-quickstart-intent-and-sentiment-analysis/hr-create-new-intent-ddl.png)

4. Agregue varias expresiones que indiquen que un empleado está haciendo algo bien o un área que necesita mejorar:

    |Grabaciones de voz|
    |--|
    |John Smith realizó un buen trabajo al darle la bienvenida a una compañera de trabajo después del permiso de maternidad.|
    |Jill Jones realizó un gran trabajo al consolar a un compañero de trabajo en momentos de dolor.|
    |Bob Barnes no tenía todas las facturas necesarias para los trámites.|
    |Todd Thomas entregó los formularios necesarios con un mes de retraso y sin firmas.|
    |Katherine Kelly no asistió a la importante reunión de marketing fuera de las instalaciones.|
    |Denise Dillard se perdió la reunión para las revisiones de junio.|
    |Mark Mathews fue la estrella de las ventas en Harvard.|
    |Walter Williams realizó un gran trabajo en la presentación en Stanford.|

    Seleccione **View options** (Ver opciones) y **Show entity values** (Mostrar valores de entidad) para ver los nombres.

    [![Captura de pantalla de la aplicación de LUIS con expresiones de ejemplo en la intención EmployeeFeedback](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png)](./media/luis-quickstart-intent-and-sentiment-analysis/hr-utterance-examples.png#lightbox)

## <a name="add-example-utterances-to-the-none-intent"></a>Incorporación de expresiones de ejemplo a la intención None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Entrenamiento de la aplicación para probar los cambios en la intención 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="configure-app-to-include-sentiment-analysis"></a>Configuración de la aplicación para que incluya el análisis de opiniones

1. Seleccione **Manage** (Administrar) en la navegación superior derecha y luego seleccione **Publish settings** (Publicar configuración) en el menú izquierdo.

1. Seleccione **Use sentiment analysis to determine if a user's utterance is positive, negative, or neutral** (Usar análisis de opiniones para determinar si la expresión de un usuario es positiva, negativa o neutral). para habilitar esta configuración. 

    ![Activación del Análisis de sentimiento como configuración de publicación](./media/luis-quickstart-intent-and-sentiment-analysis/turn-on-sentiment-analysis-as-publish-setting.png)

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Publicación de la aplicación para que se pueda consultar al modelo entrenado desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-the-sentiment-of-an-utterance-from-the-endpoint"></a>Obtención de la opinión de una expresión desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

1. Vaya al final de la dirección URL y escriba la siguiente expresión:

    `Jill Jones work with the media team on the public portal was amazing` 

    El último parámetro de la cadena de consulta es `q`, la expresión **query**. Esta expresión no es la misma que cualquiera de las expresiones etiquetadas, por lo que es una buena prueba y debería devolver la intención `EmployeeFeedback` con el análisis de sentimiento extraído.
    
    ```json
    {
      "query": "Jill Jones work with the media team on the public portal was amazing",
      "topScoringIntent": {
        "intent": "EmployeeFeedback",
        "score": 0.9616192
      },
      "intents": [
        {
          "intent": "EmployeeFeedback",
          "score": 0.9616192
        },
        {
          "intent": "None",
          "score": 0.09347677
        }
      ],
      "entities": [
        {
          "entity": "jill jones",
          "type": "builtin.personName",
          "startIndex": 0,
          "endIndex": 9
        }
      ],
      "sentimentAnalysis": {
        "label": "positive",
        "score": 0.8694164
      }
    }
    ```

    El análisis de sentimiento es positivo con una puntuación de 86 %. 

    Pruebe otra expresión, para lo que debe quitar el valor de `q` en la barra de direcciones del explorador: `William Jones did a terrible job presenting his ideas.` La puntuación de opiniones indica que si se devuelve una puntuación baja, la opinión es negativa`0.18597582`.

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Información relacionada

* [Text Analytics](../Text-Analytics/index.yml) de Cognitive Services ofrece Análisis de sentimiento. La característica está restringida a los [idiomas admitidos](luis-language-support.md##languages-supported) por Text Analytics.
* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)


## <a name="next-steps"></a>Pasos siguientes
Este tutorial agrega el análisis de opiniones como una configuración de publicación para extraer los valores de las opiniones de la expresión en su conjunto.

> [!div class="nextstepaction"] 
> [Revisión de las expresiones de punto de conexión en la aplicación de RR. HH.](luis-tutorial-review-endpoint-utterances.md) 

