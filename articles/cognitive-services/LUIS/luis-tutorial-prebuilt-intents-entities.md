---
title: 'Intenciones y entidades precompiladas: LUIS'
titleSuffix: Azure Cognitive Services
description: En este tutorial, agregue intenciones y entidades creadas previamente a la aplicación para obtener de manera inmediata predicciones de intenciones y extracciones de datos. No necesita etiquetar ninguna expresión con entidades pregeneradas. La entidad se detecta automáticamente.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 08/20/2019
ms.author: diberry
ms.openlocfilehash: 4697bad15a374bed0de08b7cabc5aceaad7f1259
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876704"
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

1. Seleccione todas las intenciones y, luego, **Done** (Listo). Estas intenciones son útiles para determinar, en la conversación, dónde está el usuario y qué es lo que pide que se haga. 

## <a name="add-prebuilt-entities-to-help-with-common-data-type-extraction"></a>Incorporación de entidades creadas previamente para ayudarle con la extracción de tipos de datos habituales

LUIS proporciona varias entidades creadas previamente para la extracción de datos comunes. 

1. Seleccione **Entities** (Entidades) en el menú de navegación izquierdo.

1. Seleccione el botón **Add prebuilt entity** (Agregar entidad creada previamente).

1. Seleccione las siguientes entidades de la lista de entidades creadas previamente y, después, seleccione **Done** (Listo):

   * **[PersonName](luis-reference-prebuilt-person.md)** 
   * **[GeographyV2](luis-reference-prebuilt-geographyV2.md)**

     ![Captura de pantalla del número seleccionado en el cuadro de diálogo de entidades precompiladas](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

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
      "query": "I want to cancel my trip to Seattle to see Bob Smith.",
      "topScoringIntent": {
        "intent": "Utilities.ReadAloud",
        "score": 0.100361854
      },
      "intents": [
        {
          "intent": "Utilities.ReadAloud",
          "score": 0.100361854
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.08102781
        },
        {
          "intent": "Utilities.SelectNone",
          "score": 0.0398852825
        },
        {
          "intent": "Utilities.Cancel",
          "score": 0.0277276486
        },
        {
          "intent": "Utilities.SelectItem",
          "score": 0.0220712926
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0145813478
        },
        {
          "intent": "None",
          "score": 0.012434179
        },
        {
          "intent": "Utilities.Escalate",
          "score": 0.0122632384
        },
        {
          "intent": "Utilities.ShowNext",
          "score": 0.008534077
        },
        {
          "intent": "Utilities.ShowPrevious",
          "score": 0.00547111453
        },
        {
          "intent": "Utilities.SelectAny",
          "score": 0.00152912608
        },
        {
          "intent": "Utilities.Repeat",
          "score": 0.0005556819
        },
        {
          "intent": "Utilities.FinishTask",
          "score": 0.000169488427
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.000149565312
        },
        {
          "intent": "Utilities.GoBack",
          "score": 0.000141017343
        },
        {
          "intent": "Utilities.Reject",
          "score": 6.27324E-06
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

