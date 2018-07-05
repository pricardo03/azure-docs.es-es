---
title: Inicio rápido en Emotion API en cURL | Microsoft Docs
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Emotion API con cURL en Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: a7ca2cac718797462bb4dc889b3f1361b252435e
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37021105"
---
# <a name="emotion-api-curl-quick-start"></a>Inicio rápido en Emotion API en cURL

> [!IMPORTANT]
> La versión preliminar de Video API terminará el 30 de octubre de 2017. Pruebe la versión preliminar de la nueva [Video Indexer API con una extracción sencilla de detalles en](https://azure.microsoft.com/services/cognitive-services/video-indexer/) vídeos y mejore las experiencias de detección de contenido, como los resultados de la búsqueda, gracias al reconocimiento de texto oral, caras, caracteres y emociones. [Más información](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Este artículo proporciona información y ejemplos de código para ayudarle a empezar a usar rápidamente el [método de reconocimiento de Emotion API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) con cURL para reconocer las emociones expresadas por una o varias personas en una imagen. 

## <a name="prerequisite"></a>Requisito previo
* Obtenga su clave de suscripción gratuita [aquí](https://azure.microsoft.com/try/cognitive-services/).

## <a name="recognize-emotions-curl-example-request"></a>Ejemplo de solicitud de reconocimiento de emociones en cURL

> [!NOTE]
> Debe utilizar la misma ubicación en la llamada de REST que utilizó para obtener las claves de la suscripción. Por ejemplo, si obtuvo sus claves de suscripción de westcentralus, reemplace "westus" en la siguiente URL por "westcentralus".

```json
@ECHO OFF

curl -v -X POST "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}" 
```

## <a name="recognize-emotions-sample-response"></a>Respuesta de ejemplo de reconocimiento de emociones
Una llamada correcta devuelve una matriz de entradas de cara y sus puntuaciones de emoción asociadas, clasificadas por tamaño del rectángulo de la cara en orden descendente. Una respuesta vacía indica que no se ha detectado ninguna cara. Una entrada de emoción contiene los siguientes campos:
* faceRectangle: ubicación del rectángulo de la cara en la imagen.
* scores: puntuaciones de emociones para cada cara de la imagen. 

```json
application/json 
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]

