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
ms.openlocfilehash: f56e15e12bc176e6b6837e144494599ea4fb5403
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282539"
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

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Consulte los siguientes artículos para obtener ejemplos de código completo:

> [!div class="nextstepaction"]
> [Adición de una forma](./map-add-shape.md)

> [!div class="nextstepaction"]
> [Adición de código HTML personalizado](./map-add-custom-html.md)
