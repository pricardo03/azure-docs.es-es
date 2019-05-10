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
ms.openlocfilehash: ca842ce46a58dafa87581b77bcbd802191f7fcd1
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65511028"
---
# <a name="applicationinsightsloggerprovider-for-net-core-ilogger-logs"></a>ApplicationInsightsLoggerProvider para los registros de .NET Core ILogger

ASP.NET Core es compatible con una API de registro que funciona con diferentes tipos de proveedores de registro integrados y de terceros. El registro se realiza mediante una llamada a **Log()** o una variante de éste en *ILogger* instancias. En este artículo se muestra cómo usar *ApplicationInsightsLoggerProvider* para capturar los registros de ILogger en consola y aplicaciones de ASP.NET Core. En este artículo también se describe cómo ApplicationInsightsLoggerProvider se integra con otros datos de telemetría de Application Insights.
Para más información, consulte [Registro en ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging).

## <a name="aspnet-core-applications"></a>Aplicaciones ASP.NET Core

ApplicationInsightsLoggerProvider está habilitada de forma predeterminada en [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) versión 2.7.0-beta3 (y versiones posteriores) al activar una supervisión regular de Application Insights a través del estándar métodos:
- Mediante una llamada a la **UseApplicationInsights** método de extensión en IWebHostBuilder 
- Mediante una llamada a la **AddApplicationInsightsTelemetry** método de extensión en IServiceCollection

Los registros de ILogger que captura ApplicationInsightsLoggerProvider están sujetos a la misma configuración que los otros datos de telemetría recopilados. Tienen el mismo conjunto de TelemetryInitializers y TelemetryProcessors, use el mismo TelemetryChannel y se ponen en correlación y muestreo de la misma manera que otros datos de telemetría. Si usa 2.7.0-beta3 versión o versiones posteriores, es necesario capturar registros de ILogger nada.

Solo *advertencia* o registros superiores de ILogger (de todas las categorías) se envían a Application Insights de forma predeterminada. Pero puede [aplicar filtros para modificar este comportamiento](#control-logging-level). Se requieren pasos adicionales para capturar registros de ILogger desde **Program.cs** o **Startup.cs**. (Consulte [ILogger capturar registros de Startup.cs y Program.cs en aplicaciones ASP.NET Core](#capture-ilogger-logs-from-startupcs-and-programcs-in-aspnet-core-apps).)

Si usa una versión anterior del SDK de Microsoft.ApplicationInsights.AspNet o desea usar solo ApplicationInsightsLoggerProvider sin ninguna otra supervisión de Application Insights, use el procedimiento siguiente:

1. Instale el paquete de NuGet:

   ```xml
       <ItemGroup>
         <PackageReference Include="Microsoft.Extensions.Logging.ApplicationInsights" Version="2.9.1" />  
       </ItemGroup>
   ```

1. Modificar **Program.cs** como se muestra aquí:

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
                   // Providing an instrumentation key here is required if you're using
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

El código en el paso 2 configura `ApplicationInsightsLoggerProvider`. El código siguiente muestra un ejemplo de clase de controlador, que usa `ILogger` para enviar registros. Los registros se capturan por Application Insights.

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

### <a name="capture-ilogger-logs-from-startupcs-and-programcs-in-aspnet-core-apps"></a>Capturar registros de ILogger desde Startup.cs y Program.cs en aplicaciones ASP.NET Core

El nuevo ApplicationInsightsLoggerProvider puede capturar registros desde principio de la canalización de inicio de la aplicación. Aunque ApplicationInsightsLoggerProvider se habilita automáticamente en Application Insights (empezando por la versión 2.7.0-beta3), no tiene una clave de instrumentación establece hasta que más adelante en la canalización. Por lo tanto, solo los registros de **controlador**/ otras clases se capturará. Para capturar cada registro a partir de **Program.cs** y **Startup.cs** , debe habilitar explícitamente una clave de instrumentación para ApplicationInsightsLoggerProvider. Además, *valor de TelemetryConfiguration* no está completamente configurado cuando inician desde **Program.cs** o **Startup.cs** propio. Por lo tanto, esos registros tendrá una configuración mínima que no utiliza InMemoryChannel, sin muestreo y los inicializadores de telemetría estándar o procesadores.

Los siguientes ejemplos muestran esta capacidad con **Program.cs** y **Startup.cs**.

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
            // Providing an instrumentation key here is required if you're using
            // standalone package Microsoft.Extensions.Logging.ApplicationInsights
            // or if you want to capture logs from early in the application startup 
            // pipeline from Startup.cs or Program.cs itself.
            builder.AddApplicationInsights("ikey");

            // Adding the filter below to ensure logs of all severity from Program.cs
            // is sent to ApplicationInsights.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             (typeof(Program).FullName, LogLevel.Trace);

            // Adding the filter below to ensure logs of all severity from Startup.cs
            // is sent to ApplicationInsights.
            builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                             (typeof(Startup).FullName, LogLevel.Trace);
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

## <a name="migrate-from-the-old-applicationinsightsloggerprovider"></a>Migrar desde el antiguo ApplicationInsightsLoggerProvider

Las versiones del SDK de Microsoft.ApplicationInsights.AspNet antes 2.7.0-beta2 admita un proveedor de registro que está ahora obsoleto. Este proveedor se habilitó a través de la **AddApplicationInsights()** método de extensión de ILoggerFactory. Le recomendamos que migre al nuevo proveedor, lo que implica dos pasos:

1. Quitar el *ILoggerFactory.AddApplicationInsights()* llamar desde el **Startup.Configure()** método para evitar un registro doble.
2. Volver a aplicar las reglas de filtrado en el código, porque no se respetarán el nuevo proveedor. Las sobrecargas de *ILoggerFactory.AddApplicationInsights()* tardó mínimas funciones LogLevel o filtro. Con el nuevo proveedor, el filtrado de forma parte de la propia plataforma de registro. No se realiza por el proveedor de Application Insights. Por lo tanto, los filtros que se proporcionan a través de *ILoggerFactory.AddApplicationInsights()* sobrecargas deben quitarse. Y se deben proporcionar las reglas de filtrado siguiendo el [controlar el nivel de registro](#control-logging-level) instrucciones. Si usas *appsettings.json* para filtrar el registro, seguirán funcionando con el nuevo proveedor, ya que ambos usan el mismo alias de proveedor, *ApplicationInsights*.

Todavía puede usar el proveedor anterior. (Se quitará sólo en un cambio de versión principal a 3. *xx*.) Pero se recomienda migrar al nuevo proveedor por las razones siguientes:

- El proveedor anterior no es compatible con [ámbitos de registro](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-scopes). En el nuevo proveedor, las propiedades de ámbito se agregan automáticamente como propiedades personalizadas a los datos de telemetría recopilados.
- Ahora se pueden capturar registros mucho anteriormente en la canalización de inicio de la aplicación. Los registros desde el **programa** y **inicio** ahora se pueden capturar las clases.
- Con el nuevo proveedor, el filtrado se realiza en el nivel del marco de trabajo. Puede filtrar los registros para el proveedor de Application Insights en la misma manera que otros proveedores, incluidos los proveedores integrados, como la consola de depuración y así sucesivamente. También puede aplicar los mismos filtros en varios proveedores.
- En ASP.NET Core (2.0 y versiones posteriores), el método recomendado para [permiten a los proveedores de registro](https://github.com/aspnet/Announcements/issues/255) es mediante los métodos de extensión en ILoggingBuilder en **Program.cs** propio.

> [!Note]
> El nuevo proveedor está disponible para las aplicaciones que tienen como destino NETSTANDARD2.0 o posterior. Si la aplicación tiene como destino versiones anteriores de .NET Core, por ejemplo, .NET Core 1.1, o si tiene como destino .NET Framework, seguir usando el proveedor anterior.

## <a name="console-application"></a>Aplicación de consola

El código siguiente muestra una aplicación de consola de ejemplo que está configurada para enviar los seguimientos de ILogger a Application Insights.

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
        // Create the DI container.
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

Este ejemplo usa el paquete independiente `Microsoft.Extensions.Logging.ApplicationInsights`. De forma predeterminada, esta configuración usa el "mínimo" valor de TelemetryConfiguration para enviar datos a Application Insights. Mínimo significa que el canal que se utiliza InMemoryChannel. No hay ningún TelemetryInitializers muestreo y no estándar. Este comportamiento puede invalidarse para una aplicación de consola, como se muestra en el ejemplo siguiente.

Instale este paquete adicional:

```xml
<PackageReference Include="Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel" Version="2.9.1" />
```

En la sección siguiente se muestra cómo invalidar el valor de TelemetryConfiguration predeterminado mediante el uso de la **servicios. Configurar<TelemetryConfiguration>()** método. En este ejemplo configura `ServerTelemetryChannel` y muestreo. Agrega un inicializador de telemetría personalizada para el valor de TelemetryConfiguration.

```csharp
    // Create the DI container.
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

    // Explicitly calling Flush() followed by sleep is required in Console Apps.
    // This is to ensure that even if the application terminates, telemetry is sent to the back end.
    serverChannel.Flush();
    Thread.Sleep(1000);
```

## <a name="control-logging-level"></a>Nivel de registro de control

ASP.NET Core *ILogger* infra tiene un mecanismo integrado para aplicar [filtrado del registro](https://docs.microsoft.com/aspnet/core/fundamentals/logging/?view=aspnetcore-2.2#log-filtering). Esto le permite controlar los registros que se envían a cada proveedor registrado, incluido el proveedor de Application Insights. El filtrado puede realizarse en configuración (normalmente mediante un *appsettings.json* archivo) o en el código. Esta función se proporciona el marco de trabajo. No es específico del proveedor de Application Insights.

Los ejemplos siguientes aplican las reglas de filtro a ApplicationInsightsLoggerProvider.

### <a name="create-filter-rules-in-configuration-with-appsettingsjson"></a>Crear reglas de filtro en la configuración con appsettings.json

Para ApplicationInsightsLoggerProvider, el alias de proveedor es `ApplicationInsights`. La siguiente sección de *appsettings.json* configura los registros para *advertencia* y versiones posteriores de todas las categorías y *Error* y versiones posteriores de las categorías que comienzan con " Microsoft"se envíen a `ApplicationInsightsLoggerProvider`.

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

El siguiente fragmento de código configura los registros para *advertencia* y versiones posteriores de todas las categorías y de *Error* y versiones posteriores de las categorías que comienzan con "Microsoft" se envíen a `ApplicationInsightsLoggerProvider`. Esta configuración es igual que en la sección anterior en *appsettings.json*.

```csharp
    WebHost.CreateDefaultBuilder(args)
    .UseStartup<Startup>()
    .ConfigureLogging(logging =>
      logging.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("", LogLevel.Warning)
             .AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                        ("Microsoft", LogLevel.Error);
```

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="what-are-the-old-and-new-versions-of-applicationinsightsloggerprovider"></a>¿Cuáles son las versiones antiguas y nuevas de ApplicationInsightsLoggerProvider?

[SDK de Microsoft.ApplicationInsights.AspNet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) incluye un ApplicationInsightsLoggerProvider integrado (Microsoft.ApplicationInsights.AspNetCore.Logging.ApplicationInsightsLoggerProvider), que se habilita a través de  **ILoggerFactory** métodos de extensión. Este proveedor se marca obsoleto desde la versión 2.7.0-beta2. Se eliminará por completo en el siguiente cambio de versión principal. El [Microsoft.ApplicationInsights.AspNetCore 2.6.1](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) propio paquete no está obsoleto. Es necesario para habilitar la supervisión de las solicitudes, dependencias y así sucesivamente.

La alternativa sugerida es el nuevo paquete independiente [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights), que contiene una (ApplicationInsightsLoggerProvider mejorada Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider) y métodos de extensión en ILoggerBuilder para habilitarlo.

[SDK de Microsoft.ApplicationInsights.AspNet](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) versión 2.7.0-beta3 toma una dependencia en el nuevo paquete y permite la captura de ILogger automáticamente.

### <a name="why-are-some-ilogger-logs-shown-twice-in-application-insights"></a>¿Por qué se muestran algunos registros de ILogger dos veces en Application Insights?

La duplicación puede ocurrir si tiene la versión anterior (ya obsoleta) de ApplicationInsightsLoggerProvider habilitado mediante una llamada a `AddApplicationInsights` en `ILoggerFactory`. Compruebe si su **configurar** método tiene las siguientes y quítelo:

```csharp
 public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
 {
     loggerFactory.AddApplicationInsights(app.ApplicationServices, LogLevel.Warning);
     // ..other code.
 }
```

Si experimenta registro doble cuando se depura desde Visual Studio, establezca `EnableDebugLogger` a *false* en el código que permite a Application Insights, como se indica a continuación. Esta duplicación y la corrección solo es pertinente cuando se depura la aplicación.

```csharp
 public void ConfigureServices(IServiceCollection services)
 {
     ApplicationInsightsServiceOptions options = new ApplicationInsightsServiceOptions();
     options.EnableDebugLogger = false;
     services.AddApplicationInsightsTelemetry(options);
     // ..other code.
 }
```

### <a name="i-updated-to-microsoftapplicationinsightsaspnet-sdkhttpswwwnugetorgpackagesmicrosoftapplicationinsightsaspnetcore-version-270-beta3-and-logs-from-ilogger-are-captured-automatically-how-do-i-turn-off-this-feature-completely"></a>Actualicé a [Microsoft.ApplicationInsights.AspNet SDK](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) 2.7.0-beta3 de versión y los registros de ILogger se capturan automáticamente. ¿Cómo desactiva esta característica completamente?

Consulte la [controlar el nivel de registro](../../azure-monitor/app/ilogger.md#control-logging-level) sección para ver cómo filtrar los registros en general. Para desactivar ApplicationInsightsLoggerProvider, utilice `LogLevel.None`:

**En el código:**

```csharp
    builder.AddFilter<Microsoft.Extensions.Logging.ApplicationInsights.ApplicationInsightsLoggerProvider>
                      ("", LogLevel.None);
```

**En la configuración:**

```json
{
  "Logging": {
    "ApplicationInsights": {
      "LogLevel": {
        "Default": "None"
      }
}
```

### <a name="why-do-some-ilogger-logs-not-have-the-same-properties-as-others"></a>¿Por qué algunos registros de ILogger no tiene las mismas propiedades que otros usuarios?

Application Insights captura y envía los registros de ILogger mediante el mismo valor de TelemetryConfiguration que se usa para otros datos de telemetría cada. Pero hay una excepción. De forma predeterminada, valor de TelemetryConfiguration no está completamente configurado cuando inician desde **Program.cs** o **Startup.cs**. Los registros de estos lugares no tienen la configuración predeterminada, por lo que no se ejecutará todas TelemetryInitializers y TelemetryProcessors.

### <a name="im-using-the-standalone-package-microsoftextensionsloggingapplicationinsights-and-i-want-to-log-some-additional-custom-telemetry-manually-how-should-i-do-that"></a>Estoy usando el paquete independiente Microsoft.Extensions.Logging.ApplicationInsights, y quiero iniciar manualmente algunos datos de telemetría personalizados adicionales. ¿Cómo debo hacerlo?

Cuando se usa el paquete independiente, `TelemetryClient` no se insertan en el contenedor de DI, por lo que necesita crear una nueva instancia de `TelemetryClient` y utilizan la misma configuración que utilice el proveedor de registrador, como se muestra en el código siguiente. Esto garantiza que se usa la misma configuración para todos los datos de telemetría personalizados así como los datos de telemetría de ILogger.

```csharp
public class MyController : ApiController
{
   // This telemtryclient can be used to track additional telemetry using TrackXXX() api.
   private readonly TelemetryClient _telemetryClient;
   private readonly ILogger _logger;

   public MyController(IOptions<TelemetryConfiguration> options, ILogger<MyController> logger)
   {
        _telemetryClient = new TelemetryClient(options.Value);
        _logger = logger;
   }  
}
```

> [!NOTE]
> Si utiliza el paquete Microsoft.ApplicationInsights.AspNetCore para habilitar Application Insights, modifique este código para obtener `TelemetryClient` directamente en el constructor. Para obtener un ejemplo, vea [estas preguntas más frecuentes](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core-no-visualstudio#frequently-asked-questions).


### <a name="what-application-insights-telemetry-type-is-produced-from-ilogger-logs-or-where-can-i-see-ilogger-logs-in-application-insights"></a>¿Qué tipo de datos de telemetría de Application Insights se genera a partir de los registros de ILogger? O bien, ¿dónde puedo ver que ILogger registros en Application Insights?

ApplicationInsightsLoggerProvider captura los registros de ILogger y crea TraceTelemetry a partir de ellas. Si un objeto de excepción se pasa a la **Log()** método en ILogger, *ExceptionTelemetry* se crea en lugar de TraceTelemetry. Estos elementos de telemetría pueden encontrarse en los mismos lugares como cualquier otro TraceTelemetry o ExceptionTelemetry para Application Insights, incluido el portal, análisis o depurador local de Visual Studio.

Si prefiere enviar siempre TraceTelemetry, use este fragmento de código: ```builder.AddApplicationInsights((opt) => opt.TrackExceptionsAsExceptionTelemetry = false);```

### <a name="i-dont-have-the-sdk-installed-and-i-use-the-azure-web-apps-extension-to-enable-application-insights-for-my-aspnet-core-applications-how-do-i-use-the-new-provider"></a>No tengo instalado el SDK, y usar la extensión de Azure Web Apps para habilitar Application Insights para mis aplicaciones de ASP.NET Core. ¿Cómo se puede usar el nuevo proveedor? 

La extensión de Application Insights en Azure Web Apps usa el proveedor anterior. Puede modificar las reglas de filtrado en el *appsettings.json* archivo para su aplicación. Para aprovechar las ventajas del nuevo proveedor, use la instrumentación en tiempo de compilación tomando una dependencia de NuGet en el SDK. En este artículo se actualizará cuando cambia la extensión para utilizar el nuevo proveedor.

### <a name="im-using-the-standalone-package-microsoftextensionsloggingapplicationinsights-and-enabling-application-insights-provider-by-calling-builderaddapplicationinsightsikey-is-there-an-option-to-get-an-instrumentation-key-from-configuration"></a>Estoy usando el paquete independiente Microsoft.Extensions.Logging.ApplicationInsights y habilitar el proveedor de Application Insights mediante una llamada a **generador. AddApplicationInsights("ikey")**. ¿Hay una opción para obtener una clave de instrumentación de configuración?


Modifique Program.cs y appsettings.json como sigue:

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

   La sección correspondiente de `appsettings.json`:

   ```json
   {
     "myikeyfromconfig": "putrealikeyhere"
   }
   ```

Se requiere este código solo cuando se usa un proveedor de registro independiente. Para la supervisión regular de Application Insights, la clave de instrumentación se carga automáticamente desde la ruta de acceso de configuración *Application Insights: Instrumentationkey*. AppSettings.JSON debe tener este aspecto:

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

* [Registro de ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/logging)
* [Los registros de seguimiento de .NET en Application Insights](../../azure-monitor/app/asp-net-trace-logs.md)
