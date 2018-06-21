---
title: Azure Application Insights para ASP.NET Core | Microsoft Docs
description: Supervise la disponibilidad, el rendimiento y el uso de las aplicaciones web.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 3b722e47-38bd-4667-9ba4-65b7006c074c
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 06/03/2018
ms.author: mbullwin
ms.openlocfilehash: 261bc78bfe427173ba81eef731e33eddd2ec379b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294282"
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights para ASP.NET Core

Azure Application Insights proporciona una supervisión detallada de la aplicación web hasta el nivel de código. Puede supervisar fácilmente la disponibilidad, el rendimiento y el uso de su aplicación web. También puede identificar y diagnosticar errores en la aplicación rápidamente sin tener que esperar a que un usuario informe de ellos.

En este artículo se explica cómo crear una aplicación de las [páginas de Razor](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) de ASP.NET Core de ejemplo en Visual Studio y cómo empezar la supervisión con Azure Application Insights.

## <a name="prerequisites"></a>requisitos previos

- SDK de .NET Core 2.0.0 o posterior.
- Versión 15.7.3 o posterior de [Visual Studio 2017](https://www.visualstudio.com/downloads/) con la carga de trabajo de desarrollo web y ASP.NET. 

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Creación de un proyecto de ASP.NET Core en Visual Studio

1. Haga clic con el botón derecho en **Visual Studio 2017** e inícielo como administrador.
2. Seleccione **Archivo** > **Nuevo** > **Proyecto** (Ctrl-Mayús-N).

   ![Captura de pantalla del menú Archivo > Nuevo > Proyecto de Visual Studio](./media/app-insights-asp-net-core/001-new-project.png)

3. Expanda **Visual C#** > seleccione **.NET Core** > **Aplicación web ASP.NET Core**. Escriba un **Nombre** > **Nombre de la solución** > active **Crear nuevo repositorio Git**.

   ![Captura de pantalla del Asistente Archivo > Nuevo > Proyecto de Visual Studio](./media/app-insights-asp-net-core/002-asp-net-core-web-application.png)

4. Seleccione **.Net Core** > **ASP.NET Core 2.0** **Web Application** > **Aceptar**.

    ![Captura de pantalla del menú de selección Archivo > Nuevo > Proyecto de Visual Studio](./media/app-insights-asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Búsqueda de Application Insights

De forma predeterminada, en Visual Studio versión 2015 Update 2 o posterior con un proyecto basado en ASP.NET Core 2 +, puede utilizar la [búsqueda de Application Insights](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-visual-studio) incluso antes que agregar explícitamente Application Insights al proyecto.

Para probar esta funcionalidad, siga estos pasos:

1. Ejecute la aplicación mediante un clic en IIS Express ![Captura de pantalla del icono de IIS Express de Visual Studio](./media/app-insights-asp-net-core/004-iis-express.png)

2. Seleccione **Ver** > **Otras ventanas** > **Búsqueda de Application Insights**.

   ![Captura de pantalla de Herramientas de diagnóstico de Visual Studio](./media/app-insights-asp-net-core/005-view-other-windows-search.png)

3. La telemetría de la sesión de depuración está disponible actualmente solo para análisis locales. Para habilitar completamente Application Insights, seleccione **Disponibilidad de telemetría** en la esquina superior derecha y siga los pasos descritos en la sección de a continuación.

   ![Captura de pantalla de Búsqueda de Application Insights en Visual Studio](./media/app-insights-asp-net-core/006-search.png)

> [!NOTE]
> Para obtener más información sobre cómo Visual Studio activa características como [Búsqueda Application Insights](app-insights-visual-studio.md) y [CodeLens](app-insights-visual-studio-codelens.md) localmente antes de agregar Application Insights al proyecto de ASP.NET Core, consulte la explicación al [final de este artículo.](#Application-Insights-search-continued)

## <a name="add-application-insights-telemetry"></a>Incorporación de los datos de telemetría de Application Insights

1. Seleccione **Proyecto** > **Agregar telemetría de Application Insights...**. También puede hacer clic con el botón derecho en **Servicios conectados** y seleccionar Agregar servicio conectado.

    ![Captura de pantalla del menú de selección Archivo > Nuevo > Proyecto de Visual Studio](./media/app-insights-asp-net-core/007-project-add-telemetry.png)

2. Seleccione **Comenzar**. En función de la versión de Visual Studio, el texto puede variar ligeramente. Algunas versiones anteriores tienen un botón **Comenzar gratis**.

    ![Captura de pantalla del menú de selección Archivo > Nuevo > Proyecto de Visual Studio](./media/app-insights-asp-net-core/008-get-started.png)

3. Seleccione **Suscripción** > **Recurso** > **Registro**.

## <a name="changes-made-to-your-project"></a>Cambios en el proyecto

Application Insights tiene poca sobrecarga. Para revisar las modificaciones que se realizaron en el proyecto al agregar la telemetría de Application Insights:

Seleccione **Ver** > **Team Explorer** (Ctrl+\, Ctrl+M) > **Proyecto** > **Cambios**

- Cuatro cambios en total:

  ![Captura de pantalla de los archivos que se modificaron al agregar Application Insights](./media/app-insights-asp-net-core/009-changes.png)

- Se crea un archivo nuevo:

   _ConnectedService.json_

```json
{
  "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
  "Version": "8.12.10405.1",
  "GettingStartedDocument": {
    "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
  }
}
```

- Se modifican los tres archivos: (comentarios adicionales agregados para resaltar los cambios)

  _appsettings.json_

```json
{
  "Logging": {
    "IncludeScopes": false,
    "LogLevel": {
      "Default": "Warning"
    }
  },
// Changes to file post adding Application Insights Telemetry:
  "ApplicationInsights": {
    "InstrumentationKey": "10101010-1010-1010-1010-101010101010"
  }
}
//
```

  _ContosoDotNetCore.csproj_

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
 <!--Changes to file post adding Application Insights Telemetry:-->
    <ApplicationInsightsResourceId>/subscriptions/2546c5a9-fa20-4de1-9f4a-62818b14b8aa/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/DotNetCore</ApplicationInsightsResourceId>
    <ApplicationInsightsAnnotationResourceId>/subscriptions/2546c5a9-fa20-4de1-9f4a-62818b14b8aa/resourcegroups/Default-ApplicationInsights-EastUS/providers/microsoft.insights/components/DotNetCore</ApplicationInsightsAnnotationResourceId>
<!---->
  </PropertyGroup>
  <ItemGroup>
 <!--Changes to file post adding Application Insights Telemetry:-->
    <PackageReference Include="Microsoft.ApplicationInsights.AspNetCore" Version="2.1.1" />
<!---->
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.8" />
  </ItemGroup>
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.4" />
  </ItemGroup>
<!--Changes to file post adding Application Insights Telemetry:-->
  <ItemGroup>
    <WCFMetadata Include="Connected Services" />
  </ItemGroup>
<!---->
</Project>
```

   _Program.cs_

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

namespace DotNetCore
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
// Change to file post adding Application Insights Telemetry:
                .UseApplicationInsights()
//
                .UseStartup<Startup>()
                .Build();
    }
}
```

## <a name="synthetic-transactions-with-powershell"></a>Transacciones sintéticas con PowerShell

Para automatizar las solicitudes a la aplicación con las transacciones sintéticas.

1. Ejecute la aplicación mediante un clic en IIS Express ![Captura de pantalla del icono de IIS Express de Visual Studio](./media/app-insights-asp-net-core/004-iis-express.png)

2. Copie la dirección URL de la barra de direcciones del explorador. Se encuentra en el formato http://localhost:{random número de puerto}

   ![Captura de pantalla de la barra de direcciones URL del explorador](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Ejecute el bucle de PowerShell siguiente para crear 100 transacciones sintéticas en la aplicación de prueba. Modifique el número de puerto que aparece después de **localhost:** para que coincida con la dirección URL que copió en el paso anterior.

   ```PowerShell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-application-insights-portal"></a>Apertura del portal de Application Insights

Después de ejecutar la instancia de PowerShell de la sección anterior, inicie Application Insights para ver las transacciones y confirmar que se recopilan los datos. 

En el menú de Visual Studio, seleccione **Proyecto** > **Application Insights** > **Abrir portal de Application Insights**

   ![Captura de pantalla de la información general de Application Insights](./media/app-insights-asp-net-core/010-portal.png)

> [!NOTE]
> En la captura de pantalla de ejemplo anterior, **Live Stream**, **Tiempo de carga de la vista de página** y **Solicitudes con error** no se recopilan actualmente. En la sección siguiente se explicará cómo agregar cada una de ellas. Si ya recopila **Live Stream** y **Tiempo de carga de la vista de página**, solo siga los pasos para **Solicitudes con error**.

## <a name="collect-failed-requests-live-stream--page-view-load-time"></a>Recopilación de Solicitudes con error, Live Stream y Tiempo de carga de la vista de página

### <a name="failed-requests"></a>Solicitudes con error

Técnica, las **solicitudes con error** se están recopilando, pero todavía no ocurre ninguna. Para acelerar el proceso, se puede agregar una excepción personalizada al proyecto existente para simular una excepción real. Si la aplicación sigue ejecutándose en Visual Studio, antes de continuar debe **detener la depuración** (Mayús+F5)

1. En el **Explorador de soluciones** > expanda **Páginas** > **About.cshtml** > abra **About.cshtml.cs**.

   ![Captura de pantalla del Explorador de soluciones de Visual Studio](./media/app-insights-asp-net-core/011-about.png)

2. Agregue una excepción en ``Message=`` y guarde el cambio en el archivo.

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.AspNetCore.Mvc.RazorPages;

    namespace DotNetCore.Pages
    {
        public class AboutModel : PageModel
        {
            public string Message { get; set; }

            public void OnGet()
            {
                Message = "Your application description page.";
                throw new Exception("Test Exception");
            }
        }
    }
    ```

### <a name="live-stream"></a>Live Stream

Para acceder a la funcionalidad Live Stream de Application Insights con la actualización de ASP.NET Core a los paquetes NuGet **Microsoft.ApplicationInsights.AspNetCore 2.2.0**.

En Visual Studio, seleccione **Proyecto** > **Administrar paquetes NuGet** > **Microsoft.ApplicationInsights.AspNetCore** > Versión **2.2.0** > **Actualizar**.

  ![Captura de pantalla del Administrador de paquetes NuGet](./media/app-insights-asp-net-core/012-nuget-update.png)

Verá varios mensajes de confirmación. Lea y acepte si está de acuerdo con los cambios.

### <a name="page-view-load-time"></a>Tiempo de carga de la vista de página

1. En Visual Studio, vaya al **Explorador de soluciones** > **Páginas** > se deben modificar dos archivos: __Layout.cshtml_ y __ViewImports.cshtml_

2. En __ViewImports.cshtml_, agregue:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. En **Layout.cshtml**, agregue la línea siguiente antes de la etiqueta ``</head>`` y antes de cualquier otro script.

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-live-stream"></a>Prueba de Solicitudes con error, Tiempo de carga de la vista de página, Live Stream

Para probar y confirmar que todo funciona:

1. Ejecute la aplicación mediante un clic en IIS Express ![Captura de pantalla del icono de IIS Express de Visual Studio](./media/app-insights-asp-net-core/0012-iis-express.png)

2. Vaya a la página **About** (Información) para desencadenar la excepción de prueba. Si se está ejecutando en el modo de depuración, deberá hacer clic en **Continuar** en Visual Studio antes de que Application Insights muestre la excepción.

3. Vuelva a ejecutar el script de transacción de PowerShell simulado anterior (es posible que tenga que ajustar el número de puerto en el script).

4. Si todavía no está abierta la ventana de información general, en el menú de Visual Studio seleccione **Proyecto** > **Application Insights** > **Abrir portal de Application Insights**. 

   > [!TIP]
   > Si todavía no ve el tráfico nuevo, compruebe el **Intervalo de tiempo** y haga clic en **Actualizar**.

   ![Captura de pantalla de la ventana de información general](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Seleccione Live Stream

   ![Captura de pantalla de Live Metrics Stream](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Si el script de PowerShell todavía está en ejecución, debería ver Live Metrics. Si se detuvo, vuelva a ejecutar el script con Live Stream abierto).

## <a name="app-insights-sdk-comparison"></a>Comparación de los SDK de App Insights

El grupo de productos de Application Insights se ha esforzado para lograr una paridad de características entre el [SDK de .NET Framework completo](https://github.com/Microsoft/ApplicationInsights-dotnet) y el SDK de .NET Core. La versión 2.2.0 del [SDK de ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore) para Application Insights ha permitido estrechar considerablemente las distancias entre las características.

Para conocer mejor las diferencias y similitudes entre [.NET y .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server).

   | Comparación de los SDK | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Live Metrics**      | **+** |**-** | **+** |
   | **Canal de telemetría del servidor** | **+** |**-** | **+**|
   |**Muestreo adaptable**| **+** | **-** | **+**|
   | **Llamadas de dependencia de SQL**     | **+** |**-** | **+**|
   | **Contadores de rendimiento*** | **+** | **-**| **-**|

En este contexto, _Contadores de rendimiento_ hace referencia a los [contenedores de rendimiento del lado servidor](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters), como el uso de disco, la memoria y el procesador.

## <a name="open-source-sdk"></a>SDK de código abierto
[Lectura y contribución al código](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates)

## <a name="application-insights-search-continued"></a>Búsqueda de Application Insights (continuación)

Para entender mejor cómo funciona la búsqueda de Application Insights en Visual Studio para un proyecto de ASP.NET Core 2, aunque aún no se haya realizado una instalación explícita de los paquetes de NuGet. Puede resultar útil examinar el resultado de depuración.

Si busca la salida de la palabra _insight_, se resaltarán resultados similares al siguiente:

```DebugOuput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

CoreCLR carga dos ensamblados: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

Y la palabra _unconfigured_ de cada instancia de la telemetría de Application Insights indica que esta aplicación no está asociada a una ikey, por lo que los datos que se generan mientras se está ejecutando la aplicación no se envían a Azure, y solo es están disponibles para propósitos de análisis y búsqueda local.

Esto es posible, en parte, porque el paquete de NuGet _Microsoft.AspNetCore.All_ toma como una dependencia [_Microsoft.ASPNetCoreApplicationInsights.HostingStartup_](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1).

![Captura de pantalla de gráfico de dependencias de NuGet para Microsoft.AspNETCore.all](./media/app-insights-asp-net-core/013-dependency.png)

Fuera de Visual Studio, si estaba modificando un proyecto de ASP.NET Core en VSCode o en algún otro editor de estos ensamblados, no se cargarían automáticamente durante la depuración si aún no ha agregado explícitamente Application Insights al proyecto.

Sin embargo, en Visual Studio, esta activación local de las características de Application Insights desde ensamblados externos se logra usando la [interfaz de IHostingStartup](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) que agrega Application Insights dinámicamente durante la depuración.

Puede obtener más información sobre cómo mejorar una aplicación desde un [ensamblado externo en ASP.NET Core con IHostingStartup](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="how-to-disable-application-insights-in-visual-studio-net-core-projects"></a>Cómo deshabilitar Application Insights en proyectos de Visual Studio .NET Core

Aunque la activación automática de la funcionalidad de búsqueda de Application Insights puede ser útil para algunos, puede ser confuso ver que la telemetría de depuración se genera en lugares inesperados.

Si le basta con deshabilitar la generación de telemetría, puede agregar este bloque de código al método de configuración de su archivo _Startup.cs_:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

CoreCLR seguirá cargando _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ y _Microsoft.ApplicationInsights.AspNetCore.dll_, pero no funcionarán.

Si desea deshabilitar por completo Application Insights en el proyecto de Visual Studio .NET Core, el método preferido consiste en seleccionar **Herramientas** > **Opciones**  >   **Proyectos y soluciones** > **Proyectos web** y active la casilla para deshabilitar Application Insights local en proyectos web de ASP.NET Core. Esta funcionalidad se agregó en Visual Studio 15.6.

![Captura de pantalla de Proyectos web en la ventana de opciones de Visual Studio](./media/app-insights-asp-net-core/014-disable.png)

Si está ejecutando una versión anterior de Visual Studio y desea quitar completamente todos los ensamblados cargados a través de IHostingStartup, puede agregar

`.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")`

a _Program.cs_:

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;

namespace DotNetCore
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
                .UseStartup<Startup>()
                .Build();
    }
}
```

También puede agregar ``"ASPNETCORE_preventHostingStartup": "True"`` a las variables de entorno de _launchSettings.json_.

El problema con el uso de cualquiera de estos métodos es que no deshabilitará Application Insights, sino la funcionalidad de activación IHostingStartup de Visual Studio.

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Pasos siguientes
* [Explore los flujos de usuarios](app-insights-usage-flows.md) para saber cómo los usuarios navegan por la aplicación.
* [Configure la colección de instantáneas](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) para ver el estado del código fuente y las variables en el momento en que se produce una excepción.
* [Use la API](app-insights-api-custom-events-metrics.md) para enviar sus propios eventos y métricas para obtener una vista más detallada del rendimiento y el uso de la aplicación.
* [Las pruebas de disponibilidad](app-insights-monitor-web-app-availability.md) comprueban su aplicación constantemente desde cualquier parte del mundo.
