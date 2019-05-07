---
title: Usar inserción de dependencias en .NET de Azure Functions
description: Aprenda a usar la inserción de dependencias para el registro y uso de servicios en las funciones de .NET
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
keywords: funciones de azure, funciones, arquitectura sin servidor
ms.service: azure-functions
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/22/2019
ms.author: jehollan
ms.openlocfilehash: 1ef688dff65dc11f875f76e2f9127bf89af2f2b9
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65074596"
---
# <a name="use-dependency-injection-in-net-azure-functions"></a>Usar inserción de dependencias en .NET de Azure Functions

Azure Functions admite el patrón de diseño de dependencia (DI) de inyección software, que es una técnica para lograr [inversión de Control (IoC)](https://docs.microsoft.com/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) entre clases y sus dependencias.

Azure Functions se basa en las características de inserción de dependencias de ASP.NET Core.  Debe comprender los servicios, duraciones y patrones de diseño de [inserción de dependencias de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) antes de usarlos en funciones.

## <a name="installing-dependency-injection-packages"></a>Instalación de paquetes de inyección de dependencia

Para poder usar las características de inyección de dependencia, deberá incluir el paquete de NuGet que expone las API.

```powershell
Install-Package Microsoft.Azure.Functions.Extensions
```

## <a name="registering-services"></a>Registrar servicios

Para registrar los servicios, puede crear un método configure y agregar componentes a un `IFunctionsHostBuilder` instancia.  El host de Azure Functions crea una `IFunctionsHostBuilder` y lo pasa directamente a su método configurado.

Para registrar la configuración del método, debe agregar un atributo de ensamblado que especifica el tipo de su configuración mediante el método la `FunctionsStartup` atributo.

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

## <a name="service-lifetimes"></a>Duración de los servicios

Aplicaciones de función de Azure proporcionan la misma duración de servicio como [inserción de dependencias de ASP.NET](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes), transitoria, ámbito y singleton.

En una aplicación de función, una duración de servicio con ámbito coincide con una duración de ejecución de la función. Los servicios de ámbito se crean una vez por cada ejecución.  Las solicitudes posteriores para ese servicio durante la ejecución reutilización esa instancia.  Una duración de singleton del servicio coincide con la duración de host y se reutiliza en ejecuciones de función en esa instancia.

Servicios de duración de singleton se recomiendan para las conexiones y los clientes, por ejemplo un `SqlConnection`, `CloudBlobClient`, o `HttpClient`.

Ver o descargar un [ejemplo de duración de los diferentes servicios](http://aka.ms/functions/di-sample).

## <a name="logging-services"></a>Servicios de registro

Si tiene su propio proveedor de registro, el método recomendado es registrar un `ILoggerProvider`.  Para Application Insights, las funciones agrega Application Insights automáticamente para usted.  

> [!WARNING]
> No agregue `AddApplicationInsightsTelemetry()` a los servicios de recopilación ya que registran los servicios que entrarán en conflicto con lo que es proporcionada por el entorno. 
 
## <a name="function-app-provided-services"></a>Servicios de aplicación proporcionada (función)

El host de Functions registrará muchos servicios.  A continuación se muestran los servicios que son seguros tomar una dependencia en.  No se admiten otros servicios de host para registrar o dependen.  Si hay otros servicios que desea tomar una dependencia en, por favor, [crear un problema y debate en GitHub](https://github.com/azure/azure-functions-host).

|Tipo de servicio|Vigencia|DESCRIPCIÓN|
|--|--|--|
|`Microsoft.Extensions.Configuration.IConfiguration`|singleton|Configuración de tiempo de ejecución|
|`Microsoft.Azure.WebJobs.Host.Executors.IHostIdProvider`|singleton|Responsable de proporcionar el identificador de la instancia de host|

### <a name="overriding-host-services"></a>Reemplazar los servicios de Host

Reemplazar servicios proporcionados por el host no se admite.  Si hay servicios que desea reemplazar, inicie [crear un problema y debate en GitHub](https://github.com/azure/azure-functions-host).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte los siguientes recursos:

* [Cómo supervisar la aplicación de función](functions-monitoring.md)
* [Procedimientos recomendados para las funciones](functions-best-practices.md)