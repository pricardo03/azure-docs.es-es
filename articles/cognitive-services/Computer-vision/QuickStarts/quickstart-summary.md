---
title: Resumen de la guía de inicio rápido de Computer Vision
titleSuffix: Azure Cognitive Services
description: En estas guías de inicio rápido, analizará una imagen, creará una miniatura y extraerá texto impreso y manuscrito mediante Computer Vision API.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 15b3d634599a88468b24d64b6d41c2d6b1ff59a1
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55865130"
---
# <a name="quickstart-summary"></a>Inicio rápido: Resumen

Estas guías de inicio rápido ofrecen información y ejemplos de código para ayudarle a iniciarse rápidamente en el uso del servicio Computer Vision.

En los ejemplos se realizan llamadas HTTP directas a Computer Vision API. Consulte la sección *Guías de inicio rápido del SDK* para ver ejemplos que usan las bibliotecas cliente de Computer Vision, que proporcionan prácticos métodos que encapsulan las llamadas HTTP.

Para experimentar rápidamente con las versiones de Computer Vision API, pruebe la [consola de pruebas de Open API](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

Puede usar el servicio Computer Vision para realizar las siguientes tareas:

* Analizar una imagen remota
* Analizar una imagen local
* Detectar personajes y sitios emblemáticos (modelos de dominio)
* Generación inteligente de una miniatura
* Detectar y extraer texto impreso (OCR) de una imagen
* Detectar y extraer texto escrito a mano de una imagen

El código de cada ejemplo es similar. Sin embargo, resaltan distintas características de Computer Vision, junto con las distintas técnicas para intercambiar datos con el servicio, como:

* _Generate a thumbnail_ devuelve una imagen como una matriz de bytes en el cuerpo de la respuesta.
* _Analyze a local image_ requiere que se incluya la imagen en la solicitud como una matriz de bytes.
* _Extract handwritten text_ requiere dos llamadas para recuperar el texto.

## <a name="summary"></a>Resumen

| Guía de inicio rápido               | Parámetros de solicitud                          | Response          |
| ------------------------ | ------------------------------------------- | ----------------  |
| Analizar una imagen remota   | visualFeatures=Categories,Description,Color | Cadena JSON.       |
| Analizar una imagen local    | data=image_data (byte array)                | Cadena JSON.       |
| Detectar personajes       | model=celebrities                           | Cadena JSON.       |
| Generar una miniatura     | width=200&height=150&smartCropping=true     | byte array        |
| Extraer texto impreso     | language=unk&detectOrientation=true         | Cadena JSON.       |
| Extraer texto manuscrito | handwriting=true                            | URL, cadena JSON  |

## <a name="next-steps"></a>Pasos siguientes

Explore las versiones de Computer Vision API que se usan para analizar una imagen, detectar personajes y sitios emblemáticos, crear una miniatura y extraer texto impreso y escrito a mano.

> [!div class="nextstepaction"]
> [Exploración de las versiones de Computer Vision API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
