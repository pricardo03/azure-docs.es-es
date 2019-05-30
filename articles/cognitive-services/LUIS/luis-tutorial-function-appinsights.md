---
title: Application Insights, Node.js
titleSuffix: Azure Cognitive Services
description: Cree un bot integrado con una aplicación de LUIS y Application Insights mediante Node.js.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 05/30/2019
ms.author: diberry
ms.openlocfilehash: bde1983f89cb2fcd0a6fddadc2c3379dee4310be
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399627"
---
# <a name="add-luis-results-to-application-insights-and-azure-functions"></a>Incorporación de resultados de LUIS a Application Insights y Azure Functions
En este tutorial se agrega información de solicitud y respuesta de LUIS al almacenamiento de datos de telemetría de [Application Insights](https://azure.microsoft.com/services/application-insights/). Una vez que los datos, puede consultar con el lenguaje de Kusto o Power BI para analizar, agregado e informar sobre las intenciones y entidades de la declaración en tiempo real. Este análisis le ayuda a determinar si debe agregar o editar las intenciones y las entidades de la aplicación de LUIS.

El bot se compila con Bot Framework 3.x y el bot de aplicación web de Azure. Un [Bot Framework 4.x con el tutorial de LUIS](luis-nodejs-tutorial-bf-v4.md) también está disponible.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Adición de la biblioteca de Application Insights a un bot de aplicación web
> * Captura y envío de los resultados de consulta de LUIS a Application Insights
> * Consulta en Application Insights de la intención, puntuación y expresión principales

## <a name="prerequisites"></a>Requisitos previos

* El bot de aplicación web de LUIS del **[tutorial anterior](luis-nodejs-tutorial-build-bot-framework-sample.md)** con Application Insights está activado. 

> [!Tip]
> Si aún no tiene una suscripción, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/).

Todo el código de este tutorial está disponible en el [repositorio Azure-Samples de GitHub](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/nodejs) y cada línea asociada con este tutorial tiene el comentario `//APPINSIGHT:`. 

## <a name="web-app-bot-with-luis"></a>Bot de aplicación web con LUIS
En este tutorial se da por supuesto que tiene código similar al siguiente o que ha completado el [otro tutorial](luis-nodejs-tutorial-build-bot-framework-sample.md): 

   [!code-javascript[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/nodejs/app.js "Web app bot with LUIS")]

## <a name="add-application-insights-library-to-web-app-bot"></a>Adición de la biblioteca de Application Insights a un bot de aplicación web
En la actualidad, el servicio de Application Insights que se usa en este bot de aplicación web recopila telemetría de estado general para el bot. No recopila información de solicitud y respuesta de LUIS necesaria para comprobar y corregir las intenciones y las entidades. 

Para poder capturar la respuesta de LUIS, el bot de aplicación web necesita que se instale y configure el paquete de NPM de **[Application Insights](https://www.npmjs.com/package/applicationinsights)** en el archivo **app.js**. A continuación, los controladores de cuadro de diálogo de la intención tienen que enviar la información de solicitud y respuesta de LUIS a Application Insights. 

1. En Azure Portal, en el servicio de bots de aplicación web, seleccione **Build** (Compilar) en la sección **Bot Management** (Administración de bots). 

    ![En Azure Portal, en el servicio de bots de aplicación web, seleccione "Build" (Compilar) en la sección "Bot Management" (Administración de bots).](./media/luis-tutorial-appinsights/build.png)

2. Se abre una nueva pestaña del explorador con el Editor de App Service. Seleccione el nombre de la aplicación en la barra superior y, a continuación, seleccione **Open Kudu Console** (Abrir consola de Kudu). 

    ![Seleccione el nombre de la aplicación en la barra superior y, a continuación, seleccione "Open Kudu Console" (Abrir consola de Kudu).](./media/luis-tutorial-appinsights/kudu-console.png)

3. En la consola, escriba el siguiente comando para instalar los paquetes de subrayado y Application Insights:

    ```console
    cd site\wwwroot && npm install applicationinsights && npm install underscore
    ```

    ![Use los comandos npm para instalar los paquetes de subrayado y Application Insights.](./media/luis-tutorial-appinsights/npm-install.png)

    Espere a que se instalen los paquetes:

    ```console
    luisbot@1.0.0 D:\home\site\wwwroot
    `-- applicationinsights@1.0.1 
      +-- diagnostic-channel@0.2.0 
      +-- diagnostic-channel-publishers@0.2.1 
      `-- zone.js@0.7.6 
    
    npm WARN luisbot@1.0.0 No repository field.
    luisbot@1.0.0 D:\home\site\wwwroot
    +-- botbuilder-azure@3.0.4
    | `-- azure-storage@1.4.0
    |   `-- underscore@1.4.4 
    `-- underscore@1.8.3 
    ```

    Ya ha terminado con la pestaña del explorador de la consola de Kudu.

## <a name="capture-and-send-luis-query-results-to-application-insights"></a>Captura y envío de los resultados de consulta de LUIS a Application Insights
1. En la pestaña del explorador Editor de App Service, abra el archivo **app.js**.

2. Agregue las siguientes bibliotecas de NPM debajo de las líneas `requires` existentes:

   [!code-javascript[Add NPM packages to app.js](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=12-16 "Add NPM packages to app.js")]

3. Cree el objeto de Application Insights y use la configuración de aplicación **BotDevInsightsKey**del bot de aplicación web: 

   [!code-javascript[Create the Application Insights object](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=68-80 "Create the Application Insights object")]

4. Agregue la función **appInsightsLog**:

   [!code-javascript[Add the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=82-109 "Add the appInsightsLog function")]

    La última línea de la función es donde se agregan los datos a Application Insights. El nombre del evento es **LUIS-results**, un nombre único independiente de cualquier otro dato de telemetría recopilado por este bot de aplicación web. 

5. Use la función **appInsightsLog**. Agréguela a cada cuadro de diálogo de intención:

   [!code-javascript[Use the appInsightsLog function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/nodejs/app.js?range=117-118 "Use the appInsightsLog function")]

6. Para probar el bot de aplicación web, abra la característica **Test in Web Chat** (Probar en Chat en web). No debería ver ninguna diferencia porque todo el trabajo está en Application Insights, no en las respuestas del bot.

## <a name="view-luis-entries-in-application-insights"></a>Visualización de las entradas de LUIS en Application Insights
Abra Application Insights para ver las entradas de LUIS. 

1. En el portal, seleccione **Todos los recursos** y, después, filtre por el nombre del bot de aplicación web. Haga clic en el recurso con el tipo de **Application Insights**. El icono de Application Insights es una bombilla. 

    ! [[Busque app insights en Azure portal](./media/luis-tutorial-appinsights/search-for-app-insights.png)

2. Cuando el recurso se abra, haga clic en el icono **Buscar** de la lupa en el panel del extremo derecho. Se muestra un panel nuevo a la derecha. En función de cuántos datos de telemetría se encuentren, el panel puede tardar unos segundos en abrirse. Busque `LUIS-results` y presione ENTRAR en el teclado. La lista se limita simplemente a los resultados de consulta de LUIS que se agregaron con este tutorial.

    ![Filtrar las dependencias](./media/luis-tutorial-appinsights/app-insights-filter.png)

3. Seleccione la entrada superior. En una ventana nueva se muestran datos más detallados, incluidos los datos personalizados para la consulta de LUIS en el extremo derecho. Los datos incluyen la intención superior y su puntuación.

    ![Detalles de la dependencia](./media/luis-tutorial-appinsights/app-insights-detail.png)

    Cuando haya terminado, haga clic en la **X** del extremo superior derecho para volver a la lista de elementos de dependencia. 


> [!Tip]
> Si quiere guardar la lista de dependencias y volver a ella más adelante, haga clic en **...Más** y, después, en **Guardar favorito**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Consultar en Application Insights de la intención, puntuación y expresión
Application Insights proporciona la capacidad de consultar los datos con el [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics) lenguaje, así como exportar a [Power BI](https://powerbi.microsoft.com). 

1. Haga clic en **Analytics** (Análisis) en la parte superior de la lista de dependencias, encima del cuadro de filtro. 

    ![Botón Análisis](./media/luis-tutorial-appinsights/analytics-button.png)

2. Se abre una ventana nueva con una ventana de consulta en la parte superior y una ventana de tabla de datos por debajo. Si ha usado bases de datos antes, esta disposición le resultará familiar. En la consulta se incluyen todos los elementos de las últimas 24 horas que empiezan por el nombre `LUIS-results`. La columna **CustomDimensions** contiene los resultados de consulta de LUIS como pares nombre-valor.

    ![Ventana de consulta de Analytics](./media/luis-tutorial-appinsights/analytics-query-window.png)

3. Para extraer la intención, puntuación y expresión superiores, agregue lo siguiente justo encima de la última línea en la ventana de consulta:

    ```kusto
    | extend topIntent = tostring(customDimensions.LUIS_intent_intent)
    | extend score = todouble(customDimensions.LUIS_intent_score)
    | extend utterance = tostring(customDimensions.LUIS_text)
    ```

4. Ejecute la consulta. Desplácese hasta el extremo derecho de la tabla de datos. Las nuevas columnas de intención superior, puntuación y expresión están disponibles. Haga clic en la columna de intención superior para ordenar.

    ![Intención de análisis superior](./media/luis-tutorial-appinsights/app-insights-top-intent.png)


Obtenga más información sobre la [lenguaje de consulta Kusto](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) o [exportar los datos a Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 

## <a name="next-steps"></a>Pasos siguientes

Otra información que es posible que quiera agregar a los datos de Application Insights incluye el id. de la aplicación, el Id. de versión, la última fecha de cambio del modelo, la última fecha de entrenamiento y la última fecha de publicación. Estos valores se pueden obtener de la dirección URL del punto de conexión (los identificadores de la aplicación y la versión), o bien de una llamada a la [API de creación](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d), que después se establecen en la configuración del bot de aplicación web y se extraen desde allí.  

Si usa la misma suscripción de punto de conexión para más de una aplicación de LUIS, también debe incluir el identificador de suscripción y una propiedad que indique que es una clave compartida. 

> [!div class="nextstepaction"]
> [Más información sobre las expresiones de ejemplo](luis-how-to-add-example-utterances.md)
