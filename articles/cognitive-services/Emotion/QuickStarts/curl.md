---
title: 'Inicio rápido: Reconocimiento de emociones en las caras de una imagen (Emotion API, cURL)'
titlesuffix: Azure Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Emotion API con cURL.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: bd157bfeff8ceaa18a75f15895b7304385c5e522
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56232044"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Inicio rápido: Creación de una aplicación para reconocer emociones en las caras de una imagen.

> [!IMPORTANT]
> Emotion API dejará de usarse el 15 de febrero de 2019. La funcionalidad de reconocimiento de emociones está ahora disponible con carácter general como parte de [Face API](https://docs.microsoft.com/azure/cognitive-services/face/).

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
