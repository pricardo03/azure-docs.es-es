---
title: 'Tutorial: Exploración de la biblioteca de cliente JavaScript de Azure Time Series Insights | Microsoft Docs'
description: Obtenga información acerca de la biblioteca de cliente JavaScript de Azure Time Series Insights y el modelo de programación relacionado.
author: ashannon7
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: tutorial
ms.date: 05/06/2019
ms.author: anshan
ms.custom: seodec18
ms.openlocfilehash: 7208c0d42cba0e7f04fc6876bf3ada9fa65a00d9
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991451"
---
# <a name="tutorial-explore-the-azure-time-series-insights-javascript-client-library"></a>Tutorial: Exploración de la biblioteca de cliente JavaScript de Azure Time Series Insights

Para ayudar a los desarrolladores web a consultar y visualizar datos almacenados en Time Series Insights, se ha desarrollado la biblioteca cliente de Azure Time Series Insights basada en D3 de JavaScript. Este tutorial le guiará por la biblioteca cliente de Time Series Insights y el modelo de programación mediante una aplicación de ejemplo hospedada.

El tutorial detalla cómo trabajar con la biblioteca, acceder a los datos de Time Series Insights y utilizar los controles de gráfico para representar y visualizar datos. También aprenderá a experimentar con distintos tipos de representaciones para visualizar los datos. Al final del tutorial, podrá usar la biblioteca cliente para incorporar características de Time Series Insights a su propia aplicación web.

En concreto, aprenderá sobre lo siguiente:

> [!div class="checklist"]
> * Aplicación Time Series Insights de ejemplo
> * Biblioteca cliente JavaScript de Time Series Insights
> * Cómo usa la aplicación de ejemplo la biblioteca para visualizar datos de Time Series Insights

> [!NOTE]
> * Este tutorial utiliza una [demostración web de Time Series Insights](https://insights.timeseries.azure.com/clientsample) hospedada y gratuita.
> * Los archivos de origen de la aplicación de ejemplo de Time Series Insights se encuentran en el [repositorio de ejemplo de GitHub](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).
> * Lea la [documentación de referencia sobre el cliente de Time Series Insights](https://github.com/microsoft/tsiclient/blob/master/docs/API.md).

## <a name="video"></a>Vídeo

En este vídeo, le presentamos el SDK de JavaScript para Time Series Insights en código abierto:
<br /><br />

> [!VIDEO https://www.youtube.com/embed/X8sSm7Pl9aA]

## <a name="prerequisites"></a>Requisitos previos

Este tutorial usa la característica **Herramientas de desarrollo** del explorador. Los exploradores web modernos ([Microsoft Edge](/microsoft-edge/devtools-guide), [Chrome](https://developers.google.com/web/tools/chrome-devtools/), [FireFox](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools), [Safari](https://developer.apple.com/safari/tools/) y otros) suelen proporcionar acceso a la **vista del inspector web** mediante la tecla de acceso rápido F12 del teclado. Otra forma de acceder a la vista consiste en hacer clic con el botón derecho en una página web y, a continuación, seleccionar **Inspeccionar elemento**.

## <a name="time-series-insights-sample-application"></a>Aplicación de Time Series Insights de ejemplo

En este tutorial, se usa una aplicación Time Series Insights de ejemplo hospedada y gratuita para explorar el código fuente subyacente de la aplicación y explorar la biblioteca cliente JavaScript de Time Series Insights. Mediante el uso de la aplicación de ejemplo, aprenderá a cómo interactuar con Time Series Insights en JavaScript y a visualizar los datos mediante tablas y gráficos.

1. Vaya a la [aplicación de Time Series Insights de ejemplo](https://insights.timeseries.azure.com/clientsample). Aparece la siguiente solicitud de inicio de sesión:

   [![Solicitud de inicio de sesión de ejemplo de Time Series Insights](media/tutorial-explore-js-client-lib/tcs-sign-in.png)](media/tutorial-explore-js-client-lib/tcs-sign-in.png#lightbox)

1. Seleccione **Iniciar sesión** y escriba sus credenciales, o selecciónelas. Use una cuenta de empresa o de organización (Azure Active Directory) o una cuenta personal (cuenta Microsoft).

   [![Petición de credenciales de ejemplo de cliente de Time Series Insights](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png)](media/tutorial-explore-js-client-lib/tcs-sign-in-enter-account.png#lightbox)

1. Después de iniciar sesión, aparece una página que muestra gráficos con datos de Time Series Insights. Tanto su cuenta de usuario como la opción **Cerrar sesión** se pueden ver en la esquina superior derecha:

   [![Página principal del ejemplo de cliente de Time Series Insights después de iniciar sesión](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png)](media/tutorial-explore-js-client-lib/tcs-main-after-signin.png#lightbox)

### <a name="page-source-and-structure"></a>Estructura y origen de la página

Primero, vamos a ver el [código fuente HTML y JavaScript](https://github.com/Microsoft/tsiclient/blob/tutorial/pages/tutorial/index.html) de la página web representada:

1. Abra **Herramientas de desarrollo** en el explorador. Inspeccione los elementos HTML que componen la página actual (también conocidos como árbol de DOM o HTML).

1. Expanda los elementos `<head>` y `<body>` y observe las siguientes secciones:

   * En el elemento `<head>`, encontrará los metadatos de la página y las dependencias que permiten que la aplicación se ejecute:
     * Un elemento `<script>` que se utiliza para hacer referencia al archivo *adal.min.js* de la Biblioteca de autenticación de Azure Active Directory. ADAL es una biblioteca de JavaScript que proporciona autenticación OAuth 2.0 (inicio de sesión) y adquisición de tokens para acceder a las API.
     * Varios elementos `<link>` para las hojas de estilos (también conocidas como *CSS*) como *sampleStyles.css* y *tsiclient.css*. Las hojas de estilo se utilizan para controlar los detalles estilísticos visuales de una página, como los colores, las fuentes y el espaciado.
     * Un elemento `<script>` que se usa para hacer referencia a la biblioteca cliente de JavaScript para Time Series Insights *tsiclient.js*. La página usa la biblioteca para llamar a las API del servicio Time Series Insights y representar los controles de gráfico en la página.

     >[!NOTE]
     > * El código fuente de la biblioteca JavaScript de ADAL está disponible en el [repositorio azure-activedirectory-library-for-js](https://github.com/AzureAD/azure-activedirectory-library-for-js).
     > * El código fuente de la biblioteca cliente de JavaScript para Time Series Insights está disponible en el [repositorio tsiclient](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial).

   * En `<body>`, encontrará elementos `<div>`, que ayudan a definir el diseño de los elementos en la página, y otro elemento `<script>`:
     * El primer elemento `<div>` especifica el cuadro de diálogo **Iniciar sesión** (`id="loginModal"`).
     * El segundo elemento `<div>` actúa como elemento primario de:
       * Un elemento `<div>` del encabezado que se usa para los mensajes de estado y la información de inicio de sesión de la parte superior de la página (`class="header"`).
       * Un elemento `<div>` para el resto de los elementos del cuerpo de la página, incluidos los gráficos (`class="chartsWrapper"`).
       * Una sección `<script>` que contiene el código JavaScript que se usa para controlar la página.

   [![Ejemplo de cliente de Time Series Insights con Herramientas de desarrollo](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-head-body.png#lightbox)

1. Expanda el elemento `<div class="chartsWrapper">` y descubrirá más elementos `<div>` secundarios. Estos elementos se utilizan para colocar cada uno de los ejemplos de control de gráfico. Hay varios pares de elementos `<div>`, uno para cada ejemplo de gráfico:

   * El primer elemento (`class="rowOfCardsTitle"`) contiene un título descriptivo que resume lo que muestran los gráficos. Por ejemplo: `Static Line Charts With Full-Size Legends.`
   * El segundo elemento (`class="rowOfCards"`) es un elemento primario, que contiene elementos `<div>` secundarios adicionales que colocan los controles de gráficos reales en una fila.

   [![Elementos div del cuerpo](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-divs.png#lightbox)

1. Expanda el elemento `<script type="text/javascript">` que está inmediatamente debajo de `<div class="chartsWrapper">`. El principio de la sección de JavaScript de nivel de página se usa para controlar toda la lógica de la página (la autenticación, la llamada a las API del servicio Time Series Insights y la representación de los controles de gráfico, entre otras tareas):

   [![Script del cuerpo](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png)](media/tutorial-explore-js-client-lib/tcs-devtools-callouts-body-script.png#lightbox)

## <a name="time-series-insights-javascript-client-library-concepts"></a>Conceptos de la biblioteca cliente JavaScript de Time Series Insights

La biblioteca cliente de Time Series Insights (*tsclient.js*) proporciona abstracciones para dos funciones importantes de JavaScript:

* **Métodos de contenedor para llamar a las API de consulta de Time Series Insights**: API REST que puede usar para consultar los datos de Time Series Insights mediante el uso de expresiones de agregado. Los métodos se organizan en el espacio de nombres TsiClient.Server de la biblioteca.

* **Métodos para crear y rellenar varios tipos de controles de gráficos**: Métodos que puede usar para representar los datos de agregado de Time Series Insights en una página web. Los métodos se organizan en el espacio de nombres TsiClient.UX de la biblioteca.

Mediante estas simplificaciones, los desarrolladores pueden crear componentes de gráficos de IU que funcionan con datos de Time Series Insights con mayor facilidad.

### <a name="authentication"></a>Authentication

La [aplicación de ejemplo de Time Series Insights](https://insights.timeseries.azure.com/clientsample) es una aplicación de página única que admite la autenticación de usuario de ADAL OAuth 2.0:

1. Al usar ADAL para la autenticación, la aplicación cliente debe registrarse en Azure Active Directory (Azure AD). De hecho, la aplicación de una página está registrada para usar el [flujo de concesión implícita de OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-implicit-grant-flow).
1. La aplicación debe especificar algunas de las propiedades de registro en el entorno de ejecución. Las propiedades incluyen el GUID del cliente (`clientId`) y el URI de redireccionamiento (`postLogoutRedirectUri`).
1. Más adelante, la aplicación solicita un *token de acceso* de Azure AD. El token de acceso se emite para un conjunto finito de permisos, para un identificador de servicio o API específico (https:\//api.timeseries.azure.com). Los permisos de token se emiten en nombre del usuario con la sesión iniciada. El identificador del servicio o la API es otra propiedad que se incluye en el registro de Azure AD de la aplicación.
1. Una vez que ADAL devuelve el token de acceso a la aplicación, se pasa como un *token de portador* al acceder a las API del servicio Time Series Insights.

   [!code-javascript[head-sample](~/samples-javascript/pages/tutorial/index.html?range=147-204&highlight=3-7,34-37)]

> [!TIP]
> Para más información acerca de las bibliotecas de autenticación de Azure AD compatibles con Microsoft, consulte la [documentación de referencia de la Biblioteca de autenticación de Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries#microsoft-supported-client-libraries).

### <a name="control-identification"></a>Identificación del control

En el ejemplo proporcionado, los elementos de `<div>` se organizan en el elemento `<body>` principal para proporcionar un diseño razonable para los controles de gráfico que se representan en la página.

Todos los elementos `<div>` especifican las propiedades de la posición y los atributos visuales de los controles de gráfico. Las propiedades del elemento HTML `id` sirven como identificadores únicos para enlazar a controles específicos para representar y actualizar los datos visualizados.

### <a name="aggregate-expressions"></a>Expresiones de agregado

Las API de la biblioteca cliente de Time Series Insights usan expresiones de agregado:

* Una expresión de agregado proporciona la capacidad de crear uno o más *términos de búsqueda*.

* Las API de cliente están diseñadas para ofrecer una funcionalidad similar a otra aplicación de demostración (el [explorador de Time Series Insights](https://insights.timeseries.azure.com/demo)), que usa el intervalo de búsqueda, predicados `where`, medidas y valores `splitBy`.

* La mayoría de las API de biblioteca cliente toman una matriz de expresiones de agregado que usa el servicio para crear una consulta de datos de Time Series Insights.

### <a name="call-pattern"></a>Patrón de llamada

Las acciones de rellenar y representar controles de gráfico siguen un patrón general. Puede observar el patrón general en la aplicación de ejemplo y este puede ayudarle cuando use la biblioteca cliente:

1. Declare un elemento `array` que contenga una o varias expresiones de agregado de Time Series Insights:

   ```javascript
   var aes =  [];
   ```

1. Compile *1* a *n* objetos de expresión de agregado. Luego, agréguelos a la matriz de la expresión de agregado:

   ```javascript
   var ae = new tsiClient.ux.aggregateExpression(predicateObject, measureObject, measureTypes, searchSpan, splitByObject, color, alias, contextMenuActions);
   aes.push(ae);
   ```

   **parámetros aggregateExpression**

   | Parámetro | DESCRIPCIÓN | Ejemplo |
   | --------- | ----------- | ------- |
   | `predicateObject` | La expresión de filtrado de datos |`{predicateString: "Factory = 'Factory3'"}` |
   | `measureObject`   | El nombre de propiedad de la medida que se usa | `{property: 'Temperature', type: "Double"}` |
   | `measureTypes`    | Las agregaciones de la propiedad de medida que desea | `['avg', 'min']` |
   | `searchSpan`      | Duración y tamaño del intervalo de la expresión de agregado | `{from: startDate, to: endDate, bucketSize: '2m'}` |
   | `splitByObject`   | Propiedad de cadena por la que quiere realizar la división (opcional: puede ser null) | `{property: 'Station', type: 'String'}` |
   | `color`         | Color de los objetos que quiere representar | `'pink'` |
   | `alias`           | Nombre descriptivo para la expresión de agregado | `'Factory3Temperature'` |
   | `contextMenuActions` | Matriz de acciones que se enlazará a los objetos de la serie temporal en una visualización (opcional) | Para más información, consulte [Menús contextuales emergentes](#pop-up-context-menus). |

1. Llame a una consulta de Time Series Insights mediante las API de TsiClient.Server para solicitar los datos de agregados:

   ```javascript
   tsiClient.server.getAggregates(token, envFQDN, aeTsxArray);
   ```

   **parámetros getAggregates**

   | Parámetro | DESCRIPCIÓN | Ejemplo |
   | --------- | ----------- | ------- |
   | `token`     | El token de acceso para la API de Time Series Insights |  `authContext.getTsiToken()`<br />Para más información, consulte [Autenticación](#authentication). |
   | `envFQDN`   | El nombre de dominio completo (FQDN) del entorno de Time Series Insights | Desde Azure Portal. Por ejemplo: `10000000-0000-0000-0000-100000000108.env.timeseries.azure.com`. |
   | `aeTsxArray` | Una matriz de expresiones de consulta de Time Series Insights | Use la variable `aes` como se ha descrito anteriormente: `aes.map(function(ae){return ae.toTsx()}`. |

1. Transforme el resultado comprimido que devuelve la consulta de Time Series Insights en formato JSON para verlo:

   ```javascript
   var transformedResult = tsiClient.ux.transformAggregatesForVisualization(result, aes);
   ```

1. Cree un control de gráfico con las API de TsiClient.UX. Enlace a uno de los elementos `<div>` de la página:

   ```javascript
   var barChart = new tsiClient.ux.BarChart(document.getElementById('chart3'));
   ```

1. Rellene el control de gráfico con los objetos de datos JSON transformados y represéntelo en la página:

   ```javascript
   barChart.render(transformedResult, {grid: true, legend: 'compact', theme: 'light'}, aes);
   ```

## <a name="render-controls"></a>Representación de controles

La biblioteca de cliente de Time Series Insights proporciona ocho controles únicos y analíticos listos para usar:

* **gráfico de líneas**
* **gráfico circular**
* **gráfico de barras**
* **heatmap**
* **controles de la jerarquía**
* **cuadrícula accesible**
* **escalas de tiempo de eventos discretos**
* **escalas de tiempo de transición de estado**

### <a name="line-chart-bar-chart-and-pie-chart-examples"></a>Ejemplos de gráfico de líneas, gráfico de barras y gráfico circular

Examine el código de demostración usado para representar algunos de los controles de gráfico estándar. Observe el modelo de programación y los patrones para crear esos controles. En concreto, examine el HTML del comentario `// Example 3/4/5`, que representa los controles con los valores de HTML `id`, `chart3`, `chart4` y `chart5`.

Recuerde que en el paso 3 de la [sección Estructura y origen de la página](#page-source-and-structure), los controles de gráfico se organizan en filas en la página. Cada uno de los controles de gráfico tiene una fila con el título descriptivo. En este ejemplo, los tres gráficos se rellenan en el elemento `Multiple Chart Types From the Same Data` `<div>` del título, y se enlazan a los tres elementos `<div>` que se encuentran debajo de este título:

[!code-html[code-sample1-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=59-73&highlight=1,5,9,13)]

La siguiente sección del código JavaScript usa los patrones que se esbozaron anteriormente: generar expresiones de agregado de Time Series Insights, usarlas para realizar consultas en los datos de Time Series Insights y representar los tres gráficos. Se usan tres tipos de gráficos desde el espacio de nombres tsiClient.ux: `LineChart`, `BarChart` y `PieChart`. Los tipos de gráficos se utilizan para crear y representar los gráficos respectivos. Los tres gráficos pueden usar los mismos datos de expresiones de agregado `transformedResult`:

[!code-javascript[code-sample2-line-bar-pie](~/samples-javascript/pages/tutorial/index.html?range=241-262&highlight=13-14,16-17,19-20)]

Los tres gráficos aparecen de la siguiente manera cuando se representan:

[![Varios tipos de gráficos a partir de los mismos datos](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png)](media/tutorial-explore-js-client-lib/tcs-multiple-chart-types-from-the-same-data.png#lightbox)

## <a name="advanced-features"></a>Características avanzadas

La biblioteca cliente de Time Series Insights tiene varias características adicionales que puede utilizar para implementar las visualizaciones de datos de forma creativa.

### <a name="states-and-events"></a>Estados y eventos

Una funcionalidad avanzada que se ofrece es la capacidad de agregar transiciones de estado y eventos discretos a los gráficos. Esta característica es útil para resaltar incidentes y alertas y crear conmutadores de estado (como activado/desactivado).

Examine el código que rodea al comentario `// Example 10`. El código representa un control de línea debajo del título `Line Charts with Multiple Series Types` y lo enlaza al elemento `<div>` con el valor de HTML `id` `chart10`.

Los siguientes pasos describen el proceso:

1. Se define una estructura denominada `events4`, que contendrá los elementos de cambio de estado a los que se va a realizar un seguimiento. La estructura contiene:

   * Una clave de cadena denominada `Component States`.
   * Una matriz de objetos de valor que representan los estados. Todos estos objeto incluyen:
     * Una clave de cadena que contiene una marca de tiempo ISO de JavaScript.
     * Una matriz que contiene las características del estado: un color y una descripción.

1. La estructura `events5` se define para `Incidents`, que contiene una matriz de los elementos de eventos de los que se va a realizar un seguimiento. La estructura de la matriz tiene la misma forma que la que se esquematiza para `events4`.

1. Se representa el gráfico de líneas, en el que se usan las dos estructuras con los parámetros de las opciones del gráfico: `events:` y `states:`. Observe los demás parámetros de opción, para especificar `tooltip:`, `theme:` o `grid:`.

[!code-javascript[code-sample-states-events](~/samples-javascript/pages/tutorial/index.html?range=337-389&highlight=5,26,51)]

Los marcadores con forma de rombo o las ventanas emergentes se utilizan para indicar incidentes, mientras que las barras de color o las ventanas emergentes de la escala de tiempo indican cambios de estado:

[![Gráficos de líneas con varios tipos de series](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png)](media/tutorial-explore-js-client-lib/tcs-line-charts-with-multiple-series-types.png#lightbox)

### <a name="pop-up-context-menus"></a>Menús contextuales emergentes

Otra funcionalidad avanzada es la capacidad de crear menús contextuales personalizados (menús emergentes que se abren al hacer clic con el botón derecho). Los menús contextuales personalizados son útiles para habilitar acciones y pasos lógicos en el ámbito de una aplicación.

Busque código en torno al comentario `// Example 13/14/15`. Inicialmente, este código representa un gráfico de líneas en el título `Line Chart with Context Menu to Create Pie/Bar Chart`. El gráfico está enlazado al elemento `<div>` con el valor de HTML `id` `chart13`.

Gracias a los menús contextuales, el gráfico de líneas proporciona la funcionalidad de crear de forma dinámica un gráfico circular y un gráfico de barras que están enlazados a los elementos `<div>` con los identificadores `chart14` y `chart15`. El gráfico circular y el gráfico de barras también utilizan los menús contextuales para habilitar sus propias características: la capacidad de copiar datos del gráfico circular al gráfico de barras y de imprimir los datos del gráfico de barras en la ventana de consola del explorador, respectivamente.

Los siguientes pasos describen el proceso:

1. Se define una serie de acciones personalizadas. Cada acción contiene una matriz con uno o varios elementos. Cada elemento define un elemento de menú contextual único:

   * `barChartActions`: esta acción define el menú contextual del gráfico circular, que contiene un elemento para definir un elemento individual:
     * `name`: el texto que se usa para el elemento de menú: "Imprimir parámetros en la consola".
     * `action`: la acción asociada al elemento de menú. Dicha acción es siempre una función anónima que toma tres argumentos que se basan en la expresión de agregado que se usa para crear el gráfico. En este caso, los argumentos se escriben en la ventana de la consola del explorador:
       * `ae`: la matriz de la expresión de agregado.
       * `splitBy`: el valor de `splitBy`.
       * `timestamp`: la marca de tiempo.

   * `pieChartActions`: esta acción define el menú contextual del gráfico de barras, que contiene un elemento para definir un elemento individual. La forma y el esquema coinciden con los del elemento `barChartActions` que se ha descrito anteriormente, pero tenga en cuenta que la función `action` es completamente diferente: crea instancias del gráfico de barras y lo representa. El argumento `ae` se utiliza para especificar la matriz de la expresión de agregado que se usa en el runtime cuando se abre el elemento de menú. La función también establece la propiedad `ae.contextMenu` con el menú contextual `barChartActions`.
   * `contextMenuActions`: esta acción define el menú contextual del gráfico de líneas, que contiene tres elementos para definir tres elementos de menú. La forma y el esquema de cada elemento coinciden con los elementos que se han descrito anteriormente. Al igual que el elemento `barChartActions`, el primer elemento escribe los tres argumentos de la función en la ventana de la consola del explorador. De forma similar al elemento `pieChartActions`, los dos elementos segundos crean una instancia de los gráficos circular y de barras, respectivamente, y los representan. Los segundos dos elementos también establecen sus propiedades `ae.contextMenu` con los menús contextuales `pieChartActions` y `barChartActions`, respectivamente.

1. Se insertan dos expresiones de agregado en la matriz de la expresión de agregado `aes`. Se especifica la matriz `contextMenuActions` para cada elemento. Dichas expresiones se usan con el control del gráfico de líneas.

1. Solo el gráfico de líneas se representa inicialmente, desde el cual se pueden representar el gráfico circular y el gráfico de barras en el runtime.

[!code-javascript[code-sample-context-menus](~/samples-javascript/pages/tutorial/index.html?range=461-540&highlight=7,16,29,61-64,78)]

La captura de pantalla siguiente muestra los gráficos con sus respectivos menús contextuales emergentes. Los gráficos circulares y de barras se crearon de forma dinámica con las opciones del menú contextual del gráfico de líneas.

[![Gráfico de líneas con menú contextual para crear gráficos circulares o de barras](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart.png#lightbox)

### <a name="brushes"></a>Pinceles

Los pinceles pueden utilizarse para definir el ámbito de un intervalo de tiempo para definir acciones como las de zoom y exploración.

El código que sirve para ilustrar los pinceles se muestra en el ejemplo `Line Chart with Context Menu to Create Pie/Bar Chart` que describe los menús contextuales emergentes.

* Las acciones de los pinceles se parecen mucho a las de un menú contextual en que definen una serie de acciones personalizadas para el pincel. Cada acción contiene una matriz con uno o varios elementos. Cada elemento define un elemento de menú contextual único:
   * `name`: el texto que se usa para el elemento de menú: "Imprimir parámetros en la consola".
   * `action`: la acción que está asociada con el elemento de menú, que siempre es una función anónima que toma dos argumentos. En este caso, los argumentos se escriben en la ventana de la consola del explorador:
     * `fromTime`: la marca de tiempo `from` de la selección de pincel.
     * `toTime`: la marca de tiempo `to` de la selección de pincel.

* Las acciones de pincel se agregan como otra propiedad de la opción de gráfico. La propiedad `brushContextMenuActions: brushActions` se pasa a la llamada `linechart.Render`.

[!code-javascript[code-sample-brushes](~/samples-javascript/pages/tutorial/index.html?range=526-540&highlight=1,13)]

[![Gráfico de líneas con menú contextual para crear gráficos circulares o de barras mediante pinceles](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png)](media/tutorial-explore-js-client-lib/tcs-line-chart-with-context-menu-to-create-pie-bar-chart-brushes.png#lightbox)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Iniciar sesión en la aplicación de ejemplo de Time Series Insights y su origen, y explorarlos.
> * Usar las API de la biblioteca cliente JavaScript de Time Series Insights.
> * Usar JavaScript para crear y rellenar controles de gráfico con los datos de Time Series Insights.

La aplicación Time Series Insights de ejemplo usa un conjunto de datos de demostración. Para aprender a crear un entorno de Time Series Insights y un conjunto de datos propios, lea el siguiente artículo:

> [!div class="nextstepaction"]
> [Tutorial: Creación de un entorno de Azure Time Series Insights](tutorial-create-populate-tsi-environment.md)

O bien, vea los archivos de origen de la aplicación de ejemplo de Time Series Insights:

> [!div class="nextstepaction"]
> [Repositorio de la aplicación de ejemplo de Time Series Insights](https://github.com/Microsoft/tsiclient/tree/tutorial/pages/tutorial)

Lea la documentación de referencia sobre la API del cliente de Time Series Insights:

> [!div class="nextstepaction"]
> [Documentación de referencia sobre la API de Time Series Insights](https://github.com/microsoft/tsiclient/blob/master/docs/API.md)
