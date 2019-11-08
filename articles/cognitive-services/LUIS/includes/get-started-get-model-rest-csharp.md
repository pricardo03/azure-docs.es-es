---
title: Obtención del modelo con la llamada de REST en C#
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 10/18/2019
ms.author: diberry
ms.openlocfilehash: 503482243f5aa2e7f833257a3a6eb91a3b5c5ec1
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505839"
---
## <a name="prerequisites"></a>Requisitos previos

* Clave de inicio.
* Importe la aplicación [TravelAgent](https://github.com/Azure-Samples/cognitive-services-language-understanding/blob/master/documentation-samples/quickstarts/change-model/TravelAgent.json) desde el repositorio de GitHub cognitive-services-language-understanding.
* El identificador de LUIS de la aplicación TravelAgent importada. El identificador de aplicación se muestra en el panel de la aplicación.
* El identificador de versión dentro de la aplicación que recibe las expresiones. El id. predeterminado es "0.1".
* [.NET Core V2.2+](https://dotnet.microsoft.com/download)
* [Visual Studio Code](https://code.visualstudio.com/)

## <a name="example-utterances-json-file"></a>Archivo JSON de expresiones de ejemplo

[!INCLUDE [Quickstart explanation of example utterance JSON file](get-started-get-model-json-example-utterances.md)]

## <a name="get-luis-key"></a>Obtención de la clave de LUIS

[!INCLUDE [Use authoring key for endpoint](../includes/get-key-quickstart.md)]

## <a name="change-model-programmatically"></a>Cambio de modelo mediante programación

Use C# para agregar una [API](https://aka.ms/luis-apim-v3-authoring) de entidad de aprendizaje automático a la aplicación. 

1. Cree una aplicación de consola con C# como lenguaje de destino, con `model-with-rest` como nombre del proyecto y de la carpeta. 

    ```console
    dotnet new console -lang C# -n model-with-rest
    ```

1. Instale las dependencias necesarias con los siguientes comandos de la CLI de dotnet.

    ```console
    dotnet add package System.Net.Http
    dotnet add package JsonFormatterPlus
    ```
1. Sobrescriba Program.cs con el código siguiente:

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Text;
    using System.Threading.Tasks;
    using System.Collections.Generic;
    using System.Linq;
    
    // 3rd party NuGet packages
    using JsonFormatterPlus;
    
    namespace AddUtterances
    {
        class Program
        {
            // NOTE: use your starter key value
            static string authoringKey = "YOUR-KEY";
    
            // NOTE: Replace this endpoint with your starter key endpoint
            // for example, westus.api.cognitive.microsoft.com
            static string endpoint = "YOUR-ENDPOINT";
    
            // NOTE: Replace this with the ID of your LUIS application
            static string appID = "YOUR-APP-ID";
    
            // NOTE: Replace this your version number
            static string appVersion = "0.1";
    
            static string host = String.Format("https://{0}/luis/authoring/v3.0-preview/apps/{1}/versions/{2}/", endpoint, appID, appVersion);
    
            // GET request with authentication
            async static Task<HttpResponseMessage> SendGet(string uri)
            {
                using (var client = new HttpClient())
                using (var request = new HttpRequestMessage())
                {
                    request.Method = HttpMethod.Get;
                    request.RequestUri = new Uri(uri);
                    request.Headers.Add("Ocp-Apim-Subscription-Key", authoringKey);
                    return await client.SendAsync(request);
                }
            }
            // POST request with authentication
            async static Task<HttpResponseMessage> SendPost(string uri, string requestBody)
            {
                using (var client = new HttpClient())
                using (var request = new HttpRequestMessage())
                {
                    request.Method = HttpMethod.Post;
                    request.RequestUri = new Uri(uri);
    
                    if (!String.IsNullOrEmpty(requestBody))
                    {
                        request.Content = new StringContent(requestBody, Encoding.UTF8, "text/json");
                    }
    
                    request.Headers.Add("Ocp-Apim-Subscription-Key", authoringKey);
                    return await client.SendAsync(request);
                }
            }        
            // Add utterances as string with POST request
            async static Task AddUtterances(string utterances)
            {
                string uri = host + "examples";
    
                var response = await SendPost(uri, utterances);
                var result = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Added utterances.");
                Console.WriteLine(JsonFormatter.Format(result));
            }
            // Train app after adding utterances
            async static Task Train()
            {
                string uri = host  + "train";
    
                var response = await SendPost(uri, null);
                var result = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Sent training request.");
                Console.WriteLine(JsonFormatter.Format(result));
            }    
            // Check status of training
            async static Task Status()
            {
                var response = await SendGet(host  + "train");
                var result = await response.Content.ReadAsStringAsync();
                Console.WriteLine("Requested training status.");
                Console.WriteLine(JsonFormatter.Format(result));
            }    
            // Add utterances, train, check status
            static void Main(string[] args)
            {
                string utterances = @"
                [
                    {
                    'text': 'go to Seattle today',
                    'intentName': 'BookFlight',
                    'entityLabels': [
                        {
                        'entityName': 'Location::LocationTo',
                        'startCharIndex': 6,
                        'endCharIndex': 12
                        }
                    ]
                    },
                    {
                        'text': 'a barking dog is annoying',
                        'intentName': 'None',
                        'entityLabels': []
                    }
                ]
                ";            
                AddUtterances(utterances).Wait();
                Train().Wait();
                Status().Wait();
            }
        }
    }
    ```

1. Reemplace los valores siguientes:

    * `YOUR-KEY` por la clave de inicio.
    * `YOUR-ENDPOINT` por el punto de conexión, por ejemplo, `westus2.api.cognitive.microsoft.com`.
    * `YOUR-APP-ID` por el identificador de la aplicación.

1. Compile la aplicación de la consola. 

    ```console
    dotnet build
    ```

1. Ejecución de la aplicación de consola. La salida de la consola muestra el mismo código JSON que vio anteriormente en la ventana del explorador.

    ```console
    dotnet run
    ```

## <a name="luis-keys"></a>Claves de LUIS

[!INCLUDE [Use authoring key for endpoint](../includes/starter-key-explanation.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con este inicio rápido, elimine el archivo del sistema de archivos. 

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Procedimientos recomendados para una aplicación](../luis-concept-best-practices.md)