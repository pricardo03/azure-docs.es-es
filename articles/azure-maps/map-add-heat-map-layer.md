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
ms.openlocfilehash: ec1343f85216171adac22f873f9be2e72bb4c282
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "52893318"
---
# <a name="add-a-heat-map-layer"></a>Adición de una capa de mapa térmico

Los mapas térmicos, también conocidos como mapas de densidad de puntos, son un tipo de visualización de datos utilizado para representar la densidad de los datos mediante una gama de colores. A menudo se utilizan para mostrar las "zonas activas" de datos en un mapa y son una forma excelente de representar grandes conjuntos de datos de puntos.  Por ejemplo, la representación de decenas de miles de puntos dentro de la vista del mapa cubre la mayor parte del área del mapa y daría lugar a que muchos símbolos se tapen entre sí, lo que dificulta la comprensión de los datos. Sin embargo, la visualización de este mismo conjunto de datos como un mapa térmico facilita la visualización dónde los datos de puntos son más densos y la densidad relativa a otras áreas. Hay muchos escenarios en los que se usan los mapas térmicos. Estos son algunos ejemplos:

* Los datos de temperatura normalmente se representan como mapa térmico, ya que este proporciona aproximaciones de temperatura entre dos puntos de datos.
* La representación de datos de sensores de ruido como un mapa térmico no solo muestra la alteración del ruido donde se encuentra el sensor, sino que también puede proporcionar información sobre la degradación a lo largo de una distancia. Puede que el nivel de ruido de cualquier sitio no sea elevado; sin embargo, si el área de cobertura de ruido de varios sensores se superpone, es posible que esta área superpuesta pueda experimentar niveles de ruido más elevados y, por tanto, serían visibles en el mapa térmico.
* La visualización de un seguimiento por GPS que incluya la velocidad como un mapa de altura y peso donde la intensidad de cada punto se basa en la velocidad es una forma magnífica de ver rápidamente dónde acelera el vehículo.

> [!TIP]
> Las capas de burbuja de forma predeterminada representarán las coordenadas de todos los objetos geométricos en un origen de datos. Para limitar la capa de tal forma que solo represente las características geométricas de punto, establezca la propiedad `filter` de la capa en `['==', '$type', 'Point']`.

## <a name="add-a-heat-map-layer"></a>Adición de una capa de mapa térmico

Para representar un origen de datos de puntos como un mapa térmico solo tiene que pasar el origen de datos a una instancia de la clase HeatMapLayer y agregarlo al mapa como se muestra aquí.

<br/>

<iframe height='500' scrolling='no' title='Capa de mapa térmico sencilla' src='//codepen.io/azuremaps/embed/gQqdQB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/gQqdQB/'>Capa de mapa térmico sencilla</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En este ejemplo, cada punto térmico tiene un radio de 10 píxeles a todos los niveles de zoom. Al agregar la capa de mapa térmico al mapa, en este ejemplo se inserta debajo de la capa de la etiqueta. De esta forma, se crea una experiencia de usuario mejorada, ya que las etiquetas se ven claramente por encima del mapa térmico. Los datos de este ejemplo proceden de [USGS Earthquake Hazards Program](https://earthquake.usgs.gov/) y constan de puntos que representan importantes terremotos que se han producido en los 30 últimos días.

## <a name="customizing-the-heat-map-layer"></a>Personalización de la capa de mapa térmico

En el ejemplo anterior se personalizó el mapa térmico con la configuración de las opciones de radio y opacidad. La capa de mapa térmico ofrece varias opciones de personalización:

* `radius`: define un radio de píxel en el que representar cada punto de datos. El radio puede establecerse como un número fijo o como una expresión. Establecer el radio como una expresión que establece el radio según el nivel de zoom puede dar lugar a mapas térmicos que parecen tener un radio que representa un área espacial coherente en el mapa.
* `color`: especifica cómo se colorea el mapa térmico. Se suele utilizar una paleta de colores degradados para los mapas térmicos, pero las paletas de colores escalonados también son útiles si desea que el mapa térmico se asemeje más a datos de contorno. Las paletas de colores definen los colores desde valores de intensidad mínimos hasta máximos. Los valores de color de los mapas térmicos se especifican como una expresión en el valor `heatmap-density`. El color del índice 0 de una expresión de degradado o el color predeterminado de un color escalonado define el color del área donde no hay ningún dato o el color de fondo. Muchos prefieren establecer este valor en transparente o en un negro semitransparente. Estos son ejemplos de expresiones de color:

| Expresión de color degradado | Expresión de color escalonado | 
|---------------------------|--------------------------|
| \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'interpolate',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['linear'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;0, 'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'purple',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.5, '#fb00fb',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1, '#00c3ff'<br/>\] | \[<br/>&nbsp;&nbsp;&nbsp;&nbsp;'step',<br/>&nbsp;&nbsp;&nbsp;&nbsp;\['heatmap-density'\],<br/>&nbsp;&nbsp;&nbsp;&nbsp;'transparent',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.01, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.25, 'navy',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.50, 'green',<br/>&nbsp;&nbsp;&nbsp;&nbsp;0.75, 'yellow',<br/>&nbsp;&nbsp;&nbsp;&nbsp;1.00, 'red'<br/>\] |   

* `opacity`: especifica el grado de opacidad o transparencia de la capa de mapa térmico.
* `intensity`: aplica un multiplicador al peso de cada punto de datos para aumentar la intensidad general del mapa térmico. Esto ayuda a que las pequeñas diferencias en el peso de los puntos de datos resulten más fáciles de visualizar.
* `weight`: de forma predeterminada, todos los puntos de datos tienen un peso de 1; por lo tanto, todos los puntos de datos se ponderan equitativamente. La opción de peso actúa como un multiplicador y puede establecerse como un número o una expresión. Si se establece un número como número, digamos 2, sería el equivalente a colocar cada punto de datos dos veces en el mapa, duplicando así la intensidad. Establecer la opción de peso en un número representa el mapa térmico de forma similar a la opción de intensidad. Sin embargo, si se usa una expresión, el peso de cada punto de datos puede basarse en otros aspectos, como alguna métrica de las propiedades del punto. Si consideramos los datos de los terremotos como ejemplo, cada punto de datos representa un terremoto y una métrica importante que cada terremoto tiene es la magnitud. Los terremotos se producen a menudo, pero la mayoría tienen una magnitud baja y casi no se sienten. El uso del valor de magnitud en una expresión para asignar la opción de peso dará lugar a que se destaquen más los terremotos más importantes, a fin de que estén mejor representados en el mapa térmico.
* Además de las opciones de la capa base, es decir, zoom mínimo y máximo, visible y filtro, también existe una opción `source` si desea actualizar el origen de datos y la opción `source-layer` si el origen de datos es un origen de mosaico de vector.

A continuación, se muestra una herramienta para probar las diferentes opciones de la capa de mapa térmico.

<br/>

<iframe height='700' scrolling='no' title='Opciones de la capa de mapa térmico' src='//codepen.io/azuremaps/embed/WYPaXr/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WYPaXr/'>Opciones de la capa de mapa térmico</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Al habilitar la agrupación en clústeres en el origen de datos, los puntos que están cerca unos de otros se agrupan como un punto agrupado. El número de puntos de cada clúster puede utilizarse como la expresión de peso para el mapa térmico y reducir significativamente el número de puntos que se deben representar. El número de punto de un clúster se almacena en la propiedad point_count de la característica de puntos como se muestra a continuación. 
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

