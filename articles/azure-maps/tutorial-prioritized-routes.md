---
title: Rutas múltiples con Azure Maps | Microsoft Docs
description: Búsqueda de rutas para diferentes modos de desplazamiento mediante Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 10/29/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 67b68489f2e06b9149f842f293a769fa7f688be0
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412710"
---
# <a name="find-routes-for-different-modes-of-travel-using-azure-maps"></a>Búsqueda de rutas para diferentes modos de desplazamiento mediante Azure Maps

En este tutorial se muestra cómo usar la cuenta de Azure Maps y servicio de rutas para buscar la ruta al punto de interés, con una prioridad establecida según el modo de desplazamiento. Se muestran dos rutas distintas en el mapa, una para automóviles y otra para camiones que podrían tener restricciones de ruta debido a la altura, el peso o una carga peligrosa. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una nueva página web con Map Control API
> * Visualizar el flujo de tráfico en el mapa
> * Crear consultas de ruta que declaran el modo de desplazamiento
> * Mostrar varias rutas en el mapa

## <a name="prerequisites"></a>Requisitos previos

Antes de continuar, siga los pasos del primer tutorial para la [Creación de una cuenta de Azure Maps](./tutorial-search-location.md#createaccount) y la [Obtención de la clave de suscripción de la cuenta](./tutorial-search-location.md#getkey).

## <a name="create-a-new-map"></a>Creación de un nuevo mapa

En los pasos siguientes se muestra cómo crear una página HTML estática insertada con Map Control API.

1. En la máquina local, cree un nuevo archivo y asígnele el nombre **MapTruckRoute.html**.
2. Agregue los siguientes componentes HTML al archivo:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Multiple routes by mode of travel</title>
        
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

        <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
        <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
        <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>

        <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
        <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1"></script>

        <script>
            var map, datasource, client;

            function GetMap() {
                //Add Map Control JavaScript code here.
            }
        </script>
        <style>
            html,
            body {
                width: 100%;
                height: 100%;
                padding: 0;
                margin: 0;
            }

            #myMap {
                position: relative;
                width: 100%;
                height: 100%;
            }
        </style>
    </head>
    <body onload="GetMap()">
        <div id="myMap"></div>
    </body>
    </html>
    ```
    
    Observe que el encabezado HTML incluye los archivos de recursos CSS y JavaScript hospedados por la biblioteca de Control de mapa de Azure. Observe el evento `onload` en el cuerpo de la página, el cual llamará a la función `GetMap` cuando el cuerpo de la página se haya cargado. Esta función contendrá el código JavaScript insertado para acceder a las API de Azure Maps.

3. Agregue el siguiente código JavaScript a la función `GetMap`. Reemplace la cadena **\<su clave de Azure Maps\>** por la clave principal que copió de la cuenta de Maps.

    ```JavaScript
    //Add your Azure Maps subscription key to the map SDK. 
    atlas.setSubscriptionKey('<Your Azure Maps Key>');

    //Initialize a map instance.
    map = new atlas.Map('myMap');
    ```

    **atlas.Map** proporciona el control para un mapa web visual e interactivo, y es un componente de la API de Control de mapa de Azure.

4. Guarde el archivo y ábralo en el explorador. En este punto, tiene un mapa básico que puede desarrollar aún más.

   ![Visualización del mapa básico](./media/tutorial-prioritized-routes/basic-map.png)

## <a name="visualize-traffic-flow"></a>Visualizar el flujo de tráfico

1. Agregue la visualización del flujo de tráfico al mapa. `map.events.add` garantiza que todas las funciones de mapas agregadas al mapa se cargan después de que el mapa se haya cargado completamente.

    ```JavaScript
    map.events.add("load", function() {
        // Add Traffic Flow to the Map
        map.setTraffic({
            flow: "relative"
        });
    });
    ```
    
     Se agrega un evento de carga al mapa que se activará cuando los recursos del mapa se hayan cargado por completo. En el controlador de eventos de carga del mapa, la configuración del flujo de tráfico en el mapa está establecida en `relative`, que es la velocidad de la carretera relativa a la libre circulación. También podría establecerlo en velocidad `absolute` de la carretera o `relative-delay`, que muestra la velocidad relativa en el punto en que varía con respecto a la libre circulación.

2. Guarde el archivo **MapTruckRoute.html** y actualice la página en el explorador. Debería ver las calles de Los Ángeles con sus datos de tráfico actuales.

   ![Visualización del mapa del tráfico](./media/tutorial-prioritized-routes/traffic-map.png)

<a id="queryroutes"></a>

## <a name="define-how-the-route-will-be-rendered"></a>Definición de cómo se representará la ruta

En este tutorial, se calcularán y se representarán dos rutas en el mapa. Una ruta con carreteras accesibles para los coches y otra accesible para camiones. Cuando se representen, aparecerá un icono de inicio y otro de fin de ruta, y diferentes líneas de colores para cada ruta.

1. En la función GetMap, después de inicializar el mapa, agregue el siguiente código JavaScript.

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: ['get', 'strokeColor'],
            strokeWidth: ['get', 'strokeWidth'],
            lineJoin: 'round',
            lineCap: 'round',
            filter: ['==', '$type', 'LineString']
        }), 'labels');

        //Add a layer for rendering point data.
        map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
            iconOptions: {
                image: ['get', 'icon'],
                allowOverlap: true
            },
            textOptions: {
                textField: ['get', 'title'],
                offset: [0, 1.2]
            },
            filter: ['==', '$type', 'Point']
        }));
    });
    ```
    
    Se agrega un evento de carga al mapa que se activará cuando los recursos del mapa se hayan cargado por completo. En el controlador de eventos de carga del mapa, se crea un origen de datos para almacenar las líneas de las rutas, así como los puntos inicial y final. Se crea una capa de línea y se asocia al origen de datos para definir cómo se representará la línea de la ruta. Las expresiones se usan para recuperar la anchura y el color de línea desde las propiedades de la característica de línea de ruta. Se agrega un filtro para garantizar que en esta capa solo se representan los datos de GeoJSON LineString. Al agregar la capa al mapa, se pasa un segundo parámetro con el valor `'labels'` que especifica que esta capa se debe representar por debajo de las etiquetas del mapa. Esto garantiza que la línea de ruta no cubrirá las etiquetas de la carretera. Se crea una capa de símbolos y se asocia al origen de datos. Esta capa especifica cómo se representarán los puntos inicial y final. En este caso, se han agregado expresiones para recuperar la imagen del icono y la información de la etiqueta de texto a partir de las propiedades de cada objeto de punto. 
    
2. Para este tutorial, establezca el punto inicial como una compañía ficticia en Seattle llamada Fabrikam y el punto de destino como una oficina de Microsoft. En el controlador de eventos de carga del mapa, agregue el código siguiente.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.356099, 47.580045]), {
        title: 'Fabrikam, Inc.',
        icon: 'pin-round-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.201164, 47.616940]), {
        title: 'Microsoft - Lincoln Square',
        icon: 'pin-blue'
    });
    ```
    
    Este código crea dos [objetos GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) para representar los puntos inicial y final de la ruta. Se agrega una propiedad `title` y `icon` a cada punto.

3. A continuación, agregue el siguiente código JavaScript para agregar los marcadores de los puntos inicial y final al mapa:

    ```JavaScript
    //Add the data to the data source.
    datasource.add([startPoint, endPoint]);

    //Fit the map window to the bounding box defined by the start and end positions.
    map.setCamera({
        bounds: atlas.data.BoundingBox.fromData([startPoint, endPoint]),
        padding: 100
    });
    ```
    
    Los puntos inicial y final se agregan al origen de datos. El rectángulo delimitador de los puntos inicial y final se calcula utilizando la función `atlas.data.BoundingBox.fromData`. Este rectángulo delimitador se usa para establecer la vista de las cámaras del mapa sobre los puntos inicial y final mediante la función `map.setCamera`. Se agrega un relleno para compensar las dimensiones de píxeles de los iconos de símbolos.

4. Guarde el archivo y actualice el explorador para ver los anclajes en el mapa. Ahora el mapa se centra en Seattle, y puede ver como la chincheta azul redonda marca el punto de inicio y otra chincheta azul el punto de fin.

   ![Visualización del mapa con los puntos de inicio y fin](./media/tutorial-prioritized-routes/pins-map.png)

<a id="multipleroutes"></a>

## <a name="render-routes-prioritized-by-mode-of-travel"></a>Representar las rutas con una prioridad establecida según el modo de desplazamiento

En esta sección se muestra cómo usar la API del servicio de rutas de Maps para buscar varias rutas desde un punto inicial dado hasta un destino, según el modo de transporte. El servicio de rutas proporciona varias API para planear la ruta *más rápida*, *más corta*, *más económica* o *más emocionante* entre dos ubicaciones, teniendo en cuenta las condiciones del tráfico en tiempo real. También permite a los usuarios planear rutas futuras mediante el uso de bases de datos que contienen un historial amplio del tráfico y la predicción de duraciones de una ruta en cualquier día y a cualquier hora. Para más información, consulte [Direcciones de GetRoute](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Todos los bloques de código siguientes se deben agregar **dentro del elemento eventListener de carga de mapas** para asegurarse de que se cargan después de que el mapa se carga completamente.

1. Para crear instancias del cliente de servicio, agregue el siguiente código Javascript en el controlador de eventos de carga del mapa.

    ```JavaScript
    //If the service client hasn't been created, create an instance of it.
    if (!client) {
        client = new atlas.service.Client(atlas.getSubscriptionKey());
    }
    ```
    
2. Agregue el siguiente bloque de código para construir una cadena de consulta de la ruta.

    ```JavaScript
    //Create the route request with the query being the start and end point in the format 'startLongitude,startLatitude:endLongitude,endLatitude'.
    var routeQuery = startPoint.geometry.coordinates[1] +
        ',' +
        startPoint.geometry.coordinates[0] +
        ':' +
        endPoint.geometry.coordinates[1] +
        ',' +
        endPoint.geometry.coordinates[0];
    ```

3. Agregue el siguiente código JavaScript para solicitar la ruta de un camión que transporta una carga clasificada como USHazmatClass2 y mostrar los resultados:

    ```JavaScript
    //Execute the truck route query then add the route to the map.
    client.route.getRouteDirections(routeQuery, {
        travelMode: 'truck',
        vehicleWidth: 2,
        vehicleHeight: 2,
        vehicleLength: 5,
        vehicleLoadType: 'USHazmatClass2'
    }).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Get the route line and add some style properties to it.
        var routeLine = geoJsonResponse.getGeoJsonRoutes().features[0];
        routeLine.properties.strokeColor = '#2272B9';
        routeLine.properties.strokeWidth = 9;

        //Add the route line to the data source. We want this to render below the car route which will likely be added to the data source faster, so insert it at index 0.
        datasource.add(routeLine, 0);
    });
    ```
    Este fragmento de código consulta el servicio de enrutamiento de Azure Maps mediante el método [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) y, después, analiza la respuesta en formato GeoJSON mediante [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest). A continuación, crea una matriz de coordenadas para la ruta devuelta y la agrega al origen de datos, pero también agrega un índice de 0 para garantizar que esta se representa antes de cualquier otra línea del origen de datos. Esto se hace ya que, a menudo, el cálculo de la ruta del camión será más lento que el de la ruta de un coche, y si la línea de ruta del camión se agrega al origen de datos después de la ruta del coche, esta se representará por encima. Se agregan dos propiedades a la línea de ruta del camión, una pincelada con un agradable tono azul y una pincelada de 9 píxeles de ancho. 

4. Agregue el siguiente código JavaScript para solicitar la ruta para automóvil y mostrar los resultados:

    ```JavaScript
    //Execute the car route query then add the route to the map.
    client.route.getRouteDirections(routeQuery).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Get the route line and add some style properties to it.
        var routeLine = geoJsonResponse.getGeoJsonRoutes().features[0];
        routeLine.properties.strokeColor = '#B76DAB';
        routeLine.properties.strokeWidth = 5;

        //Add the route line to the data source.
        datasource.add(routeLine);
    });
    ```
    Este fragmento de código usa la misma consulta de ruta de camión para un automóvil. Consulta el servicio de enrutamiento de Azure Maps mediante el método [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) y, después, analiza la respuesta en formato GeoJSON mediante [getGeoJsonRouteDirectionsResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest). Luego, crea una matriz de coordenadas para la ruta devuelta y la agrega al origen de datos. Se agregan dos propiedades a la línea de ruta del coche, una pincelada con un tono morado y una pincelada de 5 píxeles de ancho. 

5. Guarde el archivo **MapTruckRoute.html** y actualice el explorador para observar el resultado. Para una conexión correcta con las API de Maps, debe ver un mapa similar al siguiente.

    ![Rutas con prioridad establecida con Azure Route Service](./media/tutorial-prioritized-routes/prioritized-routes.png)

    La ruta para camión es azul y más gruesa, mientras que la ruta para automóvil es púrpura y más estrecha. La ruta para automóvil transcurre a través de Lake Washington por la I-90, que pasa por túneles en áreas residenciales y por lo que se restringe la carga de residuos peligrosos. La ruta para camión, que especifica un tipo de carga USHazmatClass2, se dirige correctamente para usar una autopista diferente.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Crear una nueva página web con Map Control API
> * Visualizar el flujo de tráfico en el mapa
> * Crear consultas de ruta que declaran el modo de desplazamiento
> * Mostrar varias rutas en el mapa

Puede acceder al ejemplo de código de este tutorial, aquí:

> [Varias rutas con Azure Maps](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/truckRoute.html)

[Consulte este ejemplo aquí](https://azuremapscodesamples.azurewebsites.net/?sample=Multiple%20routes%20by%20mode%20of%20travel)

Para más información sobre la cobertura y las funcionalidades de Azure Maps:

> [!div class="nextstepaction"]
> [Niveles de zoom y cuadrícula de mosaico](zoom-levels-and-tile-grid.md)

Para ver más ejemplos de código y una experiencia interactiva de codificación:

> [!div class="nextstepaction"]
> [Uso del control de mapa](how-to-use-map-control.md)
