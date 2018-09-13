---
title: Presentación de indicaciones con Azure Maps | Microsoft Docs
description: Procedimiento para mostrar indicaciones entre dos ubicaciones en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 80abd6db9888524aa6a66d9861d8dc2d05188e19
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2018
ms.locfileid: "43781503"
---
# <a name="show-directions-from-a-to-b"></a>Presentación de indicaciones de ruta de A a B 

Este artículo muestra cómo realizar una solicitud de ruta y mostrar la ruta en el mapa. 

## <a name="understand-the-code"></a>Comprensión del código

<iframe height='500' scrolling='no' title='Presentación de indicaciones de ruta de A a B en un mapa (módulo de servicio)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/RBZbep/'>Presentación de indicaciones de ruta de A a B en un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

La línea en el segundo bloque de código crea una instancia de un cliente de servicio.

El tercer bloque de código inicializa la [capa Cadena de línea](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) en el mapa.

El cuarto bloque de código crea y agrega anclajes en el mapa para representar los puntos inicial y final de la ruta. Puede consultar [Adición de un anclaje en el mapa](map-add-pin.md) para obtener instrucciones.

El siguiente bloque de código usa la función [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) de la clase Map para establecer el cuadro de límites del mapa en función de los puntos inicial y final de la ruta.

El sexto bloque de código crea una consulta de ruta.

El último bloque de código envía una consulta al servicio de enrutamiento de Azure Maps mediante el método [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) para obtener una ruta entre los puntos inicial y final. A continuación, se analiza la respuesta en formato GeoJSON con el método [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes). Y agrega todas esas líneas en el mapa para representar la ruta. Para más información, consulte [Adición de una línea al mapa](./map-add-shape.md#addALine).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo: 

* [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections)
* [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes)
* [Capa Línea de cadena](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos: 
* [Visualización del tráfico en el mapa](./map-show-traffic.md)
* [Interacción con el mapa: eventos del mouse](./map-events.md)
