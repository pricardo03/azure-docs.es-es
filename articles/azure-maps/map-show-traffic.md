---
title: Visualización del tráfico con Azure Maps | Microsoft Docs
description: Cómo mostrar los datos de tráfico en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 6ff7a0270509c244fc97bd04d8ba648fd262dc58
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600126"
---
# <a name="show-traffic-on-the-map"></a>Visualización del tráfico en el mapa

En este artículo se muestra cómo mostrar información de tráfico y de los incidentes en el mapa. 

## <a name="understand-the-code"></a>Comprensión del código

<iframe height='456' scrolling='no' title='Visualización del tráfico en un mapa' src='//codepen.io/azuremaps/embed/WMLRPw/?height=456&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el lápiz <a href='https://codepen.io/azuremaps/pen/WMLRPw/'>Mostrar tráfico en un mapa</a> por Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](map-create.md) para obtener instrucciones.

El segundo bloque de código utiliza la función [setTraffic ](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic) de la clase map para mostrar los flujos de tráfico y las incidencias en el mapa.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo: 
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [setTraffic](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#settraffic)

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos: 
* [Interacción con el mapa: eventos del mouse](./map-events.md)
* [Building an accessible map](./map-accessibility.md) (Creación de un mapa accesible)

Visite nuestra [página de ejemplos de código](http://aka.ms/AzureMapsSamples) para ver más escenarios de asignación.
