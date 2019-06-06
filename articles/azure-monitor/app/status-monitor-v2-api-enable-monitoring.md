---
title: 'Referencia de API v2 de Azure Monitor de estado: Habilitar la supervisión | Microsoft Docs'
description: Referencia de API v2 de Monitor de estado. Enable-ApplicationInsightsMonitoring. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: e87bfad11eee5b86d35e6b4f2846b094c467e0ef
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734181"
---
# <a name="status-monitor-v2-api-enable-applicationinsightsmonitoring-v021-alpha"></a>API de v2 del Monitor de estado: Enable-ApplicationInsightsMonitoring (v0.2.1-alpha)

Este artículo describe un cmdlet que forme parte del [módulo de Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no es aconsejable para cargas de trabajo de producción. Podrían no admitir algunas características, y algunas pueden tener funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIPCIÓN

Sin código permite adjuntar la supervisión de aplicaciones IIS en un equipo de destino.

Este cmdlet modificará applicationHost.config IIS y establecer algunas claves del registro.
También creará un archivo applicationinsights.ikey.config, que define la clave de instrumentación usada por cada aplicación.
IIS cargará el RedfieldModule durante el inicio, que se introducen los SDK de Application Insights en aplicaciones como las aplicaciones de inicio.
Reinicie IIS para que los cambios surtan efecto.

Después de habilitar la supervisión, se recomienda que use [Live Metrics](live-stream.md) para comprobar rápidamente si la aplicación enviaba telemetría.


> [!NOTE] 
> - Para empezar, necesita una clave de instrumentación. Para obtener más información, consulte [crear un recurso](create-new-resource.md#copy-the-instrumentation-key).
> - Este cmdlet requiere que revise y acepte la licencia y declaración de privacidad.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador y una directiva de ejecución con privilegios elevados. Para obtener más información, consulte [ejecutar PowerShell como administrador con una directiva de ejecución con privilegios elevados](status-monitor-v2-detailed-instructions.md#run-powershell-as-admin-with-an-elevated-execution-policy).

## <a name="examples"></a>Ejemplos

### <a name="example-with-a-single-instrumentation-key"></a>Ejemplo con una única clave de instrumentación
En este ejemplo, todas las aplicaciones en el equipo actual se asignan a una única clave de instrumentación.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-an-instrumentation-key-map"></a>Ejemplo con un mapa de claves de instrumentación
En este ejemplo:
- `MachineFilter` coincide con el equipo actual usando el `'.*'` comodín.
- `AppFilter='WebAppExclude'` Proporciona un `null` clave de instrumentación. No se puede instrumentar la aplicación especificada.
- `AppFilter='WebAppOne'` asigna una clave de instrumentación única de la aplicación especificada.
- `AppFilter='WebAppTwo'` asigna una clave de instrumentación única de la aplicación especificada.
- Por último, `AppFilter` también usa el `'.*'` comodín que coincide con todas las aplicaciones web que no estén asignadas por las reglas anteriores y asignación una clave de instrumentación de forma predeterminada.
- Se agregan espacios para mejorar la legibilidad.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'})

```


## <a name="parameters"></a>Parámetros

### <a name="-instrumentationkey"></a>-InstrumentationKey
**Requerido.** Use este parámetro para proporcionar una única clave de instrumentación para su uso por todas las aplicaciones en el equipo de destino.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Requerido.** Use este parámetro para proporcionar varias claves de instrumentación y una asignación de las claves de instrumentación utilizados por cada aplicación.
Puede crear un script de instalación única para varios equipos estableciendo `MachineFilter`.

> [!IMPORTANT]
> Las aplicaciones se harán coincidir con las reglas en el orden en que se proporcionan las reglas. Por lo que debe especificar las reglas más específicas en primer lugar y la última de las reglas más genéricas.

#### <a name="schema"></a>Esquema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'})`

- **MachineFilter** es obligatoria C# regex del equipo o el nombre de la máquina virtual.
    - ". *" coincide con todos
    - 'ComputerName' coincidirá con solo los equipos con el nombre exacto especificado.
- **AppFilter** es obligatoria C# regex del equipo o el nombre de la máquina virtual.
    - ". *" coincide con todos
    - 'ApplicationName' hará coincidir solo las aplicaciones IIS con el nombre exacto especificado.
- **InstrumentationKey** se requiere para habilitar la supervisión de aplicaciones que coinciden con los dos filtros anteriores.
    - Deje este valor null si desea definir reglas para excluir de la supervisión.


### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**Opcional.** Utilice este modificador para habilitar el motor de instrumentación recopilar eventos y mensajes sobre lo que sucede durante la ejecución de un proceso administrado. Estos eventos y mensajes incluyen códigos de resultado de la dependencia, verbos HTTP y texto de comando SQL.

El motor de instrumentación se sobrecarga y está desactivada de forma predeterminada.

### <a name="-acceptlicense"></a>-AcceptLicense
**Opcional.** Use este modificador para aceptar la licencia y declaración de privacidad en las instalaciones sin periféricos.

### <a name="-verbose"></a>-Verbose
**Parámetro común.** Utilice este modificador para mostrar los registros detallados.

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
 - [Explorar métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y uso.
- [Busque eventos y registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
- [Uso de Analytics](../../azure-monitor/app/analytics.md) para obtener más consultas avanzadas.
- [Crear paneles](../../azure-monitor/app/overview-dashboard.md).
 
 Agregue más telemetría:
 - [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar seguimiento y registrar las llamadas.
 
 Hacer más con el Monitor de estado v2:
 - Use nuestra guía para [solucionar](status-monitor-v2-troubleshoot.md) v2 del Monitor de estado.
 - [Obtener la configuración](status-monitor-v2-api-get-config.md) para confirmar que la configuración se registraron correctamente.
 - [Obtener el estado](status-monitor-v2-api-get-status.md) para inspeccionar la supervisión.
