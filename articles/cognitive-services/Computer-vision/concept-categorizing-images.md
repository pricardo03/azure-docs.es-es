---
title: 'Categorización de imágenes: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre los conceptos relacionados con la característica de categorización de imágenes de Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 257da397e11843ee96e93f7b3e9bc5ada29822cf
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60203352"
---
# <a name="categorize-images-by-subject-matter"></a>Categorización de las imágenes por materia

Además de etiquetas y descripciones, Computer Vision devuelve las categorías basadas en la taxonomía que se detectan en una imagen. A diferencia de las etiquetas, las categorías se organizan en una jerarquía hereditaria de elementos primarios y secundarios, y son menos numerosas (86 en lugar de miles de etiquetas). Todos los nombres de categorías están en inglés. La categorización puede realizarse en solitario o junto con un nuevo modelo de etiquetas.

## <a name="the-86-category-concept"></a>El concepto de las 86 categorías

Computer Vision puede categorizar una imagen de manera general o específica utilizando las 86 categorías del diagrama siguiente. Consulte la taxonomía completa en formato de texto en [Taxonomía de las categorías](category-taxonomy.md).

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
