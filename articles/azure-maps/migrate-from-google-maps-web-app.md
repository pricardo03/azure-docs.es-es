---
title: 'Tutorial: Migración de una aplicación web desde Google Maps | Microsoft Azure Maps'
description: Migración de una aplicación web de Google Maps a Microsoft Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 12/17/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: d9b873a058410219bc55abc4f575823b519a646b
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76989120"
---
# <a name="migrate-a-web-app-from-google-maps"></a>Migración de una aplicación web desde Google Maps

La mayoría de las aplicaciones web que usan Google Maps usan el SDK de JavaScript de Google Maps V3. El SDK web para Azure Maps es el SDK adecuado basado en Azure al que se debe migrar. El SDK web de Azure Maps permite personalizar mapas interactivos con contenido propio e imágenes. Puede ejecutar la aplicación en aplicaciones web o móviles. Este control usa WebGL, lo que permite representar grandes conjuntos de datos con alto rendimiento. Desarrolle con este SDK mediante JavaScript o TypeScript.

Si migra una aplicación web existente, compruebe si usa una biblioteca de control de mapa de código abierto. Ejemplos de bibliotecas de control de mapa de código abierto: Cesium, Leaflet y OpenLayers. Si es así y no quiere usar el SDK web de Azure Maps, otra opción para la migración de la aplicación es continuar usando el control de mapa de código abierto y conectarlo a los servicios de mosaico de Azure Maps ([mosaicos de carretera](https://docs.microsoft.com/rest/api/maps/render/getmaptile) \| [mosaicos de satélite](https://docs.microsoft.com/rest/api/maps/render/getmapimagerytile)). Los puntos siguientes detallan cómo usar Azure Maps en algunas bibliotecas de control de mapa de código abierto usadas habitualmente.

- Cesium: un control de mapa 3D para la Web. [Código de ejemplo](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20Cesium%20JS) \| [Documentación](https://cesiumjs.org/)
- Leaflet: control de mapa 2D ligero para la Web. [Código de ejemplo](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Azure%20Maps%20Raster%20Tiles%20in%20Leaflet%20JS) \| [Documentación](https://leafletjs.com/)
- OpenLayers: un control de mapa 2D para la Web que admite proyecciones. [Código de ejemplo](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Raster%20Tiles%20in%20OpenLayers) \| [Documentación](https://openlayers.org/)

## <a name="key-features-support"></a>Compatibilidad de características clave

En la siguiente tabla se enumeran las características clave de API del SDK de JavaScript de Google Maps V3 y la característica de API admitida en el SDK web de Azure Maps.

| Característica de Google Maps     | Compatibilidad del SDK web de Azure Maps |
|-------------------------|:--------------------------:|
| Marcadores                 | ✓                          |
| Agrupación en clústeres de marcadores       | ✓                          |
| Polilíneas y polígonos    | ✓                          |
| Capas de datos             | ✓                          |
| Superposiciones de suelo         | ✓                          |
| Mapas térmicos               | ✓                          |
| Capas de mosaicos             | ✓                          |
| Capa KML               | ✓                          |
| Diseño de herramientas           | ✓                          |
| Servicio de codificador geográfico        | ✓                          |
| Servicio de direcciones      | ✓                          |
| Servicio de matriz de distancia | ✓                          |
| Servicio de elevación       | Planeado                    |

## <a name="notable-differences-in-the-web-sdks"></a>Diferencias notables en los SDK web

Las siguientes son algunas de las diferencias principales entre los SDK web de Google Maps y Azure Maps que se deben tener en cuenta:

- Además de proporcionar un punto de conexión hospedado para acceder al SDK web para Azure Maps, también está disponible un paquete NPM para insertar el SDK web en aplicaciones, si lo prefiere. Para más información, consulte [esta documentación](how-to-use-map-control.md). Este paquete también incluye las definiciones de TypeScript.
- Después de crear una instancia de la clase Map en Azure Maps, el código debe esperar a que se active el evento `ready` o `load` del mapa antes de interactuar con el mapa. Este orden garantizará que todos los recursos del mapa se hayan cargado y estén listos para su acceso.
- Ambas plataformas usan un sistema de mosaicos similar para los mapas base; sin embargo, los mosaicos de Google Maps tienen 256 píxeles de dimensión, mientras que los mosaicos de Azure Maps tienen 512. Por lo tanto, para obtener la misma vista de mapa en Azure Maps que en Google Maps, al nivel de zoom usado en Google Maps debe restarse uno en Azure Maps.
- Las coordenadas de Google Maps se conocen como "latitud, longitud", mientras que Azure Maps usa "longitud, latitud". El formato de Azure Maps se alinea con el estándar `[x, y]`, que siguen la mayoría de las plataformas de GIS.
- Las formas del SDK web para Azure Maps se basan en el esquema GeoJSON. Las clases auxiliares se exponen a través del [espacio de nombres *atlas.data*](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data?view=azure-iot-typescript-latest). También está la clase [*atlas.Shape*](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape). Esta clase se puede usar para encapsular objetos GeoJSON y facilitar su actualización y mantenimiento de un modo enlazable de datos.
- Las coordenadas en Azure Maps se definen como objetos Position. Una coordenada se especifica como una matriz de números con el formato `[longitude, latitude]` o se especifica mediante new atlas.data.Position(longitude, latitude).
    > [!TIP]
    > La clase Position tiene un método auxiliar estático para importar las coordenadas que están en formato "latitud, longitud". El método [atlas.data.Position.fromLatLng](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position?view=azure-iot-typescript-latest) se puede reemplazar a menudo por el método `new google.maps.LatLng` en el código de Google Maps.
- En lugar de especificar la información de estilo en cada una de las formas que se agregan al mapa, Azure Maps separa los estilos de los datos. Los datos se almacenan en orígenes de datos conectados a las capas de representación. El código de Azure Maps usa orígenes de datos para representar los datos. Este enfoque proporciona una ventaja de rendimiento mejorado. Además, muchas capas admiten estilo controlado por datos, en el que se puede agregar lógica de negocios a las opciones de estilo de la capa. Esta compatibilidad cambia el modo en que se representan las formas individuales dentro de una capa en función de las propiedades definidas en la forma.

## <a name="web-sdk-side-by-side-examples"></a>Ejemplos en paralelo del SDK web

La colección siguiente tiene ejemplos de código para cada plataforma, cada uno de los cuales trata casos de uso comunes. Está diseñada para ayudarle a migrar la aplicación web desde el SDK de JavaScript de Google Maps V3 al SDK web de Azure Maps. Los ejemplos de código relacionados con aplicaciones web se proporcionan en JavaScript. Sin embargo, Azure Maps también proporciona definiciones de TypeScript como opción adicional mediante un [módulo de NPM](how-to-use-map-control.md).

### <a name="load-a-map"></a>Carga de un mapa

La carga de un mapa sigue el mismo conjunto de pasos en ambos SDK:

- Agregue una referencia al SDK de mapa.
- Agregue una etiqueta `div` al cuerpo de la página, que actuará como marcador de posición para el mapa.
- Cree una función de JavaScript a la que se llamará cuando se cargue la página.
- Cree una instancia de la clase de mapa correspondiente.

**Algunas diferencias clave**

- Google Maps requiere que se especifique una clave de cuenta en la referencia de script de la API. Las credenciales de autenticación para Azure Maps se especifican como opciones de la clase map. Esta credencial puede ser una clave de suscripción o información de Azure Active Directory.
- Google Maps toma una función de devolución de llamada en la referencia de script de la API, que se usa para llamar a una función de inicialización para cargar el mapa. Con Azure Maps debe usarse el evento onload de la página.
- Al hacer referencia al elemento `div` en el que se va a representar el mapa, la clase `Map` de Azure Maps solo requiere el valor `id`, mientras que Google Maps requiere un objeto `HTMLElement`.
- Las coordenadas en Azure Maps se definen como objetos Position, que se pueden especificar como una matriz de números simple con el formato `[longitude, latitude]`.
- El nivel de zoom en Azure Maps es un nivel inferior al nivel de zoom en Google Maps. Esta discrepancia se debe a la diferencia de tamaños en el sistema de mosaicos de las dos plataformas.
- Azure Maps no agrega ningún control de navegación al lienzo del mapa. Por lo tanto, de forma predeterminada, un mapa no tiene botones de zoom ni botones de estilo del mapa. Sin embargo, hay controles para agregar un selector de estilo de mapa, botones de zoom, control de la brújula o giro y un control de paso.
- Se agrega un controlador de eventos en Azure Maps para supervisar el evento `ready` de la instancia de mapa. Este evento se activará cuando el mapa haya terminado de cargar el contexto WebGL y todos los recursos necesarios. Agregue el código que desee ejecutar después de que se complete la carga del mapa a este controlador de eventos.

Los ejemplos siguientes muestran cómo cargar un mapa básico, que está centrado en Nueva York en las coordenadas (longitud: -73,985, latitud: 40,747) y está en el nivel de zoom 12 en Google Maps.

**Antes: Google Maps**

El código siguiente es un ejemplo de cómo mostrar un mapa de Google centrado y acercarlo sobre una ubicación.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(40.747, -73.985),
                zoom: 12
            });
        }
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

Al ejecutar este código en un explorador, se mostrará un mapa similar al de la siguiente imagen:

<center>

![Google Maps simple](media/migrate-google-maps-web-app/simple-google-map.png)</center>

**Después: Azure Maps**

En el código siguiente se muestra cómo cargar un mapa con la misma vista en Azure Maps junto con un control de estilo de mapa y botones de zoom.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;

        function initMap() {
            map = new atlas.Map('myMap', {
                center: [-73.985, 40.747],  //Format coordinates as longitude, latitude.
                zoom: 11,   //Subtract the zoom level by one.

                //Specify authentication information when loading the map.
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {
                //Add zoom controls to bottom right of map.
                map.controls.add(new atlas.control.ZoomControl(), {
                    position: 'bottom-right'
                });

                //Add map style control in top left corner of map.
                map.controls.add(new atlas.control.StyleControl(), {
                    position: 'top-left'
                });
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

Al ejecutar este código en un explorador, se mostrará un mapa similar al de la siguiente imagen:

<center>

![Azure Maps simple](media/migrate-google-maps-web-app/simple-azure-maps.png)</center>

Puede encontrar documentación detallada sobre cómo configurar y usar el control de mapa de Azure Maps en una aplicación web [aquí](how-to-use-map-control.md).

> [!NOTE]
> A diferencia de Google Maps, Azure Maps no necesita que se especifiquen un centro y un nivel de zoom iniciales al cargar el mapa. Si no se proporciona esta información al cargar el mapa, este intentará determinar en qué ciudad se encuentra el usuario, y centrará y aplicará el zoom del mapa allí.

**Recursos adicionales:**

- Azure Maps también proporciona controles de navegación para girar y desplazar la vista del mapa como se documenta [aquí](map-add-controls.md).

### <a name="localizing-the-map"></a>Localización del mapa

Si el público está distribuido entre varios países o habla distintos idiomas, la localización es importante.

**Antes: Google Maps**

Para localizar Google Maps, se agregan los parámetros de idioma y región a

```html
<script type="text/javascript" src=" https://maps.googleapis.com/maps/api/js?callback=initMap&key=[api_key]& language=[language_code]&region=[region_code]" async defer></script>
```

Este es un ejemplo de Google Maps con el idioma establecido en "fr-FR".

<center>

![Localización de Google Maps](media/migrate-google-maps-web-app/google-maps-localization.png)</center>

**Después: Azure Maps**

Azure Maps proporciona dos formas diferentes de establecer el idioma y la vista regional del mapa. La primera opción es agregar esta información al espacio de nombres *atlas* global, lo que provocará que estos valores sean los predeterminados en todas las instancias del control de mapa de la aplicación. El código siguiente establece el idioma en francés ("fr-FR") y la vista regional en "auto":

```javascript
atlas.setLanguage('fr-FR');
atlas.setView('auto');
```

La segunda opción consiste en usar esta información a las opciones del mapa al cargar el mapa como:

```javascript
map = new atlas.Map('myMap', {
    language: 'fr-FR',
    view: 'auto',

    authOptions: {
        authType: 'subscriptionKey',
        subscriptionKey: '<Your Azure Maps Key>'
    }
});
```

> [!NOTE]
> Con Azure Maps es posible cargar varias instancias del mapa en la misma página con diferentes configuraciones de idioma y región. Además, también es posible actualizar esta configuración en el mapa una vez que se haya cargado. [Aquí](supported-languages.md) se puede encontrar una lista detallada de idiomas admitidos en Azure Maps.

Este es un ejemplo de Azure Maps con el idioma establecido en "fr" y la región del usuario establecida en "fr-FR".

<center>

![Localización de Azure Maps](media/migrate-google-maps-web-app/azure-maps-localization.png)</center>

### <a name="setting-the-map-view"></a>Establecimiento de la vista del mapa

Los mapas dinámicos en Azure Maps y Google Maps se pueden mover mediante programación a nuevas ubicaciones geográficas. Para ello, llame a las funciones adecuadas en JavaScript. En los ejemplos se muestra cómo hacer que el mapa muestre las imágenes aéreas de satélite, se centre sobre una ubicación y cambie el nivel de zoom a 15 en Google Maps. Se usan las siguientes coordenadas de ubicación: longitud: -111,0225 y latitud: 35,0272.

> [!NOTE]
> Google Maps usa mosaicos de 256 píxeles en las dimensiones mientras que Azure Maps usa un mosaico mayor de 512 píxeles. Esto reduce el número de solicitudes de red que Azure Maps necesita para cargar la misma área del mapa que Google Maps. Sin embargo, debido a la forma en que las pirámides de mosaicos funcionan en los controles de mapa, que los mosaicos de Azure Maps sean mayores significa que, para conseguir esa misma área visible como mapa en Google Maps, debe restar uno al nivel de zoom usado en Google Maps cuando use Azure Maps.

**Antes: Google Maps**

El control de mapa de Google Maps se puede mover mediante programación con el método `setOptions`. Este método permite especificar el centro del mapa y un nivel de zoom.

```javascript
map.setOptions({
    mapTypeId: google.maps.MapTypeId.SATELLITE,
    center: new google.maps.LatLng(35.0272, -111.0225),
    zoom: 15
});
```

<center>

![Vista establecida de Google Maps](media/migrate-google-maps-web-app/google-maps-set-view.png)</center>

**Después: Azure Maps**

En Azure Maps, la posición del mapa se puede cambiar mediante programación con el método `setCamera` del mapa y el estilo del mapa se puede cambiar con el método `setStyle`. Las coordenadas en Azure Maps tienen el formato "longitud, latitud" y se resta uno al valor del nivel de zoom.

```javascript
map.setCamera({
    center: [-111.0225, 35.0272],
    zoom: 14
});

map.setStyle({
    style: 'satellite'
});
```

<center>

![Vista establecida de Azure Maps](media/migrate-google-maps-web-app/azure-maps-set-view.jpeg)</center>

**Recursos adicionales:**

- [Elección de un estilo de mapa](choose-map-style.md)
- [Estilos de mapa admitidos](supported-map-styles.md)

### <a name="adding-a-marker"></a>Adición de un marcador

En Azure Maps hay varias maneras en las que se pueden representar los datos del punto en el mapa:

- **Marcadores HTML**: representan los puntos mediante los elementos DOM tradicionales. Los marcadores HTML admiten el arrastre.
- **Capa de símbolos**: representa los puntos con un icono o texto dentro del contexto WebGL.
- **Capa de burbujas**: representa los puntos como círculos en el mapa. Los radios de los círculos se pueden escalar en función de las propiedades de los datos.

Las capas de símbolos y de burbujas se representan en el contexto WebGL. Ambas capas son capaces de representar grandes conjuntos de puntos en el mapa. Estas capas requieren que los datos se almacenen en un origen de datos. Los orígenes de datos y las capas de representación deben agregarse al mapa una vez activado el evento `ready`. Los marcadores HTML se representan como elementos DOM dentro de la página y no usan un origen de datos. Cuanto más elementos DOM tenga una página, más lenta es. Si se representan más de unos cientos de puntos en un mapa, se recomienda usar en su lugar una de las capas de representación.

En los siguientes ejemplos se agrega un marcador al mapa en (longitud:-0,2, latitud: 51,5) con el número 10 superpuesto como etiqueta.

**Antes: Google Maps**

Con Google Maps, los marcadores se agregan al mapa mediante la clase `google.maps.Marker` y especificando el mapa como una de las opciones.

```javascript
//Create a marker and add it to the map.
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(51.5, -0.2),
    label: '10',
    map: map
});
```

<center>

![Marcador de Google Maps](media/migrate-google-maps-web-app/google-maps-marker.png)</center>

**Después: Azure Maps con marcadores HTML**

En Azure Maps, se pueden usar marcadores HTML para mostrar un punto en el mapa. Los marcadores HTML se recomiendan para aplicaciones sencillas que únicamente necesitan mostrar un pequeño número de puntos en el mapa. Para usar un marcador HTML, puede crear una instancia de la clase `atlas.HtmlMarker`, establecer las opciones de texto y posición y agregar el marcador al mapa mediante el método `map.markers.add`.

```javascript
//Create a HTML marker and add it to the map.
map.markers.add(new atlas.HtmlMarker({
    text: '10',
    position: [-0.2, 51.5]
}));
```

<center>

![Marcador HTML de Azure Maps](media/migrate-google-maps-web-app/azure-maps-html-marker.png)</center>

**Después: Azure Maps con una capa de símbolos**

Cuando se usa una capa de símbolos, los datos se deben agregar a un origen de datos y adjuntar el origen de datos a la capa. Además, el origen de datos y la capa deben agregarse al mapa una vez que se ha activado el evento `ready`. Para representar un valor de texto único encima de un símbolo, la información del texto se debe almacenar como una propiedad del punto de datos y hacer referencia a la propiedad en la opción `textField` de la capa. Este enfoque representa un poco más de trabajo que el uso de marcadores HTML, pero tiene ventajas de rendimiento.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map, datasource;

        function initMap() {
            map = new atlas.Map('myMap', {
                center: [-0.2, 51.5],
                zoom: 9,
                
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                //Create a point feature, add a property to store a label for it, and add it to the data source.
                datasource.add(new atlas.data.Feature(new atlas.data.Point([-0.2, 51.5]), {
                    label: '10'
                }));

                //Add a layer for rendering point data as symbols.
                map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
                    textOptions: {
                        //Use the label property to populate the text for the symbols.
                        textField: ['get', 'label'],
                        color: 'white',
                        offset: [0, -1]
                    }
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Capa de símbolos de Azure Maps](media/migrate-google-maps-web-app/azure-maps-symbol-layer.png)</center>

**Recursos adicionales:**

- [Creación de un origen de datos](create-data-source-web-sdk.md)
- [Adición de una capa de símbolos](map-add-pin.md)
- [Adición de una capa de burbujas](map-add-bubble-layer.md)
- [Datos de punto de clúster](clustering-point-data-web-sdk.md)
- [Adición de marcadores HTML](map-add-custom-html.md)
- [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)
- [Opciones de icono de capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)
- [Opción de texto de capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)
- [Clase de marcador HTML](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)
- [Opciones de marcador HTML](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

### <a name="adding-a-custom-marker"></a>Adición de un marcador personalizado

Se pueden usar imágenes personalizadas para representar puntos en un mapa. La siguiente imagen de mapa utiliza una imagen personalizada para mostrar un punto en el mapa. El punto se muestra en latitud: 51,5, longitud: -0,2. El delimitador desplaza la posición del marcador, de modo que el punto del icono de chincheta se alinee con la posición correcta en el mapa.

<center>

![imagen de marcador amarillo](media/migrate-google-maps-web-app/ylw_pushpin.png)<br/>
ylw\_pushpin.png</center>

**Antes: Google Maps**

En Google Maps, se crea un marcador personalizado mediante la especificación de un objeto `Icon` que contiene la dirección `url` para la imagen, un punto `anchor` para alinear el punto de la imagen de marcador con la coordenada en el mapa. El valor del delimitador en Google Maps está en relación con la esquina superior izquierda de la imagen.

```javascript
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(51.5, -0.2),
    icon: {
        url: 'https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png',
        anchor: new google.maps.Point(5, 30)
    },
    map: map
});
```

<center>

![Marcador personalizado de Google Maps](media/migrate-google-maps-web-app/google-maps-custom-marker.png)</center>

**Después: Azure Maps con marcadores HTML**

Para personalizar un marcador HTML en Azure Maps se puede pasar un elemento HTML `string` o `HTMLElement` a la opción `htmlContent` del marcador. En Azure Maps, se usa la opción `anchor` para especificar la posición relativa del marcador con respecto a la coordenada de posición mediante uno de los nueve puntos de referencia definidos. Los puntos de referencia definidos son: "center", "top", "bottom", "left", "right", "top-left", "top-right", "bottom-left" y "bottom-right". El contenido se delimita en el centro inferior del contenido HTML de forma predeterminada. Para que sea más fácil migrar el código de Google Maps, establezca el elemento `anchor` en "top-left" y, a continuación, use la opción `pixelOffset` con el mismo desplazamiento usado en Google Maps. Los desplazamientos en Azure Maps se mueven en dirección opuesta a la de Google Maps, por lo que se deben multiplicar por menos uno.

> [!TIP]
> Agregue `pointer-events:none` como estilo en el contenido HTML para deshabilitar el comportamiento de arrastre predeterminado en Microsoft Edge, que mostrará un icono no deseado.

```javascript
map.markers.add(new atlas.HtmlMarker({
    htmlContent: '<img src="https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png" style="pointer-events: none;" />',
    anchor: 'top-left',
    pixelOffset: [-5, -30],
    position: [-0.2, 51.5]
}));
```

<center>

![Marcador HTML personalizado de Azure Maps](media/migrate-google-maps-web-app/azure-maps-custom-html-marker.png)</center>

**Después: Azure Maps con una capa de símbolos**

Además, las capas de símbolos de Azure Maps admiten imágenes personalizadas, pero la imagen debe cargarse primero en los recursos del mapa y debe tener asignado un identificador único. Después, la capa de símbolos puede hacer referencia a este identificador. El símbolo se puede desplazar para alinearse con el punto correcto de la imagen mediante la opción `offset` del icono. En Azure Maps, se usa la opción `anchor` para especificar la posición relativa del símbolo con respecto a la coordenada de posición mediante uno de los nueve puntos de referencia definidos. Los coordenadas de posición definidas son: "center", "top", "bottom", "left", "right", "top-left", "top-right", "bottom-left" y "bottom-right". El contenido se delimita en el centro inferior del contenido HTML de forma predeterminada. Para que sea más fácil migrar el código de Google Maps, establezca el elemento `anchor` en "top-left" y, a continuación, use la opción `offset` con el mismo desplazamiento usado en Google Maps. Los desplazamientos en Azure Maps se mueven en dirección opuesta a la de Google Maps, por lo que se deben multiplicar por menos uno.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map, datasource;

        function initMap() {
            map = new atlas.Map('myMap', {
                center: [-0.2, 51.5],
                zoom: 9,
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Load the custom image icon into the map resources.
                map.imageSprite.add('my-yellow-pin', 'https://azuremapscodesamples.azurewebsites.net/Common/images/icons/ylw-pushpin.png').then(function () {

                    //Create a data source and add it to the map.
                    datasource = new atlas.source.DataSource();
                    map.sources.add(datasource);

                    //Create a point and add it to the data source.
                    datasource.add(new atlas.data.Point([-0.2, 51.5]));

                    //Add a layer for rendering point data as symbols.
                    map.layers.add(new atlas.layer.SymbolLayer(datasource, null, {
                        iconOptions: {
                            //Set the image option to the id of the custom icon that was loaded into the map resources.
                            image: 'my-yellow-pin',
                            anchor: 'top-left',
                            offset: [-5, -30]
                        }
                    }));
                });
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Capa de símbolos de icono personalizado de Azure Maps](media/migrate-google-maps-web-app/azure-maps-custom-icon-symbol-layer.png)</center>

> [!TIP]
> Para crear una representación de puntos personalizada avanzada, use varias capas de representación juntas. Por ejemplo, supongamos que desea tener varias chinchetas que tengan el mismo icono en diferentes círculos de color. En lugar de crear un grupo de imágenes para cada superposición de color, agregue una capa de símbolos encima de una capa de burbujas y haga que las chinchetas hagan referencia al mismo origen de datos. Este enfoque será más eficaz que crear y mantener un grupo de imágenes diferentes.

**Recursos adicionales:**

- [Creación de un origen de datos](create-data-source-web-sdk.md)
- [Adición de una capa de símbolos](map-add-pin.md)
- [Adición de marcadores HTML](map-add-custom-html.md)
- [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)
- [Opciones de icono de capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)
- [Opción de texto de capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)
- [Clase de marcador HTML](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)
- [Opciones de marcador HTML](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

### <a name="adding-a-polyline"></a>Adición de una polilínea

Las polilíneas se usan para representar una línea o una ruta en el mapa. En los ejemplos siguientes se muestra cómo crear una polilínea con guiones en el mapa.

**Antes: Google Maps**

En Google Maps, la clase Polyline toma un conjunto de opciones. Se pasa una matriz de coordenadas en la opción `path` de la polilínea.

```javascript
//Get the center of the map.
var center = map.getCenter();

//Define a symbol using SVG path notation, with an opacity of 1.
var lineSymbol = {
    path: 'M 0,-1 0,1',
    strokeOpacity: 1,
    scale: 4
};

//Create the polyline.
var line = new google.maps.Polyline({
    path: [
        center,
        new google.maps.LatLng(center.lat() - 0.5, center.lng() - 1),
        new google.maps.LatLng(center.lat() - 0.5, center.lng() + 1)
    ],
    strokeColor: 'red',
    strokeOpacity: 0,
    strokeWeight: 4,
    icons: [{
        icon: lineSymbol,
        offset: '0',
        repeat: '20px'
    }]
});

//Add the polyline to the map.
line.setMap(map);
```

<center>

![Polilínea de Google Maps](media/migrate-google-maps-web-app/google-maps-polyline.png)</center>

**Después: Azure Maps**

En Azure Maps, las polilíneas se llaman objetos `LineString` o `MultiLineString`. Estos objetos se pueden agregar a un origen de datos y se representan mediante una capa de línea.

```javascript
//Get the center of the map.
var center = map.getCamera().center;

//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a line and add it to the data source.
datasource.add(new atlas.data.LineString([
    center,
    [center[0] - 1, center[1] - 0.5],
    [center[0] + 1, center[1] - 0.5]
]));

//Add a layer for rendering line data.
map.layers.add(new atlas.layer.LineLayer(datasource, null, {
    strokeColor: 'red',
    strokeWidth: 4,
    strokeDashArray: [3, 3]
}));
```

<center>

![Polilínea de Azure Maps](media/migrate-google-maps-web-app/azure-maps-polyline.png)</center>

**Recursos adicionales:**

- [Adición de líneas al mapa](map-add-line-layer.md)
- [Opciones de capa de línea](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)
- [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)

### <a name="adding-a-polygon"></a>Adición de un polígono

Los polígonos se usan para representar un área en el mapa. Azure Maps y Google Maps proporcionan una compatibilidad similar para los polígonos. En los ejemplos siguientes se muestra cómo crear un polígono que forme un triángulo basado en la coordenada del centro del mapa.

**Antes: Google Maps**

En Google Maps, la clase Polygon toma un conjunto de opciones. Se pasa una matriz de coordenadas en la opción `paths` del polígono.

```javascript
//Get the center of the map.
var center = map.getCenter();

//Create a polygon.
var polygon = new google.maps.Polygon({
    paths: [
        center,
        new google.maps.LatLng(center.lat() - 0.5, center.lng() - 1),
        new google.maps.LatLng(center.lat() - 0.5, center.lng() + 1),
        center
    ],
    strokeColor: 'red',
    strokeWeight: 2,
    fillColor: 'rgba(0, 255, 0, 0.5)'
});

//Add the polygon to the map
polygon.setMap(map);
```

<center>

![Polígono de Google Maps](media/migrate-google-maps-web-app/google-maps-polygon.png)</center>

**Después: Azure Maps**

En Azure Maps, se pueden agregar objetos `Polygon` y `MultiPolygon` a un origen de datos y se representan en el mapa mediante capas. El área de un polígono se puede representar en una capa de polígono. El contorno de un polígono se puede representar con una capa de línea.

```javascript
//Get the center of the map.
var center = map.getCamera().center;

//Create a data source and add it to the map.
datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a polygon and add it to the data source.
datasource.add(new atlas.data.Polygon([
    center,
    [center[0] - 1, center[1] - 0.5],
    [center[0] + 1, center[1] - 0.5],
    center
]));

//Add a polygon layer for rendering the polygon area.
map.layers.add(new atlas.layer.PolygonLayer(datasource, null, {
    fillColor: 'rgba(0, 255, 0, 0.5)'
}));

//Add a line layer for rendering the polygon outline.
map.layers.add(new atlas.layer.LineLayer(datasource, null, {
    strokeColor: 'red',
    strokeWidth: 2
}));
```

<center>

![Polígono de Azure Maps](media/migrate-google-maps-web-app/azure-maps-polygon.png)</center>

**Recursos adicionales:**

- [Adición de un polígono al mapa](map-add-shape.md)
- [Adición de un círculo al mapa](map-add-shape.md#add-a-circle-to-the-map)
- [Opciones de capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)
- [Opciones de capa de línea](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)
- [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)

### <a name="display-an-info-window"></a>Mostrar una ventana de información

Se puede mostrar en el mapa información adicional de una entidad como una clase `google.maps.InfoWindow` en Google Maps. En Azure Maps, esta funcionalidad se puede lograr con la clase `atlas.Popup`. En los siguientes ejemplos se agrega un marcador al mapa y, cuando se hace clic en él, se muestra una ventana de información o un elemento emergente.

**Antes: Google Maps**

Con Google Maps, se crea una ventana de información mediante el constructor `google.maps.InfoWindow`.

```javascript
//Add a marker in which to display an infowindow for.
var marker = new google.maps.Marker({
    position: new google.maps.LatLng(47.6, -122.33),
    map: map
});

//Create an infowindow.
var infowindow = new google.maps.InfoWindow({
    content: '<div style="padding:5px"><b>Hello World!</b></div>'
});

//Add a click event to the marker to open the infowindow.
marker.addListener('click', function () {
    infowindow.open(map, marker);
});
```

<center>

![Elemento emergente de Google Maps](media/migrate-google-maps-web-app/google-maps-popup.png)</center>

**Después: Azure Maps**

En Azure Maps se puede usar un elemento emergente para mostrar información adicional de una ubicación. Se puede pasar un objeto HTML `string` o `HTMLElement` a la opción `content` del elemento emergente. Si lo desea, los elementos emergentes se pueden mostrar independientemente de cualquier forma. Por ello, los elementos emergentes requieren que se especifique un valor `position`. Para mostrar un elemento emergente, llame al método `open` y pase el `map` en el que se va a mostrar el elemento emergente.

```javascript
//Add a marker to the map in which to display a popup for.
var marker = new atlas.HtmlMarker({
    position: [-122.33, 47.6]
});

//Add the marker to the map.
map.markers.add(marker);

//Create a popup.
var popup = new atlas.Popup({
    content: '<div style="padding:5px"><b>Hello World!</b></div>',
    position: [-122.33, 47.6],
    pixelOffset: [0, -35]
});

//Add a click event to the marker to open the popup.
map.events.add('click', marker, function () {
    //Open the popup
    popup.open(map);
});
```

<center>

![Elemento emergente de Azure Maps](media/migrate-google-maps-web-app/azure-maps-popup.png)</center>

> [!NOTE]
> Para hacer lo mismo con una capa de símbolos, burbujas, líneas o polígonos, simplemente pase la capa al código de evento del mapa en lugar de a un marcador.

**Recursos adicionales:**

- [Adición de un elemento emergente](map-add-popup.md)
- [Elemento emergente con contenido multimedia](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popup%20with%20Media%20Content)
- [Elementos emergentes en formas](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Popups%20on%20Shapes)
- [Reutilización de un elemento emergente con varias chinchetas](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Reusing%20Popup%20with%20Multiple%20Pins)
- [Clase de elemento emergente](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
- [Opciones de elemento emergente](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

### <a name="import-a-geojson-file"></a>Importación de un archivo GeoJSON

Google Maps admite la carga y la aplicación dinámica de estilo a los datos GeoJSON mediante la clase `google.maps.Data`. La funcionalidad de esta clase se alinea mucho más con el estilo controlado por datos de Azure Maps. Una diferencia clave es que con Google Maps se especifica una función de devolución de llamada. La lógica de negocios para aplicar estilos a cada característica se procesa individualmente en el subproceso de la interfaz de usuario. En Azure Maps, las capas admiten la especificación de expresiones controladas por datos como opciones de estilo. Estas expresiones se procesan en tiempo de representación en un subproceso independiente. Por ello, este enfoque aumenta el rendimiento de la representación. Esta ventaja se observa cuando se representan rápidamente conjuntos de datos más grandes.

En los siguientes ejemplos se carga una fuente GeoJSON de todos los terremotos en los últimos siete días de los USGS. Se representan como círculos a escala en el mapa. El color y la escala de cada círculo se basan en la magnitud de cada terremoto, que está almacenada en la propiedad `"mag"` de cada característica del conjunto de datos. Si la magnitud es mayor o igual que cinco, el círculo será rojo. Si es mayor o igual que tres, pero menor que cinco, el círculo será naranja. Si es inferior a tres, el círculo será verde. El radio de cada círculo será el valor exponencial de la magnitud multiplicado por 0,1.

**Antes: Google Maps**

En Google Maps, se puede especificar una función de devolución de llamada única en el método `map.data.setStyle`. Este método se usará para aplicar la lógica de negocios a cada una de las características cargadas desde la fuente GeoJSON mediante el método `map.data.loadGeoJson`.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(20, -160),
                zoom: 2
            });

            //Define a callback to style each feature.
            map.data.setStyle(function (feature) {

                //Extract the 'mag' property from the feature.
                var mag = parseFloat(feature.getProperty('mag'));

                //Set the color value to 'green' by default.
                var color = 'green';

                //If the mag value is greater than 5, set the color to 'red'.
                if (mag >= 5) {
                    color = 'red';
                }
                //If the mag value is greater than 3, set the color to 'orange'.
                else if (mag >= 3) {
                    color = 'orange';
                }

                return /** @type {google.maps.Data.StyleOptions} */({
                    icon: {
                        path: google.maps.SymbolPath.CIRCLE,

                        //Scale the radius based on an exponential of the 'mag' value.
                        scale: Math.exp(mag) * 0.1,
                        fillColor: color,
                        fillOpacity: 0.75,
                        strokeWeight: 2,
                        strokeColor: 'white'
                    }
                });
            });

            //Load the data feed.
            map.data.loadGeoJson(earthquakeFeed);
        }
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![GeoJSON de Google Maps](media/migrate-google-maps-web-app/google-maps-geojson.png)</center>

**Después: Azure Maps**

GeoJSON es el tipo de datos base en Azure Maps y se puede importar fácilmente a un origen de datos mediante el método `datasource.importFromUrl`. Una capa de burbujas proporciona funcionalidad para representar círculos a escala en función de las propiedades de las características de un origen de datos. En lugar de tener una función de devolución de llamada, la lógica de negocios se convierte en una expresión y se pasa a las opciones de estilo. Las expresiones definen cómo funciona la lógica de negocios. Las expresiones se pueden pasar a otro subproceso y evaluarse con los datos de la característica. Se pueden agregar varios orígenes de datos y capas a Azure Maps, cada uno con una lógica de negocios diferente. Esta característica permite representar varios conjuntos de datos en el mapa de maneras diferentes.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-160, 20],
                zoom: 1,

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                //Load the earthquake data.
                datasource.importDataFromUrl(earthquakeFeed);

                //Create a layer to render the data points as scaled circles.
                map.layers.add(new atlas.layer.BubbleLayer(datasource, null, {
                    //Make the circles semi-transparent.
                    opacity: 0.75,

                    color: [
                        'case',

                        //If the mag value is greater than 5, return 'red'.
                        ['>=', ['get', 'mag'], 5],
                        'red',

                        //If the mag value is greater than 3, return 'orange'.
                        ['>=', ['get', 'mag'], 3],
                        'orange',

                        //Return 'green' as a fallback.
                        'green'
                    ],

                    //Scale the radius based on an exponential of the 'mag' value.
                    radius: ['*', ['^', ['e'], ['get', 'mag']], 0.1]
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![GeoJSON de Azure Maps](media/migrate-google-maps-web-app/azure-maps-geojson.png)</center>

**Recursos adicionales:**

- [Adición de una capa de símbolos](map-add-pin.md)
- [Adición de una capa de burbujas](map-add-bubble-layer.md)
- [Datos de punto de clúster](clustering-point-data-web-sdk.md)
- [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)

### <a name="marker-clustering"></a>Agrupación en clústeres de marcadores

Cuando se visualizan muchos puntos de datos en el mapa, estos se superponen, el mapa presenta un aspecto desordenado y resulta difícil consultar y usar. La agrupación en clústeres de datos de punto puede utilizarse para mejorar esta experiencia de usuario y también el rendimiento. La agrupación en clústeres de datos de punto es el proceso de combinar datos de punto que están cerca unos de otros y representarlos en el mapa como un único punto de datos agrupados en clúster. Cuando el usuario acerca el mapa, los clústeres se separan en sus puntos de datos individuales.

En los siguientes ejemplos, el código carga una fuente GeoJSON de datos de terremotos de la semana pasada y la agrega al mapa. Los clústeres se representan como círculos con una escala y color que dependen del número de puntos que contengan.

> [!NOTE]
> Hay varios algoritmos diferentes que se usan para la agrupación en clústeres de marcadores. Google y Azure Maps usan algoritmos ligeramente diferentes. Por lo tanto, a veces la distribución de puntos en los clústeres puede variar.

**Antes: Google Maps**

En Google Maps, los marcadores se pueden agrupar en clústeres mediante la carga en la biblioteca MarkerClusterer. Los iconos de clúster se limitan a imágenes que tienen los números del uno al cinco como su nombre y se hospedan en el mismo directorio.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(20, -160),
                zoom: 2
            });

            //Download the GeoJSON data.
            fetch(earthquakeFeed)
                .then(function (response) {
                    return response.json();
                }).then(function (data) {
                    //Loop through the GeoJSON data and create a marker for each data point.
                    var markers = [];

                    for (var i = 0; i < data.features.length; i++) {

                        markers.push(new google.maps.Marker({
                            position: new google.maps.LatLng(data.features[i].geometry.coordinates[1], data.features[i].geometry.coordinates[0])
                        }));
                    }

                    //Create a marker clusterer instance and tell it where to find the cluster icons.
                    var markerCluster = new MarkerClusterer(map, markers,
                        { imagePath: 'https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/m' });
                });
        }
    </script>

    <!-- Load the marker cluster library. -->
    <script src="https://developers.google.com/maps/documentation/javascript/examples/markerclusterer/markerclusterer.js"></script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Agrupación en clústeres de Google Maps](media/migrate-google-maps-web-app/google-maps-clustering.png)</center>

**Después: Azure Maps**

En Azure Maps, un origen de datos agrega y administra los datos. Las capas se conectan a los orígenes de datos y representan los datos en ellas. La clase `DataSource` en Azure Maps proporciona varias opciones de agrupación en clústeres.

- `cluster`: indica al origen de datos que agrupe en clústeres los datos de punto.
- `clusterRadius`: el radio en píxeles de los puntos de clúster juntos.
- `clusterMaxZoom`: el nivel de zoom máximo en el que se produce la agrupación en clústeres. Si amplía más allá de este nivel, todos los puntos se representan como símbolos.
- `clusterProperties`: define las propiedades personalizadas que se calculan mediante expresiones en todos los puntos de cada clúster y se agregan a las propiedades de cada punto de clúster.

Cuando la agrupación en clústeres está habilitada, el origen de datos enviará puntos de datos agrupados y no agrupados en clústeres a capas para su representación. El origen de datos es capaz de agrupar en clústeres cientos de miles de puntos de datos. Un punto de datos en clúster contiene las siguientes propiedades:

| Nombre de propiedad             | Tipo    | Descripción   |
|---------------------------|---------|---------------|
| `cluster`                 | boolean | Indica si la característica representa un clúster. |
| `cluster_id`              | string  | Un id. exclusivo para el clúster que se puede usar con los métodos `getClusterExpansionZoom`, `getClusterChildren` y `getClusterLeaves` de DataSource. |
| `point_count`             | number  | El número de puntos que contiene el clúster.  |
| `point_count_abbreviated` | string  | Una cadena que abrevia el valor de `point_count`, si es largo (por ejemplo, 4000 se convierte en 4 K).  |

La clase `DataSource` tiene la siguiente función auxiliar para acceder a información adicional sobre un clúster mediante `cluster_id`.

| Método | Tipo de valor devuelto | Descripción |
|--------|-------------|-------------|
| `getClusterChildren(clusterId: number)` | Promesa&lt;Matriz&lt;Característica&lt;Geometría, cualquiera&gt;\| Forma&gt;&gt; | Recupera los elementos secundarios del clúster especificado en el siguiente nivel de zoom. Estos elementos secundarios pueden ser una combinación de formas y subclústeres. Los subclústeres serán características con propiedades que coincidan con ClusteredProperties. |
| `getClusterExpansionZoom(clusterId: number)` | Promesa&lt;número&gt; | Calcula un nivel de zoom en el que el clúster empezará a expandirse o separarse. |
| `getClusterLeaves(clusterId: number, limit: number, offset: number)` | Promesa&lt;Matriz&lt;Característica&lt;Geometría, cualquiera&gt;\| Forma&gt;&gt; | Recupera todos los puntos de un clúster. Establezca `limit` para que devuelva un subconjunto de los puntos y use `offset` para paginar a través de los puntos. |

Al representar los datos en clúster en el mapa, a menudo es más fácil usar dos o más capas. En el ejemplo siguiente se usan tres capas. Una capa de burbujas para dibujar círculos de color a escala en función del tamaño de los clústeres. Una capa de símbolos para representar el tamaño del clúster como texto. Además, utiliza una segunda capa de símbolos para representar los puntos no agrupados. Hay muchas otras maneras de representar datos en clúster. Para más información, consulte la documentación [Datos del punto de clúster](clustering-point-data-web-sdk.md).

Los datos GeoJSON se pueden importar directamente a Azure Maps mediante la función `importDataFromUrl` de la clase `DataSource`.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map, datasource;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson';

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-160, 20],
                zoom: 1,

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource(null, {
                    //Tell the data source to cluster point data.
                    cluster: true
                });
                map.sources.add(datasource);

                //Create layers for rendering clusters, their counts and unclustered points and add the layers to the map.
                map.layers.add([
                    //Create a bubble layer for rendering clustered data points.
                    new atlas.layer.BubbleLayer(datasource, null, {
                        //Scale the size of the clustered bubble based on the number of points inthe cluster.
                        radius: [
                            'step',
                            ['get', 'point_count'],
                            20,         //Default of 20 pixel radius.
                            100, 30,    //If point_count >= 100, radius is 30 pixels.
                            750, 40     //If point_count >= 750, radius is 40 pixels.
                        ],

                        //Change the color of the cluster based on the value on the point_cluster property of the cluster.
                        color: [
                            'step',
                            ['get', 'point_count'],
                            'lime',            //Default to lime green. 
                            100, 'yellow',     //If the point_count >= 100, color is yellow.
                            750, 'red'         //If the point_count >= 750, color is red.
                        ],
                        strokeWidth: 0,
                        filter: ['has', 'point_count'] //Only rendered data points which have a point_count property, which clusters do.
                    }),

                    //Create a symbol layer to render the count of locations in a cluster.
                    new atlas.layer.SymbolLayer(datasource, null, {
                        iconOptions: {
                            image: 'none' //Hide the icon image.
                        },
                        textOptions: {
                            textField: ['get', 'point_count_abbreviated'],
                            offset: [0, 0.4]
                        }
                    }),

                    //Create a layer to render the individual locations.
                    new atlas.layer.SymbolLayer(datasource, null, {
                        filter: ['!', ['has', 'point_count']] //Filter out clustered points from this layer.
                    })
                ]);

                //Retrieve a GeoJSON data set and add it to the data source. 
                datasource.importDataFromUrl(earthquakeFeed);
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Agrupación en clústeres de Azure Maps](media/migrate-google-maps-web-app/azure-maps-clustering.png)</center>

**Recursos adicionales:**

- [Adición de una capa de símbolos](map-add-pin.md)
- [Adición de una capa de burbujas](map-add-bubble-layer.md)
- [Datos de punto de clúster](clustering-point-data-web-sdk.md)
- [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)

### <a name="add-a-heat-map"></a>Adición de un mapa térmico

Los mapas térmicos, también conocidos como mapas de densidad de puntos, son un tipo de visualización de datos. Se usan para representar la densidad de datos mediante una gama de colores. Y, a menudo, se usan para mostrar las "zonas activas" de datos en un mapa. Los mapas térmicos son una excelente manera de representar conjuntos de datos de puntos de gran tamaño.

En los siguientes ejemplos se carga una fuente GeoJSON de todos los terremotos del último mes de los USGS y se representan como un mapa térmico ponderado en el que se usa la propiedad `"mag"` como la ponderación.

**Antes: Google Maps**

En Google Maps, para crear un mapa térmico es necesario cargar la biblioteca "visualization" agregando `&libraries=visualization` a la dirección URL del script de la API. La capa de mapa térmico de Google Maps no admite directamente datos GeoJSON. Los datos deben descargarse primero y convertirse en una matriz de puntos de datos ponderados.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson';

        function initMap() {

            var map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(20, -160),
                zoom: 2,
                mapTypeId: 'satellite'
            });

            //Download the GeoJSON data.
            fetch(url).then(function (response) {
                return response.json();
            }).then(function (res) {
                var points = getDataPoints(res);

                var heatmap = new google.maps.visualization.HeatmapLayer({
                    data: points
                });
                heatmap.setMap(map);
            });
        }

        function getDataPoints(geojson, weightProp) {
            var points = [];

            for (var i = 0; i < geojson.features.length; i++) {
                var f = geojson.features[i];

                if (f.geometry.type === 'Point') {
                    points.push({
                        location: new google.maps.LatLng(f.geometry.coordinates[1], f.geometry.coordinates[0]),
                        weight: f.properties[weightProp]
                    });
                }
            }

            return points;
        } 
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]&libraries=visualization" async defer></script>
</head>
<body>
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Mapa térmico de Google Maps](media/migrate-google-maps-web-app/google-maps-heatmap.png)</center>

**Después: Azure Maps**

En Azure Maps, cargue los datos GeoJSON en un origen de datos y conecte el origen de datos a una capa del mapa térmico. La propiedad que se usará para la ponderación se puede pasar a la opción `weight` mediante una expresión. Los datos GeoJSON se pueden importar directamente a Azure Maps mediante la función `importDataFromUrl` de la clase `DataSource`.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;
        var earthquakeFeed = 'https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_month.geojson';

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-160, 20],
                zoom: 1,
                style: 'satellite',

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create a data source and add it to the map.
                datasource = new atlas.source.DataSource();
                map.sources.add(datasource);

                //Load the earthquake data.
                datasource.importDataFromUrl(earthquakeFeed);

                //Create a layer to render the data points as a heat map.
                map.layers.add(new atlas.layer.HeatMapLayer(datasource, null, {
                    weight: ['get', 'mag'],
                    intensity: 0.005,
                    opacity: 0.65,
                    radius: 10
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Mapa térmico de Azure Maps](media/migrate-google-maps-web-app/azure-maps-heatmap.png)</center>

**Recursos adicionales:**

- [Adición de una capa de mapa térmico](map-add-heat-map-layer.md)
- [Clase de capa de mapa térmico](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.heatmaplayer?view=azure-iot-typescript-latest)
- [Opciones de capa de mapa térmico](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)
- [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)

### <a name="overlay-a-tile-layer"></a>Superposición de una capa de mosaicos

Las capas de mosaicos también se conocen como superposiciones de imágenes en Google Maps. Las capas de mosaicos permiten superponer imágenes grandes que se han dividido en imágenes en mosaico más pequeñas y que se alinean con el sistema de mosaicos de los mapas. Este enfoque se usa normalmente para superponer imágenes grandes o grandes conjuntos de datos.

En los ejemplos siguientes se superpone una capa de mosaicos de radar meteorológico de Iowa Environmental Mesonet of Iowa State University.

**Antes: Google Maps**

En Google Maps, las capas de mosaicos se pueden crear mediante la clase `google.maps.ImageMapType`.

```javascript
map.overlayMapTypes.insertAt(0, new google.maps.ImageMapType({
    getTileUrl: function (coord, zoom) {
        return "https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/" + zoom + "/" + coord.x + "/" + coord.y;
    },
    tileSize: new google.maps.Size(256, 256),
    opacity: 0.8
}));
```

<center>

![Capa de mosaicos de Google Maps](media/migrate-google-maps-web-app/google-maps-tile-layer.png)</center>

**Después: Azure Maps**

En Azure Maps, se puede agregar una capa de mosaicos al mapa de la misma manera que cualquier otra capa. Se usa una dirección URL con formato que tiene marcadores de posición X, Y y zoom (`{x}`, `{y}`, `{z}` respectivamente) para indicar a la capa dónde acceder a los mosaicos. Las capas de mosaicos de Azure Maps también admiten los marcadores de posición `{quadkey}`, `{bbox-epsg-3857}` y `{subdomain}`.

> [!TIP]
> En Azure Maps las capas se pueden representar fácilmente bajo otras capas, incluidas las capas base del mapa. A menudo es conveniente representar las capas de mosaicos debajo de las etiquetas del mapa para que resulten fáciles de leer. El método `map.layers.add` toma un segundo parámetro que es el identificador de la capa en la que se va a insertar la siguiente capa nueva. Para insertar una capa de mosaicos debajo de las etiquetas del mapa se puede usar el código siguiente: `map.layers.add(myTileLayer, "labels");`

```javascript
//Create a tile layer and add it to the map below the label layer.
map.layers.add(new atlas.layer.TileLayer({
    tileUrl: 'https://mesonet.agron.iastate.edu/cache/tile.py/1.0.0/nexrad-n0q-900913/{z}/{x}/{y}.png',
    opacity: 0.8,
    tileSize: 256
}), 'labels');
```

<center>

![Capa de mosaicos de Azure Maps](media/migrate-google-maps-web-app/azure-maps-tile-layer.png)</center>

> [!TIP]
> Las solicitudes de mosaicos se pueden capturar con la opción `transformRequest` del mapa. Esto le permitirá modificar o agregar encabezados a la solicitud si lo desea.

**Recursos adicionales:**

- [Adición de capas de mosaicos](map-add-tile-layer.md)
- [Clase de capa de mosaico](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)
- [Opciones de capa de mosaico](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest)

### <a name="show-traffic"></a>Visualización del tráfico

Los datos de tráfico se pueden superponer en Azure Maps y Google Maps.

**Antes: Google Maps**

En Google Maps, los datos de tráfico se pueden superponer en el mapa con la capa de tráfico.

```javascript
var trafficLayer = new google.maps.TrafficLayer();
trafficLayer.setMap(map);
```

<center>

![Tráfico de Google Maps](media/migrate-google-maps-web-app/google-maps-traffic.png)</center>

**Después: Azure Maps**

Azure Maps proporciona varias opciones diferentes para mostrar el tráfico. Los incidentes de tráfico, como los cierres de carreteras y los accidentes, se pueden mostrar como iconos en el mapa. El flujo de tráfico o las carreteras codificadas mediante colores se pueden superponer en el mapa y los colores se pueden modificar para que se basen en el límite de velocidad registrado, el retraso normal esperado o el retraso absoluto. Los datos de incidentes en Azure Maps se actualizan cada minuto y los datos de flujo se actualizan cada dos minutos.

```javascript
map.setTraffic({
    incidents: true,
    flow: 'relative'
});
```

<center>

![Tráfico de Azure Maps](media/migrate-google-maps-web-app/azure-maps-traffic.png)</center>

Si hace clic en uno de los iconos de tráfico de Azure Maps, se mostrará información adicional en un menú emergente.

<center>

![Incidente de tráfico en Azure Maps](media/migrate-google-maps-web-app/azure-maps-traffic-incident.png)</center>

**Recursos adicionales:**

- [Visualización del tráfico en el mapa](map-show-traffic.md)
- [Opciones de superposición de tráfico](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Traffic%20Overlay%20Options)

### <a name="add-a-ground-overlay"></a>Adición de una superposición de suelo

Tanto Azure como Google Maps admiten la superposición de imágenes georreferenciadas en el mapa para que se muevan y escalen a medida que se desplaza lateralmente y se amplía el mapa. En Google Maps se conocen como superposiciones de suelo mientras que en Azure Maps se conocen como capas de imagen. Son excelentes para la creación de planos de piso, la superposición de mapas antiguos o las imágenes de un dron.

**Antes: Google Maps**

Al crear una superposición de suelo en Google Maps, debe especificar la dirección URL de la imagen que se va a superponer y un rectángulo delimitador para enlazar la imagen en el mapa. En este ejemplo se superpone una imagen de mapa de [Newark New Jersey de 1922](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) en el mapa.

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <script type='text/javascript'>
        var map, historicalOverlay;

        function initMap() {
            map = new google.maps.Map(document.getElementById('myMap'), {
                center: new google.maps.LatLng(40.740, -74.18),
                zoom: 12
            });

            var imageBounds = {
                north: 40.773941,
                south: 40.712216,
                east: -74.12544,
                west: -74.22655
            };

            historicalOverlay = new google.maps.GroundOverlay(
                'https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg',
                imageBounds);
            historicalOverlay.setMap(map);
        }
    </script>

    <!-- Google Maps Script Reference -->
    <script src="https://maps.googleapis.com/maps/api/js?callback=initMap&key=[Your Google Maps Key]" async defer></script>
</head>
<body>
    <div id="myMap" style="position:relative;width:600px;height:400px;"></div>
</body>
</html>
```

Al ejecutar este código en un explorador, se mostrará un mapa similar al de la siguiente imagen:

<center>

![Superposición de imágenes de Google Maps](media/migrate-google-maps-web-app/google-maps-image-overlay.png)</center>

**Después: Azure Maps**

En Azure Maps, las imágenes georreferenciadas pueden superponerse mediante la clase `atlas.layer.ImageLayer`. Esta clase requiere una dirección URL para una imagen y un conjunto de coordenadas para las cuatro esquinas de la imagen. La imagen debe estar hospedada en el mismo dominio o tener COR habilitados.

> [!TIP]
> Si solo tiene información de norte, sur, este, oeste y giro, y no hay ninguna coordenada para cada esquina de la imagen, puede usar el método estático [`atlas.layer.ImageLayer.getCoordinatesFromEdges`](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest#getcoordinatesfromedges-number--number--number--number--number-).

```html
<!DOCTYPE html>
<html>
<head>
    <title></title>
    <meta charset="utf-8" />
    <meta http-equiv="x-ua-compatible" content="IE=Edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no" />

    <!-- Add references to the Azure Maps Map control JavaScript and CSS files. -->
    <link rel="stylesheet" href="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.css" type="text/css" />
    <script src="https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas.min.js"></script>

    <script type='text/javascript'>
        var map;

        function initMap() {
            //Initialize a map instance.
            map = new atlas.Map('myMap', {
                center: [-74.18, 40.740],
                zoom: 12,

                //Add your Azure Maps subscription key to the map SDK. Get an Azure Maps key at https://azure.com/maps
                authOptions: {
                    authType: 'subscriptionKey',
                    subscriptionKey: '<Your Azure Maps Key>'
                }
            });

            //Wait until the map resources are ready.
            map.events.add('ready', function () {

                //Create an image layer and add it to the map.
                map.layers.add(new atlas.layer.ImageLayer({
                    url: 'newark_nj_1922.jpg',
                    coordinates: [
                        [-74.22655, 40.773941], //Top Left Corner
                        [-74.12544, 40.773941], //Top Right Corner
                        [-74.12544, 40.712216], //Bottom Right Corner
                        [-74.22655, 40.712216]  //Bottom Left Corner
                    ]
                }));
            });
        }
    </script>
</head>
<body onload="initMap()">
    <div id='myMap' style='position:relative;width:600px;height:400px;'></div>
</body>
</html>
```

<center>

![Superposición de imágenes de Azure Maps](media/migrate-google-maps-web-app/azure-maps-image-overlay.png)</center>

**Recursos adicionales:**

- [Superposición de una imagen](map-add-image-layer.md)
- [Clase de capa de imagen](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

## <a name="additional-code-samples"></a>Ejemplos de código adicionales

A continuación, se muestran algunos ejemplos de código adicionales relacionados con la migración de Google Maps:

- [Herramientas de dibujo](map-add-drawing-toolbar.md)
- [Limitar el plano a dos desplazamientos laterales de dedo](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Limit%20Map%20to%20Two%20Finger%20Panning)
- [Limitar el zoom de rueda del mouse de desplazamiento](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Limit%20Scroll%20Wheel%20Zoom)
- [Crear un control de pantalla completa](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Create%20a%20Fullscreen%20Control)

**Servicios:**

- [Uso del módulo de servicios de Azure Maps](how-to-use-services-module.md)
- [Búsqueda de puntos de interés](map-search-location.md)
- [Obtención de información de una coordenada (geocódigo invertido)](map-get-information-from-coordinate.md)
- [Presentación de indicaciones de ruta de A a B](map-route.md)
- [Buscar sugerencias automáticas con la interfaz de usuario JQuery](https://azuremapscodesamples.azurewebsites.net/index.html?sample=Search%20Autosuggest%20and%20JQuery%20UI)

## <a name="google-maps-v3-to-azure-maps-web-sdk-class-mapping"></a>Asignación de clases de Google Maps V3 al SDK web de Azure Maps

En el siguiente apéndice se proporciona una referencia cruzada de las clases que se usan con más frecuencia en Google Maps V3 y sus equivalentes del SDK web de Azure Maps.

### <a name="core-classes"></a>Clases principales

| Google Maps   | Azure Maps  |
|---------------|-------------|
| `google.maps.Map` | [atlas.Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)  |
| `google.maps.InfoWindow` | [atlas.Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)  |
| `google.maps.InfoWindowOptions` | [atlas.PopupOptions](https://docs.microsoft.com/) |
| `google.maps.LatLng`  | [atlas.data.Position](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.position?view=azure-iot-typescript-latest)  |
| `google.maps.LatLngBounds` | [atlas.data.BoundingBox](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) |
| `google.maps.MapOptions`  | [atlas.CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraoptions?view=azure-iot-typescript-latest)<br/>[atlas.CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.cameraboundsoptions?view=azure-iot-typescript-latest)<br/>[atlas.ServiceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.serviceoptions?view=azure-iot-typescript-latest)<br/>[atlas.StyleOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.styleoptions?view=azure-iot-typescript-latest)<br/>[atlas.UserInteractionOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.userinteractionoptions?view=azure-iot-typescript-latest) |
| `google.maps.Point`  | [atlas.Pixel](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.pixel?view=azure-iot-typescript-latest)   |

## <a name="overlay-classes"></a>Clases de superposición

| Google Maps  | Azure Maps  |
|--------------|-------------|
| `google.maps.Marker` | [atlas.HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)<br/>[atlas.data.Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest)  |
| `google.maps.MarkerOptions`  | [atlas.HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)<br/>[atlas.layer.SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)<br/>[atlas.SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)<br/>[atlas.IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)<br/>[atlas.TextOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)<br/>[atlas.layer.BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)<br/>[atlas.BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest) |
| `google.maps.Polygon`  | [atlas.data.Polygon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)               |
| `google.maps.PolygonOptions` |[atlas.layer.PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest)<br/> [atlas.PolygonLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest)<br/> [atlas.layer.LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest)<br/> [atlas.LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest)|
| `google.maps.Polyline` | [atlas.data.LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest)         |
| `google.maps.PolylineOptions` | [atlas.layer.LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-maps-typescript-latest)<br/>[atlas.LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-maps-typescript-latest) |
| `google.maps.Circle`  | Vea [Adición de un círculo al mapa](map-add-shape.md#add-a-circle-to-the-map)                                     |
| `google.maps.ImageMapType`  | [atlas.TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)         |
| `google.maps.ImageMapTypeOptions` | [atlas.TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.tilelayeroptions?view=azure-iot-typescript-latest) |
| `google.maps.GroundOverlay`  | [atlas.layer.ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)<br/>[atlas.ImageLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest) |

## <a name="service-classes"></a>Clases de servicio

El SDK web de Azure Maps incluye un módulo de servicios que se puede cargar por separado. Este módulo encapsula los servicios REST de Azure Maps con una API web y se puede usar en aplicaciones JavaScript, TypeScript y Node.js.

| Google Maps | Azure Maps  |
|-------------|-------------|
| `google.maps.Geocoder` | [atlas.service.SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)  |
| `google.maps.GeocoderRequest`  | [atlas.SearchAddressOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAddressRevrseOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreverseoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAddressReverseCrossStreetOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressreversecrossstreetoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAddressStructuredOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchaddressstructuredoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchAlongRouteOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchalongrouteoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchFuzzyOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchfuzzyoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchInsideGeometryOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchinsidegeometryoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchNearbyOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchnearbyoptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchPOIOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoioptions?view=azure-iot-typescript-latest)<br/>[atlas.SearchPOICategoryOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchpoicategoryoptions?view=azure-iot-typescript-latest) |
| `google.maps.DirectionsService`  | [atlas.service.RouteUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest)  |
| `google.maps.DirectionsRequest`  | [atlas.CalculateRouteDirectionsOptions](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.calculateroutedirectionsoptions?view=azure-iot-typescript-latest) |
| `google.maps.places.PlacesService` | [atlas.service.SearchUrl](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest)  |

## <a name="libraries"></a>Bibliotecas

Las bibliotecas agregan funcionalidad adicional al mapa. Muchos de ellos se encuentran en el SDK básico de Azure Maps. Estas son algunas clases equivalentes que se pueden usar en lugar de estas bibliotecas de Google Maps

| Google Maps           | Azure Maps   |
|-----------------------|--------------|
| Biblioteca de dibujo       | [Módulo de herramientas de dibujo](set-drawing-options.md) |
| Biblioteca de geometría      | [atlas.math](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math?view=azure-iot-typescript-latest)   |
| Biblioteca de visualización | [Capa de mapa térmico](map-add-heat-map-layer.md) |

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre el SDK web de Azure Maps.

> [!div class="nextstepaction"]
> [Uso del control de mapa](how-to-use-map-control.md)

> [!div class="nextstepaction"]
> [Uso del módulo de servicios](how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Uso del módulo de herramientas de dibujo](set-drawing-options.md)

> [!div class="nextstepaction"]
> [Ejemplos de código](https://docs.microsoft.com/samples/browse/?products=azure-maps)

