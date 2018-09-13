---
title: Agregar controles de mapa en Azure Maps | Microsoft Docs
description: Cómo agregar control de zoom, control de inclinación, control de giro y un selector de estilos a un mapa en Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 08/29/2018
ms.topic: how-to-guides
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: 435c6545b69b4457c3e1035fb8125399d4c9c23a
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666138"
---
# <a name="add-map-controls-to-azure-maps"></a>Agregar controles de mapa a Azure Maps

En este artículo se muestra cómo agregar controles de mapa a un mapa. También se ofrece información sobre cómo crear un mapa con todos los controles y un [selector de estilos](https://docs.microsoft.com/azure/azure-maps/choose-map-style#adding-the-style-picker).

## <a name="add-zoom-control"></a>Agregar un control de zoom

<iframe height='500' scrolling='no' title='Adición de un control de zoom' src='//codepen.io/azuremaps/embed/WKOQyN/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WKOQyN/'>Adding a zoom control</a> (Agregar un control de zoom) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código del código anterior crea un objeto de mapa. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El segundo bloque de código crea un objeto de control de zoom mediante el atlas [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest).

El control de zoom agrega la capacidad de acercar o alegar la vista en el mapa. El tercer bloque agrega el control de zoom al mapa mediante el método [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) del mapa.

## <a name="add-pitch-control"></a>Agregar un control de inclinación

<iframe height='500' scrolling='no' title='Adición de un control de inclinación' src='//codepen.io/azuremaps/embed/xJrwaP/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/xJrwaP/'>Adding a pitch control</a> (Agregar un control de inclinación) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código del código anterior crea un objeto de mapa con el estilo establecido en Escala de grises. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El segundo bloque de código crea un objeto de control de inclinación mediante el atlas [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest).

El control de inclinación agrega la capacidad de cambiar la inclinación del mapa. El tercer bloque agrega el control de inclinación al mapa mediante el método [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) del mapa.

## <a name="add-compass-control"></a>Agregar un control de brújula

<iframe height='500' scrolling='no' title='Adición de un control de giro' src='//codepen.io/azuremaps/embed/GBEoRb/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/GBEoRb/'>Adding a rotate control</a> (Agregar un control de giro) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código del código anterior crea un objeto de mapa. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El segundo bloque de código crea un objeto de control de brújula mediante el atlas [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol). También agrega el control de brújula al mapa mediante el método [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) del mapa.

## <a name="a-map-with-all-controls"></a>Mapa con todos los controles

<iframe height='500' scrolling='no' title='Mapa con todos los controles' src='//codepen.io/azuremaps/embed/qyjbOM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qyjbOM/'>A map with all the controls</a> (Mapa con todos los controles) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código del código anterior crea un objeto de mapa. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El segundo bloque de código crea un objeto de control brújula mediante el atlas [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol) y lo agrega al mapa mediante el método [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) del mapa.

El tercer bloque de código crea un objeto de control de zoom mediante la atlas [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest) y lo agrega al mapa mediante el método [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) del mapa.

El cuarto bloque de código crea un objeto de control de inclinación mediante el atlas [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest) y lo agrega al mapa mediante el método [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) del mapa.

El último bloque de código agrega un objeto de selector de estilos mediante el atlas [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol) y lo agrega al mediante el método [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo: 
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)

* [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)
    * [CompassControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.compasscontrol?view=azure-iot-typescript-latest#compasscontrol)
    * [ZoomControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.zoomcontrol?view=azure-iot-typescript-latest)
    * [PitchControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.pitchcontrol?view=azure-iot-typescript-latest)
    * [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol)
    
Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos: 
* [Adición de un anclaje](./map-add-pin.md)
* [Adición de un elemento emergente](./map-add-popup.md)