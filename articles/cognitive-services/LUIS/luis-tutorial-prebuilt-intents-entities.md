---
title: Intenciones y entidades precompiladas
titleSuffix: Azure Cognitive Services
description: Agregue intenciones y entidades creadas previamente a la aplicación de tutorial de recursos humanos para obtener de manera inmediata predicciones de intenciones y extracciones de datos. No necesita etiquetar ninguna expresión con entidades pregeneradas. La entidad se detecta automáticamente.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 09/09/2018
ms.author: diberry
ms.openlocfilehash: b6fb603b84cdcf3cb0f75d0020fa2047a0a838d1
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074088"
---
# <a name="tutorial-2-identify-common-intents-and-entities"></a>Tutorial 2: Identificación de intenciones y entidades habituales
En este tutorial, modificará la aplicación HumanResources. Agregue intenciones y entidades creadas previamente a la aplicación de tutorial de recursos humanos para obtener de manera inmediata predicciones de intenciones y extracciones de datos. No es necesario etiquetar ninguna expresión con entidades creadas previamente porque la entidad se detecta automáticamente.

Los modelos creados previamente de dominios de asunto y tipos de datos comunes le ayudan a crear rápidamente el modelo, además de proporcionar un ejemplo del aspecto del modelo. 

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Usar la aplicación del tutorial existente
> * Agregar intenciones creadas previamente 
> * Agregar entidades creadas previamente 
> * Train 
> * Publicar 
> * Obtener intenciones y entidades del punto de conexión

[!INCLUDE [LUIS Free account](../../../includes/cognitive-services-luis-free-key-short.md)]

## <a name="use-existing-app"></a>Usar una aplicación existente
Continúe con la aplicación creada en el último tutorial, denominada **HumanResources**. 

Si no tiene la aplicación HumanResources del tutorial anterior, siga estos pasos:

1.  Descargue y guarde el [archivo JSON de la aplicación](https://github.com/Microsoft/LUIS-Samples/blob/master/documentation-samples/tutorials/custom-domain-intent-only-HumanResources.json).

2. Importe el archivo JSON en una aplicación nueva.

3. Desde la sección **Manage** (Administrar), en la pestaña **Versions** (Versiones), clone la versión y asígnele el nombre `prebuilts`. La clonación es una excelente manera de trabajar con distintas características de LUIS sin que afecte a la versión original. Dado que el nombre de la versión se usa como parte de la ruta de la dirección URL, el nombre no puede contener ningún carácter que no sea válido en una dirección URL. 

## <a name="add-prebuilt-intents"></a>Agregar intenciones creadas previamente
LUIS proporciona varias intenciones creadas previamente para ayudarle con las intenciones de usuario comunes.  

1. [!INCLUDE [Start in Build section](../../../includes/cognitive-services-luis-tutorial-build-section.md)]

2. Seleccione **Add prebuilt intent** (Agregar intención creada previamente). 

3. Busque `Utilities`. 

    [ ![Captura de pantalla del cuadro de diálogo de intenciones creadas previamente con las utilidades en el cuadro de búsqueda](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png)](./media/luis-tutorial-prebuilt-intents-and-entities/prebuilt-intent-utilities.png#lightbox)

4. Seleccione las siguientes intenciones y haga clic en **Done** (Listo): 

    * Utilities.Cancel
    * Utilities.Confirm
    * Utilities.Help
    * Utilities.StartOver
    * Utilities.Stop


## <a name="add-prebuilt-entities"></a>Agregar entidades creadas previamente
LUIS proporciona varias entidades creadas previamente para la extracción de datos comunes. 

1. Seleccione **Entities** (Entidades) en el menú de navegación izquierdo.

2. Seleccione el botón **Manage prebuilt entities** (Administrar entidades creadas previamente).

3. Seleccione **number** (número) y **datetimeV2** en la lista de entidades creadas previamente y, después, haga clic en **Done** (Listo).

    ![Captura de pantalla del cuadro de diálogo de la selección de número en entidades creadas previamente](./media/luis-tutorial-prebuilt-intents-and-entities/select-prebuilt-entities.png)

## <a name="train"></a>Train

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="publish"></a>Publicar

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="get-intent-and-entities-from-endpoint"></a>Obtención de intenciones y entidades del punto de conexión

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)]

2. Vaya al final de la dirección URL en la barra de direcciones del explorador y escriba `I want to cancel on March 3`. El último parámetro de la cadena de consulta es `q`, la expresión **query**. 

    ```json
    {
      "query": "I want to cancel on March 3",
      "topScoringIntent": {
        "intent": "Utilities.Cancel",
        "score": 0.7818295
      },
      "intents": [
        {
          "intent": "Utilities.Cancel",
          "score": 0.7818295
        },
        {
          "intent": "ApplyForJob",
          "score": 0.0237864349
        },
        {
          "intent": "GetJobInformation",
          "score": 0.017576348
        },
        {
          "intent": "Utilities.StartOver",
          "score": 0.0130122062
        },
        {
          "intent": "Utilities.Help",
          "score": 0.006731322
        },
        {
          "intent": "None",
          "score": 0.00524190161
        },
        {
          "intent": "Utilities.Stop",
          "score": 0.004912514
        },
        {
          "intent": "Utilities.Confirm",
          "score": 0.00092950504
        }
      ],
      "entities": [
        {
          "entity": "march 3",
          "type": "builtin.datetimeV2.date",
          "startIndex": 20,
          "endIndex": 26,
          "resolution": {
            "values": [
              {
                "timex": "XXXX-03-03",
                "type": "date",
                "value": "2018-03-03"
              },
              {
                "timex": "XXXX-03-03",
                "type": "date",
                "value": "2019-03-03"
              }
            ]
          }
        },
        {
          "entity": "3",
          "type": "builtin.number",
          "startIndex": 26,
          "endIndex": 26,
          "resolution": {
            "value": "3"
          }
        }
      ]
    }
    ```

    El resultado predice la intención Utilities.Cancel y extrae la fecha del 3 de marzo y el número 3. 

    Hay dos valores para el 3 de marzo porque la expresión no indicaba si el 3 de marzo es pasado o futuro. Depende de la aplicación cliente hacer una suposición o pedir aclaraciones, si es necesario. 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Al agregar intenciones y entidades creadas previamente, la aplicación cliente puede establecer las intenciones comunes de los usuarios y extraer tipos de datos comunes. 

> [!div class="nextstepaction"]
> [Adición de una entidad de expresión regular a la aplicación](luis-quickstart-intents-regex-entity.md)

