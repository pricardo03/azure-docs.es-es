---
title: Funcionalidades de estilo de mapa en Azure Maps | Microsoft Docs
description: Obtenga información sobre las funcionalidades de Azure Maps relacionadas con el estilo.
author: walsehgal
ms.author: v-musehg
ms.date: 08/31/2018
ms.topic: how-to-guides
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: b95e3c325160d1c0b775320e25fdd40514b02b9f
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666878"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Elegir un estilo de mapa en Azure Maps
Azure Maps tiene cuatro estilos de mapas entre los que puede elegir. Para obtener más información sobre los estilos de mapa, consulte [Estilos de mapa admitidos en Azure Maps](./supported-map-styles.md). En este artículo se muestra cómo usar las funcionalidades relacionadas con el estilo para establecer un estilo al cargar un mapa, establecer un estilo nuevo y usar el control de selector de estilos.

## <a name="setting-style-on-map-load"></a>Establecer el estilo al cargar un mapa

<iframe height='500' scrolling='no' title='Establecer el estilo al cargar un mapa' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>Setting the style on map load</a> (Establecer el estilo al cargar un mapa) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El código anterior crea un objeto de mapa con el estilo establecido en escala de grises. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

## <a name="updating-the-style"></a>Actualizar el estilo

<iframe height='500' scrolling='no' title='Actualizar el estilo' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>Updating the style</a> (Actualizar el estilo) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código en el código anterior crea un objeto de mapa sin tener que configurar previamente el estilo. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El segundo reloj de código usa el método [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle) del mapa para establecer el estilo de mapa a satélite.

## <a name="adding-the-style-picker"></a>Agregar el selector de estilos

<iframe height='500' scrolling='no' title='Agregar el selector de estilos' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>Adding the style picker</a> (Agregar el selector de estilos) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código en el código anterior crea un objeto de mapa sin tener que configurar previamente el estilo. Consulte [Crear un mapa](./map-create.md) para obtener instrucciones sobre cómo crear un mapa.

El segundo bloque de código construye un selector de estilo mediante el constructor de atlas [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol).

Un selector de estilos habilita la selección de estilo para el mapa. El tercer bloque de código agrega el selector de estilos para el mapa mediante el método [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol) del mapa.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo: 
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setstyle)
    * [addControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addcontrol)

* [Atlas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas?view=azure-iot-typescript-latest)
    * [StyleControl](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.control.stylecontrol?view=azure-iot-typescript-latest#stylecontrol)
    
Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos:
* [Agregar controles de mapa](./map-add-controls.md)
* [Adición de un anclaje](./map-add-pin.md)
