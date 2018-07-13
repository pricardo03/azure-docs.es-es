---
title: 'Uso de Anomaly Finder API con PHP: Microsoft Cognitive Services | Microsoft Docs'
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Anomaly Finder con PHP en Cognitive Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: f81c99b77f931b5b259633fa8fcd0bf3e358e281
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381310"
---
# <a name="use-the-anomaly-finder-api-with-php"></a>Uso de Anomaly Finder API con PHP

En este artículo se ofrece información y ejemplos de código para ayudarle a comenzar a usar rápidamente Anomaly Finder API con PHP para realizar la tarea de obtener resultados de anomalías en los datos de serie temporal.

## <a name="prerequisites"></a>requisitos previos

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-php"></a>Obtener puntos de anomalías con Anomaly Finder API mediante PHP
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Ejemplo de datos de series temporales
El ejemplo de los datos de serie temporal es el siguiente.
[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-php-example"></a>Ejemplo de análisis de datos y obtención de puntos de anomalías en PHP
1. Reemplace el valor de `[YOUR_SUBSCRIPTION_KEY]` por su clave de suscripción válida.
2. Reemplace `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` con el ejemplo o por sus propios puntos de datos.
3. Ejecute y compruebe la respuesta.

```PHP
<?php
# This sample uses the Apache HTTP client from HTTP components (http://hc.apache.org/httpcomponents-client-ga/)
require_once 'HTTP/Request2.php';

$request = new HTTP_Request2('https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection');
$url = $request->getUrl();

$requestData = '[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]';

$headers = array(
    # Request headers
    'Content-Type' => 'application/json',
    # NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key' => '[YOUR_SUBSCRIPTION_KEY]',
);

$request->setHeader($headers);

$request->setMethod(HTTP_Request2::METHOD_POST);

# Request body
$request->setBody($requestData);

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

### <a name="example-response"></a>Respuesta de ejemplo

Se devuelve una respuesta correcta en JSON. A continuación se muestra una respuesta de ejemplo.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
