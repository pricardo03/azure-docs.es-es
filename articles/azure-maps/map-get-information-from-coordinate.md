---
title: Presentación de información sobre una coordenada con Azure Maps | Microsoft Docs
description: Procedimiento para mostrar información sobre una dirección del mapa cuando un usuario selecciona una coordenada
author: jingjing-z
ms.author: jinzh
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 331e687c40f21b0bf6074239969848c632682773
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/04/2018
ms.locfileid: "43667488"
---
# <a name="get-information-from-a-coordinate"></a>Obtención de información de una coordenada

Este artículo muestra cómo realizar una búsqueda inversa de dirección y, tras un clic del mouse, mostrar la dirección de la ubicación donde ha hecho clic en un cuadro emergente.

## <a name="understand-the-code"></a>Comprensión del código

<iframe height='500' scrolling='no' title='Obtención de información de una coordenada (módulo de servicio)' src='//codepen.io/azuremaps/embed/ejEYMZ/?height=265&theme-id=0&default-tab=js,result&embed-version=2' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ejEYMZ/'>Get information from a coordinate (Service Module)</a> (Obtención de información de una coordenada) de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

La línea en el segundo bloque de código crea una instancia de un cliente de servicio.

El tercer bloque de código actualiza el estilo del cursor del mouse a un puntero.

El cuarto bloque de código crea un elemento emergente. Puede consultar [Adición de un elemento emergente en el mapa](./map-add-popup.md) para obtener instrucciones.

El último bloque de código agrega un agente de escucha de eventos para los clics del mouse. Tras un clic del mouse, crea una consulta de búsqueda con las coordenadas del punto donde hizo clic. A continuación, usa el punto de conexión [getSearchAddressReverse](https://docs.microsoft.com/javascript/api/azure-maps-rest/services.search?view=azure-iot-typescript-latest#getsearchaddressreverse) del mapa para consultar la dirección de las coordenadas.

Para una respuesta correcta, recopila la dirección de la ubicación donde hizo clic y define el contenido del elemento emergente y la posición mediante la función [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions) de la clase Popup.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo: 
* [Búsqueda de direcciones inversa](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addeventlistener)
* [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [open](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#open)
    * [close](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#close)

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos:
* [Presentación de indicaciones de ruta de A a B](./map-route.md)
* [Visualización del tráfico](./map-show-traffic.md)
