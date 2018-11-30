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
ms.openlocfilehash: 3e838cd45eefa5b5b644992bb8eae36abf3ec33a
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "52276353"
---
# <a name="application-insights-for-aspnet-core"></a>Application Insights para ASP.NET Core

Azure Application Insights proporciona una supervisión detallada de la aplicación web hasta el nivel de código. Puede supervisar fácilmente la disponibilidad, el rendimiento y el uso de su aplicación web. También puede identificar y diagnosticar errores en la aplicación rápidamente sin tener que esperar a que un usuario informe de ellos.

En este artículo se explican los pasos para crear una aplicación de [Razor Pages](https://docs.microsoft.com/aspnet/core/mvc/razor-pages/?tabs=visual-studio) de ASP.NET Core de ejemplo en Visual Studio. También se muestra cómo iniciar la supervisión mediante Application Insights.

## <a name="prerequisites"></a>Requisitos previos

- SDK de .NET Core 2.0.0 o posterior
- Versión 15.7.3 o posterior de [Visual Studio 2017](https://www.visualstudio.com/downloads/) con la carga de trabajo de desarrollo web y ASP.NET

## <a name="create-an-aspnet-core-project-in-visual-studio"></a>Creación de un proyecto de ASP.NET Core en Visual Studio

1. Haga clic con el botón derecho en **Visual Studio 2017** y, a continuación, seleccione **Ejecutar como administrador**.
2. Seleccione **Archivo** > **Nuevo** > **Proyecto** (Ctrl+Mayús+N).

   ![Captura de pantalla del menú Nuevo proyecto de Visual Studio](./media/app-insights-asp-net-core/001-new-project.png)

3. Expanda **Visual C#**. Seleccione **.Net Core** > **ASP.NET Core Web Application**. Escriba un nombre de proyecto y un nombre de solución, y, a continuación, seleccione **Crear nuevo repositorio Git**.

   ![Captura de pantalla del Asistente para nuevo proyecto de Visual Studio](./media/app-insights-asp-net-core/002-asp-net-core-web-application.png)

4. Seleccione **.NET Core** > **ASP.NET Core 2.0** **Web Application** > **Aceptar**.

    ![Captura de pantalla de la selección de plantilla del nuevo proyecto de Visual Studio](./media/app-insights-asp-net-core/003-web-application.png)

## <a name="application-insights-search"></a>Búsqueda de Application Insights

En Visual Studio 2015 Update 2 o posterior con un proyecto basado en ASP.NET Core 2+, puede utilizar la [búsqueda de Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-visual-studio), incluso antes de agregar explícitamente Application Insights al proyecto.

Para probar esta funcionalidad:

1. Ejecute la aplicación. Para ejecutar la aplicación, seleccione el icono de **IIS Express** (![Captura de pantalla del icono de Visual Studio de IIS Express](./media/app-insights-asp-net-core/004-iis-express.png)).

2. Seleccione **Ver** > **Otras ventanas** > **Búsqueda de Application Insights**.

   ![Captura de pantalla de la selección de herramientas de diagnóstico de Visual Studio](./media/app-insights-asp-net-core/005-view-other-windows-search.png)

3. Actualmente, la telemetría de la sesión de depuración está disponible solo para análisis locales. Para habilitar completamente Application Insights, seleccione **Disponibilidad de telemetría** en la esquina superior derecha y siga los pasos enumerados en la sección de a continuación.

   ![Captura de pantalla de Búsqueda de Application Insights en Visual Studio](./media/app-insights-asp-net-core/006-search.png)

> [!NOTE]
> Para más información sobre cómo Visual Studio activa características como [Búsqueda de Application Insights](app-insights-visual-studio.md) y [CodeLens](app-insights-visual-studio-codelens.md) localmente antes de agregar Application Insights al proyecto de ASP.NET Core, consulte [Búsqueda de Application Insights (continuación)](#application-insights-search-continued).

## <a name="add-application-insights-telemetry"></a>Incorporación de los datos de telemetría de Application Insights

1. Seleccione **Proyecto** > **Agregar Telemetría de Application Insights**. También puede hacer clic con el botón derecho en **Servicios conectados** y seleccionar **Agregar servicio conectado**.

    ![Captura de pantalla del menú de selección del nuevo proyecto de Visual Studio](./media/app-insights-asp-net-core/007-project-add-telemetry.png)

2. Seleccione **Comenzar**. En función de la versión de Visual Studio, el texto puede variar ligeramente. Algunas versiones anteriores tienen un botón **Comenzar gratis** en cambio.

    ![Captura de pantalla del botón de información general de Application Insights](./media/app-insights-asp-net-core/008-get-started.png)

3. Seleccione la suscripción y luego seleccione **Recurso** > **Registrar**.

## <a name="changes-made-to-your-project"></a>Cambios en el proyecto

Application Insights tiene poca sobrecarga. Para revisar las modificaciones que se realizaron en el proyecto al agregar la telemetría de Application Insights:

Seleccione **Ver** > **Team Explorer** (Ctrl+\, Ctrl+M) > **Proyecto** > **Cambios**

- Aparecen cuatro cambios en total:

  ![Captura de pantalla de los archivos que se modificaron al agregar Application Insights](./media/app-insights-asp-net-core/009-changes.png)

- Se crea un archivo nuevo:

  -  _ConnectedService.json_

    ```json
    {
      "ProviderId": "Microsoft.ApplicationInsights.ConnectedService.ConnectedServiceProvider",
      "Version": "8.12.10405.1",
      "GettingStartedDocument": {
        "Uri": "https://go.microsoft.com/fwlink/?LinkID=798432"
      }
    }
    ```

- Se modifican tres archivos (se agregan comentarios adicionales para resaltar los cambios):

  - _appsettings.json_:

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

  - _ContosoDotNetCore.csproj_:

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

  -  _Program.cs_:

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

Para automatizar las solicitudes a la aplicación con las transacciones sintéticas:

1. Para ejecutar la aplicación, seleccione el icono ![Captura de pantalla del icono de IIS Express de Visual Studio](./media/app-insights-asp-net-core/004-iis-express.png) .

2. Copie la dirección URL de la barra de direcciones del explorador. La dirección URL está en el formato `http://localhost:<port number>`.

   ![Captura de pantalla de la barra de direcciones URL del explorador](./media/app-insights-asp-net-core/0013-copy-url.png)

3. Ejecute el bucle de PowerShell siguiente para crear 100 transacciones sintéticas en la aplicación de prueba. Modifique el número de puerto que aparece después de `localhost:` para que coincida con la URI que copió en el paso anterior. Por ejemplo: 

   ```PowerShell
   for ($i = 0 ; $i -lt 100; $i++)
   {
    Invoke-WebRequest -uri http://localhost:50984/
   }
   ```

## <a name="open-the-application-insights-portal"></a>Apertura del portal de Application Insights

Después de ejecutar los comandos de PowerShell de la sección anterior, inicie Application Insights para ver las transacciones y confirmar que se recopilan los datos. 

En el menú de Visual Studio, seleccione **Proyecto** > **Application Insights** > **Abrir portal de Application Insights**.

   ![Captura de pantalla de la información general de Application Insights](./media/app-insights-asp-net-core/010-portal.png)

> [!NOTE]
> En la captura de pantalla de ejemplo anterior, **Live Stream**, **Tiempo de carga de la vista de página** y **Solicitudes con error** no se recopilan. La siguiente sección le guiará por los pasos para agregar cada uno de ellos. Si ya recopila **Live Stream** y **Tiempo de carga de la vista de página**, complete los pasos solo para **Solicitudes con error**.

## <a name="collect-failed-requests-live-stream-and-page-view-load-time"></a>Recopilación de Solicitudes con error, Live Stream y Tiempo de carga de la vista de página

### <a name="failed-requests"></a>Solicitudes con error

Técnicamente, las solicitudes con error se están recopilando, pero todavía no se ha producido ninguna. Para acelerar el proceso, puede agregar una excepción personalizada al proyecto existente para simular una excepción real. Si la aplicación sigue ejecutándose en Visual Studio, antes de continuar, seleccione **Detener la depuración** (Mayús+F5).

1. En el **Explorador de soluciones**, expanda **Páginas** > **About.cshtml** y después abra *About.cshtml.cs*.

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

Para acceder a la funcionalidad Live Stream de Application Insights con la actualización de ASP.NET Core, actualice los paquetes NuGet de Microsoft.ApplicationInsights.AspNetCore 2.2.0.

En Visual Studio, seleccione **Proyecto** > **Administrar paquetes NuGet** > **Microsoft.ApplicationInsights.AspNetCore** > Versión **2.2.0** > **Actualizar**.

  ![Captura de pantalla del Administrador de paquetes NuGet](./media/app-insights-asp-net-core/012-nuget-update.png)

Aparecen varios mensajes de confirmación. Lea y acepte si está de acuerdo con los cambios.

### <a name="page-view-load-time"></a>Tiempo de carga de la vista de página

1. En Visual Studio, vaya a la página **Explorador de soluciones** > **Páginas**. Debe modificar dos archivos: *Layout.cshtml* y *ViewImports.cshtml*.

2. En *ViewImports.cshtml*, agregue este código:

   ```csharp
   @using Microsoft.ApplicationInsights.AspNetCore
   @inject JavaScriptSnippet snippet
   ```

3. En *Layout.cshtml*, agregue el código siguiente antes de la etiqueta ``</head>`` y antes de cualquier otro script:

    ```csharp
    @Html.Raw(snippet.FullScript)
    ```

### <a name="test-failed-requests-page-view-load-time-and-live-stream"></a>Prueba de Solicitudes con error, Tiempo de carga de la vista de página y Live Stream

Para probar y confirmar que todo funciona:

1. Ejecute la aplicación. Para ejecutar la aplicación, seleccione el icono ![Captura de pantalla del icono de IIS Express de Visual Studio](./media/app-insights-asp-net-core/004-iis-express.png) .

2. Vaya a la página **About** (Acerca de) para desencadenar la excepción de prueba. (Si está en el modo de depuración, en Visual Studio, seleccione **Continuar** para que Application Insights muestre la excepción).

3. Vuelva a ejecutar el script de transacción de PowerShell simulado que usó anteriormente. Es posible que deba ajustar el número de puerto en el script.

4. Si todavía no está abierta la página **Información general** en Applications Insights, en el menú de Visual Studio, seleccione **Proyecto** > **Application Insights** > **Abrir portal de Application Insights**. 

   > [!TIP]
   > Si no ve el tráfico nuevo, compruebe el valor de **Intervalo de tiempo** y, a continuación, seleccione **Actualizar**.

   ![Captura de pantalla de la ventana de información general](./media/app-insights-asp-net-core/0019-overview-updated.png)

5. Seleccione **Live Stream**.

   ![Captura de pantalla de Live Metrics Stream](./media/app-insights-asp-net-core/0020-live-metrics-stream.png)

   (Si todavía se está ejecutando el script de PowerShell, debería ver las métricas en directo. Si el script de PowerShell ha dejado de ejecutarse, vuelva a ejecutar el script con Live Stream abierto).

## <a name="application-insights-sdk-comparison"></a>Comparación de los SDK de Application Insights

El grupo de productos de Application Insights se ha esforzado para lograr una paridad de características entre el [SDK de .NET Framework completo](https://github.com/Microsoft/ApplicationInsights-dotnet) y el SDK de .NET Core. La versión 2.2.0 del [SDK de ASP.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore) para Application Insights ha permitido estrechar considerablemente las distancias entre las características.

En la tabla siguiente se describen varias de las diferencias y ventajas e inconvenientes de [.NET y .NET Core](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server):

   | Comparación de los SDK | ASP.NET        | ASP.NET Core 2.1.0    | ASP.NET Core 2.2.0 |
  |:-- | :-------------: |:------------------------:|:----------------------:|
   | **Live metrics**      | **+** |**-** | **+** |
   | **Canal de telemetría del servidor** | **+** |**-** | **+**|
   |**Muestreo adaptable**| **+** | **-** | **+**|
   | **Llamadas de dependencia de SQL**     | **+** |**-** | **+**|
   | **Contadores de rendimiento*** | **+** | **-**| **-**|

En este contexto, los contadores de rendimiento hacen referencia a los [contenedores de rendimiento del lado servidor](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters), como el uso de disco, la memoria y el procesador.

## <a name="open-source-sdk"></a>SDK de código abierto
[Lectura y contribución al código](https://github.com/Microsoft/ApplicationInsights-aspnetcore#recent-updates).

## <a name="application-insights-search-continued"></a>Búsqueda de Application Insights (continuación)

Esta sección puede ayudarle a comprender mejor cómo funciona la búsqueda de Application Insights en Visual Studio para un proyecto de ASP.NET Core 2. Funciona así, incluso cuando no ha instalado de forma explícita los paquetes NuGet de Application Insights. También puede resultar útil examinar el resultado de la depuración.

Si busca la salida para la palabra _insight_, se resaltarán resultados similares al siguiente:

```DebugOutput
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.aspnetcore.applicationinsights.hostingstartup\2.0.3\lib\netcoreapp2.0\Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll'.
'dotnet.exe' (CoreCLR: clrhost): Loaded 'C:\Program Files\dotnet\store\x64\netcoreapp2.0\microsoft.applicationinsights.aspnetcore\2.1.1\lib\netstandard1.6\Microsoft.ApplicationInsights.AspNetCore.dll'.

Application Insights Telemetry (unconfigured): {"name":"Microsoft.ApplicationInsights.Dev.Message","time":"2018-06-03T17:32:38.2796801Z","tags":{"ai.location.ip":"127.0.0.1","ai.operation.name":"DEBUG /","ai.internal.sdkVersion":"aspnet5c:2.1.1","ai.application.ver":"1.0.0.0","ai.cloud.roleInstance":"CONTOSO-SERVER","ai.operation.id":"de85878e-4618b05bad11b5a6","ai.internal.nodeName":"CONTOSO-SERVER","ai.operation.parentId":"|de85878e-4618b05bad11b5a6."},"data":{"baseType":"MessageData","baseData":{"ver":2,"message":"Request starting HTTP/1.1 DEBUG http://localhost:53022/  0","severityLevel":"Information","properties":{"AspNetCoreEnvironment":"Development","Protocol":"HTTP/1.1","CategoryName":"Microsoft.AspNetCore.Hosting.Internal.WebHost","Host":"localhost:53022","Path":"/","Scheme":"http","ContentLength":"0","DeveloperMode":"true","Method":"DEBUG"}}}}
```

En la salida, CoreCLR carga dos ensamblados: 

- _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_
- _Microsoft.ApplicationInsights.AspNetCore.dll_.

La referencia _no configurada_ en cada instancia de la telemetría de Application Insights indica que esta aplicación no está asociada con un valor de ikey. Los datos que se generan mientras se está ejecutando la aplicación no se envían a Azure. Los datos están disponibles solo para el análisis y la búsqueda locales.

La funcionalidad es posible, en parte, porque el paquete NuGet _Microsoft.AspNetCore.All_ toma [_Microsoft.ASPNetCoreApplicationInsights.HostingStartup_](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.applicationinsights.hostingstartup.applicationinsightshostingstartup?view=aspnetcore-2.1) como una dependencia.

![Captura de pantalla del grafo de dependencias de NuGet para Microsoft.AspNETCore.all](./media/app-insights-asp-net-core/013-dependency.png)

Fuera de Visual Studio, si estaba modificando un proyecto de ASP.NET Core en VSCode o en algún otro editor, estos ensamblados no se cargarían automáticamente durante la depuración si aún no ha agregado de modo explícito Application Insights al proyecto.

Sin embargo, en Visual Studio, esta activación local de las características de Application Insights desde ensamblados externos se logra con la [interfaz IHostingStartup](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1). La interfaz agrega Application Insights durante la depuración de forma dinámica.

Más información sobre cómo mejorar una aplicación desde un [ensamblado externo en ASP.NET Core con IHostingStartup](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/platform-specific-configuration?view=aspnetcore-2.1). 

### <a name="disable-application-insights-in-visual-studio-net-core-projects"></a>Deshabilitar Application Insights en proyectos de Visual Studio .NET Core

Aunque la activación automática de la funcionalidad de búsqueda de Application Insights puede ser útil, ver que la telemetría de depuración se genera en lugares inesperados puede resultar confuso.

Si le basta con deshabilitar la generación de telemetría, puede agregar este bloque de código al método **configure** del archivo _Startup.cs_:

```csharp
  var configuration = app.ApplicationServices.GetService<Microsoft.ApplicationInsights.Extensibility.TelemetryConfiguration>();
            configuration.DisableTelemetry = true;
            if (env.IsDevelopment())
```

CoreCLR seguirá cargando _Microsoft.AspNetCore.ApplicationInsights.HostingStartup.dll_ y _Microsoft.ApplicationInsights.AspNetCore.dll_, pero los archivos no hacen nada.

Si desea deshabilitar por completo Application Insights en el proyecto de Visual Studio .NET Core, el método preferido consiste en seleccionar **Herramientas** > **Opciones**  >   **Proyectos y soluciones** > **Proyectos web**. Active la casilla **Deshabilitar la instancia local de Application Insights para proyectos web de ASP.NET Core**. Esta funcionalidad se agregó en Visual Studio 15.6.

![Captura de pantalla de Proyectos web en la ventana de opciones de Visual Studio](./media/app-insights-asp-net-core/014-disable.png)

Si está ejecutando una versión anterior de Visual Studio y desea quitar completamente todos los ensamblados cargados mediante *IHostingStartup*, tiene dos opciones:

* Agregar `.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")` a _Program.cs_:

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

* Agregar ``"ASPNETCORE_preventHostingStartup": "True"`` a las variables de entorno de _launchSettings.json_.

El problema de usar alguno de estos métodos es que no deshabilitan solo Application Insights. También deshabilitan todo lo que, en Visual Studio, estuviera utilizando la funcionalidad de *IHostingStartup*.

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player] 

## <a name="next-steps"></a>Pasos siguientes
* [Explore los flujos de usuarios](app-insights-usage-flows.md) para saber cómo navegan por la aplicación.
* [Configure la colección de instantáneas](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger#configure-snapshot-collection-for-aspnet-core-20-applications) para ver el estado del código fuente y las variables en el momento en que se produzca una excepción.
* [Use la API](app-insights-api-custom-events-metrics.md) para enviar sus propios eventos y métricas para obtener una vista más detallada del rendimiento y el uso de la aplicación.
* [Las pruebas de disponibilidad](app-insights-monitor-web-app-availability.md) comprueban la aplicación constantemente desde cualquier parte del mundo.
