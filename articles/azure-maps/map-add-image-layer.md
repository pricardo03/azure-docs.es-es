---
title: Adición de una capa de imagen a un mapa | Microsoft Azure Maps
description: En este artículo, obtendrá información sobre cómo superponer una capa de imagen en un mapa mediante el SDK web de Microsoft Azure Maps.
author: rbrundritt
ms.author: richbrun
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: cfdf7dfd4c16f70065e338f8983d2124d3f6f0ef
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76933213"
---
# <a name="add-an-image-layer-to-a-map"></a>Adición de una capa de imagen a un mapa

En este artículo se explica cómo puede superponer una imagen a un conjunto fijo de coordenadas del mapa. Estos son algunos ejemplos del tipo de imágenes que suelen superponerse en los mapas:

* Imágenes capturadas por drones
* Planos de edificios
* Imágenes de mapa históricas u otras imágenes de mapa especializadas
* Planos técnicos de sitios de trabajo
* Imágenes de radar meteorológico

> [!TIP]
> [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) es un modo sencillo de superponer una imagen en un mapa. Tenga en cuenta que los exploradores pueden tener dificultades para cargar una imagen grande. En este caso, considere la posibilidad de dividir la imagen en mosaicos y cargarlos en el mapa como [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest).

La capa de imagen admite los siguientes formatos de imagen:

- JPEG
- PNG
- BMP
- GIF (sin animaciones)

## <a name="add-an-image-layer"></a>Adición de una capa de imagen

En el código siguiente se superpone una imagen de un [mapa de Newark, Nueva Jersey, de 1922](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) en el mapa. Se crea un elemento [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) pasando una dirección URL a una imagen y las coordenadas de las cuatro esquinas en el formato `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]`.

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

A continuación se muestra el código de ejemplo de ejecución completo del código anterior.

<br/>

<iframe height='500' scrolling='no' title='Capa de imagen sencilla' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/eQodRo/'>capa de imagen sencilla</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="import-a-kml-file-as-ground-overlay"></a>Importación de un archivo KML como una superposición de suelo

En el siguiente ejemplo se muestra cómo superponer la información de superposición de suelo KML como capa de imagen en el mapa. Las superposiciones de suelo KML proporcionan las coordenadas de norte, sur, este y oeste y una rotación en sentido contrario a las agujas del reloj, mientras que la capa de imagen espera las coordenadas para cada esquina de la imagen. La superposición de suelo KML de este ejemplo se corresponde con la catedral de Chartres, cuya fuente es [Wikimedia](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml).

En el código siguiente se usa la función `getCoordinatesFromEdges` estática de la clase [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest). Este código calcula las cuatro esquinas de la imagen a partir de la información de norte, sur, este, oeste y rotación procedente de la superposición de suelo KML.

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