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
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59547992"
---
# <a name="add-a-shape-to-a-map"></a>Adición de una forma a un mapa

Este artículo muestra cómo representar geometrías en el mapa con capas de líneas y polígonos. El SDK Web de mapas de Azure también admite la creación de geometrías de círculo tal como se define en el [esquema extendido de GeoJSON](extend-geojson.md#circle). Todas las geometrías de característica también se pueden fácilmente actualizar si el ajusta con la [forma](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) clase.

<a id="addALine"></a>

## <a name="add-lines-to-the-map"></a>Agregar líneas al mapa

`LineString` y `MultiLineString` características se utilizan para representar las rutas de acceso y se describen en el mapa.

### <a name="add-a-line"></a>Adición de una línea

<iframe height='500' scrolling='no' title='Adición de una línea a un mapa' src='//codepen.io/azuremaps/embed/qomaKv/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qomaKv/'>Adición de una línea a un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código del código anterior construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Se crea un objeto [LineString](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.linestring?view=azure-iot-typescript-latest) y se agrega al origen de datos.

Una clase [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) presenta objetos de línea encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). El último bloque de código crea una capa de línea y la agrega al mapa. Puede consultar las propiedades de una capa de líneas en [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). El origen de datos y la capa de línea se crean y agregan a la asignación dentro de la [controlador de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que se muestra la línea después de que el mapa se cargue completamente.

### <a name="add-symbols-along-a-line"></a>Agregar símbolos a lo largo de una línea

Este ejemplo muestra cómo agregar iconos de flecha situados a lo largo de una línea en el mapa. Cuando el uso de una capa de símbolos, Establece la opción "selección" a "línea", se representan los símbolos a lo largo de la línea y girar los iconos (0 grados = right).

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Mostrar la flecha situada junto a línea" src="//codepen.io/azuremaps/embed/drBJwX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/drBJwX/'>flecha mostrar a lo largo de la línea</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="line-stroke-gradient"></a> Agregar un degradado de trazo a una línea

Además de poder aplicar un color de trazo único a una línea también puede rellenar una línea con un degradado de colores para mostrar la transición de un segmento de línea a la siguiente. Por ejemplo, los degradados de línea pueden utilizarse para representar los cambios en el tiempo y distancia o las temperaturas diferentes a través de una línea de objetos conectados. Para poder aplicar esta característica a una línea, el origen de datos debe tener la `lineMetrics` opción establecido en true y, a continuación, se puede pasar una expresión de degradado de color a la `strokeColor` opción de la línea. La expresión de degradado del trazo tiene para referencia el `['line-progress']` expresión de datos que expone las métricas de línea calculado a la expresión.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Línea de degradado de trazo" src="//codepen.io/azuremaps/embed/wZwWJZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/wZwWJZ/'>línea con trazo degradado</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="customize-a-line-layer"></a>Personalizar una capa de línea

La capa de línea tiene varias opciones de estilo. Esta es una herramienta para probarlas.

<br/>

<iframe height='700' scrolling='no' title='Opciones de capa de línea' src='//codepen.io/azuremaps/embed/GwLrgb/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/GwLrgb/'>Opciones de capa de línea</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addAPolygon"></a>

## <a name="add-a-polygon-to-the-map"></a>Adición de un polígono al mapa

`Polygon` y `MultiPolygon` características a menudo se utilizan para representar un área en un mapa. 

### <a name="use-a-polygon-layer"></a>Usar una capa de polígono 

Una capa de polígono representa el área de un polígono. 

<iframe height='500' scrolling='no' title='Adición de un polígono a un mapa ' src='//codepen.io/azuremaps/embed/yKbOvZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/yKbOvZ/'>Adición de un polígono a un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Un [polígono](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) se crea a partir de una matriz de coordenadas y se agrega al origen de datos. 

En el mapa, un objeto [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) presenta los datos encapsulados en la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). El último bloque de código crea una capa de polígono y la agrega al mapa. Consulte las propiedades de una capa de polígonos en [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). El origen de datos y la capa de polígono se crean y agregan a la asignación dentro de la [controlador de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que el polígono se muestra después de que el mapa se cargue completamente.

### <a name="use-a-polygon-and-line-layer-together"></a>Usar conjuntamente una capa de polígono y línea

Una capa de línea se puede usar para presentar el contorno de un polígono. 

<iframe height='500' scrolling='no' title='Capas de polígono y línea para agregar un polígono' src='//codepen.io/azuremaps/embed/aRyEPy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen sobre <a href='https://codepen.io/azuremaps/pen/aRyEPy/'>capas de polígono y línea para agregar un polígono</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Un [polígono](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest) se crea a partir de una matriz de coordenadas y se agrega al origen de datos. 

En el mapa, un objeto [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) presenta los datos encapsulados en la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Consulte las propiedades de una capa de polígonos en [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). Una clase [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) es una matriz de líneas. Puede consultar las propiedades de una capa de líneas en [LineLayerOptions](/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest). El tercer bloque de código crea las capas de polígono y línea.

El último bloque de código agrega las capas de polígono y línea al mapa. El origen de datos y las capas se crean y agregan a la asignación dentro de la [controlador de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que el polígono se muestra después de que el mapa se cargue completamente.

> [!TIP]
> Las capas de la línea de forma predeterminada representarán las coordenadas de polígonos, así como líneas en un origen de datos. Para limitar la capa de forma que solo se representa LineString características conjunto el `filter` propiedad de la capa a `['==', ['geometry-type'], 'LineString']` o `['any', ['==', ['geometry-type'], 'LineString'], ['==', ['geometry-type'], 'MultiLineString']]` si desea incluir también las características de MultiLineString.

### <a name="fill-a-polygon-with-a-pattern"></a>Rellenar un polígono con un patrón

Además de rellenar un polígono con un color también se puede usar un patrón de la imagen. Cargar un patrón de imagen en los recursos de mapas de imágenes sprite y, a continuación, hacer referencia a esta imagen con el `fillPattern` propiedad de la capa de polígono.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Trama de relleno de polígono" src="//codepen.io/azuremaps/embed/JzQpYX/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/JzQpYX/'>trama de relleno de polígono</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="customize-a-polygon-layer"></a>Personalizar una capa de polígono

La capa de polígono solo tiene algunas opciones de estilo. Esta es una herramienta para probarlas.

<br/>

<iframe height='700' scrolling='no' title='LXvxpg' src='//codepen.io/azuremaps/embed/LXvxpg/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/LXvxpg/'>LXvxpg</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="addACircle"></a>

## <a name="add-a-circle-to-the-map"></a>Adición de un círculo al mapa

Azure Maps usa una versión extendida del esquema GeoJSON que proporciona una definición para círculos como se indicó [aquí](extend-geojson.md#circle). Un círculo puede representarse en el mapa mediante la creación de un `Point` característica que tiene un `subType` propiedad con un valor de `"Circle"` y un `radius` propiedad que tiene un número que represente el radio en metros. Por ejemplo: 

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

El SDK de Azure Maps Web convierte estos `Pooint` características en `Polygon` características en segundo plano y se puede representar en el mapa con capas de polígono y línea como se muestra aquí.

<iframe height='500' scrolling='no' title='Adición de un círculo a un mapa' src='//codepen.io/azuremaps/embed/PRmzJX/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/PRmzJX/'>Adición de un círculo a un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código del código anterior construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Un círculo es un [característica](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) de [punto](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) y tiene un `subType` propiedad establecida en `"Circle"` y un `radius` valor de propiedad en metros. Cuando una característica de punto con un `subType` de `"Circle"` se agrega a un origen de datos, convierte en un polígono circular dentro del mapa.

En el mapa, un objeto [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) presenta los datos encapsulados en la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). El último bloque de código crea una capa de polígono y la agrega al mapa. Consulte las propiedades de una capa de polígonos en [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). El origen de datos y la capa de polígono se crean y agregan a la asignación dentro de la [controlador de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que el círculo se muestra después de que el mapa se cargue completamente.

## <a name="make-a-geometry-easy-to-update"></a>Facilitar una geometría actualizar

Un `Shape` clase ajusta un [geometría](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.geometry?view=azure-iot-typescript-latest) o [característica](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) y facilita la actualización y mantenerlos.
`new Shape(data: Feature<data.Geometry, any>)` construye un objeto Shape y lo inicializa con la característica especificada.

<br/>

<iframe height='500' scrolling='no' title='Actualización de propiedades de forma' src='//codepen.io/azuremaps/embed/ZqMeQY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen sobre <a href='https://codepen.io/azuremaps/pen/ZqMeQY/'>actualización del estilo</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el primer bloque de código anterior se construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

Un punto es un objeto [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) de la clase [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest). El segundo bloque de código inicializa el valor de radio del elemento del control deslizante HTML y luego construye un objeto de punto y lo encapsula en un objeto de clase [Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest).

El tercer bloque de código crea una función que toma el valor del elemento de control deslizante del intervalo HTML y cambia el valor del radio con el método [addProperty](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) de la clase Shape.

En el cuarto bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Luego se agrega el punto al origen de datos.

En el mapa, un objeto [PolygonLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonlayer?view=azure-iot-typescript-latest) presenta los datos encapsulados en la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). El tercer bloque de código crea una capa de polígono. Consulte las propiedades de una capa de polígonos en [PolygonLayerOptions](/javascript/api/azure-maps-control/atlas.polygonlayeroptions?view=azure-iot-typescript-latest). El origen de datos, el controlador de eventos click y la capa de polígono se crean y agregan a la asignación dentro de la [controlador de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que se muestra el punto después de que el mapa se cargue completamente.

## <a name="next-steps"></a>Pasos siguientes

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos:

> [!div class="nextstepaction"]
> [Usar expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)
