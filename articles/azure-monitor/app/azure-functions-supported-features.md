---
title: 'Características compatibles de Azure Application Insights: Azure Functions'
description: Características compatibles de Application Insights para Azure Functions
ms.topic: reference
author: TimothyMothra
ms.author: tilee
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.openlocfilehash: cf0c97fd65f9966bf42fa22e2c8f92263952cb7a
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77655657"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Características compatibles de Application Insights para Azure Functions

Azure Functions ofrece [integración incorporada](../../azure-functions/functions-monitoring.md) con Application Insights, que está disponible a través de la interfaz de ILogger. A continuación se muestra una lista de las características que se admiten actualmente. Para [empezar](../../azure-functions/functions-monitoring.md#enable-application-insights-integration), consulte la guía de Azure Functions.

Para más información sobre las versiones de runtime de Functions, vaya [aquí](../../azure-functions/functions-versions.md).

Para obtener más información sobre las versiones compatibles de Application Insights, vea [Dependencias](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/).

## <a name="supported-features"></a>Características admitidas

| Azure Functions                       | V1                | v2 y v3   | 
|-----------------------------------    |---------------    |------------------ |
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
