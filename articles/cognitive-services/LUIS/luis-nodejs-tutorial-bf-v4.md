---
title: Bot - Node.js - v4
titleSuffix: Azure Cognitive Services
description: Use Node.js para crear un bot de chat que se integre con Language Understanding (LUIS). Este bot de chat utiliza la aplicación de recursos humanos para implementar con rapidez una solución de bot. El bot se compila con la versión 4 de Bot Framework y el bot de aplicación web de Azure.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: cjgronlund
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 01/30/2019
ms.author: diberry
ms.openlocfilehash: 8a24ffa4717244d8ce2ef507183cff06cbea5797
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55492239"
---
# <a name="tutorial-luis-bot-in-nodejs-with-the-bot-framework-4x-and-the-azure-web-app-bot"></a>Tutorial: Bot de LUIS en Node.js con Bot Framework 4.x y el bot de aplicación web de Azure
Con Node.js puede crear un bot de chat que se integre con Language Understanding (LUIS). Este bot usa la aplicación HomeAutomation para implementar una solución de bot. El bot se compila con el [bot de aplicación web](https://docs.microsoft.com/azure/bot-service/) de Azure y con la [versión 4 de Bot Framework](https://github.com/Microsoft/botbuilder-js).

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Crear un bot de aplicación web. Este proceso crea una nueva aplicación de LUIS.
> * Agregar un dominio creado previamente para el nuevo modelo de LUIS.
> * Descargar el proyecto creado por el servicio de bot web.
> * Iniciar el emulador y el bot localmente en el equipo.
> * Modificar el código del bot para nuevas intenciones de LUIS.
> * Ver los resultados de expresiones del bot.

## <a name="prerequisites"></a>Requisitos previos

* [Bot Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-web-app-bot"></a>Creación de una aplicación web

1. En [Azure Portal](https://portal.azure.com), haga clic en **Crear un recurso**.

2. En el cuadro de búsqueda, busque y seleccione **Bot de aplicación web**. Seleccione **Crear**.

3. En **Servicio de bots**, proporcione la información necesaria:

    |Configuración|Propósito|Configuración sugerida|
    |--|--|--|
    |Nombre del bot|Nombre del recurso|`luis-nodejs-bot-` + `<your-name>`, por ejemplo, `luis-nodejs-bot-johnsmith`|
    |Subscription|Suscripción donde se desea crear el bot.|Suscripción principal.
    |Grupos de recursos|Grupo lógico de recursos de Azure|Crear un nuevo grupo para almacenar todos los recursos utilizados con este bot, denominando el grupo `luis-nodejs-bot-resource-group`.|
    |Ubicación|Región de Azure: no tiene que ser la misma que la región de creación o publicación de LUIS.|`westus`|
    |Plan de tarifa|Se usa para los límites de solicitud de servicio y facturación.|`F0` es el plan gratuito.
    |Nombre de la aplicación|El nombre se usa como el subdominio cuando el bot se implementa en la nube (por ejemplo, humanresourcesbot.azurewebsites.net).|`luis-nodejs-bot-` + `<your-name>`, por ejemplo, `luis-nodejs-bot-johnsmith`|
    |Plantilla de bot|Configuración de Bot Framework: ver la tabla siguiente|
    |Ubicación de la aplicación de LUIS|Debe ser la misma que la región de recursos de LUIS.|`westus`|

4. En la **configuración de la plantilla del bot**, seleccione lo siguiente y luego elija el botón **Seleccionar** en esta configuración:

    |Configuración|Propósito|Número de selección|
    |--|--|--|
    |Versión del SDK|Versión de Bot Framework|**SDK v4**|
    |Lenguaje de SDK|Lenguaje de programación del bot|**Node.js**|
    |Bot básico o de eco|Tipo de bot|**Bot básico**|
    
5. Seleccione **Crear**. Esto crea e implementa el servicio de bots en Azure. Parte de este proceso crea una aplicación de LUIS denominada `luis-nodejs-bot-XXXX`. Este nombre se basa en el nombre del bot y de la aplicación de la sección anterior.

    [ ![Creación de una aplicación web](./media/bfv4-nodejs/create-web-app-service.png) ](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

6. Deje esta pestaña del explorador abierta. Para cualquier paso con el portal de LUIS, abra una nueva pestaña del explorador. Continúe con la sección siguiente cuando se implemente el nuevo servicio de bots.

## <a name="add-prebuilt-domain-to-model"></a>Incorporación de un dominio creado previamente al modelo
Parte de la implementación del servicio de bots crea una nueva aplicación de LUIS con intenciones y expresiones de ejemplo. El bot proporciona asignación de intenciones a la nueva aplicación de LUIS para las intenciones siguientes: 

|Intenciones de LUIS del bot básico|Expresión de ejemplo|
|--|--|
|Cancelar|`stop`|
|Greeting|`hello`|
|Ayuda|`help`|
|None|Nada fuera del dominio de la aplicación.|

Agregue al modelo la aplicación HomeAutomation creada previamente para controlar expresiones como: `Turn off the living room lights`

1. Vaya al portal de [LUIS](https://www.luis.ai) e inicie sesión.
2. En la página **Mis aplicaciones**, seleccione la columna **Fecha de creación** para ordenar por la fecha en la que se creó la aplicación. Azure Bot Service creó una nueva aplicación en la sección anterior. Su nombre es `luis-nodejs-bot-` + `<your-name>` + 4 caracteres aleatorios.
3. Abra la aplicación y seleccione la sección **Compilar** en el panel de navegación superior.
4. En el panel de navegación izquierdo, seleccione **Prebuilt Domains** (Dominios creados previamente).
5. Seleccione el dominio **HomeAutomation** en **Agregar dominio** de su tarjeta.
6. Seleccione **Train** (Entrenar) en el menú de la esquina superior derecha.
7. Seleccione **Publicar** (Publicar) en el menú de la esquina superior derecha. 

    La aplicación creada por Azure Bot Service ahora tiene intenciones nuevas:

    |Nuevas intenciones del bot básico|Expresión de ejemplo|
    |--|--|
    |HomeAutomation.TurnOn|`turn the fan to high`
    |HomeAutomation.TurnOff|`turn off ac please`|

## <a name="download-the-web-app-bot"></a>Descarga del bot de aplicación web 
Con el fin de desarrollar el código del bot de aplicación web, descargue el código y úselo en el equipo local. 

1. En Azure Portal, aún en el recurso del bot de aplicación web, seleccione **Configuración de la aplicación** y copie los valores de **botFilePath** y **botFileSecret**. Deberá agregarlos a un archivo de entorno más adelante. 

2. En Azure Portal, seleccione **Compilar** en la sección **Bot Management** (Administración del bot). 

3. Seleccione **Download Bot source code** (Descargar el código fuente del bot). 

    [ ![Descargar el código fuente del bot de aplicación web para el bot básico](../../../includes/media/cognitive-services-luis/bfv4/download-code.png) ](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

4. Cuando el código fuente se haya comprimido, un mensaje proporcionará un vínculo para descargar el código. Seleccione el vínculo. 

5. Guarde el archivo ZIP en el equipo local y extraiga los ficheros. Abra el proyecto. 

6. Abra el archivo bot.js y busque `const results = await this.luisRecognizer.recognize(context);`. Aquí es donde la expresión del usuario especificada en el bot se envía a LUIS.

   ```javascript
    /**
     * Driver code that does one of the following:
     * 1. Display a welcome card upon startup
     * 2. Use LUIS to recognize intents
     * 3. Start a greeting dialog
     * 4. Optionally handle Cancel or Help interruptions
     *
     * @param {Context} context turn context from the adapter
     */
    async onTurn(context) {
        // Create a dialog context
        const dc = await this.dialogs.createContext(context);

        if(context.activity.type === ActivityTypes.Message) {
            // Perform a call to LUIS to retrieve results for the current activity message.
            const results = await this.luisRecognizer.recognize(context);
            
            const topIntent = LuisRecognizer.topIntent(results);

            // handle conversation interrupts first
            const interrupted = await this.isTurnInterrupted(dc, results);
            if(interrupted) {
                return;
            }

            // Continue the current dialog
            const dialogResult = await dc.continue();

            switch(dialogResult.status) {
                case DialogTurnStatus.empty:
                    switch (topIntent) {
                        case GREETING_INTENT:
                            await dc.begin(GREETING_DIALOG);
                            break;

                        case NONE_INTENT:
                        default:
                            // help or no intent identified, either way, let's provide some help
                            // to the user
                            await dc.context.sendActivity(`I didn't understand what you just said to me. topIntent ${topIntent}`);
                            break;
                    }

                case DialogTurnStatus.waiting:
                    // The active dialog is waiting for a response from the user, so do nothing
                break;

                case DialogTurnStatus.complete:
                    await dc.end();
                    break;

                default:
                    await dc.cancelAll();
                    break;

            }

        } else if (context.activity.type === 'conversationUpdate' && context.activity.membersAdded[0].name === 'Bot') {
            // When activity type is "conversationUpdate" and the member joining the conversation is the bot
            // we will send our Welcome Adaptive Card.  This will only be sent once, when the Bot joins conversation
            // To learn more about Adaptive Cards, see https://aka.ms/msbot-adaptivecards for more details.
            const welcomeCard = CardFactory.adaptiveCard(WelcomeCard);
            await context.sendActivity({ attachments: [welcomeCard] });
        }
    }
    ```

    El bot envía la expresión del usuario a LUIS y obtiene los resultados. La intención principal determina el flujo de conversación. 


## <a name="start-the-bot"></a>Inicio del bot
Antes de cambiar cualquier código o configuración, compruebe los trabajos del bot. 

1. En Visual Studio Code, abra una ventana de terminal. 

2. Instale las dependencias de npm para este bot. 

    ```bash
    npm install
    ```
3. Cree un archivo para almacenar las variables de entorno que busca el código de bot. Asigne el nombre `.env` al archivo. Agregue estas variables de entorno:

    <!--there is no code language that represents an .env file correctly-->
    ```env
    botFilePath=
    botFileSecret=
    ```

    Establezca los valores de las variables de entorno en los valores que copió de la configuración de la aplicación de Azure Bot Service en el paso 1 de la sección **[Descarga del bot de aplicación web](#download-the-web-app-bot)**.

4. Inicie el bot en el modo de inspección. Los cambios que realice en el código después de este inicio harán que la aplicación se reinicie de manera automática.

    ```bash
    npm run watch
    ```

5. Cuando el bot se inicia, la ventana de terminal muestra el puerto local en que se ejecuta el bot:

    ```console
    > basic-bot@0.1.0 start C:\Users\pattiowens\repos\BFv4\luis-nodejs-bot-src
    > node ./index.js NODE_ENV=development

    restify listening to http://[::]:3978
    
    Get the Emulator: https://aka.ms/botframework-emulator
    
    To talk to your bot, open the luis-nodejs-bot-pattiowens.bot file in the Emulator
    ```

## <a name="start-the-emulator"></a>Inicio del emulador

1. Inicie Bot Emulator. 

2. En Bot Emulator, seleccione el archivo *.bot en la raíz del proyecto. Este archivo `.bot` incluye el punto de conexión de dirección URL del bot para mensajes:

    [ ![Bot Emulator v4](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png) ](../../../includes/media/cognitive-services-luis/bfv4/bot-emulator-v4.png#lightbox)

3. Introduzca el secreto de bot que copió de la configuración de la aplicación de Azure Bot Service en el paso 1 de la sección **[Descarga del bot de aplicación web](#download-the-web-app-bot)**. Esto permite que el emulador obtenga acceso a los campos cifrados del archivo .bot.

    ![Secreto de Bot Emulator v4](../../../includes/media/cognitive-services-luis/bfv4/bot-secret.png)


4. En Bot Emulator, escriba `Hello` y obtendrá la respuesta apropiada para el bot básico.

    [ ![Respuesta de bot básico en el emulador](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png) ](../../../includes/media/cognitive-services-luis/bfv4/emulator-test.png#lightbox)

## <a name="modify-bot-code"></a>Modificación del código del bot 

En el archivo `bot.js`, agregue código para controlar las intenciones nuevas. 

1. En la parte superior del archivo, busque la sección **Supported LUIS Intents** (Intenciones admitidas de LUIS) y agregue constantes para las intenciones de HomeAutomation:

   ```javascript
    // Supported LUIS Intents
    const GREETING_INTENT = 'Greeting';
    const CANCEL_INTENT = 'Cancel';
    const HELP_INTENT = 'Help';
    const NONE_INTENT = 'None';
    const TURNON_INTENT = 'HomeAutomation_TurnOn'; // new intent
    const TURNOFF_INTENT = 'HomeAutomation_TurnOff'; // new intent
    ```

    Tenga en cuenta que el punto, `.`, entre el dominio y la intención de la aplicación del portal de LUIS se reemplaza por un guion bajo, `_`. 

2. Busque **isTurnInterrupted** que recibe la predicción de LUIS de la expresión y agregue una línea para imprimir el resultado en la consola.

   ```javascript
    /**
     * Look at the LUIS results and determine if we need to handle
     * an interruptions due to a Help or Cancel intent
     *
     * @param {DialogContext} dc - dialog context
     * @param {LuisResults} luisResults - LUIS recognizer results
     */
    async isTurnInterrupted(dc, luisResults) {
        console.log(JSON.stringify(luisResults));
    ...
    ```

    El bot no tiene la misma respuesta exacta como una solicitud de API de REST de LUIS, por lo que es importante conocer las diferencias examinando el JSON de respuesta. Las propiedades de texto y de las intenciones son las mismas, pero se han modificado los valores de propiedad de las entidades. 

    ```json
    {
        "$instance": {
            "HomeAutomation_Device": [
                {
                    "startIndex": 23,
                    "endIndex": 29,
                    "score": 0.9776345,
                    "text": "lights",
                    "type": "HomeAutomation.Device"
                }
            ],
            "HomeAutomation_Room": [
                {
                    "startIndex": 12,
                    "endIndex": 22,
                    "score": 0.9079433,
                    "text": "livingroom",
                    "type": "HomeAutomation.Room"
                }
            ]
        },
        "HomeAutomation_Device": [
            "lights"
        ],
        "HomeAutomation_Room": [
            "livingroom"
        ]
    }
    ```

3. Agregue las intenciones a la instrucción switch del método onTurn para el caso `DialogTurnStatus.empty`:

   ```javascript
    switch (topIntent) {
        case GREETING_INTENT:
            await dc.begin(GREETING_DIALOG);
            break;

        // New HomeAutomation.TurnOn intent
        case TURNON_INTENT: 

            await dc.context.sendActivity(`TurnOn intent found, entities included: ${JSON.stringify(results.entities)}`);
            break;

        // New HomeAutomation.TurnOff intent
        case TURNOFF_INTENT: 

            await dc.context.sendActivity(`TurnOff intent found, entities included: ${JSON.stringify(results.entities)}`);
            break;

        case NONE_INTENT:
        default:
            // help or no intent identified, either way, let's provide some help
            // to the user
            await dc.context.sendActivity(`I didn't understand what you just said to me. topIntent ${topIntent}`);
            break;
    }
    ```

## <a name="view-results-in-bot"></a>Visualización de los resultados en el bot

1. En Bot Emulator, escriba la expresión: `Turn on the livingroom lights to 50%`

2. El bot responde con:

    ```json
    TurnOn intent found, entities included: {"$instance":{“HomeAutomation_Device”:[{“startIndex”:23,“endIndex”:29,“score”:0.9776345,“text”:“lights”,“type”:“HomeAutomation.Device”}],“HomeAutomation_Room”:[{“startIndex”:12,“endIndex”:22,“score”:0.9079433,“text”:“livingroom”,“type”:“HomeAutomation.Room”}]},“HomeAutomation_Device”:[“lights”],“HomeAutomation_Room”:[“livingroom”]}
    ```

## <a name="learn-more-about-bot-framework"></a>Más información sobre Bot Framework
Azure Bot Service usa el SDK de Bot Framework. Más información sobre el SDK y bot framework:

* Documentación sobre [Azure Bot Service](https://docs.microsoft.com/azure/bot-service/bot-service-overview-introduction?view=azure-bot-service-4.0) v4
* [Ejemplos de Bot Builder](https://github.com/Microsoft/botbuilder-samples)
* [SDK de Bot Builder](https://docs.microsoft.com/javascript/api/botbuilder-core/?view=botbuilder-ts-latest)
* [Herramientas de Bot Builder](https://github.com/Microsoft/botbuilder-tools):

## <a name="next-steps"></a>Pasos siguientes

Creó una instancia de Azure Bot Service, copió el secreto del bot y la ruta de acceso al archivo .bot y descargó el archivo ZIP del código. Agregó el dominio HomeAutomation creado previamente a la aplicación LUIS que se creó como parte de la nueva instancia de Azure Bot Service y luego entrenó y publicó nuevamente la aplicación. Extrajo el código del proyecto, creó un archivo de entorno (`.env`) y estableció el secreto del bot y la ruta de acceso al archivo .bot. En el archivo bot.js, agregó código para controlar las dos intenciones nuevas. Luego probó el bot en Bot Emulator para ver la respuesta de LUIS para una expresión de una de las intenciones nuevas. 


> [!div class="nextstepaction"]
> [Compilación de un dominio personalizado en LUIS](luis-quickstart-intents-only.md)
