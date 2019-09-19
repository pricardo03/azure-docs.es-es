---
title: Uso de la inserción de dependencias en Azure Functions con .NET
description: Aprenda a usar la inserción de dependencias para el registro y uso de servicios en las funciones de .NET.
services: functions
documentationcenter: na
author: craigshoemaker
manager: gwallace
keywords: funciones de azure, funciones, arquitectura sin servidor
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/28/2019
ms.author: cshoe
ms.reviewer: jehollan
ms.openlocfilehash: e31f3dc166177ce36289b97d85d90a9582c9cae5
ms.sourcegitcommit: aebe5a10fa828733bbfb95296d400f4bc579533c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2019
ms.locfileid: "70375995"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>Uso de la inserción de dependencias en Azure Functions con .NET

Azure Functions admite el modelo de diseño de software de inserción de dependencias (DI), que es una técnica para lograr la [inversión de control (IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre las clases y sus dependencias.

Azure Functions se basa en las características de inserción de dependencias en ASP.NET Core. Se recomienda tener en cuenta los servicios, la duración y los modelos de diseño de la [inserción de dependencias en Core](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) antes de usar las características de DI en una aplicación de Azure Functions.

La compatibilidad con la inserción de dependencias comienza con Azure Functions 2.x.

## <a name="prerequisites"></a>Requisitos previos

Para poder usar la inserción de dependencias, debe instalar los siguientes paquetes NuGet:

- [Microsoft.Azure.Functions.Extensions](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- Versión 1.0.28 o posterior del [paquete de Microsoft.NET.Sdk.Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/)

- Opcional: [Microsoft.Extensions.Http](https://www.nuget.org/packages/Microsoft.Extensions.Http/) solo se necesita para registrar HttpClient al inicio

## <a name="register-services"></a>Registro de los servicios

Para registrar los servicios, puede crear un método para configurar y agregar componentes a una instancia de `IFunctionsHostBuilder`.  El host de Azure Functions crea una instancia de `IFunctionsHostBuilder` y la pasa directamente al método.

Para registrar el método, agregue el atributo de ensamblado `FunctionsStartup` que especifica el nombre del tipo usado durante el inicio. El código también hace referencia a una versión preliminar de [Microsoft.Azure.Cosmos](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) en Nuget.

```csharp
using System;
using Microsoft.Azure.Functions.Extensions.DependencyInjection;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Http;
using Microsoft.Extensions.Logging;
using Microsoft.Azure.Cosmos;

[assembly: FunctionsStartup(typeof(MyNamespace.Startup))]

namespace MyNamespace
{
    public class Startup : FunctionsStartup
    {
        public override void Configure(IFunctionsHostBuilder builder)
        {
            builder.Services.AddHttpClient();
            builder.Services.AddSingleton((s) => {
                return new CosmosClient(Environment.GetEnvironmentVariable("COSMOSDB_CONNECTIONSTRING"));
            });
            builder.Services.AddSingleton<ILoggerProvider, MyLoggerProvider>();
        }
    }
}
```

## <a name="use-injected-dependencies"></a>Uso de dependencias insertadas

ASP.NET Core usa la inserción de constructores para que las dependencias estén disponibles para la función. El ejemplo siguiente muestra cómo las dependencias `IMyService` y `HttpClient` se insertan en una función desencadenada por HTTP. 

```csharp
using System;
using System.IO;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Azure.WebJobs;
using Microsoft.Azure.WebJobs.Extensions.Http;
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Logging;
using Newtonsoft.Json;

namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly IMyService _service;
        private readonly HttpClient _client;

        public HttpTrigger(IMyService service, IHttpClientFactory httpClientFactory)
        {
            _service = service;
            _client = httpClientFactory.CreateClient();
        }

        [FunctionName("GetPosts")]
        public async Task<IActionResult> Get(
            [HttpTrigger(AuthorizationLevel.Function, "get", Route = "posts")] HttpRequest req,
            ILogger log)
        {
            log.LogInformation("C# HTTP trigger function processed a request.");
            var res = await _client.GetAsync("https://microsoft.com");
            await _service.AddResponse(res);

            return new OkResult();
        }
    }
}
```

El uso de la inserción de constructores significa que no debe usar funciones estáticas si quiere aprovechar las ventajas de la inserción de dependencias. Para el cliente de cosmos, consulte [esto](https://github.com/Azure/azure-cosmos-dotnet-v3/blob/master/Microsoft.Azure.Cosmos.Samples/CodeSamples/AzureFunctions/AzureFunctionsCosmosClient.cs).

## <a name="service-lifetimes"></a>Vigencia de los servicios

Las aplicaciones de Azure Functions proporcionan la misma vigencia de servicio que la [inserción de dependencias de ASP.NET](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes): transitoria, de ámbito y singleton.

En una aplicación de Functions, una vigencia de servicio de ámbito coincide con una vigencia de ejecución de la función. Los servicios de ámbito se crean una vez por cada ejecución. Las solicitudes posteriores para ese servicio durante la ejecución vuelven a usar la instancia de servicio existente. Una vigencia singleton del servicio coincide con la vigencia del host y se reutiliza en ejecuciones de la función en esa instancia.

Los servicios de vigencia singleton se recomiendan para conexiones y clientes, por ejemplo, para instancias `SqlConnection`, `CloudBlobClient` o `HttpClient`.

Consulte o descargue un [ejemplo de vigencia de servicio diferente](https://aka.ms/functions/di-sample) en GitHub.

## <a name="logging-services"></a>Registro de servicios

Si tiene su propio proveedor de registro, el método recomendado consiste en registrar una instancia `ILoggerProvider`. Azure Functions agrega Application Insights automáticamente.

> [!WARNING]
> No agregue `AddApplicationInsightsTelemetry()` a la colección de servicios, ya que registra los servicios que entran en conflicto con los servicios proporcionados por el entorno.

## <a name="function-app-provided-services"></a>Servicios proporcionados por la aplicación de funciones

El host de la función registra muchos servicios. Los siguientes servicios son seguros para tomar como dependencia en la aplicación:

|Tipo de servicio|Vigencia|DESCRIPCIÓN|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|Singleton|Configuración del entorno en tiempo de ejecución|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|Singleton|Responsable de proporcionar el id. de la instancia de host|

Si hay otros servicios en los que quiere tomar una dependencia, [cree un problema y propóngalo en GitHub](https://github.com/azure/azure-functions-host).

### <a name="overriding-host-services"></a>Invalidación de los servicios de host

La invalidación de los servicios proporcionados por el host no se admite.  Si hay otros servicios que quiere invalidar, [cree un problema y propóngalo en GitHub](https://github.com/azure/azure-functions-host).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

- [Cómo supervisar la aplicación de funciones](functions-monitoring.md)
- [Procedimientos recomendados para Functions](functions-best-practices.md)
