---
title: Visualización del tráfico con Azure Maps | Microsoft Docs
description: Cómo mostrar los datos de tráfico en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 09/14/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6d5c721ab84c28bae9415dceeaa09fd12cc05824
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45733032"
---
# <a name="show-traffic-on-the-map"></a>Visualización del tráfico en el mapa

En este artículo se muestra cómo mostrar información de tráfico e incidentes en el mapa.

## <a name="understand-the-code"></a>Comprendiendo el código

<iframe height='456' scrolling='no' title='Visualización del tráfico en un mapa' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el fragmento de código (pen) <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>Mostrar tráfico en un mapa</a> por Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](map-create.md) para obtener instrucciones.

El segundo bloque de código utiliza la función [setTraffic ](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#settraffic) de la clase map para mostrar los flujos de tráfico y las incidencias en el mapa.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Consulte los siguientes artículos para obtener ejemplos de código completos:

> [!div class="nextstepaction"]
> [Página de ejemplos de código](http://aka.ms/AzureMapsSamples)

Mejore sus experiencias de usuario:

> [!div class="nextstepaction"]
> [Interacción del mapa con eventos del mouse](./map-events.md)

> [!div class="nextstepaction"]
> [Building an accessible map](./map-accessibility.md) (Creación de un mapa accesible)
