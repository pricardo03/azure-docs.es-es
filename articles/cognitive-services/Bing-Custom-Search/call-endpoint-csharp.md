---
title: 'Guía de inicio rápido: Llamada al punto de conexión mediante C# en Bing Custom Search'
titlesuffix: Azure Cognitive Services
description: Esta guía de inicio rápido muestra cómo solicitar los resultados de la búsqueda a la instancia de búsqueda personalizada usando C# para llamar al punto de conexión de Bing Custom Search.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 1c3b1031c2d08b1f346216b54d351c99f01db933
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167314"
---
# <a name="quickstart-call-bing-custom-search-endpoint-c"></a>Guía de inicio rápido: Llamada al punto de conexión de Bing Custom Search (C#)

Esta guía de inicio rápido muestra cómo solicitar los resultados de la búsqueda a la instancia de búsqueda personalizada usando C# para llamar al punto de conexión de Bing Custom Search. 

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido necesita instalar:

- Una instancia de Custom Search lista para usar. Consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).
- [.NET Core](https://www.microsoft.com/net/download/core) instalado.
- Una clave de suscripción. Puede obtener una clave de suscripción cuando active su [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) o puede utilizar una clave de suscripción de pago desde el panel de Azure (consulte [Cuenta de la API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).    


## <a name="run-the-code"></a>Ejecución del código

Para ejecutar el ejemplo, siga estos pasos:

1. Cree una carpeta para el código.  
  
2. Desde un terminal o símbolo del sistema, vaya a la carpeta que acaba de crear.  
  
3. Ejecute los comandos siguientes:
    ```
    dotnet new console -o BingCustomSearch
    cd BingCustomSearch
    dotnet add package Newtonsoft.Json
    dotnet restore
    ```
  
4. Copie el código siguiente a Program.cs. Reemplace los valores de **YOUR-SUBSCRIPTION-KEY** y **YOUR-CUSTOM-CONFIG-ID** por su clave de suscripción y su identificador de configuración.

    ```csharp
    using System;
    using System.Net.Http;
    using System.Web;
    using Newtonsoft.Json;
    
    namespace bing_custom_search_example_dotnet
    {
        class Program
        {
            static void Main(string[] args)
            {
                var subscriptionKey = "YOUR-SUBSCRIPTION-KEY";
                var customConfigId = "YOUR-CUSTOM-CONFIG-ID";
                var searchTerm = args.Length > 0 ? args[0]: "microsoft";            
    
                var url = "https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?" +
                    "q=" + searchTerm +
                    "&customconfig=" + customConfigId;
    
                var client = new HttpClient();
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
                var httpResponseMessage = client.GetAsync(url).Result;
                var responseContent = httpResponseMessage.Content.ReadAsStringAsync().Result;
                BingCustomSearchResponse response = JsonConvert.DeserializeObject<BingCustomSearchResponse>(responseContent);
                
                for(int i = 0; i < response.webPages.value.Length; i++)
                {                
                    var webPage = response.webPages.value[i];
                    
                    Console.WriteLine("name: " + webPage.name);
                    Console.WriteLine("url: " + webPage.url);                
                    Console.WriteLine("displayUrl: " + webPage.displayUrl);
                    Console.WriteLine("snippet: " + webPage.snippet);
                    Console.WriteLine("dateLastCrawled: " + webPage.dateLastCrawled);
                    Console.WriteLine();
                }            
            }
        }
    
        public class BingCustomSearchResponse
        {        
            public string _type{ get; set; }            
            public WebPages webPages { get; set; }
        }
    
        public class WebPages
        {
            public string webSearchUrl { get; set; }
            public int totalEstimatedMatches { get; set; }
            public WebPage[] value { get; set; }        
        }
    
        public class WebPage
        {
            public string name { get; set; }
            public string url { get; set; }
            public string displayUrl { get; set; }
            public string snippet { get; set; }
            public DateTime dateLastCrawled { get; set; }
            public string cachedPageUrl { get; set; }
            public OpenGraphImage openGraphImage { get; set; }        
        }
        
        public class OpenGraphImage
        {
            public string contentUrl { get; set; }
            public int width { get; set; }
            public int height { get; set; }
        }
    }
    ```
6. Compile la aplicación con el comando siguiente. Anote la ruta de acceso del archivo DLL al que hace referencia la salida del comando.

    <pre>
    dotnet build 
    </pre>
    
7. Ejecute la aplicación utilizando el comando siguiente y reemplazando **PATH TO OUTPUT** por la ruta de acceso del archivo DLL a la que se hace referencia en el paso 6.

    <pre>    
    dotnet **PATH TO OUTPUT**
    </pre>

## <a name="next-steps"></a>Pasos siguientes
- [Configuración de la experiencia de interfaz de usuario hospedada](./hosted-ui.md)
- [Uso de marcadores de decoración para resaltar texto](./hit-highlighting.md)
- [Paginación de páginas web](./page-webpages.md)
