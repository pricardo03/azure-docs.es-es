---
title: 'Uso de Anomaly Finder API con Java: Microsoft Cognitive Services | Microsoft Docs'
description: Obtenga información y ejemplos de código que le ayudarán a empezar a usar Java y Anomaly Detection en Cognitive Services rápidamente.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: kefre
ms.openlocfilehash: 0f96ee13e6020bc69508f8ced45741fe7dec396a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60495058"
---
# <a name="use-the-anomaly-finder-api-with-java"></a>Uso de Anomaly Finder API con Java

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

En este artículo se ofrece información y ejemplos de código que le ayudan a comenzar a usar rápidamente Anomaly Detection API con Java para realizar la tarea de obtener resultados de la detección de anomalías en los datos de series temporales.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-the-anomaly-detection-api-using-java"></a>Obtención de puntos de anomalías con Anomaly Detection API mediante Java

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Ejemplo de datos de series temporales

Este es el ejemplo de los puntos de datos de series temporales.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-java-example"></a>Ejemplo de análisis de datos y obtención de puntos de anomalías en Java

Para ejecutar el ejemplo, realice los siguiente pasos:
1. Crear una nueva aplicación de línea de comandos.
2. Reemplace la clase Main por el código siguiente (mantenga las instrucciones `package`).
3. Reemplace el valor de `[YOUR_SUBSCRIPTION_KEY]` por su clave de suscripción válida.
4. Reemplace `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` con los puntos de datos de ejemplo o los suyos propios.
5. Descargue estas bibliotecas globales del repositorio de Maven al directorio `lib` del proyecto:
   * `org.apache.httpcomponents:httpclient:4.5.2`
6. Ejecute "Main".

```java

import org.apache.http.HttpEntity;
import org.apache.http.client.methods.CloseableHttpResponse;
import org.apache.http.client.methods.HttpPost;
import org.apache.http.entity.StringEntity;
import org.apache.http.impl.client.CloseableHttpClient;
import org.apache.http.impl.client.HttpClients;
import org.apache.http.util.EntityUtils;

public class Main {
    // **********************************************
    // *** Update or verify the following values. ***
    // **********************************************

    // Replace the subscriptionKey string value with your valid subscription key.
    public static final String subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";

    public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

    public static void main(String[] args) {
        final String content = "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]";

        CloseableHttpClient client = HttpClients.createDefault();
        HttpPost request = new HttpPost(uriBase);

        // Request headers.
        request.setHeader("Content-Type", "application/json");
        request.setHeader("Ocp-Apim-Subscription-Key", subscriptionKey);

        try {
            StringEntity params = new StringEntity(content);
            request.setEntity(params);

            CloseableHttpResponse response = client.execute(request);
            try {
                HttpEntity respEntity = response.getEntity();
                if (respEntity != null) {
                    System.out.println("----------");
                    System.out.println(response.getStatusLine());
                    System.out.println("Response content is :\n");
                    System.out.println(EntityUtils.toString(respEntity, "utf-8"));
                    System.out.println("----------");
                }
            } catch (Exception respEx) {
                respEx.printStackTrace();
            } finally {
                response.close();
            }

        } catch (Exception ex) {
            System.err.println("Exception on Anomaly Detection: " + ex.getMessage());
            ex.printStackTrace();
        } finally {
            try {
                client.close();
            } catch (Exception e) {
                System.err.println("Exception on closing HttpClient: " + e.getMessage());
                e.printStackTrace();
            }
        }
    }
}

```

### <a name="example-response"></a>Respuesta de ejemplo

Se devuelve una respuesta correcta en JSON. A continuación se muestra una respuesta de ejemplo.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación Java](../tutorials/java-tutorial.md)
