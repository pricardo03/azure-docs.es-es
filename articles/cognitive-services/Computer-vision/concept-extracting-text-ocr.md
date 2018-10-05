---
title: 'Extracción de texto con OCR: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la extracción de texto con reconocimiento óptico de caracteres (OCR) con Computer Vision API.
services: cognitive-services
author: deken
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: conceptual
ms.date: 08/29/2018
ms.author: v-deken
ms.openlocfilehash: 4cbc8ebcdd19aa65a37ffe0f82019a7681f1c6c5
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47434881"
---
# <a name="extracting-text-with-ocr"></a>Extracción de texto con OCR

La tecnología de reconocimiento óptico de caracteres (OCR) de Computer Vision detecta el contenido de texto de una imagen y extrae el texto identificado en una secuencia de caracteres de lectura mecánica. El resultado se puede usar para búsquedas y otros propósitos, como registros médicos, seguridad y banca. Detecta automáticamente el idioma. OCR ahorra tiempo y es cómodo para los usuarios, ya que permite tomar fotografías del texto en lugar de transcribirlo.

OCR admite 25 idiomas, que son: árabe, chino simplificado, chino tradicional, checo, danés, holandés, inglés, finés, francés, alemán, griego, húngaro, italiano, japonés, coreano, noruego, polaco, portugués, rumano, ruso, serbio (cirílico y latino), eslovaco, español, sueco y turco.

Si es necesario, OCR corrige el giro del texto reconocido, en grados, alrededor del eje horizontal de la imagen. OCR proporciona las coordenadas del marco de cada palabra, como se observa en la ilustración siguiente.

![Información general de OCR](./Images/vision-overview-ocr.png)

## <a name="ocr-requirements"></a>Requisitos de OCR

Computer Vision puede extraer texto con OCR de las imágenes que cumplan los requisitos siguientes:

* La imagen se debe presentar en formato JPEG, PNG, GIF o BMP
* El tamaño de la imagen de entrada debe estar entre 50 x 50 y 4200 x 4200 píxeles.
* La imagen no puede ser mayor de 10 megapíxeles.

La imagen de entrada se puede girar en cualquier múltiplo de 90 grados, además de un pequeño ángulo de hasta 40 grados.

## <a name="improving-ocr-accuracy"></a>Mejora de la precisión de OCR

La precisión del reconocimiento de texto depende de la calidad de la imagen. El origen de una lectura imprecisa puede ser:

* Imágenes borrosas.
* Texto escrito a mano o en cursiva.
* Estilos de fuente artísticos.
* Tamaño de texto pequeño.
* Fondos complejos, sombras, brillo sobre el texto o distorsión de la perspectiva.
* Letra mayúscula grande o falta de letras mayúsculas iniciales.
* Texto de superíndice, subíndice o tachado.

### <a name="ocr-limitations"></a>Limitaciones de OCR

En las fotografías en las que predomina el texto, las palabras que se reconocen de forma parcial pueden producir falsos positivos. En algunas fotografías, especialmente las que no tienen texto, la precisión puede variar considerablemente según el tipo de imagen.

## <a name="next-steps"></a>Pasos siguientes

Más información de conceptos sobre [reconocimiento de texto manuscrito e impreso](concept-recognizing-text.md).
