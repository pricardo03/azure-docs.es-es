---
title: Reconocer texto manuscrito o impreso, Computer Vision
titleSuffix: Azure Cognitive Services
description: Conceptos relacionados con el reconocimiento del texto manuscrito e impreso en imágenes mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: bcaa990cc2186a5f1eecdbbca91804c92370277c
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66357178"
---
# <a name="recognize-printed-and-handwritten-text"></a>Reconocimiento de texto manuscrito e impreso

Visión de equipo proporciona varios servicios que detectar y extraer texto manuscrito o impreso que aparece en las imágenes. Esto es útil en una variedad de escenarios, como la toma de notas, los registros médicos, seguridad y banca. Lo siguientes tres secciones detalle tres diferentes reconocimiento de texto API, cada uno optimizado para distintos casos de uso.

## <a name="read-api"></a>API de lectura

La API de lectura detecta el contenido de texto de una imagen mediante nuestros modelos más recientes de reconocimiento y convierte en una secuencia de caracteres que el texto identificado. Está optimizado para imágenes de gran cantidad de texto (por ejemplo, los documentos que han sido analizados digitalmente) y para las imágenes con mucho ruido visual. Determinará qué modelo de reconocimiento que se usará para cada línea de texto, compatibilidad con imágenes con texto escrito a mano e impresa. La API de lectura se ejecuta de forma asincrónica porque los documentos más grandes pueden tardar varios minutos en devolver un resultado.

La operación de lectura mantiene las agrupaciones de línea original de palabras reconocidas en su salida. Cada línea incluye las coordenadas del cuadro de límite, y cada palabra dentro de la línea también tiene su propio coordenadas. Si una palabra se reconoció con confianza baja, dicha información se muestra también. Consulte la [documentos de referencia de API de lectura](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) para obtener más información.

> [!NOTE]
> Esta característica solo está disponible para texto en inglés.

### <a name="image-requirements"></a>Requisitos de imagen

La API de lectura funciona con imágenes que cumplan los requisitos siguientes:

- La imagen debe presentarse en formato JPEG, PNG, BMP, TIFF o PDF.
- Las dimensiones de la imagen deben estar entre 50 x 50 y 4200 x 4200 píxeles. Páginas PDF deben ser 17 x 17 pulgadas o más pequeños.
- El tamaño del archivo de la imagen debe ser menor que 20 megabytes (MB).

### <a name="limitations"></a>Limitaciones

Si usa una suscripción de nivel gratis, la API de lectura solo procesará las dos primeras páginas de un documento PDF o TIFF. Con una suscripción de pago, procesará hasta 200 páginas. Tenga en cuenta también que la API detectará un máximo de 300 líneas por página.

## <a name="ocr-optical-character-recognition-api"></a>OCR (reconocimiento óptico de caracteres) API

Es similar a la API de lectura API de reconocimiento óptico de caracteres (OCR de Computer Vision), pero se ejecuta de forma sincrónica y no está optimizado para documentos de gran tamaño. Usa un modelo de reconocimiento anterior, pero funciona con más idiomas; consulte [compatibilidad con idiomas](language-support.md#text-recognition) para obtener una lista completa de los idiomas admitidos.

Si es necesario, OCR corrige el giro del texto reconocido devolviendo el desplazamiento de rotación en grados alrededor del eje horizontal de la imagen. OCR también proporciona las coordenadas del marco de cada palabra, tal como se muestra en la siguiente ilustración.

![Que se va a girar una imagen y su texto que se va a leer y escriben](./Images/vision-overview-ocr.png)

Consulte la [documentos de referencia de OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) para obtener más información.

### <a name="image-requirements"></a>Requisitos de imagen

Funciona de la API de reconocimiento óptico de caracteres en las imágenes que cumplan los requisitos siguientes:

* La imagen debe presentarse en formato JPEG, PNG, GIF o BMP.
* El tamaño de la imagen de entrada debe estar entre 50 x 50 y 4200 x 4200 píxeles.
* El texto de la imagen se puede girar cualquier múltiplo de 90 grados, además de un pequeño ángulo de hasta 40 grados.

### <a name="limitations"></a>Limitaciones

En las fotografías en las que predomina el texto, las palabras que se reconocen de forma parcial pueden producir falsos positivos. En algunas fotografías, especialmente las fotografías sin texto, la precisión puede variar según el tipo de imagen.

## <a name="recognize-text-api"></a>Reconocer texto API

> [!NOTE]
> Reconocer Text API está en desuso en favor de la API de lectura. La API de lectura tiene capacidades similares y se actualiza para controlar los archivos de varias páginas, TIFF y PDF.

Reconocer Text API es similar a reconocimiento óptico de caracteres, pero se ejecuta de forma asincrónica y se usa modelos de reconocimiento actualizada. Consulte la [documentos de referencia de API de texto de reconocer](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) para obtener más información.

### <a name="image-requirements"></a>Requisitos de imagen

La API de texto reconocer funciona con imágenes que cumplan los requisitos siguientes:

- La imagen debe presentarse en formato JPEG, PNG o BMP.
- Las dimensiones de la imagen deben estar entre 50 x 50 y 4200 x 4200 píxeles.
- El tamaño del archivo de la imagen debe ser inferior a 4 megabytes (MB).

## <a name="limitations"></a>Limitaciones

La precisión de las operaciones de reconocimiento de texto depende de la calidad de las imágenes. Los siguientes factores pueden provocar una lectura imprecisa:

* Imágenes borrosas.
* Texto escrito a mano o en cursiva.
* Estilos de fuente artísticos.
* Tamaño de texto pequeño.
* Fondos complejos, sombras, brillo sobre el texto o distorsión de la perspectiva.
* Letras mayúsculas grandes o falta en los comienzos de palabras.
* Texto de superíndice, subíndice o tachado.

## <a name="next-steps"></a>Pasos siguientes

Siga el [extraer el texto impreso (OCR)](./quickstarts/csharp-print-text.md) inicio rápido para implementar el reconocimiento de texto en un sencillo C# app.
