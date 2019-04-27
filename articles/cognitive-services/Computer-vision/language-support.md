---
title: Compatibilidad con Language - Computer Vision
titleSuffix: Azure Cognitive Services
description: Una lista de los lenguajes naturales compatibles con las características de Computer Vision.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: article
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: 1a70d1b2ea504d0ccfba925810a2d19d0c7583cc
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60759612"
---
# <a name="language-support-for-computer-vision"></a>Compatibilidad con lenguajes para Computer Vision

Algunas características de Computer Vision admiten varios idiomas; las características que no se mencionan aquí solo admiten a inglés.

## <a name="text-recognition"></a>Reconocimiento de texto

Computer Vision puede reconocer texto en varios idiomas. En concreto, el [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) API admite una variedad de lenguajes, mientras que el [lectura](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) API y [reconocer texto](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) API solo admite inglés. Consulte [reconocer texto escrito a mano e impreso](concept-recognizing-text.md) para obtener más información sobre esta funcionalidad y las ventajas de cada API.

OCR detecta automáticamente el idioma del material de entrada, por lo que es necesario especificar un código de idioma en la llamada de API. Sin embargo, siempre se devuelven los códigos de idioma como el valor de la `"language"` nodo en la respuesta JSON.

|Idioma| Código de idioma | API DE RECONOCIMIENTO ÓPTICO DE CARACTERES |
|:-----|:----:|:-----:|
|Árabe | `ar`|✔ |
|Chino (simplificado) | `zh-Hans`|✔ |
|Chino (tradicional) | `zh-Hant`|✔ |
|Checo | `cs` |✔ |
|Danés | `da` |✔ |
|Neerlandés | `nl` |✔ |
|English | `en` |✔ |
|Finés | `fi` |✔ |
|Francés | `fr` |✔ |
|Alemán | `de` |✔ |
|Griego | `el` |✔ |
|Húngaro | `hu` |✔ |
|Italiano | `it` |✔ |
|Japonés | `ja` |✔ |
|Coreano | `ko` |✔ |
|Noruego | `nb` |✔ |
|Polaco | `pl` |✔ |
|Portugués | `pt` |✔ |
|Rumano | `ro` |✔ |
|Ruso | `ru` |✔ |
|Serbio (cirílico) | `sr-Cyrl` |✔ |
|Serbio (latino) | `sr-Latn` |✔ |
|Eslovaco | `sk` |✔ |
|Español | `es` |✔ |
|Sueco | `sw` |✔ |
|Turco | `tr` |✔ |

## <a name="image-analysis"></a>Análisis de imágenes

Algunas acciones de la [analizar: imagen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) API puede devolver resultados en otros lenguajes, especificados con el `language` parámetro de consulta. Otras acciones, devuelven los resultados en inglés, independientemente de qué idioma se especifica, y otras iniciarán una excepción para lenguajes no admitidos. Las acciones se especifican con el `visualFeatures` y `details` parámetros de consulta; vea la [Overview](home.md) para obtener una lista de todas las acciones que puede hacer con análisis de imágenes.

|Idioma | Código de idioma | Categorías | Etiquetas | DESCRIPCIÓN | Adultos | Marcas | Color | Caras | ImageType | Objetos | Celebridades | Puntos de referencia |
|:---|:---:|:----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Chino | `zh`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|English | `en`   | ✔ | ✔| ✔|✔|✔|✔|✔|✔|✔|✔|✔|
|Japonés | `ja`   | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Portugués | `pt` | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Español | `es`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|

## <a name="next-steps"></a>Pasos siguientes

Introducción al uso de las características de Computer Vision mencionadas en esta guía.

* [Analizar una imagen local (REST)](./quickstarts/csharp-analyze.md)
* [Extraer el texto impreso (REST)](./quickstarts/csharp-print-text.md)