---
title: Crear una aplicación accesible con Azure Maps| Microsoft Docs
description: Cómo crear una aplicación accesible mediante Azure Maps
services: azure-maps
keywords: ''
author: chgennar
ms.author: chgennar
ms.date: 09/17/2018
ms.topic: article
ms.service: azure-maps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.openlocfilehash: ef948b4dca3d3800a81ac52f3a73beee2558d21c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60769709"
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
