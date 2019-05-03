---
title: 'Inicio rápido: Reconocer la entrada de lápiz digital con la API de REST de módulo de reconocimiento de tinta y Java'
description: Use la API de reconocimiento de tinta para iniciar reconocer trazos de tinta digital.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: article
ms.date: 05/02/2019
ms.author: aahi
ms.openlocfilehash: 43dc388c9f24e3975d6efddbeec6e096bf062548
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65026278"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-java"></a>Inicio rápido: Reconocer la entrada de lápiz digital con la API de REST de módulo de reconocimiento de tinta y Java

Use esta guía de inicio rápido para empezar a usar la API de reconocimiento de tinta en trazos de tinta digital. Esta aplicación de Java envía una solicitud de API que contiene los datos del trazo de tinta con formato JSON y obtiene la respuesta.

Si bien esta aplicación está escrita en Java, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

Normalmente se llamaría a la API desde una aplicación de entrada de lápiz digital. En este tutorial rápido envía los datos del trazo de tinta para el siguiente ejemplo escritas a mano desde un archivo JSON.

![una imagen de texto escrito a mano](../media/handwriting-sample.jpg)

El código fuente de este inicio rápido está disponible en [GitHub](https://go.microsoft.com/fwlink/?linkid=2089904).

## <a name="prerequisites"></a>Requisitos previos

- El [Java&trade; desarrollo Kit(JDK) 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) o una versión posterior.

- Estas bibliotecas de importación del repositorio de Maven
    - [JSON en Java](https://mvnrepository.com/artifact/org.json/json) paquete
    - [Apache HttpClient](https://mvnrepository.com/artifact/org.apache.httpcomponents/httpclient) paquete

- Los datos del trazo de tinta de ejemplo para este inicio rápido pueden encontrarse en [GitHub](https://go.microsoft.com/fwlink/?linkid=2089904).

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]

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
    import java.io.IOException;
    import java.nio.file.Files;
    import java.nio.file.Paths;
    ```

2. Cree variables para su clave de suscripción y el punto de conexión. A continuación es el URI que se puede usar para el reconocimiento de tinta. Se anexará a su punto de conexión de servicio más adelante para crear la dirección URL de solicitud de API.

    ```java
    // Replace the subscriptionKey string value with your valid subscription key.
    static final String subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
    // Replace the dataPath string with a path to the JSON formatted ink stroke data file.
    static final String dataPath = "PATH_TO_INK_STROKE_DATA";
    
    static final String endpoint = "https://api.cognitive.microsoft.com";
    static final String inkRecognitionUrl = "/inkrecognizer/v1.0-preview/recognize";
    ```

## <a name="create-a-function-to-send-requests"></a>Cree una función para enviar solicitudes

1. Crear una nueva función llamada `sendRequest()` que toma las variables que creó anteriormente. Después, lleve a cabo los siguiente pasos.

2. Crear un `CloseableHttpClient` objeto que puede enviar solicitudes a la API. Enviar la solicitud a un `HttpPut` objeto request combinando el punto de conexión y la dirección URL del módulo de reconocimiento de tinta.

3. Usar la solicitud `setHeader()` función para establecer el `Content-Type` encabezado a `application/json`y agregue su clave de suscripción para el `Ocp-Apim-Subscription-Key` encabezado.

4. Usar la solicitud `setEntity()` función a los datos se envíen.   

5. Use el cliente `execute()` función para enviar la solicitud y guárdelo en un `CloseableHttpResponse` objeto. 

6. Crear un `HttpEntity` objeto para almacenar el contenido de la respuesta. Obtener el contenido con `getEntity()`. Si la respuesta no está vacía, la devuelve.
    
    ```java
    static String sendRequest(String apiAddress, String endpoint, String subscriptionKey, String requestData) {
        try (CloseableHttpClient client = HttpClients.createDefault()) {
            HttpPut request = new HttpPut(endpoint + apiAddress);
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
            System.err.println("Exception recognizing ink: " + ex.getMessage());
            ex.printStackTrace();
        }
        return null;
    }
    ```

## <a name="send-an-ink-recognition-request"></a>Enviar una solicitud de reconocimiento de tinta

Cree un método llamado `recognizeInk()` para reconocer los datos del trazo de tinta. Llame a la `sendRequest()` método creada anteriormente con el punto de conexión, dirección url, la clave de suscripción y datos json. Obtener el resultado e imprimirlo en la consola.

```java
static void recognizeInk(String requestData) {
    System.out.println("Sending an ink recognition request");
    String result = sendRequest(inkRecognitionUrl, endpoint, subscriptionKey, requestData);
    System.out.println(result);
}
```

## <a name="load-your-digital-ink-data-and-send-the-request"></a>Cargue los datos de entrada de lápiz digital y enviar la solicitud

1. En el método main de la aplicación, en el archivo JSON que contiene los datos que se agregarán a las solicitudes de lectura.

2. Llame a la función de reconocimiento de tinta que creó anteriormente.
    
    ```java
    public static void main(String[] args) throws Exception {
        String requestData = new String(Files.readAllBytes(Paths.get(dataPath)), "UTF-8");
        recognizeInk(requestData);
    }
    ```

## <a name="run-the-application-and-view-the-response"></a>Ejecute la aplicación y ver la respuesta

Ejecute la aplicación. Se devuelve una respuesta correcta en formato JSON. También puede encontrar la respuesta JSON en [GitHub](https://go.microsoft.com/fwlink/?linkid=2089904).

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://go.microsoft.com/fwlink/?linkid=2089907)


Para ver cómo funciona la API de reconocimiento de tinta en una aplicación de entrada de lápiz digital, eche un vistazo a las siguientes aplicaciones de ejemplo en GitHub:
* [C#y Platform(UWP) Windows Universal](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C#y Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [Aplicación de explorador web de JavaScript](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Aplicación móvil de Java y Android](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Aplicación móvil de iOS y SWIFT](https://go.microsoft.com/fwlink/?linkid=2089805)
