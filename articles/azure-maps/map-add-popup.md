---
title: Adición de un elemento emergente a un punto de un mapa | Microsoft Azure Maps
description: En este artículo, obtendrá información sobre cómo agregar un elemento emergente a un punto mediante el SDK web de Microsoft Azure Maps.
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 45d210725f7f09663b126528479655d7f4d9c19f
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76933355"
---
# <a name="add-a-popup-to-the-map"></a>Adición de un elemento emergente al mapa

En este artículo se muestra cómo agregar un elemento emergente a un punto de un mapa.

## <a name="understand-the-code"></a>Comprendiendo el código

El código siguiente agrega una característica de punto, que tiene las propiedades `name` y `description`, al mapa mediante una capa de símbolo. Se crea una instancia de la [clase popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) pero no se muestra. Los eventos del mouse se agregan a la capa de símbolos para desencadenar la apertura y el cierre del elemento emergente. Cuando el mouse se mantiene sobre el símbolo del marcador, la propiedad `position` del elemento emergente se actualiza con la posición del marcador y la opción `content` se actualiza con el código HTML que contiene las propiedades `name` y `description` de la característica de puntos que se está activando. Después, se muestra el elemento emergente en el mapa mediante su función `open`.

```javascript
//Define an HTML template for a custom popup content laypout.
var popupTemplate = '<div class="customInfobox"><div class="name">{name}</div>{description}</div>';

//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

dataSource.add(new atlas.data.Feature(new atlas.data.Point([-122.1333, 47.63]), {
  name: 'Microsoft Building 41', 
  description: '15571 NE 31st St, Redmond, WA 98052'
}));

//Create a layer to render point data.
var symbolLayer = new atlas.layer.SymbolLayer(dataSource);

//Add the polygon and line the symbol layer to the map.
map.layers.add(symbolLayer);

//Create a popup but leave it closed so we can update it and display it later.
popup = new atlas.Popup({
  pixelOffset: [0, -18],
  closeButton: false
});

//Add a hover event to the symbol layer.
map.events.add('mouseover', symbolLayer, function (e) {
  //Make sure that the point exists.
  if (e.shapes && e.shapes.length > 0) {
    var content, coordinate;
    var properties = e.shapes[0].getProperties();
    content = popupTemplate.replace(/{name}/g, properties.name).replace(/{description}/g, properties.description);
    coordinate = e.shapes[0].getCoordinates();

    popup.setOptions({
      //Update the content of the popup.
      content: content,

      //Update the popup's position with the symbol's coordinate.
      position: coordinate

    });
    //Open the popup.
    popup.open(map);
  }
});

map.events.add('mouseleave', symbolLayer, function (){
  popup.close();
});
```

A continuación se muestra el código de ejemplo de ejecución completo de la funcionalidad anterior.

<br/>

<iframe height='500' scrolling='no' title='Adición de un elemento emergente con Azure Maps' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>Adición de un elemento emergente con Azure Maps</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="reusing-a-popup-with-multiple-points"></a>Reutilizar un elemento emergente con varios puntos

Si tiene una gran cantidad de puntos y solo desea mostrar un elemento emergente a la vez, lo mejor es crear un único elemento emergente y reutilizarlo. Al reutilizar el elemento emergente, el número de elementos DOM creados por la aplicación se reduce tanto que puede proporcionar un mejor rendimiento. En el ejemplo siguiente se crean tres características de punto. Si hace clic en cualquiera de ellas, se mostrará un menú emergente con el contenido de esa característica de punto.

<br/>

<iframe height='500' scrolling='no' title='Reutilizar un elemento emergente con varias marcas' src='//codepen.io/azuremaps/embed/rQbjvK/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen sobre <a href='https://codepen.io/azuremaps/pen/rQbjvK/'>reutilización de elementos emergentes con varias marcas</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customizing-a-popup"></a>Personalización de un elemento emergente

De forma predeterminada, el elemento emergente tiene un fondo blanco, una flecha de puntero en la parte inferior y un botón de cierre en la esquina superior derecha. En el ejemplo siguiente se cambia el color de fondo a negro mediante la opción `fillColor` del elemento emergente. El botón de cierre se quita al establecer la opción `CloseButton` en false. El contenido HTML del elemento emergente utiliza un relleno de 10 píxeles desde los bordes del elemento emergente. El texto aparece en color blanco para que pueda verse claramente sobre el fondo negro.  

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Elemento emergente personalizado" src="//codepen.io/azuremaps/embed/ymKgdg/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el fragmento de código (pen) <a href='https://codepen.io/azuremaps/pen/ymKgdg/'>elemento emergente personalizado</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="popup-events"></a>Eventos emergentes

Los elementos emergentes se pueden abrir, cerrar y arrastrar. La clase del elemento emergente proporciona eventos que ayudan a los desarrolladores a reaccionar ante estos eventos. En el ejemplo siguiente, aparecen resaltados los eventos que se activan cuando un usuario abre, cierra o arrastra el elemento emergente. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Eventos emergentes" src="//codepen.io/azuremaps/embed/BXrpvB/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el fragmento de código (pen) <a href='https://codepen.io/azuremaps/pen/BXrpvB/'>eventos de elemento emergente</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popupoptions?view=azure-iot-typescript-latest)

Consulte los siguientes artículos para obtener ejemplos de código completo:

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Adición de un marcador HTML](./map-add-custom-html.md)

> [!div class="nextstepaction"]
> [Adición de una capa de línea](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de polígono](map-add-shape.md)
