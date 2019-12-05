---
title: Adición de una capa de imagen a Azure Maps | Microsoft Docs
description: Cómo agregar una capa de imagen al SDK web de Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: fadaaf7c64b11a6d6d94c68234f8288d1b3f8d07
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2019
ms.locfileid: "74480482"
---
# <a name="add-an-image-layer-to-a-map"></a>Adición de una capa de imagen a un mapa

En este artículo se explica cómo puede superponer una imagen a un conjunto fijo de coordenadas del mapa. Hay muchos escenarios en los que se realiza la superposición de una imagen en el mapa. Estos son algunos ejemplos del tipo de imágenes que suelen superponerse en los mapas:

* Imágenes capturadas de drones.
* Creación de planos.
* Imágenes de mapas históricas u otras imágenes de mapa especializadas.
* Proyectos de sitios de trabajo.
* Imágenes de radar meteorológico.

> [!TIP]
> Un elemento [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) es una manera rápida y sencilla de superponer una imagen en un mapa. Sin embargo, si la imagen es grande, el explorador puede tener dificultades para cargarla. En este caso, considere la posibilidad de dividir la imagen en mosaicos y cargarlos en el mapa como un elemento [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest).

La capa de imagen admite los siguientes formatos de imagen:

- JPEG
- PNG
- BMP
- GIF (sin animaciones)

## <a name="add-an-image-layer"></a>Adición de una capa de imagen

En el código siguiente se superpone una imagen de un [mapa de Newark, Nueva Jersey, de 1922](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) en el mapa. Se crea un elemento [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) pasando una dirección URL a una imagen y las coordenadas de las cuatro esquinas en el formato `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]`.

```javascript
//Create an image layer and add it to the map.
map.layers.add(new atlas.layer.ImageLayer({
    url: 'newark_nj_1922.jpg',
    coordinates: [
        [-74.22655, 40.773941], //Top Left Corner
        [-74.12544, 40.773941], //Top Right Corner
        [-74.12544, 40.712216], //Bottom Right Corner
        [-74.22655, 40.712216]  //Bottom Left Corner
    ]
}));
```

A continuación se muestra el código de ejemplo de ejecución completo de la funcionalidad anterior.

<br/>

<iframe height='500' scrolling='no' title='Capa de imagen sencilla' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/eQodRo/'>capa de imagen sencilla</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="import-a-kml-ground-overlay"></a>Importar una superposición de suelo KML

Este ejemplo muestra cómo superponer la información de superposición de suelo KML como una capa de imagen en el mapa. Las superposiciones de suelo KML proporcionan las coordenadas de norte, sur, este y oeste y una rotación hacia la izquierda, mientras que la capa de imagen espera las coordenadas para cada esquina de la imagen. La superposición de suelo KML de este ejemplo se corresponde con la catedral de Chartres, cuya fuente es [Wikimedia](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml).

En el código siguiente se usa la función `getCoordinatesFromEdges` estática de la clase [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) para calcular las cuatro esquinas de la imagen desde el norte, el sur, el este, el oeste, y la información de rotación a partir de la superposición de suelo KML.

<br/>

<iframe height='500' scrolling='no' title='Superposición de suelo KML como capa de imagen' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>Superposición de suelo KML como capa de imagen</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-an-image-layer"></a>Personalización de una capa de imagen

La capa de imagen tiene muchas opciones de estilo. Esta es una herramienta para probarlas.

<br/>

<iframe height='700' scrolling='no' title='Opciones de capa de imagen' src='//codepen.io/azuremaps/embed/RqOGzx/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/RqOGzx/'>Opciones de capa de imagen</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [ImageLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.imagelayeroptions?view=azure-iot-typescript-latest)

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Adición de una capa de icono](./map-add-tile-layer.md)