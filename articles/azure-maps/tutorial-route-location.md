---
title: Búsqueda de una ruta con Azure Maps | Microsoft Docs
description: Ruta a un punto de interés mediante Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 11/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 7b5b82e80ab4998f7cd106f469bf7ac8e271285d
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2019
ms.locfileid: "56588371"
---
# <a name="route-to-a-point-of-interest-using-azure-maps"></a>Ruta a un punto de interés mediante Azure Maps

En este tutorial se muestra cómo usar la cuenta de Azure Maps y el SDK de Route Service para buscar la ruta al punto de interés. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Crear una nueva página web con Map Control API
> * Establecer las coordenadas de dirección
> * Consultar en Route Service direcciones a un punto de interés

## <a name="prerequisites"></a>Requisitos previos

Antes de continuar, siga los pasos del tutorial anterior [Create your Azure Maps account](./tutorial-search-location.md#createaccount) (Creación de una cuenta con Azure Maps) y [Get the subscription key for your account](./tutorial-search-location.md#getkey) (Obtención de la clave principal para la cuenta).

<a id="getcoordinates"></a>

## <a name="create-a-new-map"></a>Creación de un nuevo mapa

En los pasos siguientes se muestra cómo crear una página HTML estática insertada con Map Control API.

1. En la máquina local, cree un nuevo archivo y asígnele el nombre **MapRoute.html**.
2. Agregue los siguientes componentes HTML al archivo:

    ```HTML
    <!DOCTYPE html>
    <html>
    <head>
        <title>Map Route</title>
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

            #map {
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

    `atlas.Map` proporciona el control para un mapa web visual e interactivo, y es un componente de la API de Control de mapa de Azure.

4. Guarde el archivo y ábralo en el explorador. En este punto, tiene un mapa básico que puede desarrollar aún más.

   ![Visualización del mapa básico](./media/tutorial-route-location/basic-map.png)

## <a name="define-how-the-route-will-be-rendered"></a>Definición de cómo se representará la ruta

En este tutorial, se representará una ruta simple mediante un icono de símbolo para el inicio y el final de la ruta, y una línea para el trazado de la ruta.

1. En la función GetMap, después de inicializar el mapa, agregue el siguiente código JavaScript.

    ```JavaScript
    //Wait until the map resources have fully loaded.
    map.events.add('load', function () {

        //Create a data source and add it to the map.
        datasource = new atlas.source.DataSource();
        map.sources.add(datasource);

        //Add a layer for rendering the route lines and have it render under the map labels.
        map.layers.add(new atlas.layer.LineLayer(datasource, null, {
            strokeColor: '#2272B9',
            strokeWidth: 5,
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
    
    Se agrega un evento de carga al mapa que se activará cuando los recursos del mapa se hayan cargado por completo. En el controlador de eventos de carga del mapa, se crea un origen de datos para almacenar las líneas de las rutas, así como los puntos inicial y final. Se crea una capa de línea y se asocia al origen de datos para definir cómo se representará la línea de la ruta. La línea de ruta tendrá una bonita tonalidad azul con un ancho de 5 píxeles y uniones de líneas y extremos redondeados. Se agrega un filtro para garantizar que en esta capa solo se representan los datos de GeoJSON LineString. Al agregar la capa al mapa, se pasa un segundo parámetro con el valor `'labels'` que especifica que esta capa se debe representar por debajo de las etiquetas del mapa. Esto garantiza que la línea de ruta no cubrirá las etiquetas de la carretera. Se crea una capa de símbolos y se asocia al origen de datos. Esta capa especifica cómo se representarán los puntos inicial y final. En este caso, se han agregado expresiones para recuperar la imagen del icono y la información de la etiqueta de texto a partir de las propiedades de cada objeto de punto. 
    
2. En este tutorial, establezca como punto inicial Microsoft y como punto final una gasolinera de Seattle. En el controlador de eventos de carga del mapa, agregue el código siguiente.

    ```JavaScript
    //Create the GeoJSON objects which represent the start and end point of the route.
    var startPoint = new atlas.data.Feature(new atlas.data.Point([-122.130137, 47.644702]), {
        title: 'Microsoft',
        icon: 'pin-round-blue'
    });

    var endPoint = new atlas.data.Feature(new atlas.data.Point([-122.3352, 47.61397]), {
        title: 'Contoso Oil & Gas',
        icon: 'pin-blue'
    });    
    ```

    Este código crea dos [objetos de punto de GeoJSON](https://en.wikipedia.org/wiki/GeoJSON) para representar los puntos inicial y final de la ruta. Se agrega una propiedad `title` y `icon` a cada punto.
    
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
    
    Los puntos inicial y final se agregan al origen de datos. El rectángulo delimitador de los puntos inicial y final se calcula utilizando la función `atlas.data.BoundingBox.fromData`. Este rectángulo delimitador se usa para establecer la vista de las cámaras del mapa sobre los puntos inicial y final mediante la función **map.setCamera**. Se agrega un relleno para compensar las dimensiones de píxeles de los iconos de símbolos.

3. Guarde el archivo **MapRoute.html** y actualice el explorador. Ahora el mapa se centra en Seattle, y puede ver como la chincheta azul redonda marca el punto de inicio y otra chincheta azul el punto de fin.

   ![Visualización del mapa con los puntos de inicio y fin marcados](./media/tutorial-route-location/map-pins.png)

<a id="getroute"></a>

## <a name="get-directions"></a>Obtención de las direcciones

En esta sección se muestra cómo usar la API del servicio de ruta de Maps para buscar la ruta desde un punto de inicio hasta un destino. El servicio de ruta proporciona las API para planear la ruta *más rápida*, *más corta*, *ecológica* o *emocionante* entre dos ubicaciones. También permite a los usuarios planear rutas futuras mediante el uso de bases de datos que contienen un historial amplio del tráfico y la predicción de duraciones de una ruta en cualquier día y a cualquier hora. Para más información, consulte [Obtención de direcciones de ruta](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Todas las funcionalidades siguientes deben agregarse **dentro del elemento eventListener de carga de mapas** para asegurarse de que se cargan después de que el mapa se ha cargado completamente.

1. Para crear instancias del cliente de servicio, agregue el siguiente código Javascript en el controlador de eventos de carga del mapa.

    ```JavaScript
    //If the service client hasn't already been created, create an instance.
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

3. Para obtener la ruta, agregue el siguiente bloque de código al script. Este consulta el servicio de enrutamiento de Azure Maps mediante el método [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.models.routedirectionsrequestbody?view=azure-iot-typescript-latest) y, después, analiza la respuesta en formato GeoJSON mediante [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routegeojson?view=azure-iot-typescript-latest). A continuación, agrega la línea de ruta en la respuesta al origen de datos, que la representa automáticamente en el mapa.

    ```JavaScript
    //Execute the car route query then add the route to the map once a response is received.
    client.route.getRouteDirections(routeQuery).then(function (response) {
        // Parse the response into GeoJSON
        var geoJsonResponse = new atlas.service.geojson.GeoJsonRouteDirectionsResponse(response);

        //Add the route line to the data source.
        datasource.add(geoJsonResponse.getGeoJsonRoutes().features[0]);
    });
    ```

5. Guarde el archivo **MapRoute.html** y actualice el explorador web. Para conectarse correctamente con las API de Maps, debe ver un mapa similar al siguiente.

    ![Control de mapa y Route Service de Azure](./media/tutorial-route-location/map-route.png)

## <a name="next-steps"></a>Pasos siguientes

En este tutorial aprendió lo siguiente:

> [!div class="checklist"]
> * Crear una nueva página web con Map Control API
> * Establecer las coordenadas de dirección
> * Consultar en el servicio de ruta las direcciones a un punto de interés

Puede acceder al ejemplo de código de este tutorial, aquí:

> [Búsqueda de rutas con Azure Maps](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/route.html)

[Consulte este ejemplo aquí](https://azuremapscodesamples.azurewebsites.net/?sample=Route%20to%20a%20destination)

En el siguiente tutorial se muestra cómo crear una consulta de ruta con restricciones, como el modo de desplazamiento o el tipo de carga y, después, visualizar varias rutas en el mismo mapa.

> [!div class="nextstepaction"]
> [Búsqueda de rutas para diferentes modos de desplazamiento](./tutorial-prioritized-routes.md)
