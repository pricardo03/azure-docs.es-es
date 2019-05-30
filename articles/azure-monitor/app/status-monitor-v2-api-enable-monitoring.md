---
title: 'Referencia de API de estado de Azure Monitor v2: Habilitar la supervisión | Microsoft Docs'
description: Habilitación de referencia de API-ApplicationInsightsMonitoring de estado Monitor v2. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: 0a443df0c55dc916ef6d12d53811e9d9932370e7
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255910"
---
# <a name="status-monitor-v2-api-enable-applicationinsightsmonitoring-v021-alpha"></a>API de v2 del Monitor de estado: Enable-ApplicationInsightsMonitoring (v0.2.1-alpha)

Este documento describe un cmdlet que se distribuye como un miembro de la [módulo de Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para obtener más información, consulte [términos de uso complementarios para vistas previas de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>DESCRIPCIÓN

Habilitar código menos adjuntar la supervisión de aplicaciones de IIS en un equipo de destino.
Este cmdlet modificará applicationHost.config IIS y establecer algunas claves del registro.
Este cmdlet también creará un applicationinsights.ikey.config, que define la aplicación que usa la clave de instrumentación.
IIS cargará el RedfieldModule al inicio, que se introducen los SDK de Application Insights en aplicaciones como estas aplicaciones se inician.
Reinicie IIS para que los cambios surtan efecto.

Después de habilitar la supervisión, se recomienda usar [Live Metrics](live-stream.md) a observar rápidamente si la aplicación enviaba telemetría.


> [!NOTE] 
> Para empezar, debe tener una clave de instrumentación. Para obtener más información, lea [crear recurso nuevo](create-new-resource.md#copy-the-instrumentation-key).


> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador y con una directiva de ejecución con privilegios elevados. Lectura [aquí](status-monitor-v2-detailed-instructions.md#run-powershell-as-administrator-with-an-elevated-execution-policy) para obtener más información.

> [!NOTE] 
> Este cmdlet, deberá revisar y aceptar la licencia y declaración de privacidad.


## <a name="examples"></a>Ejemplos

### <a name="example-with-single-instrumentation-key"></a>Ejemplo con la única clave de instrumentación
En este ejemplo, todas las aplicaciones en la máquina actual se asignará una única clave de instrumentación.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-instrumentation-key-map"></a>Ejemplo con el mapa de claves de instrumentación
En este ejemplo, 
- `MachineFilter` coincidirá con la máquina actual con el `'.*'` comodín.
- `AppFilter='WebAppExclude'` Proporciona un `null` InstrumentationKey. No se puede instrumentar esta aplicación.
- `AppFilter='WebAppOne'` asignará esta aplicación específica una clave de instrumentación única.
- `AppFilter='WebAppTwo'` También asignará esta aplicación específica una clave de instrumentación única.
- Por último, `AppFilter` también usa el `'.*'` comodín que coincide con todas las demás aplicaciones web no coinciden con las reglas anteriores y se asigna una clave de instrumentación de forma predeterminada.
- Para mejorar la legibilidad sólo de espacios.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>Parámetros 

### <a name="-instrumentationkey"></a>-InstrumentationKey
**Requerido.** Use este parámetro para proporcionar una clave iKey única para su uso por todas las aplicaciones del equipo de destino.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Requerido.** Use este parámetro para proporcionar varias claves iKey y una asignación de las aplicaciones que desea usar qué ikey. Puede crear un script de instalación única para varios equipos estableciendo el MachineFilter. 

> [!IMPORTANT] 
> Las aplicaciones se harán coincidir con las reglas en el orden en que les proporciona. Por lo tanto debe especificar las reglas más específicas en primer lugar y la última de las reglas más genéricas.

#### <a name="schema"></a>Esquema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** un regex requiere c# del nombre de equipo o máquina virtual.
    - ". *" coincide con todos
    - 'ComputerName' coincidirá con solo los equipos con ese nombre exacto.
- **AppFilter** un regex requiere c# del nombre de equipo o máquina virtual.
    - ". *" coincide con todos
    - 'ApplicationName' coincidirá con aplicaciones de IIS solo con ese nombre exacto.
- **InstrumentationKey** se requiere para habilitar la supervisión de las aplicaciones que coinciden con los dos filtros anteriores.
    - Deje este valor null si desea definir reglas para excluir de la supervisión


### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**Opcional.** Utilice este modificador para habilitar el motor de instrumentación recopilar eventos y mensajes de lo que sucede a durante la ejecución de un proceso administrado. Incluyendo pero sin limitarse a códigos de resultado de la dependencia, verbos HTTP y texto de comando SQL. El motor de instrumentación se agregará una sobrecarga adicional y está desactivada de forma predeterminada.

### <a name="-acceptlicense"></a>-AcceptLicense
**Opcional.** Use este modificador para aceptar la licencia y declaración de privacidad en las instalaciones sin periféricos.

### <a name="-verbose"></a>-Verbose
**Parámetro común.** Use este modificador para enviar registros detallados.

### <a name="-whatif"></a>-WhatIf 
**Parámetro común.** Utilice este modificador para probar y validar los parámetros de entrada sin habilitar realmente la supervisión.

## <a name="output"></a>Salida


#### <a name="example-output-from-a-successful-enablement"></a>Ejemplo de salida de una activación correcta

```
Initiating Disable Process
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z'
in :1,237
No element in the source document matches '/configuration/location[@path='']/system.webServer/modules/add[@name='ManagedHttpModuleHelper']'
Not executing RemoveAll (transform line 1, 546)
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'disable'
GAC Module will not be removed, since this operation might cause IIS instabilities
Configuring IIS Environment for codeless attach...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring IIS Environment for instrumentation engine...
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\IISADMIN[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W3SVC[Environment]
Registry: skipping non-existent 'HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WAS[Environment]
Configuring registry for instrumentation engine...
Successfully disabled Application Insights Status Monitor
Installing GAC module 'C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\0.2.0\content\Runtime\Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll'
Applying transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config'
Found GAC module Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35
'C:\Windows\System32\inetsrv\config\applicationHost.config' backed up to 'C:\Windows\System32\inetsrv\config\applicationHost.config.backup-2019-03-26_08-59-52z_1'
Transformation to 'C:\Windows\System32\inetsrv\config\applicationHost.config' was successfully applied. Operation: 'enable'
Configuring IIS Environment for codeless attach...
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
Updating app pool permissions...
Successfully enabled Application Insights Status Monitor
```

## <a name="next-steps"></a>Pasos siguientes

  Vea la telemetría:
 - [Explore las métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y uso.
- [Busque eventos y registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas
- [Análisis](../../azure-monitor/app/analytics.md) para más consultas avanzadas
- [Creación de paneles](../../azure-monitor/app/overview-dashboard.md)
 
 Agregue más telemetría:
 - [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para que le permitan insertar llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar seguimiento y registrar las llamadas
 
 Hacer más con el Monitor de estado v2:
 - Use nuestra guía para [solucionar](status-monitor-v2-troubleshoot.md) v2 del Monitor de estado.
 - [Obtener la configuración](status-monitor-v2-api-get-config.md) para confirmar que la configuración se registraron correctamente.
 - [Obtener el estado](status-monitor-v2-api-get-status.md) para inspeccionar la supervisión.
