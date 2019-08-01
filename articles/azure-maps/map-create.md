---
title: Creación de un mapa con Azure Maps | Microsoft Docs
description: Procedimiento para crear un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 8a4f67290e93d8b296added9023fe9b6947ba02c
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638685"
---
# <a name="create-a-map"></a>Creación de un mapa

En este artículo se muestran formas de crear y animar un mapa.  

## <a name="understand-the-code"></a>Comprendiendo el código

Hay dos maneras de crear un mapa. Puede establecer la cámara del mapa especificando el punto central y el nivel de zoom o puede establecer los límites de la cámara del mapa especificando los puntos de límite suroeste y nordeste.

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Establecimiento de la cámara

<iframe height='500' scrolling='no' title='Creación de un mapa mediante CameraOptions' src='//codepen.io/azuremaps/embed/qxKBMN/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>Creación de un mapa mediante `CameraOptions` </a>de Azure Location Based Services (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, se crea un [objeto Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) a través de `new atlas.Map()` y se establecen el centro y el zoom. Las propiedades del mapa, como el centro y el nivel de zoom, forman parte de [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions).

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Establecimiento de los límites de cámara

<iframe height='500' scrolling='no' title='Creación de un mapa mediante CameraBoundsOptions' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>Creación de un mapa mediante `CameraBoundsOptions` </a>de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior se crea un [objeto mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) mediante `new atlas.Map()`. Las propiedades del mapa, como `CameraBoundsOptions`, se pueden definir a través de la función [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) de la clase Map. Los límites y las propiedades de relleno se establecen mediante `setCamera`.

### <a name="animate-map-view"></a>Animación de la vista de mapa

<iframe height='500' scrolling='no' title='Animación de la vista de mapa' src='//codepen.io/azuremaps/embed/WayvbO/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WayvbO/'>Animación de la vista de mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código crea un [objeto Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) mediante `new atlas.Map()`. Las propiedades del mapa, como el centro y el nivel de zoom, forman parte de [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions). `CameraOptions` se puede definir en el constructor del mapa o a través de la función [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) de la clase mapa. El [estilo de mapa](https://docs.microsoft.com/azure/azure-maps/supported-map-styles) ese establece en `road`.

El segundo bloque de código crea una función de animación de mapa, que anima el cambio en la vista del mapa definiendo [AnimateOptions](/javascript/api/azure-maps-control/atlas.animationoptions) a través de la función [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest). La función se desencadena mediante el botón "Animate Map" (Animar mapa) para generar un nivel de zoom aleatorio tras cada clic.

## <a name="try-out-the-code"></a>Prueba del código

Eche un vistazo al código de ejemplo anterior. Puede modificar el código JavaScript en la **pestaña JS** a la izquierda y ver los cambios de la vista de mapa en la **pestaña de resultados** de la derecha. También puede hacer clic en el botón **Editar en CodePen** y modificar el código en CodePen.

<a id="relatedReference"></a>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Vea los siguientes ejemplos de código para agregar funcionalidad a la aplicación:

> [!div class="nextstepaction"]
> [Elección de un estilo de mapa](choose-map-style.md)

> [!div class="nextstepaction"]
> [Incorporación de controles de mapa](map-add-controls.md)
