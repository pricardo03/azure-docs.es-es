---
title: Crear una aplicación accesible con Azure Maps| Microsoft Docs
description: Cómo crear una aplicación accesible mediante Azure Maps
services: azure-maps
keywords: ''
author: chgennar
ms.author: chgennar
ms.date: 05/18/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.openlocfilehash: 2523287669628b8c58028cae9887743c20b6bc5e
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/01/2018
ms.locfileid: "34659839"
---
# <a name="building-an-accessible-application"></a>Creación de una aplicación accesible

Este artículo muestra cómo crear una aplicación de mapas que se puede usar con un lector de pantalla.

## <a name="understand-the-code"></a>Comprensión del código

<iframe height='500' scrolling='no' title='Crear una aplicación accesible' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Vea el lápiz <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>Crear una aplicación accesible</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El mapa está creado previamente con algunas características de accesibilidad. Un usuario puede desplazarse por el mapa mediante el teclado y, si se está ejecutando un lector de pantalla, el mapa notificará al usuario los cambios en su estado. Por ejemplo, se notificará al usuario la nueva latitud, longitud, zoom y localidad del mapa cuando se desplace lateralmente o amplíe el mapa. Cualquier información adicional que se coloque en el mapa base debería tener la información textual correspondiente para los usuarios de lectores de pantalla. El uso de la clase [Popup](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) es una manera de lograrlo. En el ejemplo de búsqueda anterior, se agrega un Popup con información textual al mapa para cada uno de los marcadores colocados en el mapa. El método Attach de [Popup](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest) permite que un lector de pantalla vea el elemento emergente sin mostrarlo visualmente en el mapa.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la clase Popup y los métodos utilizados en este artículo:

* [Popup](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest)
    * [attach](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#attach)
    * [remove](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#remove)
    * [open](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#open)
    * [close](https://docs.microsoft.com/javascript/api/azure-maps-javascript/popup?view=azure-iot-typescript-latest#close)

Visite nuestra [página de ejemplos de código](http://aka.ms/AzureMapsSamples) para ver más escenarios de asignación.
