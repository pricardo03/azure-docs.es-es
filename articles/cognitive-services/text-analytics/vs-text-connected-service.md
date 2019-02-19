---
title: 'Tutorial: Conexión al servicio Text Analytics con Servicios conectados en Visual Studio'
titleSuffix: Azure Cognitive Services
description: Aprenda cómo conectar Text Analytics desde una aplicación web ASP.NET Core.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: tutorial
ms.date: 02/13/2019
ms.author: aahi
ms.openlocfilehash: a37ea5fdad941e1559e470ccb22303a8e3c7ca3d
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243899"
---
# <a name="tutorial-connect-to-the-text-analytics-service-with-connected-services-in-visual-studio"></a>Tutorial: Conexión al servicio Text Analytics con Servicios conectados en Visual Studio

Al usar el servicio Text Analytics, puede extraer información muy completa para clasificar y procesar datos visuales y llevar a cabo una moderación automática de las imágenes con el fin de facilitar el mantenimiento de los servicios.

En este artículo y sus artículos complementarios se proporcionan detalles acerca del uso de la característica de servicios conectados de Visual Studio para el servicio Text Analytics. La funcionalidad está disponible en Visual Studio 2017 15.7 o posterior, con la extensión Cognitive Services instalada.

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene, puede registrarse para obtener una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).
- Visual Studio 2017 15.7 con la carga de trabajo de Desarrollo web instalada. [Descárguelo ahora](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).

[!INCLUDE [vs-install-cognitive-services-vsix](../../../includes/vs-install-cognitive-services-vsix.md)]

## <a name="add-support-to-your-project-for-the-text-analytics-service"></a>Adición de compatibilidad al proyecto para el servicio Text Analytics

1. Cree un nuevo proyecto de web ASP.NET Core denominado TextAnalyticsDemo. Utilice la plantilla de proyecto de aplicación web (Model-View-Controller) con todos los valores predeterminados. Es importante ponerle un nombre al proyecto MyWebApplication, para que el espacio de nombres coincida cuando copie código en el proyecto.  El ejemplo de este artículo utiliza MVC, pero puede utilizar el servicio conectado de Text Analytics con cualquier tipo de proyecto ASP.NET.

1. En el **Explorador de soluciones**, haga doble clic en elemento **Servicio conectado**.
   Aparece la página del servicio conectado con los servicios que puede agregar al proyecto.

   ![Captura de pantalla del servicio conectado en el Explorador de soluciones](../media/vs-common/Connected-Services-Solution-Explorer.PNG)

1. En el menú de servicios disponibles, elija **Evaluate Sentiment with Text Analytics** (Evaluar opiniones con Text Analytics).

   ![Captura de pantalla de la pantalla Servicios conectados](./media/vs-text-connected-service/Cog-Text-Connected-Service-0.PNG)

   Si ha iniciado sesión en Visual Studio y tiene una suscripción de Azure asociada a su cuenta, aparece una página con una lista desplegable con las suscripciones.

   ![Captura de pantalla de la pantalla Servicio conectado de Text Analytics](media/vs-text-connected-service/Cog-Text-Connected-Service-1.PNG)

1. Seleccione la suscripción que desea utilizar y, a continuación, elija un nombre para el servicio Text Analytics, o elija el vínculo **Editar** para modificar el nombre generado automáticamente, elija el grupo de recursos y el plan de tarifa.

   ![Captura de pantalla del grupo de recursos y los campos del plan de tarifa](media/vs-text-connected-service/Cog-Text-Connected-Service-2.PNG)

   Siga el vínculo para obtener más información sobre los planes de tarifa.

1. Elija **Agregar** para agregar compatibilidad con el servicio conectado.
   Visual Studio modifica su proyecto para agregar paquetes NuGet, las entradas del archivo de configuración y otros cambios para admitir una conexión con el servicio Text Analytics. La **ventana de salida** muestra el registro de lo que sucede en el proyecto. Debe ver algo parecido a lo siguiente:

   ```output
    [6/1/2018 3:04:02.347 PM] Adding Text Analytics to the project.
    [6/1/2018 3:04:02.906 PM] Creating new Text Analytics...
    [6/1/2018 3:04:06.314 PM] Installing NuGet package 'Microsoft.Azure.CognitiveServices.Language' version 1.0.0-preview...
    [6/1/2018 3:04:56.759 PM] Retrieving keys...
    [6/1/2018 3:04:57.822 PM] Updating appsettings.json setting: 'ServiceKey' = '<service key>'
    [6/1/2018 3:04:57.827 PM] Updating appsettings.json setting: 'ServiceEndPoint' = 'https://westus.api.cognitive.microsoft.com/text/analytics/v2.0'
    [6/1/2018 3:04:57.832 PM] Updating appsettings.json setting: 'Name' = 'TextAnalyticsDemo'
    [6/1/2018 3:05:01.840 PM] Successfully added Text Analytics to the project.
    ```
 
## <a name="use-the-text-analytics-service-to-detect-the-language-for-a-text-sample"></a>Usar el servicio Text Analytics para detectar el idioma de un ejemplo de texto.

1. Agregue lo siguiente usando las instrucciones de Startup.cs.
 
   ```csharp
   using System.IO;
   using System.Net.Http;
   using System.Net.Http.Headers;
   using System.Text;
   using Microsoft.Extensions.Configuration;
   ```
 
1. Agregue un campo de configuración y después un constructor que inicializa el campo de configuración de la clase Startup para habilitar la configuración del programa.

   ```csharp
      private IConfiguration configuration;

      public Startup(IConfiguration configuration)
      {
          this.configuration = configuration;
      }
   ```

1. Agregue un archivo de clase en la carpeta Controllers denominado DemoTextAnalyzeController y reemplace su contenido por el código siguiente:

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Extensions.Configuration;
    using TextAnalyticsDemo.Models;
    
    namespace TextAnalyticsDemo.Controllers
    {
        public class DemoTextAnalyzeController : Controller
        {
            private IConfiguration configuration;
    
            public DemoTextAnalyzeController(IConfiguration configuration)
            {
                this.configuration = configuration;
            }
    
            public IActionResult TextAnalyzeResult(TextAnalyzeModel model)
            {
    
                if (!string.IsNullOrWhiteSpace(model.TextStr))
                {
                    ITextAnalyticsAPI client = this.GetTextAnalyzeClient(new MyHandler());
                    model.AnalyzeResult = client.DetectLanguage(
                        new BatchInput(
                            new List<Input>()
                            {
                                new Input("id",model.TextStr)
                            }));
                }
                return View(model);
            }
    
            [HttpPost("Analyze")]
            public IActionResult Analyze(TextAnalyzeModel model)
            {
                return RedirectToAction("TextAnalyzeResult", model);
            }
    
            // Using the ServiceKey from the configuration file,
            // get an instance of the Text Analytics client.
            private ITextAnalyticsAPI GetTextAnalyzeClient(DelegatingHandler handler)
            {
                string key = configuration.GetSection("CognitiveServices")["TextAnalytics:ServiceKey"];
    
                ITextAnalyticsAPI client = new TextAnalyticsAPI( handlers: handler);
                client.SubscriptionKey = key;
                client.AzureRegion = AzureRegions.Westus;
    
                return client;
            }
        }
    }
    ```
    
    El código incluye GetTextAnalyzeClient para obtener el objeto cliente que puede usar para llamar a Text Analytics API y un controlador de peticiones que llama a DetectLanguage en un texto dado.

1. Agregue la clase auxiliar MyHandler que se utiliza por el código anterior.

    ```csharp
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

1. En la carpeta Models, agregue una clase para el modelo.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    
    namespace Demo.Models
    {
        public class TextAnalyzeModel
        {
            public string TextStr { get; set; }
    
            public LanguageBatchResult AnalyzeResult { get; set; }
    
            public SentimentBatchResult AnalyzeResult2 { get; set; }
        }
    }
    ```

1. Agregue una vista para mostrar el texto analizado, el lenguaje determinado y la puntuación que representa el nivel de confianza en el análisis. Para ello, haga doble clic en la carpeta **Views**, elija **Agregar** y, después, **Vista**. En el cuadro de diálogo que aparece, asígnele el nombre _TextAnalyzeResult_, acepte los valores predeterminados para agregar un nuevo archivo llamado _TextAnalyzeResult.cshtml_ en la carpeta **Views** y copie el siguiente contenido en él:
    
    ```cshtml
    @using System
    @model Demo.Models.TextAnalyzeModel
    
    @{
        ViewData["Title"] = "TextAnalyzeResult";
    }
    
    <h2>Text Language</h2>
    
    <div class="row">
        <section>
            <form asp-controller="DemoTextAnalyze" asp-action="Analyze" method="POST"
                  class="form-horizontal" enctype="multipart/form-data">
                <table width="90%">
                    <tr>
                        <td>
                            <input type="text" name="TextStr" class="form-control" />
                        </td>
                        <td>
                            <button type="submit" class="btn btn-default">Analyze</button>
                        </td>
                    </tr>
                </table>
            </form>
        </section>
    </div>
    
    <h2>Result</h2>
    <div>
        <dl class="dl-horizontal">
            <dt>
                Text :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.TextStr)
            </dd>
            <dt>
                Language Name :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.AnalyzeResult.Documents[0].DetectedLanguages[0].Name)
            </dd>
            <dt>
                Score :
            </dt>
            <dd>
                @Html.DisplayFor(model => model.AnalyzeResult.Documents[0].DetectedLanguages[0].Score)
            </dd>
        </dl>
    </div>
    <div>
        <hr />
        <p>
            <a asp-controller="Home" asp-action="Index">Return to Index</a>
        </p>
    </div>
    
    ```
 
1. Cree y ejecute la aplicación localmente. Escriba algún texto y compruebe qué idioma detecta Text Analytics.
   
## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no necesite el grupo de recursos, elimínelo. De esta forma, se eliminan la instancia de Cognitive Services y los recursos relacionados. Para eliminar el grupo de recursos mediante el portal:

1. Escriba el nombre del grupo de recursos en el cuadro de búsqueda de la parte superior del portal. Cuando vea el grupo de recursos que se utiliza en este tutorial en los resultados de búsqueda, selecciónelo.
2. Seleccione **Eliminar grupo de recursos**.
3. En el cuadro **ESCRIBA EL NOMBRE DEL GRUPO DE RECURSOS:** escriba el nombre del grupo de recursos y seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre el servicio Text Analytics, lea la [documentación del servicio Text Analytics](index.yml).
