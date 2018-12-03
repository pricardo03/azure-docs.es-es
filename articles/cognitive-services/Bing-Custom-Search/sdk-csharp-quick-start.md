---
title: 'Guía de inicio rápido: SDK de Custom Search con C#'
titleSuffix: Azure Cognitive Services
description: Configuración de la aplicación de consola del SDK de Custom Search para C#.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 09/06/2018
ms.author: scottwhi
ms.openlocfilehash: 7ac298ad5c5b93b5dce0ce2dd59ffe541888db88
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "52307770"
---
# <a name="quickstart-using-the-bing-custom-search-sdk-with-c"></a>Guía de inicio rápido: Uso del SDK de Bing Custom Search con C#

Bing Custom Search SDK proporciona un modelo de programación más fácil que Bing Custom Search REST API. Esta sección le indicará cómo realizar las primeras llamadas a Custom Search con el SDK para C#.

## <a name="prerequisites"></a>Requisitos previos

Para completar este inicio rápido necesita instalar:

- Una instancia de Custom Search lista para usar. Consulte [Create your first Bing Custom Search instance](quick-start.md) (Creación de la primera instancia de Bing Custom Search).  
  
- Una clave de suscripción. Puede obtener una clave de suscripción cuando active su [evaluación gratuita](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search) o puede utilizar una clave de suscripción de pago desde el panel de Azure (consulte [Cuenta de la API de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account)).  Consulte también [Precios de Cognitive Services - Bing Search API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).
  
- Visual Studio 2017 instalado. Si aún no lo ha hecho, descargue e instale la **versión gratuita** de [Visual Studio 2017 Community Edition](https://www.visualstudio.com/downloads/).  
  
- El paquete [NuGet Custom Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) instalado. En el Explorador de soluciones en Visual Studio, haga clic con el botón derecho en el proyecto y seleccione `Manage NuGet Packages` en el menú. Instale el paquete `Microsoft.Azure.CognitiveServices.Search.CustomSearch`.

Al instalar el paquete NuGet Custom Search, también se instalarán los ensamblados siguientes:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json



## <a name="run-the-code"></a>Ejecución del código

> [!TIP]
> Obtenga el código más reciente como solución de Visual Studio desde [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingCustomWebSearch).

Para ejecutar el ejemplo, siga estos pasos:

1. Abra Visual Studio 2017.
  
2. Haga clic en el menú **Archivo**, haga clic en **Nuevo**, **Proyecto** y, a continuación, haga clic en la plantilla **Aplicación de consola de Visual C#**.
  
3. Asigne a la solución el nombre CustomSearchSolution y vaya a la carpeta donde se va a poner.
  
4. Haga clic en Aceptar para crear la solución.  
  
4. En el Explorador de soluciones, haga clic con el botón derecho en el proyecto (tiene el mismo nombre que la solución) y seleccione `Manage NuGet Packages`. En el Administrador de paquetes NuGet, haga clic en **Examinar**. Escriba Microsoft.Azure.CognitiveServices.Search.CustomSearch en el cuadro de búsqueda y presione ENTRAR. Seleccione el paquete y haga clic en Instalar.  
  
4. Copie el código siguiente en el archivo Program.cs. Reemplace los valores de **YOUR-SUBSCRIPTION-KEY** y **YOUR-CUSTOM-CONFIG-ID** por su clave de suscripción y su identificador de configuración.  
  
    ```csharp
    using System;
    using System.Linq;
    using Microsoft.Azure.CognitiveServices.Search.CustomSearch;

    namespace CustomSrchSDK
    {
        class Program
        {
            static void Main(string[] args)
            {

                var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-SUBSCRIPTION-KEY"));

                try
                {
                    // This will look up a single query (Xbox).
                    var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
                    Console.WriteLine("Searched for Query# \" Xbox \"");

                    //WebPages
                    if (webData?.WebPages?.Value?.Count > 0)
                    {
                        // find the first web page
                        var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

                        if (firstWebPagesResult != null)
                        {
                            Console.WriteLine("Number of webpage results {0}", webData.WebPages.Value.Count);
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
                }
                catch (Exception ex)
                {
                    Console.WriteLine("Encountered exception. " + ex.Message);
                }

                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

        }
    }
    ```  
  
5. Compile y ejecute (depure) la solución. 




## <a name="breaking-it-down"></a>Desglose

Después de instalar el paquete NuGet Custom Search, deberá agregar una directiva using para él.

```csharp
using Microsoft.Azure.CognitiveServices.Search.CustomSearch;
```

Después, cree una instancia de Custom Search, que usará para realizar las solicitudes de búsqueda. Asegúrese de reemplazar `YOUR-SUBSCRIPTION-KEY` por su clave.

```csharp
var client = new CustomSearchAPI(new ApiKeyServiceClientCredentials("YOUR-CUSTOM-SEARCH-KEY"));
```

Ahora puede usar al cliente para enviar una solicitud de búsqueda. No olvide reemplazar `YOUR-CUSTOM-CONFIG-ID` por el identificador de configuración de la instancia (encontrará el identificador en el [portal de Custom Search](https://www.customsearch.ai/)). Este ejemplo busca Xbox. Actualice según sea necesario.

```csharp
var webData = client.CustomInstance.SearchAsync(query: "Xbox", customConfig: Int32.Parse("YOUR-CUSTOM-CONFIG-ID")).Result;
```

El método `SearchAsync` devuelve un objeto `WebData`. Use `WebData` para iterar por cualquier `WebPages` que se encuentre. Este código busca el primer resultado de página web e imprime los valores de `Name` y `URL` de la página web.

```csharp
//WebPages
if (webData?.WebPages?.Value?.Count > 0)
{
    // find the first web page
    var firstWebPagesResult = webData.WebPages.Value.FirstOrDefault();

    if (firstWebPagesResult != null)
    {
        Console.WriteLine("Webpage Results#{0}", webData.WebPages.Value.Count);
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

```


## <a name="next-steps"></a>Pasos siguientes

Consulte los ejemplos de SDK. Cada ejemplo incluye un archivo Léame con detalles sobre los requisitos previos y la instalación y ejecución de los ejemplos.

* Introducción a [ejemplos de .NET](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7) 
    * [Paquete NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0)
    * Consulte también [bibliotecas de .NET](https://github.com/Azure/azure-sdk-for-net/tree/psSdkJson6/src/SDKs/CognitiveServices/dataPlane/Search/BingCustomSearch) para las definiciones y dependencias.
* Introducción a [ejemplos de NodeJS](https://github.com/Azure-Samples/cognitive-services-node-sdk-samples) 
    * Consulte también [bibliotecas de NodeJS](https://github.com/Azure/azure-sdk-for-node/tree/master/lib/services/customSearch) para las definiciones y dependencias.
* Introducción a [ejemplos de Java](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples) 
    * Consulte también [bibliotecas de Java](https://github.com/Azure/azure-sdk-for-java/tree/master/cognitiveservices/azure-customsearch) para las definiciones y dependencias.
* Introducción a [ejemplos de Python](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples) 
    * Consulte también [bibliotecas de Python](https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-search-customsearch) para las definiciones y dependencias.

