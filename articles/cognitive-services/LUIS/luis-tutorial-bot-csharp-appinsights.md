---
title: Datos de Application Insights desde LUIS mediante C#
titleSuffix: Azure Cognitive Services
description: Cree un bot integrado con una aplicación de LUIS y Application Insights mediante C#.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/24/2018
ms.author: diberry
ms.openlocfilehash: 83ad70e1242af1e01af06206a3a141f455072a44
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038959"
---
# <a name="add-luis-results-to-application-insights"></a>Adición de resultados de LUIS a Application Insights
En este tutorial se agrega información de respuesta de LUIS al almacenamiento de datos de telemetría de [Application Insights](https://azure.microsoft.com/services/application-insights/). Una vez que tenga que los datos, puede consultarlos con el lenguaje Kusto o Power BI para analizar, agregar e informar sobre las intenciones y entidades de la expresión en tiempo real. Este análisis le ayuda a determinar si debe agregar o editar las intenciones y las entidades de la aplicación de LUIS. 

El bot se compila con Bot Framework 3.x y el bot de aplicación web de Azure.

En este tutorial, aprenderá a:

> [!div class="checklist"]
* Agregar Application Insights a un bot de aplicación web
* Capturar y enviar los resultados de consulta de LUIS a Application Insights
* Consulta en Application Insights de la intención, puntuación y expresión principales

## <a name="prerequisites"></a>Requisitos previos

* El bot de aplicación web de LUIS del **[tutorial anterior](luis-csharp-tutorial-build-bot-framework-sample.md)** con Application Insights está activado. 
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) instalado localmente en el equipo.

> [!Tip]
> Si aún no tiene una suscripción, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/free/).

Todo el código de este tutorial está disponible en el [repositorio LUIS-Samples de GitHub](https://github.com/Microsoft/LUIS-Samples/tree/master/documentation-samples/tutorial-web-app-bot-application-insights/csharp) y cada línea asociada con este tutorial se comenta con `//LUIS Tutorial:`. 

## <a name="review-luis-web-app-bot"></a>Revisar el bot de aplicación web de LUIS
En este tutorial se da por supuesto que tiene código similar al siguiente o que ha completado el [otro tutorial](luis-csharp-tutorial-build-bot-framework-sample.md): 

   [!code-csharp[Web app bot with LUIS](~/samples-luis/documentation-samples/tutorial-web-app-bot/csharp/BasicLuisDialog.cs "Web app bot with LUIS")]

## <a name="application-insights-in-web-app-bot"></a>Application Insights en el bot de aplicación web
En la actualidad, el servicio de Application Insights, que se agrega como parte de la creación del servicio de bot de aplicación web, recopila telemetría de estado general para el bot. No recopila información sobre las respuestas de LUIS. Para poder analizar y mejorar LUIS, necesita la información de respuesta de LUIS.  

Para poder capturar la respuesta de LUIS, el bot de aplicación web necesite que se instale y configure **[Application Insights](https://www.nuget.org/packages/Microsoft.ApplicationInsights/)** para el proyecto. 

## <a name="download-web-app-bot"></a>Descargar el bot de aplicación web
Use [Visual Studio 2017](https://www.visualstudio.com/downloads/) para agregar y configurar Application Insights para el bot de aplicación web. Para poder usar el bot de aplicación web en Visual Studio, descargue su código.

1. En Azure Portal, para el bot de aplicación web, seleccione **Compilar**.

    ![Selección de Compilar en el portal](./media/luis-tutorial-bot-csharp-appinsights/download-build-menu.png)

2. Haga clic en **Descargar archivo ZIP** y espere hasta que el archivo esté preparado.

    ![Descargar el archivo ZIP](./media/luis-tutorial-bot-csharp-appinsights/download-link.png)

3. Haga clic en **Descargar archivo ZIP** en la ventana emergente. Recuerde la ubicación en el equipo, ya que la necesitará en la sección siguiente.

    ![Ventana emergente de descarga del archivo ZIP](./media/luis-tutorial-bot-csharp-appinsights/download-popup.png)

## <a name="open-solution-in-visual-studio-2017"></a>Abrir la solución en Visual Studio 2017

1. Extraiga el archivo en una carpeta. 

2. Abra Visual Studio 2017 y el archivo de solución, `Microsoft.Bot.Sample.LuisBot.sln`. Si aparece la advertencia de seguridad, haga clic en "Aceptar".

    ![Abrir la solución en Visual Studio 2017](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-security-warning.png)

3. Visual Studio necesita agregar dependencias a la solución. En el **Explorador de soluciones**, haga clic con el botón derecho en **Referencias** y seleccione **Administrar paquetes NuGet...**. 

    ![Manage NuGet packages](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-manage-nuget-packages.png)

4. En el Administrador de paquetes NuGet se muestra una lista de los paquetes instalados. Haga clic en **Restaurar** en la barra de color amarillo. Espere a que finalice el proceso de restauración.

    ![Restaurar paquetes NuGet.](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-restore-packages.png)

## <a name="add-application-insights-to-the-project"></a>Agregar Application Insights al proyecto
Instale y configure Application Insights en Visual Studio. 

1. En Visual Studio 2017, en el menú superior, seleccione **Proyecto** y después **Agregar telemetría de Application Insights...**.

2. En la ventana **Configuración de Application Insights**, haga clic en **Inicio gratis**.

    ![Inicio de la configuración de Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configure-app-insights.png)

3. Registre la aplicación en Application Insights. Es posible que tenga que escribir las credenciales de Azure Portal. 

4. Visual Studio agrega Application Insights al proyecto, y muestra el estado al mismo tiempo. 

    ![Estado de Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-adding-application-insights-to-project.png)

    Cuando el proceso finaliza, se muestra el estado del progreso en **Configuración de Application Insights**. 

    ![Estado del progreso de Application Insights](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-configured-application-insights-to-project.png)

    La opción **Habilitar recopilación de seguimientos** se muestra en color rojo, lo que significa que no está habilitada. En este tutorial no se usa esta característica. 

## <a name="build-and-resolve-errors"></a>Compilar y resolver los errores

1. Para compilar la solución, seleccione el menú **Compilar**  y después **Recompilar solución**. Espere a que finalice la compilación. 

2. Si se produce un error `CS0104` en la compilación, tendrá que solucionarlo. En la carpeta `Controllers`, en el `MessagesController.cs file`, corrija el uso ambiguo del tipo `Activity` anteponiendo el tipo de actividad con el tipo de conector. Para ello, cambie el nombre `Activity` en las líneas 22 y 36 de `Activity` a `Connector.Activity`. Vuelva a compilar la solución. No debería haber más errores de compilación.

    El código fuente completo de ese archivo es el siguiente:

    [!code-csharp[MessagesController.cs file](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/MessagesController.cs "MessagesController.cs file")]

## <a name="publish-project-to-azure"></a>Publicar el proyecto en Azure
Ahora el paquete **Application Insights** está en el proyecto, configurado correctamente para las credenciales de Azure Portal. Es necesario volver a publicar en Azure los cambios para el proyecto.

1. En el **Explorador de soluciones**, haga clic con el botón derecho en el nombre del proyecto y seleccione **Publicar**.

    ![Publicar el proyecto en el portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish.png)

2. En la ventana **Publicar**, haga clic en **Crear perfil nuevo**.

    ![Publicar el proyecto en el portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-1.png)

3. Haga clic en **Importar perfil** y después en **Aceptar**.

    ![Publicar el proyecto en el portal](./media/luis-tutorial-bot-csharp-appinsights/vs-2017-publish-2.png)

4. En las ventanas **Importar archivo de configuración de publicación**, navegue hasta la carpeta del proyecto, vaya a la carpeta `PostDeployScripts`, seleccione el archivo que termine en `.PublishSettings` y haga clic en `Open`. Ya ha configurado la publicación para este proyecto. 

5. Para publicar el código fuente local en Bot Service, haga clic en el botón **Publicar**. En la ventana **Salida** se muestra el estado. El resto del tutorial se completa en el Azure Portal. Cierre Visual Studio 2017. 

## <a name="open-three-browser-tabs"></a>Abra tres pestañas del explorador
En Azure Portal, busque el bot de aplicación web y ábralo. En los pasos siguientes se usan tres vistas diferentes del bot de aplicación web. Probablemente sea más fácil abrir tres pestañas independientes en el explorador: 
  
>  * Probar en Chat en web
>  * Compilar/Abrir editor de código en línea -> Editor de App Service
>  * Editor de App Service/Abrir la consola de Kudu -> Consola de diagnóstico

## <a name="modify-basicluisdialogcs-code"></a>Modificar el código de BasicLuisDialog.cs

1. En la pestaña del explorador **Editor de App Service**, abra el archivo `BasicLuisDialog.cs`.

2. Agregue la dependencia de NuGet siguiente debajo de las líneas `using` existentes:

   [!code-csharp[Add using statement](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=11-12 "Add using statement")]

3. Agregue la función `LogToApplicationInsights`:

   [!code-csharp[Add the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=61-92 "Add the LogToApplicationInsights function")]

    La clave de instrumentación de Application Insights ya está en la configuración de la aplicación `BotDevInsightsKey` del bot de aplicación web. 

    La última línea de la función agrega los datos a Application Insights. El nombre del seguimiento es `LUIS`, un nombre único independiente de cualquier otro dato de telemetría recopilado por este bot de aplicación web. Todas las propiedades también tienen el prefijo `LUIS_` para que pueda ver los datos que este tutorial agrega en comparación a la información que proporciona el bot de aplicación web.

4. Llame a la función `LogToApplicationInsights` en la parte superior de la función `ShowLuisResult`:

   [!code-csharp[Use the LogToApplicationInsights function](~/samples-luis/documentation-samples/tutorial-web-app-bot-application-insights/csharp/BasicLuisDialog.cs?range=114-115 "Use the LogToApplicationInsights function")]

## <a name="build-web-app-bot"></a>Compilar el bot de aplicación web
1. Compile el bot de aplicación web de una de estas dos maneras. El primer método consiste en hacer clic con el botón derecho en `build.cmd` en el **Editor de App Service** y después seleccionar **Run from Console** (Ejecutar desde la consola). Se muestra el resultado de la consola y se completa con `Finished successfully.`

2. Si esto no se completa correctamente, abra la consola, desplácese hasta el script y ejecútelo mediante los pasos siguientes. En el **Editor de App Service**, en la barra de color azul superior, haga clic en el nombre del bot y luego seleccione **Open Kudu Console** (Abrir la consola de Kudu) en la lista desplegable.

    ![Abrir la consola de Kudu](./media/luis-tutorial-bot-csharp-appinsights/open-kudu-console.png)

3. En la ventana de la consola, escriba el comando siguiente:

    ```
    cd site\wwwroot && build.cmd
    ```

    Espere a que la compilación finalice con `Finished successfully.`.

## <a name="test-the-web-app-bot"></a>Probar el bot de aplicación web

1. Para probar el bot de aplicación web, abra la característica **Test in Web Chat** (Probar en Chat en web) en el portal. 

2. Escriba la frase `Coffee bar on please`.  

    ![Probar el bot de aplicación web en el chat](./media/luis-tutorial-bot-csharp-appinsights/test-in-web-chat.png)

3. No debería ver ninguna diferencia en la respuesta del bot de chat. El cambio envía los datos a Application Insights, no en las respuestas del bot. Escriba alguna expresión más para que haya más datos en Application Insights:

```
Please deliver a pizza
Turn off all the lights
Turn on the hall light
```

## <a name="view-luis-entries-in-application-insights"></a>Ver las entradas de LUIS en Application Insights
Abra Application Insights para ver las entradas de LUIS. 

1. En el portal, seleccione **Todos los recursos** y, después, filtre por el nombre del bot de aplicación web. Haga clic en el recurso con el tipo de **Application Insights**. El icono de Application Insights es una bombilla. 

    ![Buscar Application Insights](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights.png)

2. Cuando el recurso se abra, haga clic en el icono **Buscar** de la lupa en el panel del extremo derecho. Se muestra un panel nuevo a la derecha. En función de cuántos datos de telemetría se encuentren, el panel puede tardar unos segundos en abrirse. Busque `LUIS`. La lista se limita simplemente a los resultados de consulta de LUIS que se agregaron con este tutorial.

    ![Buscar seguimientos](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace.png)

3. Seleccione la entrada superior. En una ventana nueva se muestran datos más detallados, incluidos los datos personalizados para la consulta de LUIS en el extremo derecho. Los datos incluyen la intención superior y su puntuación.

    ![Revisión del elemento de seguimiento](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-trace-item.png)

    Cuando haya terminado, haga clic en la **X** del extremo superior derecho para volver a la lista de elementos de dependencia. 


> [!Tip]
> Si quiere guardar la lista de dependencias y volver a ella más adelante, haga clic en **...Más** y, después, en **Guardar favorito**.

## <a name="query-application-insights-for-intent-score-and-utterance"></a>Consultar en Application Insights de la intención, puntuación y expresión
Application Insights ofrece la capacidad de consultar los datos con el lenguaje [Kusto](https://docs.microsoft.com/azure/application-insights/app-insights-analytics#query-data-in-analytics), así como exportarlos a [Power BI](https://powerbi.microsoft.com). 

1. Haga clic en **Analytics** (Análisis) en la parte superior de la lista de dependencias, encima del cuadro de filtro. 

    ![Botón Análisis](./media/luis-tutorial-bot-csharp-appinsights/portal-service-list-app-insights-search-luis-analytics-button.png)

2. Se abre una ventana nueva con una ventana de consulta en la parte superior y una ventana de tabla de datos por debajo. Si ha usado bases de datos antes, esta disposición le resultará familiar. En la consulta se incluyen todos los elementos de las últimas 24 horas que empiezan por el nombre `LUIS`. La columna **CustomDimensions** contiene los resultados de consulta de LUIS como pares nombre-valor.

    ![Informe de análisis predeterminado](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-1.png)

3. Para extraer la intención, puntuación y expresión superiores, agregue lo siguiente justo encima de la última línea en la ventana de consulta:

    ```SQL
    | extend topIntent = tostring(customDimensions.LUIS_topScoringIntent)
    | extend score = todouble(customDimensions.LUIS_topScoringIntentScore)
    | extend utterance = tostring(customDimensions.LUIS_query)
    ```

4. Ejecute la consulta. Desplácese hasta el extremo derecho de la tabla de datos. Las nuevas columnas de intención superior, puntuación y expresión están disponibles. Haga clic en la columna de intención superior para ordenar.

    ![Informe de análisis personalizado](./media/luis-tutorial-bot-csharp-appinsights/analytics-query-2.png)


Obtenga más información sobre el [lenguaje de consultas Kusto](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries) o [la exportación de los datos a Power BI](https://docs.microsoft.com/azure/application-insights/app-insights-export-power-bi). 


## <a name="learn-more-about-bot-framework"></a>Más información sobre Bot Framework
Obtenga más información sobre [Bot Framework](https://dev.botframework.com/).

## <a name="next-steps"></a>Pasos siguientes

Otra información que es posible que quiera agregar a los datos de Application Insights incluye el id. de la aplicación, el Id. de versión, la última fecha de cambio del modelo, la última fecha de entrenamiento y la última fecha de publicación. Estos valores se pueden obtener de la dirección URL del punto de conexión (los identificadores de la aplicación y la versión), o bien de una llamada a la [API de creación](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c3d), que después se establecen en la configuración del bot de aplicación web y se extraen desde allí.  

Si usa la misma suscripción de punto de conexión para más de una aplicación de LUIS, también debe incluir el identificador de suscripción y una propiedad que indique que es una clave compartida. 

> [!div class="nextstepaction"]
> [Más información sobre las expresiones de ejemplo](luis-how-to-add-example-utterances.md)
