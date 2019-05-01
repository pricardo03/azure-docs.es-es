---
title: Habilitar el depurador de instantáneas para aplicaciones .NET en Azure Service Fabric, servicio en la nube y máquinas virtuales | Microsoft Docs
description: Habilitar el depurador de instantáneas para aplicaciones .NET en Azure Service Fabric, servicio en la nube y máquinas virtuales
services: application-insights
documentationcenter: ''
author: brahmnes
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.reviewer: mbullwin
ms.date: 03/07/2019
ms.author: brahmnes
ms.openlocfilehash: ac937ddb1bcaed6813a0de4d631f820eff01e26f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60783507"
---
# <a name="enable-snapshot-debugger-for-net-apps-in-azure-service-fabric-cloud-service-and-virtual-machines"></a>Habilitar el depurador de instantáneas para aplicaciones .NET en Azure Service Fabric, servicio en la nube y máquinas virtuales

Si su ASP.NET o ASP.NET core se ejecuta la aplicación en Azure App Service, también se pueden usar las instrucciones siguientes. A menos que la aplicación requiera una configuración personalizada del depurador de instantáneas, se recomienda encarecidamente a [habilitar depurador de instantáneas a través de la página del portal de Application Insights](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json). Si la aplicación se ejecuta en Azure Service Fabric, servicio en la nube, máquinas virtuales o máquinas locales, se deben usar las instrucciones siguientes. 
    
## <a name="configure-snapshot-collection-for-aspnet-applications"></a>Configurar la recopilación de instantáneas para aplicaciones ASP.NET

1. [Habilite Application Insights en su aplicación web](../../azure-monitor/app/asp-net.md), si aún no lo ha hecho.

2. Incluya el paquete NuGet [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) en la aplicación.

3. Si es necesario, personalizar la configuración de Snapshot Debugger agregada a [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md). La configuración predeterminada del depurador de instantáneas está principalmente vacía y todas las configuraciones son opcionales. Este es un ejemplo que muestra una configuración equivalente a la configuración predeterminada:

    ```xml
    <TelemetryProcessors>
        <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
        <!-- The default is true, but you can disable Snapshot Debugging by setting it to false -->
        <IsEnabled>true</IsEnabled>
        <!-- Snapshot Debugging is usually disabled in developer mode, but you can enable it by setting this to true. -->
        <!-- DeveloperMode is a property on the active TelemetryChannel. -->
        <IsEnabledInDeveloperMode>false</IsEnabledInDeveloperMode>
        <!-- How many times we need to see an exception before we ask for snapshots. -->
        <ThresholdForSnapshotting>1</ThresholdForSnapshotting>
        <!-- The maximum number of examples we create for a single problem. -->
        <MaximumSnapshotsRequired>3</MaximumSnapshotsRequired>
        <!-- The maximum number of problems that we can be tracking at any time. -->
        <MaximumCollectionPlanSize>50</MaximumCollectionPlanSize>
        <!-- How often we reconnect to the stamp. The default value is 15 minutes.-->
        <ReconnectInterval>00:15:00</ReconnectInterval>
        <!-- How often to reset problem counters. -->
        <ProblemCounterResetInterval>1.00:00:00</ProblemCounterResetInterval>
        <!-- The maximum number of snapshots allowed in ten minutes.The default value is 1. -->
        <SnapshotsPerTenMinutesLimit>3</SnapshotsPerTenMinutesLimit>
        <!-- The maximum number of snapshots allowed per day. -->
        <SnapshotsPerDayLimit>30</SnapshotsPerDayLimit>
        <!-- Whether or not to collect snapshot in low IO priority thread. The default value is true. -->
        <SnapshotInLowPriorityThread>true</SnapshotInLowPriorityThread>
        <!-- Agree to send anonymous data to Microsoft to make this product better. -->
        <ProvideAnonymousTelemetry>true</ProvideAnonymousTelemetry>
        <!-- The limit on the number of failed requests to request snapshots before the telemetry processor is disabled. -->
        <FailedRequestLimit>3</FailedRequestLimit>
        </Add>
    </TelemetryProcessors>
    ```

4. Las instantáneas solo se recopilan en las excepciones de las que se informa a Application Insights. En algunos casos (por ejemplo, en versiones anteriores de la plataforma. NET), es posible que tenga que [configurar la recopilación de excepciones](../../azure-monitor/app/asp-net-exceptions.md#exceptions) para ver las excepciones con las instantáneas en el portal.


## <a name="configure-snapshot-collection-for-aspnet-core-20-applications"></a>Configurar la recopilación de instantáneas para aplicaciones ASP.NET Core 2.0

1. [Habilite Application Insights en su aplicación web ASP.NET Core](../../azure-monitor/app/asp-net-core.md), si aún no lo ha hecho.

    > [!NOTE]
    > Procure que la aplicación haga referencia a la versión 2.1.1 (o una más reciente) del paquete Microsoft.ApplicationInsights.AspNetCore.

2. Incluya el paquete NuGet [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) en la aplicación.

3. Modifique la clase `Startup` de la aplicación para agregar y configurar el procesador de telemetría del recopilador de instantáneas.

    Agregue las siguientes instrucciones using a `Startup.cs`

   ```csharp
   using Microsoft.ApplicationInsights.SnapshotCollector;
   using Microsoft.Extensions.Options;
   using Microsoft.ApplicationInsights.AspNetCore;
   using Microsoft.ApplicationInsights.Extensibility;
   ```

   Agregue la siguiente clase `SnapshotCollectorTelemetryProcessorFactory` a la clase `Startup`.

   ```csharp
   class Startup
   {
       private class SnapshotCollectorTelemetryProcessorFactory : ITelemetryProcessorFactory
       {
           private readonly IServiceProvider _serviceProvider;

           public SnapshotCollectorTelemetryProcessorFactory(IServiceProvider serviceProvider) =>
               _serviceProvider = serviceProvider;

           public ITelemetryProcessor Create(ITelemetryProcessor next)
           {
               var snapshotConfigurationOptions = _serviceProvider.GetService<IOptions<SnapshotCollectorConfiguration>>();
               return new SnapshotCollectorTelemetryProcessor(next, configuration: snapshotConfigurationOptions.Value);
           }
       }
       ...
    ```
    Agregue los servicios `SnapshotCollectorConfiguration` y `SnapshotCollectorTelemetryProcessorFactory` a la canalización de inicio:

    ```csharp
       // This method gets called by the runtime. Use this method to add services to the container.
       public void ConfigureServices(IServiceCollection services)
       {
           // Configure SnapshotCollector from application settings
           services.Configure<SnapshotCollectorConfiguration>(Configuration.GetSection(nameof(SnapshotCollectorConfiguration)));

           // Add SnapshotCollector telemetry processor.
           services.AddSingleton<ITelemetryProcessorFactory>(sp => new SnapshotCollectorTelemetryProcessorFactory(sp));

           // TODO: Add other services your application needs here.
       }
   }
   ```

4. Si es necesario, puede personalizar la configuración del depurador de instantáneas mediante la adición de una sección de SnapshotCollectorConfiguration en appsettings.json. Todas las configuraciones en la configuración del depurador de instantáneas son opcionales. Este es un ejemplo que muestra una configuración equivalente a la configuración predeterminada:

   ```json
   {
     "SnapshotCollectorConfiguration": {
       "IsEnabledInDeveloperMode": false,
       "ThresholdForSnapshotting": 1,
       "MaximumSnapshotsRequired": 3,
       "MaximumCollectionPlanSize": 50,
       "ReconnectInterval": "00:15:00",
       "ProblemCounterResetInterval":"1.00:00:00",
       "SnapshotsPerTenMinutesLimit": 1,
       "SnapshotsPerDayLimit": 30,
       "SnapshotInLowPriorityThread": true,
       "ProvideAnonymousTelemetry": true,
       "FailedRequestLimit": 3
     }
   }
   ```

## <a name="configure-snapshot-collection-for-other-net-applications"></a>Configurar la recopilación de instantáneas para otras aplicaciones .NET

1. Si la aplicación aún no tiene Application Insights, debe empezar por [habilitar Application Insights y establecer la clave de instrumentación](../../azure-monitor/app/windows-desktop.md).

2. Agregue el paquete NuGet [Microsoft.ApplicationInsights.SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) en la aplicación.

3. Las instantáneas solo se recopilan en las excepciones de las que se informa a Application Insights. Es posible que tenga que modificar el código para informar de las excepciones. El código de control de excepciones depende de la estructura de la aplicación, pero a continuación se muestra un ejemplo:
    ```csharp
   TelemetryClient _telemetryClient = new TelemetryClient();

   void ExampleRequest()
   {
        try
        {
            // TODO: Handle the request.
        }
        catch (Exception ex)
        {
            // Report the exception to Application Insights.
            _telemetryClient.TrackException(ex);

            // TODO: Rethrow the exception if desired.
        }
   }
    ```

## <a name="next-steps"></a>Pasos siguientes

- Genere tráfico a la aplicación que puede desencadenar una excepción. A continuación, espere 10 a 15 minutos para que las instantáneas se envían a la instancia de Application Insights.
- Consulte [instantáneas](snapshot-debugger.md?toc=/azure/azure-monitor/toc.json) en Azure portal.
- Para solucionar problemas de Profiler, vea [solución de problemas de Snapshot Debugger](snapshot-debugger-troubleshoot.md?toc=/azure/azure-monitor/toc.json).
