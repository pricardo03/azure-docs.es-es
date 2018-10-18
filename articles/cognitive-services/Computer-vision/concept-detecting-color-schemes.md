---
title: 'Detección de combinaciones de colores: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la detección de combinaciones de colores en imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 5d0cb6ca751c844846288e8fe26f6ae542e89831
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49339499"
---
# <a name="detecting-color-schemes"></a>Detección de combinaciones de colores

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
|![Vision Analyze Flower](./Images/flower.png)| Primer plano: negro<br/>Fondo: blanco<br/>Colores: negro, blanco, verde|
![Vision Analyze Train Station](./Images/train_station.png) | Primer plano: negro<br/>Fondo: negro<br/>Colores: negro |

### <a name="accent-color-examples"></a>Ejemplos de color de énfasis

 En la tabla siguiente se describen los colores de énfasis, como un valor hexadecimal de HTML, para cada imagen de ejemplo tal y como Computer Vision los ha devuelto.

| Imagen | Color de énfasis |
|-------|--------------|
|![Outdoor Mountain](./Images/mountain_vista.png) | #BB6D10 |
|![Vision Analyze Flower](./Images/flower.png) | #C6A205 |
|![Vision Analyze Train Station](./Images/train_station.png) | #474A84 |

### <a name="black--white-detection-examples"></a>Ejemplos de detección en blanco y negro

En la tabla siguiente se indica si cada imagen de ejemplo está en blanco y negro, tal como lo devuelve Computer Vision.

| Imagen | ¿Blanco y negro? |
|-------|----------------|
|![Vision Analyze Building](./Images/bw_buildings.png) | true |
|![Vision Analyze House Yard](./Images/house_yard.png) | false |

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos sobre [detectar tipos de imagen](concept-detecting-image-types.md).