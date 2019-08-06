---
title: 'Application Insights: lenguajes, plataformas e integraciones| Microsoft Docs'
description: Lenguajes, plataformas e integraciones disponibles para Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/18/2019
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: dd2fb0c9ef157e3706ca3d902f7feebaf4c2f279
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68478908"
---
# <a name="supported-languages"></a>Idiomas admitidos

* [C#|VB (.NET)](../../azure-monitor/app/asp-net.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [JavaScript](../../azure-monitor/app/javascript.md)
* [Node.JS](../../azure-monitor/app/nodejs.md)

## <a name="supported-platforms-and-frameworks"></a>Plataformas y marcos compatibles

### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Instrumentación de aplicaciones ya implementadas (sin código y basadas en agentes)
* [Conjuntos de escalado de máquinas virtuales de Azure y Azure VM](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [Azure App Service](../../azure-monitor/app/azure-web-apps.md)
* [ASP.NET: para aplicaciones que ya están activas](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Azure Cloud Services](../../azure-monitor/app/cloudservices.md), incluidos los roles web y de trabajo
* [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
### <a name="instrumentation-through-code-sdks"></a>Instrumentación a través del código (SDK)
* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Android](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (App Center)
* [iOS](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Java EE](../../azure-monitor/app/java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [Aplicación Windows universal](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Aplicaciones, servicios y roles de trabajo del escritorio de Windows](../../azure-monitor/app/windows-desktop.md)

## <a name="logging-frameworks"></a>Marcos de registro
* [ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger)
* [Log4Net, NLog o System.Diagnostics.Trace](../../azure-monitor/app/asp-net-trace-logs.md)
* [Java, Log4J o Logback](../../azure-monitor/app/java-trace-logs.md)
* [Complemento LogStash](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure Monitor](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)

## <a name="export-and-data-analysis"></a>Exportación y análisis de datos
* [Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](../../azure-monitor/app/export-power-bi.md)

## <a name="unsupported-community-sdks"></a>SDK de comunidad no admitidos

Existe una serie de SDK de Application Insights de la comunidad. Microsoft no admite oficialmente los SDK de la comunidad ni los mantiene. No podemos proporcionar soporte técnico para ningún SDK que no se encuentre en la lista oficialmente admitida. 