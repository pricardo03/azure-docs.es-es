---
title: 'Implementación de un bot de Conversation Learner: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Aprenda a implementar un bot de Conversation Learner.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 77cc998227d996a6e52b1b5629204da5dc735ede
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381731"
---
# <a name="how-to-deploy-a-conversation-learner-bot"></a>Implementación de un bot de Conversation Learner

En este documento se explica cómo implementar un bot de Conversation Learner, tanto localmente como en Azure.

## <a name="prerequisite-determine-the-application-id"></a>Requisito previo: determinar el identificador de la aplicación 

Para ejecutar un bot fuera de la interfaz de usuario de Conversation Learner, es preciso establecer el identificador de aplicación de Conversation Learner que va a usar el bot, es decir, el identificador del modelo de aprendizaje automático en la nube de Conversation Learner  (por el contrario, cuando el bot se ejecuta el componente a través de la interfaz de usuario de Conversation Learner, es dicha interfaz la que elige el identificador de la aplicación).  

Así es como se obtiene el identificador de aplicación:

1. Inicie tanto el bot como la interfaz de usuario de Conversation Learner.  Para ver las instrucciones completas consulte la Guía de inicio rápido, pero para resumir:

    En una ventana Comandos:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

    En otra ventana Comandos:

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

2. Abra el explorador en http://localhost:5050 

3. Haga clic en la aplicación Conversation Learner cuyo identificador desea obtener

4. Haga clic en "Settings" (Configuración) en la barra de navegación de la izquierda.

5. El GUID "App ID" (Identificador de la aplicación) se muestra cerca de la parte superior de la página.

## <a name="option-1-deploying-a-conversation-learner-bot-to-run-locally"></a>Opción1: Implementación de un bot de Conversation Learner para que se ejecute localmente

Esto implementa un componente en el equipo local y muestra cómo acceder a él mediante el emulador de Bot Framework.

### <a name="configure-your-bot-for-access-outside-the-conversation-learner-ui"></a>Configuración del bot para el acceso fuera de la interfaz de usuario de Conversation Learner

Cuando ejecute un bot localmente, agregue el identificador de la aplicación al archivo `.env` del bot:

    ```
    CONVERSATION_LEARNER_APP_ID=<YOUR_APP_ID>
    ```

Luego, inicie el bot:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

El bot se ejecuta localmente.  Puede acceder a él con el emulador de Bot Framework.

### <a name="download-and-install-the-emulator"></a>Descarga e instalación del emulador

    ```
    git clone https://github.com/Microsoft/BotFramework-Emulator
    npm install
    npm run build
    npm start
    ```

### <a name="connect-the-emulator-to-your-bot"></a>Conexión del emulador al bot

1. En la parte superior izquierda del emulador, en el cuadro "Enter your endpoint URL" (Escriba la URL del punto de conexión), escriba `http://127.0.0.1:3978/api/messages`.  Deje los demás campos en blanco y haga clic en "Connect" (Conectar).

2. Ya está conversando con el bot.

## <a name="option-2-deploy-to-azure"></a>Opción 2: Implementación en Azure

Publique el bot de Conversation Learner de forma similar a como publicaría cualquier otro bot. En un nivel alto, cargue el código en un sitio web hospedado, establezca los valores de configuración adecuados y registre el bot en varios canales. Encontrará instrucciones detalladas en este vídeo, en el que se muestra cómo publicar un bot mediante Azure Bot Service.

Una vez que el bot se implementa y está en ejecución es posible conectarle distintos canales como Facebook, Teams, Skype, etc. mediante Azure Bot Channel Registration. Para obtener documentación acerca de dicho proceso, consulte: https://docs.microsoft.com/en-us/bot-framework/bot-service-quickstart-registration

A continuación encontrará instrucciones detalladas para implementar un bot de Conversation Learner en Azure.  En estas instrucciones se da por hecho que el origen del bot está disponible desde un origen en la nube, como VSTS, GitHub, BitBucket o OneDrive, y que configurará el bot para una implementación continua.

1. Inicie sesión en Azure Portal en https://portal.azure.com

2. Cree un nuevo recurso de "Web App Bot" 

    1. Asigne un nombre al bot
    2. Haga clic en "Bot Template" (Plantilla de bot), elija "Node.js", elija "Basic" (Básico) y luego haga clic en el botón "Select" (Seleccionar)
    3. Haga clic en "create" (crear) para crear el recurso de Web App Bot.
    4. Espere a que se cree el recurso de Web App Bot.

3. En Azure Portal, edite el recurso de Web App Bot que acaba de crear.

    1. Haga clic en el elemento de navegación "Application Settings" (Configuración de la aplicación) de la izquierda
    1. Desplácese hasta la sección "App Settings" (Configuración de la aplicación)
    2. Agregue estos valores:

        Variable de entorno | value
        --- | --- 
        CONVERSATION_LEARNER_SERVICE_URI | "https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/"
        CONVERSATION_LEARNER_APP_ID      | GUID de identificador de la aplicación, obtenido de la interfaz de usuario de Conversation Learner en la "configuración" de la aplicación >
        LUIS_AUTHORING_KEY               | Clave de creación de LUIS para esta aplicación
    
    4. Haga clic en "Save"(Guardar) cerca de la parte superior de la página
    5. Abra el elemento de navegación "Build" (Compilar) de la izquierda
    6. Haga clic en "Configure continuous deployment" (Configurar implementación continua) 
    7. Haga clic en el icono "Setup" (Instalación) en las implementaciones
    8. Haga clic en "Required Settings" (Valores obligatorios)
    9. Seleccione el origen en el que está disponible el código del bot y configure dicho origen.
