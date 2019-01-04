---
title: Adición de un elemento emergente con Azure Maps | Microsoft Docs
description: Procedimiento para agregar un elemento emergente a un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 11/09/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: a6c8a8aa954379036ce566a205b8cb4e97952727
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "52887854"
---
# <a name="add-a-popup-to-the-map"></a>Adición de un elemento emergente al mapa

En este artículo se muestra cómo agregar un elemento emergente a un punto de un mapa.

## <a name="understand-the-code"></a>Comprendiendo el código

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Adición de un elemento emergente con Azure Maps' src='//codepen.io/azuremaps/embed/MPRPvz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/MPRPvz/'>Adición de un elemento emergente con Azure Maps</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones. También crea contenido HTML que se mostrará en el elemento emergente.

El segundo bloque de código crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Un punto es un objeto [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) de la clase [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest). A continuación, se crea un objeto de punto con las propiedades de nombre y descripción y, después, se agrega al origen de datos.

Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa.  En el tercer bloque de código, se crea una capa de símbolos. El origen de datos se agrega a la capa de símbolos y, después, esta se agrega al mapa.

El cuarto bloque de código crea un [objeto Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) mediante `new atlas.Popup()`. Las propiedades del elemento emergente, como pixelOffset y la posición, forman parte de [PopupOptions](/javascript/api/azure-maps-control/atlas.popupoptions). PopupOptions se puede definir en el constructor del elemento emergente o a través de la función [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) de la clase Popup. A continuación, se crea un agente de escucha de eventos `mouseover` para la capa de símbolos.

El último bloque de código crea una función que desencadena el agente de escucha de eventos `mouseover`. Establece las propiedades de la ventana emergente y el contenido, y agrega el objeto del elemento emergente al mapa.

## <a name="reusing-a-popup-with-multiple-points"></a>Reutilizar un elemento emergente con varios puntos

Cuando tiene una gran cantidad de puntos y solo desea mostrar un elemento emergente a la vez, lo mejor es crear un elemento emergente y reutilizarlo, en lugar de crear un elemento emergente para cada característica de punto. Al hacerlo, el número de elementos DOM creados por la aplicación se reduce tanto que puede proporcionar un mejor rendimiento. En este ejemplo se crean tres características de punto. Si hace clic en cualquiera de ellas, se mostrará un menú emergente con el contenido de esa característica de punto.

<br/>

<iframe height='500' scrolling='no' title='Reutilizar un elemento emergente con varias marcas' src='//codepen.io/azuremaps/embed/rQbjvK/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen sobre <a href='https://codepen.io/azuremaps/pen/rQbjvK/'>reutilización de elementos emergentes con varias marcas</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
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
> [Adición de una forma](./map-add-shape.md)