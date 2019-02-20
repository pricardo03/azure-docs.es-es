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
ms.openlocfilehash: 957ce60b8519ccb1e3287232f7a5459a56b25bb7
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2019
ms.locfileid: "55960622"
---
# <a name="add-a-heat-map-layer"></a>Adición de una capa de mapa térmico

Los mapas térmicos, también conocidos como mapas de densidad de puntos, son un tipo de visualización de datos utilizado para representar la densidad de los datos mediante una gama de colores. A menudo se utilizan para mostrar las "zonas activas" de los datos en un mapa y son una forma excelente de representar grandes conjuntos de datos de puntos.  Por ejemplo, si se representaran decenas de miles de puntos como símbolos en la vista del mapa, se cubriría cubre la mayor parte del área del mapa, lo que daría lugar a que muchos símbolos quedaran tapados por otros y dificultaría la comprensión de los datos. Sin embargo, la visualización de este mismo conjunto de datos como un mapa térmico facilita la visualización dónde los datos de puntos son más densos y la densidad relativa a otras áreas. Hay muchos escenarios en los que se usan los mapas térmicos. Estos son algunos ejemplos:

* Los datos de temperatura normalmente se representan como mapa térmico, ya que este proporciona aproximaciones de temperatura entre dos puntos de datos.
* La representación de datos de sensores de ruido como un mapa térmico no solo muestra la intensidad del ruido donde se encuentra el sensor, sino que también puede proporcionar información sobre la degradación a lo largo de una distancia. Puede que el nivel de ruido de un sitio no sea elevado; sin embargo, si el área de cobertura de ruido de varios sensores se superpone, es posible que esta área superpuesta pueda experimentar niveles de ruido más elevados y, por tanto, serían visibles en el mapa térmico.
* La visualización de un seguimiento por GPS que incluya la velocidad como un mapa de altura ponderada, donde la intensidad de cada punto de datos se basaría en la velocidad, es una forma magnífica de ver rápidamente dónde acelera el vehículo.

> [!TIP]
> De forma predeterminada, las capas de un mapa térmico representarán las coordenadas de todos los objetos geométricos de un origen de datos. Para limitar la capa de tal forma que solo represente las características geométricas de punto, establezca la propiedad `filter` de la capa en `['==', '$type', 'Point']`.

## <a name="add-a-heat-map-layer"></a>Adición de una capa de mapa térmico

Para representar un origen de datos de puntos como un mapa térmico, solo tiene que pasar el origen de datos a una instancia de la clase `HeatMapLayer` y agregarlo al mapa tal y como se muestra aquí.

<br/>

<iframe height='500' scrolling='no' title='Capa de mapa térmico sencilla' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>Capa de mapa térmico sencilla</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En este ejemplo, cada punto térmico tiene un radio de 10 píxeles a todos los niveles de zoom. Al agregar la capa de mapa térmico al mapa, en este ejemplo se inserta debajo de la capa de la etiqueta. De esta forma, se crea una experiencia de usuario mejorada, ya que las etiquetas se ven claramente por encima del mapa térmico. Los datos de este ejemplo proceden de [USGS Earthquake Hazards Program](https://earthquake.usgs.gov/) y constan de puntos que representan importantes terremotos que se han producido en los 30 últimos días.

## <a name="customizing-the-heat-map-layer"></a>Personalización de la capa de mapa térmico

En el ejemplo anterior se personalizó el mapa térmico con la configuración de las opciones de radio y opacidad. La capa de mapa térmico ofrece varias opciones de personalización:

* `radius`: define un radio de píxel en el que representar cada punto de datos. El radio puede establecerse como un número fijo o como una expresión. Mediante una expresión, es posible escalar el radio en función del nivel de zoom, que parece representar un área espacial coherente del mapa (por ejemplo, un radio de 5 millas).
* `color`: especifica cómo se colorea el mapa térmico. Se suele utilizar una paleta de colores degradados para los mapas térmicos, pero las paletas de colores escalonados también son útiles si desea que el mapa térmico se asemeje más a datos de contorno. Las paletas de colores definen los colores desde valores de intensidad mínimos hasta máximos. Los valores de color de los mapas térmicos se especifican como una expresión en el valor `heatmap-density`. El color del índice 0 de una expresión de degradado o el color predeterminado de un color escalonado define el color del área donde no hay ningún dato y puede utilizarse para especificar el color de fondo. Muchos prefieren establecer este valor en transparente o en un negro semitransparente. Estos son ejemplos de expresiones de color:

| Expresión de color degradado | Expresión de color escalonado | 
|---------------------------|--------------------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'interpolate',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['linear'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, 'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'purple',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'step',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.25, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.50, 'green',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.75, 'yellow',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1.00, 'red'<br/>\] |   

* `opacity`: especifica el grado de opacidad o transparencia de la capa de mapa térmico.
* `intensity`: aplica un multiplicador al peso de cada punto de datos para aumentar la intensidad general del mapa térmico. Esto ayuda a que las pequeñas diferencias en el peso de los puntos de datos resulten más fáciles de visualizar.
* `weight`: de forma predeterminada, todos los puntos de datos tienen un peso de 1; por lo tanto, todos los puntos de datos se ponderan equitativamente. La opción de peso actúa como un multiplicador y puede establecerse como un número o una expresión. Si el peso se establece con un número, digamos 2, equivaldría a colocar cada punto de datos dos veces en el mapa, por lo que se duplicaría la densidad. Establecer la opción de peso en un número representa el mapa térmico de forma similar a la opción de intensidad. Sin embargo, si se usa una expresión, el peso de cada punto de datos puede basarse en algunas métricas de las propiedades de dichos puntos de datos. Si tomamos los datos de un terremoto como ejemplo, cada punto de datos representaría un terremoto. Las métricas importantes que tenga cada punto de datos se considerarán valores de magnitud. Los terremotos se producen a menudo, pero la mayoría tienen una magnitud baja y casi no se sienten. Si utilizamos el valor de magnitud de una expresión para asignar el peso a cada punto de datos, conseguiremos que los terremotos más importantes se representen mejor en el mapa térmico.
* Además de las opciones de la capa base, el zoom mínimo y máximo, la opción Visible y el filtro, también está la opción `source` si desea actualizar el origen de datos y la opción `source-layer` si el origen de dato es un origen de corte vectorial.

A continuación, se muestra una herramienta para probar las diferentes opciones de la capa de mapa térmico.

<br/>

<iframe height='700' scrolling='no' title='Opciones de la capa de mapa térmico' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>Opciones de la capa de mapa térmico</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Al habilitar la agrupación en clústeres en el origen de datos, los puntos que están cerca unos de otros se agrupan como un punto agrupado. El número de puntos de cada clúster puede utilizarse como la expresión de peso para el mapa térmico y reducir significativamente el número de puntos que se deben representar. El número de puntos de un clúster se almacena en la propiedad `point_count property` de la característica de puntos, tal y como se muestra a continuación. 
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

