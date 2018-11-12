---
title: Diez minutos a su primera aplicación de LUIS
titleSuffix: Azure Cognitive Services
description: Cree una aplicación de LUIS que utiliza el dominio previamente creado `HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: d7c80cce3b046bb6c8e4412272c7d166bc038d9c
ms.sourcegitcommit: ba4570d778187a975645a45920d1d631139ac36e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51282125"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Guía de inicio rápido: uso de la aplicación de automatización del hogar pregenerada

En esta guía de inicio rápido, creará una aplicación de LUIS que utiliza el dominio pregenerado`HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.

## <a name="prerequisites"></a>Requisitos previos

Para este artículo, necesita una cuenta gratuita de LUIS creada en el portal de LUIS en [https://www.luis.ai](https://www.luis.ai). 

## <a name="create-a-new-app"></a>Creación de una nueva aplicación
Las aplicaciones se pueden crear y administrar en **My Apps** (Mis aplicaciones). 

1. Inicie sesión en el portal de LUIS.

2. Seleccione **Create new app** (Crear nueva aplicación).

    [![](media/luis-quickstart-new-app/app-list.png "Captura de pantalla de una lista de aplicaciones")](media/luis-quickstart-new-app/app-list.png)

3. En el cuadro de diálogo asigne el nombre "Home Automation" a la aplicación.

    [![](media/luis-quickstart-new-app/create-new-app-dialog.png "Captura de pantalla del cuadro de diálogo emergente Create new app (Crear aplicación)")](media/luis-quickstart-new-app/create-new-app-dialog.png)

4. Elija la referencia cultural de la aplicación. Para esta aplicación Home Automation, elija el inglés. A continuación, seleccione **Done** (Listo). LUIS crea la aplicación Home Automation. 

    >[!NOTE]
    >La referencia cultural no se puede cambiar una vez creada la aplicación. 

## <a name="add-prebuilt-domain"></a>Incorporación de un dominio creado previamente

Seleccione **Prebuilt domains** (Dominios pregenerados) en el panel de navegación izquierdo. A continuación, haga una búsqueda con "Home". Seleccione **Add domain** (Agregar dominio).

[![](media/luis-quickstart-new-app/home-automation.png "Captura de pantalla del dominio Home Automation indicado en el menú de dominios pregenerados")](media/luis-quickstart-new-app/home-automation.png)

Cuando el dominio se agrega correctamente, el cuadro de dominio pregenerado muestra un botón **Remove domain** (Quitar dominio).

[![](media/luis-quickstart-new-app/remove-domain.png "Captura de pantalla del dominio Home Automation con el botón Remove domain (Quitar dominio)")](media/luis-quickstart-new-app/remove-domain.png)

## <a name="intents-and-entities"></a>Intenciones y entidades

Seleccione **Intents** (Intenciones) en el panel de navegación izquierdo para revisar las intenciones del dominio HomeAutomation. Cada intención tiene expresiones de ejemplo.

> [!NOTE]
> **None** (Ninguna) es una intención que proporcionan todas las aplicaciones de LUIS. Se usa para controlar las expresiones que no corresponden a la funcionalidad que proporciona la aplicación. 

Seleccione la intención **HomeAutomation.TurnOff**. Puede ver que la intención contiene una lista de expresiones que están etiquetados con las entidades.

[![](media/luis-quickstart-new-app/home-automation-turnon.png "Captura de pantalla de la intención HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-the-luis-app"></a>Entrenamiento de la aplicación de LUIS

[!INCLUDE [LUIS How to Train steps](../../../includes/cognitive-services-luis-tutorial-how-to-train.md)]

## <a name="test-your-app"></a>Prueba de la aplicación
Una vez que haya entrenado a su aplicación, puede probarla. Seleccione **Test** (Prueba) en la barra de navegación superior. Escriba una expresión de prueba, como "Turn off the lights" (Apagar las luces) en el panel de prueba interactiva y presione ENTRAR. 

```
Turn off the lights
```

Compruebe que la intención con puntuación más alta se corresponde con la intención que espera para cada expresión de prueba.

En este ejemplo, "Turn off the lights" está identificada correctamente como la intención con puntuación más alta de "HomeAutomation.TurnOff".

[![](media/luis-quickstart-new-app/test.png "Captura de pantalla del panel de prueba con expresión resaltada")](media/luis-quickstart-new-app/test.png)


Vuelva a seleccionar **Test** (Prueba) para contraer el panel de prueba. 

<a name="publish-your-app"></a>

## <a name="publish-the-app-to-get-the-endpoint-url"></a>Publicación de la aplicación para obtener la dirección URL del punto de conexión

[!INCLUDE[LUIS How to Publish steps](../../../includes/cognitive-services-luis-tutorial-how-to-publish.md)]

## <a name="query-the-endpoint-with-a-different-utterance"></a>Consulta del punto de conexión con una expresión diferente

1. [!INCLUDE[LUIS How to get endpoint first step](../../../includes/cognitive-services-luis-tutorial-how-to-get-endpoint.md)] 

2. Vaya al final de la dirección URL en la dirección, escriba `turn off the living room light` y presione Intro. El navegador muestra la respuesta JSON de su punto de conexión HTTP.

    [![](media/luis-quickstart-new-app/turn-off-living-room.png "Captura de pantalla del explorador con el resultado JSON detecta la intención de TurnOff")](media/luis-quickstart-new-app/turn-off-living-room.png)
    
## <a name="clean-up-resources"></a>Limpieza de recursos

[!INCLUDE[LUIS How to clean up resources](../../../includes/cognitive-services-luis-tutorial-how-to-clean-up-resources.md)]

## <a name="next-steps"></a>Pasos siguientes

Puede llamar al punto de conexión desde el código:

> [!div class="nextstepaction"]
> [Llamada a un punto de conexión de LUIS mediante código](luis-get-started-cs-get-intent.md)
