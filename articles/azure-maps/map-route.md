---
title: Presentación de indicaciones con Azure Maps | Microsoft Docs
description: Procedimiento para mostrar indicaciones entre dos ubicaciones en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: ed522779f5a86e38ee12a246cea9ac85d0379f9e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729070"
---
# <a name="show-directions-from-a-to-b"></a>Presentación de indicaciones de ruta de A a B

Este artículo muestra cómo realizar una solicitud de ruta y mostrar la ruta en el mapa.

Hay dos formas de hacerlo. La primera es consultar la [Route API de Azure Maps](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) a través de un módulo de servicio. La segunda es hacer realizar una solicitud [XMLHttpRequest](https://xhr.spec.whatwg.org/) a la API. Ambos métodos se describen a continuación.

## <a name="query-the-route-via-service-module"></a>Consultas a la ruta a través del módulo de servicio

<iframe height='500' scrolling='no' title='Presentación de indicaciones de ruta de A a B en un mapa (módulo de servicio)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/RBZbep/'>Presentación de indicaciones de ruta de A a B en un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

La línea en el segundo bloque de código crea una instancia de un cliente de servicio.

El tercer bloque de código inicializa la [capa Cadena de línea](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings) en el mapa.

El cuarto bloque de código crea y agrega anclajes en el mapa para representar los puntos inicial y final de la ruta. Puede consultar [Adición de un anclaje en el mapa](map-add-pin.md) para obtener instrucciones acerca del uso de [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

El siguiente bloque de código usa la función [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) de la clase Map para establecer el cuadro de límites del mapa en función de los puntos inicial y final de la ruta.

El sexto bloque de código crea una consulta de ruta.

El último bloque de código envía una consulta al servicio de enrutamiento de Azure Maps mediante el método [getRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.route?view=azure-iot-typescript-latest#getroutedirections) para obtener una ruta entre los puntos inicial y final. A continuación, se analiza la respuesta en formato GeoJSON con el método [getGeoJsonRoutes](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonroutedirectionsresponse?view=azure-iot-typescript-latest#getgeojsonroutes). Y agrega todas esas líneas en el mapa para representar la ruta. Para más información, consulte [Adición de una línea al mapa](./map-add-shape.md#addALine).

## <a name="query-the-route-via-xmlhttprequest"></a>Consultas a la ruta mediante XMLHttpRequest

<iframe height='500' scrolling='no' title='Presentación de indicaciones de ruta de A a B en un mapa' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Presentación de indicaciones de ruta de A a B en un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea y agrega anclajes en el mapa para representar los puntos inicial y final de la ruta. Puede consultar [Adición de un anclaje en el mapa](map-add-pin.md) para obtener instrucciones acerca del uso de [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

El tercer bloque de código usa la función [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) de la clase mapa para establecer el cuadro de límites del mapa en función de los puntos inicial y final de la ruta.

El cuarto bloque de código envía una solicitud [XMLHttpRequest](https://xhr.spec.whatwg.org/) a [Route API de Azure Maps](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

El último bloque de código analiza la respuesta de entrada. Para una respuesta correcta, recopila la información de latitud y longitud de cada punto de referencia. Crea una matriz de líneas mediante la conexión de cada punto de referencia a su punto de referencia posterior. Y agrega todas esas líneas en el mapa para representar la ruta. Puede consultar [Adición de una línea en el mapa](./map-add-shape.md#addALine) para obtener instrucciones.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Consulte los siguientes artículos para obtener ejemplos de código completo:

> [!div class="nextstepaction"]
> [Visualización del tráfico en el mapa](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Interacción con el mapa: eventos del mouse](./map-events.md)
