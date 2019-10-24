---
title: 'Referencia de la API de Azure Application Insights Agent: Deshabilitación del motor de instrumentación | Microsoft Docs'
description: Referencia de la API de Application Insights Agent. Disable-InstrumentationEngine. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
services: application-insights
documentationcenter: .net
author: TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: aaf493b5f2075ead62087824773f21643a1246e4
ms.sourcegitcommit: bb65043d5e49b8af94bba0e96c36796987f5a2be
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72388306"
---
# <a name="application-insights-agent-api-disable-instrumentationengine"></a>API de Application Insights Agent: Disable-InstrumentationEngine

Este artículo describe un cmdlet que forma parte del [módulo Az.ApplicationMonitor de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

## <a name="description"></a>DESCRIPCIÓN
Deshabilita el motor de instrumentación mediante la eliminación de algunas claves del Registro.
Reinicie IIS para aplicar los cambios.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.

## <a name="examples"></a>Ejemplos

```powershell
PS C:\> Disable-InstrumentationEngine
```

## <a name="parameters"></a>Parámetros 

### <a name="-verbose"></a>-Verbose
**Parámetro común.** Use este modificador para generar registros detallados.

## <a name="output"></a>Output


#### <a name="example-output-from-successfully-disabling-the-instrumentation-engine"></a>Salida de ejemplo al deshabilitar correctamente el motor de instrumentación

```
Configuring IIS Environment for instrumentation engine...
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]'
Configuring registry for instrumentation engine...
```


## <a name="next-steps"></a>Pasos siguientes

 Haga mucho más con Application Insights Agent:
 - Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) de Application Insights Agent.
