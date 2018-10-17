---
title: Búsqueda con Azure Maps | Microsoft Docs
description: Búsqueda de un punto cercano de interés mediante Azure Maps
author: dsk-2015
ms.author: dkshir
ms.date: 10/02/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 761674c5839f0513532355116db07604f9e9d9dc
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816827"
---
# <a name="search-nearby-points-of-interest-using-azure-maps"></a>Búsqueda de puntos cercanos de interés mediante Azure Maps

En este tutorial se muestra cómo configurar una cuenta con Azure Maps y luego usar las API de Maps para buscar un punto de interés. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una cuenta de Azure Maps
> * Recuperar la clave principal de la cuenta de Maps
> * Crear una nueva página web mediante la API de control de mapa
> * Usar el servicio de búsqueda de Maps para encontrar un punto cercano de interés

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com).

<a id="createaccount"></a>

## <a name="create-an-account-with-azure-maps"></a>Crear una cuenta con Azure Maps

Cree una nueva cuenta de Maps con los pasos siguientes:

1. En la esquina superior izquierda de [Azure Portal](https://portal.azure.com), haga clic en **Crear un recurso**.
2. En el campo *Buscar en el Marketplace*, escriba **Maps**.
3. En *Resultados*, seleccione **Maps**. Haga clic en el botón **Crear** que aparece debajo del mapa.
4. En la página **Create Maps Account** (Crear una cuenta de Azure Maps), escriba los siguientes valores:
    * El *nombre* de la nueva cuenta.
    * La *suscripción* que quiere usar para esta cuenta.
    * El nombre del *grupo de recursos* para esta cuenta. Puede elegir *Crear nuevo* o *Usar existente* para el grupo de recursos.
    * Seleccione *Ubicación del grupo de recursos*.
    * Lea la *licencia* y la *declaración de privacidad* y active la casilla para aceptar los términos.
    * Haga clic en el botón **Crear**.

    ![Creación de una cuenta de Maps en el portal](./media/tutorial-search-location/create-account.png)

<a id="getkey"></a>

## <a name="get-the-primary-key-for-your-account"></a>Obtención de la clave principal de una cuenta

Una vez que se haya creado correctamente la cuenta de Maps, recupere la clave que le permite consultar las API de Maps.

1. Abra su cuenta de Maps en el portal.
2. En la sección de configuración, seleccione **Claves**.
3. Copie la **clave principal** al Portapapeles. Guárdela localmente para usarla más adelante en este tutorial.

    ![Obtención de la clave principal en el portal](./media/tutorial-search-location/get-key.png)

<a id="createmap"></a>

## <a name="create-a-new-map"></a>Creación de un nuevo mapa

Map Control API es una práctica biblioteca cliente que le permite integrar fácilmente Maps en su aplicación web. Oculta la complejidad de las llamadas sin servicio de REST e impulsa la productividad con componentes que se pueden diseñar y personalizar. En los pasos siguientes se muestra cómo crear una página HTML estática insertada con Map Control API.

1. En el equipo local, cree un nuevo archivo y asígnele el nombre **MapSearch.html**.
2. Agregue los siguientes componentes HTML al archivo:

    ```HTML
    <!DOCTYPE html>
    <html lang="en">

    <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, user-scalable=no" />
        <title>Map Search</title>

        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.min.js?api-version=1"></script>

        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #map {
                width: 100%;
                height: 100%;
            }
        </style>
    </head>

    <body>
        <div id="map"></div>
        <script>
            // Embed Map Control JavaScript code here
        </script>
    </body>

    </html>
    ```
    Observe que el encabezado HTML incluye los archivos de recursos CSS y JavaScript hospedados por la biblioteca de Control de mapa de Azure. Tenga en cuenta el segmento *script* agregado al *cuerpo* del archivo HTML. Este segmento contendrá el código JavaScript insertado para acceder a las API de Azure Maps.

3. Agregue el siguiente código JavaScript al bloque *script* del archivo HTML. Reemplace la cadena **\<su clave de cuenta\>** por la clave principal que copió de la cuenta de Maps.

    ```JavaScript
    // Instantiate map to the div with id "map"
    var MapsAccountKey = "<your account key>";
    var map = new atlas.Map("map", {
        "subscription-key": MapsAccountKey
    });
    ```
    Este segmento inicializa Map Control API para la clave de cuenta de Azure Maps. **Atlas** es el espacio de nombres que contiene la API y los componentes visuales relacionados. **Atlas.Map** proporciona el control para un mapa web visual e interactivo.

4. Guarde los cambios en el archivo y abra la página HTML en un explorador. Este es el mapa más básico que puede crear con una llamada a **atlas.map** y una clave de cuenta.

   ![Visualización del mapa](./media/tutorial-search-location/basic-map.png)

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Adición de funcionalidades de búsqueda

En esta sección se muestra cómo usar Search API de Azure Maps para buscar un punto de interés en el mapa. Se trata de una API RESTful diseñada para los desarrolladores para buscar direcciones, puntos de interés y otra información geográfica. El servicio Search asigna información de latitud y longitud a una dirección especificada. El **módulo de servicio** que se explica a continuación, puede usarse para buscar una ubicación mediante Maps Search API.

### <a name="service-module"></a>Módulo de servicio

1. Agregue una nueva capa a su mapa para mostrar los resultados de búsqueda. Agregue el siguiente código Javascript al bloque de script, tras el código que inicializa el mapa.

    ```JavaScript
    // Initialize the pin layer for search results to the map
    var searchLayerName = "search-results";
    ```

2. Para crear una instancia del servicio de cliente, agregue el siguiente código Javascript al bloque de script, tras el código que inicializa el mapa.

    ```JavaScript
    var client = new atlas.service.Client(MapsAccountKey);
    ```

3. Todas las funciones en el mapa se deben cargar una vez cargado el mapa. Para asegurarse de ello, coloque todas las funciones de mapa dentro del bloque eventListener del mapa. Agregue las siguientes líneas de código para agregar un elemento [eventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener) al mapa a fin de asegurarse de que el mapa se carga totalmente antes de agregar funcionalidades.
    
    ```JavaScript
         map.addEventListener("load", function() {
         });
    ```

4. Agregue el siguiente bloque de script **dentro del elemento eventListener de carga de mapas** para compilar la consulta. Este fragmento de código usa Fuzzy Search Service, que es una API de búsqueda básica de Search Service. Fuzzy Search Service controla la mayoría de las entradas aproximadas como, por ejemplo, cualquier combinación de tokens de dirección y puntos de interés (POI). Por ejemplo, busca gasolineras cercanas dentro del radio especificado. Después, se analiza la respuesta en formato GeoJSON y se convierte en características de puntos, que se agregan al mapa como chinchetas. La última parte del script agrega los límites de la cámara del mapa mediante la propiedad [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) del mapa.

    ```JavaScript

            // Execute a POI search query then add pins to the map for each result once a response is received
            client.search.getSearchFuzzy("gasoline station", {
            lat: 47.6292,
            lon: -122.2337,
            radius: 100000
            }).then(response => {
       
            // Parse the response into GeoJSON 
            var geojsonResponse = new atlas.service.geojson.GeoJsonSearchResponse(response);

            // Create the point features that will be added to the map as pins
            var searchPins = geojsonResponse.getGeoJsonResults().features.map(poiResult => {
               var poiPosition = [poiResult.properties.position.lon, poiResult.properties.position.lat];
               return new atlas.data.Feature(new atlas.data.Point(poiPosition), {
                name: poiResult.properties.poi.name,
                address: poiResult.properties.address.freeformAddress,
                position: poiPosition[1] + ", " + poiPosition[0]
               });
            });

            // Add pins to the map for each POI
            map.addPins(searchPins, {
               name: searchLayerName,
               cluster: false, 
               icon: "pin-round-darkblue" 
            });

            // Set the camera bounds
            map.setCameraBounds({
               bounds: geojsonResponse.getGeoJsonResults().bbox,
               padding: 50
            );
        });
    ```
5. Guarde el archivo **MapSearch.html** y actualice el explorador. Ahora verá que el mapa se centra en Seattle y que aparecen marcadas con chinchetas azules las ubicaciones de las gasolineras del área.

   ![Visualización del mapa con los resultados de la búsqueda](./media/tutorial-search-location/pins-map.png)

6. Puede ver los datos sin procesar que está representando el mapa especificando la siguiente solicitud HTTPRequest en el explorador. Reemplace \<su clave de cuenta\> por la clave principal.

   ```http
   https://atlas.microsoft.com/search/fuzzy/json?api-version=1.0&query=gasoline%20station&subscription-key=<your account key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

En este momento, la página MapSearch puede mostrar las ubicaciones de los puntos de interés que se devuelven en una consulta de búsqueda aproximada. Vamos a agregar algunas funcionalidades interactivas y más información sobre las ubicaciones.

## <a name="add-interactive-data"></a>Adición de datos interactivos

La asignación que hemos hecho hasta ahora solo examina los datos de latitud o longitud de los resultados de búsqueda. Sin embargo, si examina el archivo JSON sin formato que devuelve el servicio Search de Azure Maps, verá que contiene información adicional sobre cada gasolinera, incluidos el nombre y la dirección postal. Puede incorporar esos datos al mapa con cuadros emergentes interactivos.

1. Agregue las líneas siguientes al bloque *script* para crear elementos emergentes para los puntos de interés devueltos por Search Service:

    ```JavaScript
    // Add a popup to the map which will display some basic information about a search result on hover over a pin
    var popup = new atlas.Popup();
    map.addEventListener("mouseover", searchLayerName, (e) => {
        var popupContentElement = document.createElement("div");
        popupContentElement.style.padding = "5px";

        var popupNameElement = document.createElement("div");
        popupNameElement.innerText = e.features[0].properties.name;
        popupContentElement.appendChild(popupNameElement);

        var popupAddressElement = document.createElement("div");
        popupAddressElement.innerText = e.features[0].properties.address;
        popupContentElement.appendChild(popupAddressElement);

        var popupPositionElement = document.createElement("div");
        popupPositionElement.innerText = e.features[0].properties.position;
        popupContentElement.appendChild(popupPositionElement);

        popup.setPopupOptions({
            position: e.features[0].geometry.coordinates,
            content: popupContentElement
        });

        popup.open(map);
    });
    ```
    La API **atlas.Popup** proporciona una ventana de información anclada en la posición requerida en el mapa. Este fragmento de código establece el contenido y la posición de la ventana emergente. También agrega un agente de escucha de eventos al control `map` que esperará a que el _mouse_ se desplace sobre la ventana emergente.

2. Guarde el archivo y actualice el explorador. Ahora el mapa del explorador muestra los cuadros emergentes de información cuando se mantiene el mouse sobre cualquiera de las chinchetas de búsqueda.

    ![Control de mapa y Search Service de Azure](./media/tutorial-search-location/popup-map.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Crear una cuenta con Azure Maps
> * Obtención de la clave principal de una cuenta
> * Crear una nueva página web mediante la API de Control de mapa
> * Uso de Search Service para encontrar un punto de interés cercano

Puede acceder al ejemplo de código de este tutorial, aquí:

> [Búsqueda de ubicaciones con Azure Maps](https://github.com/Azure-Samples/azure-maps-samples/blob/master/src/search.html)

En el siguiente tutorial se muestra cómo visualizar una ruta entre dos ubicaciones.

> [!div class="nextstepaction"]
> [Ruta a un destino](./tutorial-route-location.md)
