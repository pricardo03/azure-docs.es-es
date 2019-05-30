---
title: Presentación de información sobre una coordenada con Azure Maps | Microsoft Docs
description: Procedimiento para mostrar información sobre una dirección del mapa cuando un usuario selecciona una coordenada
author: jingjing-z
ms.author: jinzh
ms.date: 3/7/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 3b635ab96f1438377cb0d282c17304c2531185be
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357801"
---
# <a name="get-information-from-a-coordinate"></a>Obtención de información de una coordenada

En este artículo se muestra cómo realizar una búsqueda de dirección inversa que muestre la dirección de una ubicación emergente donde ha hecho clic.

Hay dos maneras de realizar una búsqueda de dirección inversa. Una es consultar la [Reverse Address Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) a través de un módulo de servicio. La otra manera es utilizar el [API capturar](https://fetch.spec.whatwg.org/) para realizar una solicitud a la [Reverse Address Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) para buscar una dirección. Ambas formas se describen a detalle a continuación.

## <a name="make-a-reverse-search-request-via-service-module"></a>Realización de una solicitud de búsqueda inversa a través del módulo de servicio

<iframe height='500' scrolling='no' title='Obtención de información de una coordenada (módulo de servicio)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>Get information from a coordinate (Service Module)</a> (Obtención de información de una coordenada) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa y establece el mecanismo de autenticación para usar la clave de suscripción. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea un `SubscriptionKeyCredentialPolicy` para autenticar las solicitudes HTTP a Azure Maps con la clave de suscripción. El `atlas.service.MapsURL.newPipeline()` toma el `SubscriptionKeyCredential` directiva y crea un [canalización](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.pipeline?view=azure-maps-typescript-latest) instancia. `searchURL` representa una dirección URL para las operaciones [Search](https://docs.microsoft.com/rest/api/maps/search) de Azure Maps.

El tercer bloque de código actualiza el estilo del cursor del mouse en un puntero y crea un [emergente](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open) objeto. Puede consultar [Adición de un elemento emergente en el mapa](./map-add-popup.md) para obtener instrucciones.

El cuarto bloque de código agrega un clic del mouse [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events). Cuando se desencadena, crea una consulta de búsqueda con las coordenadas del punto donde ha hecho clic. A continuación, usa el módulo de servicio [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/atlas.service.searchurl?view=azure-iot-typescript-latest#searchaddressreverse-aborter--geojson-position--searchaddressreverseoptions-) método para consultar el [obtener dirección de búsqueda inversa API](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) para la dirección de las coordenadas. Una colección de características de GeoJSON de la respuesta, a continuación, se extrae mediante el `geojson.getFeatures()` método.

El quinto bloque de código configura el contenido HTML del elemento emergente para mostrar la dirección de respuesta para la posición de la coordenada donde ha hecho clic.

El cambio de cursor, un objeto Popup y el evento de clic se crean en el [agente de escucha de eventos de carga](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) del mapa para garantizar que el mapa se carga totalmente antes de que se pueda recuperar la información de las coordenadas.

## <a name="make-a-reverse-search-request-via-fetch-api"></a>Realizar una solicitud de búsqueda inversa mediante API de captura

Haga clic en el mapa para realizar una solicitud de código geográfico inversa para esa ubicación utilizar fetch.

<iframe height='500' scrolling='no' title='Obtención de información de una coordenada' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>Obtención de información de una coordenada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa y establece el mecanismo de autenticación para usar la clave de suscripción. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código actualiza el estilo del cursor del mouse a un puntero y a un objeto [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open). Puede consultar [Adición de un elemento emergente en el mapa](./map-add-popup.md) para obtener instrucciones.

El tercer bloque de código agrega un agente de escucha de eventos para los clics del mouse. Tras un clic del mouse, utiliza el [API capturar](https://fetch.spec.whatwg.org/) para consultar el [Reverse Address Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) para la dirección de coordenadas donde ha hecho clic. Para una respuesta correcta, recopila la dirección de la ubicación donde hizo clic y define el contenido del elemento emergente y la posición mediante la función [setOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setoptions-popupoptions-) de la clase Popup.

El cambio de cursor, un objeto Popup y el evento de clic se crean en el [agente de escucha de eventos de carga](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) del mapa para garantizar que el mapa se carga totalmente antes de que se pueda recuperar la información de las coordenadas.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Consulte los siguientes artículos para obtener ejemplos de código completo:

> [!div class="nextstepaction"]
> [Presentación de indicaciones de ruta de A a B](./map-route.md)

> [!div class="nextstepaction"]
> [Visualización del tráfico](./map-show-traffic.md)
