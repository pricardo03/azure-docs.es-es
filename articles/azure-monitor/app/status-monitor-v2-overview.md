---
title: Introducción al Monitor de estado de Azure v2 | Microsoft Docs
description: Introducción al Monitor de estado v2. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: 2126408222433e6339723dc2da0d2611bb234fe8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66734159"
---
# <a name="status-monitor-v2"></a>Monitor de estado v2

Monitor de estado V2 es un módulo de PowerShell publicado en la [Galería de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor).
Reemplaza al [Monitor de estado](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now).
El módulo proporciona instrumentación sin código de aplicaciones web .NET hospedadas con IIS.
Se envía telemetría a Azure Portal, donde puede [supervisar](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) la aplicación.

> [!IMPORTANT]
> Monitor de estado v2 está actualmente en la versión preliminar pública.
> Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="powershell-gallery"></a>Galería de PowerShell

La Galería de PowerShell se encuentra aquí: https://www.powershellgallery.com/packages/Az.ApplicationMonitor.


## <a name="instructions"></a>Instrucciones
- Consulte las [instrucciones de introducción](status-monitor-v2-get-started.md) para empezar con ejemplos concisos de código.
- Consulte las [instrucciones detalladas](status-monitor-v2-detailed-instructions.md) para profundizar.

## <a name="powershell-api-reference"></a>Referencia de la API de PowerShell
- [Disable-ApplicationInsightsMonitoring](status-monitor-v2-api-disable-monitoring.md)
- [Disable-InstrumentationEngine](status-monitor-v2-api-disable-instrumentation-engine.md)
- [Enable-ApplicationInsightsMonitoring](status-monitor-v2-api-enable-monitoring.md)
- [Enable-InstrumentationEngine](status-monitor-v2-api-enable-instrumentation-engine.md)
- [Get-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-get-config.md)
- [Get-ApplicationInsightsMonitoringStatus](status-monitor-v2-api-get-status.md)
- [Set-ApplicationInsightsMonitoringConfig](status-monitor-v2-api-set-config.md)

## <a name="troubleshooting"></a>solución de problemas
- [Solución de problemas](status-monitor-v2-troubleshoot.md)
- [Problemas conocidos](status-monitor-v2-troubleshoot.md#known-issues)


## <a name="faq"></a>Preguntas más frecuentes

- ¿El Monitor de estado v2 es compatible con instalaciones de proxy?

  *Sí*. Hay varias maneras de descargar el Monitor de estado v2. Si el equipo tiene acceso a internet, puede incorporarlo en la Galería de PowerShell mediante los parámetros `-Proxy`.
Puede descargar manualmente el módulo e instalarlo en el equipo o usarlo directamente.
Cada una de estas opciones se describe en las [instrucciones detalladas](status-monitor-v2-detailed-instructions.md).
  
- ¿Cómo se puede comprobar que la habilitación se realizó correctamente?

   No hay ningún cmdlet para eso.
Se recomienda usar [Live Metrics](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) para determinar rápidamente si la aplicación envía telemetría.

   También puede usar [Log Analytics](../log-query/get-started-portal.md) para enumerar todos los roles en la nube que están enviando actualmente telemetría:
   ```Kusto
   union * | summarize count() by cloud_RoleName, cloud_RoleInstance
   ```

## <a name="next-steps"></a>Pasos siguientes

Vea la telemetría:

* [Explore las métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y el uso.
* [Busque en los eventos y los registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
* [Use Analytics](../../azure-monitor/app/analytics.md) para consultas más avanzadas.
* [Cree paneles](../../azure-monitor/app/overview-dashboard.md).

Agregue más telemetría:

* [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
* [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
* [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar llamadas de seguimiento y registro.

