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
ms.openlocfilehash: 9ad0579ff9c25753b1e4816b80948b4d8d1232f7
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54082557"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Características compatibles de Application Insights para Azure Functions

Azure Functions ofrece [integración incorporada](https://docs.microsoft.com/azure/azure-functions/functions-monitoring) con Application Insights, que está disponible a través de la interfaz de ILogger. A continuación se muestra una lista de las características que se admiten actualmente. Para [empezar](https://github.com/Azure/Azure-Functions/wiki/App-Insights), consulte la guía de Azure Functions.

## <a name="supported-features"></a>Características compatibles

| Azure Functions                       | V1                | V2 (Ignite 2018)  | 
|-----------------------------------    |---------------    |------------------ |
| **SDK de .NET de Application Insights**   | **2.5.0**       | **2.7.2**         |
| | | | 
| **Recopilación automática de**        |                 |                   |               
| &bull;Solicitudes                     | SÍ             | SÍ               | 
| &bull;Excepciones                   | SÍ             | SÍ               | 
| &bull;Dependencias                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash; HTTP      |                 | SÍ               | 
| &nbsp;&nbsp;&nbsp;&mdash; ServiceBus|                 | SÍ               | 
| &nbsp;&nbsp;&nbsp;&mdash; EventHub  |                 | SÍ               | 
| &nbsp;&nbsp;&nbsp;&mdash; SQL       |                 | SÍ               | 
| | | | 
| **Características compatibles**                |                   |                   |               
| &bull;QuickPulse/LiveMetrics       | SÍ             | SÍ               | 
| &bull;Muestreo                     | SÍ             | SÍ               | 
| &bull;Latidos                   |                 | SÍ               | 
| | | | 
| **Correlación**                       |                   |                   |               
| &bull; ServiceBus                     |                   | SÍ               | 
| &bull; EventHub                       |                   | SÍ               | 
| | | | 
| **Configurable**                      |                   |                   |           
| &bull;Totalmente configurable.<br/>Consulte [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) para obtener instrucciones al respecto.<br/>Consulte [Asp.NET Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) para ver todas las opciones.               |                   | SÍ                   | 


## <a name="sampling"></a>muestreo

Azure Functions habilita el muestreo de forma predeterminada en su configuración. Para más información, consulte [Configuración del muestreo](https://docs.microsoft.com/azure/azure-functions/functions-monitoring#configure-sampling).
