---
title: Tutorial del token ImageInsightsToken del SDK de Bing Visual Search | Microsoft Docs
description: Describe cómo usar el SDK de Bing Visual Search para obtener las direcciones URL de imágenes especificadas por ImageInsightsToken.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/21/2018
ms.author: rosh
ms.openlocfilehash: 8f6e7f7e88ae78fe7e8a9be4adefd689dd26d0f9
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/14/2018
ms.locfileid: "41929721"
---
# <a name="tutorial-bing-visual-search-sdk-imageinsightstoken-and-results"></a>Tutorial: ImageInsightsToken del SDK de Bing Visual Search y resultados
El SDK de Visual Search incluye una opción para buscar imágenes en línea de una búsqueda anterior que devuelve un token `ImageInsightsToken`.  En este ejemplo se obtiene un token `ImageInsightsToken` que se usa en una búsqueda posterior.  El código envía el token `ImageInsightsToken` a Bing y devuelve los resultados que incluyen direcciones URL de Bing Search y las direcciones URL de imágenes similares que se encuentran en línea.

## <a name="prerequisites"></a>Requisitos previos
Visual Studio 2017. Si es necesario, puede descargar la versión gratuita de la comunidad aquí: https://www.visualstudio.com/vs/community/.
Se requiere una clave de API de Cognitive Services para autenticar las llamadas de SDK. Regístrese para obtener una clave de evaluación gratuita. La clave de evaluación es válida durante siete días con una llamada por segundo. Para escenarios de producción, compre una clave de acceso. Consulte también la información sobre precios.
La capacidad para ejecutar el SDK de .NET Core, aplicaciones de .Net Core 1.1. Puede obtener CORE, Framework y Runtime aquí: https://www.microsoft.com/net/download/.

##<a name="application-dependencies"></a>Dependencias de aplicaciones
Para configurar una aplicación de consola con el SDK de Bing Web Search, vaya a la opción Administrar paquetes NuGet del Explorador de soluciones en Visual Studio. Agregue:
* Microsoft.Azure.CognitiveServices.Search.VisualSearch
* Paquetes Microsoft.Azure.CognitiveServices.Search.ImageSearchpackage.

La instalación del paquete del SDK de NuGet Web Search también instala las dependencias, entre las que se incluyen:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="get-the-imageinsightstoken-from-image-search"></a>Obtener el token ImageInsightsToken de Image Search
En este ejemplo se utiliza un token `ImageInsightsToken` obtenido mediante el método siguiente.  Para obtener más información acerca de esta llamada, vea [Image Search SDK C# quickstart](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart) (Inicio rápido en C# del SDK de Image Search).

El código busca los resultados en una consulta de 'Canadian Rockies' y obtiene un token ImageInsightsToken. Imprime el token de Insights de la primera imagen, la dirección URL de la miniatura y la dirección URL del contenido de la imagen.  El método devuelve el token `ImageInsightsToken` para su uso en una solicitud de Visual Search.

```
        private static String ImageResults(String subKey)
        {
            String insightTok = "None";
            try
            {
                var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        insightTok = firstImageResult.ImageInsightsToken;
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");  
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        insightTok = "None found";
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }

            return insightTok;
        }
```

## <a name="specify-the-imageinsightstoken-for-visual-search-request"></a>Especificar el token ImageInsightsToken para la solicitud de Visual Search
En este ejemplo se usa el token de Insights devuelto por el método anterior. El siguiente código crea un objeto `ImageInfo` a partir del token `ImageInsightsToken` y carga el objeto ImageInfo en una solicitud `VisualSearchRequest`. Especifique `ImageInsightsToken` en un elemento `ImageInfo` para la solicitud `VisualSearchRequest`.

```
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```
## <a name="use-visual-search-to-find-images-from-an-imageinsightstoken"></a>Usar Visual Search para buscar imágenes desde un token ImageInsightsToken
La solicitud `VisualSearchRequest` contiene información sobre la imagen que se va a buscar en el objeto `ImageInfo`.  El método `VisualSearchMethodAsync` obtiene los resultados.
```
// An image binary is not necessary here, as the image is specified by insights token.
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
Console.WriteLine("\r\nVisual search request with knowledgeRequest");

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>Obtención de los datos de dirección URL de ImageModuleAction
Los resultados de Visual Search son objetos `ImageTag`.  Cada etiqueta contiene una lista de objetos `ImageAction`.  Cada elemento `ImageAction` contiene un campo `Data` que es una lista de valores que dependen del tipo de acción:

Puede obtener los distintos tipos con el código siguiente:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
La aplicación completa devuelve:

* ActionType: MoreSizes -> WebSearchUrl:
* ActionType: VisualSearch -> WebSearchUrl:
* ActionType: ImageById -> WebSearchUrl:
* ActionType: RelatedSearches -> WebSearchUrl:
* ActionType: DocumentLevelSuggestions -> WebSearchUrl:
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx,5823.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx,5831.1

Como se muestra en la lista anterior, los tipos `TopicResults` y `ImageResults` contienen consultas de imágenes relacionadas. Las direcciones URL de la lista vinculan a los resultados de Bing Search.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>Direcciones URL de PagesIncluding ActionType de las imágenes encontradas por Visual Search

Para la obtención de las direcciones URL de imágenes reales se requiere una conversión que lea un elemento `ActionType` como `ImageModuleAction`, que contenga un elemento `Data` con una lista de valores.  Cada valor es la dirección URL de una imagen.  El siguiente código convierte el tipo de acción `PagesIncluding` en `ImageModuleAction` y lee los valores.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```
Para obtener más información acerca de estos tipos de datos, vea [Images - Visual Search](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) (Imágenes: búsqueda visual).
## <a name="complete-code"></a>Código completo

El código siguiente ejecuta los ejemplos anteriores. Envía el token `ImageInsightsToken` en una solicitud POST. A continuación, imprime las direcciones URL de Bing Search para cada ActionType. Si ActionType es `PagesIncluding`, el código obtiene los elementos `ImageObject` de `Data`.  `Data` contiene una lista de valores, que son las direcciones URL de imágenes de las páginas web.  Copie las direcciones URL de Visual Search resultantes y péguelas en el explorador para mostrar los resultados. Copie los elementos ContentUrl y péguelos en el explorador para mostrar las imágenes.

```
using System;
using System.IO;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;

namespace VisualSearchFeatures
{
    class Program
    {
        static void Main(string[] args)
        {
            String subscriptionKey = "YOUR-ACCESS-KEY";

            insightsToken = ImageResults(subscriptionKey);

            VisualSearchInsightsToken(subscriptionKey, insightsToken);

            Console.WriteLine("Any key to quit...");
            Console.ReadKey();

        }

        // Searches for results on query (Canadian Rockies) and gets an ImageInsightsToken.
        // Also prints first image insights token, thumbnail url, and image content url.
        private static String ImageResults(String subKey)
        {
            String insightTok = "None";
            try
            {
                var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        insightTok = firstImageResult.ImageInsightsToken;
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");  
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        insightTok = "None found";
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }

            return insightTok;
        }


        // This method will get Visual Search results from an imageInsightsToken obtained from the return value of the ImageResults method.
        // The method includes imageInsightsToken the in a knowledgeRequest parameter, along with a cropArea object. 
        // It prints out the imageInsightsToken uploaded in the request.
        // Finally the example prints URLs of images found using the imageInsightsToken.

        public static void VisualSearchInsightsToken(string subscriptionKey, string insightsTok)
        {
            var client = new VisualSearchAPI(new Microsoft.Azure.CognitiveServices.Search.VisualSearch.ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via an insights token, in the ImageInfo object.
                ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);

                // An image binary is not necessary here, as the image is specified by insights token.
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with imageInsightsToken and knowledgeRequest");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {insightsTok}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    if (visualSearchResults.Tags.Count > 0)
                    {
                        // List of tags
                        foreach (ImageTag t in visualSearchResults.Tags)
                        {
                            foreach (ImageAction i in t.Actions)
                            {
                                Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " WebSearchURL: " + i.WebSearchUrl);

                                if (i.ActionType == "PagesIncluding")
                                {
                                    foreach (ImageObject o in (i as ImageModuleAction).Data.Value)
                                    {
                                        Console.WriteLine("ContentURL: " + o.ContentUrl);
                                    }
                                }
                            }
                        }
                    }

                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }

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
## <a name="next-steps"></a>Pasos siguientes
[Respuesta de Visual Search](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)
