---
title: 'Inicio rápido: Reconocimiento de emociones en las caras de una imagen (Emotion API, Ruby)'
titlesuffix: Azure Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Emotion API con Ruby.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: b1bddbffa3a58158b3a87c4681557ff8f10a6334
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56232680"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Inicio rápido: Creación de una aplicación para reconocer emociones en las caras de una imagen.

> [!IMPORTANT]
> Emotion API dejará de usarse el 15 de febrero de 2019. La funcionalidad de reconocimiento de emociones está ahora disponible con carácter general como parte de [Face API](https://docs.microsoft.com/azure/cognitive-services/face/). 

Este artículo proporciona información y ejemplos de código para ayudarle a empezar a usar rápidamente el [método de reconocimiento de Emotion API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) con Ruby para reconocer las emociones expresadas por una o varias personas en una imagen.

## <a name="prerequisite"></a>Requisito previo
* Obtenga su clave de suscripción gratuita [aquí](https://azure.microsoft.com/try/cognitive-services/).

## <a name="recognize-emotions-ruby-example-request"></a>Ejemplo de solicitud de reconocimiento de emociones en Ruby

Cambie la URL REST para usar la ubicación donde obtuvo sus claves de suscripción, reemplace el valor "Ocp-Apim-Subscription-Key" por su clave de suscripción válida y agregue una dirección URL a una fotografía a la variable `body`.

```ruby
require 'net/http'

# NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
#   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the
#   URL below with "westcentralus".
uri = URI('https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize')
uri.query = URI.encode_www_form({
})

request = Net::HTTP::Post.new(uri.request_uri)
# Request headers
request['Content-Type'] = 'application/json'
# NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
request['Ocp-Apim-Subscription-Key'] = '13hc77781f7e4b19b5fcdd72a8df7156'
# Request body
request.body = "{\"url\":\"http://example.com/1.jpg\"}"

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

puts response.body

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
