---
title: Creación de un mapa con Azure Maps | Microsoft Azure Maps
description: En este artículo, se aprende a representar un mapa en una página web mediante el SDK web de Microsoft Azure Maps.
author: jingjing-z
ms.author: jinzh
ms.date: 07/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 578abae5b206b31674b00b9d27ef34174b93759f
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76988590"
---
# <a name="create-a-map"></a>Creación de un mapa

En este artículo se muestran formas de crear y animar un mapa.  

## <a name="loading-a-map"></a>Carga de un mapa

Para cargar una asignación, cree una nueva instancia de la [clase Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest). Al inicializar el mapa, se pasa un identificador de elemento DIV para representar el mapa y un conjunto de opciones que se van a usar cuando se cargue este mapa. Si no se especifica la información de autenticación predeterminada en el espacio de nombres `atlas`, esta información tendrá que especificarse en las opciones del mapa cuando se cargue el mapa. El mapa carga varios recursos de forma asincrónica para mayor rendimiento. Por lo tanto, después de crear la instancia del mapa, asocie un evento `ready` o `load` al mapa y, luego, agregue al controlador de eventos código adicional que interactúe con el mapa. El evento `ready` se activa en cuanto el mapa tiene suficientes recursos cargados para que se interactúe con él mediante programación. El evento `load` se activa después de que la vista inicial del mapa ha terminado de cargarse por completo. 

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Carga básica del mapa" src="//codepen.io/azuremaps/embed/rXdBXx/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/rXdBXx/'>Basic map load</a> (Carga básica del mapa) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Puede cargar varios mapas en la misma página. Cuando hay varios mapas en la misma página, se puede usar la misma configuración de autenticación e idioma u otra diferente.

## <a name="show-a-single-copy-of-the-world"></a>Mostrar una sola copia del mundo

Cuando se aleja el mapa en una pantalla ancha, se mostrarán varias copias del mundo horizontalmente. Esta opción es excelente en algunos escenarios, pero para otras aplicaciones es aconsejable tener una sola copia del mundo. Para implementar este comportamiento se establece la opción `renderWorldCopies` de los mapas en `false`.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="renderWorldCopies = false" src="//codepen.io/azuremaps/embed/eqMYpZ/?height=500&theme-id=0&default-tab=js,result" frameborder="no" allowtransparency="true" allowfullscreen="true">
Consulte el Pen <a href='https://codepen.io/azuremaps/pen/eqMYpZ/'>renderWorldCopies = false</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="controlling-the-map-camera"></a>Control de la cámara del mapa

Hay dos maneras de establecer el área mostrada del mapa mediante la cámara de un mapa. Puede establecer las opciones de la cámara al cargar el mapa. O bien, puede llamar a la opción `setCamera` en cualquier momento después de que se haya cargado el mapa para actualizar la vista del mapa mediante programación.  

<a id="setCameraOptions"></a>

### <a name="set-the-camera"></a>Establecimiento de la cámara

En el código siguiente, se crea un [objeto Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) y se establecen las opciones de centro y zoom. Las propiedades del mapa, como centro y nivel de zoom, forman parte de [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions).

<br/>

<iframe height='500' scrolling='no' title='Creación de un mapa mediante CameraOptions' src='//codepen.io/azuremaps/embed/qxKBMN/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qxKBMN/'>Creación de un mapa mediante `CameraOptions` </a>de Azure Location Based Services (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

<a id="setCameraBoundsOptions"></a>

### <a name="set-the-camera-bounds"></a>Establecimiento de los límites de cámara

En el código siguiente, se crea un [objeto Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) mediante `new atlas.Map()`. Las propiedades del mapa, como `CameraBoundsOptions`, se pueden definir a través de la función [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) de la clase Map. Los límites y las propiedades de relleno se establecen mediante `setCamera`.

<br/>

<iframe height='500' scrolling='no' title='Creación de un mapa mediante CameraBoundsOptions' src='//codepen.io/azuremaps/embed/ZrRbPg/?height=543&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ZrRbPg/'>Creación de un mapa mediante `CameraBoundsOptions` </a>de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

### <a name="animate-map-view"></a>Animación de la vista de mapa

En el código siguiente, el primer bloque de código crea un mapa y establece los estilos de mapa de zoom y entrada. En el segundo bloque de código, se crea un controlador de eventos click para el botón Animate. Cuando se hace clic en este botón, se llama a la función `setCamera` con algunos valores aleatorios para [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions) y [AnimationOptions](/javascript/api/azure-maps-control/atlas.animationoptions).

<br/>

<iframe height='500' scrolling='no' title='Animación de la vista de mapa' src='//codepen.io/azuremaps/embed/WayvbO/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WayvbO/'>Animación de la vista de mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="try-out-the-code"></a>Prueba del código

Examine los ejemplos de código. Puede editar el código JavaScript dentro de la **pestaña JS** y ver los cambios en la vista del mapa en la **pestaña Resultado**. También puede hacer clic en **Editar en CodePen**, en la esquina superior derecha, y modificar el código en CodePen.

<a id="relatedReference"></a>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [CameraOptions](/javascript/api/azure-maps-control/atlas.cameraoptions)

> [!div class="nextstepaction"]
> [AnimationOptions](/javascript/api/azure-maps-control/atlas.animationoptions)

Vea los siguientes ejemplos de código para agregar funcionalidad a la aplicación:

> [!div class="nextstepaction"]
> [Cambio del estilo del mapa](choose-map-style.md)

> [!div class="nextstepaction"]
> [Incorporación de controles al mapa](map-add-controls.md)

> [!div class="nextstepaction"]
> [Ejemplos de código](https://docs.microsoft.com/samples/browse/?products=azure-maps)
