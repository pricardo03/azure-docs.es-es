---
title: Adición de una capa de mapa térmico a Azure Maps | Microsoft Docs
description: Cómo agregar una capa de mapa térmico al mapa de JavaScript
author: rbrundritt
ms.author: richbrun
ms.date: 12/2/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 82a6d6b2af7df91696844b09fb7650c547cb6bd1
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59258868"
---
# <a name="add-a-heat-map-layer"></a>Adición de una capa de mapa térmico

Los mapas térmicos, también conocidos como mapas de densidad de puntos, son un tipo de visualización de datos utilizado para representar la densidad de los datos mediante una gama de colores. A menudo se utilizan para mostrar las "zonas activas" de los datos en un mapa y son una forma excelente de representar grandes conjuntos de datos de puntos.  Por ejemplo, representar decenas de miles de puntos dentro de la vista de mapa como símbolos, abarca la mayoría del área de mapa y daría lugar a demasiados símbolos superpuestos mutuamente, lo que dificulta la comprensión mucho en los datos. Sin embargo, la visualización de este mismo conjunto de datos como un mapa térmico facilita la visualización dónde los datos de puntos son más densos y la densidad relativa a otras áreas. Hay muchos escenarios en los que se usan los mapas térmicos. Estos son algunos ejemplos:

* Los datos de temperatura normalmente se representan como mapa térmico, ya que este proporciona aproximaciones de temperatura entre dos puntos de datos.
* La representación de datos de sensores de ruido como un mapa térmico no solo muestra la intensidad del ruido donde se encuentra el sensor, sino que también puede proporcionar información sobre la degradación a lo largo de una distancia. Puede que el nivel de ruido de un sitio no sea elevado; sin embargo, si el área de cobertura de ruido de varios sensores se superpone, es posible que esta área superpuesta pueda experimentar niveles de ruido más elevados y, por tanto, serían visibles en el mapa térmico.
* Visualizar un GPS seguimiento que incluya la velocidad como una asignación ponderada alto donde la intensidad de cada punto de datos se basa en la velocidad es una excelente manera de ver dónde se acelera el vehículo.

> [!TIP]
> De forma predeterminada, las capas de un mapa térmico representarán las coordenadas de todos los objetos geométricos de un origen de datos. Para limitar la capa para que solo representa elija las características de geometría, establecer el `filter` propiedad de la capa a `['==', ['geometry-type'], 'Point']` o `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` si desea incluir también las características de MultiPoint.

## <a name="add-a-heat-map-layer"></a>Adición de una capa de mapa térmico

Para representar un origen de datos de puntos como un mapa térmico, solo tiene que pasar el origen de datos a una instancia de la clase `HeatMapLayer` y agregarlo al mapa tal y como se muestra aquí.

<br/>

<iframe height='500' scrolling='no' title='Capa de mapa térmico sencilla' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>Capa de mapa térmico sencilla</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En este ejemplo, cada punto térmico tiene un radio de 10 píxeles a todos los niveles de zoom. Al agregar la capa de mapa térmico al mapa, en este ejemplo se inserta debajo de la capa de etiqueta para crear una mejor experiencia de usuario como las etiquetas son claramente por encima del mapa térmico. Los datos en este ejemplo se originan los [USG terremoto peligros programa](https://earthquake.usgs.gov/) y representa los terremotos importantes que se han producido en los últimos 30 días.

## <a name="customizing-the-heat-map-layer"></a>Personalización de la capa de mapa térmico

En el ejemplo anterior se personalizó el mapa térmico con la configuración de las opciones de radio y opacidad. La capa de mapa térmico ofrece varias opciones de personalización:

* `radius`: define un radio de píxel en el que representar cada punto de datos. El radio puede establecerse como un número fijo o como una expresión. Mediante una expresión, es posible escalar el radio según el nivel de zoom, que aparece para representar un área espacial coherente en el mapa (por ejemplo, el radio de 5 millas).
* `color`: especifica cómo se colorea el mapa térmico. Un degradado de color se suele utilizar para mapas térmicos y se puede lograr con un `interpolate` expresión. Mediante un `step` expresión para colorear el mapa térmico desglosa la densidad visualmente en intervalos que más se parezca a un mapa de estilo de contorno o radial. Las paletas de colores definen los colores desde valores de intensidad mínimos hasta máximos. Los valores de color de los mapas térmicos se especifican como una expresión en el valor `heatmap-density`. El color en el índice 0 en una expresión de interpolación o el color predeterminado de una expresión de paso, define el color del área donde no hay ningún dato y puede utilizarse para definir un color de fondo. Muchos prefieren establecer este valor en transparente o en un negro semitransparente. Estos son ejemplos de expresiones de color:

| Expresión de Color de interpolación | Expresión de color escalonado | 
|--------------------------------|--------------------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'interpolate',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['linear'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, 'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'purple',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'step',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.25, 'verde',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,50, "amarillo",<br/>&nbsp;&nbsp;&nbsp;&nbsp;0,75, "rojo"<br/>\] | 

* `opacity`: especifica el grado de opacidad o transparencia de la capa de mapa térmico.
* `intensity`: Se aplica un multiplicador para el peso de cada punto de datos para aumentar la intensidad de los mapas térmicos global y contribuye a que las pequeñas diferencias en el peso de puntos de datos resultan más fáciles de visualizar.
* `weight`: de forma predeterminada, todos los puntos de datos tienen un peso de 1; por lo tanto, todos los puntos de datos se ponderan equitativamente. La opción de peso actúa como un multiplicador y puede establecerse como un número o una expresión. Si el peso se establece con un número, digamos 2, equivaldría a colocar cada punto de datos dos veces en el mapa, por lo que se duplicaría la densidad. Establecer la opción de peso en un número representa el mapa térmico de forma similar a la opción de intensidad. Sin embargo, si se utiliza una expresión, el peso de cada punto de datos se puede basar en las propiedades de cada punto de datos. Si tomamos los datos de un terremoto como ejemplo, cada punto de datos representaría un terremoto. Las métricas importantes que tenga cada punto de datos se considerarán valores de magnitud. Los terremotos se producen a menudo, pero la mayoría tienen una magnitud baja y casi no se sienten. Utilizando el valor de magnitud en una expresión para asignar el peso a los datos de cada punto de forma, terremotos más importantes para representarse mejor en el mapa de calor.
* Además de las opciones de la capa base, el zoom mínimo y máximo, la opción Visible y el filtro, también está la opción `source` si desea actualizar el origen de datos y la opción `source-layer` si el origen de dato es un origen de corte vectorial.

A continuación, se muestra una herramienta para probar las diferentes opciones de la capa de mapa térmico.

<br/>

<iframe height='700' scrolling='no' title='Opciones de la capa de mapa térmico' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>Opciones de la capa de mapa térmico</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="consistent-zoomable-heat-map"></a>Mapa térmico se puede acercar coherente

De forma predeterminada, el radio de puntos de datos representados en la capa de mapa térmico tiene un radio de píxel fijo para todos los niveles de zoom. Como el mapa se amplía los agregados de datos juntos y la capa de mapa térmico tiene un aspecto diferente. Un `zoom` expresión puede utilizarse para escalar el radio para cada nivel de zoom de forma que cada punto de datos trata de la misma área física de la asignación. Esto hará que la capa de mapa térmico buscar más coherente y estático. Cada nivel de zoom del mapa tiene dos veces tantas píxeles vertical y horizontalmente como nivel de zoom anterior. Escala el radio de forma que duplica con cada nivel de zoom, se creará un mapa térmico que tiene un aspecto coherente en todos los niveles de zoom. Esto puede realizarse mediante el uso de la `zoom` en base 2 `exponential interpolation` expresión tal como se muestra en el ejemplo siguiente. Zoom del mapa para ver cómo se escala el mapa térmico con el nivel de zoom.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Mapa térmico se puede acercar coherente" src="//codepen.io/azuremaps/embed/OGyMZr/?height=500&theme-id=light&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/OGyMZr/'>mapa térmico se puede acercar coherente</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Al habilitar la agrupación en clústeres en el origen de datos, los puntos que están cerca unos de otros se agrupan como un punto agrupado. El número de puntos de cada clúster puede utilizarse como la expresión de peso para el mapa térmico y reducir significativamente el número de puntos que se deben representar. El número de punto de un clúster se almacena en un `point_count` propiedad de la característica punto tal como se muestra a continuación. 
> ```JavaScript
> var layer = new atlas.layer.HeatMapLayer(datasource, null, {
>    weight: ['get', 'point_count']
> });
> ```
> Si el radio de agrupación en clústeres es solo de unos cuantos píxeles, habrá poca diferencia visual en la representación. Un radio más grande agrupará más puntos en cada clúster y mejorará el rendimiento del mapa térmico, pero las diferencias serán más evidentes.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [HeatMapLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HeatMapLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.heatmaplayeroptions?view=azure-iot-typescript-latest)

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos:

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Usar expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)