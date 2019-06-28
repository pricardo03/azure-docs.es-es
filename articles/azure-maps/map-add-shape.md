---
title: Adición de una forma con Azure Maps | Microsoft Docs
description: Procedimiento para agregar una forma a un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: f61c7a939902ee5d02b2e9ba896c7555968f9d0d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60769522"
---
# <a name="add-a-shape-to-a-map"></a>Adición de una forma a un mapa

En este artículo se muestra cómo representar geometrías en el mapa con capas de líneas y polígonos. El SDK web de Azure Maps también admite la creación de geometrías de círculo tal como se define en el [esquema extendido de GeoJSON](extend-geojson.md#circle). Todas las geometrías de características también se pueden actualizar fácilmente si se ajustan con la clase [Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest).

<a id="addALine"></a>

## <a name="add-lines-to-the-map"></a>Adición de líneas al mapa

Las características `LineString` y `MultiLineString` se utilizan para representar rutas y contornos en el mapa.

### <a name="add-a-line"></a>Adición de una línea

<iframe height='500' scrolling='no' title='Adición de una línea a un mapa' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>Adición de una línea a un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código del código anterior construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Se crea un objeto [LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest) y se agrega al origen de datos.

Una clase [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) presenta objetos de línea encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). El último bloque de código crea una capa de línea y la agrega al mapa. Puede consultar las propiedades de una capa de líneas en [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). Se crean el origen de datos y la capa de línea, y se agregan al mapa en el [controlador de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que la línea se muestra una vez que el mapa se cargue completamente.

### <a name="add-symbols-along-a-line"></a>Adición de símbolos a lo largo de una línea

En este ejemplo se muestra cómo agregar iconos de flecha situados a lo largo de una línea en el mapa. Cuando use una capa de símbolos, establezca la opción "colocación" en "línea" para que se representan los símbolos a lo largo de la línea y se giren los iconos (0 grados = derecha).

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Visualización de la flecha a lo largo de la línea" src="//codepen.io/azuremaps/embed/drBJwX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/drBJwX/'>Visualización de la flecha a lo largo de la línea</a> por Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="line-stroke-gradient"></a> Adición de un degradado de trazo a una línea

Además de poder aplicar un color de trazo único a una línea, también puede rellenar una línea con un degradado de colores para mostrar la transición de un segmento de línea al siguiente. Por ejemplo, los degradados de línea pueden utilizarse para representar cambios en el tiempo y en la distancia, o bien diferentes temperaturas a lo largo de una línea de objetos conectada. Para poder aplicar esta característica a una línea, el origen de datos debe tener la opción `lineMetrics` establecida en true; de este modo, se puede pasar una expresión de degradado a la opción `strokeColor` de la línea. La expresión de degradado del trazo tiene que hacer referencia a la expresión de datos `['line-progress']` que expone las métricas de línea calculadas a la expresión.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Línea con degradado de trazo" src="//codepen.io/azuremaps/embed/wZwWJZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/wZwWJZ/'>Línea con degradado de trazo</a> por Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="customize-a-line-layer"></a>Personalizar una capa de línea

La capa de línea tiene varias opciones de estilo. Esta es una herramienta para probarlas.

<br/>

<iframe height='700' scrolling='no' title='Opciones de capa de línea' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>Opciones de capa de línea</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addAPolygon"></a>

## <a name="add-a-polygon-to-the-map"></a>Adición de un polígono al mapa

Las características `Polygon` y `MultiPolygon` a menudo se utilizan para representar un área en un mapa. 

### <a name="use-a-polygon-layer"></a>Uso de una capa de polígono 

Una capa de polígono representa el área de un polígono. 

<iframe height='500' scrolling='no' title='Adición de un polígono a un mapa ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>Adición de un polígono a un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Un [polígono](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) se crea a partir de una matriz de coordenadas y se agrega al origen de datos. 

En el mapa, un objeto [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) presenta los datos encapsulados en la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). El último bloque de código crea una capa de polígono y la agrega al mapa. Consulte las propiedades de una capa de polígonos en [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Se crean el origen de datos y la capa de polígono, y se agregan al mapa en el [controlador de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que el polígono se muestra una vez que el mapa se cargue completamente.

### <a name="use-a-polygon-and-line-layer-together"></a>Uso de un polígono y una capa de línea conjuntamente

Una capa de línea se puede usar para presentar el contorno de un polígono. 

<iframe height='500' scrolling='no' title='Capas de polígono y línea para agregar un polígono' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen sobre <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>capas de polígono y línea para agregar un polígono</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Un [polígono](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) se crea a partir de una matriz de coordenadas y se agrega al origen de datos. 

En el mapa, un objeto [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) presenta los datos encapsulados en la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Consulte las propiedades de una capa de polígonos en [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Una clase [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) es una matriz de líneas. Puede consultar las propiedades de una capa de líneas en [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). El tercer bloque de código crea las capas de polígono y línea.

El último bloque de código agrega las capas de polígono y línea al mapa. Se crean el origen de datos y las capas, y se agregan al mapa en el [controlador de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que el polígono se muestra una vez que el mapa se cargue completamente.

> [!TIP]
> Las capas de línea de forma predeterminada representarán las coordenadas de los polígonos y las líneas en un origen de datos. Para limitar la capa de forma que solo represente las características de LineString, configure la propiedad `filter` de la capa en `['==', ['geometry-type'], 'LineString']` o `['any', ['==', ['geometry-type'], 'LineString'], ['==', ['geometry-type'], 'MultiLineString']]` si desea incluir también las características de MultiLineString.

### <a name="fill-a-polygon-with-a-pattern"></a>Relleno de un polígono con un patrón

Además de rellenar un polígono con un color, también se puede usar un patrón de imagen. Cargue un patrón de imagen en los recursos de sprite de la imagen de los mapas y luego haga referencia a esta imagen con la propiedad `fillPattern` de la capa de polígono.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Patrón de relleno poligonal" src="//codepen.io/azuremaps/embed/JzQpYX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/JzQpYX/'>Patrón de relleno poligonal</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="customize-a-polygon-layer"></a>Personalizar una capa de polígono

La capa de polígono solo tiene algunas opciones de estilo. Esta es una herramienta para probarlas.

<br/>

<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addACircle"></a>

## <a name="add-a-circle-to-the-map"></a>Adición de un círculo al mapa

Azure Maps usa una versión extendida del esquema GeoJSON que proporciona una definición para círculos como se indica [aquí](extend-geojson.md#circle). Un círculo puede representarse en el mapa mediante la creación de una característica `Point` que tiene una propiedad `subType` con un valor de `"Circle"` y una propiedad `radius` que tiene un número que representa el radio en metros. Por ejemplo:

```javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-122.126986, 47.639754]
    },
    "properties": {
        "subType": "Circle",
        "radius": 100
    }
}  
```

El SDK web de Azure Maps convierte estas características `Pooint` en características `Polygon` en segundo plano y se pueden representar en el mapa con capas de polígono y línea como se muestra aquí.

<iframe height='500' scrolling='no' title='Adición de un círculo a un mapa' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>Adición de un círculo a un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código del código anterior construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Un círculo es un objeto [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) de [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) y tiene una propiedad `subType` establecida en `"Circle"` y un valor de propiedad `radius` en metros. Cuando se agrega una característica de punto con un `subType` de `"Circle"` a un origen de datos, lo convierte en un polígono circular dentro del mapa.

En el mapa, un objeto [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) presenta los datos encapsulados en la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). El último bloque de código crea una capa de polígono y la agrega al mapa. Consulte las propiedades de una capa de polígonos en [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Se crean el origen de datos y la capa de polígono, y se agregan al mapa en el [controlador de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que el círculo se muestra una vez que el mapa se cargue completamente.

## <a name="make-a-geometry-easy-to-update"></a>Fácil actualización para geometría

Una clase `Shape` encapsula un objeto [Geometry](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest) o [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) y simplifica su actualización y mantenimiento.
`new Shape(data: Feature<data.Geometry, any>)` construye un objeto Shape y lo inicializa con la característica especificada.

<br/>

<iframe height='500' scrolling='no' title='Actualización de propiedades de forma' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen sobre <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>actualización del estilo</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el primer bloque de código anterior se construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

Un punto es un objeto [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) de la clase [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest). El segundo bloque de código inicializa el valor de radio del elemento del control deslizante HTML y luego construye un objeto de punto y lo encapsula en un objeto de clase [Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest).

El tercer bloque de código crea una función que toma el valor del elemento de control deslizante del intervalo HTML y cambia el valor del radio con el método [addProperty](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) de la clase Shape.

En el cuarto bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Luego se agrega el punto al origen de datos.

En el mapa, un objeto [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) presenta los datos encapsulados en la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). El tercer bloque de código crea una capa de polígono. Consulte las propiedades de una capa de polígonos en [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Se crean el origen de datos, el controlador de eventos de clic y la capa de polígono, y se agregan al mapa en el [controlador de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que el punto se muestra una vez que el mapa se cargue completamente.

## <a name="next-steps"></a>Pasos siguientes

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos:

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)
