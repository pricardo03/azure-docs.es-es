---
title: Incorporación de una capa de polígono de extrusión a Azure Maps | Microsoft Docs
description: Cómo agregar una capa de polígono de extrusión al SDK web de Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: e6858359549f6a54513eda7bc692adcbc7d7e71b
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74484331"
---
# <a name="add-an-extrusion-polygon-layer-to-the-map"></a>Incorporación de una capa de polígono de extrusión al mapa

En este artículo se muestra cómo usar la capa de extrusión de polígono para representar áreas de las geometrías de características `Polygon` y `MultiPolygon` como formas extruidas en el mapa. El SDK web de Azure Maps también admite la creación de geometrías de círculo tal como se define en el [esquema extendido de GeoJSON](extend-geojson.md#circle). Estos círculos se transforman en polígonos cuando se representan en el mapa. Todas las geometrías de características también se pueden actualizar fácilmente si se encapsulan con la clase [atlas.Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest).


## <a name="use-a-polygon-extrusion-layer"></a>Uso de una capa de extrusión de polígono

Cuando una [capa de extrusión de polígono](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest) se conecta al origen de datos y se carga en el mapa, representa las áreas de las características `Polygon` y `MultiPolygon` como formas extruidas. Las propiedades `height` y `base` de la capa de extrusión de polígono definen la distancia base desde el suelo y la altura de la forma extruida en **metros**. En el código siguiente se muestra cómo crear un polígono, agregarlo a un origen de datos y representarlo con la clase de la capa de extrusión de polígono.

> [!Note]
> El valor `base` definido en la capa de extrusión de polígono debe ser menor o igual al de `height`.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Polígono extruido" src="https://codepen.io/azuremaps/embed/wvvBpvE?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/wvvBpvE'>Polígono extruido</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.</iframe>


## <a name="add-data-driven-multipolygons"></a>Incorporación de multipolígonos controlados por datos

Un mapa de coropletas se puede representar mediante la capa de extrusión de polígono. Para ello, establezca sus propiedades `height` y `fillColor` en proporción a la medición de la variable estadística en las geometrías de características `Polygon` y `MultiPolygon`. En el ejemplo de código siguiente se muestra un mapa de coropletas extruido de los Estados Unidos basado en la medida de la densidad de población por estado.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Mapa de coropletas extruido" src="https://codepen.io/azuremaps/embed/eYYYNox?height=265&theme-id=0&default-tab=result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/eYYYNox'>Mapa de coropletas extruido</a> por Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-a-circle-to-the-map"></a>Adición de un círculo al mapa

Azure Maps usa una versión extendida del esquema GeoJSON que proporciona una definición para círculos como se indica [aquí](https://docs.microsoft.com/azure/azure-maps/extend-geojson#circle). Se puede representar un círculo en el mapa mediante la creación de una característica `point` que tiene una propiedad `subType` con un valor de `Circle` y una propiedad `Radius` numerada que representa el radio en **metros**. Por ejemplo:

```Javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-105.203135, 39.664087]
    },
    "properties": {
        "subType": "Circle",
        "radius": 1000
    }
} 
```

El SDK web de Azure Maps convierte estas características `Point` en características `Polygon` en segundo plano y se pueden representar en el mapa con una capa de extrusión de polígono como se muestra en el código de ejemplo siguiente.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Polígono de espacio aéreo de dron" src="https://codepen.io/azuremaps/embed/zYYYrxo?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/zYYYrxo'>Polígono de espacio aéreo de dron</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-polygon-extrusion-layer"></a>Personalización de una capa de extrusión de polígono

La capa de extrusión de polígono ofrece varias opciones de estilo. Esta es una herramienta para probarlas.

<br/>

<iframe height='700' scrolling='no' title='PoogBRJ' src='//codepen.io/azuremaps/embed/PoogBRJ/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/PoogBRJ/'>PoogBRJ</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Polygon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Capa de extrusión de polígono](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest)

Recursos adicionales:

> [!div class="nextstepaction"]
> [Extensión de la especificación GeoJSON de Azure Maps](extend-geojson.md#circle)
