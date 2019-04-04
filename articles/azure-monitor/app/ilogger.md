---
title: Exploración de los registros de seguimiento de .NET en Azure Application Insights con ILogger
description: Ejemplos del uso del proveedor de Azure Application Insights ILogger con aplicaciones de consola y ASP.NET Core.
services: application-insights
author: cijothomas
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 02/19/2019
ms.reviewer: mbullwin
ms.author: cithomas
ms.openlocfilehash: ea7f2e730b4963016d221705ba8c9356efffa858
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58905280"
---
# <a name="applicationinsightsloggerprovider-for-net-core-ilogger-logs"></a>ApplicationInsightsLoggerProvider para los registros de .NET Core ILogger

ASP.NET Core es compatible con una API de registro que funciona con diferentes tipos de proveedores de registro integrados y de terceros. El registro se realiza mediante una llamada a Log() o una variante de éste en `ILogger` instancias. En este artículo se muestra cómo usar `ApplicationInsightsLoggerProvider` para capturar `ILogger` inicia sesión en la consola y aplicaciones de ASP.NET Core. Este artículo se describe también cómo `ApplicationInsightsLoggerProvider` está integrado con otros datos de telemetría de Application Insights.
Para obtener la información de registro más en Asp.Net Core, consulte [en este artículo](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="aspnet-core-applications"></a>Aplicaciones ASP.NET Core

A partir de [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) 2.7.0-beta3 de versión y versiones posteriores, `ApplicationInsightsLoggerProvider` está habilitada de forma predeterminada cuando se habilita la supervisión de Application Insights regular mediante de los métodos estándar - por una llamada a `UseApplicationInsights` método de extensión en IWebHostBuilder o `AddApplicationInsightsTelemetry` método de extensión en IServiceCollection. `ILogger` los registros capturan por `ApplicationInsightsLoggerProvider` están sujetos a la misma configuración que cualquier telemetría recopilado. es decir, tienen el mismo conjunto de `TelemetryInitializer`s, `TelemetryProcessor`s, usa el mismo `TelemetryChannel`y se correlacionan y muestreados en la misma manera que cada otros datos de telemetría.  Si se encuentra en esta versión de SDK o superior, a continuación, se requiere ninguna acción para capturar `ILogger` registros.

De forma predeterminada, sólo `ILogger` los registros de `Warning` o anterior (de todas las categorías) se envían a Application Insights. Se puede cambiar este comportamiento aplicando filtros como se muestra [aquí](#control-logging-level). También se requieren pasos adicionales si `ILogger` los registros de `Program.cs` o `Startup.cs` son que va a capturar como se muestra [aquí](#capturing-ilogger-logs-from-startupcs-programcs-in-aspnet-core-applications).

Si usa una versión anterior del SDK de Microsoft.ApplicationInsights.AspNet, o desea usar solo ApplicationInsightsLoggerProvider, sin ninguna otra supervisión de Application Insights, siga estos pasos.

1. Instale el paquete de nuget.

```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />  
    </ItemGroup>
```

2. modifique `Program.cs` las indicadas a continuación

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(
            builder =>
            {
                // Providing an instrumentation key here is required if you are using
                // standalone package Microsoft.Extensions.Logging.ApplicationInsights
                // or if you want to capture logs from early in the application startup
                // pipeline from Startup.cs or Program.cs itself.
                builder.AddApplicationInsights("ikey");

                // Optional: Apply filters to control what logs are sent to Application Insights.
                // The following configures LogLevel Information or above to be sent to
                // Application Insights for all categories.
                builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                                 ("", LogLevel.Information);
            }
        );
}
```

El código anterior se configurará `ApplicationInsightsLoggerProvider`. Un ejemplo de clase de controlador, que usa `ILogger` para enviar los registros que se capturan por Application Insights.

```csharp
public class ValuesController : ControllerBase
{
    private readonly `ILogger` _logger;

    public ValuesController(ILogger<ValuesController> logger)
    {
        _logger = logger;
    }

    // GET api/values
    [HttpGet]
    public ActionResult<IEnumerable<string>> Get()
    {
        // All the following logs will be picked up by Application Insights.
        // and all of them will have ("MyKey", "MyValue") in Properties.
        using (_logger.BeginScope(new Dictionary<string, object> { { "MyKey", "MyValue" } }))
            {
                _logger.LogWarning("An example of a Warning trace..");
                _logger.LogError("An example of an Error level message");
            }
        return new string[] { "value1", "value2" };
    }
}
```

### <a name="capturing-ilogger-logs-from-startupcs-programcs-in-aspnet-core-applications"></a>Captura de registros de ILogger desde Startup.cs, Program.cs en aplicaciones Asp.Net Core

Con el nuevo ApplicationInsightsLoggerProvider, es posible capturar registros desde principio de la canalización de inicio de la aplicación. Incluso aunque ApplicationInsightsLoggerProvider es automáticamente habilitado Application Insights (desde 2.7.0-beta3 y versiones posteriores), no tiene la configuración de clave de instrumentación hasta más adelante en la canalización, lo que solo los registros de controlador / otras clases se capturará. Para capturar todos los registros a partir de `Program.cs` y `Startup.cs` , uno debe habilitar explícitamente ApplicationInsightsLoggerProvider con una clave de instrumentación. También es importante tener en cuenta que `TelemetryConfiguration` es no configurar cuándo se registra completamente algo de `Program.cs` o `Startup.cs` Sí, por lo que esos registros usará una configuración mínima que usa ninguna telemetría estándar, sin muestreo e inmemorychannel. inicializadores o procesadores.

A continuación muestra ejemplos de `Program.cs` y `Startup.cs` con esta capacidad.

#### <a name="example-programcs"></a>Ejemplo Program.cs

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Logging;

public class Program
{
    public static void Main(string[] args)
    {
        var host = CreateWebHostBuilder(args).Build();
        var logger = host.Services.GetRequiredService<ILogger<Program>>();
        // This will be picked up by AI
        logger.LogInformation("From Program. Running the host now..");
        host.Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureLogging(
        builder =>
            {
            // providing an instrumentation key here is required if you are using
            // standalone package Microsoft.Extensions.Logging.ApplicationInsights
            // or if you want to capture logs from early in the application startup 
            // pipeline from Startup.cs or Program.cs itself.
            builder.AddApplicationInsights("ikey");

            // Adding the filter below to ensure logs of all severity from Program.cs
            // is sent to ApplicationInsights.
            // Replace YourAppName with the namespace of your application's Program.cs
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("YourAppName.Program", LogLevel.Trace);
            // Adding the filter below to ensure logs of all severity from Startup.cs
            // is sent to ApplicationInsights.
            // Replace YourAppName with the namespace of your application's Startup.cs
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("YourAppName.Startup", LogLevel.Trace);
            }
        );
}
```

#### <a name="example-startupcs"></a>Ejemplo Startup.cs

```csharp
public class Startup
{
    private readonly `ILogger` _logger;

    public Startup(IConfiguration configuration, ILogger<Startup> logger)
    {
        Configuration = configuration;
        _logger = logger;
    }

    public IConfiguration Configuration { get; }

    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following will be picked up by Application Insights.
        _logger.LogInformation("Logging from ConfigureServices.");
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

## <a name="migrating-from-old-applicationinsightsloggerprovider"></a>Migración desde ApplicationInsightsLoggerProvider antiguo

Versiones del SDK de Microsoft.ApplicationInsights.AspNet antes 2.7.0-beta2, admite un proveedor de registro ahora está obsoleto. Este proveedor se habilitó con `AddApplicationInsights()` método de extensión de `ILoggerFactory`. Este proveedor está ahora obsoleto, y se sugieren los usuarios para migrar al nuevo proveedor. La migración implica dos pasos.

1. Quite llamada ILoggerFactory.AddApplicationInsights() de `Startup.Configure()` método para evitar un registro doble.
2. Volver a aplicar las reglas de filtrado en el código que no se respetarán el nuevo proveedor. Las sobrecargas de ILoggerFactory.AddApplicationInsights() tardaron mínimas funciones LogLevel o filtro. Con el nuevo proveedor, filtrado es parte de la plataforma de registro y no ha realizado por el proveedor de Application Insights. Modo que los filtros proporcionados a través de `ILoggerFactory.AddApplicationInsights()` sobrecargas deben quitarse y reglas de filtrado deben proporcionarse siguiendo [estos](#control-logging-level) instrucciones. Si usas `appsettings.json` para filtrar el registro, seguirán funcionando con el nuevo proveedor, ya que ambas usan el mismo Alias de proveedor - **ApplicationInsights**.

Aunque todavía puede usar el proveedor anterior (ahora está obsoleta y se eliminará solo en el cambio de versión principal a 3.xx), la migración a un proveedor más reciente es muy recomendable por las razones siguientes.

1. Proveedor anterior carecía de compatibilidad de [ámbitos](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-scopes). En el nuevo proveedor, las propiedades de ámbito se agregan automáticamente como propiedades personalizadas a los datos de telemetría recopilados.
2. Ahora se pueden capturar registros mucho anteriormente en la canalización de inicio de la aplicación. es decir, Ahora se pueden capturar registros desde las clases de inicio y el programa.
3. Con el nuevo proveedor, el filtrado se realiza en el nivel del marco de trabajo. Filtrado de registros al proveedor de Application Insights puede realizarse en la misma manera que para otros proveedores, incluidos los proveedores integrados, como la consola de depuración y así sucesivamente. También es posible aplicar los mismos filtros a varios proveedores.
4. El [recomienda](https://github.com/aspnet/Announcements/issues/255) forma en Asp.Net Core (2.0 y versiones posteriores) para habilitar los proveedores de registro es mediante los métodos de extensión en ILoggingBuilder en `Program.cs` propio.

> [!Note]
El nuevo proveedor está disponible para las aplicaciones destinadas a `NETSTANDARD2.0` o superior. Si la aplicación tiene como destino versiones anteriores de .NET Core como .NET Core 1.1 o si el destino es .NET Framework, seguir usando el proveedor anterior.

## <a name="console-application"></a>Aplicación de consola

El código siguiente muestra una aplicación de consola de ejemplo configurada para enviar `ILogger` seguimientos a Application Insights.

Paquetes instalados:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="2.1.0" />  
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
        services.AddLogging(builder =>
        {
            // Optional: Apply filters to configure LogLevel Trace or above is sent to
            // Application Insights for all categories.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             ("", LogLevel.Trace);
            builder.AddApplicationInsights("--YourAIKeyHere--");
        });

        // Build ServiceProvider.
        IServiceProvider serviceProvider = services.BuildServiceProvider();

        ILogger<Program> logger = serviceProvider.GetRequiredService<ILogger<Program>>();

        // Begin a new scope. This is optional.
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

En el ejemplo anterior, el paquete independiente `Microsoft.Extensions.Logging.ApplicationInsights` se utiliza. De forma predeterminada, esta configuración utiliza el mínimo `TelemetryConfiguration` para enviar datos a Application Insights. Mínimo significa que el canal utilizado será `InMemoryChannel`, sin muestreo y sin inicializadores de telemetría estándar. Este comportamiento se puede invalidar para una aplicación de consola, tal como se muestra en el siguiente ejemplo.

Instale este paquete adicional:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

En la sección siguiente se muestra cómo invalidar el valor predeterminado `TelemetryConfiguration` utilizando `services.Configure<TelemetryConfiguration>()` método. En este ejemplo configura `ServerTelemetryChannel`, muestreo y agrega un personalizado `ITelemetryInitializer` a la `TelemetryConfiguration`.

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

    // Add the logging pipelines to use. We are adding Application Insights only.
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

## <a name="control-logging-level"></a>Nivel de registro de control

Asp.Net Core `ILogger` infra tiene un mecanismo integrado para aplicar [filtrado](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering) de registros, lo que permite a los usuarios controlar los registros enviados a cada los proveedores registrados, incluido el proveedor de Application Insights. Este filtrado se puede hacer en la configuración (normalmente mediante `appsettings.json` archivo) o en el código. Esta función se proporciona el marco de trabajo y no es específica de proveedor de Application Insights.

A continuación se proporcionan ejemplos de aplicar las reglas de filtro a ApplicationInsightsLoggerProvider.

### <a name="create-filter-rules-in-configuration-with-appsettingsjson"></a>Crear reglas de filtro en la configuración con appsettings.json

Para ApplicationInsightsLoggerProvider, el alias de proveedor es `ApplicationInsights`. La siguiente sección se muestra en `appsettings.json` configura los registros `Warning` y versiones posteriores de todas las categorías, `Error` y versiones posteriores de las categorías a partir de "Microsoft" se envíen a `ApplicationInsightsLoggerProvider`.

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "Warning",
        "Microsoft": "Error"
      }
    },
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### <a name="create-filter-rules-in-code"></a>Crear reglas de filtro en el código

El código siguiente fragmento de código configura los registros `Warning` y versiones posteriores de todas las categorías, `Error` y versiones posteriores de las categorías a partir de 'Microsoft' para enviarse a `ApplicationInsightsLoggerProvider`. Esta configuración es igual que la configuración anterior en `appsettings.json`.

```csharp
    WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
      logging.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("", LogLevel.Warning)
             .AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("Microsoft", LogLevel.Error);
```

## <a name="frequently-asked-questions"></a>Preguntas frecuentes

*1. ¿Qué es el ApplicationInsightsLoggerProvider antiguo y nuevo?*

* [SDK de Microsoft.ApplicationInsights.AspNet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) incluyó un ApplicationInsightsLoggerProvider integrado (Microsoft.ApplicationInsights.AspNetCore.Logging.ApplicationInsightsLoggerProvider), que se ha habilitado mediante ILoggerFactory métodos de extensión. Este proveedor está marcado como obsoleto desde 2.7.0-beta2 y versiones posteriores y se quitará por completo en el siguiente cambio de versión principal. [Esto](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) empaquetar propio no obsoleto y se requiere para habilitar la supervisión de las solicitudes, dependencias etcetera.

* La alternativa sugerida es el nuevo paquete independiente [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights), que contiene una (ApplicationInsightsLoggerProvider mejorada Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider) y los métodos de extensiones en ILoggerBuilder para habilitarlo.

* [SDK de Microsoft.ApplicationInsights.AspNet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) versión 2.7.0-beta3 adoptará una dependencia en el paquete anterior y versiones posteriores y permite `ILogger` capturar automáticamente.

*2. ¿Veo algunos `ILogger` los registros se muestran dos veces en Application Insights?*

* Es posible si tiene la versión anterior (ya obsoleta) de esta duplicación `ApplicationInsightsLoggerProvider` habilitado mediante una llamada a `AddApplicationInsights` en `ILoggerFactory`. Compruebe si su `Configure` método tiene las siguientes y quitarlo.

   ```csharp
    public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
    {
        loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
        // ..other code.
    }
   ```

* Si experimenta registro doble al depurar desde Visual Studio, a continuación, modificar el código que se usa para habilitar Application Insights como sigue, estableciendo `EnableDebugLogger` sea false. Este problema de duplicación y la corrección solo es pertinente cuando se depura la aplicación.

   ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
        options.EnableDebugLogger = false;
        services.AddApplicationInsightsTelemetry(options);
        // ..other code.
    }
   ```

*3. Actualicé a [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) 2.7.0-beta3 versión y ahora veo que los registros de `ILogger` se capturan automáticamente. ¿Cómo puedo desactivar esta característica completamente?*

* Consulte [esto](../../azure-monitor/app/ilogger.md#control-logging-level) sección para saber cómo filtrar los registros en general. Para desactivar ApplicationInsightsLoggerProvider usar `LogLevel.None` para él.

  En el código

    ```csharp
        builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                          ("", LogLevel.None);
    ```

  En la configuración

    ```json
    {
      "Logging": {
        "ApplicationInsights": {
          "LogLevel": {
            "Default": "None"
          }
    }
    ```

*4. ¿Veo algunos `ILogger` registros no tienen la mismas propiedades que otros usuarios?*

* Aplicación capturas Insights y envía `ILogger` registra con el mismo `TelemetryConfiguration` utilizado para cada otros datos de telemetría. Hay una excepción a esta regla. El valor predeterminado `TelemetryConfiguration` es no configurar cuándo se registra completamente algo de `Program.cs` o `Startup.cs` Sí, por lo que los registros de estos lugares no tendrá la configuración predeterminada y, por tanto, no se ejecutarán todos los `TelemetryInitializer`s y `TelemetryProcessor`s.

*5. ¿Qué tipo de datos de telemetría de Application Insights se genera a partir `ILogger` registros? o ¿dónde puedo ver `ILogger` registros en Application Insights?*

* Captura de ApplicationInsightsLoggerProvider `ILogger` registra y crea `TraceTelemetry` de él. Si un objeto de excepción se pasa al método Log() en ILogger, a continuación, en lugar de `TraceTelemetry`, un `ExceptionTelemetry` se crea. Estos elementos de telemetría pueden encontrarse en los mismos lugares que con cualquier otra `TraceTelemetry` o `ExceptionTelemetry` para Application Insights, incluido el portal, análisis o depurador local de Visual Studio.
Si prefiere enviar siempre `TraceTelemetry`, a continuación, use el fragmento de código ```builder.AddApplicationInsights((opt) => opt.TrackExceptionsAsExceptionTelemetry = false);```.

*5. No tengo SDK instalado, y usar extensión de aplicación Web de Azure para habilitar Application Insights para mis aplicaciones de Asp.Net Core. ¿Cómo se puede usar el nuevo proveedor?*

* Extensión de Application Insights en Azure Web Apps usa el proveedor anterior. Las reglas de filtrado se pueden modificar en `appsettings.json` para la aplicación. Si desea aprovechar las ventajas del nuevo proveedor, use la instrumentación en tiempo de compilación tomando la dependencia de nuget en el SDK de. Este documento se actualizará cuando se cambia de extensión para usar el nuevo proveedor.

*6. Soy mediante el paquete independiente Microsoft.Extensions.Logging.ApplicationInsights y habilitar el proveedor de Application Insights por el generador que realiza la llamada. AddApplicationInsights("ikey"). ¿Hay una opción para obtener la clave de instrumentación de configuración?*


* Modificar `Program.cs` y `appsettings.json` tal como se muestra a continuación.

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .ConfigureLogging((hostingContext, logging) =>
            {
                // hostingContext.HostingEnvironment can be used to determine environments as well.
                var appInsightKey = hostingContext.Configuration["myikeyfromconfig"];
                logging.AddApplicationInsights(appInsightKey);
            });
}
```

Sección correspondiente de `appsettings.json`

```json
{
  "myikeyfromconfig": "putrealikeyhere"
}
```

El código anterior se requiere solo cuando se usa el proveedor de registro independiente. Para la supervisión regular de Application Insights, clave de instrumentación se carga automáticamente desde la ruta de acceso de configuración `ApplicationInsights:Instrumentationkey` y `appsettings.json` debe ser similar al siguiente.

```json
{
  "ApplicationInsights":
    {
        "Instrumentationkey":"putrealikeyhere"
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

Más información sobre:

* [Registro de Asp.Net Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
* [Los registros de seguimiento de .NET en Application Insights](../../azure-monitor/app/asp-net-trace-logs.md)
