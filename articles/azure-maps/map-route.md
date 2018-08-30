---
title: Presentación de indicaciones con Azure Maps | Microsoft Docs
description: Procedimiento para mostrar indicaciones entre dos ubicaciones en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 52462c1c5a2a1a9698a2b51708e63b1bb1664f6e
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2018
ms.locfileid: "42745543"
---
# <a name="show-directions-from-a-to-b"></a>Presentación de indicaciones de ruta de A a B 

Este artículo muestra cómo realizar una solicitud de ruta y mostrar la ruta en el mapa. 

## <a name="understand-the-code"></a>Comprensión del código

<iframe height='500' scrolling='no' title='Presentación de indicaciones de ruta de A a B en un mapa' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Presentación de indicaciones de ruta de A a B en un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea y agrega anclajes en el mapa para representar los puntos inicial y final de la ruta. Puede consultar [Adición de un anclaje en el mapa](map-add-pin.md) para obtener instrucciones.

El tercer bloque de código usa la función [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds) de la clase Map para establecer el cuadro de límites del mapa en función de los puntos inicial y final de la ruta.

El cuarto bloque de código envía una solicitud [XMLHttpRequest](https://xhr.spec.whatwg.org/) a [Route API de Azure Maps](https://docs.microsoft.com/rest/api/maps/route/getroutedirections).

El último bloque de código analiza la respuesta de entrada. Para una respuesta correcta, recopila la información de latitud y longitud de cada punto de referencia. Crea una matriz de líneas mediante la conexión de cada punto de referencia a su punto de referencia posterior. Y agrega todas esas líneas en el mapa para representar la ruta. Puede consultar [Adición de una línea en el mapa](./map-add-shape.md#addALine) para obtener instrucciones.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo: 

* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamerabounds)
    * [addLinestrings](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addlinestrings)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins)

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos: 
* [Visualización del tráfico en el mapa](./map-show-traffic.md)
* [Interacción con el mapa: eventos del mouse](./map-events.md)
