---
title: 'Reconocimiento del texto manuscrito e impreso: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con el reconocimiento del texto manuscrito e impreso en imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 6ee2b3282a5f895c61b9f0b55aa3353e534b6e37
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55191048"
---
# <a name="recognizing-printed-and-handwritten-text"></a>Reconocimiento de texto manuscrito e impreso

Computer Vision puede detectar y extraer texto impreso o manuscrito de imágenes que muestren diversos objetos con diferentes superficies y fondos, como recibos, pósteres, tarjetas de visita, cartas y pizarras.

El reconocimiento de texto le ahorrará tiempo y esfuerzo. Asimismo, puede ser más productivo si usa una imagen de texto en lugar de tener que transcribirlo. Gracias al reconocimiento de texto, puede digitalizar sus notas. Esta digitalización permite implementar una búsqueda rápida y sencilla, así como reducir el uso de papel.

## <a name="text-recognition-requirements"></a>Requisitos del reconocimiento de texto

Computer Vision puede reconocer el texto impreso y manuscrito de las imágenes que cumplan los requisitos siguientes:

- La imagen debe estar en formato JPEG, PNG, GIF o BMP
- El tamaño de archivo de la imagen debe ser inferior a 4 megabytes (MB)
- El tamaño de la imagen debe estar entre 50 x 50 y 4200 x 4200 píxeles.

> [!NOTE]
> Esta tecnología está actualmente en versión preliminar y solo está disponible para textos en inglés.

## <a name="next-steps"></a>Pasos siguientes

Conozca los conceptos acerca de la [extracción de texto con OCR](concept-extracting-text-ocr.md).
