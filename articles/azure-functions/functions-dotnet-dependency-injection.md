---
title: Usar inserción de dependencias en .NET de Azure Functions
description: Aprenda a usar la inserción de dependencias para el registro y uso de servicios en las funciones de .NET
services: functions
documentationcenter: na
author: craigshoemaker
manager: jeconnoc
keywords: funciones de azure, funciones, arquitectura sin servidor
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 05/28/2019
ms.author: jehollan, glenga, cshoe
ms.openlocfilehash: b1a6751f0d788c26af60b28eee994dc9b3877f00
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693255"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>Usar inserción de dependencias en .NET de Azure Functions

Azure Functions admite el patrón de diseño de dependencia (DI) de inyección software, que es una técnica para lograr [inversión de Control (IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre clases y sus dependencias.

Azure Functions se basa en las características de inserción de dependencias de ASP.NET Core. Ser consciente de los servicios, duraciones y patrones de diseño de [inserción de dependencias de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) antes de usar las características de inserción de dependencias en un Azure Functions se recomienda la aplicación.

## <a name="prerequisites"></a>Requisitos previos

Para poder usar la inserción de dependencias, debe instalar los siguientes paquetes NuGet:

- [Microsoft.Azure.Functions.Extensions](https://www.nuget.org/packages/Microsoft.Azure.Functions.Extensions/)

- [Paquete Microsoft.NET.Sdk.Functions](https://www.nuget.org/packages/Microsoft.NET.Sdk.Functions/) versión 1.0.28 o posterior

## <a name="register-services"></a>Registro de los servicios

Para registrar los servicios, puede crear un método para configurar y agregar componentes a un `IFunctionsHostBuilder` instancia.  El host de Azure Functions crea una instancia de `IFunctionsHostBuilder` y lo pasa directamente a su método.

Para registrar el método, agregue el `FunctionsStartup` usa el atributo de ensamblado que especifica el nombre de tipo durante el inicio.

```csharp
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

## <a name="use-injected-dependencies"></a>Usar dependencias insertadas

ASP.NET Core usa la inserción del constructor para que las dependencias estén disponibles para la función. El ejemplo siguiente se muestra cómo el `IMyService` y `HttpClient` dependencias se insertan en una función desencadenada por HTTP.

```csharp
namespace MyNamespace
{
    public class HttpTrigger
    {
        private readonly IMyService _service;
        private readonly HttpClient _client;

        public HttpTrigger(IMyService service, IHttpClientFactory httpClientFactory)
        {
            _service = service;
            _client = httpClientFactory.CreateClient();;
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

El uso de inserción del constructor significa que no debe utilizar las funciones estáticas si desea aprovechar las ventajas de inserción de dependencias.

## <a name="service-lifetimes"></a>Duración de los servicios

Aplicaciones de funciones de Azure proporcionan la misma duración de servicio como [inserción de dependencias de ASP.NET](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes): transitorios, de ámbito y singleton.

En una aplicación de functions, una duración de servicio con ámbito coincide con una duración de ejecución de la función. Los servicios de ámbito se crean una vez por cada ejecución. Las solicitudes posteriores para ese servicio durante la ejecución volver a usar la instancia de servicio existente. Una duración de singleton del servicio coincide con la duración de host y se reutiliza en ejecuciones de función en esa instancia.

Servicios de duración de singleton se recomiendan para las conexiones y los clientes, por ejemplo `SqlConnection`, `CloudBlobClient`, o `HttpClient` instancias.

Ver o descargar un [ejemplo de duración de los diferentes servicios](https://aka.ms/functions/di-sample) en GitHub.

## <a name="logging-services"></a>Servicios de registro

Si tiene su propio proveedor de registro, el método recomendado es registrar un `ILoggerProvider` instancia. Application Insights se agrega automáticamente mediante Azure Functions.

> [!WARNING]
> No agregue `AddApplicationInsightsTelemetry()` a la colección de servicios tal y como registros de servicios que entran en conflicto con los servicios proporcionados por el entorno.

## <a name="function-app-provided-services"></a>Servicios de aplicación proporcionada (función)

El host de la función registra muchos servicios. Los siguientes servicios son seguros tomar como una dependencia en la aplicación:

|Tipo de servicio|Vigencia|DESCRIPCIÓN|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|singleton|Configuración de tiempo de ejecución|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|singleton|Responsable de proporcionar el identificador de la instancia de host|

Si hay otros servicios que desea tomar una dependencia en, [crear un problema y proponer a ellos en GitHub](https://github.com/azure/azure-functions-host).

### <a name="overriding-host-services"></a>Reemplazo host de servicios

Reemplazar servicios proporcionados por el host no se admite.  Si hay servicios que desee invalidar, [crear un problema y proponer a ellos en GitHub](https://github.com/azure/azure-functions-host).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

- [Cómo supervisar la aplicación de función](functions-monitoring.md)
- [Procedimientos recomendados para las funciones](functions-best-practices.md)
