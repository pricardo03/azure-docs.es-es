---
title: 'Uso de Anomaly Finder API con Python: Microsoft Cognitive Services | Microsoft Docs'
description: Obtenga información y ejemplos de código que le ayuden a empezar a usar rápidamente Anomaly Finder con Python en Cognitive Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: c14916b0644edab613b298d6e71f8bbb9a6bb804
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35381298"
---
# <a name="use-the-anomaly-finder-api-with-python"></a>Uso de Anomaly Finder API con Python

En este artículo se ofrece información y ejemplos de código para ayudarle a comenzar a usar rápidamente Anomaly Finder API con Python para realizar la tarea de obtener resultados de anomalías en los datos de serie temporal.

## <a name="prerequisites"></a>requisitos previos

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-python"></a>Obtener puntos de anomalías con Anomaly Finder API mediante Python 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Ejemplo de datos de series temporales

El ejemplo de los puntos de datos de serie temporal es el siguiente.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-python-example"></a>Ejemplo de análisis de datos y obtención de puntos de anomalías en Python

Asegúrese de haber instalado python3 y luego cree un archivo ejecutable de Python denominado detect.py. En detect.py, debe incluir el código siguiente. Antes de ejecutar el código, recuerde reemplazar el valor `[YOUR_SUBSCRIPTION_KEY]` con la clave de una suscripción válida.
Reemplace `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` con los puntos de datos.

```python
import requests
import json


def detect(url, subscription_key, request_data):
    headers = {'Content-Type': 'application/json', 'Ocp-Apim-Subscription-Key': subscription_key}
    response = requests.post(url, data=json.dumps(request_data), headers=headers)
    if response.status_code == 200:
        return json.loads(response.content.decode("utf-8"))
    else:
        print(response.status_code)
        raise Exception(response.text)


sample_data = "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]"
endpont = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
subscription_key = "[YOUR_SUBSCRIPTION_KEY]"

result = detect(endpont, subscription_key, sample_data)
print(result)

```

### <a name="example-response"></a>Respuesta de ejemplo

Se devuelve una respuesta correcta en JSON. A continuación se muestra una respuesta de ejemplo.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación Python](../tutorials/python-tutorial.md)
