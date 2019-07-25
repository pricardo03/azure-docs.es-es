---
title: 'Referencia de la API del Monitor de estado de Azure v2: Obtención de la configuración | Microsoft Docs'
description: Referencia de la API del Monitor de estado de Azure v2. Get-ApplicationInsightsMonitoringConfig. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: 1f7061c9823ddc8ff7f8f42976041f1c9ff68fc0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66514398"
---
# <a name="status-monitor-v2-api-get-applicationinsightsmonitoringconfig-v021-alpha"></a>API del Monitor de estado v2: Get-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

Este artículo describe un cmdlet que forma parte del [módulo Az.ApplicationMonitor de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Monitor de estado v2 está actualmente en la versión preliminar pública.
> Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIPCIÓN

Obtiene el archivo de configuración e imprime los valores en la consola.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.

## <a name="examples"></a>Ejemplos

```powershell
PS C:\> Get-ApplicationInsightsMonitoringConfig
```

## <a name="parameters"></a>Parámetros

No se requiere ningún parámetro.

## <a name="output"></a>Salida


#### <a name="example-output-from-reading-the-config-file"></a>Salida de ejemplo al leer el archivo de configuración

```
RedfieldConfiguration:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
```

## <a name="next-steps"></a>Pasos siguientes

  Vea la telemetría:
 - [Explore las métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y el uso.
- [Busque en los eventos y los registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
- Use [análisis](../../azure-monitor/app/analytics.md) para consultas más avanzadas.
- [Cree paneles](../../azure-monitor/app/overview-dashboard.md).
 
 Agregue más telemetría:
 - [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar llamadas de seguimiento y registro.
 
 Hacer más con el Monitor de estado v2:
 - Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) del Monitor de estado v2.
 - Realice cambios en la configuración mediante el cmdlet [Set config](status-monitor-v2-api-set-config.md).
