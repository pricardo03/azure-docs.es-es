---
title: 'Referencia de la API del Monitor de estado de Azure v2: Habilitación de la supervisión | Microsoft Docs'
description: Referencia de la API del Monitor de estado de Azure v2. Enable-ApplicationInsightsMonitoring. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: 1e5a4ac9f159ce238149d44127fd63dcf3279623
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67807097"
---
# <a name="status-monitor-v2-api-enable-applicationinsightsmonitoring-v040-alpha"></a>API del Monitor de estado v2: Enable-ApplicationInsightsMonitoring (v0.4.0-alpha)

Este artículo describe un cmdlet que forma parte del [módulo Az.ApplicationMonitor de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Monitor de estado v2 está actualmente en la versión preliminar pública.
> Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIPCIÓN

Permite adjuntar sin código la supervisión de aplicaciones IIS en un equipo de destino.

Este cmdlet modificará el archivo IIS applicationHost.config y establecerá algunas claves del Registro.
También creará un archivo applicationinsights.ikey.config, que define la clave de instrumentación usada por cada aplicación.
IIS cargará RedfieldModule durante el inicio, lo que insertará los SDK de Application Insights en aplicaciones como las de inicio.
Reinicie IIS para que los cambios surtan efecto.

Después de habilitar la supervisión, se recomienda que use [Live Metrics](live-stream.md) para comprobar rápidamente si la aplicación enviaba telemetría.


> [!NOTE] 
> - Para empezar, necesita una clave de instrumentación. Para más información, consulte [Crear un recurso](create-new-resource.md#copy-the-instrumentation-key).
> - Este cmdlet requiere que revise y acepte la licencia, y la declaración de privacidad.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador y una directiva de ejecución elevada. Para más información, consulte [Ejecute PowerShell como administrador con una directiva de ejecución con privilegios elevados](status-monitor-v2-detailed-instructions.md#run-powershell-as-admin-with-an-elevated-execution-policy).

## <a name="examples"></a>Ejemplos

### <a name="example-with-a-single-instrumentation-key"></a>Ejemplo con una única clave de instrumentación
En este ejemplo, a todas las aplicaciones en el equipo actual se les asigna una única clave de instrumentación.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### <a name="example-with-an-instrumentation-key-map"></a>Ejemplo con un mapa de claves de instrumentación
En este ejemplo:
- `MachineFilter` busca la correspondencia con el equipo actual usando el comodín `'.*'`.
- `AppFilter='WebAppExclude'` proporciona una clave de instrumentación `null`. No se puede instrumentar la aplicación especificada.
- `AppFilter='WebAppOne'` asigna una clave de instrumentación única a la aplicación especificada.
- `AppFilter='WebAppTwo'` asigna una clave de instrumentación única a la aplicación especificada.
- Por último, `AppFilter` también usa el comodín `'.*'` para buscar la correspondencia de todas las aplicaciones web que no coincidan según las reglas anteriores y les asigna una clave de instrumentación predeterminada.
- Se agregan espacios para mejorar la legibilidad.

```powershell
PS C:\> Enable-ApplicationInsightsMonitoring -InstrumentationKeyMap 
    @(@{MachineFilter='.*';AppFilter='WebAppExclude'},
      @{MachineFilter='.*';AppFilter='WebAppOne';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx1'}},
      @{MachineFilter='.*';AppFilter='WebAppTwo';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2'}},
      @{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault'}})

```


## <a name="parameters"></a>Parámetros

### <a name="-instrumentationkey"></a>-InstrumentationKey
**Requerido.** Use este parámetro para proporcionar una única clave de instrumentación para que la usen todas las aplicaciones en el equipo de destino.

### <a name="-instrumentationkeymap"></a>-InstrumentationKeyMap
**Requerido.** Use este parámetro para proporcionar varias claves de instrumentación y una asignación de las claves de instrumentación utilizadas por cada aplicación.
Puede crear un solo script de instalación para varios equipos si establece `MachineFilter`.

> [!IMPORTANT]
> Las aplicaciones se harán coincidir con las reglas en el orden en que estas se proporcionan. Por tanto, debe especificar las reglas más específicas en primer lugar y las más genéricas las últimas.

#### <a name="schema"></a>Esquema
`@(@{MachineFilter='.*';AppFilter='.*';InstrumentationSettings=@{InstrumentationKey='xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'}})`

- **MachineFilter** es una expresión regular de C# requerida del nombre de la máquina virtual o del equipo.
    - ".*" coincidirá con todos
    - "ComputerName" coincidirá solo son aquellos equipos que tengan el nombre exacto especificado.
- **AppFilter** es una expresión regular de C# requerida del nombre del sitio de IIS. Para obtener una lista de sitios en el servidor, ejecute el comando [get-iissite](https://docs.microsoft.com/powershell/module/iisadministration/get-iissite).
    - ".*" coincidirá con todos
    - "SiteName" coincidirá solo son el sitio de IIS que tenga el nombre exacto especificado.
- **InstrumentationKey** se requiere para habilitar la supervisión de las aplicaciones que coincidan con los dos filtros anteriores.
    - Deje este valor como null si desea definir reglas para excluir la supervisión.


### <a name="-enableinstrumentationengine"></a>-EnableInstrumentationEngine
**Opcional.** Utilice este modificador para habilitar el motor de instrumentación y recopilar eventos y mensajes sobre lo que sucede durante la ejecución de un proceso administrado. Estos eventos y mensajes incluyen códigos de resultado de la dependencia, verbos HTTP y texto de comando SQL.

El motor de instrumentación se sobrecarga y está desactivado de forma predeterminada.

### <a name="-acceptlicense"></a>-AcceptLicense
**Opcional.** Use este modificador para aceptar la licencia y la declaración de privacidad en las instalaciones sin periféricos.

### <a name="-ignoresharedconfig"></a>-IgnoreSharedConfig
Si tiene un clúster de servidores web, puede que esté usando una [configuración compartida](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211).
HttpModule no pueden insertarse en esta configuración compartida.
Este script generará un error con un mensaje que indica que se necesitan pasos de instalación adicionales.
Use este modificador para ignorar esta comprobación y seguir instalando requisitos previos. Para obtener más información, consulte [Conflicto con la configuración compartida de IIS](status-monitor-v2-troubleshoot.md#conflict-with-iis-shared-configuration).

### <a name="-verbose"></a>-Verbose
**Parámetro común.** Utilice este modificador para mostrar registros detallados.

### <a name="-whatif"></a>-WhatIf 
**Parámetro común.** Utilice este modificador para probar y validar los parámetros de entrada sin habilitar realmente la supervisión.

## <a name="output"></a>Output


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
 - [Explore las métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y el uso.
- [Busque en los eventos y los registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
- [Use Analytics](../../azure-monitor/app/analytics.md) para consultas más avanzadas.
- [Cree paneles](../../azure-monitor/app/overview-dashboard.md).
 
 Agregue más telemetría:
 - [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar llamadas de seguimiento y registro.
 
 Hacer más con el Monitor de estado v2:
 - Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) del Monitor de estado v2.
 - [Obtenga la configuración](status-monitor-v2-api-get-config.md) para confirmar que sus opciones se registraron correctamente.
 - [Obtenga el estado](status-monitor-v2-api-get-status.md) para inspeccionar la supervisión.
