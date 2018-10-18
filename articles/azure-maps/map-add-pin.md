---
title: Adición de un anclaje con Azure Maps | Microsoft Docs
description: Procedimiento para agregar un anclaje a un mapa de Javascript
author: jingjing-z
ms.author: jinzh
ms.date: 05/07/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 35b22e28a6a339af6f6f6e8db0655f2ae6de0118
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "46122185"
---
# <a name="add-pins-to-the-map"></a>Adición de anclajes al mapa

Este artículo muestra cómo agregar un anclaje a un mapa.  

## <a name="understand-the-code"></a>Comprensión del código

<iframe height='500' scrolling='no' title='Adición de un anclaje a un mapa' src='//codepen.io/azuremaps/embed/ZrVpEa/?height=504&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/ZrVpEa/'>Adición de un anclaje a un mapa</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un anclaje y se agrega al mapa. Un anclaje es una [Feature (característica)](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) de [Point (punto)](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) con [PinProperties](https://docs.microsoft.com/javascript/api/azure-maps-control/models.pinproperties?view=azure-iot-typescript-latest) como su propiedad Feature. Use `new atlas.data.Feature(new atlas.data.Point())` para crear un anclaje y definir sus propiedades. Una capa de anclajes es una matriz de anclajes. Use la función [addPins](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#addpins) de la clase map para agregar una capa de anclajes al mapa y definir las propiedades de la capa de anclajes. Consulte las propiedades de una capa de anclajes en [PinLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/models.pinlayeroptions?view=azure-iot-typescript-latest).

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Adición de un elemento emergente](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Adición de una forma](./map-add-shape.md)