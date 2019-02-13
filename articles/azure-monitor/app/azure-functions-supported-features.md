---
title: Características compatibles con Azure Application Insights:Azure Functions | Microsoft Docs
description: Características compatibles de Application Insights para Azure Functions
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: ''
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 10/05/2018
ms.reviewer: mbullwin
ms.author: tilee
ms.openlocfilehash: dd28bc3925b0f07a441c46a26498ef1a14c3e650
ms.sourcegitcommit: fea5a47f2fee25f35612ddd583e955c3e8430a95
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55510330"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Características compatibles de Application Insights para Azure Functions

Azure Functions ofrece [integración incorporada](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) con Application Insights, que está disponible a través de la interfaz de ILogger. A continuación se muestra una lista de las características que se admiten actualmente. Para [empezar](https://github.com/Azure/Azure-Functions/wiki/App-Insights), consulte la guía de Azure Functions.

## <a name="supported-features"></a>Características compatibles

| Azure Functions                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **SDK de .NET de Application Insights**   | **2.5.0**       | **2.7.2**         |
| | | | 
| **Recopilación automática de**        |                 |                   |               
| &bull;Solicitudes                     | Sí             | Sí               | 
| &bull;Excepciones                   | Sí             | Sí               | 
| &bull; Contadores de rendimiento         | Sí             |                   |
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
