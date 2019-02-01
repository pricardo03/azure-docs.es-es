---
title: 'Uso de Anomaly Finder API con Ruby: Microsoft Cognitive Services | Microsoft Docs'
description: Obtendrá información y ejemplos de código que le ayudarán a empezar a usar Ruby y Anomaly Finder API en Cognitive Services rápidamente.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: a229011617239b829ce2622a0482fd9b4908aca6
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55224214"
---
# <a name="use-the-anomaly-finder-api-with-ruby"></a>Uso de Anomaly Finder API con Ruby

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

En este artículo se ofrece información y ejemplos de código para ayudarle a comenzar a usar rápidamente Anomaly Finder API con Ruby para realizar la tarea de obtener resultados de detección de anomalías en los datos de serie temporal.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-ruby"></a>Obtener puntos de anomalías con Anomaly Finder API mediante Ruby 
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Ejemplo de datos de series temporales
Este es el ejemplo de los puntos de datos de series temporales:

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-ruby-example"></a>Ejemplo de análisis de datos y obtención de puntos de anomalías en Ruby

Los pasos necesarios para usar el ejemplo son los siguientes.

1. Ejecute "gem install rest-client" para instalar [rest-client](https://github.com/rest-client/rest-client).
2. Guarde el siguiente código como un archivo .rb.
3. Reemplace el valor de `[YOUR_SUBSCRIPTION_KEY]` por su clave de suscripción válida.
4. Reemplace `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` con los puntos de datos de ejemplo o los suyos propios.
5. Ejecute y compruebe la respuesta.

```ruby
# https://github.com/rest-client/rest-client
require 'rest_client'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscription_key = '[YOUR_SUBSCRIPTION_KEY]';

endpoint = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

# Replace the request data with your real data.
requestData = '[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]';

header = {
    :content_type => 'application/json',
    :'Ocp-Apim-Subscription-Key' => subscription_key
}

response = RestClient::Request.execute(
    :url => endpoint,
    :method => :post,
    :verify_ssl => true,
    :payload => requestData,
    :header => header)

# You will see the response with anomaly results
puts response.body
```

### <a name="example-response"></a>Respuesta de ejemplo

Se devuelve una respuesta correcta en JSON. A continuación se muestra una respuesta de ejemplo.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
