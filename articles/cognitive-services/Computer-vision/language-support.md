---
title: 'Compatibilidad con idiomas: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Una lista de los idiomas naturales admitidos por características de Computer Vision.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.openlocfilehash: 516d21bc69bbc20f924a3bdf39eda7245fc08a28
ms.sourcegitcommit: aa042d4341054f437f3190da7c8a718729eb675e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68882962"
---
# <a name="language-support-for-computer-vision"></a>Compatibilidad con idiomas para Computer Vision

Algunas características de Computer Vision admiten varios idiomas; las características que no se mencionan aquí solo admiten inglés.

## <a name="text-recognition"></a>Reconocimiento de texto

Computer Vision puede reconocer texto en varios idiomas. Específicamente, la API de [OCR](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) admite varios idiomas, mientras que la API de [Leer](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) y [Reconocer texto](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/587f2c6a154055056008f200) solo son admiten el inglés. Consulte [Reconocimiento de texto manuscrito e impreso](concept-recognizing-text.md) para obtener más información sobre esta funcionalidad y las ventajas de cada API.

OCR detecta automáticamente el idioma del material de entrada, por lo que no es necesario especificar un código de idioma en la llamada a la API. Sin embargo, siempre se devuelven los códigos de idioma como el valor del nodo `"language"` en la respuesta JSON.

|Idioma| Código de idioma | API de OCR |
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

Algunas acciones de la API [analizar: imagen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) pueden devolver resultados en otros idiomas, especificados con el parámetro de consulta `language`. Otras acciones devuelven resultados en inglés, independientemente del idioma que se especifique, y otros generan una excepción para los idiomas no admitidos. Las acciones se especifican con los parámetros de consulta `visualFeatures` y `details`; vea la [información general](home.md) para obtener una lista de todas las acciones que puede hacer con el análisis de imágenes.

|Idioma | Código de idioma | Categorías | Etiquetas | DESCRIPCIÓN | Adultos | Marcas | Color | Caras | ImageType | Objetos | Celebridades | Puntos de referencia |
|:---|:---:|:----:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Chino | `zh`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|English | `en`   | ✔ | ✔| ✔|✔|✔|✔|✔|✔|✔|✔|✔|
|Japonés | `ja`   | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Portugués | `pt` | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|
|Español | `es`    | ✔ | ✔| ✔|-|-|-|-|-|❌|✔|✔|

## <a name="next-steps"></a>Pasos siguientes

Conozca el uso de las características de Computer Vision mencionadas en esta guía.

* [Análisis de imágenes locales (REST)](./quickstarts/csharp-analyze.md)
* [Extracción de texto impreso (REST)](./quickstarts/csharp-print-text.md)