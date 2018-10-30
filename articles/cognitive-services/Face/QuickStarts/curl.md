---
title: 'Guía de inicio rápido: Detección de caras en una imagen mediante la API REST y cURL'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido se detectan caras de una imagen mediante Face API con cURL.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: quickstart
ms.date: 05/10/2018
ms.author: pafarley
ms.openlocfilehash: ab403ec6a9db4d1a0dc03074044eeb424e4ba875
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953355"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-rest-api-and-curl"></a>Guía de inicio rápido: Detección de caras en una imagen mediante la API REST y cURL

En esta guía de inicio rápido, se detectan caras de una imagen mediante Face API.

## <a name="prerequisites"></a>Requisitos previos

Necesita una clave de suscripción para ejecutar el ejemplo. Puede obtener las claves de la suscripción de evaluación gratuita en la página de [Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api).

## <a name="detect-faces-in-an-image"></a>Detectar caras en una imagen

Use el método [Face - Detect](https://westcentralus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) para detectar los rostros en una imagen y devolver atributos de la cara, como:

* Id. de rostro: identificador único que se usa en varios escenarios de Face API.
* Rectángulo facial: las indicaciones de ubicación (izquierda, parte superior, ancho y alto) del rostro en la imagen.
* Puntos de referencia: matriz de 27 puntos faciales de referencia de las posiciones importantes de los componentes del rostro.
* Atributos faciales como la edad, el sexo, la intensidad de la sonrisa, la postura de la cabeza y el vello facial.

Para ejecutar el ejemplo, siga estos pasos:

1. Abra el símbolo del sistema.
2. Reemplace `<Subscription Key>` por una clave de suscripción válida.
3. Si es necesario, cambie la dirección URL (`https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect`) para usar la ubicación donde obtuvo las claves de suscripción.
4. Además, puede cambiar la imagen (`"{\"url\":...`) que se va a analizar.
5. Pegue el código en la ventana de comandos.
6. Ejecute el comando.

### <a name="face---detect-request"></a>Solicitud Face - Detect

> [!NOTE]
> Debe utilizar la misma ubicación en la llamada de REST que utilizó para obtener las claves de la suscripción. Por ejemplo, si obtuvo sus claves de suscripción de westus, reemplace "westcentralus" en la siguiente dirección URL por "westus".

```shell
curl -H "Ocp-Apim-Subscription-Key: <Subscription Key>" "https://westcentralus.api.cognitive.microsoft.com/face/v1.0/detect?returnFaceId=true&returnFaceLandmarks=false&returnFaceAttributes=age,gender,headPose,smile,facialHair,glasses,emotion,hair,makeup,occlusion,accessories,blur,exposure,noise" -H "Content-Type: application/json" --data-ascii "{\"url\":\"https://upload.wikimedia.org/wikipedia/commons/c/c3/RH_Louise_Lillian_Gish.jpg\"}"
```

### <a name="face---detect-response"></a>Respuesta de Face - Detect

Se devuelve una respuesta correcta en JSON.

```json
[
  {
    "faceId": "49d55c17-e018-4a42-ba7b-8cbbdfae7c6f",
    "faceRectangle": {
      "top": 131,
      "left": 177,
      "width": 162,
      "height": 162
    },
    "faceAttributes": {
      "smile": 0,
      "headPose": {
        "pitch": 0,
        "roll": 0.1,
        "yaw": -32.9
      },
      "gender": "female",
      "age": 22.9,
      "facialHair": {
        "moustache": 0,
        "beard": 0,
        "sideburns": 0
      },
      "glasses": "NoGlasses",
      "emotion": {
        "anger": 0,
        "contempt": 0,
        "disgust": 0,
        "fear": 0,
        "happiness": 0,
        "neutral": 0.986,
        "sadness": 0.009,
        "surprise": 0.005
      },
      "blur": {
        "blurLevel": "low",
        "value": 0.06
      },
      "exposure": {
        "exposureLevel": "goodExposure",
        "value": 0.67
      },
      "noise": {
        "noiseLevel": "low",
        "value": 0
      },
      "makeup": {
        "eyeMakeup": true,
        "lipMakeup": true
      },
      "accessories": [],
      "occlusion": {
        "foreheadOccluded": false,
        "eyeOccluded": false,
        "mouthOccluded": false
      },
      "hair": {
        "bald": 0,
        "invisible": false,
        "hairColor": [
          {
            "color": "brown",
            "confidence": 1
          },
          {
            "color": "black",
            "confidence": 0.87
          },
          {
            "color": "other",
            "confidence": 0.51
          },
          {
            "color": "blond",
            "confidence": 0.08
          },
          {
            "color": "red",
            "confidence": 0.08
          },
          {
            "color": "gray",
            "confidence": 0.02
          }
        ]
      }
    }
  }
]
```

## <a name="next-steps"></a>Pasos siguientes

Explore las versiones de Face API que se utilizan para detectar rostros humanos en una imagen, delimitarlos con rectángulos y devolver atributos como la edad y el sexo.

> [!div class="nextstepaction"]
> [Versiones de Face API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
