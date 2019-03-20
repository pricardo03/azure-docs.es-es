---
title: Presentación de los resultados de la búsqueda con Azure Maps | Microsoft Docs
description: Procedimiento para realizar una solicitud de búsqueda con Azure Maps y luego mostrar los resultados en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 3/7/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 8ae6c8a20a05df723d3f6b394e0639f218896a85
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57845144"
---
# <a name="show-search-results-on-the-map"></a>Presentación de los resultados de la búsqueda en el mapa

En este artículo se describe cómo buscar ubicaciones de interés y mostrar los resultados de búsqueda en el mapa.

Hay dos maneras de buscar una ubicación de interés. Una consiste en usar un módulo de servicio para realizar una solicitud de búsqueda. La otra consiste en realizar una solicitud de búsqueda a [Azure Maps Fuzzy search API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) a través de la [API capturar](https://fetch.spec.whatwg.org/). Ambos métodos se describen a continuación.

## <a name="make-a-search-request-via-service-module"></a>Realización de una solicitud de búsqueda a través del módulo de servicio

<iframe height='500' scrolling='no' title='Presentación de los resultados de búsqueda en un mapa (módulo de servicio)' src='//codepen.io/azuremaps/embed/zLdYEB/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/zLdYEB/'>Show search results on a map (Service Module)</a> (Presentación de los resultados de búsqueda en un mapa [módulo de servicio]) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa y establece el mecanismo de autenticación para usar la clave de suscripción. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea un **SubscriptionKeyCredentialPolicy** para autenticar las solicitudes HTTP a Azure Maps con la clave de suscripción. El **atlas.service.MapsURL.newPipeline()** toma el **SubscriptionKeyCredential** directiva y crea un [canalización](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-iot-typescript-latest) instancia. El **searchURL** representa una dirección URL de Azure Maps [búsqueda](https://docs.microsoft.com/rest/api/maps/search) operaciones.

El tercer bloque de código crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) y le agrega los resultados de la búsqueda. Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa.  A continuación, se crea una capa de símbolos y se le agrega el origen de datos y, después, esta se agrega al mapa.

El cuarto bloque de código usa el [SearchFuzzy](/javascript/api/azure-maps-rest/atlas.service.models.searchgetsearchfuzzyoptionalparams) método en el [módulo service](https://atlas.microsoft.com/sdk/js/atlas-service.js?api-version=2). Permite realizar una búsqueda de texto de forma libre a través de la [API de rest de obtención de búsqueda aproximada](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) para buscar el punto de interés. API de Get Search Fuzzy puede controlar cualquier combinación de entradas aproximadas. Una colección de características de GeoJSON de la respuesta, a continuación, se extrae mediante el **geojson.getFeatures()** método y se agrega al origen de datos, lo que genera automáticamente en los datos que se representa en el mapa a través de la capa de símbolos.

El último bloque de código ajusta los límites de cámara para el mapa mediante la propiedad [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) del mapa.

Solicitud de la búsqueda, se crean el origen de datos y capa de símbolos y los límites de cámara y conjunto dentro de la asignación de la lista [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para asegurarse de que se muestran los resultados después de que el mapa se cargue completamente.


## <a name="make-a-search-request-via-fetch-api"></a>Realizar una solicitud de búsqueda a través de API de captura

<iframe height='500' scrolling='no' title='Presentación de los resultados de la búsqueda en un mapa' src='//codepen.io/azuremaps/embed/KQbaeM/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/KQbaeM/'>Presentación de los resultados de la búsqueda en un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa y establece el mecanismo de autenticación para usar la clave de suscripción. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea una dirección URL para realizar una solicitud de búsqueda a. También crea dos matrices para almacenar límites y PIN de los resultados de búsqueda.

El tercer bloque de código usa el [API capturar](https://fetch.spec.whatwg.org/) para realizar una solicitud a [Azure Maps Fuzzy search API](https://docs.microsoft.com/rest/api/maps/search/getsearchfuzzy) para buscar los puntos de interés. Fuzzy Search API puede administrar cualquier combinación de entradas aproximadas. A continuación, controla y analiza la respuesta de búsqueda y agrega las clavijas del resultado a la matriz searchPins.

El cuarto bloque de código crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) y le agrega los resultados de la búsqueda. Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa. A continuación, se crea una capa de símbolos y se le agrega el origen de datos y, después, esta se agrega al mapa.

El último bloque de código crea un objeto [BoundingBox](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.boundingbox?view=azure-iot-typescript-latest) con la matriz de resultados y, después, ajusta los límites de la cámara del mapa con el objeto [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) del mapa. A continuación, representa las clavijas del resultado.

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
