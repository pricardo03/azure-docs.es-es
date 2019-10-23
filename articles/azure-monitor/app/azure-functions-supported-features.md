---
title: Características compatibles con Azure Application Insights:Azure Functions | Microsoft Docs
description: Características compatibles de Application Insights para Azure Functions
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: reference
author: TimothyMothra
ms.author: tilee
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 31f962ca96ca5c47d18f9250e567abb8f4024e6f
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72677558"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Características compatibles de Application Insights para Azure Functions

Azure Functions ofrece [integración incorporada](../../azure-functions/functions-monitoring.md) con Application Insights, que está disponible a través de la interfaz de ILogger. A continuación se muestra una lista de las características que se admiten actualmente. Para [empezar](../../azure-functions/functions-monitoring.md#enable-application-insights-integration), consulte la guía de Azure Functions.

## <a name="supported-features"></a>Características compatibles

| Azure Functions                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **SDK de .NET de Application Insights**   | **2.5.0**       | **2.9.1**         |
| | | | 
| **Recopilación automática de**        |                 |                   |               
| &bull;Solicitudes                     | Sí             | Sí               | 
| &bull;Excepciones                   | Sí             | Sí               | 
| &bull; Contadores de rendimiento         | Sí             | Sí               |
| &bull;Dependencias                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | Sí               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | Sí               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | Sí               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | Sí               | 
| | | | 
| **Características compatibles**                |                   |                   |               
| &bull;QuickPulse/LiveMetrics       | Sí             | Sí               | 
| &nbsp;&nbsp;&nbsp;&mdash; Canal de control seguro|                 | Sí               | 
| &bull;Muestreo                     | Sí             | Sí               | 
| &bull;Latidos                   |                 | Sí               | 
| | | | 
| **Correlación**                       |                   |                   |               
| &bull; ServiceBus                     |                   | Sí               | 
| &bull; EventHub                       |                   | Sí               | 
| | | | 
| **Configurable**                      |                   |                   |           
| &bull;Totalmente configurable.<br/>Consulte [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) para obtener instrucciones al respecto.<br/>Consulte [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) para ver todas las opciones.               |                   | Sí                   | 


## <a name="performance-counters"></a>Contadores de rendimiento

La recopilación automática de los contadores de rendimiento solo funciona en las máquinas Windows.


## <a name="live-metrics--secure-control-channel"></a>Live Metrics y canal de control seguro

Los criterios de filtros personalizados que especifique se envían hacia el componente de Live Metrics del SDK de Application Insights. Los filtros podrían contener información confidencial, como identificadores de clientes. Puede proteger el canal con una clave de API secreta. Consulte las instrucciones en [Protección del canal de control](https://docs.microsoft.com/azure/azure-monitor/app/live-stream#secure-the-control-channel).

## <a name="sampling"></a>muestreo

Azure Functions habilita el muestreo de forma predeterminada en su configuración. Para más información, consulte [Configuración del muestreo](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).

Si el proyecto toma una dependencia del SDK de Application Insights para realizar el seguimiento de telemetría manual, puede experimentar un comportamiento extraño si la configuración de muestreo es diferente de la configuración de muestreo de Functions. 

Se recomienda usar la misma configuración que Functions. Con **Functions v2**, puede obtener la misma configuración mediante la inserción de dependencias en el constructor:

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;

public class Function1 
{

    private readonly TelemetryClient telemetryClient;

    public Function1(TelemetryConfiguration configuration)
    {
        this.telemetryClient = new TelemetryClient(configuration);
    }

    [FunctionName("Function1")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger logger)
    {
        this.telemetryClient.TrackTrace("C# HTTP trigger function processed a request.");
    }
}
```
