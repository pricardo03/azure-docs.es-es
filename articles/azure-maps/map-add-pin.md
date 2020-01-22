---
title: Adición de una capa de símbolo a un mapa | Microsoft Azure Maps
description: En este artículo, obtendrá información sobre cómo usar la capa de símbolo para personalizar y agregar símbolos a un mapa mediante el SDK web de Microsoft Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 744d5ecd3aab02071f7c3aaff7dd760fc14a2a62
ms.sourcegitcommit: f9601bbccddfccddb6f577d6febf7b2b12988911
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/12/2020
ms.locfileid: "75911156"
---
# <a name="add-a-symbol-layer-to-a-map"></a>Adición de una capa de símbolo a un mapa

Un símbolo puede estar conectado a un origen de datos y usarse para representar un icono o texto en un punto determinado. Las capas de símbolos se representan mediante WebGL y se pueden usar para representar grandes colecciones de puntos en el mapa. Esta capa puede representar muchos más datos de puntos en el mapa, con un buen rendimiento, que los que se pueden lograr con marcadores HTML. Sin embargo, la capa de símbolos no es compatible con los elementos CSS y HTML tradicionales para aplicar estilos.  

> [!TIP]
> De forma predeterminada, las capas de símbolo representarán las coordenadas de todos los objetos geométricos en un origen de datos. Para limitar la capa de forma que solo represente las características de geometría de puntos, configure la propiedad `filter` de la capa en `['==', ['geometry-type'], 'Point']` o `['any', ['==', ['geometry-type'], 'Point'], ['==', ['geometry-type'], 'MultiPoint']]` si desea incluir también las características de MultiPoint.

El administrador de sprites de imágenes de mapas, que se usa para cargar imágenes personalizadas que utiliza la capa de símbolos, admite los formatos de imagen siguientes:

- JPEG
- PNG
- SVG
- BMP
- GIF (sin animaciones)

## <a name="add-a-symbol-layer"></a>Adición de una capa de símbolo

Para agregar una capa de símbolos a los datos del mapa y de representación, primero es necesario crear un origen de datos y agregar el mapa. Después, se puede crear una capa de símbolos y pasarla en el origen de datos para recuperar los datos. Por último, los datos deben agregarse en el origen de datos para que haya algo que representar. En el código siguiente se muestra el código que debe agregarse al mapa una vez que se ha cargado para representar un único punto en el mapa mediante una capa de símbolos. 

```javascript
//Create a data source and add it to the map.
var dataSource = new atlas.source.DataSource();
map.sources.add(dataSource);

//Create a symbol layer to render icons and/or text at points on the map.
var layer = new atlas.layer.SymbolLayer(dataSource);

//Add the layer to the map.
map.layers.add(layer);

//Create a point and add it to the data source.
dataSource.add(new atlas.data.Point([0, 0]));
```

Hay cuatro tipos diferentes de datos de punto que se pueden agregar al mapa:

- Geometría Point de GeoJSON: Este objeto solo contiene una coordenada de un punto y nada más. La clase auxiliar `atlas.data.Point` se puede usar para crear fácilmente estos objetos.
- Geometría MultiPoint de GeoJSON: Este objeto contiene las coordenadas de varios puntos, pero nada más. La clase auxiliar `atlas.data.MultiPoint` se puede usar para crear fácilmente estos objetos.
- GeoJSON Feature: Este objeto se compone de cualquier geometría GeoJSON y un conjunto de propiedades que contienen metadatos asociados a la geometría. La clase auxiliar `atlas.data.Feature` se puede usar para crear fácilmente estos objetos.
- La clase `atlas.Shape` es similar a GeoJSON Feature en que se compone de una geometría GeoJSON y un conjunto de propiedades que contienen metadatos asociados a la geometría. Sin embargo, si se agrega un objeto GeoJSON a un origen de datos, puede representarse fácilmente en una capa; no obstante, si se actualiza la propiedad coordinates de ese objeto GeoJSON, el origen de datos y el mapa no cambian, ya que no hay ningún mecanismo en el objeto JSON para desencadenar una actualización. La clase Shape proporciona funciones para actualizar los datos que contiene y, cuando se hace un cambio, se notifica al origen de datos y al mapa y se actualizan automáticamente. 

En el ejemplo de código siguiente se crea una geometría Point de GeoJSON y se pasa a la clase `atlas.Shape` para facilitar su actualización. El centro del mapa se usa inicialmente para representar un símbolo. Se agrega un evento click al mapa, de modo que, cuando se activa, las coordenadas de donde se hizo clic con el mouse se usan con la función `setCoordinates` de la forma, lo que actualiza la ubicación del símbolo en el mapa.

<br/>

<iframe height='500' scrolling='no' title='Cambio del anclaje de ubicación' src='//codepen.io/azuremaps/embed/ZqJjRP/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen para <a href='https://codepen.io/azuremaps/pen/ZqJjRP/'>cambiar el anclaje de ubicación</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> De forma predeterminada, para el rendimiento, las capas de símbolos optimizan la representación de los símbolos ocultando los símbolos que se superponen. A medida que se acerca, los símbolos ocultos se hacen visibles. Para deshabilitar esta característica y representar todos los símbolos en todo momento, establezca la propiedad `allowOverlap` de las opciones `iconOptions` en `true`.

## <a name="add-a-custom-icon-to-a-symbol-layer"></a>Adición de un icono personalizado a una capa de símbolo

Las capas de símbolo se representan mediante WebGL. Por tanto, todos los recursos, como las imágenes de icono, se deben cargar en el contexto de WebGL. Este ejemplo muestra cómo agregar un icono personalizado a los recursos del mapa para usarlo a continuación para representar datos de punto con un símbolo personalizado en el mapa. La propiedad `textField` de la capa de símbolo requiere que se especifique una expresión. En este caso, queremos representar la propiedad de temperatura, pero puesto que es un número, debe convertirse en una cadena. Además, queremos anexarle "°F". Para hacer esto se puede usar una expresión; `['concat', ['to-string', ['get', 'temperature']], '°F']`. 

<br/>

<iframe height='500' scrolling='no' title='Icono de imagen de símbolo personalizado' src='//codepen.io/azuremaps/embed/WYWRWZ/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el fragmento de código <a href='https://codepen.io/azuremaps/pen/WYWRWZ/'>Custom Symbol Image Icon</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> El SDK de Azure Maps para web proporciona varias plantillas de imagen personalizables que puede usar con la capa de símbolo. Para más información, consulte el documento [How to use image templates](how-to-use-image-templates-web-sdk.md) (Uso de plantillas de imagen).

## <a name="customize-a-symbol-layer"></a>Personalización de una capa de símbolo 

La capa de símbolo tiene muchas opciones de estilo disponibles. Con esta herramienta puede probar las distintas opciones de estilo.

<br/>

<iframe height='700' scrolling='no' title='Opciones de capa de símbolo' src='//codepen.io/azuremaps/embed/PxVXje/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el fragmento de código <a href='https://codepen.io/azuremaps/pen/PxVXje/'>Symbol Layer Options</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> Si solo quiere representar texto con una capa de símbolo, puede ocultar el icono estableciendo la propiedad `image` de las opciones de icono en `'none'`.

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [SymbolLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.symbollayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [SymbolLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.symbollayeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [IconOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.iconoptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TextOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.textoptions?view=azure-iot-typescript-latest)

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Creación de un origen de datos](create-data-source-web-sdk.md)

> [!div class="nextstepaction"]
> [Adición de un elemento emergente](map-add-popup.md)

> [!div class="nextstepaction"]
> [Uso de expresiones de estilo controladas por datos](data-driven-style-expressions-web-sdk.md)

> [!div class="nextstepaction"]
> [Uso de plantillas de imagen](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Adición de una capa de línea](map-add-line-layer.md)

> [!div class="nextstepaction"]
> [Adición de una capa de polígono](map-add-shape.md)

> [!div class="nextstepaction"]
> [Adición de una capa de burbuja](map-add-bubble-layer.md)

> [!div class="nextstepaction"]
> [Adición de marcadores HTML](map-add-bubble-layer.md)
