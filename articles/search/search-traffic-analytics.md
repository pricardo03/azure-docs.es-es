---
title: Informe sobre los datos de análisis del tráfico de búsqueda
titleSuffix: Azure Cognitive Search
description: Habilite el análisis del tráfico de búsqueda para Azure Cognitive Search, recopile datos de telemetría y eventos iniciados por el usuario mediante Application Insights y, después, analice los resultados en un informe de Power BI.
author: HeidiSteen
manager: nitinme
ms.author: heidist
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 12/11/2019
ms.openlocfilehash: 84e60b0a942bad94d8e36eb20b5be8e3f55af80a
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190937"
---
# <a name="implement-search-traffic-analytics-in-azure-cognitive-search"></a>Implementación del análisis del tráfico de búsqueda en Azure Cognitive Search

Análisis de tráfico de búsqueda es un modelo de implementación de un bucle de comentarios para el servicio de búsqueda. El objetivo es recopilar la telemetría de eventos iniciados por el usuario mediante entradas de teclado y clics. Con esta información, puede determinar la eficacia de la solución de búsqueda, incluidos los términos de búsqueda más populares, la tasa de clics y las entradas de consulta que producen cero resultados.

Este patrón depende de [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) (una característica de [Azure monitor](https://docs.microsoft.com/azure/azure-monitor/)) para recopilar datos de usuario. También deberá agregar la instrumentación a su código de cliente, tal como se describe en este artículo. Por último, necesitará una herramienta de informes para analizar los datos. Se recomienda Power BI pero puede utilizar cualquier herramienta que se conecte a Application Insights.

> [!NOTE]
> El patrón que se describe en este artículo es para escenarios avanzados e información de la secuencia de clic generada por el cliente. También puede notificar la información de registro que genera el servicio de búsqueda. Para más información sobre los informes de registro del servicio, consulte [Supervisión del consumo de recursos y la actividad de consulta](search-monitor-usage.md).

## <a name="identify-relevant-search-data"></a>Identificación de los datos de búsqueda pertinentes

Para hacer que las métricas de búsqueda sean útiles, es necesario registrar algunas de las señales de los usuarios de la aplicación de búsqueda. Estas señales indican el contenido en el que los usuarios están interesados y que consideran relevantes para sus necesidades.

Hay dos señales que necesita Análisis de tráfico de búsqueda:

+ Eventos de búsqueda generados por el usuario: solo las consultas de búsqueda iniciadas por un usuario son interesantes. Las solicitudes de búsqueda usadas para rellenar las facetas, el contenido adicional o cualquier información interna, no son importantes y sesgan y desvían los resultados.

+ Eventos de clic generados por el usuario: con clics nos referimos en este documento a que un usuario selecciona un resultado de búsqueda determinado devuelto por una consulta de búsqueda. Un clic generalmente significa que un documento es un resultado relevante para una consulta de búsqueda específica.

Si vincula la búsqueda y eventos de clic con un identificador de correlación, es posible analizar los comportamientos de los usuarios en la aplicación. Estas informaciones de búsqueda son imposibles de obtener con solo los registros de tráfico de búsqueda.

## <a name="add-search-traffic-analytics"></a>Incorporación de los análisis del tráfico de búsqueda

Las señales que se mencionan en la sección anterior se deben recopilar a partir de la aplicación de búsqueda a medida que el usuario interactúa con ella. Application Insights es una solución de supervisión extensible, disponible para varias plataformas, con opciones de instrumentación flexibles. El uso de Application Insights le permite aprovechar las ventajas de los informes de búsqueda de Power BI creados por Azure Cognitive Search para facilitar el análisis de datos.

En la página del [portal](https://portal.azure.com) para el servicio Azure Cognitive Search, la página de análisis del tráfico de búsqueda contiene una hoja de referencia para seguir este modelo de telemetría. También puede seleccionar o crear un recurso de Application Insights y ver los datos necesarios, todo en el mismo lugar.

![Instrucciones de Análisis de tráfico de búsqueda][1]

## <a name="1---select-a-resource"></a>1- Selección de un recurso

Tiene que seleccionar un recurso de Application Insights para usar o crear uno si aún no lo tiene. Puede utilizar un recurso que ya esté en uso para registrar los eventos personalizados necesarios.

Al crear un nuevo recurso de Application Insights, todos los tipos de aplicación son válidos para este escenario. Seleccione el que mejor se adapte a la plataforma que usa.

Necesita la clave de instrumentación para crear el cliente de telemetría para la aplicación. Puede obtenerlo desde el panel del portal de Application Insights o desde la página de Análisis de tráfico de búsqueda seleccionando la instancia que desee usar.

## <a name="2---add-instrumentation"></a>2- Incorporación de la instrumentación

En este paso se instrumenta su propia aplicación de búsqueda con el recurso de Application Insights creado en el paso anterior. Existen cuatro pasos para este proceso:

**Paso 1: Creación de un cliente de telemetría**

Este es el objeto que envía eventos al recurso de Application Insights.

*C#*

    private TelemetryClient telemetryClient = new TelemetryClient();
    telemetryClient.InstrumentationKey = "<YOUR INSTRUMENTATION KEY>";

*JavaScript*

    <script type="text/javascript">var appInsights=window.appInsights||function(config){function r(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},u=document,e=window,o="script",s=u.createElement(o),i,f;s.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js";u.getElementsByTagName(o)[0].parentNode.appendChild(s);try{t.cookie=u.cookie}catch(h){}for(t.queue=[],i=["Event","Exception","Metric","PageView","Trace","Dependency"];i.length;)r("track"+i.pop());return r("setAuthenticatedUserContext"),r("clearAuthenticatedUserContext"),config.disableExceptionTracking||(i="onerror",r("_"+i),f=e[i],e[i]=function(config,r,u,e,o){var s=f&&f(config,r,u,e,o);return s!==!0&&t["_"+i](config,r,u,e,o),s}),t}
    ({
    instrumentationKey: "<YOUR INSTRUMENTATION KEY>"
    });
    window.appInsights=appInsights;
    </script>

Para otros lenguajes y plataformas, vea la [lista](https://docs.microsoft.com/azure/application-insights/app-insights-platforms) completa.

**Paso 2: Solicitud de un identificador de búsqueda para la correlación**

Para correlacionar las solicitudes de búsqueda con clics, es necesario tener un identificador de correlación que relacione estos dos eventos distintos. Azure Cognitive Search proporciona un identificador de búsqueda cuando lo solicita con un encabezado:

*C#*

    // This sample uses the .NET SDK https://www.nuget.org/packages/Microsoft.Azure.Search

    var client = new SearchIndexClient(<SearchServiceName>, <IndexName>, new SearchCredentials(<QueryKey>)
    var headers = new Dictionary<string, List<string>>() { { "x-ms-azs-return-searchid", new List<string>() { "true" } } };
    var response = await client.Documents.SearchWithHttpMessagesAsync(searchText: searchText, searchParameters: parameters, customHeaders: headers);
    IEnumerable<string> headerValues;
    string searchId = string.Empty;
    if (response.Response.Headers.TryGetValues("x-ms-azs-searchid", out headerValues)){
     searchId = headerValues.FirstOrDefault();
    }

*JavaScript*

    request.setRequestHeader("x-ms-azs-return-searchid", "true");
    request.setRequestHeader("Access-Control-Expose-Headers", "x-ms-azs-searchid");
    var searchId = request.getResponseHeader('x-ms-azs-searchid');

**Paso 3: Eventos de búsqueda de registros**

Cada vez que un usuario emite una solicitud de búsqueda, debe registrarla como un evento de búsqueda con el esquema siguiente en un evento personalizado de Application Insights:

**SearchServiceName**: (cadena) nombre de servicio de búsqueda **SearchId**: (guid) identificador único de una consulta de búsqueda (viene con la respuesta de búsqueda) **IndexName**: (cadena) índice de servicio que se va a consultar **QueryTerms**: (cadena) términos de búsqueda especificados por el usuario **ResultCount**: (int) número de documentos devueltos (viene en la respuesta de búsqueda) **ScoringProfile**: (cadena) nombre del perfil de puntuación usado, si lo hubiera

> [!NOTE]
> Solicite el recuento de consultas generadas por el usuario agregando $count=true a la consulta de búsqueda. Puede obtener más información [aquí](/rest/api/searchservice/search-documents#counttrue--false).
>

> [!NOTE]
> Recuerde registrar solo consultas de búsqueda generadas por usuarios.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search Id>},
    {"IndexName", <index name>},
    {"QueryTerms", <search terms>},
    {"ResultCount", <results count>},
    {"ScoringProfile", <scoring profile used>}
    };
    telemetryClient.TrackEvent("Search", properties);

*JavaScript*

    appInsights.trackEvent("Search", {
    SearchServiceName: <service name>,
    SearchId: <search id>,
    IndexName: <index name>,
    QueryTerms: <search terms>,
    ResultCount: <results count>,
    ScoringProfile: <scoring profile used>
    });

**Paso 4: Registro de eventos de clic**

Cada vez que un usuario hace clic en un documento, es una señal de que debe registrarse para fines de análisis de búsqueda. Utilice eventos personalizados de Application Insights para registrar estos eventos con el siguiente esquema:

**ServiceName**: (cadena) nombre de servicio de búsqueda **SearchId**: (guid) identificador único de la consulta de búsqueda relacionada **DocId**: (cadena) identificador del documento **Position**: (int) posición del documento en la página de resultados de búsqueda

> [!NOTE]
> La posición hace referencia al orden cardinal en la aplicación. Puede establecer este número, siempre que en todo momento sea el mismo, para permitir la comparación.
>

*C#*

    var properties = new Dictionary <string, string> {
    {"SearchServiceName", <service name>},
    {"SearchId", <search id>},
    {"ClickedDocId", <clicked document id>},
    {"Rank", <clicked document position>}
    };
    telemetryClient.TrackEvent("Click", properties);

*JavaScript*

    appInsights.trackEvent("Click", {
        SearchServiceName: <service name>,
        SearchId: <search id>,
        ClickedDocId: <clicked document id>,
        Rank: <clicked document position>
    });

## <a name="3---analyze-in-power-bi"></a>3- Análisis en Power BI

Una vez que haya instrumentado la aplicación y comprobado que esta se ha conectado correctamente a Application Insights, debe descargar una plantilla de informe predefinida para analizar los datos en Power BI Desktop. El informe contiene tablas y gráficos predefinidos útiles para analizar los datos adicionales que se capturaron para los análisis de tráfico de búsqueda. 

1. En el panel de navegación izquierdo de Azure Cognitive Search, en **Configuración**, haga clic en **Análisis de tráfico de búsqueda**.

2. En la página **Análisis de tráfico de búsqueda**, en el paso 3, haga clic en **Obtener Power BI Desktop** para instalar Power BI.

   ![Obtención de informes de Power BI](./media/search-traffic-analytics/get-use-power-bi.png "Obtención de informes de Power BI")

2. En la misma página, haga clic en **Descargar informe de Power BI**.

3. El informe se abrirá en Power BI Desktop y se le pedirá que se conecte a Application Insights y proporcione las credenciales. Puede encontrar la información de conexión en las páginas de Azure Portal para el recurso de Application Insights. Para obtener las credenciales, proporcione el mismo nombre de usuario y la misma contraseña que usa para iniciar sesión en el portal.

   ![Conexión a Application Insights](./media/search-traffic-analytics/connect-to-app-insights.png "Conexión a Application Insights")

4. Haga clic en **Cargar**.

El informe contiene gráficos y tablas que le ayudarán a tomar decisiones más informadas para mejorar la relevancia y el rendimiento de las búsquedas.

Las métricas incluyen los siguientes elementos:

* Busque los pares de términos y documentos más populares: términos resultantes en el mismo documento en el que se hizo clic, ordenados por clics.
* Búsquedas sin clics: términos de las consultas principales que no registran ningún clic

La siguiente captura de pantalla muestra los gráficos e informes integrados para analizar el análisis de tráfico de búsqueda.

![Panel de Power BI para Azure Cognitive Search](./media/search-traffic-analytics/azuresearch-powerbi-dashboard.png "Panel de Power BI para Azure Cognitive Search")

## <a name="next-steps"></a>Pasos siguientes
Instrumente la aplicación de búsqueda para obtener datos eficaces y reveladores sobre el servicio de búsqueda.

Puede encontrar más información en [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) y visitar la [página de precios](https://azure.microsoft.com/pricing/details/application-insights/) para conocer más sobre los distintos niveles de servicio.

Obtenga más información sobre cómo crear informes increíbles. Consulte [Introducción a Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-getting-started/) para más información.

<!--Image references-->
[1]: ./media/search-traffic-analytics/azuresearch-trafficanalytics.png
[2]: ./media/search-traffic-analytics/azuresearch-appinsightsdata.png
[3]: ./media/search-traffic-analytics/azuresearch-pbitemplate.png
