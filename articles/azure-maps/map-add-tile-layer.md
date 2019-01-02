---
title: Adición de una capa de mosaico a Azure Maps | Microsoft Docs
description: Cómo agregar una capa de mosaico al mapa de JavaScript
author: rbrundritt
ms.author: richbrun
ms.date: 12/3/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: 57cde41bf65c370f86ebc57a39e917b95721ca81
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "52893324"
---
# <a name="add-a-tile-layer-to-a-map"></a>Adición de una capa de mosaico a un mapa

En este artículo se explica cómo puede superponer una capa de mosaico en el mapa. Las capas de mosaico permiten superponer imágenes encima de los mosaicos de mapa base de Azure Maps. Puede encontrar más información en el sistema de mosaico de Azure Maps en la documentación [Niveles de zoom y cuadrícula de mosaico](zoom-levels-and-tile-grid.md).

Una carga de capa de mosaico en los mosaicos desde un servidor. Estas imágenes pueden ser representadas previamente y almacenadas como cualquier otra imagen en un servidor mediante una convención de nomenclatura que entiende la capa de mosaico o un servicio dinámico que genera las imágenes sobre la marcha. Hay tres convenciones diferentes de nomenclatura de servicio de mosaico compatibles con la clase [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) de Azure Maps: 

* X, Y notación de zoom: según el nivel de zoom, x es la columna e y es la posición de fila del mosaico en la cuadrícula de mosaico.
* Notación Quadkey: combinación de la información de x, y y zoom en un solo valor de cadena que es un identificador único de un mosaico.
* Rectángulo delimitador: las coordenadas del rectángulo delimitador pueden usarse para especificar una imagen con el formato `{west},{south},{east},{north}`, que suele utilizarse en [Web Mapping Services (WMS)](http://www.opengeospatial.org/standards/wms).

> [!TIP]
> Un elemento [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) es una excelente manera de visualizar grandes conjuntos de datos en el mapa. No solo puede generarse una capa de mosaico a partir de una imagen, sino que también se pueden representar datos de vector como una capa de mosaico. Con la representación de datos de vectores como una capa de mosaico, el control de mapa solo necesita cargar los mosaicos que pueden tener un tamaño de archivo bastante más reducido que los datos de vector que representan. Esta técnica la usan muchos usuarios que necesitan representar millones de filas de datos en el mapa.

La dirección URL del mosaico pasada a una capa de mosaico debe ser una dirección URL HTTP/HTTPS que apunte a un recurso TileJSON o a una plantilla de URL de mosaico que usa los siguientes parámetros: 

* `{x}`: posición del mosaico en X. También necesita `{y}` y `{z}`.
* `{y}`: posición del mosaico en Y. También necesita `{x}` y `{z}`.
* `{z}`: nivel de zoom del mosaico. También necesita `{x}` y `{y}`.
* `{quadkey}`: identificador quadkey de mosaico basado en la convención de nomenclatura del sistema de mosaico de Bing Maps.
* `{bbox-epsg-3857}`: una cadena de un cuadro delimitador con el formato `{west},{south},{east},{north}` en el sistema de referencia espacial EPSG 3857.
* `{subdomain}`: un marcador de posición donde se agregarán los valores de subdominio si se especifican.

## <a name="add-a-tile-layer"></a>Adición de una capa de icono

 En este ejemplo se muestra cómo crear una capa de mosaico que apunte a un conjunto de mosaicos que usan el sistema de mosaico de zoom, x e y. La fuente de esta capa de mosaico es una superposición de radar meteorológico de [Iowa Environmental Mesonet of Iowa State University](http://mesonet.agron.iastate.edu/ogc/).

<br/>

<iframe height='500' scrolling='no' title='Capa de mosaico que usa X, Y y Z' src='//codepen.io/azuremaps/embed/BGEQjG/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/BGEQjG/'>capa de mosaico que usa X, Y y Z</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

En el código anterior, el primer bloque de código construye un objeto de mapa. Puede consultar [Creación de un mapa](./map-create.md) para obtener instrucciones.

En el segundo bloque de código, se crea un elemento [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest) pasando una dirección URL con formato a un servicio de mosaico, el tamaño de mosaico y una opacidad para que sea semitransparente. Además, al agregar la capa de mosaico al mapa, se agrega a continuación de la capa `labels` para que las etiquetas sigan siendo claramente visibles.

## <a name="customize-a-tile-layer"></a>Personalizar una capa de mosaico

La capa de mosaico solo tiene muchas opciones de estilo. Esta es una herramienta para probarlas.

<br/>

<iframe height='700' scrolling='no' title='Opciones de capa de mosaico' src='//codepen.io/azuremaps/embed/xQeRWX/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Consulte el Pen <a href='https://codepen.io/azuremaps/pen/xQeRWX/'>opciones de capa de mosaico</a> de Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) en <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Pasos siguientes

Más información sobre las clases y los métodos utilizados en este artículo:

> [!div class="nextstepaction"]
> [TileLayer](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayer?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [TileLayerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.tilelayeroptions?view=azure-iot-typescript-latest)

Para obtener más ejemplos de código para agregar a los mapas:

> [!div class="nextstepaction"]
> [Adición de una capa de imagen](./map-add-image-layer.md)