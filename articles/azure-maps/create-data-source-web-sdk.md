---
title: Creación de un origen de datos con Azure Maps | Microsoft Docs
description: Procedimiento para crear un origen de datos y usarlo con el SDK de Azure Maps para web.
author: rbrundritt
ms.author: richbrun
ms.date: 08/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: codepen
ms.openlocfilehash: b83a66296d54a179a56e37de199ec900ae23a1db
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75433012"
---
# <a name="create-a-data-source"></a>Creación de un origen de datos

El SDK de Azure Maps para web almacena los datos en orígenes de datos que optimizan los datos para la consulta y la representación. Actualmente hay dos tipos de orígenes de datos:

**Origen de datos de GeoJSON**

Un origen de datos basado en GeoJSON puede cargar y almacenar datos localmente mediante la clase `DataSource`. Los datos de GeoJSON se pueden crear manualmente o mediante las clases auxiliares del espacio de nombres de [atlas.data](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data). La clase `DataSource` proporciona funciones para importar archivos GeoJSON locales o remotos. Los archivos GeoJSON remotos deben hospedarse en un punto de conexión habilitado para CORs. La clase `DataSource` proporciona funcionalidad para agrupar en clústeres los datos de punto. Los datos se pueden agregar, quitar y actualizar con facilidad con la clase `DataSource`.


> [!TIP]
> Si quiere sobrescribir todos los datos en un `DataSource`, puede realizar llamadas a las funciones `clear` y `add`. De esta forma, el mapa intentará volver a representarse dos veces, lo que puede provocar un cierto retraso. En su lugar, use la función `setShapes`, que quitará y reemplazará todos los datos del origen de datos y solo desencadenará una nueva representación del mapa.

**Origen de mosaico vectorial**

Un origen de mosaico vectorial describe cómo obtener acceso a una capa de mosaico vectorial y se puede crear con la clase [VectorTileSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.vectortilesource). Azure Maps se alinea con la [especificación de mosaicos vectoriales de MapBox](https://github.com/mapbox/vector-tile-spec), que es un estándar abierto. Las capas de mosaicos vectoriales son similares a las capas de mosaicos; sin embargo, en lugar de que cada mosaico sea una imagen de trama, son archivos comprimidos (formato PBF) que contienen datos de mapas vectoriales y una o varias capas que se pueden representar y dar estilo en el cliente en función del estilo de cada capa. Los datos de un mosaico vectorial contienen características geográficas en forma de puntos, líneas y polígonos. Hay varias ventajas de las capas de mosaicos vectoriales en las capas de mosaicos de trama.

 - Un tamaño de archivo de un mosaico vectorial suele ser mucho menor que el de un mosaico de trama equivalente. Como tal, se usa menos ancho de banda, lo que significa una menor latencia y una asignación más rápida. Esto crea una mejor experiencia de usuario.
 - Como los mosaicos vectoriales se representan en el cliente, pueden adaptarse a la resolución del dispositivo en el que se muestran. Esto permite que los mapas representados aparezcan mucho más definidos y con etiquetas más claras. 
 - Cambiar el estilo de los datos en los mapas vectoriales no requiere descargar los datos de nuevo ya que el nuevo estilo se puede aplicar en el cliente. Por el contrario, cambiar el estilo de una capa de mosaico de trama requiere normalmente la carga de mosaicos del servidor que tienen aplicado el nuevo estilo.
 - Dado que los datos se entregan en forma de vector, se requiere menos procesamiento del lado servidor para preparar los datos, lo que significa que los datos más recientes pueden ponerse a disposición con más rapidez.

Todas las capas que usan un origen vectorial deben especificar un valor `sourceLayer`. 

Una vez creados, los orígenes de datos se pueden agregar a la asignación mediante la propiedad `map.sources`, que es un objeto [SourceManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.sourcemanager). El código siguiente muestra cómo crear un `DataSource` y agregarlo al mapa.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);
```

## <a name="connecting-a-data-source-to-a-layer"></a>Conexión de un origen de datos a una capa

Los datos se representan en el mapa mediante las capas de representación. Una o varias capas de representación pueden hacer referencia a un único origen de datos. Las siguientes capas de representación requieren que un origen de datos las alimente:

- [Capa de burbuja](map-add-bubble-layer.md): representa los datos de puntos como círculos a escala en el mapa.
- [Capa de símbolo](map-add-pin.md): representa los datos del punto como iconos o texto.
- [Capa de mapa térmico](map-add-heat-map-layer.md): representa datos de puntos como un mapa térmico de densidad.
- [Capa de línea](map-add-shape.md): se puede usar para representar una línea o el contorno de los polígonos. 
- [Capa de polígono](map-add-shape.md): rellena el área de un polígono con un patrón de imagen o un color sólido.

En el código siguiente se muestra cómo crear un origen de datos, agregarlo a la asignación y conectarlo a una capa de burbuja y, a continuación, importar los datos de punto de GeoJSON desde una ubicación remota a él. 

```javascript
//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Create a layer that defines how to render points in the data source and add it to the map.
map.layers.add(new atlas.layer.BubbleLayer(datasource));

//Load the earthquake data.
datasource.importDataFromUrl('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/significant_month.geojson');
```

Hay capas de representación adicionales que no se conectan a estos orígenes de datos, sino que, en su lugar, cargan los datos que representan directamente. 

- [Capa de imagen](map-add-image-layer.md): superpone una sola imagen en la parte superior del mapa y enlaza sus esquinas a un conjunto de coordenadas especificadas.
- [Capa de mosaico](map-add-tile-layer.md): superpone una capa de mosaico de trama en la parte superior del mapa.

## <a name="one-data-source-with-multiple-layers"></a>Un origen de datos con varias capas

Varias capas pueden estar conectadas a un solo origen de datos. Esto puede sonar extraño, pero hay diversos escenarios en los que esto resulta útil. Por ejemplo, tomemos el escenario en el que crea una experiencia de dibujo de un polígono. Cuando se le permite a un usuario dibujar un polígono, debemos representar el área de relleno del polígono a media que el usuario agrega puntos al mapa. Agregar una línea con estilo que contornee el polígono hará que sea más fácil ver los bordes del polígono mientras se dibuja. Finalmente, agregar algún tipo de manipulador, como un marcador o una chincheta, sobre cada posición en el polígono permitiría editar más fácilmente cada posición. En la siguiente imagen se muestra este escenario.

![Mapa que muestra varias capas que representan datos de un único origen de datos](media/create-data-source-web-sdk/multiple-layers-one-datasource.png)

Para lograr este escenario en la mayoría de las plataformas de mapas, tendría que crear un objeto de polígono, un objeto de línea y una chincheta para cada posición en el polígono. A medida que le polígono se modifica, tendría que actualizar manualmente las líneas y las chinchetas, que pueden volverse complejas rápidamente.

Con Azure Maps, lo único que necesita es un solo polígono en un origen de datos, como se muestra en el siguiente código.

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a polygon and add it to the data source.
dataSource.add(new atlas.data.Polygon([[[/* Coordinates for polygon */]]]));

//Create a polygon layer to render the filled in area of the polygon.
var polygonLayer = new atlas.layer.PolygonLayer(dataSource, 'myPolygonLayer', {
     fillColor: 'rgba(255,165,0,0.2)'
});

//Create a line layer for greater control of rendering the outline of the polygon.
var lineLayer = new atlas.layer.LineLayer(dataSource, 'myLineLayer', {
     color: 'orange',
     width: 2
});

//Create a bubble layer to render the vertices of the polygon as scaled circles.
var bubbleLayer = new atlas.layer.BubbleLayer(dataSource, 'myBubbleLayer', {
     color: 'orange',
     radius: 5,
     outlineColor: 'white',
     outlineWidth: 2
});

//Add all layers to the map.
map.layers.add([polygonLayer, lineLayer, bubbleLayer]);
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [DataSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.datasourceoptions?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [VectorTileSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.vectortilesource?view=azure-maps-typescript-latest)

> [!div class="nextstepaction"]
> [VectorTileSourceOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.vectortilesourceoptions?view=azure-maps-typescript-latest)

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Adición de un elemento emergente](map-add-popup.md)

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](map-add-pin.md)

> [!div class="nextstepaction"]
> [Adición de una capa de burbuja](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de línea](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de polígono](map-add-shape.md)

> [!div class="nextstepaction"]
> [Adición de un mapa térmico](map-add-heat-map-layer.md)

> [!div class="nextstepaction"]
> [Ejemplos de código](https://docs.microsoft.com/samples/browse/?products=azure-maps)