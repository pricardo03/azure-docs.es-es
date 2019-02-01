---
title: 'Detección de tipos de imagen: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la característica de detección de tipos de imagen de Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: da7cce0cd69823f254cd6b666dc468f155b9e097
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55156282"
---
# <a name="detecting-image-types-with-computer-vision"></a>Detección de tipos de imagen con Computer Vision

Computer Vision puede analizar el tipo de contenido de las imágenes e indicar si se trata de una imagen prediseñada (mediante la evaluación de la probabilidad según una escala) o un dibujo lineal.

## <a name="detecting-clip-art"></a>Detección de imágenes prediseñadas

Computer Vision analiza una imagen y evalúa la probabilidad de que sea una imagen prediseñada en una escala de 0 a 3, tal y como se describe en la tabla siguiente.

| Valor | Significado |
|-------|---------|
| 0 | No es imagen prediseñada |
| 1 | Ambigua |
| 2 | Imagen prediseñada normal |
| 3 | Imagen prediseñada buena |

### <a name="clip-art-detection-examples"></a>Ejemplos de detección de imágenes prediseñadas

Las respuestas JSON siguientes muestran lo que devuelve Computer Vision cuando evalúa la probabilidad de que las imágenes de ejemplo sean imágenes prediseñadas.

![Una imagen prediseñada de un pedazo de queso](./Images/cheese_clipart.png)

```json
{
    "imageType": {
        "clipArtType": 3,
        "lineDrawingType": 0
    },
    "requestId": "88c48d8c-80f3-449f-878f-6947f3b35a27",
    "metadata": {
        "height": 225,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Una casa azul y el patio delantero](./Images/house_yard.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "a9c8490a-2740-4e04-923b-e8f4830d0e47",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="detecting-line-drawings"></a>Detección de dibujos lineales

Computer Vision analiza una imagen y devuelve un valor booleano que indica si la imagen es un dibujo lineal.

### <a name="line-drawing-detection-examples"></a>Ejemplos de detección de dibujos lineales

Las respuestas JSON siguientes muestran lo que devuelve Computer Vision al informar si las imágenes de ejemplo son dibujos lineales.

![Una imagen de dibujo lineal de un león](./Images/lion_drawing.png)

```json
{
    "imageType": {
        "clipArtType": 2,
        "lineDrawingType": 1
    },
    "requestId": "6442dc22-476a-41c4-aa3d-9ceb15172f01",
    "metadata": {
        "height": 268,
        "width": 300,
        "format": "Jpeg"
    }
}
```

![Una flor blanca con un fondo verde](./Images/flower.png)

```json
{
    "imageType": {
        "clipArtType": 0,
        "lineDrawingType": 0
    },
    "requestId": "98437d65-1b05-4ab7-b439-7098b5dfdcbf",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos de [etiquetado de imágenes](concept-tagging-images.md) y de [categorización de imágenes](concept-categorizing-images.md).
