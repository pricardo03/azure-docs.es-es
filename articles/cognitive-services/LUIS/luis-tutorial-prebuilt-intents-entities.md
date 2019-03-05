---
title: Intenciones y entidades precompiladas
titleSuffix: Azure Cognitive Services
description: En este tutorial, agregue intenciones y entidades creadas previamente a la aplicación para obtener de manera inmediata predicciones de intenciones y extracciones de datos. No necesita etiquetar ninguna expresión con entidades pregeneradas. La entidad se detecta automáticamente.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 12/21/2018
ms.author: diberry
ms.openlocfilehash: cf16f27a8d39871491b7cf46a509b9714a669667
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56873821"
---
# <a name="tutorial-identify-common-intents-and-entities"></a>Tutorial: Identificación de intenciones y entidades habituales

En este tutorial, agregue intenciones y entidades creadas previamente a una aplicación de tutorial de recursos humanos para obtener de manera inmediata predicciones de intenciones y extracciones de datos. No es necesario marcar ninguna expresión con entidades creadas previamente porque la entidad se detecta automáticamente.

Los modelos creados previamente (dominios, intenciones y entidades) ayudan a crear rápidamente el modelo.

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Crear nueva aplicación
> * Agregar intenciones creadas previamente 
> * Agregar entidades creadas previamente 
> * Train 
> * Publicar 
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="create-a-new-app"></a>Creación de una nueva aplicación

[!INCLUDE [Follow these steps to create a new LUIS app](../../../includes/cognitive-services-luis-create-new-app-steps.md)]


## <a name="add-prebuilt-intents-to-help-with-common-user-intentions"></a>Incorporación de intenciones creadas previamente para ayudarle con las intenciones de usuario comunes

LUIS proporciona varias intenciones creadas previamente para ayudarle con las intenciones de usuario comunes.  

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

1. Haga clic en **Add prebuilt domain intent** (Agregar intención de dominio creado previamente). 

1. Busque `Utilities`. 

    [![Captura de pantalla del cuadro de diálogo de intenciones creadas previamente con las utilidades en el cuadro de búsqueda](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

1. Seleccione las siguientes intenciones y haga clic en **Done** (Listo): 

    * Utilities.Cancel
    * Utilities.Confirm
    * Utilities.Help
    * Utilities.StartOver
    * Utilities.Stop

    Estas intenciones son útiles para determinar, en la conversación, dónde está el usuario y qué es lo que pide que se haga. 


## <a name="add-prebuilt-entities-to-help-with-common-data-type-extraction"></a>Incorporación de entidades creadas previamente para ayudarle con la extracción de tipos de datos habituales

LUIS proporciona varias entidades creadas previamente para la extracción de datos comunes. 

1. Seleccione **Entities** (Entidades) en el menú de navegación izquierdo.

1. Seleccione el botón **Add prebuilt entity** (Agregar entidad creada previamente).

1. Seleccione las siguientes entidades de la lista de entidades creadas previamente y, después, seleccione **Done** (Listo):

    * **[PersonName](luis-reference-prebuilt-person.md)** 
    * **[GeographyV2](luis-reference-prebuilt-geographyV2.md)**

    ![Captura de pantalla del cuadro de diálogo de la selección de número en entidades creadas previamente](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

    Estas entidades le ayudarán a incorporar el reconocimiento de nombres y lugares a la aplicación cliente.

## <a name="add-example-utterances-to-the-none-intent"></a>Adición de expresiones de ejemplo a la intención None 

[!INCLUDE [Follow these steps to add the None intent to the app](../../../includes/cognitive-services-luis-create-the-none-intent.md)]

## <a name="train-the-app-so-the-changes-to-the-intent-can-be-tested"></a>Entrenamiento de la aplicación para probar los cambios en la intención 

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish-the-app-so-the-trained-model-is-queryable-from-the-endpoint"></a>Publicación de la aplicación para que se pueda consultar al modelo entrenado desde el punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entity-prediction-from-endpoint"></a>Obtención de intención y predicción de entidad desde el punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

1. Vaya al final de la dirección URL en la barra de direcciones del explorador y escriba `I want to cancel my trip to Seattle to see Bob Smith`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

    ```json
    {
      "query": "I want to cancel my trip to Seattle to see Bob Smith",
      "topScoringIntent": {
        "intent": "Utilities.Cancel",
        "score": 0.807676256
      },
      "intents": [
        {
          "intent": "Utilities.Cancel",
          "score": 0.807676256
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0487322025
        },
        {
          "intent": "Utilities.Help",
          "score": 0.0208660364
        },
        {
          "intent": "None",
          "score": 0.008789532
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.006929268
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00136293867
        }
      ],
      "entities": [
        {
          "entity": "seattle",
          "type": "builtin.geographyV2.city",
          "startIndex": 28,
          "endIndex": 34
        },
        {
          "entity": "bob smith",
          "type": "builtin.personName",
          "startIndex": 43,
          "endIndex": 51
        }
      ]
    }
    ```

    El resultado predijo la intención Utilities.Cancel con una confianza del 80 % y extrajo los datos de nombre de ciudad y de persona. 


## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="related-information"></a>Información relacionada

Más información sobre los modelos creados previamente:

* [Dominios creados previamente](luis-reference-prebuilt-domains.md): estos son dominios comunes que reducen la creación general de aplicaciones de LUIS
* Intenciones creadas previamente: estas son las intenciones individuales de los dominios comunes. Puede agregar intenciones individualmente en lugar de agregar todo el dominio.
* [Entidades creadas previamente](luis-prebuilt-entities.md): estas resultan tipos de datos comunes útiles para la mayoría de aplicaciones de LUIS.

Más información acerca del trabajo con la aplicación de LUIS:

* [Cómo se realiza el entrenamiento](luis-how-to-train.md)
* [Publicación](luis-how-to-publish-app.md)
* [Prueba de la aplicación el portal de LUIS](luis-interactive-test.md)

## <a name="next-steps"></a>Pasos siguientes

Al agregar intenciones y entidades creadas previamente, la aplicación cliente puede establecer las intenciones comunes de los usuarios y extraer tipos de datos comunes.  

> [!div class="nextstepaction"]
> [Adición de una entidad de expresión regular a la aplicación](luis-quickstart-intents-regex-entity.md)

