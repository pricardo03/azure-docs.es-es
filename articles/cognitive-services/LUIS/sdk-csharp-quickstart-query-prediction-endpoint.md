---
title: 'Inicio rápido: SDK de C# para la consulta de un punto de conexión de predicción'
titleSuffix: Azure Cognitive Services
description: Use el SDK de C# para enviar una expresión de usuario a LUIS y recibir una predicción.
author: diberry
manager: nitinme
ms.service: cognitive-services
services: cognitive-services
ms.subservice: language-understanding
ms.topic: quickstart
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: af20d555a83e8d229ed5d83d3b1d3f242de1e4a8
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68275803"
---
# <a name="quickstart-query-prediction-endpoint-with-c-net-sdk"></a>Inicio rápido: Consulta de un punto de conexión de predicción con el SDK de .NET de C#

Use el SDK de .NET, que se encuentra en [NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/), para enviar una expresión de usuario a Language Understanding (LUIS) y recibir una predicción de la intención del usuario. 

En este artículo de inicio rápido se envía una expresión de usuario, como `turn on the bedroom light`, a una aplicación de Language Understanding pública, que recibe la predicción y muestra la intención `HomeAutomation.TurnOn` de máxima puntuación y la entidad `HomeAutomation.Room` que se encuentra dentro de la expresión. 

## <a name="prerequisites"></a>Requisitos previos

* [Visual Studio Community Edition 2017](https://visualstudio.microsoft.com/vs/community/)
* Lenguaje de programación de C# (incluido con Visual Studio Community 2017)
* Identificador de la aplicación pública: df67dcdb-c37d-46af-88e1-8b97951ca1c2

> [!Note]
> La solución completa está disponible en el repositorio [cognitive-services-language-understanding](https://github.com/Azure-Samples/cognitive-services-language-understanding/tree/master/documentation-samples/sdk-quickstarts/c%23/UsePredictionRuntime) de GitHub.

¿Busca más documentación?

 * [Documentación de referencia del SDK](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet)


## <a name="get-cognitive-services-or-language-understanding-key"></a>Obtención de la clave de Cognitive Services o Language Understanding

Para poder usar la aplicación pública para la automatización del hogar, necesita una clave válida para las predicciones de punto de conexión. Puede usar una clave de Cognitive Services (que se crea a continuación con la CLI de Azure), válida para muchos servicios cognitivos, o una clave de `Language Understanding`. 

Use el siguiente [comando de la CLI de Azure para crear una clave de Cognitive Service](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create):

```azurecli-interactive
az cognitiveservices account create \
    -n my-cog-serv-resource \
    -g my-cog-serv-resource-group \
    --kind CognitiveServices \
    --sku S0 \
    -l WestEurope \ 
    --yes
```

## <a name="create-net-core-project"></a>Creación de un proyecto de .NET Core

Cree un proyecto de consola .NET Core en Visual Studio Community 2017.

1. Abra Visual Studio Community 2017.
1. Cree un proyecto, en la sección **Visual C#** , elija **Console App (.NET Core)** (Aplicación de consola [.NET Core]).
1. Escriba el nombre del proyecto `QueryPrediction`, deje los valores predeterminados restantes y seleccione **Aceptar**.
    Esto crea un proyecto sencillo con el archivo de código principal **Program.cs**.

## <a name="add-sdk-with-nuget"></a>Incorporación del SDK con NuGet

1. En el **Explorador de soluciones**, seleccione el proyecto en la vista de árbol denominada **QueryPrediction** y haga clic con el botón derecho. En el menú, seleccione **Administrar paquetes de NuGet...** .
1. Seleccione **Examinar** y escriba `Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime`. Cuando se muestre la información del paquete, seleccione **Instalar** para instalar el paquete en el proyecto. 
1. Agregue las siguientes instrucciones _using_ al principio del archivo **Program.cs**. No quite la instrucción _using_ existente para `System`. 

```csharp
using System.Threading;
using System.Threading.Tasks;
using Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime;
using Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime.Models;
```

## <a name="create-a-new-method-for-the-prediction"></a>Creación de un método para la predicción

Cree un método `GetPrediction` para enviar la consulta del punto de conexión de predicción. El método creará y configurará todos los objetos necesarios y devolverá `Task` con los resultados de predicción de [`LuisResult`](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.models.luisresult?view=azure-dotnet). 

```csharp
static async  Task<LuisResult> GetPrediction() {
}
```

## <a name="create-credentials-object"></a>Creación de un objeto de credenciales

Agregue el siguiente código al método `GetPrediction` para crear credenciales de cliente con su clave de Cognitive Services.

Reemplace `<REPLACE-WITH-YOUR-KEY>` por la región de la clave de Cognitive Services. La clave está en [Azure Portal](https://portal.azure.com), en la página de claves de ese recurso.

```csharp
// Use Language Understanding or Cognitive Services key
// to create authentication credentials
var endpointPredictionkey = "<REPLACE-WITH-YOUR-KEY>";
var credentials = new ApiKeyServiceClientCredentials(endpointPredictionkey);
```

## <a name="create-language-understanding-client"></a>Creación del cliente de Language Understanding

En el método `GetPrediction`, después del código anterior, agregue el siguiente código al usar las nuevas credenciales para crear un objeto de cliente de [`LUISRuntimeClient`](https://docs.microsoft.com/dotnet/api/microsoft.azure.cognitiveservices.language.luis.runtime.luisruntimeclient.-ctor?view=azure-dotnet#Microsoft_Azure_CognitiveServices_Language_LUIS_Runtime_LUISRuntimeClient__ctor_Microsoft_Rest_ServiceClientCredentials_System_Net_Http_DelegatingHandler___). 

Reemplace `<REPLACE-WITH-YOUR-KEY-REGION>` por la región de la clave, como `westus`. La región de la clave está en [Azure Portal](https://portal.azure.com), en la página de información general de ese recurso.

```csharp
// Create Luis client and set endpoint
// region of endpoint must match key's region, for example `westus`
var luisClient = new LUISRuntimeClient(credentials, new System.Net.Http.DelegatingHandler[] { });
luisClient.Endpoint = "https://<REPLACE-WITH-YOUR-KEY-REGION>.api.cognitive.microsoft.com";
```

## <a name="set-query-parameters"></a>Establecimiento de los parámetros de consulta

En el método `GetPrediction`, después del código anterior, agregue el siguiente código para establecer los parámetros de consulta.

```csharp
// public Language Understanding Home Automation app
var appId = "df67dcdb-c37d-46af-88e1-8b97951ca1c2";

// query specific to home automation app
var query = "turn on the bedroom light";

// common settings for remaining parameters
Double? timezoneOffset = null;
var verbose = true;
var staging = false;
var spellCheck = false;
String bingSpellCheckKey = null;
var log = false;
```

## <a name="query-prediction-endpoint"></a>Consulta de un punto de conexión de predicción

En el método `GetPrediction`, después del código anterior, agregue el siguiente código para establecer los parámetros de consulta:

```csharp
// Create prediction client
var prediction = new Prediction(luisClient);

// get prediction
return await prediction.ResolveAsync(appId, query, timezoneOffset, verbose, staging, spellCheck, bingSpellCheckKey, log, CancellationToken.None);
```

## <a name="display-prediction-results"></a>Visualización de los resultados de predicción

Cambie el método **Main** para llamar al nuevo método `GetPrediction` y que se devuelvan los resultados de la predicción:

```csharp
static void Main(string[] args)
{

    var luisResult = GetPrediction().Result;

    // Display query
    Console.WriteLine("Query:'{0}'", luisResult.Query);

    // Display most common properties of query result
    Console.WriteLine("Top intent is '{0}' with score {1}", luisResult.TopScoringIntent.Intent,luisResult.TopScoringIntent.Score);

    // Display all entities detected in query utterance
    foreach (var entity in luisResult.Entities)
    {
        Console.WriteLine("{0}:'{1}' begins at position {2} and ends at position {3}", entity.Type, entity.Entity, entity.StartIndex, entity.EndIndex);
    }

    Console.Write("done");

}
```

## <a name="run-the-project"></a>Ejecución del proyecto

Compile el proyecto en Studio y ejecute el proyecto para ver la salida de la consulta:

```console
Query:'turn on the bedroom light'
Top intent is 'HomeAutomation.TurnOn' with score 0.809439957
HomeAutomation.Room:'bedroom' begins at position 12 and ends at position 18
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre el [SDK de .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.LUIS.Runtime/) y la [documentación de referencia de .NET](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/languageunderstanding?view=azure-dotnet). 

> [!div class="nextstepaction"] 
> [Tutorial: Compilación de una aplicación de LUIS para determinar las intenciones del usuario](luis-quickstart-intents-only.md) 