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
ms.openlocfilehash: 3225ae919e221935b6d8a52e20d943d2178f6a47
ms.sourcegitcommit: b4ad15a9ffcfd07351836ffedf9692a3b5d0ac86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59056859"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Adición de una capa de símbolo a un mapa

En este artículo se explica cómo puede representar datos de punto de un origen de datos como una capa de símbolo en un mapa. Símbolo capas se representan mediante WebGL y admiten mucho más grandes conjuntos de puntos que marcadores de HTML, pero no son compatibles con los elementos CSS y HTML tradicionales para establecer el estilo.  

> [!TIP]
> De forma predeterminada, las capas de símbolo representarán las coordenadas de todos los objetos geométricos en un origen de datos. Para limitar la capa de forma que solo se representa la geometría de punto de conjunto de características el `filter` propiedad de la capa a `['==', ['geometry-type'], 'Point']` o `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` si desea incluir también las características de MultiPoint.

## <a name="add-a-symbol-layer"></a>Adición de una capa de símbolo

<iframe height='500' scrolling='no' title='Cambio del anclaje de ubicación' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen para <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>cambiar el anclaje de ubicación</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el primer bloque de código anterior se construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un objeto de origen de datos con la clase [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest). Una [característica] que contiene una geometría [Point](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.point?view=azure-iot-typescript-latest) se encapsula con la clase [Shape](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest) para que sea más fácil de actualizar y, a continuación, se crea y se agrega al origen de datos.

En el tercer bloque de código se crea una [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) y se actualizan las coordenadas con el clic del mouse, utilizando la clase shape del método [setCoordinates](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest).

Una [capa de símbolos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest) usa texto o iconos para representar los datos basados en puntos encapsulados en [DataSource](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.source.datasource?view=azure-iot-typescript-latest) como símbolos en el mapa.  El origen de datos, el agente de escucha de eventos de clic y la capa de símbolos se crean y agregan a la asignación dentro de la `ready` [escucha de eventos](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.map?view=azure-iot-typescript-latest#events) función para asegurarse de que el punto se muestra después de la asignación cargada y lista para tener acceso.

> [!TIP]
> De forma predeterminada, para el rendimiento, las capas de símbolos optimización el procesamiento de los símbolos ocultando los símbolos que se superponen. A medida que los símbolos ocultos se hacen visibles. Para deshabilitar esta característica y representar todos los símbolos en todo momento, establezca la `allowOverlap` propiedad de la `iconOptions` opciones `true`.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Adición de un icono personalizado a una capa de símbolo

Las capas de símbolo se representan mediante WebGL. Por tanto, todos los recursos, como las imágenes de icono, se deben cargar en el contexto de WebGL. Este ejemplo muestra cómo agregar un icono personalizado para la asignación de recursos y, a continuación, se utiliza para representar el punto de datos con un símbolo en el mapa personalizado. La propiedad `textField` de la capa de símbolo requiere que se especifique una expresión. En este caso, queremos representar la propiedad temperature pero puesto que es un número, debe convertirse en una cadena. Además, queremos anexar la "° F" en él. Una expresión puede utilizarse para hacer esto; `['concat', ['to-string', ['get', 'temperature']], '°F']`. 

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
> [Agregue los creadores de HTML](./map-add-bubble-layer.md)
