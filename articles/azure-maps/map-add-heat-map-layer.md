---
title: Adición de una capa de mapa térmico a Azure Maps | Microsoft Docs
description: Cómo agregar una capa de mapa térmico al SDK web de Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b0d07f18682bf39558180753de38a9c5ff106ee3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75408534"
---
# <a name="add-a-heat-map-layer"></a>Adición de una capa de mapa térmico

Los mapas térmicos, también conocidos como mapas de densidad de puntos, son un tipo de visualización de datos que se utiliza para representar la densidad de los datos mediante gamas de colores. Suelen utilizarse para mostrar las "zonas activas" de los datos de un mapa y son una forma excelente de representar grandes conjuntos de datos de puntos. 

Por ejemplo, si en una vista de mapa se representan decenas de miles de puntos como símbolos, la mayor parte del área del mapa quedará cubierta. Esto podría provocar que muchos símbolos se solaparan, por lo que resultaría difícil extraer información de los datos. Sin embargo, si este mismo conjunto de datos se representa en un mapa térmico, resulta más fácil ver dónde los datos de puntos son más densos y la densidad relativa con respecto a otras áreas.

Puede usar mapas térmicos en muchos escenarios diferentes, entre los que se incluyen:

- **Datos de temperatura**: proporciona aproximaciones en las que la temperatura está entre dos puntos de datos.
- **Datos de sensores de ruido**: no solo muestra la intensidad del ruido en el lugar donde se encuentra el sensor, sino que también puede proporcionar información acerca de cómo se va disipando el ruido con la distancia. El nivel de ruido de un sitio podría no ser muy alto. Sin embargo, si las zonas de cobertura de diferentes sensores de ruido se solapan, podría ocurrir que los niveles de ruido fueran mayores en esta área solapada, lo que podría verse en el mapa térmico.
- **Seguimiento mediante GPS**: incluye la velocidad como un mapa de altura ponderada en el que la intensidad de cada punto de datos depende de la velocidad. Por ejemplo, permitiría ver dónde aceleró un vehículo.

> [!TIP]
> De forma predeterminada, las capas de los mapas térmicos representan las coordenadas de todos los objetos geométricos de un origen de datos. Para limitar la capa de forma que solo represente las características geométricas del punto, establezca la propiedad `filter` de la capa en `['==', ['geometry-type'], 'Point']`. Si desea incluir también características MultiPoint, establezca la propiedad `filter` de la capa en `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]`.

<br/>

<iframe src="https://channel9.msdn.com/Shows/Internet-of-Things-Show/Heat-Maps-and-Image-Overlays-in-Azure-Maps/player" width="960" height="540" allowFullScreen frameBorder="0"></iframe>

## <a name="add-a-heat-map-layer"></a>Adición de una capa de mapa térmico

Para representar un origen de datos de puntos como un mapa térmico, solo tiene que pasar el origen de datos a una instancia de la clase `HeatMapLayer` y agregarlo al mapa.

En el código siguiente, cada punto térmico tiene un radio de 10 píxeles a todos los niveles de zoom. Al agregar la capa de mapa térmico al mapa, este ejemplo se inserta debajo de la capa de etiquetas para crear una mejor experiencia del usuario. Las etiquetas pueden verse claramente sobre el mapa térmico. Los datos de este ejemplo proceden de [USGS Earthquake Hazards Program](https://earthquake.usgs.gov/) y representan terremotos importantes que se han producido en los 30 últimos días.

```javascript
//Create a data source and add it to the map.
var datasource = new atlas.source.DataSource();
map.sources.add(datasource);

//Load a dataset of points, in this case earthquake data from the USGS.
datasource.importDataFromUrl('https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_week.geojson');

//Create a heatmap and add it to the map.
map.layers.add(new atlas.layer.HeatMapLayer(datasource, null, {
  radius: 10,
  opacity: 0.8
}), 'labels');
```

A continuación, se muestra el código de ejemplo de ejecución completo del código anterior.

<br/>

<iframe height='500' scrolling='no' title='Capa de mapa térmico sencilla' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>Capa de mapa térmico sencilla</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-the-heat-map-layer"></a>Personalización de la capa de mapa térmico

En el ejemplo anterior se personalizó el mapa térmico con la configuración de las opciones de radio y opacidad. La capa de mapa térmico ofrece varias opciones de personalización:

* `radius`: define un radio de píxel en el que representar cada punto de datos. Puede definir el radio como un número fijo o como una expresión. Si utiliza una expresión, es posible escalar el radio en función del nivel de zoom, que parece representar un área espacial coherente del mapa (por ejemplo, un radio de 5 millas).
* `color`: especifica cómo se colorea el mapa térmico. El degradado de color es una característica habitual de los mapas térmicos y ese efecto se puede conseguir con una expresión `interpolate`. También se pude usar una expresión `step` para colorear el mapa térmico y separar gráficamente la densidad en intervalos para que se asemeje más a un mapa de radar o con contornos. Las paletas de colores definen los colores desde valores de intensidad mínimos hasta máximos. 

  En los mapas térmicos, los valores de los colores se especifican como una expresión del valor `heatmap-density`. El color del índice 0 de una expresión de interpolación o el color predeterminado de una expresión escalonada definen el color del área donde no hay ningún dato. Puede utilizar esto para definir un color de fondo. Normalmente, este valor se establece como transparente o en un color negro semitransparente. 
   
  Estos son algunos ejemplos de expresiones de color:

  | Expresión de color de interpolación | Expresión de color escalonado | 
  |--------------------------------|--------------------------|
  | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'interpolate',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['linear'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, 'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'purple',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'step',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.25, 'green',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.50, 'yellow',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.75, 'red'<br/>\] |   

- `opacity`: especifica el grado de opacidad o transparencia de la capa de mapa térmico.
- `intensity`: aplica un multiplicador al peso de cada punto de datos para aumentar la intensidad general del mapa térmico. De este modo, resulta más fácil visualizar las pequeñas diferencias en el peso de los puntos de datos.
- `weight`: de forma predeterminada, todos los puntos de datos tienen un peso de 1 y, por tanto, todos los puntos tienen la misma ponderación. La opción de ponderación actúa como un multiplicador y puede establecerse como un número o una expresión. Si la ponderación se establece con un número (por ejemplo, 2), equivaldría a colocar cada punto de datos dos veces en el mapa, por lo que se duplicaría la densidad. Establecer la opción de peso en un número representa el mapa térmico de forma similar a la opción de intensidad. 

  Sin embargo, si se usa una expresión, la ponderación de cada punto de datos puede basarse en las propiedades de dicho punto de datos. Por ejemplo, supongamos que cada punto de datos representa un terremoto. Las métricas importantes que tenga cada punto de datos se considerarán valores de magnitud. Los terremotos se producen a menudo, pero la mayoría tienen una magnitud baja y casi no se sienten. Si usamos el valor de magnitud de una expresión para asignar la ponderación a cada punto de datos, conseguiremos que los terremotos más importantes se representen mejor en el mapa térmico.
- `source` y `source-layer`: permiten actualizar el origen de datos.

Esta herramienta permite probar las diferentes opciones de la capa de mapa térmico.

<br/>

<iframe height='700' scrolling='no' title='Opciones de la capa de mapa térmico' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>Opciones de la capa de mapa térmico</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="consistent-zoomable-heat-map"></a>Mapa térmico con aplicación de zoom coherente

De forma predeterminada, los radios de los puntos de datos representados en la capa del mapa térmico tienen un radio de píxel fijo para todos los niveles de zoom. A medida que se amplía el mapa, los datos se agregan juntos y la capa de mapa térmico parece diferente. 

Utilice una expresión `zoom` para ampliar el radio de cada nivel de zoom de forma que cada punto de datos cubra la misma superficie física del mapa. Esto hará que la capa de mapa térmico parezca más estática y coherente. Cada nivel de zoom del mapa tiene dos veces tantos píxeles vertical y horizontalmente que nivel de zoom anterior. 

Si el radio se amplía duplicándose con cada nivel de zoom, se creará un mapa térmico que parecerá coherente en todos los niveles de zoom. Para ello, utilice `zoom` con una expresión `exponential interpolation` de base 2, tal y como se muestra en el siguiente ejemplo. Amplía el mapa para ver cómo se escala el mapa térmico con el nivel de zoom.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Mapa térmico con aplicación de zoom coherente" src="//codepen.io/azuremaps/embed/OGyMZr/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/OGyMZr/'>Consistent zoomable heat map</a> (Mapa térmico con aplicación de zoom coherente) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Si habilita la agrupación en clústeres en el origen de datos, los puntos que estén cerca unos de otros aparecerán como un punto agrupado. Puede utilizar el número de puntos de cada clúster como expresión de ponderación del mapa térmico y reducir significativamente el número de puntos que deben representarse. El número de puntos de un clúster se almacena en la propiedad `point_count` de la característica de puntos: 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Si el radio de agrupación en clústeres es solo de unos pocos píxeles, habrá poca diferencia visual en la representación. Cuanto mayores sean los grupos de radios, mayor será el número de puntos de cada clúster, lo que mejorará el rendimiento del mapa térmico.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [HeatMapLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos:

> [!div class="nextstepaction"]
> [Creación de un origen de datos](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)
