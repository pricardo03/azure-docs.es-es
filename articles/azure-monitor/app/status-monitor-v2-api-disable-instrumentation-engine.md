---
title: 'Referencia de API v2 de Azure Monitor de estado: Deshabilitar el motor de instrumentación | Microsoft Docs'
description: Referencia de API v2 de Monitor de estado. Disable-InstrumentationEngine. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514423"
---
# <a name="status-monitor-v2-api-disable-instrumentationengine-v021-alpha"></a>API de v2 del Monitor de estado: Disable-InstrumentationEngine (v0.2.1-alpha)

Este artículo describe un cmdlet que forme parte del [módulo de Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no es aconsejable para cargas de trabajo de producción. Podrían no admitir algunas características, y algunas pueden tener funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIPCIÓN
Deshabilita el motor de instrumentación mediante la eliminación de algunas claves del registro.
Reinicie IIS para que los cambios surtan efecto.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.

## <a name="examples"></a>Ejemplos

```powershell
PS C:\> Disable-InstrumentationEngine
```

## <a name="parameters"></a>Parámetros 

### <a name="-verbose"></a>-Verbose
**Parámetro común.** Use este modificador para enviar registros detallados.

## <a name="output"></a>Salida


#### <a name="example-output-from-successfully-disabling-the-instrumentation-engine"></a>Ejemplo de salida de deshabilitar correctamente el motor de instrumentación

```
Configuring IIS Environment for instrumentation engine...
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]'
Registry: removing 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]'
Configuring registry for instrumentation engine...
```


## <a name="next-steps"></a>Pasos siguientes

 Hacer más con el Monitor de estado v2:
 - Use nuestra guía para [solucionar](status-monitor-v2-troubleshoot.md) v2 del Monitor de estado.
