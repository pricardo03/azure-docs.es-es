---
title: Adición de una capa de burbuja a Azure Maps | Microsoft Docs
description: Cómo agregar una capa de burbuja al mapa de JavaScript
author: rbrundritt
ms.author: richbrun
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: f2c4c6b8655d5efb993a2dedf536000ac94328c2
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60769692"
---
# <a name="add-a-bubble-layer-to-a-map"></a>Adición de una capa de burbuja a un mapa

En este artículo se explica cómo puede representar datos de punto de un origen de datos como una capa de burbuja en un mapa. Las capas de burbuja representan puntos como círculos en el mapa con un radio de píxel fijo. 

> [!TIP]
> Las capas de burbuja de forma predeterminada representarán las coordenadas de todos los objetos geométricos en un origen de datos. Para limitar la capa de forma que solo represente las características de geometría de puntos, configure la propiedad `filter` de la capa en `['==', ['geometry-type'], 'Point']` o `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` si desea incluir también las características de MultiPoint.

## <a name="add-a-bubble-layer"></a>Adición de una capa de burbuja

<iframe height='500' scrolling='no' title='BubbleLayer DataSource' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer DataSource</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se define una matriz de objetos [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) y se agrega al objeto [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest).

Una [capa de burbujas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) representa los datos basados en puntos encapsulados en el [origen de datos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como círculos en el mapa. El último bloque de código crea una capa de burbujas y la agrega al mapa. Puede consultar las propiedades de una capa de burbujas en [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

La matriz de objetos Point, el origen de datos y la capa de burbuja se crean y agregan al mapa dentro de la función [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para garantizar que se muestre el círculo una vez que el mapa se cargue completamente.

## <a name="show-labels-with-a-bubble-layer"></a>Mostrar etiquetas con una capa de burbuja

<iframe height='500' scrolling='no' title='MultiLayer DataSource' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>MultiLayer DataSource</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El código anterior muestra cómo visualizar y etiquetar datos en el mapa. En el primer bloque de código anterior se construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código se crea un objeto [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest). A continuación, se crea un objeto de origen de datos mediante la clase [origen de datos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) y se agrega el punto al origen de datos.

Una [capa de burbujas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) representa los datos basados en puntos encapsulados en el [origen de datos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como círculos en el mapa. El tercer bloque de código crea una capa de burbujas y la agrega al mapa. Puede consultar las propiedades de una capa de burbujas en [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa. El último bloque de código crea y agrega una capa de símbolos al mapa que representa la etiqueta de texto de la burbuja. Consulte las propiedades de una capa de símbolos en [SymbolLayerOptions](/javascript/api/azure-maps-control/atlas.symbollayeroptions).

El origen de datos y las capas se crean y agregan al mapa dentro de la función [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para garantizar que se muestren los datos una vez que el mapa se cargue completamente.

## <a name="customize-a-bubble-layer"></a>Personalización de una capa de burbuja

La capa de burbuja solo tiene algunas opciones de estilo. Esta es una herramienta para probarlas.

<br/>

<iframe height='700' scrolling='no' title='Opciones de capa de burbuja' src='//codepen.io/azuremaps/embed/eQxbGm/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/eQxbGm/'>opciones de capa de burbuja</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [BubbleLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [BubbleLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.bubblelayeroptions?view=azure-iot-typescript-latest)

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Adición de una capa de símbolo](map-add-pin.md)

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)