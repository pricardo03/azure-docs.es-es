---
title: Presentación de información sobre una coordenada con Azure Maps | Microsoft Docs
description: Procedimiento para mostrar información sobre una dirección del mapa cuando un usuario selecciona una coordenada
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: b640346b0d6f490457e1e82a65c0d3f373d658d3
ms.sourcegitcommit: 17fe5fe119bdd82e011f8235283e599931fa671a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2018
ms.locfileid: "42143844"
---
# <a name="get-information-from-a-coordinate"></a>Obtención de información de una coordenada

Este artículo muestra cómo realizar una búsqueda inversa de dirección y, tras un clic del mouse, mostrar la dirección de la ubicación donde ha hecho clic en un cuadro emergente. 

## <a name="understand-the-code"></a>Comprensión del código

<iframe height='500' scrolling='no' title='Obtención de información de una coordenada' src='//codepen.io/azuremaps/embed/ddXzoB/?height=516&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ddXzoB/'>Obtención de información de una coordenada</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código actualiza el estilo del cursor del mouse a un puntero.

El tercer bloque de código crea un elemento emergente. Puede consultar [Adición de un elemento emergente en el mapa](./map-add-popup.md) para obtener instrucciones.

El último bloque de código agrega un agente de escucha de eventos para los clics del mouse. Tras un clic del mouse, envía una solicitud [XMLHttpRequest](https://xhr.spec.whatwg.org/) a [Reverse Address Search API de Azure Maps](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse). Para una respuesta correcta, recopila la dirección de la ubicación donde ha hecho clic y define el contenido del elemento emergente y la posición mediante la función [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions) de la clase Popup

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo: 
* [Búsqueda de direcciones inversa](https://docs.microsoft.com/rest/api/maps/search/getsearchaddressreverse)
* [Map](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest)
    * [addEventListener](https://docs.microsoft.com/javascript/api/azure-maps-javascript/map?view=azure-iot-typescript-latest#addeventlistener)
* [Popup](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [setPopupOptions](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#setpopupoptions)
    * [open](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [close](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos: 
* [Presentación de indicaciones de ruta de A a B](./map-route.md)
* [Visualización del tráfico](./map-show-traffic.md)
