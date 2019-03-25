---
title: Exploración de los registros de seguimiento de .NET en Azure Application Insights con ILogger
description: Ejemplos de uso de la implementación de Azure Application Insights ILogger con aplicaciones ASP.NET Core y de consola.
services: application-insights
author: cijothomas
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 02/19/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: 4c385d2af0d9e4e213cd690b3c30d8719588220d
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58399501"
---
# <a name="ilogger"></a>ILogger

ASP.NET Core es compatible con una API de registro que funciona con una variedad de proveedores de registro integrados y de terceros. En este artículo se muestra cómo tratar el registro con la implementación de ILogger de Application Insights tanto en las aplicaciones de consola como en las de ASP.NET Core. Para más información sobre el registro basado en ILogger, consulte [este artículo](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="console-application"></a>Aplicación de consola

A continuación se muestra una aplicación de ejemplo de la consola configurada para enviar seguimientos de ILogger a Application Insights.

Paquetes instalados:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="2.1.0" />
  <PackageReference Include="Microsoft.Extensions.Logging" Version="2.1.0" />
  <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />
  <PackageReference Include="Microsoft.Extensions.Logging.Console" Version="2.1.0" />
</ItemGroup>
```

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Create DI container.
        IServiceCollection services = new ServiceCollection();

        // Channel is explicitly configured to do flush on it later.
        var channel = new InMemoryChannel();
        services.Configure<TelemetryConfiguration>(
            (config) =>
            {
                config.TelemetryChannel = channel;
            }
        );

        // Add the logging pipelines to use. We are using Application Insights only here.
        services.AddLogging(loggingBuilder =>
        {
        // Optional: Apply filters to configure LogLevel Trace or above is sent to ApplicationInsights for all
        // categories.
        loggingBuilder.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Trace);
            loggingBuilder.AddApplicationInsights("--YourAIKeyHere--");                
        });

        // Build ServiceProvider.
        IServiceProvider serviceProvider = services.BuildServiceProvider();

        ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

        // Begin a new scope. This is optional. Epecially in case of AspNetCore request info is already
        // present in scope.
        using (logger.BeginScope(new Dictionary<string, object> { { "Method", nameof(Main) } }))
        {
            logger.LogInformation("Logger is working"); // this will be captured by Application Insights.
        }

        // Explicitly call Flush() followed by sleep is required in Console Apps.
        // This is to ensure that even if application terminates, telemetry is sent to the back-end.
        channel.Flush();
        Thread.Sleep(1000);
    }
}
```

## <a name="aspnet-core-application"></a>Aplicación ASP.NET Core

A continuación se muestra una aplicación de ASP.NET Core de ejemplo configurada para enviar seguimientos de ILogger a Application Insights. Se puede seguir este ejemplo para enviar seguimientos de ILogger desde Program.cs, Startup.cs o cualquier otra lógica de controlador o aplicación.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var host = BuildWebHost(args);
        var logger = host.Services.GetRequiredService<ILogger<Program>>();
        logger.LogInformation("From Program. Running the host now.."); // This will be picked up by AI
        host.Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(logging =>
        {
            logging.AddApplicationInsights("ikeyhere");

            // Optional: Apply filters to configure LogLevel Trace or above is sent to
            // ApplicationInsights for all categories.
            logging.AddFilter<ApplicationInsightsLoggerProvider>("", LogLevel.Trace);

            // Additional filtering For category starting in "Microsoft",
            // only Warning or above will be sent to Application Insights.
            logging.AddFilter<ApplicationInsightsLoggerProvider>("Microsoft", LogLevel.Warning);
        })
        .Build();
}
```

```csharp
public class Startup
{
    private readonly ILogger _logger;

    public Startup(IConfiguration configuration, ILogger<Startup> logger)
    {
        Configuration = configuration;
        _logger = logger;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_1);

        // The following will be picked up by Application Insights.
        _logger.LogInformation("From ConfigureServices. Services.AddMVC invoked"); 
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
        // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Development environment");
            app.UseDeveloperExceptionPage();
        }
        else
        {
            // The following will be picked up by Application Insights.
            _logger.LogInformation("Configuring for Production environment");
        }

        app.UseMvc();
    }
}
```

```csharp
public class ValuesController : ControllerBase
{
    private readonly ILogger _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    // GET api/values
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        // All the following logs will be picked up by Application Insights.
        // and all have ("MyKey", "MyValue") in Properties.
        using (_logger.BeginScope(new Dictionary<string, object> { { "MyKey", "MyValue" } }))
            {
            _logger.LogInformation("An example of a Information trace..");
            _logger.LogWarning("An example of a Warning trace..");
            _logger.LogTrace("An example of a Trace level message");
            }
        return new string[] { "value1", "value2" };
    }
}
```

En los dos ejemplos anteriores, se utiliza el paquete independiente Microsoft.Extensions.Logging.ApplicationInsights. De forma predeterminada, esta configuración utiliza el mínimo `TelemetryConfiguration` para enviar datos a Application Insights. Mínimo significa que el canal utilizado será `InMemoryChannel`, sin muestreo y sin inicializadores de telemetría estándar. Este comportamiento se puede invalidar para una aplicación de consola, tal como se muestra en el siguiente ejemplo.

Instale este paquete adicional:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

En la sección siguiente se muestra cómo invalidar el valor predeterminado `TelemetryConfiguration`. En este ejemplo se configura `ServerTelemetryChannel`, el muestreo y un `ITelemetryInitializer` personalizado.

```csharp
    // Create DI container.
    IServiceCollection services = new ServiceCollection();
    var serverChannel = new ServerTelemetryChannel();
    services.Configure<TelemetryConfiguration>(
        (config) =>
        {
            config.TelemetryChannel = serverChannel;
            config.TelemetryInitializers.Add(new MyTelemetryInitalizer());
            config.DefaultTelemetrySink.TelemetryProcessorChainBuilder.UseSampling(5);
            serverChannel.Initialize(config);
        }
    );

    // Add the logging pipelines to use. We are adding ApplicationInsights only.
    services.AddLogging(loggingBuilder =>
    {
        loggingBuilder.AddApplicationInsights();
    });

    ........
    ........

    // Explicitly call Flush() followed by sleep is required in Console Apps.
    // This is to ensure that even if application terminates, telemetry is sent to the back-end.
    serverChannel.Flush();
    Thread.Sleep(1000);
```

Aunque el enfoque anterior se puede usar en una aplicación de ASP.NET Core, así, sería un enfoque más común combinar la supervisión de aplicaciones normal (solicitudes, etc. de dependencias) con la captura de ILogger tal como se muestra a continuación.

Instale este paquete adicional:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
```

Agregue lo siguiente al método `ConfigureServices`. Este código permitirá la supervisión regular de aplicaciones con la configuración predeterminada (ServerTelemetryChannel, LiveMetrics, Request/Dependencies, Correlation etc.).

```csharp
services.AddApplicationInsightsTelemetry("ikeyhere");
```

En este ejemplo, la configuración que utiliza `ApplicationInsightsLoggerProvider` es la misma que la utilizada por la supervisión regular de aplicaciones. Por lo tanto, tanto los seguimientos de `ILogger` como otras telemetrías (solicitudes, dependencias, etc.) se ejecutarán con el mismo conjunto de `TelemetryInitializers`, `TelemetryProcessors` y `TelemetryChannel`. Se correlacionarán y se muestrearán o no se muestrearán de la misma manera.

Sin embargo, hay una excepción a este comportamiento. El valor predeterminado `TelemetryConfiguration` no está completamente configurado cuando se registra algo desde `Program.cs` o `Startup.cs`, por lo que esos registros no tendrán la configuración predeterminada. Sin embargo, todos los demás registros (por ejemplo, los registros de controladores, modelos, etc.) compartirán la configuración.

## <a name="control-logging-level"></a>Nivel de registro de control

Además de filtrar los registros en el código como se muestra en los ejemplos anteriores, también es posible controlar el nivel de registro de Application Insights captura, modificando el `appsettings.json`. El [registro documentación aspectos básicos ASP.NET](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering) se muestra cómo lograr esto. Específicamente para Application Insights, el nombre del alias de proveedor es `ApplicationInsights`, tal y como se muestra en el siguiente ejemplo que configura `ApplicationInsights` para capturar solo los registros de `Warning` y versiones posteriores de todas las categorías.

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Warning"
      }
    },
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

*¿Veo que algunos registros de ILogger se muestran dos veces en Application Insights?*

* Esto es posible si tiene la versión anterior (ya obsoleta) de `ApplicationInsightsLoggerProvider` habilitado mediante una llamada a `AddApplicationInsights` en `ILoggerFactory`. Compruebe si su `Configure` método tiene las siguientes y quitarlo.

   ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
        // ..other code.
    }
   ```

* Si experimenta registro doble al depurar desde Visual Studio, a continuación, modificar el código que se usa para habilitar Application Insights como sigue, estableciendo `EnableDebugLogger` sea false. Esto solo es pertinente cuando se depura la aplicación.

   ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
        options.EnableDebugLogger = false;
        services.AddApplicationInsightsTelemetry(options);
        // ..other code.
    }
   ```



## <a name="next-steps"></a>Pasos siguientes

Más información sobre:

- [Registro basado en ILogger](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
- [Registros de seguimiento de .NET](../../azure-monitor/app/asp-net-trace-logs.md)
