---
title: Agrupación en clústeres de datos de punto en Azure Maps | Microsoft Docs
description: Cómo agrupar en clúster los datos de punto en el SDK web
author: rbrundritt
ms.author: richbrun
ms.date: 03/27/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendleton
ms.custom: codepen
ms.openlocfilehash: 6dbd4461e7b8382ec3c4075b9688de59678f98f5
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65957318"
---
# <a name="clustering-point-data"></a>Agrupación en clústeres de datos de punto

Cuando se visualizan muchos puntos de datos en el mapa, estos se superponen, el mapa presenta un aspecto desordenado y resulta difícil consultar y usar. La agrupación en clústeres de datos de punto puede utilizarse para mejorar esta experiencia de usuario. La agrupación en clústeres de datos de punto es el proceso de combinar datos de punto que están cerca unos de otros y representarlos en el mapa como un único punto de datos agrupados en clúster. Cuando el usuario acerca el mapa, los clústeres se separan en sus puntos de datos individuales.

## <a name="enabling-clustering-on-a-data-source"></a>Habilitar la agrupación en clústeres en un origen de datos

La agrupación en clústeres puede habilitarse fácilmente en la clase `DataSource` estableciendo la opción `cluster` en true. Además, se puede establecer el radio de píxel para seleccionar los puntos cercanos que se combinarán en un clúster mediante `clusterRadius` y se puede especificar un nivel de zoom en el que se deshabilitará la lógica de agrupación en clústeres con la opción `clusterMaxZoom`. Este es un ejemplo de cómo habilitar la agrupación en clústeres en un origen de datos.

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
> Si dos puntos de datos están muy próximos en la superficie, es posible que nunca se separen, con independencia de lo mucho que el usuario acerque el mapa. Para solucionarlo, puede establecer la opción `clusterMaxZoom` del origen de datos que especifica el nivel de zoom para deshabilitar la lógica de agrupación en clústeres y simplemente mostrar todos los elementos.

La clase `DataSource` también tiene los siguientes métodos relacionados con la agrupación en clústeres:

| Método | Tipo de valor devuelto | DESCRIPCIÓN |
|--------|-------------|-------------|
| getClusterChildren(clusterId: number) | Promesa&lt;Matriz&lt;Característica&lt;Geometría, cualquiera&gt; \| Forma&gt;&gt; | Recupera los elementos secundarios del clúster especificado en el siguiente nivel de zoom. Estos elementos secundarios pueden ser una combinación de formas y subclústeres. Los subclústeres serán características con propiedades que coincidan con ClusteredProperties. |
| getClusterExpansionZoom(clusterId: number) | Promesa&lt;número&gt; | Calcula un nivel de zoom en el que el clúster empezará a expandirse o separarse. |
| getClusterLeaves(clusterId: number, limit: number, offset: number) | Promesa&lt;Matriz&lt;Característica&lt;Geometría, cualquiera&gt; \| Forma&gt;&gt; | Recupera todos los puntos de un clúster. Establezca `limit` para que devuelva un subconjunto de los puntos y use `offset` para paginar a través de los puntos. |

## <a name="display-clusters-using-a-bubble-layer"></a>Mostrar clústeres con una capa de burbujas

Una capa de burbujas es una excelente manera de representar puntos agrupados en clústeres ya que se puede escalar fácilmente el radio y cambiar el color, según el número de puntos del clúster, con una expresión. Al mostrar clústeres con una capa de burbujas, también se debe usar una capa diferente para representar los puntos de datos no agrupados en clústeres. A menudo, resulta útil que también se pueda mostrar el tamaño del clúster sobre las burbujas. Se puede usar una capa de símbolos con texto y ningún icono para lograr este comportamiento. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Capa básica de burbujas de agrupación en clústeres" src="//codepen.io/azuremaps/embed/qvzRZY/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qvzRZY/'>Basic bubble layer clustering</a> (Agrupación en clústeres básica de capa de burbujas) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="display-clusters-using-a-symbol-layer"></a>Mostrar clústeres con una capa de símbolos

Al visualizar los datos de punto con la capa de símbolos, de forma predeterminada se ocultarán automáticamente los símbolos que se superponen entre sí para crear una experiencia más ordenada, pero puede que no sea la experiencia deseada si quiere ver la densidad de los puntos de datos en el mapa. Establecer la opción `allowOverlap` de la propiedad `iconOptions` de las capas de símbolos en `true` deshabilita esta experiencia, pero se mostrarán todos los símbolos. El uso de la agrupación en clústeres permite ver la densidad de todos los datos durante la creación de una experiencia de usuario agradable y ordenada. En este ejemplo, se usarán símbolos personalizados para representar los clústeres y los puntos de datos individuales.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Capa de símbolos agrupados en clúster" src="//codepen.io/azuremaps/embed/Wmqpzz/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/Wmqpzz/'>Clustered Symbol layer</a> (Capa de símbolos agrupados en clústeres) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="clustering-and-the-heat-maps-layer"></a>Agrupación en clústeres y capa de mapas térmicos

Los mapas térmicos son una excelente manera de mostrar la densidad de los datos en el mapa. Esta visualización puede controlar un gran número de puntos de datos por sí sola, pero puede controlar aún más datos si se agrupan en clústeres los puntos de datos y el tamaño del clúster se usa como peso del mapa térmico. Establezca la opción `weight` de la capa de mapa térmico en `['get', 'point_count']` para lograr esto. Cuando el radio del clúster sea pequeño, el mapa térmico será casi idéntico a un mapa térmico que use puntos de datos no agrupados en clústeres, pero funcionará mucho mejor. Aunque, cuanto menor sea el radio del clúster, más preciso será el mapa térmico, pero con menos ventajas de rendimiento.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Mapa térmico ponderado del clúster" src="//codepen.io/azuremaps/embed/VRJrgO/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/VRJrgO/'>Cluster weighted Heat Map</a> (Clúster ponderado de mapa térmico) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="mouse-events-on-clustered-data-points"></a>Eventos del mouse en los puntos de datos agrupados en clúster

Cuando se producen eventos del mouse en una capa que contiene puntos de datos agrupados en clúster, se devolverá el punto de datos agrupados en clúster para el evento como un objeto de característica de punto de GeoJSON. La característica de punto tendrá las siguientes propiedades:

| Nombre de propiedad | Type | DESCRIPCIÓN |
|---------------|------|-------------|
| cluster | boolean | Indica si la característica representa un clúster. |
| cluster_id | string | Un id. exclusivo para el clúster que se puede usar con los métodos `getClusterExpansionZoom`, `getClusterChildren` y `getClusterLeaves` de DataSource. |
| point_count | número | El número de puntos que contiene el clúster. |
| point_count_abbreviated | string | Una cadena que abrevia el valor de `point_count`, si es largo (por ejemplo, 4000 se convierte en 4 K). |

En este ejemplo, se toma una capa de burbujas que representa los puntos de clúster y agrega un evento de clic que, cuando se desencadena, calcula y acerca el mapa al siguiente nivel de zoom en el que el clúster se separará con el método `getClusterExpansionZoom` de la clase `DataSource` y la propiedad `cluster_id` del punto de datos agrupados en clústeres en el que se ha hecho clic. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Clúster getClusterExpansionZoom" src="//codepen.io/azuremaps/embed/moZWeV/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/moZWeV/'>Cluster getClusterExpansionZoom</a> (Clúster getClusterExpansionZoom) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="display-cluster-area"></a>Mostrar área de clúster 

Los datos de punto que un clúster representa se dispersan por un área. En este ejemplo, al mantener el mouse sobre un clúster, los puntos de datos individuales que contiene (hojas) se usarán para calcular una envolvente convexa y se visualizará en el mapa para mostrar el área. Se pueden recuperar todos los puntos del origen de datos contenidos en un clúster mediante el método `getClusterLeaves`. Una envolvente convexa es un polígono que encapsula un conjunto de puntos como una banda elástica y puede calcularse mediante el método `atlas.math.getConvexHull`.

<br/>

 <iframe height="500" style="width: 100%;" scrolling="no" title="Envolvente convexa del área de clúster" src="//codepen.io/azuremaps/embed/QoXqWJ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/QoXqWJ/'>Cluster area convex hull</a> (Envolvente convexa del área de clúster) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Clase DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Objeto DataSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Espacio de nombres atlas.math](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.math?view=azure-iot-typescript-latest)

Vea los siguientes ejemplos de código para agregar funcionalidad a la aplicación:

> [!div class="nextstepaction"]
> [Adición de una capa de burbuja](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](map-add-pin.md)

> [!div class="nextstepaction"]
> [Adición de una capa de mapa térmico](map-add-heat-map-layer.md)
