---
title: Adición de una capa de imagen a Azure Maps | Microsoft Docs
description: Cómo agregar una capa de imagen al mapa de JavaScript
author: rbrundritt
ms.author: richbrun
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 5396fefca3a60dea7a503f8b4e84cc575753ea30
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "54229122"
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

## <a name="add-an-image-layer"></a>Adición de una capa de imagen

En este ejemplo se muestra cómo superponer una imagen de un [mapa de Newark, Nueva Jersey, de 1922](https://www.lib.utexas.edu/maps/historical/newark_nj_1922.jpg) en el mapa.

<br/>

<iframe height='500' scrolling='no' title='Capa de imagen sencilla' src='//codepen.io/azuremaps/embed/eQodRo/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/eQodRo/'>capa de imagen sencilla</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un elemento [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) pasando una dirección URL a una imagen y las coordenadas de las cuatro esquinas en el formato `[Top Left Corner, Top Right Corner, Bottom Right Corner, Bottom Left Corner]`.

## <a name="import-a-kml-ground-overlay"></a>Importar una superposición de suelo KML

Este ejemplo muestra cómo superponer la información de superposición de suelo KML como una capa de imagen en el mapa. Las superposiciones de suelo KML proporcionan las coordenadas de norte, sur, este y oeste y una rotación hacia la izquierda, donde la capa de imagen espera las coordenadas para cada esquina de la imagen. La superposición de suelo KML de este ejemplo se corresponde con la catedral de Chartres, cuya fuente es [Wikimedia](https://commons.wikimedia.org/wiki/File:Chartres.svg/overlay.kml).

<br/>

<iframe height='500' scrolling='no' title='Superposición de suelo KML como capa de imagen' src='//codepen.io/azuremaps/embed/EOJgpj/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/EOJgpj/'>Superposición de suelo KML como capa de imagen</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

El código anterior usa la función `getCoordinatesFromEdges` estática de la clase [ImageLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.imagelayer?view=azure-iot-typescript-latest) para calcular las cuatro esquinas de la imagen desde el norte, el sur, el este, el oeste y la información de rotación a partir de la superposición de suelo KML.


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