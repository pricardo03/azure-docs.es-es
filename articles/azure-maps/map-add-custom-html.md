---
title: Adición de HTML personalizado en Azure Maps | Microsoft Docs
description: Procedimiento para agregar HTML personalizado a un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: e5cfbc7ddc10edf9b21afce73e3b7f8795fcdac9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46121964"
---
# <a name="add-custom-html-to-the-map"></a>Adición de HTML personalizado al mapa

Este artículo muestra cómo agregar código HTML personalizado, como un archivo de imagen, al mapa.

## <a name="understand-the-code"></a>Comprensión del código

<iframe height='466' scrolling='no' title='Adición de HTML personalizado a un mapa: png' src='//codepen.io/azuremaps/embed/MVoeVw/?height=466&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>Adición de HTML personalizado a un mapa: png</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

El segundo bloque de código crea un elemento HTML a partir de una imagen.

El último bloque de código usa la función [addHtml](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addhtml) de la clase Map para agregar la imagen en la posición especificada del mapa.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Para más ejemplos de código para agregar a los mapas, consulte los siguientes artículos:

> [!div class="nextstepaction"]
> [Presentación de los resultados de la búsqueda](./map-search-location.md)

> [!div class="nextstepaction"]
> [Obtención de información de una coordenada](./map-get-information-from-coordinate.md)