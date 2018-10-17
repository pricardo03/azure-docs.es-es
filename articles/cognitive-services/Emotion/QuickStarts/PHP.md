---
title: 'Guía de inicio rápido: Reconocimiento de emociones en las caras de una imagen con Emotion API en PHP'
titlesuffix: Azure Cognitive Services
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Emotion API con PHP.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: c3dffa3c42df4a30b634417b551dd0e8af04145b
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239513"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Guía de inicio rápido: Creación de una aplicación para reconocer emociones en las caras de una imagen

> [!IMPORTANT]
> Emotion API dejará de usarse el 15 de febrero de 2019. La funcionalidad de reconocimiento de emociones está ahora disponible con carácter general como parte de [Face API](https://docs.microsoft.com/azure/cognitive-services/face/). 

Este artículo proporciona información y ejemplos de código para ayudarle a empezar a usar rápidamente PHP y el [método de reconocimiento de Emotion API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) para reconocer las emociones expresadas por una o varias personas en una imagen.

## <a name="prerequisite"></a>Requisito previo
* Obtenga su clave de suscripción gratuita [aquí](https://azure.microsoft.com/try/cognitive-services/).

## <a name="recognize-emotions-php-example-request"></a>Ejemplo de solicitud de reconocimiento de emociones en PHP

Cambie la URL REST para usar la ubicación donde obtuvo sus claves de suscripción, cambie el cuerpo a la dirección URL de la imagen que desea probar y reemplace el valor "Ocp-Apim-Subscription-Key" por su clave de suscripción válida.

```php
<?php
// This sample uses the Apache HTTP client from HTTP Components (http://hc.apache.org/httpcomponents-client-ga/)
require_once 'HTTP/Request2.php';

// NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
//   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the
//   URL below with "westcentralus".
$request = new Http_Request2('https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',

    // NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key' => '13hc77781f7e4b19b5fcdd72a8df7156',
);

$request->setHeader($headers);

$parameters = array(
    // Request parameters
);

$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body
$request->setBody('{"url": "http://www.example.com/images/image.jpg"}');

try
{
    $response = $request->send();
    echo $response->getBody();
}
catch (HttpException $ex)
{
    echo $ex;
}

?>
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
