---
title: 'Creación de la primera aplicación de Language Understanding (LUIS) en 10 minutos: Cognitive Services LUIS | Microsoft Docs'
description: En esta guía de inicio rápido, creará una aplicación de LUIS que utiliza el dominio pregenerado`HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 08/22/2018
ms.author: diberry
ms.openlocfilehash: 457f23936dec0cf85e9aebbf3e54bba37c2f3ca3
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "43771643"
---
# <a name="quickstart-use-prebuilt-home-automation-app"></a>Guía de inicio rápido: uso de la aplicación de automatización del hogar pregenerada

En esta guía de inicio rápido, creará una aplicación de LUIS que utiliza el dominio pregenerado`HomeAutomation` para encender y apagar las luces y los dispositivos. Este dominio pregenerado le proporciona intenciones, entidades y expresiones de ejemplo. Cuando termine, tendrá un punto de conexión LUIS que se ejecuta en la nube.

## <a name="prerequisites"></a>Requisitos previos

Para este artículo, necesita una cuenta gratuita de LUIS creada en el portal de LUIS en [http://www.luis.ai](http://www.luis.ai). 

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

Seleccione **Intents** (Intenciones) en el panel de navegación izquierdo para revisar las intenciones del dominio HomeAutomation. 

[![](media/luis-quickstart-new-app/home-automation-intents.png "Captura de pantalla de la lista Intents (Intenciones) con el los nombres de intención en la tabla resaltados")](media/luis-quickstart-new-app/home-automation-intents.png)

Cada intención tiene expresiones de ejemplo.

> [!NOTE]
> **None** (Ninguna) es una intención que proporcionan todas las aplicaciones de LUIS. Se usa para controlar las expresiones que no corresponden a la funcionalidad que proporciona la aplicación. 

Seleccione la intención **HomeAutomation.TurnOff**. Puede ver que la intención contiene una lista de expresiones que están etiquetados con las entidades.

[![](media/luis-quickstart-new-app/home-automation-turnon.png "Captura de pantalla de la intención HomeAutomation.TurnOff")](media/luis-quickstart-new-app/home-automation-turnon.png)

## <a name="train-your-app"></a>Entrenamiento de la aplicación

Haga clic en **Train** (Entrenar) en la barra de navegación superior.

[![](media/luis-quickstart-new-app/trained.png "Captura de pantalla de la intención HomeAutomation.TurnOff con notificación de éxito verde")](media/luis-quickstart-new-app/trained.png)

## <a name="test-your-app"></a>Prueba de la aplicación
Una vez que haya entrenado a su aplicación, puede probarla. Seleccione **Test** (Prueba) en la barra de navegación superior. Escriba una expresión de prueba, como "Turn off the lights" (Apagar las luces) en el panel de prueba interactiva y presione ENTRAR. 

```
Turn off the lights
```

Compruebe que la intención con puntuación más alta se corresponde con la intención que espera para cada expresión de prueba.

En este ejemplo, "Turn off the lights" está identificada correctamente como la intención con puntuación más alta de "HomeAutomation.TurnOff".

[![](media/luis-quickstart-new-app/test.png "Captura de pantalla del panel de prueba con expresión resaltada")](media/luis-quickstart-new-app/test.png)


Vuelva a seleccionar **Test** (Prueba) para contraer el panel de prueba. 

## <a name="publish-your-app"></a>Publicación de la aplicación
Seleccione **Publish** (Publicar) en el panel de navegación superior. 

[![](media/luis-quickstart-new-app/publish.png "Captura de pantalla de la aplicación, con el botón Publish (Publicar) resaltado")](media/luis-quickstart-new-app/publish.png)

Seleccione el espacio de producción y haga clic en el botón **Publicar**.

La barra de notificación verde situada en la parte superior indica que la aplicación se publicó correctamente.

[![](media/luis-quickstart-new-app/published.png "Captura de pantalla de la aplicación con éxito en la publicación")](media/luis-quickstart-new-app/published.png)

Después de haber realizado correctamente la publicación, puede usar la dirección URL del punto de conexión que se muestra en la página **Publish app** (Publicar aplicación).

[![](media/luis-quickstart-new-app/endpoint.png "Captura de pantalla de la página Publish (Publicar) con la dirección URL del punto de conexión resaltada")](media/luis-quickstart-new-app/endpoint.png)

## <a name="use-your-app"></a>Uso de la aplicación
Puede probar el punto de conexión publicado en un explorador utilizando la dirección URL generada. Abra esta URL en el explorador, establezca el parámetro de dirección URL "& q" con la consulta de prueba. Por ejemplo, agregue `turn off the living room light` al final de la dirección URL y presione ENTRAR. El navegador muestra la respuesta JSON de su punto de conexión HTTP.


[![](media/luis-quickstart-new-app/turn-off-living-room.png "Captura de pantalla del explorador con el resultado JSON detecta la intención de TurnOff")](media/luis-quickstart-new-app/turn-off-living-room.png)

## <a name="clean-up-resources"></a>Limpieza de recursos
Cuando ya no sea necesaria, elimine la aplicación de LUIS. Para ello, seleccione el botón de puntos suspensivos (***...***) situado a la derecha del nombre de la aplicación en la lista de aplicaciones y haga clic en **Delete** (Eliminar). En el cuadro de diálogo emergente **Delete app?** (¿Eliminar aplicación?), haga clic en **Ok** (Aceptar).

## <a name="next-steps"></a>Pasos siguientes

Puede llamar al punto de conexión desde el código:

> [!div class="nextstepaction"]
> [Llamada a un punto de conexión de LUIS mediante código](luis-get-started-cs-get-intent.md)
