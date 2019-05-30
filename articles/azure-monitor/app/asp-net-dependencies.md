---
title: Seguimiento de dependencia en Azure Application Insights | Microsoft Docs
description: Supervisar las llamadas de dependencia desde su aplicación de web de Microsoft Azure con Application Insights o en el entorno local.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: d15c4ca8-4c1a-47ab-a03d-c322b4bb2a9e
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: mbullwin
ms.openlocfilehash: 479b810c5a66917bde5754d32991fb489ea26c9b
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299288"
---
# <a name="dependency-tracking-in-azure-application-insights"></a>Seguimiento de dependencia en Azure Application Insights 

Una *dependencia* es un componente externo al que llama la aplicación. Suele ser un servicio al que se llama mediante HTTP, una base de datos o un sistema de archivos. [Application Insights](../../azure-monitor/app/app-insights-overview.md) mide la duración de llamadas de dependencia, si su error o no, junto con información adicional como el nombre de la dependencia y así sucesivamente. Puede investigar las llamadas de dependencia concreta y ponerlos en correlación con las solicitudes y excepciones.

## <a name="automatically-tracked-dependencies"></a>Dependencias automáticamente sometidas a seguimiento

SDK de Application Insights para .NET y .NET Core se distribuye con `DependencyTrackingTelemetryModule` que es un módulo de telemetría que recopila automáticamente las dependencias. Esta colección de dependencias se habilita automáticamente para [ASP.NET](https://docs.microsoft.com/azure/azure-monitor/app/asp-net) y [ASP.NET Core](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) aplicaciones, cuando se configura según la documentación oficial vinculada. `DependencyTrackingTelemetryModule` se distribuye como [esto](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) paquete NuGet y se pone automáticamente cuando se usa cualquiera de los paquetes de NuGet `Microsoft.ApplicationInsights.Web` o `Microsoft.ApplicationInsights.AspNetCore`.

 `DependencyTrackingTelemetryModule` actualmente realiza un seguimiento de las dependencias siguientes automáticamente:

|Dependencias |Detalles|
|---------------|-------|
|Http/Https | Locales o remotos llamadas http/https |
|Llamadas WCF| Solo realiza un seguimiento automáticamente si se usan enlaces basados en Http.|
|SQL | Las llamadas realizadas con `SqlClient`. Consulte [esto](##advanced-sql-tracking-to-get-full-sql-query) para capturar SQL de consulta.  |
|[Almacenamiento de Azure (Blob, tabla, cola)](https://www.nuget.org/packages/WindowsAzure.Storage/) | Llamadas realizadas con el cliente de Azure Storage. |
|[SDK de cliente de centro de eventos](https://www.nuget.org/packages/Microsoft.Azure.EventHubs) | La versión 1.1.0 y versiones posteriores. |
|[SDK de cliente de Service Bus](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus)| Versión 3.0.0 y versiones posteriores. |
|Azure Cosmos DB | Solo realiza un seguimiento automáticamente si se usa HTTP/HTTPS. Application Insights no capturará el modo TCP. |


## <a name="setup-automatic-dependency-tracking-in-console-apps"></a>Instalación automática seguimiento de dependencia en las aplicaciones de consola

Para seguir automáticamente las dependencias de aplicaciones de consola.NET/.NET Core, instale el paquete Nuget `Microsoft.ApplicationInsights.DependencyCollector`e inicializar `DependencyTrackingTelemetryModule` como sigue:

```csharp
    DependencyTrackingTelemetryModule depModule = new DependencyTrackingTelemetryModule();
    depModule.Initialize(TelemetryConfiguration.Active);
```

### <a name="how-automatic-dependency-monitoring-works"></a>¿Dependencia automática supervisión funciona?

Las dependencias se recopilan automáticamente mediante una de las técnicas siguientes:

* Mediante la instrumentación del código de bytes en torno a determinados métodos. (InstrumentationEngine desde StatusMonitor o extensión de la aplicación Web de Azure)
* Devoluciones de llamada de EventSource
* Devoluciones de llamada de DiagnosticSource (en los últimos SDK de.NET/.NET Core)

## <a name="manually-tracking-dependencies"></a>Dependencias de seguimiento manualmente

Los siguientes son algunos ejemplos de las dependencias, que no se recopilan automáticamente y, por lo tanto, requieren seguimiento manual.

* Azure Cosmos DB se rastrea automáticamente solo si se usa [HTTP/HTTPS](../../cosmos-db/performance-tips.md#networking). Application Insights no capturará el modo TCP.
* Redis

Para esas dependencias recopiladas automáticamente por SDK, puede realizar un seguimiento manualmente mediante el [API de TrackDependency](api-custom-events-metrics.md#trackdependency) utilizado por los módulos de recopilación automática estándar.

Por ejemplo, si compila el código con un ensamblado que no escribió usted mismo, podría cronometrar todas las llamadas al ensamblado para averiguar cómo contribuye a los tiempos de respuesta. Para que estos datos se muestren en los gráficos de dependencia en Application Insights, envíelos mediante `TrackDependency`.

```csharp

    var startTime = DateTime.UtcNow;
    var timer = System.Diagnostics.Stopwatch.StartNew();
    try
    {
        // making dependency call
        success = dependency.Call();
    }
    finally
    {
        timer.Stop();
        telemetryClient.TrackDependency("myDependencyType", "myDependencyCall", "myDependencyData",  startTime, timer.Elapsed, success);
    }
```

Como alternativa, `TelemetryClient` proporciona métodos de extensión `StartOperation` y `StopOperation` que puede usarse para realizar un seguimiento manual de dependencias, como se muestra [aquí](custom-operations-tracking.md#outgoing-dependencies-tracking)

Si desea desactivar el módulo de seguimiento de dependencias estándar, quite la referencia a DependencyTrackingTelemetryModule en [ApplicationInsights.config](../../azure-monitor/app/configuration-with-applicationinsights-config.md) para las aplicaciones ASP.NET. Para las aplicaciones de ASP.NET Core, siga las instrucciones [aquí](asp-net-core.md#configuring-or-removing-default-telemetrymodules).

## <a name="tracking-ajax-calls-from-web-pages"></a>Seguimiento de llamadas AJAX desde páginas Web

Para las páginas web, el SDK de JavaScript de Application Insights recopila automáticamente las llamadas de AJAX como dependencias tal como se describe [aquí](javascript.md#ajax-performance). Este documento se centra en las dependencias de los componentes de servidor.

## <a name="advanced-sql-tracking-to-get-full-sql-query"></a>Avanzada de seguimiento para obtener la consulta SQL completa de SQL

Para las llamadas SQL, el nombre del servidor y la base de datos siempre se recopilan y almacenan como nombre de la información recopilada `DependencyTelemetry`. Hay un campo adicional denominado 'data', que puede contener el texto de consulta SQL completo.

Para las aplicaciones de ASP.NET Core, no hay ningún paso adicional necesario para obtener la consulta SQL completa.

Para aplicaciones ASP.NET, la consulta SQL completa se recopila con la Ayuda de instrumentación del código de bytes, que requiere que el motor de instrumentación. Específico de la plataforma, como se describe a continuación, se requieren pasos adicionales.

| Plataforma | Pasos necesarios para obtener la consulta SQL completa |
| --- | --- |
| Aplicación web de Azure |En el panel de control de la aplicación web [abrir la hoja de Application Insights](../../azure-monitor/app/azure-web-apps.md) y habilitar los comandos SQL en .NET |
| Servidor IIS (máquina virtual de Azure, en el entorno local y así sucesivamente). | [Instale el Monitor de estado en el servidor donde se ejecuta la aplicación](../../azure-monitor/app/monitor-performance-live-website-now.md) y reinicie IIS.
| Servicio en la nube de Azure |[Tarea de inicio de uso](../../azure-monitor/app/cloudservices.md) a [instale el Monitor de estado](monitor-performance-live-website-now.md#download) |
| IIS Express | No compatible

En los casos anteriores, ha instalado correctamente es la manera correcta de validar que el motor de instrumentación es validando que la versión del SDK de recopila `DependencyTelemetry` es 'rddp'. 'rdddsd' o 'rddf' indica las dependencias se recopilan a través de las devoluciones de llamada de DiagnosticSource o EventSource y, por lo tanto, no se puede capturar consulta SQL completa.

## <a name="where-to-find-dependency-data"></a>Dónde encontrar los datos de dependencia

* [Asignación de aplicación](app-map.md) visualiza las dependencias entre la aplicación y los componentes colindantes.
* [Diagnósticos de transacción](transaction-diagnostics.md) muestra unificada, correlacionar datos del servidor.
* La hoja [Exploradores](javascript.md#ajax-performance) muestra las llamadas AJAX de los exploradores de los usuarios.
* Haga clic en las solicitudes lentas o con errores para comprobar sus llamadas de dependencia.
* [Análisis](#analytics) puede utilizarse para consultar los datos de dependencia.

## <a name="diagnosis"></a> Diagnóstico de solicitudes lentas

Cada evento de solicitud está asociado a las llamadas de dependencia, las excepciones y otros eventos de los que se realizan un seguimiento mientras la aplicación está procesando la solicitud. Por lo que si algunas solicitudes hacen mal, puede averiguar si es debido a una dependencia responda con lentitud.

Veamos un ejemplo.

### <a name="tracing-from-requests-to-dependencies"></a>Seguimiento de solicitudes en dependencias

Abra la hoja Rendimiento y examine la cuadrícula de solicitudes:

![Lista de solicitudes con promedios y recuentos](./media/asp-net-dependencies/02-reqs.png)

La primera tarda mucho. Veamos si podemos averiguar dónde se está invirtiendo el tiempo.

Haga clic en esa fila para ver los eventos de solicitud individuales:

![Lista de casos de solicitudes](./media/asp-net-dependencies/03-instances.png)

Haga clic en cualquier instancia que tarda en ejecutarse para realizar una inspección más profunda y desplácese hacia abajo hasta las llamadas de dependencia remotas relacionadas con esta solicitud:

![Búsqueda de llamadas a dependencias remotas, identificación de duración inusual.](./media/asp-net-dependencies/04-dependencies.png)

Parece que la mayoría del tiempo que se ha invertido en atender a esta solicitud se ha empleado en una llamada a un servicio local.

Seleccione esa fila para obtener más información:

![Haga clic en esa dependencia remota para identificar la causa.](./media/asp-net-dependencies/05-detail.png)

Parece que esta dependencia es dónde está el problema. Hemos detectado el problema, así que ahora necesitamos averiguar por qué esa llamada tarda tanto en realizarse.

### <a name="request-timeline"></a>Escala de tiempo de solicitudes

En otro caso, no hay ninguna llamada de dependencia especialmente larga. Sin embargo, al cambiar a la vista Escala de tiempo, podemos ver dónde se produce el retraso en nuestro procesamiento interno:

![Búsqueda de llamadas a dependencias remotas, identificación de duración inusual.](./media/asp-net-dependencies/04-1.png)

Parece haber un intervalo de gran duración después de la primera llamada de dependencia, por lo que debemos examinar el código para ver el motivo.

### <a name="profile-your-live-site"></a>Generación de un perfil del sitio activo

¿Quiere saber en qué se invierte el tiempo? El [Application Insights profiler](../../azure-monitor/app/profiler.md) seguimientos HTTP llama a los sitios activos y muestra las funciones en el código que tardan más tiempo.

## <a name="failed-requests"></a>Error en las solicitudes

Las solicitudes con error también podrían estar asociadas a llamadas a dependencias con errores. Una vez más, podemos clic para realizar un seguimiento del problema.

![Haga clic en el gráfico de las solicitudes con error.](./media/asp-net-dependencies/06-fail.png)

Desplazarse hasta una repetición de una solicitud con error y examine los eventos asociados.

![Haga clic en un tipo de solicitud, haga clic en la instancia para obtener acceso a una vista diferente de la misma instancia, haga clic en ella para obtener detalles de la excepción.](./media/asp-net-dependencies/07-faildetail.png)

## <a name="analytics"></a>Análisis

Puede realizar un seguimiento de las dependencias en el [lenguaje de consulta de Kusto](/azure/kusto/query/). Estos son algunos ejemplos.

* Búsqueda de llamadas de dependencia con errores:

```

    dependencies | where success != "True" | take 10
```

* Búsqueda de llamadas AJAX:

```

    dependencies | where client_Type == "Browser" | take 10
```

* Búsqueda de llamadas de dependencia asociadas a solicitudes:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type != "Browser"
    | join (requests | where timestamp > ago(1d))
      on operation_Id  
```


* Búsqueda de llamadas AJAX asociadas a vistas de página:

```

    dependencies
    | where timestamp > ago(1d) and  client_Type == "Browser"
    | join (browserTimings | where timestamp > ago(1d))
      on operation_Id
```

## <a name="video"></a>Vídeo

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

### <a name="how-does-automatic-dependency-collector-report-failed-calls-to-dependencies"></a>*¿Cómo hace de dependencia automático recopilador no pudo informe llamadas a dependencias?*

* Las llamadas de dependencia con error tendrán el campo 'success' establecido en False. `DependencyTrackingTelemetryModule` no notifica `ExceptionTelemetry`. Se describe el modelo de datos completa para la dependencia [aquí](data-model-dependency-telemetry.md).

## <a name="open-source-sdk"></a>SDK de código abierto
Como cada SDK de Application Insights, módulo de la colección de dependencias también es código abierto. Leer y contribuir al código o notificar problemas a [el repositorio GitHub oficial](https://github.com/Microsoft/ApplicationInsights-dotnet-server).


## <a name="next-steps"></a>Pasos siguientes

* [Excepciones](../../azure-monitor/app/asp-net-exceptions.md)
* [Datos de página y usuario](../../azure-monitor/app/javascript.md)
* [Disponibilidad](../../azure-monitor/app/monitor-web-app-availability.md)
