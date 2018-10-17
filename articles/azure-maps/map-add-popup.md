---
title: Adición de un elemento emergente con Azure Maps | Microsoft Docs
description: Procedimiento para agregar un elemento emergente a un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 09/17/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 76a7e230491d5e524a1d73437a56d12594cfebe2
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127444"
---
# <a name="add-a-popup-to-the-map"></a>Adición de un elemento emergente al mapa

Este artículo muestra cómo agregar un elemento emergente a un mapa.  

## <a name="understand-the-code"></a>Comprensión del código

<a id="addAPopup"></a>

<iframe height='500' scrolling='no' title='Adición de un elemento emergente a un mapa' src='//codepen.io/azuremaps/embed/zRyKxj/?height=545&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/zRyKxj/'>Adición de un elemento emergente a un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea un anclaje y lo agrega al mapa. Puede consultar [Adición de un anclaje al mapa](./map-add-pin.md) para obtener instrucciones.

El tercer bloque de código crea el contenido que se mostrará en un elemento emergente. El contenido del elemento emergente es un elemento HTML.

El cuarto bloque de código crea un [objeto Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) mediante `new atlas.Popup()`. Las propiedades del elemento emergente, como el contenido y la posición, forman parte de [PopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.popupoptions?view=azure-iot-typescript-latest). PopupOptions se puede definir en el constructor del elemento emergente o a través de la función [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) de la clase Popup.

El último bloque de código usa la función [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener) de la clase Map para realizar escuchas para el evento mouseover en los anclajes y usa la función [open](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) de la clase Popup para abrir el elemento emergente si se produce el evento.

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
