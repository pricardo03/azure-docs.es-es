---
title: Adición de un marcador HTML a Azure Maps | Microsoft Docs
description: Cómo agregar un marcador HTML al mapa de JavaScript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 1c812a77429e13ea39b2f4946043c13e10aaf097
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55993749"
---
# <a name="add-html-markers-to-the-map"></a>Agregar marcadores HTML al mapa

En este artículo se explica cómo agregar al mapa código HTML personalizado como un archivo de imagen en forma de marcador HTML.

> [!NOTE]
> Los marcadores HTML no se conectan a los orígenes de datos. En su lugar, se agrega información de posición directamente en el marcador y el marcador se agrega a la propiedad `markers` de los mapas, que es un elemento [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest).

> [!IMPORTANT]
> A diferencia de la mayoría de las capas del control web de Azure Maps que usan WebGL para la representación, los marcadores HTML usan elementos DOM tradicionales para la representación. Por tanto, mientras más marcadores HTML se agreguen a una página, más elementos DOM habrá. Se puede degradar el rendimiento después de agregar cientos de marcadores HTML. Para conjuntos de datos grandes, considere la posibilidad de agrupar los datos en clústeres o de usar una capa de símbolo o burbuja.

## <a name="add-an-html-marker"></a>Adición de un marcador HTML

La clase HtmlMarker tiene un estilo predeterminado. Puede personalizar el marcador mediante la configuración de las opciones de color y texto del marcador. El estilo predeterminado de la clase HtmlMarker es una plantilla SVG que tiene un marcador de posición de color y texto. Configure las propiedades de color y texto en las opciones de la clase HtmlMarker para realizar una personalización rápida. 

<br/>

<iframe height='500' scrolling='no' title='Adición de un marcador HTML a un mapa' src='//codepen.io/azuremaps/embed/MVoeVw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>Adición de un marcador HTML a un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código se agrega [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) al mapa mediante la propiedad [markers](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#markers) de la clase [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest). HtmlMarker se agrega al mapa dentro de la función [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para garantizar que se muestre una vez que el mapa se cargue completamente.

## <a name="create-svg-templated-html-marker"></a>Creación de un marcador HTML con una plantilla SVG

El valor predeterminado `htmlContent` de un marcador HTML es una plantilla SVG que contiene las carpetas de posición `{color}` y `{text}`. Puede crear cadenas SVG personalizadas y agregar estos mismos marcadores de posición en las opciones SVG, de tal forma que la configuración de las opciones `color` y `text` del marcador actualice estos marcadores de posición en la plantilla SVG.

<br/>

<iframe height='500' scrolling='no' title='Marcador HTML con la plantilla SVG personalizada' src='//codepen.io/azuremaps/embed/LXqMWx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/LXqMWx/'>Marcador HTML con la plantilla SVG personalizada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-a-css-styled-html-marker"></a>Adición de un marcador HTML con estilo CSS

Una de las ventajas de los marcadores HTML es que hay muchas personalizaciones excelentes que pueden conseguirse mediante CSS. En este ejemplo, el contenido de HtmlMarker consta de HTML y CSS que crean un pin animado que se coloca en su lugar y se mueve.

<br/>

<iframe height='500' scrolling='no' title='HTML DataSource' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>HTML DataSource</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="draggable-html-markers"></a>Marcadores HTML arrastrables

En este ejemplo se muestra cómo hacer que un marcador HTML se pueda arrastrar. Los marcadores HTML admiten eventos `drag`, `dragstart` y `dragend`.

<br/>

<iframe height='500' scrolling='no' title='Marcador HTML arrastrable' src='//codepen.io/azuremaps/embed/wQZoEV/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/wQZoEV/'>Marcador HTML arrastrable</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-mouse-events-to-html-markers"></a>Adición de eventos del mouse a los marcadores HTML

En estos ejemplos se muestra cómo agregar eventos del mouse y de arrastrar a un marcador HTML.

<br/>

<iframe height='500' scrolling='no' title='Adición de eventos del mouse a los marcadores HTML' src='//codepen.io/azuremaps/embed/RqOKRz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/RqOKRz/'>Adición de eventos del mouse a los marcadores HTML</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos:

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Adición de una capa de burbuja](./map-add-bubble-layer.md)