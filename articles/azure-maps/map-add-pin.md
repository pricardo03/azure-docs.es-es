---
title: Incorporación de símbolos y marcadores con Azure Maps | Microsoft Docs
description: Cómo agregar símbolos y marcadores a un mapa de Javascript
author: walsehgal
ms.author: v-musehg
ms.date: 10/30/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: c56ac35f49c364b7b0f2ad26b82b178411419414
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "52282692"
---
# <a name="add-symbols-and-markers-to-a-map"></a>Incorporación de símbolos y marcadores a un mapa

En este artículo se muestra cómo agregar símbolos y marcadores a un mapa con un origen de datos.

## <a name="add-a-symbol-marker"></a>Adición de marcador de símbolos

<iframe height='500' scrolling='no' title='Cambio del anclaje de ubicación' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen para <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>cambiar el anclaje de ubicación</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el primer bloque de código anterior se construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Luego se crea un punto y se agrega al origen de datos. Un punto es una clase [Feature](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.feature?view=azure-iot-typescript-latest) de una clase [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest).

En el tercer bloque de código se crea una [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) y se actualizan las coordenadas con el clic del mouse, utilizando la clase shape del método [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest#setcoordinates).

Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa.  El origen de datos,la escucha de eventos de clic y la capa de símbolos se crean y agregan al mapa dentro de la función [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para garantizar que se muestre el punto una vez que el mapa se cargue completamente.

## <a name="add-a-custom-symbol"></a>Incorporación de un símbolo personalizado

<iframe height='500' scrolling='no' title='HTML DataSource' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>HTML DataSource</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código se agrega [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) al mapa mediante la propiedad [markers](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#markers) de la clase [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest). HtmlMarker se agrega al mapa dentro de la función [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para garantizar que se muestre una vez que el mapa se cargue completamente.

## <a name="add-bubble-markers"></a>Adición de marcadores de burbuja

<iframe height='500' scrolling='no' title='BubbleLayer DataSource' src='//codepen.io/azuremaps/embed/mzqaKB/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/mzqaKB/'>BubbleLayer DataSource</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se define una matriz de posiciones y se crea un objeto [MultiPoint](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.multipoint?view=azure-iot-typescript-latest). Luego se crea un objeto de origen de datos mediante la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) y se agrega el objeto MultiPoint al origen de datos.

Una [capa de burbujas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) representa los datos basados en puntos encapsulados en el [origen de datos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como círculos en el mapa. El último bloque de código crea una capa de burbujas y la agrega al mapa. Puede consultar las propiedades de una capa de burbujas en [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

El objeto MultiPoint, el origen de datos y la capa de burbujas se crean y agregan al mapa dentro de la función [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para garantizar que se muestre el círculo una vez que el mapa se cargue completamente.

## <a name="add-bubble-markers-with-label"></a>Adición de marcadores de burbuja con etiqueta

<iframe height='500' scrolling='no' title='MultiLayer DataSource' src='//codepen.io/azuremaps/embed/rqbQXy/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/rqbQXy/'>MultiLayer DataSource</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El código anterior muestra cómo visualizar y etiquetar datos en el mapa. En el primer bloque de código anterior se construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código se crea un objeto [point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest). A continuación, se crea un objeto de origen de datos mediante la clase [origen de datos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) y se agrega el punto al origen de datos.

Una [capa de burbujas](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.bubblelayer?view=azure-iot-typescript-latest) representa los datos basados en puntos encapsulados en el [origen de datos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como círculos en el mapa. El tercer bloque de código crea una capa de burbujas y la agrega al mapa. Puede consultar las propiedades de una capa de burbujas en [BubbleLayerOptions](/javascript/api/azure-maps-control/atlas.bubblelayeroptions).

Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa. El último bloque de código crea y agrega una capa de símbolos al mapa que representa la etiqueta de texto de la burbuja. Consulte las propiedades de una capa de símbolos en [SymbolLayerOptions](/javascript/api/azure-maps-control/atlas.symbollayeroptions).

El origen de datos y las capas se crean y agregan al mapa dentro de la función [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para garantizar que se muestren los datos una vez que el mapa se cargue completamente.


## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [Map](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest)

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Adición de un elemento emergente](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Adición de una forma](./map-add-shape.md)