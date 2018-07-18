---
title: Guía de inicio rápido del SDK de Web Search para C# | Microsoft Docs
description: Configuración de la aplicación de consola del SDK de Web Search para C#.
titleSuffix: Azure cognitive services Web search SDK C# quickstart
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 01/29/2018
ms.author: v-gedod
ms.openlocfilehash: 6d87b292475edff04e930ec4aa2f8e077a0fb82c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382626"
---
# <a name="web-search-sdk-c-quickstart"></a>Guía de inicio rápido del SDK de Web Search para C#

El SDK de Bing Web Search contiene la funcionalidad de la API REST para consultas web y análisis de resultados.

El [código fuente de los ejemplos del SDK de Bing Web Search para C#](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/blob/master/BingSearchv7/BingWebSearch/WebSearchSamples.cs) está disponible en GitHub.

## <a name="application-dependencies"></a>Dependencias de aplicaciones

Para configurar una aplicación de consola con el SDK de Bing Web Search, vaya a la opción `Manage NuGet Packages` del Explorador de soluciones en Visual Studio.  Agregue el paquete `Microsoft.Azure.CognitiveServices.Search.WebSearch`.

La instalación del [paquete del SDK de NuGet Web Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.WebSearch/1.2.0) también instala dependencias, entre las que se incluyen:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="web-search-client"></a>Cliente de Web Search
Para crear una instancia del cliente `WebSearchAPI`, agregue directivas Using:
```
using Microsoft.Azure.CognitiveServices.Search.WebSearch;
using Microsoft.Azure.CognitiveServices.Search.WebSearch.Models;

```
Después, cree una instancia del cliente:
```
var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));


```
Utilice al cliente para buscar con un texto de consulta:
```
// Search for "Yosemite National Park"
var webData = client.Web.Search(query: "Yosemite National Park");
Console.WriteLine("Searched for Query# \" Yosemite National Park \"");

```
Analizar las páginas web que se devuelven en los resultados de la consulta anterior:
```
//WebPages
if (webData?.WebPages?.Value?.Count > 0)
{
    // find the first web page
    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

    if (firstWebPagesResult != null)
    {
        Console.WriteLine("Webpage Results #{0}", webData.WebPages.Value.Count);
        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
    }
    else
    {
        Console.WriteLine("Couldn't find web results!)";
    }
}
else
{
    Console.WriteLine("Didn't see any Web data..");
}

```
## <a name="complete-console-application"></a>Aplicación completa de consola

La siguiente aplicación de consola ejecuta la consulta definida previamente y analiza las páginas web, imágenes, noticias y vídeos contenidos en los resultados:
```
using System;
using System.Collections.Generic;
using Microsoft.Azure.CognitiveServices.Search.WebSearch;
using Microsoft.Azure.CognitiveServices.Search.WebSearch.Models;
using System.Linq;

namespace WebSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            var client = new WebSearchAPI(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));

            WebResults(client);
            // Include the following calls to use queries defined under following headings.
            //WebResultsWithCountAndOffset(client);  
            //WebSearchWithResponseFilter(client);
            //WebSearchWithAnswerCountPromoteAndSafeSearch(client);

            Console.WriteLine("Any key to exit... ");
            Console.ReadKey();
        }

        public static void WebResults(WebSearchAPI client)
        {
            try
            {
                // Search for (Yosemite National Park"), print the number of results and print out name and url of first result.
                var webData = client.Web.Search(query: "Yosemite National Park");
                Console.WriteLine("Searched for Query# \" Yosemite National Park \"");

                //WebPages
                if (webData?.WebPages?.Value?.Count > 0)
                {
                    // find the first web page
                    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

                    if (firstWebPagesResult != null)
                    {
                        Console.WriteLine("Webpage Results #{0}", webData.WebPages.Value.Count);
                        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
                        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find web results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any Web data..");
                }

                //Images
                if (webData?.Images?.Value?.Count > 0)
                {
                    // find the first image result
                    var firstImageResult = webData.Images.Value.FirstOrDefault();

                    if (firstImageResult != null)
                    {
                        Console.WriteLine("Image Results #{0}", webData.Images.Value.Count);
                        Console.WriteLine("First Image result name: {0} ", firstImageResult.Name);
                        Console.WriteLine("First Image result URL: {0} ", firstImageResult.ContentUrl);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find first image results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any image data..");
                }

                //News
                if (webData?.News?.Value?.Count > 0)
                {
                    // find the first news result
                    var firstNewsResult = webData.News.Value.FirstOrDefault();

                    if (firstNewsResult != null)
                    {
                        Console.WriteLine("\r\nNews Results #{0}", webData.News.Value.Count);
                        Console.WriteLine("First news result name: {0} ", firstNewsResult.Name);
                        Console.WriteLine("First news result URL: {0} ", firstNewsResult.Url);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find any News results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see first news data..");
                }

                //Videos
                if (webData?.Videos?.Value?.Count > 0)
                {
                    // find the first video result
                    var firstVideoResult = webData.Videos.Value.FirstOrDefault();

                    if (firstVideoResult != null)
                    {
                        Console.WriteLine("\r\nVideo Results #{0}", webData.Videos.Value.Count);
                        Console.WriteLine("First Video result name: {0} ", firstVideoResult.Name);
                        Console.WriteLine("First Video result URL: {0} ", firstVideoResult.ContentUrl);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find first video results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any video data..");
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

        }
    }
}

```

Los ejemplos de búsqueda de Bing muestran diversas características del SDK.  Agregue las siguientes funciones a la clase `WebSrchSDK` previamente definida.

## <a name="count-and-offset-parameters"></a>Recuento y desplazamiento de parámetros

El siguiente código busca "Best restaurants in Seattle", comprueba el número de resultados e imprime el nombre y la dirección URL del primer resultado.

```
       public static void WebResultsWithCountAndOffset(WebSearchAPI client)
        {
            try
            {
                var webData = client.Web.SearchAsync(query: "Best restaurants in Seattle", offset: 10, count: 20).Result;
                Console.WriteLine("\r\nSearched for Query# \" Best restaurants in Seattle \"");

                if (webData?.WebPages?.Value?.Count > 0)
                {
                    // find the first web page
                    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

                    if (firstWebPagesResult != null)
                    {
                        Console.WriteLine("Web Results #{0}", webData.WebPages.Value.Count);
                        Console.WriteLine("First web page name: {0} ", firstWebPagesResult.Name);
                        Console.WriteLine("First web page URL: {0} ", firstWebPagesResult.Url);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find first web result!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any Web data..");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
## <a name="response-filter"></a>Filtro de respuesta

La siguiente consulta busca el término "Microsoft" mediante un filtro de respuesta establecido en `news` e imprime los detalles de los resultados.
```
        public static void WebSearchWithResponseFilter(WebSearchAPI client)
        {
            //var client = new WebSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                IList<string> responseFilterstrings = new List<string>() { "news" };
                var webData = client.Web.SearchAsync(query: "Microsoft", responseFilter: responseFilterstrings).Result;
                Console.WriteLine("\r\nSearched for Query# \" Microsoft \" with response filters \"news\"");

                //News
                if (webData?.News?.Value?.Count > 0)
                {
                    // find the first news result
                    var firstNewsResult = webData.News.Value.FirstOrDefault();

                    if (firstNewsResult != null)
                    {
                        Console.WriteLine("News Results #{0}", webData.News.Value.Count);
                        Console.WriteLine("First news result name: {0} ", firstNewsResult.Name);
                        Console.WriteLine("First news result URL: {0} ", firstNewsResult.Url);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find first News results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any News data..");
                }

            }
            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
## <a name="query-parameters---count-promotion-safe-search"></a>Parámetros de consulta: recuento, promoción, búsqueda segura

Esta consulta, "Lady Gaga", busca mediante el uso de los parámetros `answerCount` y `promote` y luego imprime los detalles de los resultados.

```
        public static void WebSearchWithAnswerCountPromoteAndSafeSearch(WebSearchAPI client)
        {
            //var client = new WebSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                IList<string> promoteAnswertypeStrings = new List<string>() { "videos" };
                var webData = client.Web.SearchAsync(query: "Lady Gaga", answerCount: 2, promote: promoteAnswertypeStrings, safeSearch: SafeSearch.Strict).Result;
                Console.WriteLine("\r\nSearched for Query# \" Lady Gaga \"");

                if (webData?.Videos?.Value?.Count > 0)
                {
                    var firstVideosResult = webData.Videos.Value.FirstOrDefault();

                    if (firstVideosResult != null)
                    {
                        Console.WriteLine("Video Results #{0}", webData.Videos.Value.Count);
                        Console.WriteLine("First Video result name: {0} ", firstVideosResult.Name);
                        Console.WriteLine("First Video result URL: {0} ", firstVideosResult.ContentUrl);
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find videos results!");
                    }
                }
                else
                {
                    Console.WriteLine("Didn't see any data..");
                }
            }
            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
```

## <a name="next-steps"></a>Pasos siguientes

[Ejemplos del SDK de Cognitive Services para .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)