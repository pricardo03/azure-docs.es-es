---
title: Búsqueda con Azure Maps | Microsoft Docs
description: Búsqueda de un punto cercano de interés mediante Azure Maps
author: walsehgal
ms.author: v-musehg
ms.date: 12/14/2018
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: c11274b8f9d6c3b8c934201b4ec4acd6842d7c0b
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53435368"
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
   <html>
   <head>
      <title>Map Search</title>
      <meta charset="utf-8" />
      <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

      <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
      <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/css/atlas.min.css?api-version=1" type="text/css" />
      <script src="https://atlas.microsoft.com/sdk/js/atlas.min.js?api-version=1"></script>

      <!-- Add a reference to the Azure Maps Services Module JavaScript file. -->
      <script src="https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1"></script>

      <script>      
         var map, datasource, client, popup;

         function GetMap(){
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

3. Agregue el siguiente código JavaScript a la función `GetMap` del archivo HTML. Reemplace la cadena **\<su clave de Azure Maps\>** por la clave principal que copió de la cuenta de Maps.

   ```JavaScript
   //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
   atlas.setSubscriptionKey('<Your Azure Maps Key>');

   //Initialize a map instance.
   map = new atlas.Map('myMap');
   ```

   Este segmento inicializa Map Control API para la clave de cuenta de Azure Maps. **Atlas** es el espacio de nombres que contiene la API y los componentes visuales relacionados. **atlas.Map** proporciona el control para un mapa visual e interactivo. 

4. Guarde los cambios en el archivo y abra la página HTML en un explorador. Este es el mapa más básico que puede crear con una llamada a **atlas.map** y una clave de cuenta.

   ![Visualización del mapa](./media/tutorial-search-location/basic-map.png)

5. En la función `GetMap`, después de inicializar el mapa, agregue el siguiente código JavaScript. 

   ```JavaScript
   //Wait until the map resources have fully loaded.
   map.events.add('load', function () {

      //Create a data source and add it to the map.
      datasource = new atlas.source.DataSource();
      map.sources.add(datasource);

      //Add a layer for rendering point data.
      var resultLayer = new atlas.layer.SymbolLayer(datasource, null, {
         iconOptions: {
            iconImage: 'pin-round-darkblue',
            anchor: 'center',
            allowOverlap: true
         }
      });
      map.layers.add(resultLayer);

      //Create a popup but leave it closed so we can update it and display it later.
      popup = new atlas.Popup();

      //Add a mouse over event to the result layer and display a popup when this event fires.
      map.events.add('mouseover', resultLayer, showPopup);
   });
   ```

   Se agrega un evento de carga al mapa que se activará cuando los recursos del mapa se hayan cargado por completo. En el controlador de eventos de carga del mapa, se crea un origen de datos para almacenar datos de resultados. Se crea una capa de símbolos y se asocia al origen de datos. Esta capa especifica cómo se deben representar los datos de resultados en el origen de datos, en este caso con un icono de chincheta redonda azul oscura centrada sobre la coordenada de resultados y que permite que otros iconos se superpongan. 

<a id="usesearch"></a>

## <a name="add-search-capabilities"></a>Adición de funcionalidades de búsqueda

En esta sección se muestra cómo usar Search API de Azure Maps para buscar un punto de interés en el mapa. Se trata de una API RESTful diseñada para los desarrolladores para buscar direcciones, puntos de interés y otra información geográfica. El servicio Search asigna información de latitud y longitud a una dirección especificada. El **módulo de servicio** que se explica a continuación, puede usarse para buscar una ubicación mediante Maps Search API.

### <a name="service-module"></a>Módulo de servicio

1. En el controlador de eventos de carga del mapa, para crear instancias del cliente de servicio, agregue el siguiente código Javascript.

    ```JavaScript
    //Create an instance of the services client.
     client = new atlas.service.Client(atlas.getSubscriptionKey());
    ```

2. A continuación, agregue el siguiente bloque de script para compilar la consulta de búsqueda. Este fragmento de código usa Fuzzy Search Service, que es una API de búsqueda básica de Search Service. Fuzzy Search Service controla la mayoría de las entradas aproximadas como, por ejemplo, direcciones, lugares y puntos de interés (POI). Este código busca gasolineras cercanas dentro del radio especificado. Después, la respuesta se analiza en formato GeoJSON y se agrega al origen de datos, lo cual automáticamente da como resultado la representación de los datos en el mapa mediante la capa de símbolos. La última parte del script permite establecer la vista de cámaras del mapa mediante el rectángulo delimitador de los resultados con la propiedad [setCamera](/javascript/api/azure-maps-control/atlas.map#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) de Map. Se agrega un relleno para compensar las dimensiones de píxeles de los iconos de símbolos ya que el rectángulo delimitador se calcula en función de las coordenadas. 
 
   ```JavaScript
   //Execute a POI search query then add the results to the map.
    client.search.getSearchPOI('gasoline station', {
        lat: 47.6292,
        lon: -122.2337,
        radius: 100000
    }).then(response => {
        //Parse the response into GeoJSON so that the map can understand.
        var geojsonResponse = new atlas.service.geojson.GeoJsonSearchResponse(response);
        var results = geojsonResponse.getGeoJsonResults();

        //Add the results to the data source so they can be rendered. 
        datasource.add(results);

        // Set the camera bounds
        map.setCamera({
            bounds: results.bbox,
            padding: 50
        });
    });
   ```
 
3. Guarde el archivo **MapSearch.html** y actualice el explorador. Ahora verá que el mapa se centra en Seattle y que aparecen marcadas con chinchetas azules las ubicaciones de las gasolineras del área.

   ![Visualización del mapa con los resultados de la búsqueda](./media/tutorial-search-location/pins-map.png)

4. Puede ver los datos sin procesar que está representando el mapa especificando la siguiente solicitud HTTPRequest en el explorador. Reemplace \<su clave de Azure Maps\> por la clave principal.

   ```http
   https://atlas.microsoft.com/search/fuzzy/json?api-version=1.0&query=gasoline%20station&subscription-key=<Your Azure Maps Key>&lat=47.6292&lon=-122.2337&radius=100000
   ```

En este momento, la página MapSearch puede mostrar las ubicaciones de los puntos de interés que se devuelven en una consulta de búsqueda aproximada. Vamos a agregar algunas funcionalidades interactivas y más información sobre las ubicaciones.

## <a name="add-interactive-data"></a>Adición de datos interactivos

La asignación que hemos hecho hasta ahora solo examina los datos de latitud o longitud de los resultados de búsqueda. Sin embargo, si examina el archivo JSON sin formato que devuelve el servicio Search de Azure Maps, verá que contiene información adicional sobre cada gasolinera, incluidos el nombre y la dirección postal. Puede incorporar esos datos al mapa con cuadros emergentes interactivos.

1. Agregue las siguientes líneas de código en el controlador de eventos de carga del mapa después del código para consultar el servicio de búsquedas aproximadas. Esto creará una instancia de un elemento emergente y agregará un evento mouseover a la capa de símbolos.

    ```JavaScript
   //Create a popup but leave it closed so we can update it and display it later.
    popup = new atlas.Popup();

    //Add a mouse over event to the result layer and display a popup when this event fires.
    map.events.add('mouseover', resultLayer, showPopup);
    ```
    
    La API **atlas.Popup** proporciona una ventana de información anclada en la posición requerida en el mapa. 
      
2. En la etiqueta *script*, después de la función `GetMap`, agregue el siguiente código para mostrar la información de resultados del evento mouseover en el elemento emergente. 

   ```JavaScript
   function showPopup(e) {
        //Get the properties and coordinates of the first shape that the event occured on.
        var p = e.shapes[0].getProperties();
        var position = e.shapes[0].getCoordinates();

        //Create HTML from properties of the selected result.
        var html = ['<div style="padding:5px"><div><b>', p.poi.name,
            '</b></div><div>', p.address.freeformAddress,
            '</div><div>', position[1], ', ', position[0], '</div></div>'];

        //Update the content and position of the popup.
        popup.setPopupOptions({
            content: html.join(''),
            position: position
        });

        //Open the popup.
        popup.open(map);
   }
   ```

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

> [Búsqueda de ubicaciones con Azure Maps](https://github.com/Azure-Samples/AzureMapsCodeSamples/blob/master/AzureMapsCodeSamples/Tutorials/search.html)

[Consulte este ejemplo aquí](https://azuremapscodesamples.azurewebsites.net/?sample=Search%20for%20points%20of%20interest)

En el siguiente tutorial se muestra cómo visualizar una ruta entre dos ubicaciones.

> [!div class="nextstepaction"]
> [Ruta a un destino](./tutorial-route-location.md)
