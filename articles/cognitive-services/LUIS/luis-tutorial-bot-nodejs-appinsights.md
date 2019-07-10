---
title: Application Insights, Node.js
titleSuffix: Azure Cognitive Services
description: En este tutorial se agrega información de bot y Language Understanding al almacenamiento de datos de telemetría de Application Insights.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: tutorial
ms.date: 06/16/2019
ms.author: diberry
ms.openlocfilehash: 5459fb5d8304a35b3f009354c446514a2831c513
ms.sourcegitcommit: 1289f956f897786090166982a8b66f708c9deea1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67155288"
---
# <a name="add-luis-results-to-application-insights-from-a-bot-in-nodejs"></a>Adición de resultados de LUIS a Application Insights desde un bot en Node.js
En este tutorial se agrega información de bot y Language Understanding al almacenamiento de datos de telemetría de [Application Insights](https://azure.microsoft.com/services/application-insights/). Una vez que tengas los datos, puedes consultarlos con el lenguaje Kusto o Power BI para analizar y agregar intenciones y entidades de la expresión en tiempo real, así como para informar sobre ellas. Este análisis le ayuda a determinar si debe agregar o editar las intenciones y las entidades de la aplicación de LUIS.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Capturar datos de bot y Language Understanding en Application Insights
> * Consultar datos de Language Understanding en Application Insights

## <a name="prerequisites"></a>Requisitos previos

* Un bot de Azure Bot Service, creado con Application Insights habilitado.
* Código de bot descargado del **[tutorial](luis-nodejs-tutorial-bf-v4.md)** de bot anterior. 
* [Bot Emulator](https://aka.ms/abs/build/emulatordownload)
* [Visual Studio Code](https://code.visualstudio.com/Download)

Todo el código de este tutorial está disponible en el [repositorio Azure-Samples de Language Understanding de GitHub](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/v4/luis-nodejs-bot-johnsmith-src-telemetry). 

## <a name="add-application-insights-to-web-app-bot-project"></a>Agregar Application Insights a un proyecto de bot de aplicación web
En la actualidad, el servicio de Application Insights que se usa en este bot de aplicación web recopila telemetría de estado general para el bot. No recopila información sobre LUIS. 

Para capturar la información de LUIS, el bot de aplicación web necesita que el paquete NPM de **[Application Insights](https://www.npmjs.com/package/applicationinsights)** esté instalado y configurado.  

1. En el terminal integrado de VSCode, en la raíz del proyecto de bot, agrega los siguientes paquetes NPM mediante el comando que se muestra: 

    ```console
    npm install applicationinsights && npm install underscore
    ```
    
    El paquete **subrayado** sirve para aplanar la estructura JSON de LUIS, para que sea más fácil de ver y usar en Application Insights.
    


## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Captura y envío de los resultados de consulta de LUIS a Application Insights

1. En VSCode, crea un nuevo archivo **appInsightsLog.js** y agrega el código siguiente:

    ```javascript
    const appInsights = require('applicationinsights');
    const _ = require("underscore");
    
    // Log LUIS results to Application Insights
    // must flatten as name/value pairs
    var appInsightsLog = (botContext,luisResponse) => {

        appInsights.setup(process.env.MicrosoftApplicationInsightsInstrumentationKey).start();
        const appInsightsClient = appInsights.defaultClient;

        // put bot context and LUIS results into single object
        var data = Object.assign({}, {'botContext': botContext._activity}, {'luisResponse': luisResponse});
    
        // Flatten data into name/value pairs
        flatten = (x, result, prefix) => {
            if(_.isObject(x)) {
                _.each(x, (v, k) => {
                    flatten(v, result, prefix ? prefix + '_' + k : k)
                })
            } else {
                result["LUIS_" + prefix] = x
            }
            return result;
        }
    
        // call fn to flatten data
        var flattenedData = flatten(data, {});
    
        // ApplicationInsights Trace 
        console.log(JSON.stringify(flattenedData));
    
        // send data to Application Insights
        appInsightsClient.trackTrace({message: "LUIS", severity: appInsights.Contracts.SeverityLevel.Information, properties: flattenedData});
    }
    
    module.exports.appInsightsLog = appInsightsLog;
    ```

    Este archivo toma el contexto del bot y la respuesta de LUIS, aplana ambos objetos y los inserta en un evento **Trace** en Application Insights. El nombre del evento es **LUIS**. 

1. Abre la carpeta **dialogs** y, luego, el archivo **luisHelper.js**. Incluye el nuevo **appInsightsLog.js** como archivo necesario y captura el contexto del bot y la respuesta de LUIS. El código completo de este archivo es: 

    ```javascript
    // Copyright (c) Microsoft Corporation. All rights reserved.
    // Licensed under the MIT License.
    
    const { LuisRecognizer } = require('botbuilder-ai');
    const { appInsightsLog } = require('../appInsightsLog');
    
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
    
                // APPINSIGHT: Log results to Application Insights
                appInsightsLog(context,recognizerResult);
    
    
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

## <a name="add-application-insights-instrumentation-key"></a>Agregar la clave de instrumentación de Application Insights 

Para agregar datos a Application Insights, necesitas la clave de instrumentación.

1. En un explorador, en [Azure Portal](https://portal.azure.com), busca el recurso**Application Insights** del bot. El nombre tendrá la mayor parte del nombre del bot, luego caracteres aleatorios al final del nombre, como `luis-nodejs-bot-johnsmithxqowom`. 
1. En el recurso de Application Insights, en la página **Información general**, copia la **Clave de instrumentación**.
1. En VSCode, abre el archivo **.env** en la raíz del proyecto de bot. En este archivo se encuentran todas las variables de entorno.  
1. Agrega una nueva variable `MicrosoftApplicationInsightsInstrumentationKey` con el valor de la clave de instrumentación. No pongas el valor entre comillas. 

## <a name="start-the-bot"></a>Inicio del bot

1. Desde el terminal integrado de VSCode, inicie el bot:
    
    ```console
    npm start
    ```

1. Inicia el emulador de bots y abre el bot. Este [paso](luis-nodejs-tutorial-bf-v4.md#use-the-bot-emulator-to-test-the-bot) se proporciona en el tutorial anterior.

1. Haz una pregunte al bot. Este [paso](luis-nodejs-tutorial-bf-v4.md#ask-bot-a-question-for-the-book-flight-intent) se proporciona en el tutorial anterior.

## <a name="view-luis-entries-in-application-insights"></a>Visualización de las entradas de LUIS en Application Insights

Abra Application Insights para ver las entradas de LUIS. Los datos pueden tardar unos minutos en aparecer en Application Insights.

1. En [Azure Portal](https://portal.azure.com), abre el recurso Application Insights del bot. 
1. Cuando se abra el recurso, selecciona **Buscar** y busca todos los datos en los últimos **30 minutos** con el tipo de evento **Seguimiento**. Selecciona el seguimiento denominado **LUIS**. 
1. La información del bot y de LUIS está disponible en **Propiedades personalizadas**. 

    ![Revisar las propiedades personalizadas de LUIS almacenadas en Application Insights](./media/luis-tutorial-appinsights/application-insights-luis-trace-custom-properties-nodejs.png)

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Consultar en Application Insights de la intención, puntuación y expresión
Application Insights ofrece la capacidad de consultar los datos con el lenguaje [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics), así como exportarlos a [Power BI](https://powerbi.microsoft.com). 

1. Selecciona **Log (Analytics)** (Registro [análisis]). Se abre una ventana nueva con una ventana de consulta en la parte superior y una ventana de tabla de datos por debajo. Si ha usado bases de datos antes, esta disposición le resultará familiar. La consulta representa los datos filtrados anteriores. La columna **CustomDimensions** tiene la información del bot y de LUIS.
1. Para extraer la intención, puntuación y expresión superiores, agregue lo siguiente justo encima de la última línea (la línea `|top...`) en la ventana de consulta:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_luisResponse_luisResult_topScoringIntent_intent)
    | extend score = todouble(customDimensions.LUIS_luisResponse_luisResult_topScoringIntent_score)
    | extend utterance = tostring(customDimensions.LUIS_luisResponse_text)
    ```

1. Ejecute la consulta. Las nuevas columnas de intención superior, puntuación y expresión están disponibles. Selecciona la columna topIntent para ordenar.

Obtén más información sobre el [lenguaje de consultas Kusto](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) o [la exportación de los datos a Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Pasos siguientes

Otra información que es posible que quiera agregar a los datos de Application Insights incluye el id. de la aplicación, el Id. de versión, la última fecha de cambio del modelo, la última fecha de entrenamiento y la última fecha de publicación. Estos valores se pueden obtener de la dirección URL del punto de conexión (los identificadores de la aplicación y la versión), o bien de una llamada a la API de creación, que después se establecen en la configuración del bot de aplicación web y se extraen desde allí.  

Si usa la misma suscripción de punto de conexión para más de una aplicación de LUIS, también debe incluir el identificador de suscripción y una propiedad que indique que es una clave compartida. 

> [!div class="nextstepaction"]
> [Más información sobre las expresiones de ejemplo](luis-how-to-add-example-utterances.md)
