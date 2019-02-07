---
title: Visualización del tráfico con Azure Maps | Microsoft Docs
description: Cómo mostrar los datos de tráfico en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 11/10/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 7cd7c0dbb375dad78927183dbaffe574a0dc10c2
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55753546"
---
# <a name="show-traffic-on-the-map"></a>Visualización del tráfico en el mapa

En este artículo se muestra cómo mostrar información de tráfico e incidentes en el mapa.

## <a name="understand-the-code"></a>Comprendiendo el código

<iframe height='456' scrolling='no' title='Visualización del tráfico en un mapa' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el fragmento de código (pen) <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>Mostrar tráfico en un mapa</a> por Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](map-create.md) para obtener instrucciones.

El segundo bloque de código utiliza la función [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest) de la función [event listener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) del mapa para mostrar los flujos de tráfico y las incidencias en el mapa.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Consulte los siguientes artículos para obtener ejemplos de código completos:

> [!div class="nextstepaction"]
> [Página de ejemplos de código](https://aka.ms/AzureMapsSamples)

Mejore sus experiencias de usuario:

> [!div class="nextstepaction"]
> [Interacción del mapa con eventos del mouse](./map-events.md)

> [!div class="nextstepaction"]
> [Building an accessible map](./map-accessibility.md) (Creación de un mapa accesible)
