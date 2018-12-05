---
title: 'Categorización de imágenes: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la categorización de imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.openlocfilehash: 7062d98d40c15f4e9e873038fc12fc1b104c996d
ms.sourcegitcommit: 922f7a8b75e9e15a17e904cc941bdfb0f32dc153
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52333486"
---
# <a name="categorizing-images"></a>Categorización de imágenes

Además de etiquetas y descripciones, Computer Vision devuelve categorías basadas en la taxonomía de versiones anteriores. Estas categorías se organizan en una taxonomía con jerarquías hereditarias de elementos principales y secundarios. Todas las categorías están en inglés. Pueden usarse solas o con nuestros nuevos modelos de etiquetado.

## <a name="the-86-category-concept"></a>El concepto de las 86 categorías

Una imagen se puede clasificar de lo más amplio a lo más específico mediante la lista de 86 conceptos en el siguiente diagrama. Consulte la taxonomía completa en formato de texto en [Taxonomía de las categorías](category-taxonomy.md).

![listas agrupadas de todas las categorías correspondientes a la taxonomía de categoría](./Images/analyze_categories-v2.png)

## <a name="image-categorization-examples"></a>Ejemplos de categorización de imágenes

La siguiente respuesta JSON muestra lo que devuelve Computer Vision al categorizar la imagen de ejemplo según sus características visuales.

![Woman Roof](./Images/woman_roof.png)

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
| ![Family Photo](./Images/family_photo.png) | people_group |
| ![Cute Dog](./Images/cute_dog.png) | animal_dog |
| ![Outdoor Mountain](./Images/mountain_vista.png) | outdoor_mountain |
| ![Vision Analyze Food Bread](./Images/bread.png) | food_bread |

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos de [etiquetado de imágenes](concept-tagging-images.md) y de [descripción de imágenes](concept-describing-images.md).