---
title: Conexión a Bing News Search API con Servicios conectados en Visual Studio y C#
titleSuffix: Azure Cognitive Services
description: Conéctese a Bing News Search con una aplicación web de ASP.NET Core.
services: cognitive-services
author: ghogen
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: tutorial
ms.date: 06/18/2019
ms.author: ghogen
ms.openlocfilehash: 85afae087b1b1e572759943142412743744ee806
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203422"
---
# <a name="tutorial-connect-to-bing-news-search-api-with-connected-services-in-visual-studio-and-c"></a>Tutorial: Conexión a Bing News Search API con Servicios conectados en Visual Studio y C#

Con el uso de Bing News Search, puede permitir que aplicaciones y servicios aprovechen la eficacia de un motor de búsqueda sin anuncios con ámbito en la Web. Bing News Search es uno de los servicios de búsqueda disponibles con Cognitive Services.

En este artículo se proporcionan detalles para utilizar la característica Servicio conectado de Visual Studio para Bing News Search. La funcionalidad está disponible en Visual Studio 2017 15.7 o posterior, con la extensión Cognitive Services instalada.

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- Visual Studio 2019 con la carga de trabajo Desarrollo web instalada. [Descárguelo ahora](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-bing-news-search-api"></a>Adición de compatibilidad a un proyecto para Bing News Search API

1. Cree un proyecto web ASP.NET Core denominado MyWebApplication. Utilice la plantilla de proyecto **Aplicación web (controlador de vista de modelos)** con todos los valores predeterminados. Es importante ponerle un nombre al proyecto MyWebApplication, para que el espacio de nombres coincida cuando copie código en el proyecto. 

1. En el **Explorador de soluciones**, elija **Agregar** > **Servicio conectado**.
   Aparece la página del servicio conectado con los servicios que puede agregar al proyecto.

   ![Captura de pantalla del elemento de menú Agregar servicio conectado](../media/vs-common/Connected-Service-Menu.PNG)

1. En el menú de servicios disponibles, elija **Bring Intelligent Search To Your Apps** (Incorporar la búsqueda inteligente a las aplicaciones).

   ![Captura de pantalla de la lista de servicios conectados](./media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-0.PNG)

   Si ha iniciado sesión en Visual Studio y tiene una suscripción de Azure asociada a su cuenta, aparece una página con una lista desplegable con las suscripciones. Seleccione la suscripción que desea utilizar y después elija un nombre para Bing News Search API. También puede elegir **Editar** para modificar el nombre generado automáticamente.

   ![Captura de pantalla de los campos de nombre y suscripción](media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-1.PNG)

1. Elija el grupo de recursos y el plan de tarifa.

   ![Captura de pantalla de los campos de grupo de recursos y plan de tarifa](media/vs-bing-news-search-connected-service/Cog-Search-Connected-Service-2.PNG)

   Si desea obtener más información sobre los planes de tarifa, seleccione **Review pricing** (Revisar precios).

1. Elija **Agregar** para agregar compatibilidad con el servicio conectado.
   Visual Studio modifica su proyecto para agregar paquetes NuGet, las entradas del archivo de configuración y otros cambios para admitir una conexión con Bing News Search API. La salida muestra el registro de lo que sucede en el proyecto. Debe ver algo parecido a lo siguiente:

   ```output
   [5/4/2018 12:41:21.084 PM] Adding Intelligent Search to the project.
   [5/4/2018 12:41:21.271 PM] Creating new Intelligent Search...
   [5/4/2018 12:41:24.128 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.ImageSearch' version 1.2.0...
   [5/4/2018 12:41:24.135 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.NewsSearch' version 1.2.0...
   [5/4/2018 12:41:24.154 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.WebSearch' version 1.2.0...
   [5/4/2018 12:41:24.168 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.CustomSearch' version 1.2.0...
   [5/4/2018 12:41:24.187 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Search.VideoSearch' version 1.2.0...
   [5/4/2018 12:42:07.287 PM] Retrieving keys...
   [5/4/2018 12:42:07.741 PM] Updating appsettings.json setting: 'ServiceKey' = 'c271412f3e4c4e1dacc7c4145fa0572a'
   [5/4/2018 12:42:07.745 PM] Updating appsettings.json setting: 'ServiceEndPoint' = 'https://api.cognitive.microsoft.com/bing/v7.0'
   [5/4/2018 12:42:07.749 PM] Updating appsettings.json setting: 'Name' = 'WebApplicationCore-Search_IntelligentSearch'
   [5/4/2018 12:42:10.217 PM] Successfully added Intelligent Search to the project.
   ```

   El archivo appsettings.json ahora contiene las siguientes opciones nuevas:

   ```json
   "CognitiveServices": {
     "IntelligentSearch": {
       "ServiceKey": "<your service key>",
       "ServiceEndPoint": "https://api.cognitive.microsoft.com/bing/v7.0",
       "Name": "WebApplicationCore-Search_IntelligentSearch"
     }
   }
   ```
 
## <a name="use-the-bing-news-search-api-to-add-search-functionality-to-a-web-page"></a>Uso de Bing News Search API para agregar la funcionalidad de búsqueda a una página web

Ahora que ha agregado compatibilidad con Bing News Search API al proyecto, a continuación le explicamos cómo usar la API para agregar la búsqueda inteligente a una página web.

1. En *Startup.cs*, en el método `ConfigureServices`, agregue una llamada a `IServiceCollection.AddSingleton`. Esto hace que el objeto de configuración que contiene la configuración clave esté disponible para el código del proyecto.
 
   ```csharp
       public void ConfigureServices(IServiceCollection services)
       {
           services.AddMvc();
           services.AddSingleton<IConfiguration>(Configuration);
       }
   ```


1. Agregue un nuevo archivo de clase en la carpeta **Modelos**, denominado *BingNewsModel.cs*. Si asigna al proyecto un nombre diferente, use su propio espacio de nombres para el proyecto, en lugar de MyWebApplication. Reemplace el contenido por el código siguiente:
 
    ```csharp
    using Microsoft.Azure.CognitiveServices.Search.NewsSearch.Models;
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    
    namespace MyWebApplication.Models
    {
        public class BingNewsModel
        {
            public News SearchResult { get; set; } 
            public string SearchText { get; set; }
        }
    }
    ```

   Este modelo se usa para almacenar los resultados de una llamada al servicio Bing News Search.
 
1. En la carpeta **Controladores**, agregue un nuevo archivo de clase denominado *IntelligentSearchController.cs*. Reemplace el contenido por el código siguiente:

   ```csharp
    using System.Net.Http;
    using System.Threading.Tasks;
    using MyWebApplication.Models;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.CognitiveServices.Search.NewsSearch;
    using Microsoft.Extensions.Configuration;
    
    namespace MyWebApplication.Controllers
    {
        // A controller to handle News Search requests
        public class IntelligentSearchController : Controller
        {
            private IConfiguration configuration;
  
            // Set up the configuration that contains the keys
            // (from the appsettings.json file)
            // that you will use to access the service  
            public IntelligentSearchController(IConfiguration configuration)
            {
                this.configuration = configuration;
            }

            // Call the Bing News Search API and put the result in the model object.    
            public async Task<IActionResult> BingSearchResult(BingNewsModel model)
            {
                if (!string.IsNullOrWhiteSpace(model.SearchText))
                { 
                    INewsSearchAPI client = this.GetNewsSearchClient(new MyHandler());
                    model.SearchResult = await client.News.SearchAsync(model.SearchText);
                }
                return View(model);
            }
    
            // Forward requests to the Search endpoint to the BingSearchResult method
            [HttpPost("Search")]
            public IActionResult Search(BingNewsModel model)
            {
                return RedirectToAction("BingSearchResult", model);
            }

            // Get the search client object
            private INewsSearchAPI GetNewsSearchClient(DelegatingHandler handler)
            {
                string key =
                   configuration.GetSection("CognitiveServices")["IntelligentSearch:ServiceKey"];
    
                INewsSearchAPI client = new NewsSearchAPI(
                   new ApiKeyServiceClientCredentials(key), handlers: handler);
    
                return client;
            }
        }
    }
   ```

   En este código, el constructor configura el objeto de configuración que contiene las claves. El método para la ruta `Search` simplemente es un redireccionamiento a la función `BingSearchResult`. Llama al método `GetNewsSearchClient` para obtener el objeto de cliente `NewsSearchAPI`.  El objeto de cliente `NewsSearchAPI` contiene el método `SearchAsync`, que en realidad llama al servicio y devuelve los resultados en el modelo `SearchResult` que acaba de crear. 

1. Agregue una clase, `MyHandler`, a la que se hizo referencia en el código anterior. Esto delega la llamada asincrónica al servicio de búsqueda en su clase raíz, `DelegatingHandler`.

   ```csharp
    using System.Net.Http;
    using System.Threading.Tasks;
    using System.Threading;

    class MyHandler : DelegatingHandler
    {
        protected async override Task<HttpResponseMessage> SendAsync(
        HttpRequestMessage request, CancellationToken cancellationToken)
        {
            // Call the inner handler.
            var response = await base.SendAsync(request, cancellationToken);
            
            return response;
        }
    }
   ```

1. Para agregar compatibilidad para enviar las búsquedas y ver los resultados, en la carpeta **Vistas**, cree una carpeta denominada **IntelligentSearch**. En esta nueva carpeta, agregue una vista *BingSearchResult.cshtml*. Copie el siguiente código:

    ```cshtml
    @using System
    @model MyWebApplication.Models.BingNewsModel
    
    @{
        ViewData["Title"] = "BingSearchResult";
    }
    
    <h2>Search News</h2>
    
    <div class="row">
        <section>
            <form asp-controller="IntelligentSearch" asp-action="Search" method="POST"
                  class="form-horizontal" enctype="multipart/form-data">
                <table width ="90%">
                    <tr>
                        <td>
                            <input type="text" name="SearchText" class="form-control" />
                        </td>
                        <td>
                            <button type="submit" class="btn btn-default">Search</button>
                        </td>
                    </tr>
                </table>
            </form>
        </section>
    </div>
    <h2>Search Result</h2=
    <table>
    @if (!string.IsNullOrEmpty(Model.SearchText)) {
        foreach (var item in Model.SearchResult.Value) {
        <tr>
            <td rowspan="2" width="90">
               <img src=@item?.Image?.Thumbnail?.ContentUrl width="80" height="80" />
            </td>
            <td><a href=@item.Url>@item.Name</a></td>
        </tr>   
        <tr>
            <td>@item.Description</td>
        </tr>
        <tr height="10">
            <td/><td/>
         </tr>
        } }
     </table>
    <div>
        <hr />
        <p>
            <a asp-controller="Home" asp-action="Index">Return to Index</a>
        </p>
    </div>
    ```

1. Inicie la aplicación web localmente, escriba la dirección URL de la página que acaba de crear (/IntelligentSearch/BingSearchResult) y publique una solicitud de búsqueda mediante el botón de búsqueda.

   ![Captura de pantalla de los resultados de Bing News Search](media/vs-bing-news-search-connected-service/Cog-News-Search-Results.PNG)
           
## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando el grupo de recursos ya no sea necesario, puede eliminarlo. De esta forma, se eliminan la instancia de Cognitive Services y los recursos relacionados. Para eliminar el grupo de recursos mediante el portal:

1. Escriba el nombre del grupo de recursos en el cuadro de búsqueda de la parte superior del portal. Seleccione el grupo de recursos que desea eliminar.
2. Seleccione **Eliminar grupo de recursos**.
3. En el cuadro **Escriba el nombre del grupo de recursos**, escriba el nombre del grupo de recursos y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Bing News Search API, vea [¿Qué es Bing News Search?](index.yml)
