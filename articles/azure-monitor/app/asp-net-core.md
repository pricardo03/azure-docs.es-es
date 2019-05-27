---
title: Azure Application Insights para ASP.NET Core | Microsoft Docs
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
ms.date: 05/21/2019
ms.author: mbullwin
ms.openlocfilehash: 8522b1d0a8f8466870966d3f11ce66f7bf15672b
ms.sourcegitcommit: db3fe303b251c92e94072b160e546cec15361c2c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66016364"
---
# <a name="application-insights-for-aspnet-core-applications"></a>Application Insights para aplicaciones de ASP.NET Core

En este artículo se describe cómo habilitar Application Insights para un [ASP.NET Core](https://docs.microsoft.com/aspnet/core) aplicación. Cuando haya completado las instrucciones de este artículo, Application Insights iniciará la recopilación de solicitudes, dependencias, excepciones, contadores de rendimiento, los latidos y los registros de la aplicación ASP.NET Core. La aplicación de ejemplo es un [aplicación MVC](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app) destinadas a `netcoreapp2.2`, pero estas instrucciones son aplicables a todas las aplicaciones de ASP.NET Core.

## <a name="supported-scenarios"></a>Escenarios admitidos

El [Application Insights SDK (Kit de desarrollo de Software) para ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore) puede supervisar sus aplicaciones con independencia de dónde y cómo se ejecuta la aplicación. Si la aplicación se está ejecutando y tiene conectividad de red en Azure, se pueden recopilar datos de telemetría. Esto significa que la supervisión de Application Insights se admite en todas partes que .NET Core es compatible. Esta compatibilidad incluye cualquier sistema operativo (Windows, Linux, Mac), el método de hospedaje (en proceso vs fuera de proceso), el método de implementación (dependiente del marco frente a autocontenida), servidor Web (IIS, Kestrel), plataforma de hospedaje (Azure Web Apps, máquina virtual de Azure, Docker, Azure Kubernetes Service (AKS) y así sucesivamente.) o IDE (Visual Studio, VS Code, línea de comandos).

## <a name="prerequisites"></a>Requisitos previos

- Una aplicación ASP.NET Core funcione. Siga el [Guía de introducción a ASP.NET Core getting](https://docs.microsoft.com/aspnet/core/getting-started/) para crear una aplicación ASP.NET Core, si es necesario.
- Una válido Application Insights clave de instrumentación, que es necesario para enviar los datos de telemetría al servicio Application Insights. Siga el [crear un recurso instrucciones](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) para crear un nuevo recurso de Application Insights, si es necesario y obtener una clave de instrumentación.

## <a name="enable-application-insights-server-side-telemetry-visual-studio"></a>Habilitar la telemetría de Application Insights del lado servidor (Visual Studio)

1. Abra el proyecto en Visual Studio.

    > [!TIP]
    > Mientras no es un paso obligatorio, puede resultar útil configurar el control de código fuente para el proyecto para que pueda seguir todos los cambios realizados por Application Insights. Para habilitar la selección del control de código fuente **archivo** > **agregar al Control de código fuente**.

2. Seleccione **Proyecto** > **Agregar Telemetría de Application Insights**.

3. Seleccione **Comenzar**. En función de la versión de Visual Studio, el texto puede variar ligeramente. Algunas versiones anteriores tienen un botón **Comenzar gratis** en cambio.

4. Seleccione la suscripción y luego seleccione **Recurso** > **Registrar**.

5. Después de agregar Application Insights al proyecto, compruebe para confirmar que está usando la versión estable más reciente del SDK. Vaya a **proyecto** > **administrar paquetes de NuGet** > **Microsoft.ApplicationInsights.AspNetCore** > Si es necesario elegir **Actualización**.

     ![Captura de pantalla de administración de pantalla del paquete de NuGet con seleccionado para la actualización del paquete de Application Insights](./media/asp-net-core/update-nuget-package.png)

6. Si ha seguido la sugerencia opcional y agrega el proyecto de control de código fuente, a continuación, puede ir a **vista** > **Team Explorer** > **cambios** y Seleccione cada archivo individual para obtener una vista de diferencias de los cambios realizados desde Agregar telemetría de Application Insights.

## <a name="enable-application-insights-server-side-telemetry-without-visual-studio"></a>Habilitar la telemetría de Application Insights del lado servidor (sin Visual Studio)

1. Instalar el [paquete NuGet del SDK de Application Insights para ASP.NET Core](https://nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore). Se recomienda usar siempre la última versión estable. Notas de la versión completa del SDK se encuentra en la [abrir el repositorio de GitHub de origen](https://github.com/Microsoft/ApplicationInsights-aspnetcore/releases).

    El fragmento de código siguiente muestra los cambios que se agregará a su proyecto `.csproj` archivo.

    ```xml
        <ItemGroup>
          <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.6.1" />
        </ItemGroup>
    ```

2. Agregar `services.AddApplicationInsightsTelemetry();` a la `ConfigureServices()` método en su `Startup` clase. Ejemplo completo siguiente.

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

3. Configurar la clave de instrumentación.

    Aunque es posible proporcionar la clave de instrumentación como argumento a `AddApplicationInsightsTelemetry`, se recomienda que especifique la clave de instrumentación en la configuración. Lo siguiente muestra cómo especificar una clave de instrumentación en `appsettings.json`. Asegúrese de que `appsettings.json` se copia en la carpeta raíz de la aplicación durante la publicación.

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

    Como alternativa, también puede especificarse la clave de instrumentación en cualquiera de las siguientes variables de entorno.

    APPINSIGHTS_INSTRUMENTATIONKEY

    ApplicationInsights:InstrumentationKey

    Ejemplo:

    `SET ApplicationInsights:InstrumentationKey=putinstrumentationkeyhere`

    `SET APPINSIGHTS_INSTRUMENTATIONKEY=putinstrumentationkeyhere`

    `APPINSIGHTS_INSTRUMENTATIONKEY` Normalmente se utiliza para especificar la clave de instrumentación para las aplicaciones implementadas en Azure Web Apps.

    > [!NOTE]
    > Una clave de instrumentación especificada en el código wins a través de la variable de entorno `APPINSIGHTS_INSTRUMENTATIONKEY`, que gane sobre otras opciones.

## <a name="run-your-application"></a>Ejecutar la aplicación

 Ejecute la aplicación y realizar solicitudes a él. Datos de telemetría ahora deben comenzar a fluir a Application Insights. La siguiente telemetría se recopila automáticamente mediante el SDK de Application Insights.

|Las solicitudes y dependencias |Detalles|
|---------------|-------|
|Solicitudes | Solicitudes web entrantes a la aplicación. |
|Http/Https | Las llamadas realizadas con `HttpClient`. |
|SQL | Las llamadas realizadas con `SqlClient`. |
|[Almacenamiento de Azure](https://www.nuget.org/packages/WindowsAzure.Storage/) | Llamadas realizadas con el cliente de Azure Storage. |
|[SDK de cliente de centro de eventos](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | La versión 1.1.0 y versiones posteriores. |
|[SDK de cliente de Service Bus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Versión 3.0.0 y versiones posteriores. |
|Azure Cosmos DB | Solo realiza un seguimiento automáticamente si se usa HTTP/HTTPS. Application Insights no capturará el modo TCP. |

### <a name="performance-counters"></a>Contadores de rendimiento

Compatibilidad con [los contadores de rendimiento](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/) en ASP.NET Core se limita a lo siguiente

   * SDK versión 2.4.1 y anteriormente recopila contadores de rendimiento si la aplicación se ejecuta en Azure Web Apps (Windows)
   * SDK versión 2.7.0-beta3 y anteriormente recopila contadores de rendimiento si la aplicación se ejecuta en Windows y destinadas a `NETSTANDARD2.0` o superior.
   * Para las aplicaciones destinadas a .NET Framework, los contadores de rendimiento se admiten en todas las versiones SDK de.
   * En este artículo se actualizará cuando se agrega compatibilidad con contadores de rendimiento en Linux.

### <a name="ilogger-logs"></a>Registros de ILogger

[Los registros de ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger) de gravedad `Warning` o versiones posteriores son automáticamente capturados desde el SDK versión 2.7.0-beta3 o superior.

### <a name="live-metrics"></a>Live Metrics

Puede tardar unos minutos para la telemetría comenzar a aparecer en el portal. Para comprobar rápidamente si todo funciona, es mejor usar [Live Metrics](https://docs.microsoft.com/azure/application-insights/app-insights-live-stream), mientras que realizar solicitudes a la aplicación en ejecución.

## <a name="enable-client-side-telemetry-for-web-applications"></a>Habilitar la telemetría del lado cliente para aplicaciones Web

Los pasos anteriores son suficientes para iniciar la recopilación de telemetría del lado servidor. Si la aplicación tiene componentes de cliente, a continuación, siga estos pasos para comenzar a recopilar [telemetría de uso](https://docs.microsoft.com/azure/azure-monitor/app/usage-overview) desde allí.

1. En `_ViewImports.cshtml`, agregue la inyección de código:

    ```cshtml
        @inject Microsoft.ApplicationInsights.AspNetCore.JavaScriptSnippet JavaScriptSnippet
    ```

2. En `_Layout.cshtml`, insertar HtmlHelper al final de `<head>` sección pero antes de cualquier otra secuencia de comandos. Debe insertarse cualquier telemetría personalizada de JavaScript que desea informar de la página después de este fragmento de código:

    ```cshtml
        @Html.Raw(JavaScriptSnippet.FullScript)
        </head>
    ```

El `.cshtml` son los nombres de archivo mencionados anteriormente desde una plantilla de aplicación de MVC predeterminada. En última instancia, debe estar presente en el fragmento de JavaScript para habilitar correctamente la supervisión de cliente para la aplicación la `<head>` sección de cada página de la aplicación que desea supervisar. Para agregar el fragmento de código de Javascript para esta plantilla de aplicación `_Layout.cshtml` eficazmente llevará a cabo este objetivo. Si el proyecto no tiene este archivo específico se puede agregar [supervisión de cliente](https://docs.microsoft.com/azure/azure-monitor/app/website-monitoring). Solo necesita como agregaría el código JavaScript a un archivo equivalente que controla el `<head>` de todas las páginas dentro de la aplicación, o como alternativa, podría agregar el fragmento de código individuales de varias páginas, aunque esto sería difícil de mantener y no suele ser se recomienda.

## <a name="configuring-application-insights-sdk"></a>Configurar el SDK de Application Insights

SDK de Application Insights para ASP.NET Core puede personalizarse para modificar la configuración predeterminada. Los usuarios de SDK de Application Insights ASP.NET pueden estar familiarizados con el uso de la configuración `ApplicationInsights.config`, o bien modificando `TelemetryConfiguration.Active`. Para ASP.NET Core, configuración se realiza de forma diferente. El SDK de ASP.NET Core se agrega a la aplicación y se configura utilizando integrada de ASP.NET Core [inserción de dependencias](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection). Casi todos los cambios de configuración se realizan en el `ConfigureServices()` método de su `Startup.cs` clase, a menos que se indique lo contrario. Siga las siguientes secciones para obtener más información.

> [!NOTE]
>  Cambiando configuración modificando `TelemetryConfiguration.Active` no se recomienda en las aplicaciones ASP.NET Core.

### <a name="configuring-using-applicationinsightsserviceoptions"></a>Configuración mediante ApplicationInsightsServiceOptions

Es posible modificar algunas opciones de configuración comunes pasando `ApplicationInsightsServiceOptions` a `AddApplicationInsightsTelemetry`. A continuación se muestra un ejemplo.

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

La lista exacta de valores configurables en `ApplicationInsightsServiceOptions` puede encontrarse [aquí](https://github.com/microsoft/ApplicationInsights-aspnetcore/blob/develop/src/Microsoft.ApplicationInsights.AspNetCore/Extensions/ApplicationInsightsServiceOptions.cs).

### <a name="sampling"></a>muestreo

SDK de Application Insights para ASP.NET Core admite FixedRate y el muestreo adaptable. Muestreo adaptable está habilitado de forma predeterminada. Siga nuestros [orientación sobre el muestreo adaptable](../../azure-monitor/app/sampling.md#configuring-adaptive-sampling-for-aspnet-core-applications), para obtener información sobre cómo configurar el muestreo para aplicaciones ASP.NET Core.

### <a name="adding-telemetryinitializers"></a>Agregar objetos Telemetryinitializer

[Los inicializadores de telemetría](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#add-properties-itelemetryinitializer) se utilizan cuando desea definir las propiedades globales que se envían con toda la telemetría.

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

Los procesadores de telemetría personalizados se pueden agregar a la `TelemetryConfiguration` mediante el método de extensión `AddApplicationInsightsTelemetryProcessor` en `IServiceCollection`. Los procesadores de telemetría se utilizan en [escenarios de filtrado avanzado](https://docs.microsoft.com/azure/azure-monitor/app/api-filtering-sampling#filtering-itelemetryprocessor) para permitir el control más directo sobre lo que se incluirán o excluirán de la telemetría envían al servicio Application Insights. Use el siguiente ejemplo.

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

Application Insights usa módulos de telemetría como una manera de [auto-recopilar información útil](https://docs.microsoft.com/azure/azure-monitor/app/auto-collect-dependencies) sobre determinadas cargas de trabajo sin necesidad de configuración adicional.

Los siguientes módulos de recopilación automática están habilitados de forma predeterminada y son responsables de recopilar automáticamente datos de telemetría. Puede deshabilitar y configurados para modificar el comportamiento predeterminado.

* `RequestTrackingTelemetryModule`
* `DependencyTrackingTelemetryModule`
* `PerformanceCollectorModule`
* `QuickPulseTelemetryModule`
* `AppServicesHeartbeatTelemetryModule`
* `AzureInstanceMetadataTelemetryModule`

Para configurar cualquier predeterminada `TelemetryModule`, utilice el método de extensión `ConfigureTelemetryModule<T>` en `IServiceCollection` tal como se muestra en el ejemplo siguiente.

```csharp
using Microsoft.ApplicationInsights.DependencyCollector;
using Microsoft.ApplicationInsights.Extensibility.PerfCounterCollector;

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
using Microsoft.ApplicationInsights.Channel;

    public void ConfigureServices(IServiceCollection services)
    {
        // use the following to replace the default channel with InMemoryChannel.
        // this can also be applied to ServerTelemetryChannel as well.
        services.AddSingleton(typeof(ITelemetryChannel), new InMemoryChannel() {MaxTelemetryBufferCapacity = 19898 });

        services.AddApplicationInsightsTelemetry();
    }
```

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="i-want-to-track-additional-telemetry-other-than-the-auto-collected-telemetry-how-do-i-do-it"></a>Me gustaría realizar un seguimiento de telemetría adicional que no sea la telemetría recopilará automáticamente. ¿Cómo lo hago?

Obtener una instancia de `TelemetryClient` mediante la inserción del Constructor y llamar a los necesarios `TrackXXX()` método en él. No se recomienda crear nuevos `TelemetryClient` instancias de aplicación de ASP.NET Core, como una instancia singleton de `TelemetryClient` ya está registrado en el contenedor de DI, que comparte `TelemetryConfiguration` con el resto de los datos de telemetría. Crear un nuevo `TelemetryClient` instancia se recomienda únicamente si debe tener una configuración independiente del resto de los datos de telemetría. En el ejemplo siguiente se muestra cómo realizar el seguimiento de un controlador de telemetría adicional.

```csharp
using Microsoft.ApplicationInsights;

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

 Consulte [métricas personalizadas referencia de API de Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/api-custom-events-metrics/) para obtener una descripción de los datos personalizados de informes en Application Insights.

### <a name="some-visual-studio-templates-used-useapplicationinsights-extension-method-on-iwebhostbuilder-to-enable-application-insights-is-this-usage-still-valid"></a>Algunas plantillas de Visual Studio usan el método de extensión UseApplicationInsights() en IWebHostBuilder para habilitar Application Insights. ¿Este uso aún es válida?

Habilitación de Application Insights con este método es válida y se usa en Visual Studio de incorporación y en las extensiones de aplicación Web de Azure también. Sin embargo, se recomienda utilizar `services.AddApplicationInsightsTelemetry()` , ya que proporciona sobrecargas para controlar de alguna configuración. Ambos métodos internamente hacen lo mismo, por tanto, si no hay ninguna configuración personalizada que se aplicará, al llamar a cualquiera está bien.

### <a name="i-am-deploying-my-aspnet-core-application-to-azure-web-apps-should-i-still-enable-the-application-insights-extension-from-web-apps"></a>Se implementa mi aplicación ASP.NET Core en Azure Web Apps. ¿Debo habilitar la extensión de Application Insights desde Web Apps?

Si está instalado el SDK en tiempo de compilación, como se muestra en este artículo, no es necesario para habilitar la extensión de Application Insights desde el portal de App Service. Incluso si la extensión está instalada, realizará una off cuando detecta que el SDK se ha agregado a la aplicación. Habilitación de la extensión de Application Insights le libera de la instalación y actualización del SDK. Sin embargo, la habilitación de Application Insights según este artículo es más flexible por los motivos siguientes.
   * Telemetría de Application Insights seguirán funcionando:
       * Todos los sistemas operativos: Windows, Linux, Mac
       * Todos los modos de - de publicación independiente o dependiente del marco.
       * Todas las plataformas de destino, incluida la versión completa de .NET Framework.
       * Todas las opciones de hospedaje: aplicación Web de Azure, las máquinas virtuales, Linux, contenedores, AKS, que no son de Azure.
   * Datos de telemetría pueden verse localmente, al depurar desde Visual Studio.
   * Permite el seguimiento de telemetría personalizada adicional utilizando `TrackXXX()` API.
   * Tiene control total sobre la configuración.

### <a name="can-i-enable-application-insights-monitoring-using-tools-like-status-monitor"></a>¿Puedo habilitar la supervisión de Application Insights con herramientas como Monitor de estado?

No. [Monitor de estado](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now) y su reemplazo próximas [v2 del Monitor de estado](https://docs.microsoft.com/azure/azure-monitor/app/status-monitor-v2-overview) actualmente admite ASP.NET 4.x solo.

### <a name="i-have-an-aspnet-core-20-application-isnt-application-insights-automatically-enabled-without-me-doing-anything"></a>Tengo una aplicación ASP.NET Core 2.0. ¿No está habilitada automáticamente Application Insights sin que hiciera nada?

`Microsoft.AspNetCore.All` metapaquete 2.0 incluye el SDK de Application Insights (versión 2.1.0) y si se ejecuta la aplicación en el depurador de Visual Studio, Visual Studio habilita Application Insights y muestra los datos de telemetría localmente en el propio IDE. No se envió telemetría al servicio Application Insights, a menos que explícitamente se especifica una clave de instrumentación. Se recomienda seguir las instrucciones de este artículo para habilitar Application Insights, incluso para 2.0 aplicaciones.

### <a name="i-run-my-application-in-linux-are-all-features-supported-in-linux-as-well"></a>Ejecutar mi aplicación en Linux. ¿Todas las características también se admiten en Linux?

* Sí. Compatibilidad de características para el SDK es la misma en todas las plataformas, con las siguientes excepciones:

    * Contadores de rendimiento no se admiten aún en que no sean Windows.
    * Aunque `ServerTelemetryChannel` está habilitada de forma predeterminada, si la aplicación se ejecuta en Linux o MacOS, el canal no crea automáticamente una carpeta de almacenamiento local para mantener los datos de telemetría temporalmente si hay problemas de red. Esta limitación hace que la telemetría se perderán si existen problemas de servidor o de red temporales. La solución alternativa para este problema es para que el usuario configurar una carpeta local para el canal, como se muestra a continuación.

```csharp
using Microsoft.ApplicationInsights.Channel;
using Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel;

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

## <a name="open-source-sdk"></a>SDK de código abierto
[Lectura y contribución al código](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="video"></a>Vídeo

- Externo vídeo paso a paso acerca de cómo [configurar Application Insights con .NET Core y Visual Studio](https://www.youtube.com/watch?v=NoS9UhcR4gA&t) desde cero.

## <a name="next-steps"></a>Pasos siguientes
* [Explore los flujos de usuarios](../../azure-monitor/app/usage-flows.md) para saber cómo navegan por la aplicación.
* [Configure la colección de instantáneas](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) para ver el estado del código fuente y las variables en el momento en que se produzca una excepción.
* [Use la API](../../azure-monitor/app/api-custom-events-metrics.md) para enviar sus propios eventos y métricas para obtener una vista más detallada del rendimiento y el uso de la aplicación.
* [Las pruebas de disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md) comprueban la aplicación constantemente desde cualquier parte del mundo.
