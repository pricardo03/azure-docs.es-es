---
title: Presentación de los resultados de la búsqueda con Azure Maps | Microsoft Docs
description: Procedimiento para realizar una solicitud de búsqueda con Azure Maps y luego mostrar los resultados en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 09/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 78ffa42bcf57b7163afc13b2550abdbae240ef00
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45729248"
---
# <a name="show-search-results-on-the-map"></a>Presentación de los resultados de la búsqueda en el mapa

En este artículo se describe cómo buscar ubicaciones de interés y mostrar los resultados de búsqueda en el mapa.

Hay dos maneras de buscar una ubicación de interés. Una consiste en usar un módulo de servicio para realizar una solicitud de búsqueda. La otra consiste en realizar una solicitud de búsqueda a través de [XMLHttpRequest](https://xhr.spec.whatwg.org/) a [Fuzzy Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy). Ambos métodos se describen a continuación.

## <a name="make-a-search-request-via-service-module"></a>Realización de una solicitud de búsqueda a través del módulo de servicio

<iframe height='500' scrolling='no' title='Presentación de los resultados de búsqueda en un mapa (módulo de servicio)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Show search results on a map (Service Module)</a> (Presentación de los resultados de búsqueda en un mapa [módulo de servicio]) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código construye un objeto de mapa e inicializa una instancia del servicio de cliente. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código usa la [Fuzzy Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) para buscar el punto de interés. Fuzzy Search API puede administrar cualquier combinación de entradas aproximadas. La respuesta del servicio de búsqueda aproximada se analiza en formato GeoJSON mediante el método [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest#geojsonsearchresponse). Los pines luego se agregan al mapa para mostrar los puntos de interés en este.

El último bloque de código ajusta los límites de cámara para el mapa mediante la propiedad [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) del mapa.

## <a name="make-a-search-request-via-xmlhttprequest"></a>Realización de una solicitud de búsqueda a través de XMLHttpRequest

<iframe height='500' scrolling='no' title='Presentación de los resultados de la búsqueda en un mapa' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Presentación de los resultados de la búsqueda en un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código agrega la capa de resultados de la búsqueda al mapa. La capa de los resultados de la búsqueda mostrará los resultados de búsqueda como pines en el mapa. Los pines se agregan mediante [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins).

El tercer bloque de código envía una solicitud [XMLHttpRequest](https://xhr.spec.whatwg.org/) a [Fuzzy Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) para buscar el punto de interés. Fuzzy Search API puede administrar cualquier combinación de entradas aproximadas.

El último bloque de código analiza la respuesta y ajusta los límites de cámara para el mapa mediante el método [setCameraBounds](https://docs.microsoft.com/javascript/api/azure-maps-control/models.cameraboundsoptions?view=azure-iot-typescript-latest) del mapa para representar los pines del resultado.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la **búsqueda aproximada**:

> [!div class="nextstepaction"]
> [Fuzzy Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy)

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Consulte los siguientes artículos para obtener ejemplos de código completo:

> [!div class="nextstepaction"]
> [Obtención de información de una coordenada](./map-get-information-from-coordinate.md)

> [!div class="nextstepaction"]
> [Presentación de indicaciones de ruta de A a B](./map-route.md)
