---
title: 'Generar miniaturas: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la generación de miniaturas de imágenes mediante Computer Vision API.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: ff16fa4d30fb60e6b1376027a40242af23be04d1
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985358"
---
# <a name="generating-thumbnails"></a>Generación de miniaturas

Una miniatura es una pequeña representación de una imagen a tamaño completo. La variedad de dispositivos, como teléfonos, tabletas y PC, hacen que sean necesarios diferentes diseños y tamaños de miniaturas para cada experiencia de usuario. Gracias a la opción del recorte inteligente, esta característica de Computer Vision le ayudará a resolver el problema.

Una vez cargada una imagen, Computer Vision genera una miniatura de alta calidad y, a continuación, analiza los objetos de la imagen para determinar la *región de interés* (ROI). Opcionalmente puede recortar la imagen para ajustarla a los requisitos de la región de interés. La miniatura generada se puede presentar con una relación de aspecto que sea diferente de la relación de aspecto de la imagen original para adaptarse a las necesidades del usuario.

El algoritmo de miniaturas funciona del siguiente modo:

1. Quita los elementos prescindibles de la imagen y reconoce el objeto principal, la "región de interés" (ROI).
2. Recorta la imagen en función de la región de interés identificada.
3. Cambia la relación de aspecto para ajustarse a las dimensiones de la miniatura de destino.

La miniatura generada puede variar enormemente en función de la altura, anchura y la función de recorte inteligente que se especifiquen, tal como se muestra en la siguiente imagen.

![Miniaturas](./Images/thumbnail-demo.png)

## <a name="thumbnail-generation-examples"></a>Ejemplos de generación de miniaturas

En la tabla siguiente se muestran las típicas miniaturas que se pueden generar con Computer Vision, teniendo en cuenta las imágenes de ejemplo. Las miniaturas se crearon para un destino con una altura y anchura específicas de 50 píxeles, y tienen la función de recorte inteligente habilitada.

| Imagen | Miniatura |
|-------|-----------|
|![Outdoor Mountain](./Images/mountain_vista.png) | ![Miniatura Outdoor Mountain](./Images/mountain_vista_thumbnail.png) |
|![Vision Analyze Flower](./Images/flower.png) | ![Miniatura Vision Analyze Flower](./Images/flower_thumbnail.png) |
|![Woman Roof](./Images/woman_roof.png) | ![Miniatura Woman Roof](./Images/woman_roof_thumbnail.png) |

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos de [etiquetado de imágenes](concept-tagging-images.md) y de [categorización de imágenes](concept-categorizing-images.md).