---
title: 'Inicio rápido: Detección de anomalías en los datos de serie temporal mediante API REST Anomaly Detector y Java'
titleSuffix: Azure Cognitive Services
description: Use la API Anomaly Detector para detectar anomalías en la serie de datos como un lote o en la transmisión de datos.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: quickstart
ms.date: 07/26/2019
ms.author: aahi
ms.openlocfilehash: 001d53cbd7e2a57615ea3da71d128bd210a79921
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565855"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-java"></a>Inicio rápido: Detección de anomalías en los datos de serie temporal mediante API REST Anomaly Detector y Java

Use este inicio rápido para comenzar a usar los dos modos de detección de la API Anomaly Detector para detectar anomalías en los datos de serie temporal. Esta aplicación de Java envía dos solicitudes de API que contienen datos de serie temporal con formato JSON y obtiene las respuestas.

| Solicitud a la API                                        | Salida de la aplicación                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Detección de anomalías como un lote                        | La respuesta JSON que contiene el estado de la anomalía (y otros datos) para cada punto de datos en los datos de serie temporal y las posiciones de las anomalías detectadas. |
| Detección del estado de anomalía del punto de datos más reciente | La respuesta JSON que contiene el estado de la anomalía (y otros datos) para el punto de datos más reciente en los datos de serie temporal.                                                                                                                                         |

 Si bien esta aplicación está escrita en Java, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

## <a name="prerequisites"></a>Requisitos previos

- Kit de desarrollo de [Java&trade; (JDK) 7](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) o superior.

- Importación de estas bibliotecas desde el repositorio de Maven
    - Paquete de [JSON en Java](https://mvnrepository.com/artifact/org.json/json)
    - Paquete de [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient)

- Un archivo JSON que contiene los puntos de datos de la serie temporal. Los datos de ejemplo de este inicio rápido están disponibles en [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

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

2. Cree variables para la clave de suscripción y el punto de conexión. A continuación se muestran los URI puede usar para la detección de anomalías. Estos se agregarán a su punto de conexión de servicio más adelante para crear las direcciones URL de solicitud de API.

    |Método de detección  |URI  |
    |---------|---------|
    |Detección por lotes    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
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

3. Lectura en el archivo de datos JSON

    ```java
    String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "utf-8");
    ```

## <a name="create-a-function-to-send-requests"></a>Creación de una función para enviar solicitudes

1. Cree una función llamada `sendRequest()` que adopte las variables creadas anteriormente. Después, lleve a cabo los siguiente pasos.

2. Cree un objeto `CloseableHttpClient` que pueda enviar solicitudes a la API. Envíe la solicitud a un objeto de solicitud `HttpPost` mediante la combinación del punto de conexión y la dirección URL de Anomaly Detector.

3. Use la función `setHeader()` de la solicitud para establecer el encabezado `Content-Type` en `application/json` y agregue su clave de suscripción al encabezado `Ocp-Apim-Subscription-Key`.

4. Use la función `setEntity()` de la solicitud para los datos que se van a enviar.

5. Use la función `execute()` del cliente para enviar la solicitud y guárdela en un objeto `CloseableHttpResponse`.

6. Cree un objeto `HttpEntity` para almacenar el contenido de respuesta. Obtenga el contenido con `getEntity()`. Si la respuesta no está vacía, se devuelve.

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

## <a name="detect-anomalies-as-a-batch"></a>Detección de anomalías como un lote

1. Cree un método llamado `detectAnomaliesBatch()` para detectar anomalías a lo largo de los datos como un lote. Llame al método `sendRequest()` creado anteriormente con el punto de conexión, dirección URL, la clave de suscripción y datos JSON. Obtenga el resultado e imprímalo en la consola.

2. Si la respuesta contiene el campo `code`, imprima el código de error y el mensaje de error.

3. En caso contrario, busque las posiciones de las anomalías en el conjunto de datos. El campo `isAnomaly` de la respuesta contiene un valor booleano sobre si un punto de datos determinado es una anomalía. Obtenga la matriz JSON e itere mediante ella, imprimiendo el índice de cualquier valor `true`. Estos valores se corresponden con el índice de los puntos de datos anómalos, si se detecta alguno.

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

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Detección del estado de anomalía del punto de datos más reciente

* Cree un método llamado `detectAnomaliesLatest()` para detectar el estado de las anomalías del último punto de datos del conjunto de datos. Llame al método `sendRequest()` creado anteriormente con el punto de conexión, dirección URL, la clave de suscripción y datos JSON. Obtenga el resultado e imprímalo en la consola.

```java
static void detectAnomaliesLatest(String requestData) {
    System.out.println("Determining if latest data point is an anomaly");
    String result = sendRequest(latestPointDetectionUrl, endpoint, subscriptionKey, requestData);
    System.out.println(result);
}
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Carga de datos de la serie temporal y envío de la solicitud

1. En el método main de la aplicación, lea el archivo JSON que contiene los datos que se agregarán a las solicitudes de lectura.

2. Llame a las dos funciones de detección de anomalías creadas anteriormente.

```java
public static void main(String[] args) throws Exception {
    String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "utf-8");
    detectAnomaliesBatch(requestData);
    detectAnomaliesLatest(requestData);
}
```

### <a name="example-response"></a>Respuesta de ejemplo

Se devuelve una respuesta correcta en formato JSON. Haga clic en los siguientes vínculos para ver la respuesta JSON en GitHub:
* [Ejemplo de respuesta de detección por lotes](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Ejemplo de respuesta de detección del punto más reciente](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)
