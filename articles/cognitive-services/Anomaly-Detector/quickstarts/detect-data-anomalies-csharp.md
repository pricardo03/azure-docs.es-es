---
title: 'Inicio rápido: Detectar anomalías en datos de series temporales mediante la API de REST del Detector de anomalías y C# | Microsoft Docs'
description: Use la API del Detector de anomalías para detectar anomalías en la serie de datos como un lote o en la transmisión de datos.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: article
ms.date: 03/26/2019
ms.author: aahi
ms.openlocfilehash: 772f15f54819f31d92411df747fc10d54b3e96cd
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59544119"
---
# <a name="quickstart-detect-anomalies-in-your-time-series-data-using-the-anomaly-detector-rest-api-and-c"></a>Inicio rápido: Detectar anomalías en datos de series temporales mediante la API de REST del Detector de anomalías yC# 

Use esta guía de inicio rápido para comenzar a usar los dos modos de detección de la API Detector de anomalías para detectar anomalías en datos de series temporales. Esto C# aplicación envía dos solicitudes de API que contiene datos de series temporales con formato JSON y obtiene las respuestas.

| Solicitud de API                                        | Salida de la aplicación                                                                                                                         |
|----------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Detectar anomalías como un lote                        | La respuesta JSON que contiene el estado de anomalías (y otros datos) para cada punto de datos en los datos de series temporales y las posiciones de las anomalías detectadas. |
| Detectar el estado de anomalías del punto de datos más reciente | La respuesta JSON que contiene el estado de anomalías (y otros datos) para el punto de datos más reciente en los datos de serie temporal.                                                                                                                                         |

 Si bien esta aplicación está escrita en C#, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

## <a name="prerequisites"></a>Requisitos previos

- Cualquier edición de [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
- [Newtonsoft.Json](https://www.newtonsoft.com/json)
    - Para instalar Newtonsoft.Json como un paquete de NuGet en Visual studio:
        1. Haga clic con el botón derecho en el **Administrador de la solución**
        2. Haga clic en **Administrar paquetes de NuGet...**
        3. Busque `Newtonsoft.Json` e instale el paquete
- Si usa Linux, MacOS, se puede ejecutar esta aplicación mediante [Mono](http://www.mono-project.com/).

- Puntos de JSON que contiene tiempo serie datos de un archivo. Los datos de ejemplo para este inicio rápido pueden encontrarse en [GitHub](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/request-data.json).

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

[!INCLUDE [cognitive-services-anomaly-detector-signup-requirements](../../../../includes/cognitive-services-anomaly-detector-signup-requirements.md)]

## <a name="create-a-new-application"></a>Creación de una aplicación

1. En Visual Studio, cree una nueva solución de consola y agregue los siguientes paquetes. 

    ```csharp
    using System;
    using System.IO;
    using System.Net;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Text;
    using System.Threading.Tasks;
    ```

2. Cree variables para su clave de suscripción y el punto de conexión. A continuación se muestran los URI puede usar para la detección de anomalías. Estos se agregarán a su punto de conexión de servicio más adelante para crear la API de direcciones URL de solicitud.

    |Método de detección  |URI  |
    |---------|---------|
    |Detección de lote    | `/anomalydetector/v1.0/timeseries/entire/detect`        |
    |Detección en el último punto de datos     | `/anomalydetector/v1.0/timeseries/last/detect`        |
    
    ```csharp
    // Replace the subscriptionKey string value with your valid subscription key.
    const string subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";
    // Replace the endpoint URL with the correct one for your subscription. 
    // Your endpoint can be found in the Azure portal. For example: https://westus2.api.cognitive.microsoft.com
    const string endpoint = "[YOUR_ENDPOINT_URL]";
    // Replace the dataPath string with a path to the JSON formatted time series data.
    const string dataPath = "[PATH_TO_TIME_SERIES_DATA]";
    const string latestPointDetectionUrl = "/anomalydetector/v1.0/timeseries/last/detect";
    const string batchDetectionUrl = "/anomalydetector/v1.0/timeseries/entire/detect";
    ```

## <a name="create-a-function-to-send-requests"></a>Cree una función para enviar solicitudes

1. Crear una nueva función asincrónica llamada `Request` que toma las variables que creó anteriormente.

2. Establecer el protocolo de seguridad del cliente y obtener información de encabezado con un `HttpClient` objeto. No olvide agregar la clave de suscripción para el `Ocp-Apim-Subscription-Key` encabezado. A continuación, cree un `StringContent` objeto de la solicitud.

3. Enviar la solicitud con `PostAsync()`y, a continuación, devolver la respuesta.

```csharp
static async Task<string> Request(string apiAddress, string endpoint, string subscriptionKey, string requestData){
    using (HttpClient client = new HttpClient { BaseAddress = new Uri(apiAddress) }){
        System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls;
        client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
        client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

        var content = new StringContent(requestData, Encoding.UTF8, "application/json");
        var res = await client.PostAsync(endpoint, content);
        return await res.Content.ReadAsStringAsync();
    }
}
```

## <a name="detect-anomalies-as-a-batch"></a>Detectar anomalías como un lote

1. Crear una nueva función llamada `detectAnomaliesBatch()`. Construir la solicitud y enviarla mediante una llamada a la `Request()` función con el punto de conexión, la clave de suscripción, la dirección URL de detección de anomalías de lote y los datos de serie temporal.

2. Deserializar el objeto JSON y escribirlo en la consola.

3. Si la respuesta contiene `code` campo, el código de error y el mensaje de error de impresión. 

4. En caso contrario, busque las posiciones de anomalías en el conjunto de datos. La respuesta `isAnomaly` campo contiene una matriz de valores booleanos, los cuales indica si un punto de datos es una anomalía. Convertir esto en una matriz de cadenas con el objeto de respuesta `ToObject<bool[]>()` función. Recorrer en iteración la matriz e imprimir el índice de cualquier `true` valores. Estos valores se corresponden con el índice de puntos de datos anómalos, si se detecta alguno.

```csharp
static void detectAnomaliesBatch(string requestData){
    System.Console.WriteLine("Detecting anomalies as a batch");

    var result = Request(
        endpoint,
        batchDetectionUrl,
        subscriptionKey,
        requestData).Result;

    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
    System.Console.WriteLine(jsonObj);

    if (jsonObj["code"] != null){
        System.Console.WriteLine($"Detection failed. ErrorCode:{jsonObj["code"]}, ErrorMessage:{jsonObj["message"]}");
    }
    else{
        bool[] anomalies = jsonObj["isAnomaly"].ToObject<bool[]>();
        System.Console.WriteLine("\nAnomalies detected in the following data positions:");
        for (var i = 0; i < anomalies.Length; i++){
            if (anomalies[i])
            {
                System.Console.Write(i + ", ");
            }
        }
    }
}
```

## <a name="detect-the-anomaly-status-of-the-latest-data-point"></a>Detectar el estado de anomalías del punto de datos más reciente

1. Crear una nueva función llamada `detectAnomaliesLatest()`. Construir la solicitud y enviarla mediante una llamada a la `Request()` función con el punto de conexión, la clave de suscripción, la dirección URL de detección de anomalías de punto más reciente y los datos de serie temporal.

2. Deserializar el objeto JSON y escribirlo en la consola.

```csharp
static void detectAnomaliesLatest(string requestData){
    System.Console.WriteLine("\n\nDetermining if latest data point is an anomaly");
    var result = Request(
        endpoint,
        latestPointDetectionUrl,
        subscriptionKey,
        requestData).Result;

    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
    System.Console.WriteLine(jsonObj);
}
```

## <a name="load-your-time-series-data-and-send-the-request"></a>Cargar datos de series temporales y enviar la solicitud

1. En el método main de la aplicación, cargue los datos de series temporales JSON con `File.ReadAllText()`. 

2. Llamar a las funciones de detección de anomalías creadas anteriormente. Use `System.Console.ReadKey()` para mantener la ventana de consola abierta después de ejecutar la aplicación.

```csharp
static void Main(string[] args){

    var requestData = File.ReadAllText(dataPath);

    detectAnomaliesBatch(requestData);
    detectAnomaliesLatest(requestData);

    System.Console.ReadKey();
}
```

### <a name="example-response"></a>Respuesta de ejemplo

Se devuelve una respuesta correcta en formato JSON. Haga clic en los siguientes vínculos para ver la respuesta JSON en GitHub:
* [Respuesta de detección por lotes de ejemplo](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/batch-response.json)
* [Respuesta de detección de punto más reciente de ejemplo](https://github.com/Azure-Samples/anomalydetector/blob/master/example-data/latest-point-response.json)

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://westus2.dev.cognitive.microsoft.com/docs/services/AnomalyDetector/operations/post-timeseries-entire-detect)