---
title: 'Inicio rápido: Detectar anomalías en datos de series temporales mediante la API de REST del Detector de anomalías y Java | Microsoft Docs'
description: Use la API del Detector de anomalías para detectar anomalías en la serie de datos como un lote o en la transmisión de datos.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 1c8ce91a0fd8805b307e1e21bc08f9050b8a47d4
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59547046"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-java"></a>Inicio rápido: Detectar anomalías en datos de series temporales mediante la API de REST del Detector de anomalías y Java

Use esta guía de inicio rápido para comenzar a usar los dos modos de detección de la API Detector de anomalías para detectar anomalías en datos de series temporales. Esta aplicación de Java envía dos solicitudes de API que contiene datos de series temporales con formato JSON y obtiene las respuestas.

| Solicitud de API                                        | Salida de la aplicación                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Detectar anomalías como un lote                        | La respuesta JSON que contiene el estado de anomalías (y otros datos) para cada punto de datos en los datos de series temporales y las posiciones de las anomalías detectadas. |
| Detectar el estado de anomalías del punto de datos más reciente | La respuesta JSON que contiene el estado de anomalías (y otros datos) para el punto de datos más reciente en los datos de serie temporal.                                                                                                                                         |

 Si bien esta aplicación está escrita en Java, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

## <a name="prerequisites"></a>Requisitos previos

- El [Java&trade; desarrollo Kit(JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) o una versión posterior.

- Estas bibliotecas de importación del repositorio de Maven
    - [JSON en Java](https://mvnrepository.com/artifact/org.json/json) paquete
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) paquete

- Puntos de JSON que contiene tiempo serie datos de un archivo. Los datos de ejemplo para este inicio rápido pueden encontrarse en [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

## <a name="create-a-new-application"></a>Creación de una aplicación

1. Cree un proyecto de Java en su IDE o editor favorito e importe las bibliotecas siguientes.

    ```java
    import org.apache.http.HttpEntity;
    import org.apache.http.client.methods.CloseableHttpResponse;
    import org.apache.http.client.methods.HttpPost;
    import org.apache.http.entity.StringEntity;
    import org.apache.http.impl.client.CloseableHttpClient;
    import org.apache.http.impl.client.HttpClients;
    import org.apache.http.util.EntityUtils;
    import org.json.JSONArray;
    import org.json.JSONObject;
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Paths;
    ```

2. Cree variables para su clave de suscripción y el punto de conexión. A continuación se muestran los URI puede usar para la detección de anomalías. Estos se agregarán a su punto de conexión de servicio más adelante para crear la API de direcciones URL de solicitud.

    |Método de detección  |URI  |
    |---------|---------|
    |Detección de lote    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Detección en el último punto de datos     | `/anomalydetector/v1.0/timeseries/last/detect`        |

    ```java
    // Replace the subscriptionKey string value with your valid subscription key.
    static final String subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";
    //replace the endpoint URL with the correct one for your subscription. Your endpoint can be found in the Azure portal. 
    //For example: https://westus2.api.cognitive.microsoft.com
    static final String endpoint = "[YOUR_ENDPOINT_URL]";
    // Replace the dataPath string with a path to the JSON formatted time series data.
    static final String dataPath = "[PATH_TO_TIME_SERIES_DATA]";
    static final String latestPointDetectionUrl = "/anomalydetector/v1.0/timeseries/last/detect";
    static final String batchDetectionUrl = "/anomalydetector/v1.0/timeseries/entire/detect";
    ```

3. Leer el archivo de datos JSON

    ```java
    String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "utf-8");
    ```

## <a name="create-a-function-to-send-requests"></a>Cree una función para enviar solicitudes

1. Crear una nueva función llamada `sendRequest()` que toma las variables que creó anteriormente. Después, lleve a cabo los siguiente pasos.

2. Crear un `CloseableHttpClient` objeto que puede enviar solicitudes a la API. Enviar la solicitud a un `HttpPost` objeto request combinando el punto de conexión y una dirección URL de Detector de anomalías.

3. Usar la solicitud `setHeader()` función para establecer el `Content-Type` encabezado a `application/json`y agregue su clave de suscripción para el `Ocp-Apim-Subscription-Key` encabezado.

4. Usar la solicitud `setEntity()` función a los datos se envíen.

5. Use el cliente `execute()` función para enviar la solicitud y guárdelo en un `CloseableHttpResponse` objeto.

6. Crear un `HttpEntity` objeto para almacenar el contenido de la respuesta. Obtener el contenido con `getEntity()`. Si la respuesta no está vacía, la devuelve.

```java
static String sendRequest(String apiAddress, String endpoint, String subscriptionKey, String requestData) {
    try (CloseableHttpClient client = HttpClients.createDefault()) {
        HttpPost request = new HttpPost(endpoint + apiAddress);
        // Request headers.
        request.setHeader("Content-Type", "application/json");
        request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);
        request.setEntity(new StringEntity(requestData));
        try (CloseableHttpResponse response = client.execute(request)) {
            HttpEntity respEntity = response.getEntity();
            if (respEntity != null) {
                return EntityUtils.toString(respEntity, "utf-8");
            }
        } catch (Exception respEx) {
            respEx.printStackTrace();
        }
    } catch (IOException ex) {
        System.err.println("Exception on Anomaly Detector: " + ex.getMessage());
        ex.printStackTrace();
    }
    return null;
}
```

## <a name="detect-anomalies-as-a-batch"></a>Detectar anomalías como un lote

1. Cree un método llamado `detectAnomaliesBatch()` para detectar anomalías a lo largo de los datos como un lote. Llame a la `sendRequest()` método creada anteriormente con el punto de conexión, dirección url, la clave de suscripción y datos json. Obtener el resultado e imprimirlo en la consola.

2. Si la respuesta contiene `code` campo, el código de error y el mensaje de error de impresión.

3. En caso contrario, busque las posiciones de anomalías en el conjunto de datos. La respuesta `isAnomaly` campo contiene un valor booleano sobre si un punto de datos determinado es una anomalía. Obtener la matriz JSON e iterar a través de él, imprimir el índice de cualquier `true` valores. Estos valores se corresponden con el índice de puntos de datos anómalos, si se detecta alguno.

```java
static void detectAnomaliesBatch(String requestData) {
    System.out.println("Detecting anomalies as a batch");
    String result = sendRequest(batchDetectionUrl, endpoint, subscriptionKey, requestData);
    if (result != null) {
        System.out.println(result);
        JSONObject jsonObj = new JSONObject(result);
        if (jsonObj.has("code")) {
            System.out.println(String.format("Detection failed. ErrorCode:%s, ErrorMessage:%s", jsonObj.getString("code"), jsonObj.getString("message")));
        } else {
            JSONArray jsonArray = jsonObj.getJSONArray("isAnomaly");
            System.out.println("Anomalies found in the following data positions:");
            for (int i = 0; i < jsonArray.length(); ++i) {
                if (jsonArray.getBoolean(i))
                    System.out.print(i + ", ");
            }
            System.out.println();
        }
    }
}
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Detectar el estado de anomalías del punto de datos más reciente

* Cree un método llamado `detectAnomaliesLatest()` para detectar el estado de anomalías del último punto de datos en el conjunto de datos. Llame a la `sendRequest()` método creada anteriormente con el punto de conexión, dirección url, la clave de suscripción y datos json. Obtener el resultado e imprimirlo en la consola.

```java
static void detectAnomaliesLatest(String requestData) {
    System.out.println("Determining if latest data point is an anomaly");
    String result = sendRequest(latestPointDetectionUrl, endpoint, subscriptionKey, requestData);
    System.out.println(result);
}
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Cargar datos de series temporales y enviar la solicitud

1. En el método main de la aplicación, en el archivo JSON que contiene los datos que se agregarán a las solicitudes de lectura.

2. Llamar a las funciones de detección de dos anomalías creadas anteriormente.

```java
public static void main(String[] args) throws Exception {
    String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "utf-8");
    detectAnomaliesBatch(requestData);
    detectAnomaliesLatest(requestData);
}
```

### <a name="example-response"></a>Respuesta de ejemplo

Se devuelve una respuesta correcta en formato JSON. Haga clic en los siguientes vínculos para ver la respuesta JSON en GitHub:
* [Respuesta de detección por lotes de ejemplo](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Respuesta de detección de punto más reciente de ejemplo](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)