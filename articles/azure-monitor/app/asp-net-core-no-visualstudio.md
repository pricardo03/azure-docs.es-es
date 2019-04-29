---
title: Azure Application Insights para ASP.NET Core sin Visual Studio | Microsoft Docs
description: Supervise la disponibilidad, el rendimiento y el uso de las aplicaciones web de ASP.NET Core.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: cithomas
ms.openlocfilehash: 8243523887ec9861459b2d196126237cf89bad97
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60691349"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights para aplicaciones de ASP.NET Core

En este artículo se describe los pasos de la habilitación de Application Insights para un [ASP.NET Core](https://docs.microsoft.com/aspnet/core/?view=aspnetcore-2.2) aplicación sin usar el IDE de Visual Studio. Si tiene instalado, a continuación, IDE de Visual Studio [esto](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) documento contiene instrucciones específicas de Visual Studio. Siguiendo las instrucciones de este artículo, Application Insights empezará a recopilar las solicitudes, dependencias, excepciones, contadores de rendimiento, los latidos y los registros de la aplicación ASP.NET Core. La aplicación de ejemplo que usa es un [aplicación MVC](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/?view=aspnetcore-2.2) destinadas a `netcoreapp2.2`, pero estas instrucciones son aplicables a todas las aplicaciones ASP.NET Core. Las excepciones se indican cuando corresponda.

## <a name="supported-scenarios"></a>Escenarios admitidos

Application Insights SDK (Kit de desarrollo de Software) para ASP.NET Core puede supervisar sus aplicaciones con independencia de dónde y cómo se ejecuta la aplicación. Si la aplicación se está ejecutando y tiene conectividad de red con el servicio de Application Insights, telemetría se espera que se recopilen. Esta compatibilidad incluye, pero no se limita a cualquier sistema operativo (Windows, Linux, Mac), el método de hospedaje (en proceso vs fuera de proceso), el método de implementación (dependiente del marco frente a autocontenida), servidor Web (IIS, Kestrel), plataforma (Azure Web Apps, máquina virtual de Azure Docker, AKS y así sucesivamente.) o IDE (Visual Studio, VS Code, línea de comandos).

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación ASP.NET Core funcione. Siga [esto](https://docs.microsoft.com/aspnet/core/getting-started/) guía para crear una aplicación ASP.NET Core, si es necesario.
- Una válido Application Insights clave de instrumentación, que es necesario para enviar los datos de telemetría al servicio Application Insights. Siga [estos](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) instrucciones para crear un nuevo recurso de Application Insights, si es necesario y obtener una clave de instrumentación.

## <a name="enabling-application-insights-server-side-telemetry"></a>Habilitar la telemetría de Application Insights del lado servidor

1. Instale el SDK de Application Insights para ASP.NET Core, que es un paquete NuGet normal. Se recomienda usar siempre la última versión estable. Algunas de las funcionalidades descritas en este artículo no estén disponibles en versiones anteriores. Notas de la versión completa del SDK se pueden encontrar [aquí](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases). 

Siguiente muestra los cambios al agregarse al archivo .csproj de su proyecto.

```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
    </ItemGroup>
```

2. Agregar `services.AddApplicationInsightsTelemetry();` a `ConfigureServices()` método en su `Startup` clase. Ejemplo completo siguiente.

```csharp
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        // The following line enables Application Insights telemetry collection.
        services.AddApplicationInsightsTelemetry();

        // code adding other services for your application
        services.AddMvc();
    }
```

3. Clave de instrumentación del programa de instalación.

   Aunque es posible proporcionar la clave de instrumentación como argumento a `services.AddApplicationInsightsTelemetry("putinstrumentationkeyhere");`, se recomienda especificar la clave de instrumentación en configuración. Lo siguiente muestra cómo especificar una clave de instrumentación en `appsettings.json`. Asegúrese de que `appsettings.json` se copia en la carpeta raíz de la aplicación durante la publicación.

```json
    {
      "ApplicationInsights": {
        "InstrumentationKey": "putinstrumentationkeyhere"
      },
      "Logging": {
        "LogLevel": {
          "Default": "Warning"
        }
      }
    }
```

    Alternately, the instrumentation key can also be specified in either of the following environment variables.
    APPINSIGHTS_INSTRUMENTATIONKEY
    ApplicationInsights:InstrumentationKey

    Example:
    `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

`APPINSIGHTS_INSTRUMENTATIONKEY` Normalmente se utiliza para especificar la clave de instrumentación para las aplicaciones implementadas en Azure Web Apps.

> [!NOTE]
> Una clave de instrumentación especificada en el código wins a través de la variable de entorno `APPINSIGHTS_INSTRUMENTATIONKEY`, que gane sobre otras opciones.

4. Ejecute la aplicación y realizar solicitudes a él. Datos de telemetría ahora deben comenzar a fluir a Application Insights. La siguiente telemetría se recopila automáticamente mediante el SDK de Application Insights.

    1. Solicitudes - solicitudes de web entrantes a la aplicación.
    1. Dependencias
        1. Llamadas de HTTP/Https realizadas con `HttpClient`
        1. Llamadas SQL realizadas con `SqlClient`
        1. Las llamadas de almacenamiento de Azure realizadas con [cliente de Azure Storage](https://www.nuget.org/packages/WindowsAzure.Storage/)*
        1. [SDK de cliente de EventHub](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) versión 1.1.0 y versiones posteriores
        1. [SDK de cliente de ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus) versión 3.0.0 y versiones posteriores

             * Azure Cosmos DB se realiza el seguimiento automáticamente solo si se usa HTTP/HTTPS. Application Insights no capturará el modo TCP.


    1. [Contadores de rendimiento](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/)
        1. Compatibilidad con contadores de rendimiento en ASP.NET Core se limita a la siguiente
            1. SDK versión 2.4.1 y anteriormente recopila contadores de rendimiento si la aplicación se ejecuta en Azure Web Apps (Windows)
            1. SDK versión 2.7.0-beta3 y anteriormente recopila contadores de rendimiento si la aplicación se ejecuta en Windows y destinadas a `NETSTANDARD2.0` o superior.
            1. Para las aplicaciones destinadas a .NET Framework completo, los contadores de rendimiento se admiten en todas las versiones SDK de.

            Este documento se actualizará cuando se agrega compatibilidad con contadores de rendimiento en Linux.
    1. [Live Metrics](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream)
    1. `ILogger` los registros de gravedad `Warning` o versiones posteriores son automáticamente capturados desde el SDK versión 2.7.0-beta3 o superior. Obtenga más información [aquí](https://docs.microsoft.com/azure/azure-monitor/app/ilogger).

Puede tardar unos minutos para la telemetría comenzar a aparecer en el portal. Para comprobar rápidamente si todo funciona, es mejor usar [Live Metrics](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream), mientras que realizar solicitudes a la aplicación en ejecución.

## <a name="send-ilogger-logs-to-application-insights"></a>Enviar registros de ILogger a Application Insights

Application Insights admite captura registros enviados a través de ILogger. Lea la documentación completa [aquí](https://docs.microsoft.com/azure/azure-monitor/app/ilogger).

## <a name="enable-client-side-telemetry-for-web-applications"></a>Habilitar la telemetría del lado cliente para aplicaciones Web

Los pasos anteriores son suficientes para iniciar la recopilación de telemetría del lado servidor. Si la aplicación tiene los componentes del lado cliente, a continuación, siga estos pasos para comenzar a recopilar [telemetría de uso](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview) desde allí.

1. En _ViewImports.cshtml, agregue la inyección de código:

```cshtml
    @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
```

2. En _Layout.cshtml, inserte HtmlHelper al final de `<head>` sección pero antes de cualquier otra secuencia de comandos. Debe insertarse cualquier telemetría personalizada de JavaScript que desea informar de la página después de este fragmento de código:

```cshtml
    @Html.Raw(JavaScriptSnippet.FullScript)
    </head>
```

Modifique los nombres de archivos según la aplicación real. Son los nombres usados por encima de una plantilla de aplicación MVC predeterminada.

## <a name="configuring-application-insights-sdk"></a>Configurar el SDK de Application Insights

SDK de Application Insights para ASP.NET Core puede personalizarse para modificar la configuración predeterminada. Los usuarios de SDK de Application Insights ASP.NET pueden estar familiarizados con el uso de la configuración `ApplicationInsights.config`, o bien modificando `TelemetryConfiguration.Active`. Para ASP.NET Core, configuración se realiza de forma diferente. SDK de ASP.NET Core se agrega a la aplicación mediante integrada de ASP.NET Core [DependencyInjection](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) mecanismo y la configuración de la misma también utilizarían DependencyInjection. Casi todos los cambios de configuración se realizan en el `ConfigureServices()` método de su `Startup.cs` clase, a menos que se indique lo contrario. Siga las siguientes secciones para obtener más información.

> [!NOTE]
> Es importante tener en cuenta que modificar la configuración mediante la modificación de `TelemetryConfiguration.Active` no se recomienda en las aplicaciones ASP.NET Core.

### <a name="configuring-using-applicationinsightsserviceoptions"></a>Configuración mediante ApplicationInsightsServiceOptions

Es posible modificar algunas opciones de configuración comunes pasando `ApplicationInsightsServiceOptions` a `services.AddApplicationInsightsTelemetry();`. A continuación se muestra un ejemplo.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions aiOptions
                    = new Microsoft.ApplicationInsights.AspNetCore.Extensions.ApplicationInsightsServiceOptions();
        // Disables adaptive sampling.
        aiOptions.EnableAdaptiveSampling = false;

        // Disables QuickPulse (Live Metrics stream).
        aiOptions.EnableQuickPulseMetricStream = false;
        services.AddApplicationInsightsTelemetry(aiOptions);
    }
```

La lista exacta de valores configurables en `ApplicationInsightsServiceOptions` puede encontrarse [aquí](https://github.com/Microsoft/ApplicationInsights-aspnetcore/blob/f0b8631e482d25982eb52092103b34e3ff6e5fef/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>muestreo

SDK de Application Insights para ASP.NET Core admite FixedRate y el muestreo adaptable. Muestreo adaptable está habilitado de forma predeterminada. Siga [esto](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications) documento para obtener información sobre cómo configurar el muestreo para aplicaciones ASP.NET Core.

### <a name="adding-telemetryinitializers"></a>Agregar objetos Telemetryinitializer

Para agregar un nuevo `TelemetryInitializer`, agréguelo en el contenedor DependencyInjection como se muestra a continuación. `TelemetryInitializer`agregadas al contenedor DependencyInjection seleccionará el SDK automáticamente.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSingleton<ITelemetryInitializer, MyCustomTelemetryInitializer>();
    }
```

### <a name="removing-telemetryinitializers"></a>Quitar objetos Telemetryinitializer

Para quitar todos o TelemetryInitializers específicos, que están presentes de forma predeterminada, use el siguiente código de ejemplo **después** una llamada a `AddApplicationInsightsTelemetry()`.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // Remove a specific built-in TelemetryInitializer
        var tiToRemove = services.FirstOrDefault<ServiceDescriptor>
                         (t => t.ImplementationType == typeof(AspNetCoreEnvironmentTelemetryInitializer));
        if (tiToRemove != null)
        {
            services.Remove(tiToRemove);
        }

        // Remove all initializers
        // This requires importing namespace using Microsoft.Extensions.DependencyInjection.Extensions;
        services.RemoveAll(typeof(ITelemetryInitializer));
    }
```

### <a name="adding-telemetryprocessors"></a>Adición de los objetos Telemetryprocessor

Los procesadores de telemetría personalizados se pueden agregar a la `TelemetryConfiguration` mediante el método de extensión `AddApplicationInsightsTelemetryProcessor` en `IServiceCollection`. Use el siguiente ejemplo.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // ...
        services.AddApplicationInsightsTelemetry();
        services.AddApplicationInsightsTelemetryProcessor<MyFirstCustomTelemetryProcessor>();

        // If you have more processors:
        services.AddApplicationInsightsTelemetryProcessor<MySecondCustomTelemetryProcessor>();
    }
```

### <a name="configuring-or-removing-default-telemetrymodules"></a>Configurar o quitar TelemetryModules predeterminada

Los siguientes módulos de recopilación automática están habilitados de forma predeterminada y son responsables de recopilar automáticamente datos de telemetría. Puede deshabilitar y configurados para modificar el comportamiento predeterminado.

1. `RequestTrackingTelemetryModule`
1. `DependencyTrackingTelemetryModule`
1. `PerformanceCollectorModule`
1. `QuickPulseTelemetryModule`
1. `AppServicesHeartbeatTelemetryModule`
1. `AzureInstanceMetadataTelemetryModule`

Para configurar cualquier predeterminada `TelemetryModule`, utilice el método de extensión `ConfigureTelemetryModule<T>` en `IServiceCollection` tal como se muestra en el ejemplo siguiente.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddApplicationInsightsTelemetry();

        // The following configures DependencyTrackingTelemetryModule.
        // Similarly, any other default modules can be configured.
        services.ConfigureTelemetryModule<DependencyTrackingTelemetryModule>((module, o) =>
                        {
                            module.EnableW3CHeadersInjection = true;
                        });

        // The following removes PerformanceCollectorModule to disable perf-counter collection.
        // Similarly, any other default modules can be removed.
        var performanceCounterService = services.FirstOrDefault<ServiceDescriptor>(t => t.ImplementationType == typeof(PerformanceCollectorModule));
        if (performanceCounterService != null)
        {
         services.Remove(performanceCounterService);
        }
    }
```

### <a name="configuring-telemetry-channel"></a>Configuración de canal de telemetría

El canal predeterminado utilizado es el `ServerTelemetryChannel`. Se puede invalidar mediante el ejemplo siguiente.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // use the following to replace the default channel with InMemoryChannel.
        // this can also be applied to ServerTelemetryChannel as well.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

*1. Me gustaría realizar un seguimiento de telemetría adicionales aparte de los datos de telemetría recopilará automáticamente. ¿Cómo lo hago?*

* Obtener una instancia de `TelemetryClient` mediante la inserción del Constructor y llamar a los necesarios `TrackXXX()` método en él. No se recomienda crear nuevos `TelemetryClient` instancias de aplicación de ASP.NET Core, como una instancia singleton de `TelemetryClient` ya está registrado en el contenedor de DI, que comparte `TelemetryConfiguration` con el resto de los datos de telemetría. Crear un nuevo `TelemetryClient` instancia se recomienda únicamente si debe tener una configuración independiente del resto de los datos de telemetría. En el ejemplo siguiente se muestra cómo realizar el seguimiento de un controlador de telemetría adicional.

```csharp
public class HomeController : Controller
{
    private TelemetryClient telemetry;

    // use constructor injection to get TelemetryClient instance
    public HomeController(TelemetryClient telemetry)
    {
        this.telemetry = telemetry;
    }

    public IActionResult Index()
    {
        // call required TrackXXX method.
        this.telemetry.TrackEvent("HomePageRequested");
        return View();
    }
```

 Consulte [métricas personalizadas referencia de API de Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/) para obtener la descripción de los datos personalizados de informes en Application Insights.

*2. Algunas plantillas de Visual Studio usan el método de extensión UseApplicationInsights() en IWebHostBuilder para habilitar Application Insights. ¿Este uso aún es válida?*

* Habilitación de Application Insights con este método es válida y se usa en Visual Studio de incorporación y en las extensiones de aplicación Web de Azure también. Sin embargo, se recomienda utilizar `services.AddApplicationInsightsTelemery()` , ya que proporciona sobrecargas para controlar de alguna configuración. Ambas método internamente hace lo mismo, por tanto, si no hay ninguna configuración personalizada que se aplicará, al llamar a cualquiera está bien.

*3. Se implementa mi aplicación ASP.NET Core en Azure Web Apps. ¿Debo habilitar la extensión de Application Insights desde Web Apps?*

* Si SDK está instalado en tiempo de compilación, como se muestra en este artículo, no es necesario para habilitar la extensión de Application Insights desde el portal de aplicaciones Web. Incluso si se instala la extensión, se retroceso, cuando detecta que el SDK ya se agrega a la aplicación. Habilitación de la extensión de Application Insights le libera de instalar y actualizar el SDK de la aplicación. Sin embargo, la habilitación de Application Insights según este artículo es más flexible por los motivos siguientes.
    1. Telemetría de Application Insights seguirán funcionando
        1. Todos los sistemas operativos: Windows, Linux, Mac
        1. Todos los publicación modo - independiente o dependiente del marco.
        1. Todas las plataformas de destino, incluida la versión completa de .NET Framework.
        1. Todas las opciones de hospedaje: aplicación Web de Azure, las máquinas virtuales, Linux, contenedores, AKS, que no son de Azure.
    1. Datos de telemetría pueden verse localmente, al depurar desde Visual Studio.
    1. Permite el seguimiento de telemetría personalizada adicional utilizando `TrackXXX()` API.
    1. Tiene control total sobre la configuración.

*4. ¿Puedo habilitar la supervisión de Application Insights con herramientas como Monitor de estado?*

* No. [Monitor de estado](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) y su reemplazo próximas [IISConfigurator](https://github.com/Microsoft/ApplicationInsights-Announcements/issues/21) actualmente, solo admite ASP.NET. El documento se actualizará cuando se admite para la aplicación de ASP.NET Core está disponible.

*5. ¿Tengo una aplicación ASP.NET Core 2.0? ¿No es Application Insights habilita automáticamente para ellos sin que hiciera nada?*

* `Microsoft.AspNetCore.All` metapaquete 2.0 incluye el SDK de Application Insights (versión 2.1.0) y si ejecuta la aplicación en el depurador de Visual Studio, Visual Studio permite la información de la aplicación y muestra los datos de telemetría localmente en el propio IDE. No se envió telemetría al servicio Application Insights, a menos que explícitamente se especifica una clave de instrumentación. Se recomienda seguir las instrucciones de este artículo para habilitar Application Insights, incluso para 2.0 aplicaciones.

*6. Ejecutar mi aplicación en Linux. ¿Todas las características también se admiten en Linux?*

* Sí. Compatibilidad de características para el SDK es la misma en todas las plataformas, con las siguientes excepciones:
    1. Contadores de rendimiento no se admiten aún en que no sean Windows. Este documento se actualizará cuando se agrega compatibilidad con Linux.
    1. Aunque `ServerTelemetryChannel` está habilitada de forma predeterminada, si la aplicación se ejecuta en el que no son de windows, el canal no crea automáticamente una carpeta de almacenamiento local para mantener los datos de telemetría temporalmente si hay problemas de red. Esta limitación hace que la telemetría se perderán si existen problemas de servidor o de red temporales. La solución alternativa para este problema es para que el usuario configurar una carpeta local para el canal, como se muestra a continuación.

```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        // The following will configure channel to use the given folder to temporarily
        // store telemetry items during network or application insights server issues.
        // User should ensure that the given folder already exists,
        // and that application has read/write permissions.
        services.AddSingleton(typeof(ITelemetryChannel),
                                new ServerTelemetryChannel () {StorageFolder = "/tmp/myfolder"});
        services.AddApplicationInsightsTelemetry();
    }
```
