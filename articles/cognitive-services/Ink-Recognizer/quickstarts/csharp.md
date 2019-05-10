---
title: 'Inicio rápido: Reconocer la entrada de lápiz digital con la API de REST de módulo de reconocimiento de tinta yC#'
description: Use la API de reconocimiento de tinta para iniciar reconocer trazos de tinta digital.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: ink-recognizer
ms.topic: article
ms.date: 05/02/2019
ms.author: aahi
ms.openlocfilehash: 9bb9c23cc1f807cae1d0d22f1652e8f4408f1f91
ms.sourcegitcommit: 17411cbf03c3fa3602e624e641099196769d718b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/10/2019
ms.locfileid: "65518671"
---
# <a name="quickstart-recognize-digital-ink-with-the-ink-recognizer-rest-api-and-c"></a>Inicio rápido: Reconocer la entrada de lápiz digital con la API de REST de módulo de reconocimiento de tinta yC#

Use esta guía de inicio rápido para empezar el envío de trazos de tinta digital a la API de reconocimiento de tinta. Esto C# aplicación envía una solicitud de API que contiene los datos del trazo de tinta con formato JSON y obtiene la respuesta.

Si bien esta aplicación está escrita en C#, la API es un servicio web RESTful compatible con la mayoría de los lenguajes de programación.

Normalmente se llamaría a la API desde una aplicación de entrada de lápiz digital. En este tutorial rápido envía los datos del trazo de tinta para el siguiente ejemplo escritas a mano desde un archivo JSON.

![una imagen de texto escrito a mano](../media/handwriting-sample.jpg)

El código fuente de este inicio rápido está disponible en [GitHub](https://go.microsoft.com/fwlink/?linkid=2089502).

## <a name="prerequisites"></a>Requisitos previos

- Cualquier edición de [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
- [Newtonsoft.Json](https://www.newtonsoft.com/json)
    - Para instalar Newtonsoft.Json como un paquete de NuGet en Visual studio:
        1. Haga clic con el botón derecho en el **Administrador de la solución**
        2. Haga clic en **Administrar paquetes de NuGet...**
        3. Busque `Newtonsoft.Json` e instale el paquete
- Si usa Linux, MacOS, se puede ejecutar esta aplicación mediante [Mono](https://www.mono-project.com/).

- Los datos del trazo de tinta de ejemplo para este inicio rápido pueden encontrarse en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/InkRecognition/quickstart/example-ink-strokes.json).

[!INCLUDE [cognitive-services-ink-recognizer-signup-requirements](../../../../includes/cognitive-services-ink-recognizer-signup-requirements.md)]


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
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    ```

2. Cree variables para su clave de suscripción y el punto de conexión. A continuación es el URI que se puede usar para el reconocimiento de tinta. Se anexará a su punto de conexión de servicio más adelante para crear la API de dirección URl de solicitud.

    ```csharp
    // Replace the subscriptionKey string with your valid subscription key.
    const string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";

    // Replace the dataPath string with a path to the JSON formatted ink stroke data.
    const string dataPath = @"PATH-TO-INK-STROKE-DATA"; 

    // URI information for ink recognition:
    const string endpoint = "https://api.cognitive.microsoft.com";
    const string inkRecognitionUrl = "/inkrecognizer/v1.0-preview/recognize";
    ```

## <a name="create-a-function-to-send-requests"></a>Cree una función para enviar solicitudes

1. Crear una nueva función asincrónica llamada `Request` que toma las variables que creó anteriormente.

2. Establecer el protocolo de seguridad del cliente y obtener información de encabezado con un `HttpClient` objeto. No olvide agregar la clave de suscripción para el `Ocp-Apim-Subscription-Key` encabezado. A continuación, cree un `StringContent` objeto de la solicitud.
 
3. Enviar la solicitud con `PutAsync()`. Si la solicitud es correcta, devuelva la respuesta.  
    
    ```csharp
    static async Task<string> Request(string apiAddress, string endpoint, string subscriptionKey, string requestData){
        
        using (HttpClient client = new HttpClient { BaseAddress = new Uri(apiAddress) }){
            System.Net.ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12 | SecurityProtocolType.Tls11 | SecurityProtocolType.Tls;
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

            var content = new StringContent(requestData, Encoding.UTF8, "application/json");
            var res = await client.PutAsync(endpoint, content);
            if (res.IsSuccessStatusCode){
                return await res.Content.ReadAsStringAsync();
            }
            else{
                return $"ErrorCode: {res.StatusCode}";
            }
        }
    }
    ```

## <a name="send-an-ink-recognition-request"></a>Enviar una solicitud de reconocimiento de tinta

1. Crear una nueva función llamada `recognizeInk()`. Construir la solicitud y enviarla mediante una llamada a la `Request()` función con el punto de conexión, una clave de suscripción, la dirección URL de la API y los datos del trazo de tinta digital.

2. Deserializar el objeto JSON y escribirlo en la consola. 
    
    ```csharp
    static void recognizeInk(string requestData){

        //construct the request
        var result = Request(
            endpoint,
            inkRecognitionUrl,
            subscriptionKey,
            requestData).Result;

        dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result);
        System.Console.WriteLine(jsonObj);
    }
    ```

## <a name="load-your-digital-ink-data"></a>Cargue los datos de entrada de lápiz digital

Crear una función denominada `LoadJson()` para cargar el archivo JSON de datos de entrada de lápiz. Use un `StreamReader` y `JsonTextReader` para crear un `JObject` y devolverlo.
    
```csharp
public static JObject LoadJson(string fileLocation){

    var jsonObj = new JObject();

    using (StreamReader file = File.OpenText(fileLocation))
    using (JsonTextReader reader = new JsonTextReader(file)){
        jsonObj = (JObject)JToken.ReadFrom(reader);
    }
    return jsonObj;
}
```

## <a name="send-the-api-request"></a>Enviar la solicitud de API

1. En el método main de la aplicación, cargar los datos JSON con la función creada anteriormente. 

2. Llame a la `recognizeInk()` función creada anteriormente. Use `System.Console.ReadKey()` para mantener la ventana de consola abierta después de ejecutar la aplicación.
    
    ```csharp
    static void Main(string[] args){

        var requestData = LoadJson(dataPath);
        string requestString = requestData.ToString(Newtonsoft.Json.Formatting.None);
        recognizeInk(requestString);
        System.Console.WriteLine("\nPress any key to exit ");
        System.Console.ReadKey();
        }
    ```

## <a name="run-the-application-and-view-the-response"></a>Ejecute la aplicación y ver la respuesta

Ejecute la aplicación. Se devuelve una respuesta correcta en formato JSON. También puede encontrar la respuesta JSON en [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/InkRecognition/quickstart/example-response.json).


## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Referencia de API de REST](https://go.microsoft.com/fwlink/?linkid=2089907)


Para ver el funcionamiento de la API Ink Recognition en una aplicación de entrada de lápiz digital, eche un vistazo a las siguientes aplicaciones de ejemplo en GitHub:
* [C# y Plataforma universal de Windows (UWP)](https://go.microsoft.com/fwlink/?linkid=2089803)  
* [C# y Windows Presentation Foundation (WPF)](https://go.microsoft.com/fwlink/?linkid=2089804)
* [Aplicación para explorador web de JavaScript](https://go.microsoft.com/fwlink/?linkid=2089908)       
* [Aplicación móvil Java y Android](https://go.microsoft.com/fwlink/?linkid=2089906)
* [Aplicación móvil iOS y SWIFT](https://go.microsoft.com/fwlink/?linkid=2089805)
