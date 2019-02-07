---
title: Presentación de los resultados de la búsqueda con Azure Maps | Microsoft Docs
description: Procedimiento para realizar una solicitud de búsqueda con Azure Maps y luego mostrar los resultados en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 11/15/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c68b4bdffde5f987fe07d50d76fa83e7bdfa5235
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/06/2019
ms.locfileid: "55755063"
---
# <a name="show-search-results-on-the-map"></a>Presentación de los resultados de la búsqueda en el mapa

En este artículo se describe cómo buscar ubicaciones de interés y mostrar los resultados de búsqueda en el mapa.

Hay dos maneras de buscar una ubicación de interés. Una consiste en usar un módulo de servicio para realizar una solicitud de búsqueda. La otra consiste en realizar una solicitud de búsqueda a través de [XMLHttpRequest](https://xhr.spec.whatwg.org/) a [Fuzzy Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy). Ambos métodos se describen a continuación.

## <a name="make-a-search-request-via-service-module"></a>Realización de una solicitud de búsqueda a través del módulo de servicio

<iframe height='500' scrolling='no' title='Presentación de los resultados de búsqueda en un mapa (módulo de servicio)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Show search results on a map (Service Module)</a> (Presentación de los resultados de búsqueda en un mapa [módulo de servicio]) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa e inicializa una instancia del servicio de cliente. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código usa el método [getSearchFuzzy](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchfuzzy-string--searchgetsearchfuzzyoptionalparams-) en el [módulo de servicio](https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=1). Permite realizar una búsqueda de texto de forma libre a través de la [Fuzzy Search rest API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) para buscar el punto de interés. Fuzzy Search API puede administrar cualquier combinación de entradas aproximadas. La respuesta del servicio de búsqueda aproximada se analiza en formato GeoJSON mediante el método [getGeoJsonSearchResponse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.geojson.geojsonsearchresponse?view=azure-iot-typescript-latest). 

El tercer bloque de código crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) y le agrega los resultados de la búsqueda. Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa.  A continuación, se crea una capa de símbolos y se le agrega el origen de datos y, después, esta se agrega al mapa.

El último bloque de código ajusta los límites de cámara para el mapa mediante la propiedad [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) del mapa.

La solicitud de búsqueda, el origen de datos, la capa de símbolos y los límites de cámara se crean y establecen en el [agente de escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) del mapa para asegurar que los resultados se muestran una vez que el mapa se carga completamente.


## <a name="make-a-search-request-via-xmlhttprequest"></a>Realización de una solicitud de búsqueda a través de XMLHttpRequest

<iframe height='500' scrolling='no' title='Presentación de los resultados de la búsqueda en un mapa' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Presentación de los resultados de la búsqueda en un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código envía una solicitud [XMLHttpRequest](https://xhr.spec.whatwg.org/) a [Fuzzy Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) para buscar el punto de interés. Fuzzy Search API puede administrar cualquier combinación de entradas aproximadas. 

El tercer bloque de código analiza la respuesta de búsqueda y almacena los resultados en una matriz para calcular los límites. A continuación, devuelve los resultados de búsqueda.

El cuarto bloque de código crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) y le agrega los resultados de la búsqueda. Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa. A continuación, se crea una capa de símbolos y se le agrega el origen de datos y, después, esta se agrega al mapa.

El último bloque de código crea un objeto [BoundingBox](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) con la matriz de resultados y, después, ajusta los límites de la cámara del mapa con el objeto [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) del mapa. A continuación, representar las chinchetas de resultados.

La solicitud de búsqueda, el origen de datos, la capa de símbolos y los límites de cámara se establecen en el [agente de escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) del mapa para asegurar que los resultados se muestran una vez que el mapa se carga completamente.

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
<!-- Comment added to suppress false positive warning -->
> [!div class="nextstepaction"]
> [Presentación de indicaciones de ruta de A a B](./map-route.md)
