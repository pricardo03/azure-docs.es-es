---
title: Presentación de información sobre una coordenada con Azure Maps | Microsoft Docs
description: Procedimiento para mostrar información sobre una dirección del mapa cuando un usuario selecciona una coordenada
author: jingjing-z
ms.author: jinzh
ms.date: 09/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: bf44437f4d0b60a5d56c2be29418b7132346da2e
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2018
ms.locfileid: "45732301"
---
# <a name="get-information-from-a-coordinate"></a>Obtención de información de una coordenada

En este artículo se muestra cómo realizar una búsqueda de dirección inversa que muestre la dirección de una ubicación emergente donde ha hecho clic.

Hay dos maneras de realizar una búsqueda de dirección inversa. Una es consultar la [Reverse Address Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse) a través de un módulo de servicio. La otra consiste en realizar una solicitud [XMLHttpRequest](https://xhr.spec.whatwg.org/) a la API para buscar una dirección. Ambas formas se describen a detalle a continuación.

## <a name="make-a-reverse-search-request-via-service-module"></a>Realización de una solicitud de búsqueda inversa a través del módulo de servicio

<iframe height='500' scrolling='no' title='Obtención de información de una coordenada (módulo de servicio)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>Get information from a coordinate (Service Module)</a> (Obtención de información de una coordenada) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

La línea en el segundo bloque de código crea una instancia de un cliente de servicio.

El tercer bloque de código actualiza el estilo del cursor del mouse a un puntero.

El cuarto bloque de código crea un elemento emergente mediante la propiedad [open](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open). Puede consultar [Adición de un elemento emergente en el mapa](./map-add-popup.md) para obtener instrucciones.

El último bloque de código [agrega un agente de escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener) para los clics del mouse. Tras un clic del mouse, crea una consulta de búsqueda con las coordenadas del punto donde hizo clic. A continuación, usa el punto de conexión [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) del mapa para consultar la dirección de las coordenadas.

Para una respuesta correcta, recopila la dirección de la ubicación donde hizo clic y define el contenido del elemento emergente y la posición mediante la función [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) de la clase Popup.

## <a name="make-a-reverse-search-request-via-xmlhttprequest"></a>Realización de una solicitud de búsqueda inversa a través de XMLHttpRequest

<iframe height='500' scrolling='no' title='Obtención de información de una coordenada' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>Obtención de información de una coordenada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código actualiza el estilo del cursor del mouse a un puntero.

El tercer bloque de código crea un elemento emergente mediante la propiedad [open](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open). Puede consultar [Adición de un elemento emergente en el mapa](./map-add-popup.md) para obtener instrucciones.

El último bloque de código agrega un agente de escucha de eventos para los clics del mouse. Tras un clic del mouse, envía una solicitud [XMLHttpRequest](https://xhr.spec.whatwg.org/) a [Reverse Address Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Para una respuesta correcta, recopila la dirección de la ubicación donde ha hecho clic y define el contenido del elemento emergente y la posición mediante la función [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) de la clase Popup

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
