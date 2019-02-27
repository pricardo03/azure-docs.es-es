---
title: 'Reconocimiento del texto manuscrito e impreso: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con el reconocimiento del texto manuscrito e impreso en imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 48ce15a11c3e3282535420f3e1bb1915276d70f5
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56313184"
---
# <a name="recognizing-printed-and-handwritten-text"></a>Reconocimiento de texto manuscrito e impreso

Computer Vision puede detectar y extraer texto impreso o manuscrito de imágenes que muestren diversos objetos con diferentes superficies y fondos, como recibos, pósteres, tarjetas de visita, cartas y pizarras.

La característica de reconocimiento de texto es muy similar al [reconocimiento óptico de caracteres (OCR)](concept-extracting-text-ocr.md), pero se diferencia en que se ejecuta de forma asincrónica y usa modelos de reconocimiento actualizados.

> [!NOTE]
> Esta tecnología está actualmente en versión preliminar y solo está disponible para textos en inglés.

## <a name="text-recognition-requirements"></a>Requisitos del reconocimiento de texto

Computer Vision puede reconocer el texto impreso y manuscrito de las imágenes que cumplan los requisitos siguientes:

- La imagen debe estar en formato JPEG, PNG, GIF o BMP
- El tamaño de archivo de la imagen debe ser inferior a 4 megabytes (MB)
- El tamaño de la imagen debe estar entre 50 x 50 y 4200 x 4200 píxeles.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [documentación de referencia del reconocimiento de texto](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) para más información.