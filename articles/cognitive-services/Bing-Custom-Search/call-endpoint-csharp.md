---
title: Llamada a un punto de conexión con C# - Bing Custom Search - Microsoft Cognitive Services
description: Esta guía de inicio rápido muestra cómo solicitar los resultados de la búsqueda a la instancia de búsqueda personalizada usando C# para llamar al punto de conexión de Bing Custom Search.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 05/07/2018
ms.author: v-brapel
ms.openlocfilehash: 87970e1c5e8487f9afca2acc680bdfeb610dc89f
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/18/2018
ms.locfileid: "41931343"
---
# <a name="call-bing-custom-search-endpoint-c"></a>Llamada a un punto de conexión de Bing Custom Search (C#)

Esta guía de inicio rápido muestra cómo solicitar los resultados de la búsqueda a la instancia de búsqueda personalizada usando C# para llamar al punto de conexión de Bing Custom Search. 

## <a name="prerequisites"></a>Requisitos previos

-  Una instancia de Custom Search lista para usar. Consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).
-  [.NET Core](https://www.microsoft.com/net/download/core) instalado.
- Una [cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) con **Bing Search APIs**. La [cuenta de evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) es suficiente para esta guía de inicio rápido. Necesita la clave de acceso que se le proporciona al activar la versión de evaluación gratuita, o puede usar una clave de suscripción de pago desde su panel de Azure.  

  >[!NOTE]  
  >Los usuarios de Bing Custom Search existentes que tengan una clave de versión preliminar aprovisionada el 15 de octubre de 2017 (o antes) podrán usar sus claves hasta el 30 de noviembre de 2017, o hasta que agoten el número máximo de consultas permitidas. Después de esta fecha tendrán que migrar a la versión de disponibilidad general en Azure. 
 
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

4. Copie el código siguiente a Program.cs.
5. Reemplace los valores de **YOUR-SUBSCRIPTION-KEY** y **YOUR-CUSTOM-CONFIG-ID** por su clave y su identificador de configuración.

    ``` CSharp
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
6. Compile la aplicación con el comando siguiente. Anote la ruta de acceso del archivo dll al que hace referencia la salida del comando.

    <pre>
    dotnet build 
    </pre>
7. Ejecute la aplicación utilizando el comando siguiente reemplazando **PATH TO OUTPUT** por la ruta de acceso a la que hace referencia el paso de compilación.

    <pre>    
    dotnet **PATH TO OUTPUT**
    </pre>

## <a name="next-steps"></a>Pasos siguientes
- [Configuración de la experiencia de interfaz de usuario hospedada](./hosted-ui.md)
- [Uso de marcadores de decoración para resaltar texto](./hit-highlighting.md)
- [Paginación de páginas web](./page-webpages.md)
