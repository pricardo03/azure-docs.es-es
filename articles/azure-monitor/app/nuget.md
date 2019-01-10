---
title: 'Azure Application Insights: recopilación automática de dependencias | Microsoft Docs'
description: Recopilación y visualización automática de dependencias de Application Insights
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 10/16/2018
ms.author: mbullwin
ms.openlocfilehash: 3ad2f4788a765366066023724772f5432d0d56eb
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54109430"
---
# <a name="application-insights-nuget-packages"></a>Paquetes NuGet de Application Insights

A continuación se encuentra la lista actual de los paquetes NuGet de versión estable de Application Insights.

## <a name="common-packages-for-aspnet"></a>Paquetes comunes para ASP.NET

| Nombre del paquete | Versión estable | DESCRIPCIÓN | Descargar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights | 2.8.0 | Proporciona funcionalidad básica para la transmisión de todos los tipos de datos de telemetría de Application Insights y es un paquete dependiente de todos los demás paquetes de Application Insights | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
|Microsoft.ApplicationInsights.Agent.Intercept | 2.4.0 | Habilita la interceptación de llamadas a método | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent.Intercept/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.8.0 | Recopilador de dependencias de Application Insights para aplicaciones .NET. Es un paquete dependiente para los paquetes específicos de la plataforma de Application Insights y proporciona la recopilación automática de telemetría de dependencias. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.8.0 | El recopilador de contadores de rendimiento de Application Insights permite enviar datos recopilados por los contadores de rendimiento a Application Insights. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.Web | 2.8.0 | Application Insights para aplicaciones web .NET | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.8.0 | El paquete NuGet de Windows Server para Application Insights ofrece una recopilación automática de telemetría de Application Insights para aplicaciones .NET. Este paquete se puede usar como un paquete dependiente para los paquetes específicos de la plataforma de Application Insights o como un paquete independiente para las aplicaciones .NET que no están cubiertas por los paquetes específicos de la plataforma (como en el caso de los roles de trabajo. NET). | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.8.0 | Proporciona un canal de telemetría para el SDK de Windows Server para Application Insights que conservará la telemetría en escenarios sin conexión. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="common-packages-for-aspnet-core"></a>Paquetes comunes para ASP.NET Core

| Nombre del paquete | Versión estable | DESCRIPCIÓN | Descargar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AspNetCore | 2.5.0 | Application Insights para aplicaciones web ASP.NET Core | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) |
| Microsoft.ApplicationInsights | 2.8.0 | Este paquete proporciona funcionalidad básica para la transmisión de todos los tipos de datos de telemetría de Application Insights y es un paquete dependiente de todos los demás paquetes de Application Insights | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.8.0 | Recopilador de dependencias de Application Insights para aplicaciones .NET. Es un paquete dependiente para los paquetes específicos de la plataforma de Application Insights y proporciona la recopilación automática de telemetría de dependencias. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.8.0 | El recopilador de contadores de rendimiento de Application Insights permite enviar datos recopilados por los contadores de rendimiento a Application Insights. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.8.0 | El paquete NuGet de Windows Server para Application Insights ofrece una recopilación automática de telemetría de Application Insights para aplicaciones .NET. Este paquete se puede usar como un paquete dependiente para los paquetes específicos de la plataforma de Application Insights o como un paquete independiente para las aplicaciones .NET que no están cubiertas por los paquetes específicos de la plataforma (como en el caso de los roles de trabajo. NET). | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  |
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.8.0 | Proporciona un canal de telemetría para el SDK de Windows Server para Application Insights que conservará la telemetría en escenarios sin conexión. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="listenerscollectorsappenders"></a>Agentes de escucha/recopiladores/appenders

| Nombre del paquete | Versión estable | DESCRIPCIÓN | Descargar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.DiagnosticSourceListener | 2.7.2 |  Permite el reenvío de eventos de DiagnosticSource a Application Insights. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/) |
| Microsoft.ApplicationInsights.EventSourceListener | 2.7.2 | EventSourceListener de Application Insights permite enviar datos de eventos de EventSource a Application Insights. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/) |
| Microsoft.ApplicationInsights.EtwCollector | 2.7.2 | EtwCollector de Application Insights permite enviar datos de Seguimiento de eventos para Windows (ETW) a Application Insights. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/) |
| Microsoft.ApplicationInsights.TraceListener | 2.7.2 | Un TraceListener personalizado que le permite enviar mensajes de registro de seguimiento a Application Insights. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/) |
| Microsoft.ApplicationInsights.Log4NetAppender | 2.7.2 | Un appender personalizado que le permite enviar mensajes de registro de Log4Net a Application Insights. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
| Microsoft.ApplicationInsights.NLogTarget | 2.7.2 |  Un destino personalizado que le permite enviar mensajes de registro de NLog a Application Insights. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
| Microsoft.ApplicationInsights.SnapshotCollector | 1.3.1 | Supervisa las excepciones en la aplicación y recopila automáticamente las instantáneas para realizar análisis sin conexión. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector/)

## <a name="service-fabric"></a>Service Fabric

| Nombre del paquete | Versión estable | DESCRIPCIÓN | Descargar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.ServiceFabric | 2.2.0 | Este paquete proporciona decoración automática de telemetría con el contexto de Service Fabric en el que se ejecuta la aplicación. No use este NuGet para aplicaciones nativas de Service Fabric. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric/) |
| Microsoft.ApplicationInsights.ServiceFabric.Native | 2.2.0 | Módulo de Application Insights para aplicaciones de Service Fabric. Use este NuGet para aplicaciones nativas de Service Fabric. Para aplicaciones que se ejecutan en contenedores, use el paquete Microsoft.ApplicationInsights.ServiceFabric. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric.Native/) |  

## <a name="status-monitor"></a>Monitor de estado

| Nombre del paquete | Versión estable | DESCRIPCIÓN | Descargar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.Agent_x64 | 2.2.1 |  Habilita la recopilación de datos en tiempo de ejecución para aplicaciones x64 | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x64/) |
| Microsoft.ApplicationInsights.Agent_x86 | 2.2.1 |  Habilita la recopilación de datos en tiempo de ejecución para aplicaciones x86. | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x86/) |

Estos paquetes forman parte de la funcionalidad básica de la supervisión en tiempo de ejecución en [Monitor de estado](../../azure-monitor/app/monitor-performance-live-website-now.md). No necesita descargar estos paquetes directamente; use solo el instalador del Monitor de estado. Si desea obtener más información sobre cómo funcionan estos paquetes en segundo plato, esta [entrada de blog](https://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/) de uno de nuestros desarrolladores es un buen punto de partida.

## <a name="additional-packages"></a>Paquetes adicionales

| Nombre del paquete | Versión estable | DESCRIPCIÓN | Descargar |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AzureWebSites | 2.6.5 | Esta extensión permite la supervisión de Application Insights en Azure App Service. Versión del SDK 2.6.1. Instrucciones: Agregue la configuración de la aplicación "APPINSIGHTS_INSTRUMENTATIONKEY" con su ikey y reinicie la aplicación web para que surta efecto.| [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AzureWebSites/) |
| Microsoft.ApplicationInsights.Injector | 2.6.7 | Este paquete contiene los archivos necesarios para la inserción de Application Insights sin código | [Descargar paquete](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Injector/) |

## <a name="next-steps"></a>Pasos siguientes

- Supervisar [ASP.NET Core](../../azure-monitor/app/asp-net-core.md).
- Generar perfiles de [aplicaciones web con Linux en Azure](../../azure-monitor/app/profiler-aspnetcore-linux.md) para ASP.NET Core.
- Depurar [instantáneas](../../azure-monitor/app/snapshot-debugger.md) de ASP.NET.