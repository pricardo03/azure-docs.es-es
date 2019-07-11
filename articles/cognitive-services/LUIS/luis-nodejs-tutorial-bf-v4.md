---
title: Bot de Language Understanding Node.js v4
titleSuffix: Azure Cognitive Services
description: Use Node.js para crear un bot de chat que se integre con Language Understanding (LUIS). Este bot de chat utiliza la aplicación de recursos humanos para implementar con rapidez una solución de bot. El bot se compila con la versión 4 de Bot Framework y el bot de aplicación web de Azure.
services: cognitive-services
author: diberry
ms.custom: seodec18
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/24/2019
ms.author: diberry
ms.openlocfilehash: a06bd5a1a061de82230e93b867ea88e333b3cc93
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67442548"
---
# <a name="tutorial-use-a-web-app-bot-enabled-with-language-understanding-in-nodejs"></a>Tutorial: Uso de un bot de aplicación web habilitado con Language Understanding en Node.js 

Use Node.js para crear un bot de chat que se integre con Language Understanding (LUIS). El bot se ha compilado con el recurso [bot de aplicación web](https://docs.microsoft.com/azure/bot-service/) de Azure y [Bot Framework versión](https://github.com/Microsoft/botbuilder-dotnet) V4.

**En este tutorial, aprenderá a:**

> [!div class="checklist"]
> * Crear un bot de aplicación web. Este proceso crea una nueva aplicación de LUIS.
> * Descargar el proyecto de bot creado por el servicio de bot web
> * Iniciar el emulador y el bot localmente en el equipo.
> * Ver los resultados de expresiones del bot.

## <a name="prerequisites"></a>Requisitos previos

* [Bot Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)


## <a name="create-a-web-app-bot-resource"></a>Crear un nuevo recurso de bot de aplicación web

1. En [Azure Portal](https://portal.azure.com), haga clic en **Crear un recurso**.

1. En el cuadro de búsqueda, busque y seleccione **Bot de aplicación web**. Seleccione **Crear**.

1. En **Servicio de bots**, proporcione la información necesaria:

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
    |Plan de App Service/ubicación|No cambie el valor predeterminado proporcionado.|
    |Application Insights|No cambie el valor predeterminado proporcionado.|
    |Id. y contraseña de aplicación de Microsoft|No cambie el valor predeterminado proporcionado.|

1. En **Bot template** (Plantilla del bot), seleccione lo siguiente y luego elija el botón **Seleccionar** en esta configuración:

    |Configuración|Propósito|Número de selección|
    |--|--|--|
    |Versión del SDK|Versión de Bot Framework|**SDK v4**|
    |Lenguaje de SDK|Lenguaje de programación del bot|**Node.js**|
    |Bot|Tipo de bot|**Bot básico**|
    
1. Seleccione **Crear**. Esto crea e implementa el servicio de bots en Azure. Parte de este proceso crea una aplicación de LUIS denominada `luis-nodejs-bot-XXXX`. Este nombre se basa en el nombre de la aplicación /Azure Bot Service.

    [![Creación de un bot de aplicación web](./media/bfv4-nodejs/create-web-app-service.png)](./media/bfv4-nodejs/create-web-app-service.png#lightbox)

    Antes de continuar, espere hasta que se crea el servicio bot.

## <a name="the-bot-has-a-language-understanding-model"></a>El bot tiene un modelo de Language Understanding.

El proceso de creación del servicio de bot también crea una nueva aplicación LUIS con intenciones y expresiones de ejemplo. El bot proporciona asignación de intenciones a la nueva aplicación de LUIS para las intenciones siguientes: 

|Intenciones de LUIS del bot básico|Expresión de ejemplo|
|--|--|
|Reservar vuelo|`Travel to Paris`|
|Cancelar|`bye`|
|None|Nada fuera del dominio de la aplicación.|

## <a name="test-the-bot-in-web-chat"></a>Probar el bot en Chat en web

1. Mientras sigue en Azure Portal para el nuevo bot, seleccione **Test in Web Chat** (Probar en chat web). 
1. En el cuadro de texto **Type your message** (Escriba su mensaje), escriba el texto `hello`. El bot responde con información sobre el marco del bot, así como consultas de ejemplo para el modelo LUIS específico, como la reserva de un vuelo a París. 

    ![Captura de pantalla de Azure Portal, escribir el texto "hello".](./media/bfv4-nodejs/ask-bot-question-in-portal-test-in-web-chat.png)

    Puede usar la funcionalidad de prueba para probar rápidamente su bot. Para pruebas más completas, como la depuración, descargue el código de bot y use Visual Studio. 

## <a name="download-the-web-app-bot-source-code"></a>Descarga del código fuente del bot de aplicación web
Con el fin de desarrollar el código del bot de aplicación web, descargue el código y úselo en el equipo local. 

1. En Azure Portal, seleccione **Compilar** en la sección **Bot Management** (Administración del bot). 

1. Seleccione **Download Bot source code** (Descargar el código fuente del bot). 

    [![Descargar el código fuente del bot de aplicación web para el bot básico](../../../includes/media/cognitive-services-luis/bfv4/download-code.png)](../../../includes/media/cognitive-services-luis/bfv4/download-code.png#lightbox)

1. Cuando el cuadro de diálogo emergente le pregunte **Include app settings in the downloaded zip file?** (¿Incluir la configuración de la aplicación en el archivo zip descargado?), seleccione **Sí**.

1. Cuando el código fuente se haya comprimido, un mensaje proporcionará un vínculo para descargar el código. Seleccione el vínculo. 

1. Guarde el archivo ZIP en el equipo local y extraiga los ficheros. Abra el proyecto con Visual Studio. 

## <a name="review-code-to-send-utterance-to-luis-and-get-response"></a>Revisión del código para enviar la expresión a LUIS y obtener respuesta

1. Abra el archivo **Cuadros de diálogo -> luisHelper.js**. Aquí es donde la expresión del usuario especificada en el bot se envía a LUIS. La respuesta de LUIS se devuelve desde el método como un objeto JSON **bookDetails**. Al crear su bot propio, también debe crear su propio objeto para devolver los detalles de LUIS. 

    ```nodejs
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { LuisRecognizer } = require('botbuilder-ai');
    
    class LuisHelper {
        /**
         * Returns an object with preformatted LUIS results for the bot's dialogs to consume.
         * @param {*} logger
         * @param {TurnContext} context
         */
        static async executeLuisQuery(logger, context) {
            const bookingDetails = {};
    
            try {
                const recognizer = new LuisRecognizer({
                    applicationId: process.env.LuisAppId,
                    endpointKey: process.env.LuisAPIKey,
                    endpoint: `https://${ process.env.LuisAPIHostName }`
                }, {}, true);
    
                const recognizerResult = await recognizer.recognize(context);
    
                const intent = LuisRecognizer.topIntent(recognizerResult);
    
                bookingDetails.intent = intent;
    
                if (intent === 'Book_flight') {
                    // We need to get the result from the LUIS JSON which at every level returns an array
    
                    bookingDetails.destination = LuisHelper.parseCompositeEntity(recognizerResult, 'To', 'Airport');
                    bookingDetails.origin = LuisHelper.parseCompositeEntity(recognizerResult, 'From', 'Airport');
    
                    // This value will be a TIMEX. And we are only interested in a Date so grab the first result and drop the Time part.
                    // TIMEX is a format that represents DateTime expressions that include some ambiguity. e.g. missing a Year.
                    bookingDetails.travelDate = LuisHelper.parseDatetimeEntity(recognizerResult);
                }
            } catch (err) {
                logger.warn(`LUIS Exception: ${ err } Check your LUIS configuration`);
            }
            return bookingDetails;
        }
    
        static parseCompositeEntity(result, compositeName, entityName) {
            const compositeEntity = result.entities[compositeName];
            if (!compositeEntity || !compositeEntity[0]) return undefined;
    
            const entity = compositeEntity[0][entityName];
            if (!entity || !entity[0]) return undefined;
    
            const entityValue = entity[0][0];
            return entityValue;
        }
    
        static parseDatetimeEntity(result) {
            const datetimeEntity = result.entities['datetime'];
            if (!datetimeEntity || !datetimeEntity[0]) return undefined;
    
            const timex = datetimeEntity[0]['timex'];
            if (!timex || !timex[0]) return undefined;
    
            const datetime = timex[0].split('T')[0];
            return datetime;
        }
    }
    
    module.exports.LuisHelper = LuisHelper;
    ```

1. Abra **Cuadros de diálogo -> bookingDialog.js** para saber cómo se usa el objeto BookingDetails para administrar el flujo de conversación. Los detalles de viaje se solicitan en pasos, luego la reserva completa se confirma y, por último, esta se repite al usuario. 

    ```nodejs
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { TimexProperty } = require('@microsoft/recognizers-text-data-types-timex-expression');
    const { ConfirmPrompt, TextPrompt, WaterfallDialog } = require('botbuilder-dialogs');
    const { CancelAndHelpDialog } = require('./cancelAndHelpDialog');
    const { DateResolverDialog } = require('./dateResolverDialog');
    
    const CONFIRM_PROMPT = 'confirmPrompt';
    const DATE_RESOLVER_DIALOG = 'dateResolverDialog';
    const TEXT_PROMPT = 'textPrompt';
    const WATERFALL_DIALOG = 'waterfallDialog';
    
    class BookingDialog extends CancelAndHelpDialog {
        constructor(id) {
            super(id || 'bookingDialog');
    
            this.addDialog(new TextPrompt(TEXT_PROMPT))
                .addDialog(new ConfirmPrompt(CONFIRM_PROMPT))
                .addDialog(new DateResolverDialog(DATE_RESOLVER_DIALOG))
                .addDialog(new WaterfallDialog(WATERFALL_DIALOG, [
                    this.destinationStep.bind(this),
                    this.originStep.bind(this),
                    this.travelDateStep.bind(this),
                    this.confirmStep.bind(this),
                    this.finalStep.bind(this)
                ]));
    
            this.initialDialogId = WATERFALL_DIALOG;
        }
    
        /**
         * If a destination city has not been provided, prompt for one.
         */
        async destinationStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            if (!bookingDetails.destination) {
                return await stepContext.prompt(TEXT_PROMPT, { prompt: 'To what city would you like to travel?' });
            } else {
                return await stepContext.next(bookingDetails.destination);
            }
        }
    
        /**
         * If an origin city has not been provided, prompt for one.
         */
        async originStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the response to the previous step's prompt
            bookingDetails.destination = stepContext.result;
            if (!bookingDetails.origin) {
                return await stepContext.prompt(TEXT_PROMPT, { prompt: 'From what city will you be travelling?' });
            } else {
                return await stepContext.next(bookingDetails.origin);
            }
        }
    
        /**
         * If a travel date has not been provided, prompt for one.
         * This will use the DATE_RESOLVER_DIALOG.
         */
        async travelDateStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the results of the previous step
            bookingDetails.origin = stepContext.result;
            if (!bookingDetails.travelDate || this.isAmbiguous(bookingDetails.travelDate)) {
                return await stepContext.beginDialog(DATE_RESOLVER_DIALOG, { date: bookingDetails.travelDate });
            } else {
                return await stepContext.next(bookingDetails.travelDate);
            }
        }
    
        /**
         * Confirm the information the user has provided.
         */
        async confirmStep(stepContext) {
            const bookingDetails = stepContext.options;
    
            // Capture the results of the previous step
            bookingDetails.travelDate = stepContext.result;
            const msg = `Please confirm, I have you traveling to: ${ bookingDetails.destination } from: ${ bookingDetails.origin } on: ${ bookingDetails.travelDate }.`;
    
            // Offer a YES/NO prompt.
            return await stepContext.prompt(CONFIRM_PROMPT, { prompt: msg });
        }
    
        /**
         * Complete the interaction and end the dialog.
         */
        async finalStep(stepContext) {
            if (stepContext.result === true) {
                const bookingDetails = stepContext.options;
    
                return await stepContext.endDialog(bookingDetails);
            } else {
                return await stepContext.endDialog();
            }
        }
    
        isAmbiguous(timex) {
            const timexPropery = new TimexProperty(timex);
            return !timexPropery.types.has('definite');
        }
    }
    
    module.exports.BookingDialog = BookingDialog;
    ```


## <a name="install-dependencies-and-start-the-bot-code-in-visual-studio"></a>Instalación de las dependencias e inicio del código del bot en Visual Studio

1. En VSCode, desde el terminal integrado, instale las dependencias con el comando `npm install`.
1. También desde el terminal integrado, inicie el bot con el comando `npm start`. 


## <a name="use-the-bot-emulator-to-test-the-bot"></a>Uso del emulador de bot para probar el bot

1. Inicie el emulador de bots y seleccione **Open Bot** (Abrir bot).
1. En el cuadro de diálogo emergente **Open a bot** (Abrir un bot), escriba la dirección URL del bot, por ejemplo, `http://localhost:3978/api/messages`. La ruta `/api/messages` es la dirección web para el bot.
1. Escriba valores en **Microsoft App ID** (Id. de aplicación de Microsoft) y **Microsoft App password** (Contraseña de aplicación de Microsoft), que se encuentran en el archivo **.env** en la raíz del código de bot que descargó.

    Si lo desea, puede crear una nueva configuración de bot y copiar los valores `MicrosoftAppId` y `MicrosoftAppPassword` desde el archivo **.env** en el proyecto de Visual Studio para el bot. El nombre del archivo de configuración de bot debe ser el mismo que el nombre del bot. 

    ```json
    {
        "name": "<bot name>",
        "description": "<bot description>",
        "services": [
            {
                "type": "endpoint",
                "appId": "<appId from .env>",
                "appPassword": "<appPassword from .env>",
                "endpoint": "http://localhost:3978/api/messages",
                "id": "<don't change this value>",
                "name": "http://localhost:3978/api/messages"
            }
        ],
        "padlock": "",
        "version": "2.0",
        "overrides": null,
        "path": "<local path to .bot file>"
    }
    ```

1. En el emulador de bot, escriba `Hello` y obtenga la misma respuesta para el bot básico que la que recibió en el **Test in Web Chat** (Probar en chat web).

    [![Respuesta de bot básico en el emulador](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png)](./media/bfv4-nodejs/ask-bot-emulator-a-question-and-get-response.png#lightbox)


## <a name="ask-bot-a-question-for-the-book-flight-intent"></a>Formule una pregunta al bot para la intención de reservar vuelo

1. En el emulador de bot, reserve un vuelo. Para ello, especifique la expresión siguiente: 

    ```bot
    Book a flight from Paris to Berlin on March 22, 2020
    ```

    El emulador de bot solicita confirmación. 

1. Seleccione **Sí**. El bot responde con un resumen de sus acciones. 
1. En el registro del emulador bot, seleccione la línea que incluye `Luis Trace`. Esto muestra la respuesta JSON de LUIS para la intención y las entidades de la expresión.

    [![Respuesta de bot básico en el emulador](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png)](./media/bfv4-nodejs/ask-luis-book-flight-question-get-json-response-in-bot-emulator.png#lightbox)


[!INCLUDE [Bot Information](../../../includes/cognitive-services-qnamaker-luis-bot-info.md)]

## <a name="next-steps"></a>Pasos siguientes

Consulte más [ejemplos](https://github.com/microsoft/botframework-solutions) con bots de conversación. 

> [!div class="nextstepaction"]
> [Build a Language Understanding app with a custom subject domain](luis-quickstart-intents-only.md) (Compilación de una aplicación Language Understanding con un dominio de sujeto personalizado)