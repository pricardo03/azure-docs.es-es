---
title: Aplicación web de una sola página de Bing Image Search | Microsoft Docs
description: Muestra cómo usar Bing Image Search API en una aplicación web de una sola página.
services: cognitive-services
author: v-jerkin
manager: ehansen
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 10/04/2017
ms.author: v-jerkin
ms.openlocfilehash: d0e1dc24513c8fc3a405cf1c18f531a0c58fad13
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382158"
---
# <a name="tutorial-single-page-web-app"></a>Tutorial: aplicación web de una sola página

Bing Image Search API permite buscar en la Web y obtener resultados de imagen correspondientes a la consulta de búsqueda. En este tutorial, creamos una aplicación web de una sola página que usa Bing Image Search API para mostrar los resultados de la búsqueda directamente en la página. La aplicación incluye componentes HTML, CSS y JavaScript.

<!-- Remove until we can sanitize images
![[Single-page Bing Image Search app]](media/cognitive-services-bing-images-api/image-search-spa-demo.png)
-->

> [!NOTE]
> Los encabezados HTTP y JSON de la parte inferior de la página revelan la respuesta JSON y la información de la solicitud HTTP al hacer clic en ellos. Estos detalles resultan útiles al explorar el servicio.

En la aplicación del tutorial se muestra cómo:

> [!div class="checklist"]
> * Realizar una llamada a Bing Image Search API en JavaScript
> * Pasar opciones de búsqueda a Bing Image Search API
> * Mostrar los resultados de la búsqueda
> * Navegar por las páginas de resultados de la búsqueda
> * Administrar el id. de cliente de Bing y la clave de suscripción de la API
> * Controlar los errores que se puedan producir

La página del tutorial es completamente independiente: no utiliza marcos, hojas de estilo ni archivos de imagen externos. Usa solo características del lenguaje JavaScript totalmente compatibles y funciona con las versiones actuales de los principales exploradores web.

En este tutorial, solo se describen determinadas partes del código fuente. El código fuente completo está disponible [en otra página](tutorial-bing-image-search-single-page-app-source.md). Copie y pegue este código en un editor de texto y guárdelo como `bing.html`.

> [!NOTE]
> Este tutorial es bastante similar al [tutorial de la aplicación Bing Web Search de una sola página](../Bing-Web-Search/tutorial-bing-web-search-single-page-app.md), pero solo se centra en los resultados de búsqueda de imágenes.

## <a name="app-components"></a>Componentes de la aplicación

Al igual que cualquier aplicación web de una sola página, la aplicación de tutorial incluye tres partes:

> [!div class="checklist"]
> * HTML: define la estructura y el contenido de la página
> * CSS: define el aspecto de la página
> * JavaScript: define el comportamiento de la página

Este tutorial no profundiza en la mayor parte del código HTML o CSS, que es sencillo.

El código HTML contiene el formulario de búsqueda en que el usuario escribe una consulta y elige las opciones de búsqueda. El formulario está conectado al código JavaScript que realmente realiza la búsqueda mediante el atributo `onsubmit` de la etiqueta `<form>`:

```html
<form name="bing" onsubmit="return newBingImageSearch(this)">
```

El controlador `onsubmit` devuelve `false`, que impide que el formulario que se envíe a un servidor. El código JavaScript hace el trabajo de recopilar la información necesaria del formulario y realizar la búsqueda.

El código HTML también contiene las divisiones (etiquetas `<div>` HTML) donde aparecen los resultados de búsqueda.

## <a name="managing-subscription-key"></a>Administrar la clave de suscripción

Para evitar tener que incluir la clave de suscripción de Bing Search API en el código, usamos el almacenamiento persistente del navegador para almacenar la clave. Si no se almacena ninguna clave, solicitamos la clave del usuario y la guardamos para usarla en otro momento. Si, más tarde, la API rechaza la clave, se invalidará la clave almacenada y se le volverá a preguntar al usuario.

Definimos funciones `storeValue` y `retrieveValue` que usan el objeto `localStorage` (si el explorador lo admite) o una cookie. Nuestra función `getSubscriptionKey()` usa estas funciones para almacenar y recuperar la clave del usuario.

```javascript
// cookie names for data we store
API_KEY_COOKIE   = "bing-search-api-key";
CLIENT_ID_COOKIE = "bing-search-client-id";

BING_ENDPOINT = "https://api.cognitive.microsoft.com/bing/v7.0/images/search";

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

La etiqueta `<form>` HTML `onsubmit` llama a la función `bingWebSearch` para devolver los resultados de la búsqueda. `bingWebSearch` utiliza `getSubscriptionKey` para autenticar cada consulta. Como se muestra en la definición anterior, `getSubscriptionKey` pide al usuario la clave si no se ha especificado. A continuación, la clave se almacena para que la siga usando la aplicación.

```html
<form name="bing" onsubmit="this.offset.value = 0; return bingWebSearch(this.query.value, 
    bingSearchOptions(this), getSubscriptionKey())">
```

## <a name="selecting-search-options"></a>Seleccionar opciones de búsqueda

![[Formulario de Bing Image Search]](media/cognitive-services-bing-images-api/image-search-spa-form.png)

El formulario HTML incluye los siguientes controles:

| | |
|-|-|
|`where`|Un menú desplegable para seleccionar el mercado (ubicación e idioma) utilizado para la búsqueda.|
|`query`|El campo de texto en que se especificarán los términos de búsqueda.|
|`aspect`|Botones de radio para elegir las proporciones de las imágenes que se encuentren: cuadradas, horizontales o verticales.|
|`color`|Selecciona color, blanco y negro, o color predominante.
|`when`|Menú desplegable para, opcionalmente, limitar la búsqueda al día, la semana o el mes más reciente.|
|`safe`|Una casilla que indica si se debe usar la característica Búsqueda segura de Bing para filtrar los resultados "para adultos".|
|`count`|Campo oculto. Número de resultados de búsqueda que se devolverán en cada solicitud. Cambie este valor para mostrar más o menos resultados por página.|
|`offset`|Campo oculto. El desplazamiento del primer resultado de búsqueda de la solicitud. Se usa para la paginación. Se restablece a `0` para cada nueva solicitud.|
|`nextoffset`|Campo oculto. Al recibir un resultado de búsqueda, este campo se define con el valor de `nextOffset` en la respuesta. El uso de este campo evita resultados superpuestos en páginas sucesivas.|
|`stack`|Campo oculto. Una lista codificada en JSON de los desplazamientos de las páginas anteriores de los resultados de búsqueda para volver a páginas anteriores.|

> [!NOTE]
> Bing Image Search ofrece muchos más parámetros de consulta. Aquí solo usamos algunos.

Nuestra función de JavaScript `bingSearchOptions()` convierte estos campos en una cadena de consulta parcial en el formato que requiere Bing Search API.

```javascript
// build query options from the HTML form
function bingSearchOptions(form) {

    var options = [];
    options.push("mkt=" + form.where.value);
    options.push("SafeSearch=" + (form.safe.checked ? "strict" : "off"));
    if (form.when.value.length) options.push("freshness=" + form.when.value);
    var aspect = "all";
    for (var i = 0; i < form.aspect.length; i++) {
        if (form.aspect[i].checked) {
            aspect = form.aspect[i].value;
            break;
        }
    }
    options.push("aspect=" + aspect);
    if (form.color.value) options.push("color=" + form.color.value);
    options.push("count=" + form.count.value);
    options.push("offset=" + form.offset.value);
    return options.join("&");
}
```

Por ejemplo, la característica Búsqueda segura puede tener el valor `strict`, `moderate` u `off`. El valor predeterminado es `moderate`. Sin embargo, nuestro formulario usa una casilla que solo tiene dos estados. El código JavaScript convierte este valor en `strict` u `off` (no usamos `moderate`).

## <a name="performing-the-request"></a>Realizar la solicitud

Dada la consulta, la cadena de opciones y la clave de API, la función `BingImageSearch` utiliza un objeto `XMLHttpRequest` para enviar la solicitud al punto de conexión de Bing Image Search.

```javascript
// perform a search given query, options string, and API key
function bingImageSearch(query, options, key) {

    // scroll to top of window
    window.scrollTo(0, 0);
    if (!query.trim().length) return false;     // empty query, do nothing

    showDiv("noresults", "Working. Please wait.");
    hideDivs("results", "related", "_json", "_http", "paging1", "paging2", "error");

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

Tras la correcta finalización de la solicitud HTTP, JavaScript llama nuestro controlador de eventos `load`, la función `handleBingResponse()`, para controlar una solicitud HTTP GET a la API correcta. 

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
            if (jsobj._type === "Images") {
                if (jsobj.nextOffset) document.forms.bing.nextoffset.value = jsobj.nextOffset;
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
> Una solicitud HTTP correcta *no* indica necesariamente que la búsqueda se haya realizado correctamente. Si se produce un error en la operación de búsqueda, Bing Image Search API devuelve un código de estado HTTP distinto de 200 e incluye información de error en la respuesta JSON. Además, si la solicitud tenía limitación de frecuencia, la API devuelve una respuesta vacía.

Gran parte del código de las dos funciones anteriores está dedicada al control de errores. Pueden producirse errores en las siguientes fases:

|Fase|Errores posibles|Controlado por|
|-|-|-|
|Creación de objeto de solicitud JavaScript|Dirección URL no válida|Bloqueo `try`/`catch`|
|Hacer la solicitud|Errores de red, conexiones anuladas|Controladores de eventos `error` y `abort`|
|Realizar la búsqueda|Solicitud no válida, JSON no válido, límites de frecuencia|Pruebas en el controlador de eventos `load`|

Los errores se controlan mediante una llamada a `renderErrorMessage()` con los detalles que se conoce sobre el error. Si la respuesta pasa todas las pruebas de error, llamamos a `renderSearchResults()` para mostrar los resultados de búsqueda en la página.

## <a name="displaying-search-results"></a>Mostrar los resultados de búsqueda

La función principal para mostrar los resultados de búsqueda es `renderSearchResults()`. Esta función utiliza el código JSON que devuelve el servicio Bing Image Search y procesa las imágenes y las búsquedas relacionadas, si las hay.

```javascript
function renderSearchResults(results) {

    // add Prev / Next links with result count
    var pagingLinks = renderPagingLinks(results);
    showDiv("paging1", pagingLinks);
    showDiv("paging2", pagingLinks);
    
    showDiv("results", renderImageResults(results.value));
    if (results.relatedSearches)
        showDiv("sidebar", renderRelatedItems(results.relatedSearches));
}
```

Se devuelven los resultados de búsqueda de imagen principal como el objeto `value` de nivel superior de la respuesta JSON. Los pasamos a nuestra función `renderImageResults()`, que los recorre en iteración y llama a otra función para representar cada elemento en HTML. El HTML resultante se devuelve a `renderSearchResults()`, donde se inserta en la división `results` de la página.

```javascript
function renderImageResults(items) {
    var len = items.length;
    var html = [];
    if (!len) {
        showDiv("noresults", "No results.");
        hideDivs("paging1", "paging2");
        return "";
    }
    for (var i = 0; i < len; i++) {
        html.push(searchItemRenderers.images(items[i], i, len));
    }
    return html.join("\n\n");
}
```

Bing Image Search API devuelve hasta cuatro tipos distintos de resultados relacionados, cada uno en su propio objeto de nivel superior. Son las siguientes:

|||
|-|-|
|`pivotSuggestions`|Consultas que reemplazan una palabra dinámica de la búsqueda original por otra. Por ejemplo, si busca "flores rojas", una palabra dinámica podría ser "rojas", y una sugerencia dinámica podría ser "flores amarillas".|
|`queryExpansions`|Consultas que limitan la búsqueda original al agregar más términos. Por ejemplo, si busca "Microsoft Surface", una expansión de consulta podría ser "Microsoft Surface Pro".|
|`relatedSearches`|Consultas que también han realizado otros usuarios que escribieron la búsqueda original. Por ejemplo, su buscó "Monte Rainier", una búsqueda relacionada podría ser "Monte Santa Elena".|
|`similarTerms`|Consultas que son similares en significado a la búsqueda original. Por ejemplo, si busca "gatos", un término similar podría ser "monos".|

Como ya hemos visto en `renderSearchResults()`, solo representamos sugerencias de `relatedItems` y colocamos los vínculos resultantes en la barra lateral de la página.

## <a name="rendering-result-items"></a>Representar elementos de resultados

En nuestro código JavaScript es un objeto, `searchItemRenderers`, que contiene *representadores:* funciones que generan código HTML para cada tipo de resultado de búsqueda.

```javascript
searchItemRenderers = { 
    images: function(item, index, count) { ... },
    relatedSearches: function(item) { ... }
}
```

Una función de representador puede aceptar los parámetros siguientes:

| | |
|-|-|
|`item`|El objeto de JavaScript que contiene las propiedades del elemento, como su dirección URL y su descripción.|
|`index`|El índice del elemento de resultado dentro de su colección.|
|`count`|El número de elementos de la colección del elemento de resultado de búsqueda.|

Los parámetros `index` y `count` pueden usarse para numerar los resultados, para generar un código HTML especial para el principio o el final de una colección, para insertar saltos de línea después de cierto número de elementos, etc. Si un representador no necesita esta funcionalidad, no es necesario aceptar estos dos parámetros.

Observemos más de cerca el representador `images`:

```javascript
    images: function (item, index, count) {
        var height = 120;
        var width = Math.max(Math.round(height * item.thumbnail.width / item.thumbnail.height), 120);
        var html = [];
        if (index === 0) html.push("<p class='images'>");
        var title = escape(item.name) + "\n" + getHost(item.hostPageDisplayUrl);
        html.push("<p class='images' style='max-width: " + width + "px'>");
        html.push("<img src='"+ item.thumbnailUrl + "&h=" + height + "&w=" + width + 
            "' height=" + height + " width=" + width + "'>");
        html.push("<br>");
        html.push("<nobr><a href='" + item.contentUrl + "'>Image</a> - ");
        html.push("<a href='" + item.hostPageUrl + "'>Page</a></nobr><br>");
        html.push(title.replace("\n", " (").replace(/([a-z0-9])\.([a-z0-9])/g, "$1.<wbr>$2") + ")</p>");
        return html.join("");
    }, // relatedSearches renderer omitted
```

Nuestra función del representador de imágenes:

> [!div class="checklist"]
> * Calcula el tamaño de miniatura de imagen (el ancho varía, con un mínimo de 120 píxeles, mientras que la altura se fija en 90 píxeles).
> * Crea la etiqueta HTML `<img>` para mostrar la miniatura de imagen. 
> * Compila etiquetas `<a>` HTML vinculadas con la imagen y la página que la contiene.
> * Crea la descripción que muestra información sobre la imagen y el sitio en que se encuentra.

Probamos la variable `index` con el fin de insertar una etiqueta `<p>` antes del primer resultado de imagen. En caso contrario, las miniaturas lindan entre sí y se encapsulan según sea necesario en la ventana del navegador.

El tamaño de las miniaturas se usa tanto en la etiqueta `<img>` como en los campos `h` y `w` en la dirección URL de miniatura. El [servicio de miniaturas de Bing](resize-and-crop-thumbnails.md), a continuación, proporciona una miniatura de exactamente ese tamaño.

## <a name="persisting-client-id"></a>Id. de cliente persistente

Las respuestas de Bing Search API pueden incluir un encabezado `X-MSEdge-ClientID` que debe devolverse a la API con las solicitudes sucesivas. Si se utilizan varias instancias de Bing Search API, se debe usar el mismo identificador de cliente con todas ellas, si es posible.

Proporcionar el encabezado `X-MSEdge-ClientID` permite a las API de Bing asociar todas las búsquedas de un usuario, lo que tiene dos ventajas importantes.

En primer lugar, permite al motor de búsqueda de Bing aplicar un contexto pasado a las búsquedas para buscar resultados que satisfagan mejor al usuario. Si un usuario ha buscado previamente términos relacionados con la navegación, por ejemplo, una búsqueda posterior de "nudos" podría devolver información acerca de los nudos que se usan en la navegación de forma preferente.

En segundo lugar, Bing puede seleccionar aleatoriamente usuarios para disfrutar de nuevas características antes de que estén disponibles públicamente. Proporcionar el mismo identificador de cliente con cada solicitud garantiza que los usuarios elegidos para ver una característica la vean siempre. Sin el identificador de cliente, el usuario puede ver una característica aparecer y desaparecer, de forma aparentemente aleatoria, en los resultados de búsqueda.

Las directivas de seguridad del explorador (CORS) pueden impedir que el encabezado `X-MSEdge-ClientID` esté disponible para JavaScript. Esta limitación tiene lugar cuando la respuesta a la búsqueda tiene un origen distinto al de la página que la solicitó. En un entorno de producción, debería abordar esta directiva mediante el hospedaje de un script de lado servidor que realice la llamada API en el mismo dominio que la página web. Puesto que el script tiene el mismo origen que la página web, el encabezado `X-MSEdge-ClientID` está disponible para JavaScript.

> [!NOTE]
> En una aplicación web de producción, debe realizar la solicitud del lado servidor de todos modos. En caso contrario, es necesario incluir la clave de Bing Search API en la página web, donde está disponible para cualquiera que vea el origen. Se le facturará por todo el uso bajo su clave de suscripción a API, incluso por las solicitudes que realicen partes no autorizadas, por lo que es importante no exponer su clave.

Para fines de desarrollo, puede realizar la solicitud de Bing Web Search API a través de un proxy CORS. La respuesta de un proxy de este tipo tiene un encabezado `Access-Control-Expose-Headers` que agrega los encabezados de respuesta a listas de permitidos y hace que estén disponibles para JavaScript.

Es fácil instalar un proxy CORS para permitir que nuestra aplicación de tutorial acceda al encabezado de identificador de cliente. En primer lugar, si aún no lo tiene, [instale Node.js](https://nodejs.org/en/download/). Escriba el comando siguiente en una ventana de comandos:

    npm install -g cors-proxy-server

A continuación, cambie el punto de conexión de Bing Web Search del archivo HTML a:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Por último, inicie el proxy CORS con el siguiente comando:

    cors-proxy-server

Deje abierta la ventana de comandos mientras usa la aplicación del tutorial. Al cerrar la ventana, se detiene el proxy. En la sección de encabezados HTTP expandibles situada bajo los resultados de búsqueda, ahora puede ver el encabezado `X-MSEdge-ClientID` (entre otras cosas) y comprobar que es el mismo para cada solicitud.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de Bing Image Search API](//docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)

