---
title: Adición de una capa de símbolo a Azure Maps | Microsoft Docs
description: Cómo agregar símbolos al mapa de JavaScript
author: rbrundritt
ms.author: richbrun
ms.date: 12/2/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 718a679418790a6bf1207a96e5c204f7962de239
ms.sourcegitcommit: 82cdc26615829df3c57ee230d99eecfa1c4ba459
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2019
ms.locfileid: "54411293"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Adición de una capa de símbolo a un mapa

En este artículo se explica cómo puede representar datos de punto de un origen de datos como una capa de símbolo en un mapa. Las capas de símbolo se representan mediante WebGL y admiten considerablemente más puntos de datos que los marcadores HTML, pero no son compatibles con los elementos CSS y HTML tradicionales de estilo.  

> [!TIP]
> De forma predeterminada, las capas de símbolo representarán las coordenadas de todos los objetos geométricos en un origen de datos. Para limitar la capa de tal forma que solo represente las características geométricas de punto, establezca la propiedad `filter` de la capa en `['==', '$type', 'Point']`.

## <a name="add-a-symbol-layer"></a>Adición de una capa de símbolo

<iframe height='500' scrolling='no' title='Cambio del anclaje de ubicación' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen para <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>cambiar el anclaje de ubicación</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el primer bloque de código anterior se construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Una [característica] que contiene una geometría [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) se encapsula con la clase [Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) para que sea más fácil de actualizar y, a continuación, se crea y se agrega al origen de datos.

En el tercer bloque de código se crea una [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) y se actualizan las coordenadas con el clic del mouse, utilizando la clase shape del método [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest).

Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa.  El origen de datos,la escucha de eventos de clic y la capa de símbolos se crean y agregan al mapa dentro de la función [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) para garantizar que se muestre el punto una vez que el mapa se cargue completamente.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Adición de un icono personalizado a una capa de símbolo

Las capas de símbolo se representan mediante WebGL. Por tanto, todos los recursos, como las imágenes de icono, se deben cargar en el contexto de WebGL. Este ejemplo muestra cómo agregar un icono de símbolo personalizado a los recursos del mapa para usarlo a continuación para representar datos de punto con un símbolo personalizado en el mapa. La propiedad `textField` de la capa de símbolo requiere que se especifique una expresión. En este caso, queremos representar la propiedad de temperatura de la característica de punto como el valor de texto. Esto puede lograrse con esta expresión: `['get', 'temperature']`. 

<br/>

<iframe height='500' scrolling='no' title='Icono de imagen de símbolo personalizado' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el fragmento de código <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>Custom Symbol Image Icon</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-symbol-layer"></a>Personalización de una capa de símbolo 

La capa de símbolo tiene muchas opciones de estilo disponibles. Con esta herramienta puede probar las distintas opciones de estilo.

<br/>

<iframe height='700' scrolling='no' title='Opciones de capa de símbolo' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el fragmento de código <a href='https://codepen.io/azuremaps/pen/PxVXje/'>Symbol Layer Options</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TexTOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Adición de un elemento emergente](./map-add-popup.md)

> [!div class="nextstepaction"]
> [Adición de una forma](./map-add-shape.md)

> [!div class="nextstepaction"]
> [Adición de una capa de burbuja](./map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Adición de marcadores HTML](./map-add-bubble-layer.md)
