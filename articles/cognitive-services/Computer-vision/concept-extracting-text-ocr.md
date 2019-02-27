---
title: Extracción de texto con reconocimiento óptico de caracteres (OCR) en Computer Vision
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con la extracción de texto con reconocimiento óptico de caracteres (OCR) con Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: deb73eb9fdd6879a5fbe1fed820bf92b2d627b65
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310447"
---
# <a name="extract-text-with-optical-character-recognition"></a>Extracción de texto con reconocimiento óptico de caracteres

La tecnología de reconocimiento óptico de caracteres (OCR) de Computer Vision detecta el contenido de texto de una imagen y convierte el texto identificado en una secuencia de caracteres legible por una máquina. El resultado se puede usar para muchos fines, como en búsquedas, historiales médicos, seguridad y banca. 

OCR admite 25 idiomas: árabe, chino simplificado, chino tradicional, checo, danés, holandés, inglés, finés, francés, alemán, griego, húngaro, italiano, japonés, coreano, noruego, polaco, portugués, rumano, ruso, serbio (cirílico y latino), eslovaco, español, sueco y turco. OCR deduce automáticamente el idioma del texto detectado.

Si es necesario, OCR corrige el giro del texto reconocido devolviendo el desplazamiento de rotación en grados alrededor del eje horizontal de la imagen. OCR también proporciona las coordenadas del marco de cada palabra, como se observa en la ilustración siguiente.

![Un diagrama que representa una imagen que se gira y su texto siendo leído y definido](./Images/vision-overview-ocr.png)

## <a name="image-requirements"></a>Requisitos de imagen

Computer Vision puede extraer texto con OCR de las imágenes que cumplan los requisitos siguientes:

* La imagen se debe presentar en formato JPEG, PNG, GIF o BMP
* El tamaño de la imagen de entrada debe estar entre 50 x 50 y 4200 x 4200 píxeles.
* El texto de la imagen se puede girar cualquier múltiplo de 90 grados, además de un pequeño ángulo de hasta 40 grados.

## <a name="improving-ocr-accuracy"></a>Mejora de la precisión de OCR

La precisión del reconocimiento de texto depende de la calidad de la imagen. El origen de una lectura imprecisa puede deberse a:

* Imágenes borrosas.
* Texto escrito a mano o en cursiva.
* Estilos de fuente artísticos.
* Tamaño de texto pequeño.
* Fondos complejos, sombras, brillo sobre el texto o distorsión de la perspectiva.
* Letra mayúscula grande o falta de letras mayúsculas iniciales.
* Texto de superíndice, subíndice o tachado.

### <a name="ocr-limitations"></a>Limitaciones de OCR

En las imágenes en las que predomina el texto, las palabras que se reconocen de forma parcial pueden producir falsos positivos. En algunas imágenes, especialmente en las que no tienen texto, la precisión puede variar considerablemente según el tipo de imagen.

## <a name="next-steps"></a>Pasos siguientes

Consulte la [documentación de referencia del reconocimiento de texto](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) para más información.
