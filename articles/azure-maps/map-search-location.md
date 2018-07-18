---
title: Presentación de los resultados de la búsqueda con Azure Maps | Microsoft Docs
description: Procedimiento para realizar una solicitud de búsqueda con Azure Maps y, a continuación, mostrar los resultados en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b77737b16b23ed00c8f12f84e6a8558a665a7d15
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599888"
---
# <a name="show-search-results-on-the-map"></a>Presentación de los resultados de la búsqueda en el mapa

Este artículo muestra cómo realizar una solicitud de búsqueda y mostrar los resultados de la búsqueda en el mapa. 

## <a name="understand-the-code"></a>Comprensión del código

<iframe height='500' scrolling='no' title='Presentación de los resultados de la búsqueda en un mapa' src='//codepen.io/azuremaps/embed/KQbaeM/?height=519&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Presentación de los resultados de la búsqueda en un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea y agrega una capa de anclajes de búsqueda en el mapa. Puede consultar [Adición de un anclaje en el mapa](./map-add-pin.md) para obtener instrucciones.

El tercer bloque de código envía una solicitud [XMLHttpRequest](https://xhr.spec.whatwg.org/) a [Fuzzy Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy).

El último bloque de código analiza la respuesta de entrada. Para una respuesta correcta, recopila la información de latitud y longitud de cada ubicación devuelta. Agrega todos los puntos de ubicación al mapa como anclajes y ajusta los límites del mapa para representar todos los anclajes.


## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo: 

* [Fuzzy Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addPins](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addpins)
    
Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos: 
* [Obtención de información de una coordenada](./map-get-information-from-coordinate.md)
* [Presentación de indicaciones de ruta de A a B](./map-route.md)
