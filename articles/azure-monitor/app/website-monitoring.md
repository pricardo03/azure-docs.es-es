---
title: Guía de inicio rápido de supervisión de sitios web con Application Insights de Azure Monitor | Microsoft Docs
description: Proporciona instrucciones para configurar rápidamente la supervisión de sitios web del cliente o explorador con Application Insights de Azure Monitor
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 07/15/2019
ms.service: application-insights
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: 9845329d30da40243751c00f7c0fa62fdcfe5a0d
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "67990091"
---
# <a name="start-monitoring-your-website"></a>Inicio de la supervisión del sitio web

Con Application Insights de Azure Monitor puede supervisar fácilmente la disponibilidad, el rendimiento y el uso del sitio web. También puede identificar y diagnosticar errores en la aplicación rápidamente sin tener que esperar a que un usuario informe de ellos. Application Insights proporciona tanto la supervisión del lado servidor como las funcionalidades de supervisión del lado cliente o explorador.

Esta guía de inicio rápido ayudará mediante la adición del [SDK de JavaScript de Application Insights de código abierto](https://github.com/Microsoft/ApplicationInsights-JS), que le permite comprender la experiencia del cliente o explorador para los visitantes del sitio web.

## <a name="prerequisites"></a>Requisitos previos

Para completar esta guía de inicio rápido:

- Necesita una suscripción de Azure.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="enable-application-insights"></a>Habilitación de Application Insights

Application Insights recopila datos de telemetría desde cualquier aplicación conectada a Internet, mediante la ejecución local o en la nube. Siga estos pasos para empezar a ver los datos.

1. Seleccione **Crear un recurso** > **Herramientas de administración** > **Application Insights**.

   > [!NOTE]
   >Si esta es la primera vez que crea un recurso de Application Insights, puede obtener más información en el artículo [Creación de recursos en Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource).

   Aparece un cuadro de configuración, use la tabla siguiente para rellenar los campos de entrada.

    | Configuración        | Valor           | DESCRIPCIÓN  |
   | ------------- |:-------------|:-----|
   | **Nombre**      | Nombre único global | Nombre que identifica la aplicación que se está supervisando |
   | **Grupo de recursos**     | myResourceGroup      | Nombre para el nuevo grupo de recursos que hospedará los datos de Application Insights |
   | **Ubicación** | Este de EE. UU | Elija una ubicación cerca de usted o de donde se hospeda la aplicación |

2. Haga clic en **Create**(Crear).

## <a name="create-an-html-file"></a>Creación de un archivo HTML

1. En el equipo local, cree un archivo llamado ``hello_world.html``. En este ejemplo el archivo se colocará en la raíz de la unidad C: en ``C:\hello_world.html``.
2. Copie el script siguiente en ``hello_world.html``:

    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <title>Azure Monitor Application Insights</title>
    </head>
    <body>
    <h1>Azure Monitor Application Insights Hello World!</h1>
    <p>You can use the Application Insights JavaScript SDK to perform client/browser-side monitoring of your website. To learn about more advanced JavaScript SDK configurations visit the <a href="https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md" title="API Reference">API reference</a>.</p>
    </body>
    </html>
    ```

## <a name="configure-app-insights-sdk"></a>Configuración del SDK de Application Insights

1. Seleccione **Introducción** > **Información esencial** > copie la **clave de instrumentación** de la aplicación.

   ![Formulario de nuevo recurso de Application Insights](media/website-monitoring/instrumentation-key-001.png)

2. Agregue el siguiente script en ``hello_world.html`` antes de la etiqueta de cierre ``</head>``:

   ```javascript
   <script type="text/javascript">
      var sdkInstance="appInsightsSDK";window[sdkInstance]="appInsights";var aiName=window[sdkInstance],aisdk=window[aiName]||function(e){
         function n(e){t[e]=function(){var n=arguments;t.queue.push(function(){t[e].apply(t,n)})}}var t={config:e};t.initialize=!0;var i=document,a=window;setTimeout(function(){var n=i.createElement("script");n.src=e.url||"https://az416426.vo.msecnd.net/next/ai.2.min.js",i.getElementsByTagName("script")[0].parentNode.appendChild(n)});try{t.cookie=i.cookie}catch(e){}t.queue=[],t.version=2;for(var r=["Event","PageView","Exception","Trace","DependencyData","Metric","PageViewPerformance"];r.length;)n("track"+r.pop());n("startTrackPage"),n("stopTrackPage");var s="Track"+r[0];if(n("start"+s),n("stop"+s),n("setAuthenticatedUserContext"),n("clearAuthenticatedUserContext"),n("flush"),!(!0===e.disableExceptionTracking||e.extensionConfig&&e.extensionConfig.ApplicationInsightsAnalytics&&!0===e.extensionConfig.ApplicationInsightsAnalytics.disableExceptionTracking)){n("_"+(r="onerror"));var o=a[r];a[r]=function(e,n,i,a,s){var c=o&&o(e,n,i,a,s);return!0!==c&&t["_"+r]({message:e,url:n,lineNumber:i,columnNumber:a,error:s}),c},e.autoExceptionInstrumented=!0}return t
      }({
         instrumentationKey:"xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx"
      });

      window[aiName]=aisdk,aisdk.queue&&0===aisdk.queue.length&&aisdk.trackPageView({});
   </script>
   ```

3. Modifique ``hello_world.html`` y agregue la clave de instrumentación.

4. Abra ``hello_world.html`` en una sesión de explorador local. Esto creará una vista de página única. Puede actualizar el explorador para generar varias vistas de páginas de prueba.

## <a name="start-monitoring-in-the-azure-portal"></a>Inicio de la supervisión en Azure Portal

1. Ahora puede volver a abrir la página **Introducción** de Application Insights en Azure Portal; para ello, de donde recuperó la clave de instrumentación, para ver los detalles de la aplicación en ejecución. Los cuatro gráficos predeterminados de la página de introducción se extienden a los datos de la aplicación del lado servidor. Dado que estamos instrumentando las interacciones del cliente o explorador con el SDK de JavaScript, esta vista en particular no se aplica a menos que también tengamos instalado un SDK del lado servidor.

2. Haga clic en el icono ![Mapa de aplicaciones](media/website-monitoring/006.png) **Analytics**.  Se abrirá **Analytics**, que proporciona un lenguaje de consulta avanzado para analizar todos los datos recopilados por Application Insights. Para ver los datos relacionados con las solicitudes de explorador del lado cliente, ejecute la siguiente consulta:

    ```kusto
    // average pageView duration by name
    let timeGrain=1s;
    let dataset=pageViews
    // additional filters can be applied here
    | where timestamp > ago(15m)
    | where client_Type == "Browser" ;
    // calculate average pageView duration for all pageViews
    dataset
    | summarize avg(duration) by bin(timestamp, timeGrain)
    | extend pageView='Overall'
    // render result in a chart
    | render timechart
    ```

   ![Gráfico de Analytics con las solicitudes de usuario durante un período de tiempo](./media/website-monitoring/analytics-query.png)

3. Vuelva a la página **Introducción**. Haga clic en **Explorador** en el encabezado **Investigar** y, después, seleccione **Rendimiento**. Aquí encontrará las métricas relacionadas con el rendimiento del sitio web. También hay una vista para analizar los errores y excepciones en su sitio web correspondiente. Puede hacer clic en **Ejemplos** para profundizar en los detalles de cada transacción. Desde aquí, puede tener acceso a la experiencia de los [detalles de la transacción de un extremo a otro](../../azure-monitor/app/transaction-diagnostics.md).

   ![Gráfico de las métricas del servidor](./media/website-monitoring/browser-performance.png)

4. Para comenzar a explorar las [herramientas de análisis del comportamiento del usuario](../../azure-monitor/app/usage-overview.md), en el menú principal de Application Insights seleccione [**Usuarios**](../../azure-monitor/app/usage-segmentation.md) bajo el encabezado **Uso**. Como estamos probando desde una sola máquina, solo se verán los datos de un usuario. Para un sitio web en vivo, la distribución de los usuarios podría tener el aspecto siguiente:

     ![Gráfico de usuario](./media/website-monitoring/usage-users.png)

5. Si se hubiera instrumentado un sitio web más complejo con múltiples páginas, otra herramienta útil es [**Flujos de usuario**](../../azure-monitor/app/usage-flows.md). Con **Flujos de usuario** puede hacer un seguimiento de la ruta que siguen los visitantes a través de las distintas partes del sitio web.

   ![Visualización de flujos de usuario](./media/website-monitoring/user-flows.png)

Para obtener configuraciones más avanzadas para supervisar sitios web, consulte la [referencia a JavaScript SDK API](https://github.com/Microsoft/ApplicationInsights-JS/blob/master/API-reference.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Si tiene previsto seguir trabajando con las siguientes guías de inicio rápido o tutoriales, no elimine los recursos creados en esta guía de inicio rápido. En caso contrario, si no tiene previsto continuar, siga estos pasos para eliminar todos los recursos creados en esta guía de inicio rápido en Azure Portal.

1. En el menú izquierdo de Azure Portal, haga clic en **Grupos de recursos** y en **myResourceGroup**.
2. En la página del grupo de recursos, haga clic en **Eliminar**, escriba **myResourceGroup** en el cuadro de texto y haga clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Búsqueda y diagnóstico de problemas de rendimiento](https://docs.microsoft.com/azure/application-insights/app-insights-analytics)
