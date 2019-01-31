---
title: 'Uso de Anomaly Finder API con cURL: Microsoft Cognitive Services | Microsoft Docs'
description: Obtenga información que le ayuden a empezar a usar rápidamente cURL y Anomaly Finder API en Cognitive Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: e055bff430c0d090b4d527a79a22922da2eb90d0
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207062"
---
# <a name="use-the-anomaly-finder-api-with-curl"></a>Uso de Anomaly Finder API con cURL

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

En este artículo se ofrece información y ejemplos de código para ayudarle a comenzar a usar rápidamente Anomaly Finder API con cURL para realizar la tarea de obtener resultados de anomalías en los datos de serie temporal.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-the-anomaly-finder-api-using-curl"></a>Obtener puntos de anomalías con Anomaly Finder API mediante cURL 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Ejemplo de datos de series temporales

Este es el ejemplo de los puntos de datos de series temporales.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-curl-example"></a>Ejemplo de análisis de datos y obtención de puntos de anomalías en cURL

Los pasos necesarios para usar el ejemplo son los siguientes.

1. Reemplace el valor de `[YOUR_SUBSCRIPTION_KEY]` por su clave de suscripción válida.
2. Reemplace `[YOUR_REGION]` para usar la ubicación donde obtuvo las claves de suscripción.
3. Reemplace `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` con el ejemplo o por sus propios puntos de datos.
4. Ejecute y compruebe la respuesta.

```cURL

curl -v -X POST "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: [YOUR_SUBSCRIPTION_KEY]"
--data-ascii "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]" 

```

### <a name="example-response"></a>Respuesta de ejemplo
Se devuelve una respuesta correcta en JSON. A continuación se muestra una respuesta de ejemplo: [!INCLUDE [Response](../includes/response.md)].

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
