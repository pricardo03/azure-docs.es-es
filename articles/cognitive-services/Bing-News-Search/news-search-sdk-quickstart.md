---
title: 'Guía de inicio rápido: Realizar una búsqueda de noticias: SDK de Bing News Search para C#'
titleSuffix: Azure Cognitive Services
description: Use este inicio rápido para buscar noticias mediante el SDK de Bing News Search para C# y procesar la respuesta.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: quickstart
ms.date: 01/10/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 9e0a4da2662b9d4b4b775631705e7b4fcc1eecea
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/16/2019
ms.locfileid: "65798669"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-c"></a>Inicio rápido: Realizar una búsqueda de noticias con el SDK de Bing News Search para C#

Use este inicio rápido para empezar a buscar noticias con el SDK de Bing News Search para C#. Aunque Bing News Search tiene una API REST compatible con la mayoría de los lenguajes de programación, el SDK proporciona una forma sencilla de integrar el servicio en sus aplicaciones. El código fuente de este ejemplo está disponible en [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingNewsSearch).

## <a name="prerequisites"></a>Requisitos previos

* Cualquier edición de [Visual Studio 2017 o posterior](https://www.visualstudio.com/downloads/).
* El marco [Json.NET](https://www.newtonsoft.com/json), disponible como un paquete NuGet.
* Si usa Linux/MacOS, esta aplicación puede ejecutarse con [Mono](https://www.mono-project.com/).

* El [paquete NuGet del SDK de Bing News Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/1.2.0). La instalación de este paquete también instala lo siguiente:
    * Microsoft.Rest.ClientRuntime
    * Microsoft.Rest.ClientRuntime.Azure
    * Newtonsoft.Json

Para configurar una aplicación de consola con el SDK de Bing News Search, vaya a la opción `Manage NuGet Packages` del Explorador de soluciones en Visual Studio.  Agregue el paquete `Microsoft.Azure.CognitiveServices.Search.NewsSearch`.

[!INCLUDE [cognitive-services-bing-news-search-signup-requirements](../../../includes/cognitive-services-bing-news-search-signup-requirements.md)]

Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).

## <a name="create-and-initialize-a-project"></a>Creación e inicialización de un proyecto

1. Cree una solución de consola de C# en Visual Studio. Después, agregue lo siguiente en el archivo de código principal.
    
    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.NewsSearch;
    ```

2. Cree una variable para la clave de API y un término de búsqueda y, a continuación, cree una instancia del cliente de búsqueda de noticias con ellos.

    ```csharp
    var key = "YOUR-ACCESS-KEY";
    var searchTerm = "Quantum Computing";
    var client = new NewsSearchClient(new ApiKeyServiceClientCredentials(key));
    ```

## <a name="send-a-request-and-parse-the-result"></a>Envío de una solicitud y análisis del resultado

1. Use el cliente para enviar una solicitud de búsqueda al servicio Bing News Search:
    ```csharp
    var newsResults = client.News.SearchAsync(query: searchTerm, market: "en-us", count: 10).Result;
    ```

2. Si se devolvió algún resultado, analícelo:

    ```csharp
    if (newsResults.Value.Count > 0)
    {
        var firstNewsResult = newsResults.Value[0];
    
        Console.WriteLine($"TotalEstimatedMatches value: {newsResults.TotalEstimatedMatches}");
        Console.WriteLine($"News result count: {newsResults.Value.Count}");
        Console.WriteLine($"First news name: {firstNewsResult.Name}");
        Console.WriteLine($"First news url: {firstNewsResult.Url}");
        Console.WriteLine($"First news description: {firstNewsResult.Description}");
        Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
        Console.WriteLine($"First news provider: {firstNewsResult.Provider[0].Name}");
    }
    
    else
    {
        Console.WriteLine("Couldn't find news results!");
    }
    Console.WriteLine("Enter any key to exit...");
    Console.ReadKey();
    ```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Creación de una aplicación web de una sola página](tutorial-bing-news-search-single-page-app.md)
