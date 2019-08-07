---
title: Agregar controles de mapa en Azure Maps | Microsoft Docs
description: Cómo agregar control de zoom, control de inclinación, control de giro y un selector de estilos a un mapa en Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 7a504b8df199a3a461d5eb4e5b7238462b4c438f
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638774"
---
# <a name="add-map-controls-to-azure-maps"></a>Agregar controles de mapa a Azure Maps

En este artículo se muestra cómo agregar controles de mapa a un mapa. También se ofrece información sobre cómo crear un mapa con todos los controles y un [selector de estilos](https://docs.microsoft.com/azure/azure-maps/choose-map-style).

## <a name="add-zoom-control"></a>Agregar un control de zoom

<iframe height='500' scrolling='no' title='Adición de un control de zoom' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>Adding a zoom control</a> (Agregar un control de zoom) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código crea un objeto de mapa mediante el mecanismo de autenticación anónima. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El control de zoom agrega la capacidad de acercar o alegar la vista en el mapa. El segundo bloque de código crea un objeto de control de zoom mediante el atlas [ZoomControl](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) y lo agrega al mapa mediante el método [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) del mapa. El control de zoom está en el **agente de escucha de eventos** del mapa para asegurarse de que se carga después de que el mapa se cargue completamente.

## <a name="add-pitch-control"></a>Agregar un control de inclinación

<iframe height='500' scrolling='no' title='Adición de un control de inclinación' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>Adding a pitch control</a> (Agregar un control de inclinación) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código crea un objeto de mapa mediante el mecanismo de autenticación anónima. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El control de inclinación agrega la capacidad de cambiar la inclinación del mapa. El segundo bloque de código crea un objeto de control de inclinación mediante el atlas [PitchControl](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) y lo agrega al mapa mediante el método [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) del mapa. El control de inclinación está en el **agente de escucha de eventos** del mapa para asegurarse de que se carga después de que el mapa se cargue completamente.

## <a name="add-compass-control"></a>Agregar un control de brújula

<iframe height='500' scrolling='no' title='Adición de un control de giro' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>Adding a rotate control</a> (Agregar un control de giro) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código crea un objeto de mapa mediante el mecanismo de autenticación anónima. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El segundo bloque de código crea un objeto de control de brújula mediante el atlas [CompassControl](/javascript/api/azure-maps-control/atlas.control.compasscontrol). También agrega el control de brújula al mapa mediante el método [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) del mapa. El control de brújula está en el **agente de escucha de eventos** del mapa para asegurarse de que se carga después de que el mapa se cargue completamente.

## <a name="a-map-with-all-controls"></a>Mapa con todos los controles

<iframe height='500' scrolling='no' title='Mapa con todos los controles' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>A map with all the controls</a> (Mapa con todos los controles) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código crea un objeto de mapa mediante el mecanismo de autenticación anónima. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El segundo bloque de código crea un objeto de control brújula mediante el atlas [CompassControl](/javascript/api/azure-maps-control/atlas.control.compasscontrol) y lo agrega al mapa mediante el método [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) del mapa.

El tercer bloque de código crea un objeto de control de zoom mediante la atlas [ZoomControl](/javascript/api/azure-maps-control/atlas.control.zoomcontrol) y lo agrega al mapa mediante el método [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) del mapa.

El cuarto bloque de código crea un objeto de control de inclinación mediante el atlas [PitchControl](/javascript/api/azure-maps-control/atlas.control.pitchcontrol) y lo agrega al mapa mediante el método [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) del mapa.

El último bloque de código crea un objeto de selector de estilos mediante el atlas [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) y lo agrega al mapa mediante el método [controls.add](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) del mapa. Todos los objetos de control se agregan al **agente de escucha de eventos** del mapa para asegurarse de que se cargan después de que el mapa se carga completamente.

El orden de los objetos de control del script determina el orden en que aparecen en el mapa. Para cambiar el orden de los controles en el mapa, puede cambiar su orden en el script.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)

Consulte los siguientes artículos para el código completo:

> [!div class="nextstepaction"]
> [Adición de un anclaje](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Adición de un elemento emergente](./map-add-popup.md)
