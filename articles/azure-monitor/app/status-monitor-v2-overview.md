---
title: Información general sobre Azure Monitor de estado v2 | Microsoft Docs
description: Información general del Monitor de estado v2. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734159"
---
# <a name="status-monitor-v2"></a>Monitor de estado v2

V2 de Monitor de estado es un PowerShell módulo publicado en el [Galería de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor).
Reemplaza [Monitor de estado](https://docs.microsoft.com/azure/azure-monitor/app/monitor-performance-live-website-now).
El módulo proporciona instrumentación sin código de aplicaciones web hospedadas con IIS.
Se envía telemetría al portal de Azure, donde puede [monitor](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) la aplicación.

> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no es aconsejable para cargas de trabajo de producción. Podrían no admitir algunas características, y algunas pueden tener funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="powershell-gallery"></a>Galería de PowerShell

La Galería de PowerShell se encuentra aquí: https://www.powershellgallery.com/packages/Az.ApplicationMonitor.


## <a name="instructions"></a>Instrucciones
- Consulte la [instrucciones de introducción](status-monitor-v2-get-started.md) para empezar con ejemplos de código conciso.
- Consulte la [instrucciones detalladas](status-monitor-v2-detailed-instructions.md) para profundizar en cómo empezar a trabajar.

## <a name="powershell-api-reference"></a>Referencia de API de PowerShell
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

- ¿V2 del Monitor de estado compatible con las instalaciones del proxy?

  *Sí*. Hay varias maneras de descargar el Monitor de estado v2. Si el equipo tiene acceso a internet, puede incorporar en la Galería de PowerShell mediante el uso de `-Proxy` parámetros.
Puede descargar manualmente el módulo e instalarlo en su equipo o usarla directamente.
Cada una de estas opciones se describe en el [instrucciones detalladas](status-monitor-v2-detailed-instructions.md).
  
- ¿Cómo se puede comprobar que la habilitación se realizó correctamente?

   No hay ningún cmdlet para comprobar la activación se realizó correctamente.
Se recomienda usar [Live Metrics](https://docs.microsoft.com/azure/azure-monitor/app/live-stream) para determinar rápidamente si la aplicación envía telemetría.

   También puede usar [Log Analytics](../log-query/get-started-portal.md) para enumerar todos los roles en la nube actualmente enviando telemetría:
   ```Kusto
   union * | summarize count() by cloud_RoleName, cloud_RoleInstance
   ```

## <a name="next-steps"></a>Pasos siguientes

Vea la telemetría:

* [Explorar métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y uso.
* [Busque eventos y registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
* [Uso de Analytics](../../azure-monitor/app/analytics.md) para obtener más consultas avanzadas.
* [Crear paneles](../../azure-monitor/app/overview-dashboard.md).

Agregue más telemetría:

* [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
* [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
* [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar seguimiento y registrar las llamadas.

