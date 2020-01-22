---
title: Funcionalidades de estilo de mapa | Microsoft Azure Maps
description: En este artículo, obtendrá información sobre las funcionalidades relacionadas con el estilo disponibles en el SDK web de Microsoft Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: b92e03c4e5346dd39eaba84cfeeedb93e418678c
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911787"
---
# <a name="choose-a-map-style-in-azure-maps"></a>Elegir un estilo de mapa en Azure Maps

Muchos de los [estilos de mapa admitidos en Azure Maps](./supported-map-styles.md) están disponibles en el SDK para web. En este artículo se muestra cómo usar las funcionalidades relacionadas con el estilo para establecer un estilo al cargar un mapa, establecer un estilo nuevo y usar el control de selector de estilos.

## <a name="set-style-on-map-load"></a>Establecimiento del estilo al cargar un mapa

En el código siguiente, la opción `style` del mapa se establece en `grayscale_dark` al inicializarse.

<br/>

<iframe height='500' scrolling='no' title='Establecer el estilo al cargar un mapa' src='//codepen.io/azuremaps/embed/WKOQRq/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/WKOQRq/'>Setting the style on map load</a> (Establecer el estilo al cargar un mapa) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="update-the-style"></a>Actualización del estilo

En el código siguiente, después de cargar una instancia de mapa, el estilo de mapa se actualiza desde `road` a `satellite` con la función [setStyle](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) del mapa.

<br/>

<iframe height='500' scrolling='no' title='Actualizar el estilo' src='//codepen.io/azuremaps/embed/yqXYzY/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/yqXYzY/'>Updating the style</a> (Actualizar el estilo) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-the-style-picker"></a>Adición del selector de estilos

El código siguiente agrega un [StyleControl](/javascript/api/azure-maps-control/atlas.control.stylecontrol) al mapa para que el usuario pueda cambiar fácilmente entre los distintos estilos de mapa. 

<br/>

<iframe height='500' scrolling='no' title='Agregar el selector de estilos' src='//codepen.io/azuremaps/embed/OwgyvG/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/OwgyvG/'>Adding the style picker</a> (Agregar el selector de estilos) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> De forma predeterminada, el control selector de estilo muestra todos los estilos disponibles cuando se usa el plan de tarifa S0 de Azure Maps de forma predeterminada. Si desea reducir el número de estilos de esta lista, pase una matriz de los estilos que desea que aparezcan en la lista a la opción `mapStyle` del selector de estilo. Si está utilizando S1 y desea mostrar todos los estilos disponibles, establezca la opción `mapStyles` del selector de estilo en `"all"`.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Adición de controles a los mapas:

> [!div class="nextstepaction"]
> [Incorporación de controles de mapa](map-add-controls.md)

> [!div class="nextstepaction"]
> [Adición de un anclaje](map-add-pin.md)
