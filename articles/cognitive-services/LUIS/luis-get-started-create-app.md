---
title: 'Inicio rápido: Creación de una aplicación (LUIS)'
titleSuffix: Azure Cognitive Services
description: Cree una aplicación de LUIS que utiliza el dominio previamente creado `HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 09/27/2019
ms.author: diberry
ms.openlocfilehash: 748c51e74db20ac101dc2dff0d924567acded114
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703239"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Inicio rápido: Uso de automatización del hogar compilada previamente

En esta guía de inicio rápido, creará una aplicación de LUIS que utiliza el dominio pregenerado`HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.

## <a name="prerequisites"></a>Requisitos previos

Para este artículo, necesita una cuenta gratuita de LUIS creada en el portal de LUIS en [https://www.luis.ai](https://www.luis.ai). 

[!INCLUDE [Sign in to LUIS](./includes/sign-in-process.md)]

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
Las aplicaciones se pueden crear y administrar en **My Apps** (Mis aplicaciones). 

1. Seleccione **Create new app** (Crear nueva aplicación).

    [![Captura de pantalla de lista de aplicaciones](media/luis-quickstart-new-app/app-list.png "Screenshot of app list")](media/luis-quickstart-new-app/app-list.png)

1. En el cuadro de diálogo asigne el nombre "Home Automation" a la aplicación.

    [![Captura de pantalla del cuadro de diálogo emergente Crear nueva aplicación](media/luis-quickstart-new-app/create-new-app-dialog.png "Screenshot of Create new app pop-up dialog")](media/luis-quickstart-new-app/create-new-app-dialog.png)

1. Elija la referencia cultural de la aplicación. Para esta aplicación Home Automation, elija el inglés. A continuación, seleccione **Done** (Listo). LUIS crea la aplicación Home Automation. 

    >[!NOTE]
    >La referencia cultural no se puede cambiar una vez creada la aplicación. 

## <a name="add-prebuilt-domain"></a>Incorporación de un dominio creado previamente

Seleccione **Prebuilt domains** (Dominios pregenerados) en el panel de navegación izquierdo. A continuación, haga una búsqueda con "Home". Seleccione **Add domain** (Agregar dominio).

[![Captura de pantalla del dominio Domótica llamado en el menú de dominios creados previamente](media/luis-quickstart-new-app/home-automation.png "Screenshot of Home Automation domain called out in prebuilt domain menu")](media/luis-quickstart-new-app/home-automation.png)

Cuando el dominio se agrega correctamente, el cuadro de dominio pregenerado muestra un botón **Remove domain** (Quitar dominio).

[![Captura de pantalla del dominio Domótica con el botón Eliminar](media/luis-quickstart-new-app/remove-domain.png "Screenshot of Home Automation domain with remove button")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Intenciones y entidades

Seleccione **Intents** (Intenciones) en el panel de navegación izquierdo para revisar las intenciones del dominio HomeAutomation. Cada intención tiene expresiones de ejemplo.

![Captura de pantalla de la lista de intenciones de HomeAutomation](media/luis-quickstart-new-app/home-automation-intents.png "Captura de pantalla de la lista de intenciones de HomeAutomation")]

> [!NOTE]
> **None** (Ninguna) es una intención que proporcionan todas las aplicaciones de LUIS. Se usa para controlar las expresiones que no corresponden a la funcionalidad que proporciona la aplicación. 

Seleccione la intención **HomeAutomation.TurnOff**. Puede ver que la intención contiene una lista de expresiones que están etiquetados con las entidades.

[![Captura de pantalla de la intención HomeAutomation.TurnOff](media/luis-quickstart-new-app/home-automation-turnoff.png "Screenshot of HomeAutomation.TurnOff intent")](media/luis-quickstart-new-app/home-automation-turnoff.png)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-your-app"></a>Prueba de la aplicación
Una vez que haya entrenado a su aplicación, puede probarla. Seleccione **Test** (Prueba) en la barra de navegación superior. Escriba una expresión de prueba, como "Turn off the lights" (Apagar las luces) en el panel de prueba interactiva y presione ENTRAR. 

```
Turn off the lights
```

Compruebe que la intención con puntuación más alta se corresponde con la intención que espera para cada expresión de prueba.

En este ejemplo, `Turn off the lights` está identificada correctamente como la intención con puntuación más alta de **HomeAutomation.TurnOff**.

[![Captura de pantalla del panel Probar con una expresión resaltada](media/luis-quickstart-new-app/test.png "Screenshot of Test panel with utterance highlighted")](media/luis-quickstart-new-app/test.png)


Seleccione **Inspeccionar** para revisar más información sobre la predicción.

![Captura de pantalla del panel de prueba con expresión resaltada](media/luis-quickstart-new-app/review-test-inspection-pane-in-portal.png)

Vuelva a seleccionar **Test** (Prueba) para contraer el panel de prueba. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión

[!INCLUDE [LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-v2-api-prediction-endpoint"></a>Consulta del punto de conexión de predicción de la API V2

1. [!INCLUDE [LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

1. Vaya al final de la dirección URL en la dirección, escriba `turn off the living room light` y presione Intro. 

    #### <a name="v2-prediction-endpointtabv2"></a>[Punto de conexión de predicción de V2](#tab/V2)

    `https://<region>.api.cognitive.microsoft.com/luis/**v2.0**/apps/<appID>?subscription-key=<YOUR_KEY>&**q=<user-utterance-text>**`

    El navegador muestra la versión de la **API V2** de la respuesta JSON del punto de conexión HTTP.

    ```json
    {
      "query": "turn off the lights",
      "topScoringIntent": {
        "intent": "HomeAutomation.TurnOff",
        "score": 0.995867
      },
      "entities": [
        {
          "entity": "lights",
          "type": "HomeAutomation.DeviceType",
          "startIndex": 13,
          "endIndex": 18,
          "resolution": {
            "values": [
              "light"
            ]
          }
        }
      ]
    }
    ```
    
    #### <a name="v3-prediction-endpointtabv3"></a>[Punto de conexión de predicción de V3](#tab/V3)

    En el caso de una [consulta de API V3](luis-migration-api-v3.md), en el explorador, cambie la solicitud HTTPS del método GET y cambie los valores entre corchetes angulares pos los suyos propios.     

    `https://<region>.api.cognitive.microsoft.com/luis/**v3.0-preview**/apps/<appID>/**slots**/**production**/**predict**?subscription-key=<YOUR_KEY>&**query=<user-utterance-text>**`

    ```json
    {
        "query": "turn off the lights",
        "prediction": {
            "normalizedQuery": "turn off the lights",
            "topIntent": "HomeAutomation.TurnOff",
            "intents": {
                "HomeAutomation.TurnOff": {
                    "score": 0.99649024
                }
            },
            "entities": {
                "HomeAutomation.DeviceType": [
                    [
                        "light"
                    ]
                ]
            }
        }
    }
    ```


    Más información acerca del [punto de conexión de predicción de V3](luis-migration-api-v3.md).
    
    * * * 

## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE [LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Puede llamar al punto de conexión desde el código:

> [!div class="nextstepaction"]
> [Llamada a un punto de conexión de LUIS mediante código](luis-get-started-cs-get-intent.md)
