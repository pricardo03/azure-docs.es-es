---
title: Bot de LUIS con Node.js - Bot de aplicación web - Bot Framework SDK 3.0
titleSuffix: Azure Cognitive Services
description: Cree un bot integrado con una aplicación de LUIS mediante Bot Framework.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 4967c6c8eb9f849006beb78cfd2e41eba53b6867
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46952977"
---
# <a name="luis-bot-in-nodejs"></a>Bot de LUIS en Node.js

Use Node.js para crear un bot de chat que se integre con Language Understanding (LUIS). Este bot de chat utiliza el dominio HomeAutomation creado previamente para implementar con rapidez una solución de bot. El bot se compila con Bot Framework 3.x y el bot de aplicación web de Azure.

## <a name="prerequisite"></a>Requisito previo

Antes de crear el bot, siga los pasos de [Create an app](./luis-get-started-create-app.md) (Crear una aplicación) para compilar la aplicación de LUIS que usa.

El bot responde a intenciones del dominio Home Automation que se encuentran en la aplicación de LUIS. Para cada una de estas intenciones, la aplicación de LUIS proporciona una intención que se le asigna. El bot proporciona un cuadro de diálogo que controla la intención que detecta LUIS.

| Intención | Expresión de ejemplo | Funcionalidad del bot |
|:----:|:----------:|---|
| HomeAutomation.TurnOn | Encender las luces. | El bot invoca a `TurnOnDialog` cuando se detecta `HomeAutomation.TurnOn`. Este cuadro de diálogo es donde invocaría a un servicio de IoT para activar un dispositivo e indicar al usuario que se ha activado. |
| HomeAutomation.TurnOff | Apagar las luces de la habitación. | El bot invoca a `TurnOffDialog` cuando se detecta `HomeAutomation.TurnOff`. Este cuadro de diálogo es donde invocaría a un servicio de IoT para desactivar un dispositivo e indicar al usuario que se ha desactivado. |


## <a name="create-a-language-understanding-bot-with-bot-service"></a>Crear un bot de Language Understanding con Bot Service

1. En [Azure Portal](https://portal.azure.com), haga clic en **Crear nuevo recurso** en la hoja de menú y, después, en **Ver todo**.

    ![Crear recurso](./media/luis-tutorial-node-bot/bot-service-creation.png)

2. En el cuadro de búsqueda, busque **Bot de aplicación web**. 

    ![Crear recurso](./media/luis-tutorial-node-bot/bot-service-selection.png)

3. En la hoja **Servicio de bots**, proporcione la información necesaria y haga clic en **Crear**. Esto crea e implementa el servicio de bots y la aplicación de LUIS en Azure. Si quiere usar la [preparación para la voz](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming), consulte [los requisitos de región](luis-resources-faq.md#what-luis-regions-support-bot-framework-speech-priming) antes de crear el bot. 
    * Establezca **Nombre de la aplicación** en el nombre del bot. El nombre se usa como el subdominio cuando el bot se implementa en la nube (por ejemplo, mynotesbot.azurewebsites.net). <!-- This name is also used as the name of the LUIS app associated with your bot. Copy it to use later, to find the LUIS app associated with the bot. -->
    * Seleccione la suscripción, el [grupo de recursos](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview), el plan de App Service y la [ubicación](https://azure.microsoft.com/regions/).
    * En **Bot template** (Plantilla de bot), seleccione:
        * **SDK v3**
        * **Node.js**
        * **Comprensión del lenguaje**
    * Seleccione el valor de **LUIS App Location** (Ubicación de la aplicación de LUIS). Esta es la [región][LUIS] de creación en la que se crea la aplicación.
    * Active la casilla de confirmación del aviso legal. Los términos del aviso legal están debajo de la casilla.

    ![Hoja Servicio de bots](./media/luis-tutorial-node-bot/bot-service-setting-callout-template.png)


4. Confirme que se ha implementado el servicio de bots.
    * Haga clic en Notificaciones (el icono de la campana que se encuentra en el borde superior de Azure Portal). La notificación cambiará de **Implementación iniciada** a **Implementación correcta**.
    * Después de que la notificación cambie a **Implementación correcta**, haga clic en **Ir al recurso** en esa notificación.

## <a name="try-the-default-bot"></a>Probar el bot predeterminado

Para confirmar que el bot se ha implementado, active las **Notificaciones**. Las notificaciones cambiarán de **Implementación en curso...** a **Implementación correcta**. Haga clic en el botón **Ir al recurso** para abrir la hoja de recursos del bot.

<!-- this step isn't supposed to be necessary -->
## <a name="install-npm-resources"></a>Instalar recursos de NPM
Instale paquetes de NPM con los pasos siguientes:

1. Haga clic en **Compilar** en la sección **Bot Management** (Administración del bot) del bot de aplicación web. 

2. Se abre una segunda ventana del explorador. Haga clic en **Open online code editor** (Abrir el editor de código en línea).

3. En la barra de navegación superior, seleccione el nombre del bot de la aplicación web `homeautomationluisbot`. 

4. En la lista desplegable, **abra la consola de Kudu**.

5. Se abre una nueva ventana del explorador. En la consola, escriba el siguiente comando:

    ```
    cd site\wwwroot && npm install
    ```

    Espere a que finalice el proceso de instalación. Vuelva a la primera ventana del explorador. 

## <a name="test-in-web-chat"></a>Probar en Chat en web
Una vez registrado el bot, haga clic en **Test in Web Chat** (Probar en Chat en web) para abrir el panel Chat en web. Escriba "hello" ("hola") en el Chat en web.

  ![Probar el bot en Chat en web](./media/luis-tutorial-node-bot/bot-service-web-chat.png)

El bot responde "You have reached Greeting. You said: hello" ("Se ha puesto en contacto con Saludo. Ha dicho: hola"). Esto confirma que el bot ha recibido el mensaje y lo ha pasado a una aplicación de LUIS predeterminada que ha creado. Esta aplicación de LUIS predeterminada ha detectado una intención Saludo. En el paso siguiente, conectará el bot con la aplicación de LUIS que ha creado anteriormente en lugar de la aplicación de LUIS predeterminada.

## <a name="connect-your-luis-app-to-the-bot"></a>Conectar la aplicación de LUIS con el bot

Abra **Configuración de la aplicación** en la primera ventana del explorador y edite el campo **LuisAppId** para que contenga el identificador de la aplicación de LUIS.

  ![Actualizar el identificador de la aplicación de LUIS en Azure](./media/luis-tutorial-node-bot/bot-service-app-id.png)

Si no tiene el identificador de la aplicación de LUIS, inicie sesión en el sitio web de [LUIS](luis-reference-regions.md) con la misma cuenta que usa para iniciar sesión en Azure. Haga clic en **Mis aplicaciones**. 

1. Busque la aplicación de LUIS que ha creado anteriormente, que contiene las intenciones y entidades del dominio HomeAutomation.

2. En la página **Configuración** de la aplicación de LUIS, busque y copie el identificador de la aplicación.

3. Si aún no ha entrenado la aplicación, haga clic en el botón **Entrenar** situado en la esquina superior derecha para entrenarla.

4. Si aún no ha publicado la aplicación, haga clic en **Publicar** en la barra de navegación superior para abrir la página **Publicar**. Seleccione el espacio de producción y haga clic en el botón **Publicar**.

## <a name="modify-the-bot-code"></a>Modificar el código del bot

Vaya a la segunda ventana del explorador si sigue abierta o, en la primera ventana del explorador, haga clic en **Compilar** y después en **Open online code editor** (Abrir el editor de código en línea).

   ![Abrir el editor de código en línea](./media/luis-tutorial-node-bot/bot-service-build.png)

En el editor de código, abra `app.js`. Contiene el siguiente código:

```javascript
/*-----------------------------------------------------------------------------
A simple Language Understanding (LUIS) bot for the Microsoft Bot Framework. 
-----------------------------------------------------------------------------*/

var restify = require('restify');
var builder = require('botbuilder');
var botbuilder_azure = require("botbuilder-azure");

// Setup Restify Server
var server = restify.createServer();
server.listen(process.env.port || process.env.PORT || 3978, function () {
   console.log('%s listening to %s', server.name, server.url); 
});
  
// Create chat connector for communicating with the Bot Framework Service
var connector = new builder.ChatConnector({
    appId: process.env.MicrosoftAppId,
    appPassword: process.env.MicrosoftAppPassword,
    openIdMetadata: process.env.BotOpenIdMetadata 
});

// Listen for messages from users 
server.post('/api/messages', connector.listen());

/*----------------------------------------------------------------------------------------
* Bot Storage: This is a great spot to register the private state storage for your bot. 
* We provide adapters for Azure Table, CosmosDb, SQL Azure, or you can implement your own!
* For samples and documentation, see: https://github.com/Microsoft/BotBuilder-Azure
* ---------------------------------------------------------------------------------------- */

var tableName = 'botdata';
var azureTableClient = new botbuilder_azure.AzureTableClient(tableName, process.env['AzureWebJobsStorage']);
var tableStorage = new botbuilder_azure.AzureBotStorage({ gzipData: false }, azureTableClient);

// Create your bot with a function to receive messages from the user
// This default message handler is invoked if the user's utterance doesn't
// match any intents handled by other dialogs.
var bot = new builder.UniversalBot(connector, function (session, args) {
    session.send('You reached the default message handler. You said \'%s\'.', session.message.text);
});

bot.set('storage', tableStorage);

// Make sure you add code to validate these fields
var luisAppId = process.env.LuisAppId;
var luisAPIKey = process.env.LuisAPIKey;
var luisAPIHostName = process.env.LuisAPIHostName || 'westus.api.cognitive.microsoft.com';

const LuisModelUrl = 'https://' + luisAPIHostName + '/luis/v2.0/apps/' + luisAppId + '?subscription-key=' + luisAPIKey;

// Create a recognizer that gets intents from LUIS, and add it to the bot
var recognizer = new builder.LuisRecognizer(LuisModelUrl);
bot.recognizer(recognizer);

// Add a dialog for each intent that the LUIS app recognizes.
// See https://docs.microsoft.com/bot-framework/nodejs/bot-builder-nodejs-recognize-intent-luis 
bot.dialog('GreetingDialog',
    (session) => {
        session.send('You reached the Greeting intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Greeting'
})

bot.dialog('HelpDialog',
    (session) => {
        session.send('You reached the Help intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Help'
})

bot.dialog('CancelDialog',
    (session) => {
        session.send('You reached the Cancel intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'Cancel'
})
```

Se omiten las intenciones existentes en app.js. Puede dejarlas. 

## <a name="add-a-dialog-that-matches-homeautomationturnon"></a>Agregar un cuadro de diálogo que coincida con HomeAutomation.TurnOn

Copie el código siguiente y agréguelo en `app.js`.

```javascript
bot.dialog('TurnOn',
    (session) => {
        session.send('You reached the TurnOn intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'HomeAutomation.TurnOn'
})
```

La opción [matches][matches] de [triggerAction][triggerAction] adjunta al cuadro de diálogo especifica el nombre de la intención. El reconocedor se ejecuta cada vez que el bot recibe una expresión del usuario. Si la intención de mayor puntuación que detecta coincide con una `triggerAction` enlazada a un cuadro de diálogo, el bot invoca ese cuadro de diálogo.

## <a name="add-a-dialog-that-matches-homeautomationturnoff"></a>Agregar un cuadro de diálogo que coincida con HomeAutomation.TurnOff

Copie el código siguiente y agréguelo en `app.js`.

```javascript
bot.dialog('TurnOff',
    (session) => {
        session.send('You reached the TurnOff intent. You said \'%s\'.', session.message.text);
        session.endDialog();
    }
).triggerAction({
    matches: 'HomeAutomation.TurnOff'
})
```
## <a name="test-the-bot"></a>Probar el bot

En Azure Portal, haga clic en **Test in Web Chat** (Probar en Chat en web) para probar el bot. Pruebe a escribir mensajes como "Turn on the lights" ("Encender las luces") o "Turn off my heater" ("Apagar la calefacción") para invocar las intenciones que le haya agregado.
   ![Probar el bot HomeAutomation en Chat en web](./media/luis-tutorial-node-bot/bot-service-chat-results.png)

> [!TIP]
> Si ve que el bot no reconoce siempre la intención o las entidades correctas, proporcione a la aplicación de LUIS más ejemplos de expresiones para entrenarla y mejorar su rendimiento. Puede volver a entrenar la aplicación de LUIS sin modificar el código del bot. Vea [Add example utterances](https://docs.microsoft.com/azure/cognitive-services/LUIS/add-example-utterances) (Agregar expresiones de ejemplo) y [Train and test your LUIS app](https://docs.microsoft.com/azure/cognitive-services/LUIS/luis-interactive-test) (Entrenar y probar la aplicación de LUIS).

## <a name="learn-more-about-bot-framework"></a>Más información sobre Bot Framework
Obtenga más información sobre [Bot Framework](https://dev.botframework.com/) y las versiones [3.x](https://github.com/Microsoft/BotBuilder) y [4.x](https://github.com/Microsoft/botbuilder-js) de los SDK.

## <a name="next-steps"></a>Pasos siguientes

<!-- From trying the bot, you can see that the recognizer can trigger interruption of the currently active dialog. Allowing and handling interruptions is a flexible design that accounts for what users really do. Learn more about the various actions you can associate with a recognized intent.--> Puede intentar agregar otras intenciones, como Ayuda, Cancelar y Saludo, a la aplicación de LUIS. Después, agregue cuadros de diálogo para las intenciones y pruébelas con el bot. 

<!-- 
> [!NOTE] 
> The default LUIS app that the template created contains example utterances for Cancel, Greeting, and Help intents. In the list of apps, find the app that begins with the name specified in **App name** in the **Bot Service** blade when you created the Bot Service. -->

> [!div class="nextstepaction"]
> [Add intents (Agregar intenciones)](./luis-how-to-add-intents.md)
> [Speech priming (Preparación para la voz)](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming)


[intentDialog]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html

[intentDialog_matches]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.intentdialog.html#matches 

[NotesSample]: https://github.com/Microsoft/BotFramework-Samples/tree/master/docs-samples/Node/basics-naturalLanguage

[triggerAction]: https://docs.botframework.com/en-us/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html#triggeraction

[confirmPrompt]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#confirmprompt

[waterfall]: bot-builder-nodejs-dialog-manage-conversation-flow.md#manage-conversation-flow-with-a-waterfall

[session_userData]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.session.html#userdata

[EntityRecognizer_findEntity]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.entityrecognizer.html#findentity

[matches]: https://docs.botframework.com/en-us/node/builder/chat-reference/interfaces/_botbuilder_d_.itriggeractionoptions.html#matches

[LUISAzureDocs]: https://docs.microsoft.com/azure/cognitive-services/LUIS/Home

[Dialog]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.dialog.html

[IntentRecognizerSetOptions]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.iintentrecognizersetoptions.html

[LuisRecognizer]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.luisrecognizer

[LUISConcepts]: https://docs.botframework.com/node/builder/guides/understanding-natural-language/

[DisambiguationSample]: https://github.com/Microsoft/BotBuilder/tree/master/Node/examples/feature-onDisambiguateRoute

[IDisambiguateRouteHandler]: https://docs.botframework.com/node/builder/chat-reference/interfaces/_botbuilder_d_.idisambiguateroutehandler.html

[RegExpRecognizer]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.regexprecognizer.html

[AlarmBot]: https://github.com/Microsoft/BotBuilder/blob/master/Node/examples/basics-naturalLanguage/app.js

[LUISBotSample]: https://github.com/Microsoft/BotBuilder-Samples/tree/master/Node/intelligence-LUIS

[UniversalBot]: https://docs.botframework.com/node/builder/chat-reference/classes/_botbuilder_d_.universalbot.html


<!-- Old Links -->
[Github-BotFramework-Emulator-Download]: https://aka.ms/bot-framework-emulator
[Github-LUIS-Samples]: https://github.com/Microsoft/LUIS-Samples
[Github-LUIS-Samples-node-hotel-bot]: https://github.com/Microsoft/LUIS-Samples/tree/master/bot-integration-samples/hotel-finder/nodejs
[NodeJs]: https://nodejs.org/
[BFPortal]: https://dev.botframework.com/
[RegisterInstructions]: https://docs.microsoft.com/bot-framework/portal-register-bot
[BotFramework]: https://docs.microsoft.com/bot-framework/
[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions

