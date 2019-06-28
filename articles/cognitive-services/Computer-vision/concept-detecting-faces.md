---
title: 'Detección de caras: Computer Vision'
titleSuffix: Azure Cognitive Services
description: Aprenda conceptos relacionados con la característica de detección de caras de la API Computer Vision.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 04/17/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 699192aba87bb009d7dbddddcc9579883bb71db9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60368142"
---
# <a name="face-detection-with-computer-vision"></a>Detección de caras con Computer Vision

Computer Vision puede detectar caras humanas en una imagen y generar la edad, el sexo y el rectángulo de cada rostro detectado. 

> [!NOTE]
> Esta característica también está disponible en el servicio Azure [Face](/azure/cognitive-services/face/). Utilice esta alternativa para realizar un análisis más detallado de los rostros que incluya la identificación de la cara y la detección de la pose. 

## <a name="face-detection-examples"></a>Ejemplos de detección de caras

En el ejemplo siguiente, se muestra la respuesta JSON devuelta por Computer Vision en una imagen con un solo rostro humano.

![Vision Analyze Woman Roof Face](./Images/woman_roof_face.png)

```json
{
    "faces": [
        {
            "age": 23,
            "gender": "Female",
            "faceRectangle": {
                "top": 45,
                "left": 194,
                "width": 44,
                "height": 44
            }
        }
    ],
    "requestId": "8439ba87-de65-441b-a0f1-c85913157ecd",
    "metadata": {
        "height": 200,
        "width": 300,
        "format": "Png"
    }
}
```

En el ejemplo siguiente, se muestra la respuesta JSON devuelta en una imagen con varios rostros humanos.

![Vision Analyze Family Photo Face](./Images/family_photo_face.png)

```json
{
    "faces": [
        {
            "age": 11,
            "gender": "Male",
            "faceRectangle": {
                "top": 62,
                "left": 22,
                "width": 45,
                "height": 45
            }
        },
        {
            "age": 11,
            "gender": "Female",
            "faceRectangle": {
                "top": 127,
                "left": 240,
                "width": 42,
                "height": 42
            }
        },
        {
            "age": 37,
            "gender": "Female",
            "faceRectangle": {
                "top": 55,
                "left": 200,
                "width": 41,
                "height": 41
            }
        },
        {
            "age": 41,
            "gender": "Male",
            "faceRectangle": {
                "top": 45,
                "left": 103,
                "width": 39,
                "height": 39
            }
        }
    ],
    "requestId": "3a383cbe-1a05-4104-9ce7-1b5cf352b239",
    "metadata": {
        "height": 230,
        "width": 300,
        "format": "Png"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el uso de la característica de detección de rostros, consulte la documentación de referencia de [Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) (Análisis de imágenes).
