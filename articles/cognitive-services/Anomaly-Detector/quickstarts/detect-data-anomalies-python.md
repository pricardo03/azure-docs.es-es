---
title: 'Inicio rápido: Detectar anomalías como un lote mediante la API de REST del Detector de anomalías y Python | Microsoft Docs'
description: Use la API del Detector de anomalías para detectar anomalías en la serie de datos como un lote o en la transmisión de datos.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 60307d51439b4474c8be4f040792c03a6f83b0fd
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58473274"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-python"></a>Inicio rápido: Detectar anomalías en datos de series temporales mediante la API de REST del Detector de anomalías y Python

Use esta guía de inicio rápido para comenzar a usar los dos modos de detección de la API Detector de anomalías para detectar anomalías en datos de series temporales. Esta aplicación de Python envía dos solicitudes de API que contiene datos de series temporales con formato JSON y obtiene las respuestas.

| Solicitud de API                                        | Salida de la aplicación                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Detectar anomalías como un lote                        | La respuesta JSON que contiene el estado de anomalías (y otros datos) para cada punto de datos en los datos de series temporales y las posiciones de las anomalías detectadas. |
| Detectar el estado de anomalías del punto de datos más reciente | La respuesta JSON que contiene el estado de anomalías (y otros datos) para el punto de datos más reciente en los datos de serie temporal.                                                                                                                                         |

 Si bien esta aplicación está escrita en Python, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

## <a name="prerequisites"></a>Requisitos previos

- [Python 2.x o 3.x](https://www.python.org/downloads/)

- El [biblioteca solicitudes](http://docs.python-requests.org) para python

- Puntos de JSON que contiene tiempo serie datos de un archivo. Los datos de ejemplo para este inicio rápido pueden encontrarse en [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]


## <a name="create-a-new-application"></a>Creación de una aplicación

1. En el editor de texto o IDE, cree un nuevo archivo de python. Agregue las siguientes importaciones.

    ```python
    import requests
    import json
    ```

2. Cree variables para su clave de suscripción y el punto de conexión. A continuación se muestran los URI puede usar para la detección de anomalías. Estos se agregarán a su punto de conexión de servicio más adelante para crear la API de direcciones URL de solicitud.

    |Método de detección  |URI  |
    |---------|---------|
    |Detección de lote    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Detección en el último punto de datos     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    ```python
    batch_detection_url = "/anomalydetector/v1.0/timeseries/entire/detect"
    latest_point_detection_url = "/anomalydetector/v1.0/timeseries/last/detect"

    endpoint = "[YOUR_ENDPOINT_URL]"
    subscription_key = "[YOUR_SUBSCRIPTION_KEY]"
    data_location = "[PATH_TO_TIME_SERIES_DATA]"
    ```

3. Leer el archivo de datos JSON, ábralo y utilizando `json.load()`. 

    ```python
    file_handler = open(data_location)
    json_data = json.load(file_handler)
    ```

## <a name="create-a-function-to-send-requests"></a>Cree una función para enviar solicitudes

1. Crear una nueva función llamada `send_request()` que toma las variables que creó anteriormente. Después, lleve a cabo los siguiente pasos.

2. Crear un diccionario para los encabezados de solicitud. Establecer el `Content-Type` a `application/json`y agregue su clave de suscripción para el `Ocp-Apim-Subscription-Key` encabezado.

3. Enviar la solicitud con `requests.post()`. Combinar el punto de conexión y URL de solicitud de dirección URL de detección de anomalías para la versión completa e incluir los encabezados y los datos de la solicitud de json. 

4. Si la solicitud es correcta, devuelva la respuesta.  
    
    ```python
    def send_request(endpoint, url, subscription_key, request_data):
        headers = {'Content-Type': 'application/json', 'Ocp-Apim-Subscription-Key': subscription_key}
        response = requests.post(endpoint+url, data=json.dumps(request_data), headers=headers)
        if response.status_code == 200:
            return json.loads(response.content.decode("utf-8"))
        else:
            print(response.status_code)
            raise Exception(response.text)
    ```

## <a name="detect-anomalies-as-a-batch"></a>Detectar anomalías como un lote

1. Cree un método llamado `detect_batch()` para detectar anomalías a lo largo de los datos como un lote. Llame a la `send_request()` método creada anteriormente con el punto de conexión, dirección url, la clave de suscripción y datos json. 

2. Llamar a `json.dumps()` en el resultado para aplicarle formato e imprimirlo en la consola.

3. Buscar las posiciones de anomalías en el conjunto de datos. La respuesta `isAnomaly` campo contiene un valor booleano sobre si un punto de datos determinado es una anomalía. Recorrer en iteración la lista e imprimir el índice de cualquier `True` valores. Estos valores se corresponden con el índice de puntos de datos anómalos, si se detecta alguno.

```python
def detect_batch(request_data):
    print("Detecting anomalies as a batch")
    result = send_request(endpoint, batch_detection_url, subscription_key, request_data)
    print(json.dumps(result, indent=4))

    # Find and display the positions of anomalies in the data set
    anomalies = result["isAnomaly"]
    print("Anomalies detected in the following data positions:")
    for x in range(len(anomalies)):
        if anomalies[x] == True:
            print (x)
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Detectar el estado de anomalías del punto de datos más reciente

1. Cree un método llamado `detect_latest()` para determinar si el punto de datos más reciente de la serie de tiempo es una anomalía. Llame a la `send_request()` método anterior con el punto de conexión, dirección url, la clave de suscripción y datos json. 

2. Llamar a `json.dumps()` en el resultado para aplicarle formato e imprimirlo en la consola.

```python
def detect_latest(request_data):
    print("Determining if latest data point is an anomaly")
    # send the request, and print the JSON result
    result = send_request(endpoint, latest_point_detection_url, subscription_key, request_data)
    print(json.dumps(result, indent=4))
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Cargar datos de series temporales y enviar la solicitud

1. Cargar los datos de series de tiempo JSON abriendo un controlador de archivo y utilizando `json.load()` en él. A continuación, llame a las anomalías métodos de detección que creó anteriormente.
    
    ```python
    file_handler = open (data_location)
    json_data = json.load(file_handler)
    
    detect_batch(json_data)
    detect_latest(json_data)
    ```

### <a name="example-response"></a>Respuesta de ejemplo

Se devuelve una respuesta correcta en formato JSON. Haga clic en los siguientes vínculos para ver la respuesta JSON en GitHub:
* [Respuesta de detección por lotes de ejemplo](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Respuesta de detección de punto más reciente de ejemplo](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)