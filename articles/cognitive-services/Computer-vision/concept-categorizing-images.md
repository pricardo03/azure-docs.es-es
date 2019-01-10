---
title: 'Categorización de imágenes: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre los conceptos relacionados con la característica de categorización de imágenes de Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 40873b13407066920caec5c04751b65c01d7e209
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53579505"
---
# <a name="image-categorization-with-computer-vision"></a>Categorización de imágenes con Computer Vision

Además de etiquetas y descripciones, Computer Vision devuelve categorías basadas en la taxonomía de versiones anteriores. Estas categorías se organizan en una taxonomía con jerarquías hereditarias de elementos principales y secundarios. Todas las categorías están en inglés. Pueden usarse solas o con nuestros nuevos modelos de etiquetado.

## <a name="the-86-category-concept"></a>El concepto de las 86 categorías

Una imagen se puede clasificar de lo más amplio a lo más específico mediante la lista de 86 conceptos en el siguiente diagrama. Consulte la taxonomía completa en formato de texto en [Taxonomía de las categorías](category-taxonomy.md).

![Listas agrupadas de todas las categorías correspondientes a la taxonomía de categoría](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Ejemplos de categorización de imágenes

La siguiente respuesta JSON muestra lo que devuelve Computer Vision al categorizar la imagen de ejemplo según sus características visuales.

![Una mujer en el tejado de un edificio](./Images/woman_roof.png)

```json
{
    "categories": [
        {
            "name": "people_",
            "score": 0.81640625
        }
    ],
    "requestId": "bae7f76a-1cc7-4479-8d29-48a694974705",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

En la tabla siguiente se muestra un conjunto típico de imágenes y la categoría devuelta por Computer Vision para cada imagen.

| Imagen | Categoría |
|-------|----------|
| ![Cuatro personas posando juntas como una familia](./Images/family_photo.png) | people_group |
| ![Un cachorro sentado en un campo de hierba](./Images/cute_dog.png) | animal_dog |
| ![Una persona en la roca de una montaña al atardecer](./Images/mountain_vista.png) | outdoor_mountain |
| ![Una pila de panecillos en una tabla](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos de [etiquetado de imágenes](concept-tagging-images.md) y de [descripción de imágenes](concept-describing-images.md).