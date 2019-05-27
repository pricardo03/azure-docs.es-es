---
title: Agrupación en clústeres de punto de datos en Azure Maps | Microsoft Docs
description: Cómo agrupar los datos del punto en el SDK Web
author: rbrundritt
ms.author: richbrun
ms.date: 03/27/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: 6dbd4461e7b8382ec3c4075b9688de59678f98f5
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/20/2019
ms.locfileid: "65957318"
---
# <a name="clustering-point-data"></a>Agrupación en clústeres punto de datos

Cuando se visualice muchos puntos de datos en el mapa, puntos superponen entre sí, la asignación presenta un aspecto desordenada y resulta difícil ver y usar. Agrupación en clústeres de punto de datos puede utilizarse para mejorar esta experiencia de usuario. Agrupación en clústeres de punto de datos es el proceso de combinar datos de puntos que están cerca entre sí y que se representan en el mapa como un punto de datos en clúster único. Como el usuario hace zoom en el mapa, separar los clústeres en sus puntos de datos individuales.

## <a name="enabling-clustering-on-a-data-source"></a>Habilitar agrupación en clústeres en un origen de datos

Agrupación en clústeres fácilmente puede habilitarse en el `DataSource` clase estableciendo el `cluster` opción en true. Además, se puede establecer el radio de píxel para seleccionar puntos cercanos a combinar en un clúster mediante el `clusterRadius` y se puede especificar un nivel de zoom en el que se va a deshabilitar la lógica de agrupación en clústeres con el `clusterMaxZoom` opción. Este es un ejemplo de cómo habilitar la agrupación en clústeres en un origen de datos.

```javascript
//Create a data source and enable clustering.
var datasource = new atlas.source.DataSource(null, {
    //Tell the data source to cluster point data.
    cluster: true,

    //The radius in pixels to cluster points together.
    clusterRadius: 45,

    //The maximum zoom level in which clustering occurs.
    //If you zoom in more than this, all points are rendered as symbols.
    clusterMaxZoom: 15 
});
```

> [!TIP]
> Si dos puntos de datos están muy próximas en cero, es posible que nunca se interrumpirán, y con independencia de cómo cerrar el usuario para acercar el clúster. Para solucionar este problema, puede establecer el `clusterMaxZoom` opción del origen de datos que se especifica en el nivel de zoom para deshabilitar la lógica de agrupación en clústeres y simplemente mostrar todos los elementos.

La `DataSource` clase también tiene los siguientes métodos relacionados con la agrupación en clústeres:

| Método | Tipo de valor devuelto | DESCRIPCIÓN |
|--------|-------------|-------------|
| getClusterChildren(clusterId: number) | Promesa&lt;matriz&lt;característica&lt;Geometry, cualquier&gt; \| forma&gt;&gt; | Recupera a los elementos secundarios del clúster especificado en el siguiente nivel de zoom. Estos elementos secundarios pueden ser una combinación de formas y subclusters. El subclusters será características cuyas propiedades coincidan con ClusteredProperties. |
| getClusterExpansionZoom(clusterId: number) | Promesa&lt;número&gt; | Calcula un nivel de zoom en el que se iniciará expandir el clúster o se divida. |
| getClusterLeaves(clusterId: number, limit: number, offset: number) | Promesa&lt;matriz&lt;característica&lt;Geometry, cualquier&gt; \| forma&gt;&gt; | Recupera todos los puntos en un clúster. Establecer el `limit` para devolver un subconjunto de los puntos y utilice la `offset` página a través de los puntos. |

## <a name="display-clusters-using-a-bubble-layer"></a>Mostrar clústeres con una capa de burbuja

Una capa de burbuja es una excelente manera de representar puntos agrupados como fácilmente puede escalar el radio y cambiar el color basándose en el número de puntos en el clúster mediante el uso de una expresión. Al mostrar clústeres con una capa de burbuja, también se debe usar una capa diferente para representar los puntos de datos no agrupado. A menudo resulta cómodo que también puede mostrar el tamaño del clúster sobre las burbujas. Una capa de símbolos con texto y ningún icono puede usarse para lograr este comportamiento. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Capa de burbuja básica de agrupación en clústeres" src="//codepen.io/azuremaps/embed/qvzRZY/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qvzRZY/'>agrupación en clústeres de capa de burbujas básico</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="display-clusters-using-a-symbol-layer"></a>Mostrar clústeres con una capa de símbolos

Al visualizar los datos de punto mediante la capa de símbolos, de forma predeterminada que se oculte automáticamente símbolos que se superponen entre sí para crear una experiencia más limpia, pero esto puede no ser la experiencia deseada si desea ver la densidad de datos de puntos en el mapa. Establecer el `allowOverlap` opción de las capas de símbolos `iconOptions` propiedad `true` deshabilita esta experiencia, pero se producirá en todos los símbolos que se muestran. Uso de clústeres permite ver la densidad de todos los datos durante la creación de una experiencia agradable al usuario limpia. En este ejemplo, se usará símbolos personalizados para representar los puntos de datos individuales y clústeres.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Capa de símbolos en clúster" src="//codepen.io/azuremaps/embed/Wmqpzz/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/Wmqpzz/'>capa agrupado símbolo</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="clustering-and-the-heat-maps-layer"></a>Capa de mapas de agrupación en clústeres y el calor

Mapas térmicos son una excelente manera de mostrar la densidad de los datos en el mapa. Esta visualización puede controlar un gran número de puntos de datos por sí mismo, pero puede controlar aún más datos si se agrupan en clústeres los puntos de datos y el tamaño del clúster se usa como el peso del mapa térmico. Establecer el `weight` opción de la capa de mapa de calor a `['get', 'point_count']` para lograr esto. Cuando el radio del clúster es pequeño, el mapa de calor será casi idéntico a un mapa térmico mediante los puntos de datos no agrupado, pero funcionará mucho mejor. Sin embargo, cuanto menor sea el radio del clúster, más precisas serán el mapa térmico se pero con menos de un rendimiento beneficiarse.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Mapa térmico ponderada del clúster" src="//codepen.io/azuremaps/embed/VRJrgO/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/VRJrgO/'>clúster ponderado mapa térmico</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="mouse-events-on-clustered-data-points"></a>Eventos del mouse en los puntos de datos en clúster

Cuando se producen los eventos del mouse en una capa que contienen puntos de datos en clúster, se devolverá el punto de datos en clúster para el evento como un objeto de función de punto de GeoJSON. Esta característica punto tendrá las siguientes propiedades:

| Nombre de propiedad | Type | DESCRIPCIÓN |
|---------------|------|-------------|
| clúster | boolean | Indica si la característica representa un clúster. |
| cluster_id | string | Un identificador único para el clúster que se puede usar con el origen de datos `getClusterExpansionZoom`, `getClusterChildren`, y `getClusterLeaves` métodos. |
| point_count | de serie | El número de puntos que contiene el clúster. |
| point_count_abbreviated | string | Una cadena que abrevia la `point_count` valor si es larga. (por ejemplo, 4.000 se convierte en 4K) |

Este ejemplo toma una capa de burbuja que representa los puntos de clúster y agrega un evento de clic que cuando se desencadena, calcular, y la asignación de zoom al siguiente nivel de zoom en el que el clúster se interrumpirá una distancia con la `getClusterExpansionZoom` método de la `DataSource` clase y el `cluster_id` propiedad de la ha hecho clic en clúster de punto de datos. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Clúster getClusterExpansionZoom" src="//codepen.io/azuremaps/embed/moZWeV/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/moZWeV/'>clúster getClusterExpansionZoom</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="display-cluster-area"></a>Área de presentación de clúster 

Los datos del punto que representa un clúster se extienden a través de un área. En este ejemplo, al mantener el mouse sobre un clúster, los datos individuales se usa para calcular una forma convexa y se mostrará en el mapa para mostrar el área de puntos que contiene (hojas). Se pueden recuperar todos los puntos de contenidos en un clúster de origen de datos mediante el `getClusterLeaves` método. Una forma convexa es un polígono que contiene un conjunto de puntos, como una banda elástica y puede calcularse mediante la `atlas.math.getConvexHull` método.

<br/>

 <iframe height="500" style="width: 100%;" scrolling="no" title="Forma convexa de área de clúster" src="//codepen.io/azuremaps/embed/QoXqWJ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/QoXqWJ/'>forma convexa de clúster área</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Clase de origen de datos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Objeto de DataSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [atlas.math namespace](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math?view=azure-iot-typescript-latest)

Vea los siguientes ejemplos de código para agregar funcionalidad a la aplicación:

> [!div class="nextstepaction"]
> [Adición de una capa de burbuja](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](map-add-pin.md)

> [!div class="nextstepaction"]
> [Agregar una capa de mapa de calor](map-add-heat-map-layer.md)
