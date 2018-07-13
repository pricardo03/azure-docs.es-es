---
title: Aplicación web de una sola página de Bing Web Search | Microsoft Docs
description: Muestra cómo utilizar Bing Web Search API en una aplicación web de una sola página.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-jerkin
ms.openlocfilehash: f22e38a1d6ee4042684b9822b58669bed6fe29a0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382159"
---
# <a name="tutorial-single-page-web-app"></a>Tutorial: Aplicación web de página única

Bing Web Search API permite buscar en la Web y obtener resultados de varios tipos apropiados para una consulta de búsqueda. En este tutorial, creamos una aplicación web de una sola página que usa Bing Web Search API para mostrar los resultados de la búsqueda directamente en la página. La aplicación incluye componentes HTML, CSS y JavaScript.

<!-- Remove until this can be replaced with a sanitized version.
![[Single-page Bing Web Search app]](media/cognitive-services-bing-web-api/web-search-spa-demo.png)
-->

> [!NOTE]
> Los encabezados HTTP y JSON de la parte inferior de la página revelan la respuesta JSON y la información de la solicitud HTTP al hacer clic en ellos. Estos detalles resultan útiles al explorar el servicio.

En la aplicación del tutorial se muestra cómo:

> [!div class="checklist"]
> * Realizar una llamada a Bing Web Search API en JavaScript
> * Pasar opciones de búsqueda a Bing Web Search API
> * Mostrar resultados de búsqueda en vídeos, imágenes, noticias y Web
> * Navegar por las páginas de resultados de la búsqueda
> * Controlar el identificador de cliente de Bing y la clave de suscripción de la API
> * Controlar los errores que se puedan producir

La página del tutorial es completamente independiente: no utiliza marcos, hojas de estilo ni archivos de imagen externos. Usa solo características del lenguaje JavaScript ampliamente compatibles y funciona con las versiones actuales de los principales exploradores web.

En este tutorial, solo se describen determinadas partes del código fuente. El código fuente completo está disponible [en otra página](tutorial-bing-web-search-single-page-app-source.md). Copie y pegue este código en un editor de texto y guárdelo como `bing.html`.

## <a name="app-components"></a>Componentes de la aplicación

Al igual que cualquier aplicación web de una sola página, la aplicación del tutorial incluye tres partes:

> [!div class="checklist"]
> * HTML: define la estructura y el contenido de la página
> * CSS: define el aspecto de la página
> * JavaScript: define el comportamiento de la página

Este tutorial no profundiza en la mayor parte de HTML o CSS, ya que es sencillo.

El código HTML contiene el formulario de búsqueda en que el usuario escribe una consulta y elige las opciones de búsqueda. El formulario está conectado al código JavaScript que realmente realiza la búsqueda mediante el atributo `onsubmit` de la etiqueta `<form>`:

```html
<form name="bing" onsubmit="return newBingWebSearch(this)">
```

El controlador `onsubmit` devuelve `false`, lo que impide que el formulario que se envíe a un servidor. El código JavaScript hace el trabajo de recopilar la información necesaria del formulario y realizar la búsqueda.

El HTML también contiene las divisiones (etiquetas `<div>` HTML) en que aparecen los resultados de la búsqueda.

## <a name="managing-subscription-key"></a>Administración de la clave de suscripción

Para evitar tener que incluir la clave de suscripción de Bing Search API en el código, usamos el almacenamiento persistente del navegador para almacenar la clave. Si no se almacena ninguna clave, solicitamos la clave del usuario y la guardamos para usarla en otro momento. Si, más tarde, la API rechaza la clave, se invalidará la clave almacenada y se le volverá a pedir al usuario.

Definimos las funciones `storeValue` y `retrieveValue` que usan el objeto `localStorage` (si el explorador lo admite) o una cookie. Nuestra función `getSubscriptionKey()` usa estas funciones para almacenar y recuperar la clave del usuario.

```javascript
// cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/search";

// ... omitted definitions of storeValue() and retrieveValue()

// get stored API subscription key, or prompt if it's not found
function getSubscriptionKey() {
    var key = retrieveValue(API_KEY_COOKIE);
    while (key.length !== 32) {
        key = prompt("Enter Bing Search API subscription key:", "").trim();
    }
    // always set the cookie in order to update the expiration date
    storeValue(API_KEY_COOKIE, key);
    return key;
}
```

La etiqueta `form` HTML `onsubmit` llama a la función `bingWebSearch` para devolver los resultados de la búsqueda. `bingWebSearch` utiliza `getSubscriptionKey` para autenticar cada consulta. Como se muestra en la definición anterior, si no se ha especificado la clave `getSubscriptionKey` se la pide al usuario. Luego la clave se almacena para que la siga usando la aplicación.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="selecting-search-options"></a>Selección de las opciones de búsqueda

![[Formulario de Bing Web Search]](media/cognitive-services-bing-web-api/web-search-spa-form.png)

El formulario HTML incluye elementos con los nombres siguientes:

| | |
|-|-|
| `where` | Un menú desplegable para seleccionar el mercado (ubicación e idioma) que se usa en la búsqueda. |
| `query` | El campo de texto en el que se especifican los términos de búsqueda. |
| `what` | Casillas para la promoción de determinados tipos de resultados. Por ejemplo, la promoción de imágenes aumenta el orden de clasificación de las imágenes. |
| `when` | Menú desplegable para, opcionalmente, limitar la búsqueda al día, semana o mes más recientes. |
| `safe` | Una casilla que indica si se usa la característica Búsqueda segura de Bing para filtrar los resultados "para adultos". |
| `count` | Campo oculto. Número de resultados de la búsqueda que se devolverán en cada solicitud. Cambie este valor para mostrar más o menos resultados por página. |
| `offset` | Campo oculto. El desplazamiento del primer resultado de la búsqueda de la solicitud; se usa para la paginación. Se restablece a `0` en cada nueva solicitud. |

> [!NOTE]
> Bing Web Search ofrece muchos más parámetros de consulta. Aquí solo vamos a usar algunos.

La función de JavaScript `bingSearchOptions()` convierte estos campos al formato que requiere Bing Search API.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var what = [];
    for (var i = 0; i < form.what.length; i++) 
        if (form.what[i].checked) what.push(form.what[i].value);
    if (what.length) {
        options.push("promote=" + what.join(","));
        options.push("answerCount=9");
    }
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    options.push("textDecorations=true");
    options.push("textFormat=HTML");
    return options.join("&");
}
```

Por ejemplo, el parámetro `SafeSearch` de una llamada API real puede ser `strict`, `moderate` o `off`, pero `moderate` es el predeterminado. Sin embargo, nuestro formulario utiliza una casilla que solo tiene dos estados. El código JavaScript convierte este valor en `strict` u `off` (`moderate` no se usa).

Si cualquiera de las casillas **Promote** está marcada, también agregamos un parámetro `answerCount`a la consulta. `answerCount` es obligatorio cuando se usa el parámetro `promote`. Simplemente lo establecemos en `9` (el número de tipos de resultado que admite Bing Web Search API) para asegurarnos de que obtenemos el número máximo posible de tipos de resultado.

> [!NOTE]
> La promoción de un tipo de resultado no *garantiza* que los resultados de las búsquedas incluyan ese tipo de resultado. En su lugar, la promoción aumenta la clasificación de esos tipos de resultados con respecto a su clasificación habitual. Para limitar las búsquedas a determinados tipos de resultados, use el parámetro de consulta `responseFilter` o llamar a un punto de conexión más concreto como Bing Image Search o Bing News Search.

También enviamos los parámetros de consulta `textDecoration` y `textFormat` para que el término de búsqueda aparezca en negrita en los resultados de las búsquedas. Estos valores están codificados en el script.

## <a name="performing-the-request"></a>Realización de la solicitud

Dada la consulta, la cadena de opciones y la clave de API, la función `BingWebSearch` utiliza un objeto `XMLHttpRequest` para enviar la solicitud al punto de conexión de Bing Web Search.

```javascript
// perform a search given query, options string, and API key
function bingWebSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("pole", "mainline", "sidebar", "_json", "_http", "paging1", "paging2", "error");

    var request = new XMLHttpRequest();
    var queryurl = BING_ENDPOINT + "?q=" + encodeURIComponent(query) + "&" + options;

    // open the request
    try {
        request.open("GET", queryurl);
    } 
    catch (e) {
        renderErrorMessage("Bad request (invalid URL)\n" + queryurl);
        return false;
    }

    // add request headers
    request.setRequestHeader("Ocp-Apim-Subscription-Key", key);
    request.setRequestHeader("Accept", "application/json");
    var clientid = retrieveValue(CLIENT_ID_COOKIE);
    if (clientid) request.setRequestHeader("X-MSEdge-ClientID", clientid);
    
    // event handler for successful response
    request.addEventListener("load", handleBingResponse);
    
    // event handler for erorrs
    request.addEventListener("error", function() {
        renderErrorMessage("Error completing request");
    });

    // event handler for aborted request
    request.addEventListener("abort", function() {
        renderErrorMessage("Request aborted");
    });

    // send the request
    request.send();
    return false;
}
```

Tras la correcta finalización de la solicitud HTTP, JavaScript llama a nuestro controlador de eventos `load`, la función `handleBingResponse()`, para controlar una solicitud GET HTTP correcta a la API. 

```javascript
// handle Bing search request results
function handleBingResponse() {
    hideDivs("noresults");

    var json = this.responseText.trim();
    var jsobj = {};

    // try to parse JSON results
    try {
        if (json.length) jsobj = JSON.parse(json);
    } catch(e) {
        renderErrorMessage("Invalid JSON response");
        return;
    }

    // show raw JSON and HTTP request
    showDiv("json", preFormat(JSON.stringify(jsobj, null, 2)));
    showDiv("http", preFormat("GET " + this.responseURL + "\n\nStatus: " + this.status + " " + 
        this.statusText + "\n" + this.getAllResponseHeaders()));

    // if HTTP response is 200 OK, try to render search results
    if (this.status === 200) {
        var clientid = this.getResponseHeader("X-MSEdge-ClientID");
        if (clientid) retrieveValue(CLIENT_ID_COOKIE, clientid);
        if (json.length) {
            if (jsobj._type === "SearchResponse" && "rankingResponse" in jsobj) {
                renderSearchResults(jsobj);
            } else {
                renderErrorMessage("No search results in JSON response");
            }
        } else {
            renderErrorMessage("Empty response (are you sending too many requests too quickly?)");
        }
    }

    // Any other HTTP response is an error
    else {
        // 401 is unauthorized; force re-prompt for API key for next request
        if (this.status === 401) invalidateSubscriptionKey();

        // some error responses don't have a top-level errors object, so gin one up
        var errors = jsobj.errors || [jsobj];
        var errmsg = [];

        // display HTTP status code
        errmsg.push("HTTP Status " + this.status + " " + this.statusText + "\n");

        // add all fields from all error responses
        for (var i = 0; i < errors.length; i++) {
            if (i) errmsg.push("\n");
            for (var k in errors[i]) errmsg.push(k + ": " + errors[i][k]);
        }

        // also display Bing Trace ID if it isn't blocked by CORS
        var traceid = this.getResponseHeader("BingAPIs-TraceId");
        if (traceid) errmsg.push("\nTrace ID " + traceid);

        // and display the error message
        renderErrorMessage(errmsg.join("\n"));
    }
}
```

> [!IMPORTANT]
> Una solicitud HTTP correcta *no* indica necesariamente que la búsqueda se haya realizado correctamente. Si se produce un error en la operación de búsqueda, Bing Web Search API devuelve un código de estado HTTP distinto de 200 e incluye información de error en la respuesta JSON. Además, si la solicitud tenía limitación de frecuencia, la API devuelve una respuesta vacía.

Gran parte del código de las dos funciones anteriores está dedicado al control de errores. Pueden producirse errores en las siguientes fases:

|Fase|Errores posibles|Controlado por|
|-|-|-|
|Crear un objeto de solicitud de JavaScript|Dirección URL no válida|Bloque `try`/`catch`|
|Hacer la solicitud|Errores de red, conexiones anuladas|Controladores de eventos `error` y `abort`|
|Realizar la búsqueda|Solicitud no válida, JSON no válido, límites de frecuencia|Pruebas en el controlador de eventos `load`|

Los errores se controlan mediante una llamada a `renderErrorMessage()` con los detalles que se conocen del error. Si la respuesta pasa todas las pruebas de error, llamamos a `renderSearchResults()` para mostrar los resultados de la búsqueda en la página.

## <a name="displaying-search-results"></a>Presentación de los resultados de la búsqueda

Bing Web Search API [le solicita que muestre los resultados en un orden específico](useanddisplayrequirements.md). Dado que la API puede devolver distintos tipos de respuestas, no es suficiente iterar a través de la colección `WebPages` de nivel superior en la respuesta JSON y mostrar esos resultados. (si desea un solo tipo de resultados, utilice el parámetro de consulta `responseFilter` o el otro punto de conexión de Bing Search).

En su lugar, usamos `rankingResponse` en los resultados de la búsqueda para ordenar los resultados que se mostrarán. Este objeto hace referencia a elementos de las colecciones `WebPages` `News`, `Images` o `Videos`, o de otras colecciones de respuestas de nivel superior de la respuesta JSON.

`rankingResponse` puede contener hasta tres colecciones designadas de resultados de búsqueda; esto es, `pole`, `mainline` y `sidebar`. 

`pole`, si está presente, es el resultado de búsqueda más apropiado y debe mostrarse de manera destacada. `mainline` se refiere a la mayor parte de los resultados de la búsqueda. Los resultados principales se deben mostrar inmediatamente después de `pole` (o antes, si `pole` no está presente). 

Por último. `sidebar` hace referencia a los resultados de búsqueda auxiliares. A menudo, estos resultados son búsquedas o imágenes relacionadas. Si es posible, estos resultados se deben mostrar en una barra lateral real. Si los límites de la pantalla hacen que una barra lateral sea poco práctica (por ejemplo, en un dispositivo móvil), deben aparecer después de los resultados de `mainline`.

Todos y cada uno de los elementos de una colección `rankingResponse` hacen referencia a los resultados reales de la búsqueda, y en dos formas diferentes pero equivalentes.

| | |
|-|-|
|`id`|`id` parece una dirección URL, pero no debe usarse para los vínculos. El tipo `id` de un resultado de clasificación coincide con el `id` de un elemento del resultado de búsqueda en una colección de respuestas, *o*  una colección de respuestas completa (como, por ejemplo, `Images`).
|`answerType`, `resultIndex`|`answerType` se refiere a la colección de respuestas de nivel superior que contiene el resultado (por ejemplo, `WebPages`). `resultIndex` se refiere al índice del resultado dentro de la colección. Si se omite `resultIndex`, el resultado de la clasificación se refiere a toda la colección.

> [!NOTE]
> Para más información acerca de esta parte de la respuesta de búsqueda, consulte [Clasificación de resultados](rank-results.md).

Puede usar el método que sea más conveniente para su aplicación para ubicar el elemento resultado de la búsqueda al que se hace referencia. En el código del tutorial, usamos `answerType` y `resultIndex` para ubicar cada resultado de búsqueda.

Por último, ha llegado el momento de echar un vistazo a nuestra función `renderSearchResults()`. Esta función se itera en las tres colecciones `rankingResponse` que representan las tres secciones de los resultados de la búsqueda. En cada sección, llamamos a `renderResultsItems()` para que represente los resultados de dicha sección.

```javascript
// render the search results given the parsed JSON response
function renderSearchResults(results) {

    // if spelling was corrected, update search field
    if (results.queryContext.alteredQuery) 
        document.forms.bing.query.value = results.queryContext.alteredQuery;

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);
    
    // for each possible section, render the resuts from that section
    for (section in {pole: 0, mainline: 0, sidebar: 0}) {
        if (results.rankingResponse[section])
            showDiv(section, renderResultsItems(section, results));
    }
}
```

`renderResultsItems()` a su vez realiza una iteración por los elementos de cada sección `rankingResponse`, asigna cada resultado de la clasificación a un resultado de la búsqueda mediante los campos `answerType` y `resultIndex`, y llama a la función de representación adecuada para generar el HTML del resultado. 

Si `resultIndex` no se especifica para un elemento determinado de la clasificación, `renderResultsItems()` realiza una iteración por todos los resultados de ese tipo y llama a la función de representación en cada elemento. 

En cualquier caso, el HTML resultante se inserta en el elemento `<div>` correspondiente de la página.

```javascript
// render search results from rankingResponse object in specified order
function renderResultsItems(section, results) {

    var items = results.rankingResponse[section].items;
    var html = [];
    for (var i = 0; i < items.length; i++) {
        var item = items[i];
        // collection name has lowercase first letter while answerType has uppercase
        // e.g. `WebPages` rankingResult type is in the `webPages` top-level collection
        var type = item.answerType[0].toLowerCase() + item.answerType.slice(1);
        // must have results of the given type AND a renderer for it
        if (type in results && type in searchItemRenderers) {
            var render = searchItemRenderers[type];
            // this ranking item refers to ONE result of the specified type
            if ("resultIndex" in item) {
                html.push(render(results[type].value[item.resultIndex], section));
            // this ranking item refers to ALL results of the specified type
            } else {
                var len = results[type].value.length;
                for (var j = 0; j < len; j++) {
                    html.push(render(results[type].value[j], section, j, len));
                }
            }
        }
    }
    return html.join("\n\n");
}
```

## <a name="rendering-result-items"></a>Representación de los elementos del resultado

Nuestro código de JavaScript es un objeto (`searchItemRenderers`) que contiene *representadores:* funciones que generan código HTML para cada tipo de resultado de búsqueda.

```javascript
// render functions for various types of search results
searchItemRenderers = { 
    webPages: function(item) { ... },
    news: function(item) { ... },
    images: function(item, section, index, count) { ... },
    videos: function(item, section, index, count) { ... },
    relatedSearches: function(item, section, index, count) { ... }
}
```

> [!NOTE]
> Nuestra aplicación del tutorial tiene representadores para páginas web, elementos de noticias, imágenes, vídeos y búsquedas relacionadas. Su propia aplicación necesita representadores para los tipos de resultados que puede recibir, entre los que se pueden incluir cálculos, sugerencias de ortografía, entidades, zonas horarias y definiciones.

Algunas de nuestras funciones de representación aceptan solo el parámetro `item`: un objeto JavaScript que representa un resultado de búsqueda individual. Otras aceptan parámetros adicionales, que se pueden usar para representar elementos de forma distinta en diferentes contextos (los representadores que no usen esta información no necesitan aceptar estos parámetros).

Los argumentos del contexto son:

| | |
|-|-|
|`section`|La sección de resultados (`pole`, `mainline`, o `sidebar`) en que aparece el elemento.
|`index`<br>`count`|Disponible cuando el elemento `rankingResponse` especifica que se deben mostrar todos los resultados de una colección determinada; en caso contrario, `undefined`. Estos parámetros reciben el índice que tiene el elemento en su colección y el número total de elementos de dicha colección. Puede usar esta información para numerar los resultados, para generar otro HTML para el primer o último resultados, etc.|

En nuestra aplicación del tutorial, los representadores `images` y `relatedSearches` usan los argumentos del contexto para personalizar el HTML que generan. Observemos más de cerca el representador `images`:

```javascript
searchItemRenderers = { 
    // render image result using thumbnail
    images: function(item, section, index, count) {
        var height = 60;
        var width = Math.round(height * item.thumbnail.width / item.thumbnail.height);
        var html = [];
        if (section === "sidebar") {
            if (index) html.push("<br>");
        } else {
            if (!index) html.push("<p class='images'>");
        }
        html.push("<a href='" + item.hostPageUrl + "'>");
        var title = escape(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + 
            "' height=" + height + " width=" + width + " title='" + title + "' alt='" + title + "'>");
        html.push("</a>");
        return html.join("");
    }, // other renderers omitted
}
```

Nuestra función del representador de imágenes:

> [!div class="checklist"]
> * Calcula el tamaño de la miniatura de la imagen (el ancho varía, mientras que la altura se fija en 60 píxeles).
> * Inserta el HTML que precede al resultado de la imagen según el contexto.
> * Compila la etiqueta `<a>` HTML que vincula a la página que contiene la imagen.
> * Crea la etiqueta HTML `<img>` para mostrar la miniatura de la imagen. 

El representador de imágenes utiliza las variables `section` y `index` para mostrar los resultados de forma diferente en función del lugar en que aparezcan. Se inserta un salto de línea (etiqueta `<br>`) entre los resultados de la imagen de la barra lateral, con el fin de que la barra lateral muestre una columna de imágenes. En otras secciones, el primer resultado de la imagen `(index === 0)` va precedido por la etiqueta `<p>`. En caso contrario, las miniaturas se pegan unas a otras y se ajustan tanto como sea necesario en la ventana del navegador.

El tamaño de las miniaturas se usa tanto en la etiqueta `<img>` como en los campos `h` y `w` en la dirección URL de la miniatura. Luego, el [servicio de miniaturas de Bing](resize-and-crop-thumbnails.md) ofrece una miniatura de exactamente ese tamaño. Los atributos `title` y `alt` (una descripción textual de la imagen) se construyen a partir de nombre de la imagen y el nombre de host de la dirección URL.

En los resultados de la búsqueda principal las imágenes aparecen como se muestran aquí.

![[Resultados de imágenes de Bing]](media/cognitive-services-bing-web-api/web-search-spa-images.png)

## <a name="persisting-client-id"></a>Identificador de cliente persistente

Las respuestas de Bing Search API pueden incluir un encabezado `X-MSEdge-ClientID` que debe devolverse a la API con las solicitudes sucesivas. Si se utilizan varias instancias de Bing Search API, se debe usar el mismo identificador de cliente con todas ellas, si es posible.

Especificar el encabezado `X-MSEdge-ClientID` permite a las API de Bing asociar todas las búsquedas de un usuario, lo que tiene dos ventajas importantes.

En primer lugar, permite al motor de búsqueda de Bing aplicar un contexto pasado a las búsquedas, con el fin de encontrar los resultados que más satisfagan al usuario. Si un usuario ha buscado previamente términos relacionados con la navegación, por ejemplo, una búsqueda posterior de "nudos" podría devolver información acerca de los nudos que se usan en la navegación de forma preferente.

En segundo lugar, Bing puede seleccionar aleatoriamente a ciertos usuarios para que prueben las nuevas características antes de que estén disponibles para el público. Especificar el mismo identificador de cliente en todas las solicitudes garantiza que los usuarios elegidos para ver una característica la ven siempre. Sin el identificador de cliente, el usuario puede ver que una característica aparece y desaparece de forma aparentemente aleatoria en los resultados de su búsqueda.

Las directivas de seguridad del explorador (CORS) pueden impedir que el encabezado `X-MSEdge-ClientID` esté disponible para JavaScript. Esta limitación tiene lugar cuando la respuesta a la búsqueda tiene un origen distinto del de la página que la solicitó. En un entorno de producción, para cumplir esta directiva debe hospedar un script de lado servidor que realice la llamada a la API en el mismo dominio que la página web. Como el script tiene el mismo origen que la página web, el encabezado `X-MSEdge-ClientID` está disponible para JavaScript.

> [!NOTE]
> En una aplicación web de producción, debe realizar la solicitud del lado servidor de todos modos. En caso contrario, es preciso incluir la clave de Bing Search API en la página web, donde está disponible para cualquiera que vea el origen. Se le facturará todo el uso bajo su clave de suscripción a API, incluso las solicitudes que realicen partes no autorizadas, por lo que es importante no exponer su clave.

Para fines de desarrollo, puede realizar la solicitud de Bing Web Search API a través de un proxy CORS. La respuesta de un proxy de este tipo tiene un encabezado `Access-Control-Expose-Headers` que agrega los encabezados de respuesta a listas blancas y hace que estén disponibles para JavaScript.

Es fácil instalar un proxy CORS que permita a nuestra aplicación del tutorial acceder al encabezado del identificador de cliente. En primer lugar, si aún no lo tiene, [instale Node.js](https://nodejs.org/en/download/). Escriba el comando siguiente en una ventana de comandos:

    npm install -g cors-proxy-server

A continuación, cambie el punto de conexión de Bing Web Search del archivo HTML a:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Por último, inicie el proxy CORS con el siguiente comando:

    cors-proxy-server

Deje abierta la ventana de comandos mientras usa la aplicación del tutorial, ya que si la cierra, se detendrá el proxy. En la sección de encabezados HTTP expandibles situada bajo los resultados de la búsqueda, puede ver el encabezado `X-MSEdge-ClientID` (entre otras cosas) y comprobar que es el mismo en todas las solicitudes.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Tutorial de la aplicación móvil Visual Search](computer-vision-web-search-tutorial.md)

> [!div class="nextstepaction"]
> [Referencia de Bing Web Search API](//docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
