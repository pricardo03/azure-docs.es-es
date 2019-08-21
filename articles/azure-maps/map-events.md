---
title: Control de los eventos del mouse con Azure Maps | Microsoft Docs
description: Procedimiento para crear un mapa interactivo de Web SDK con eventos de mapa
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 37a3fc3178fe5caeacedfd355a6065ee189a5890
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976541"
---
# <a name="interact-with-the-map---mouse-events"></a>Interacción con el mapa: eventos del mouse

En este artículo se muestra cómo usar la propiedad de [map class events](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?#events) para resaltar eventos en el mapa y en distintas capas de este. También se muestra cómo usar la propiedad de eventos clase Map para resaltar los eventos cuando se interactúa con un marcador HTML.

## <a name="interact-with-the-map"></a>Interacción con el mapa

Practique con el mapa siguiente y observe los eventos del mouse correspondientes resaltados en la parte derecha. Puede hacer clic en la **pestaña JS** para ver y editar el código JavaScript. También puede hacer clic en el botón **Editar en CodePen** y modificar el código en CodePen.

<br/>

<iframe height='600' scrolling='no' title='Interacción con el mapa: eventos del mouse' src='//codepen.io/azuremaps/embed/bLZEWd/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/bLZEWd/'>Interacción con el mapa: eventos del mouse</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="interact-with-map-layers"></a>Interacción con las capas de mapa

En el código siguiente se resalta el nombre de los eventos que se activan al interactuar con la capa de símbolos. Las capas de símbolo, burbuja, línea y polígono admiten el mismo conjunto de eventos. Las capas de mapa térmico y de mosaico no admiten ninguno de estos eventos.

<br/>

<iframe height='600' scrolling='no' title='Interacción con el mapa: eventos de las capas' src='//codepen.io/azuremaps/embed/bQRRPE/?height=600&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/bQRRPE/'>Interacción con el mapa: eventos de las capas</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="interact-with-html-marker"></a>Interacción con el marcador HTML

El código siguiente agrega eventos de mapa JavaScript a un marcador HTML. También resalta el nombre de los eventos que se activan al interactuar con el marcador HTML.

<br/>

<iframe height='500' scrolling='no' title='Interacción con el mapa: eventos del marcador HTML' src='//codepen.io/azuremaps/embed/VVzKJY/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/VVzKJY/'>Interacción con el mapa: eventos del marcador HTML</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Consulte los siguientes artículos para obtener ejemplos de código completo:

> [!div class="nextstepaction"]
> [Usar el módulo de servicios de Azure Maps](./how-to-use-services-module.md)

> [!div class="nextstepaction"]
> [Ejemplos de código](https://docs.microsoft.com/samples/browse/?products=azure-maps)
