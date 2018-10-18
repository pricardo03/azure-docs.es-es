---
title: Creación de un mapa con Azure Maps | Microsoft Docs
description: Procedimiento para crear un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 09/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 9759c4149c6b026837e550dcf3ab0a0156bbb736
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730016"
---
# <a name="create-a-map"></a>Creación de un mapa

Este artículo muestra cómo crear un mapa.  

## <a name="understand-the-code"></a>Comprensión del código

Hay dos maneras de crear un mapa. Puede establecer la cámara del mapa especificando el punto central y el nivel de zoom. Establezca los límites de cámara del mapa especificando el punto del límite suroeste y el del nordeste.

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Establecimiento de la cámara

<iframe height='310' scrolling='no' title='Creación de un mapa mediante CameraOptions' src='//codepen.io/azuremaps/embed/qxKBMN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>Creación de un mapa mediante `CameraOptions` </a>de Azure Location Based Services (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior se crea un [objeto mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) mediante `new atlas.Map()`. Las propiedades del mapa, como el centro y el nivel de zoom, forman parte de [CameraOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraoptions?view=azure-iot-typescript-latest). `CameraOptions` se puede definir en el constructor del mapa o a través de la función [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera) de la clase mapa.

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Establecimiento de los límites de cámara

<iframe height='310' scrolling='no' title='Creación de un mapa mediante CameraBoundsOptions' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>Creación de un mapa mediante `CameraBoundsOptions` </a>de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior se crea un [objeto mapa](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) mediante `new atlas.Map()`. Las propiedades del mapa, como el cuadro de límite, forman parte de [CameraBoundsOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest). Se puede definir `CameraBoundsOptions` a través de la función [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) de la clase mapa.

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
