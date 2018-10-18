---
title: 'Etiquetado de imágenes: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con el etiquetado de imágenes mediante Computer Vision API.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 159a1b9404f6ce635dcc8bb6007e52b7346ef98d
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45982976"
---
# <a name="tagging-images"></a>Etiquetado de imágenes

Computer Vision devuelve etiquetas basándose en más de 2000 objetos, seres vivos, paisajes y acciones reconocibles. Cuando las etiquetas son ambiguas o no muy comunes, la respuesta de la API contiene «indicaciones» para aclarar el significado de la etiqueta en el contexto de un entorno conocido. Las etiquetas no están organizadas por su taxonomía y no existen jerarquías de herencia. Una colección de etiquetas de contenido es la base de la "descripción" de una imagen que se muestra en lenguaje natural con formato de oraciones completas. Tenga en cuenta que, en este momento, el inglés es el único idioma admitido para la descripción de la imagen.

Después de cargar una imagen o especificar la URL de una imagen, los algoritmos de Computer Vision devuelven como salida etiquetas basadas en los objetos, seres vivos, paisajes y acciones identificados en la imagen. El etiquetado no se limita al sujeto principal, como una persona en primer plano, sino que también incluye el entorno (interior o exterior), muebles, herramientas, plantas, animales, accesorios, gadgets, etc.

## <a name="image-tagging-example"></a>Ejemplo de etiquetado de imágenes

La siguiente respuesta JSON muestra lo que devuelve Computer Vision al etiquetar las características visuales detectadas en la imagen de ejemplo.

![House_Yard](./Images/house_yard.png).

```json
{
    "tags": [
        {
            "name": "grass",
            "confidence": 0.9999995231628418
        },
        {
            "name": "outdoor",
            "confidence": 0.99992108345031738
        },
        {
            "name": "house",
            "confidence": 0.99685388803482056
        },
        {
            "name": "sky",
            "confidence": 0.99532157182693481
        },
        {
            "name": "building",
            "confidence": 0.99436837434768677
        },
        {
            "name": "tree",
            "confidence": 0.98880356550216675
        },
        {
            "name": "lawn",
            "confidence": 0.788884699344635
        },
        {
            "name": "green",
            "confidence": 0.71250593662261963
        },
        {
            "name": "residential",
            "confidence": 0.70859086513519287
        },
        {
            "name": "grassy",
            "confidence": 0.46624681353569031
        }
    ],
    "requestId": "06f39352-e445-42dc-96fb-0a1288ad9cf1",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos de [categorización de imágenes](concept-categorizing-images.md) y de [descripción de imágenes](concept-describing-images.md).