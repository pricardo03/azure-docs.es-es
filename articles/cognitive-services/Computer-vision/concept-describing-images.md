---
title: 'Descripción de imágenes: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la característica de descripción de imágenes de Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 91618b211fdd869daf74491b175d6359ffa3f30c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60368396"
---
# <a name="describe-images-with-human-readable-language"></a>Descripción de imágenes con lenguaje natural

Computer Vision puede analizar una imagen y generar una frase inteligible que describa su contenido. El algoritmo realmente devuelve varias descripciones según diferentes características visuales y cada descripción tiene una puntuación de confianza. El resultado final es una lista de descripciones ordenadas de mayor a menor confianza.

## <a name="image-description-example"></a>Ejemplo de descripción de imagen

La siguiente respuesta JSON muestra lo que devuelve Computer Vision al describir la imagen de ejemplo según sus características visuales.

![Una imagen en blanco y negro de edificios en Manhattan](./Images/bw_buildings.png)

```json
{
    "description": {
        "tags": ["outdoor", "building", "photo", "city", "white", "black", "large", "sitting", "old", "water", "skyscraper", "many", "boat", "river", "group", "street", "people", "field", "tall", "bird", "standing"],
        "captions": [
            {
                "text": "a black and white photo of a city",
                "confidence": 0.95301952483304808
            },
            {
                "text": "a black and white photo of a large city",
                "confidence": 0.94085190563213816
            },
            {
                "text": "a large white building in a city",
                "confidence": 0.93108362931954824
            }
        ]
    },
    "requestId": "b20bfc83-fb25-4b8d-a3f8-b2a1f084b159",
    "metadata": {
        "height": 300,
        "width": 239,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos de [etiquetado de imágenes](concept-tagging-images.md) y de [categorización de imágenes](concept-categorizing-images.md).
