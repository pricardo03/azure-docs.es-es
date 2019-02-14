---
title: 'Detección de combinaciones de colores: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la detección de combinaciones de colores en imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 0793f572e043248af409e65cca4fd854f1371900
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55880895"
---
# <a name="detect-color-schemes-in-images"></a>Detección de las combinaciones de colores de las imágenes

Computer Vision extrae los colores de una imagen. Después, los colores se analizan en tres contextos diferentes: el color de primer plano predominante, el color de fondo predominante y los colores predominantes de la imagen como un todo. Se agrupan en 12 colores de énfasis predominantes. que son: negro, azul, marrón, gris, verde, naranja, rosa, púrpura, rojo, verde azulado, blanco y amarillo. Computer Vision analiza los colores que se extraen de una imagen para devolver el color de énfasis que representa al color más brillante de la imagen para su visualización, mediante una combinación de colores predominantes y de saturación. En función de los colores de una imagen, se pueden devolver el blanco y negro básico o los colores de énfasis en códigos de color hexadecimales. Computer Vision también devuelve un valor booleano que indica si una imagen está en blanco y negro.

## <a name="color-scheme-detection-examples"></a>Ejemplos de detección de la combinación de colores

El ejemplo siguiente muestra la respuesta JSON devuelta por Computer Vision durante la detección de la combinación de colores de la imagen de ejemplo. En este caso, la imagen de ejemplo no es una imagen en blanco y negro, pero el color predominante de primer plano y de fondo es el negro, y los colores predominantes de la imagen como un todo son el blanco y el negro.

![Outdoor Mountain](./Images/mountain_vista.png)

```json
{
    "color": {
        "dominantColorForeground": "Black",
        "dominantColorBackground": "Black",
        "dominantColors": ["Black", "White"],
        "accentColor": "BB6D10",
        "isBwImg": false
    },
    "requestId": "0dc394bf-db50-4871-bdcc-13707d9405ea",
    "metadata": {
        "height": 202,
        "width": 300,
        "format": "Jpeg"
    }
}
```

### <a name="dominant-color-examples"></a>Ejemplos de color predominante

En la tabla siguiente se describen los colores predominantes de primer plano, fondo y de imagen para cada imagen de ejemplo tal y como Computer Vision los ha devuelto.

| Imagen | Colores predominantes |
|-------|-----------------|
|![Una flor blanca con un fondo verde](./Images/flower.png)| Primer plano: Negro<br/>Fondo: Blanco<br/>Colores: negro, blanco y verde|
![Un tren pasando por una estación](./Images/train_station.png) | Primer plano: Negro<br/>Fondo: Negro<br/>Colores: Negro |

### <a name="accent-color-examples"></a>Ejemplos de color de énfasis

 En la tabla siguiente se describen los colores de énfasis, como un valor hexadecimal de HTML, para cada imagen de ejemplo tal y como Computer Vision los ha devuelto.

| Imagen | Color de énfasis |
|-------|--------------|
|![Una persona en la roca de una montaña al atardecer](./Images/mountain_vista.png) | #BB6D10 |
|![Una flor blanca con un fondo verde](./Images/flower.png) | #C6A205 |
|![Un tren pasando por una estación](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Ejemplos de detección en blanco y negro

En la tabla siguiente se indica si cada imagen de ejemplo está en blanco y negro, tal como lo devuelve Computer Vision.

| Imagen | ¿Blanco y negro? |
|-------|----------------|
|![Una imagen en blanco y negro de edificios en Manhattan](./Images/bw_buildings.png) | true |
|![Una casa azul y el patio delantero](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos sobre [detectar tipos de imagen](concept-detecting-image-types.md).
