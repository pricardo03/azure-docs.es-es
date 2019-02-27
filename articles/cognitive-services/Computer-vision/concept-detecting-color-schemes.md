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
ms.date: 02/08/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 6b25da9b2569b0185d41684c45a22a3eb3377511
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313082"
---
# <a name="detect-color-schemes-in-images"></a>Detección de las combinaciones de colores de las imágenes

Computer Vision analiza los colores de una imagen para proporcionar tres atributos diferentes: el color de primer plano predominante, el color de fondo predominante y el conjunto de colores predominantes de la imagen como un todo. Los colores devueltos pertenecen al conjunto negro, azul, marrón, gris, verde, naranja, rosa, púrpura, rojo, verde azulado, blanco y amarillo. 

Computer Vision extrae también un color de énfasis, que representa el color más brillantes de la imagen, basándose en una combinación de colores dominantes y en la saturación. El color de énfasis se devuelve como un código de color HTML hexadecimal. 

Computer Vision también devuelve un valor booleano que indica si una imagen está en blanco y negro.

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

En la tabla siguiente se muestra el color de primer plano, el color de fondo y los colores de la imagen devueltos para cada imagen de ejemplo.

| Imagen | Colores predominantes |
|-------|-----------------|
|![Una flor blanca con un fondo verde](./Images/flower.png)| Primer plano: Negro<br/>Fondo: Blanco<br/>Colores: negro, blanco y verde|
![Un tren pasando por una estación](./Images/train_station.png) | Primer plano: Negro<br/>Fondo: Negro<br/>Colores: Negro |

### <a name="accent-color-examples"></a>Ejemplos de color de énfasis

 En la tabla siguiente se muestran los colores de énfasis devueltos, como un valor hexadecimal de HTML, para cada imagen de ejemplo.

| Imagen | Color de énfasis |
|-------|--------------|
|![Una persona en la roca de una montaña al atardecer](./Images/mountain_vista.png) | #BB6D10 |
|![Una flor blanca con un fondo verde](./Images/flower.png) | #C6A205 |
|![Un tren pasando por una estación](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Ejemplos de detección en blanco y negro

La siguiente tabla muestra la evaluación en blanco y negro de Computer Vision en las imágenes de ejemplo.

| Imagen | ¿Blanco y negro? |
|-------|----------------|
|![Una imagen en blanco y negro de edificios en Manhattan](./Images/bw_buildings.png) | true |
|![Una casa azul y el patio delantero](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos sobre [detectar tipos de imagen](concept-detecting-image-types.md).
