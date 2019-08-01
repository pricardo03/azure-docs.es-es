---
title: Crear una aplicación accesible con Azure Maps| Microsoft Docs
description: Cómo crear una aplicación accesible mediante Azure Maps
services: azure-maps
author: chgennar
ms.author: chgennar
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
manager: timlt
ms.openlocfilehash: 8865027c895be09150797608e43184f1fdefb267
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68638791"
---
# <a name="building-an-accessible-application"></a>Creación de una aplicación accesible

Este artículo muestra cómo crear una aplicación de mapas que se puede usar con un lector de pantalla.

## <a name="understand-the-code"></a>Comprendiendo el código

<iframe height='500' scrolling='no' title='Crear una aplicación accesible' src='//codepen.io/azuremaps/embed/ZoVyZQ/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Vea el lápiz <a href='https://codepen.io/azuremaps/pen/ZoVyZQ/'>Crear una aplicación accesible</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El mapa está creado previamente con algunas características de accesibilidad. Los usuarios pueden navegar por el mapa con el teclado. Si se está ejecutando un lector de pantalla, el mapa notificará al usuario los cambios en su estado.
Por ejemplo, los usuarios reciben notificaciones de cambios en el mapa cuando este se amplía o desplaza lateralmente. Cualquier información adicional que se coloque en el mapa base debería tener la información textual correspondiente para los usuarios de lectores de pantalla.

El uso de la clase [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) es una manera de lograrlo. En el ejemplo de búsqueda anterior, se agrega un Popup con información textual al mapa para cada uno de los marcadores colocados en el mapa. El método [Attach](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest#attach) de [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest) permite que un lector de pantalla vea el elemento emergente sin mostrarlo visualmente en el mapa.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre la clase Popup y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Popup](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.popup?view=azure-iot-typescript-latest)

Consulte más ejemplos de código:

> [!div class="nextstepaction"]
> [Página de ejemplos de código](https://aka.ms/AzureMapsSamples)
