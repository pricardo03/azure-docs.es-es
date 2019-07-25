---
title: 'Referencia de la API del Monitor de estado de Azure v2: Deshabilitación del motor de instrumentación | Microsoft Docs'
description: Referencia de la API del Monitor de estado de Azure v2. Disable-InstrumentationEngine. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: b5ff218ce7bc7593d38f496b22d2a03402a00cdc
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66514423"
---
# <a name="status-monitor-v2-api-disable-instrumentationengine-v021-alpha"></a>API del Monitor de estado v2: Disable-InstrumentationEngine (v0.2.1-alpha)

Este artículo describe un cmdlet que forma parte del [módulo Az.ApplicationMonitor de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Monitor de estado v2 está actualmente en la versión preliminar pública.
> Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

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

## <a name="output"></a>Salida


#### <a name="example-output-from-successfully-disabling-the-instrumentation-engine"></a>Salida de ejemplo al deshabilitar correctamente el motor de instrumentación

```
Configuring IIS Environment for instrumentation engine...
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]'
Configuring registry for instrumentation engine...
```


## <a name="next-steps"></a>Pasos siguientes

 Hacer más con el Monitor de estado v2:
 - Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) del Monitor de estado v2.
