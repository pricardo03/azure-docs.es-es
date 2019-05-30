---
title: Presentación de indicaciones con Azure Maps | Microsoft Docs
description: Procedimiento para mostrar indicaciones entre dos ubicaciones en un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 3/7/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: codepen
ms.openlocfilehash: 31403a11e8989cee6a1c166879a0b2f594892704
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357627"
---
# <a name="show-directions-from-a-to-b"></a>Presentación de indicaciones de ruta de A a B

Este artículo muestra cómo realizar una solicitud de ruta y mostrar la ruta en el mapa.

Hay dos formas de hacerlo. La primera es consultar la [Route API de Azure Maps](https://docs.microsoft.com/rest/api/maps/route/getroutedirections) a través de un módulo de servicio. La segunda manera es usar el [API capturar](https://fetch.spec.whatwg.org/) para realizar una solicitud de búsqueda a la [API de Azure Maps ruta](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). Ambos métodos se describen a continuación.

## <a name="query-the-route-via-service-module"></a>Consultas a la ruta a través del módulo de servicio

<iframe height='500' scrolling='no' title='Presentación de indicaciones de ruta de A a B en un mapa (módulo de servicio)' src='//codepen.io/azuremaps/embed/RBZbep/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/RBZbep/'>Presentación de indicaciones de ruta de A a B en un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa y establece el mecanismo de autenticación para usar la clave de suscripción. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea un `SubscriptionKeyCredentialPolicy` para autenticar las solicitudes HTTP a Azure Maps con la clave de suscripción. `atlas.service.MapsURL.newPipeline()` toma la directiva `SubscriptionKeyCredential` y crea una instancia de [canalización](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest). `routeURL` representa una dirección URL para las operaciones [Route](https://docs.microsoft.com/rest/api/maps/route) de Azure Maps.

El tercer bloque de código crea y agrega un [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) objeto a la asignación.

El cuarto bloque de código crea el inicio y finalización [puntos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) objetos y los agrega al objeto de origen de datos.

Una línea es un objeto [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) de LineString. Una clase [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) presenta objetos de línea encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como líneas en el mapa. El cuarto bloque de código crea una capa de línea y la agrega al mapa. Puede consultar las propiedades de una capa de líneas en [LinestringLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa. El quinto bloque de código se crea y agrega una capa de símbolos al mapa.

El sexto bloque de código consulta el servicio de enrutamiento de Azure Maps, que forma parte de la [módulo service](https://atlas.microsoft.com/sdk/javascript/mapcontrol/2/atlas-service.min.js). El [calculateRouteDirections](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.routeurl?view=azure-iot-typescript-latest#methods) método de la RouteURL se utiliza para obtener una ruta entre los puntos inicial y final. Una colección de características de GeoJSON de la respuesta, a continuación, se extrae mediante el `geojson.getFeatures()` método y se agrega al origen de datos. A continuación, se procesa la respuesta como una ruta en el mapa. Para obtener más información acerca de cómo agregar una línea al mapa, consulte [Adición de una línea al mapa](./map-add-shape.md#addALine).

El último bloque de código establece los límites del mapa mediante el mapa [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) propiedad.

La consulta de ruta, el origen de datos, las capas de símbolos y líneas, y los límites de cámara se crean y establecen en el [agente de escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) del mapa para asegurar que los resultados se muestran una vez que el mapa se carga completamente.

## <a name="query-the-route-via-fetch-api"></a>La ruta a través de API de captura de consulta

<iframe height='500' scrolling='no' title='Presentación de indicaciones de ruta de A a B en un mapa' src='//codepen.io/azuremaps/embed/zRyNmP/?height=469&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/zRyNmP/'>Presentación de indicaciones de ruta de A a B en un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa y establece el mecanismo de autenticación para usar la clave de suscripción. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea y agrega un objeto [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) al mapa.

El tercer bloque de código crea los puntos de inicio y de destino para la ruta y los agrega al origen de datos. Puede consultar [Adición de un anclaje en el mapa](map-add-pin.md) para obtener instrucciones acerca del uso de [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest).

Una clase [LineLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.linelayer?view=azure-iot-typescript-latest) presenta objetos de línea encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como líneas en el mapa. El cuarto bloque de código crea una capa de línea y la agrega al mapa. Puede consultar las propiedades de una capa de líneas en [LineLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.linelayeroptions?view=azure-iot-typescript-latest).

Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa. El quinto bloque de código se crea y agrega una capa de símbolos al mapa. Consulte las propiedades de una capa de símbolos en [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest).

El bloque de código siguiente crea los puntos `SouthWest` y `NorthEast` desde los puntos de inicio y de destino y establece los límites del mapa con la propiedad [setCamera](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#setcamera-cameraoptions---cameraboundsoptions---animationoptions-) del mapa.

El último bloque de código usa el [API capturar](https://fetch.spec.whatwg.org/) para realizar una solicitud de búsqueda a la [API de Azure Maps ruta](https://docs.microsoft.com/rest/api/maps/route/getroutedirections). A continuación, se analiza la respuesta. Si la respuesta fue correcta, la información de latitud y longitud se utiliza para crear una línea de una matriz mediante la conexión de esos puntos. Los datos de línea, a continuación, se agregan al origen de datos para representar la ruta en el mapa. Puede consultar [Adición de una línea en el mapa](./map-add-shape.md#addALine) para obtener instrucciones.

La consulta de ruta, el origen de datos, las capas de símbolos y líneas, y los límites de cámara se crean y establecen en el [agente de escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) del mapa para asegurar que los resultados se muestran una vez que el mapa se carga completamente.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Consulte los siguientes artículos para obtener ejemplos de código completo:

> [!div class="nextstepaction"]
> [Visualización del tráfico en el mapa](./map-show-traffic.md)

> [!div class="nextstepaction"]
> [Interacción con el mapa: eventos del mouse](./map-events.md)
