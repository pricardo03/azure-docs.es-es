---
title: Control de los eventos del mouse con Azure Maps | Microsoft Docs
description: Procedimiento para crear un mapa interactivo de Javascript con eventos de mapa
author: jingjing-z
ms.author: jinzh
ms.date: 11/29/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 4fce8eae25942d098bb3f3277938bfaa3dafa00b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60770455"
---
# <a name="interact-with-the-map---mouse-events"></a>Interacción con el mapa: eventos del mouse

En este artículo se muestra cómo usar la propiedad de [eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) [clase Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) para resaltar eventos en el mapa y en distintas capas de este. También se muestra cómo usar la propiedad de eventos clase Map para resaltar los eventos cuando se interactúa con un marcador HTML.

## <a name="interact-with-the-map"></a>Interacción con el mapa

<iframe height='600' scrolling='no' title='Interacción con el mapa: eventos del mouse' src='//codepen.io/azuremaps/embed/bLZEWd/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/bLZEWd/'>Interacción con el mapa: eventos del mouse</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

Practique con el mapa anterior y observe los eventos del mouse correspondientes resaltados en la parte derecha. Puede hacer clic en la **pestaña JS** para ver y editar el código JavaScript. También puede hacer clic en el botón **Editar en CodePen** y modificar el código en CodePen.

## <a name="interact-with-map-layers"></a>Interacción con las capas de mapa

<iframe height='600' scrolling='no' title='Interacción con el mapa: eventos de las capas' src='//codepen.io/azuremaps/embed/bQRRPE/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/bQRRPE/'>Interacción con el mapa: eventos de las capas</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El código anterior resalta el nombre de los eventos que se activan al interactuar con la capa de símbolos. Las capas de símbolos, burbujas, líneas y polígonos admiten el mismo conjunto de eventos. La capa de mosaicos no es compatible con ninguno estos eventos.

## <a name="interact-with-html-marker"></a>Interacción con el marcador HTML

<iframe height='500' scrolling='no' title='Interacción con el mapa: eventos del marcador HTML' src='//codepen.io/azuremaps/embed/VVzKJY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/VVzKJY/'>Interacción con el mapa: eventos del marcador HTML</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El código anterior agrega eventos de mapa JavaScript a un marcador HTML. También resalta el nombre de los eventos que se activan al interactuar con el marcador HTML.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Consulte los siguientes artículos para obtener ejemplos de código completo:

> [!div class="nextstepaction"]
> [Con el módulo de servicios de Azure Maps](./how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Página de ejemplos de código](https://aka.ms/AzureMapsSamples)
