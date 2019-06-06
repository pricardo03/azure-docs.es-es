---
title: Problemas conocidos y solución de Azure Monitor de estado v2 | Microsoft Docs
description: Los problemas conocidos de Monitor de estado v2 y ejemplos de solución de problemas. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: f2115ee14b58030f695c9410870615f03d353cd2
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734962"
---
# <a name="troubleshooting-status-monitor-v2"></a>Solución de problemas de estado supervisar v2

Cuando se habilita la supervisión, podría experimentar problemas que impiden la recopilación de datos.
En este artículo se enumera todos los problemas conocidos y se proporciona ejemplos de solución de problemas.
Si encuentra un problema que no aparece aquí, puede en contacto con nosotros en [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).


> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no es aconsejable para cargas de trabajo de producción. Podrían no admitir algunas características, y algunas pueden tener funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="known-issues"></a>Problemas conocidos

### <a name="conflicting-dlls-in-an-apps-bin-directory"></a>Archivos DLL en conflicto en el directorio bin de una aplicación

Si cualquiera de estas DLL están presente en el directorio bin, supervisión podría dar error:

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

Algunos de estos archivos DLL se incluyen en las plantillas de aplicación de Visual Studio de forma predeterminada, incluso si la aplicación no los use.
Puede usar las herramientas de solución de problemas para ver el comportamiento síntoma:

- PerfView:
    ```
    ThreadID="7,500" 
    ProcessorNumber="0" 
    msg="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ExtVer="2.8.13.5972" 
    SubscriptionId="" 
    AppName="" 
    FormattedMessage="Found 'System.Diagnostics.DiagnosticSource, Version=4.0.2.1, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51' assembly, skipping attaching redfield binaries" 
    ```

- IISReset y aplicación de carga (sin telemetría). Investigue con Sysinternals (Handle.exe y ListDLLs.exe):
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="conflict-with-iis-shared-configuration"></a>Conflicto con la configuración compartida de IIS

Si tiene un clúster de servidores web, puede que esté usando un [configuración compartida](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211).
HttpModule no pueden insertarse en la configuración compartida.
Ejecute el comando Enable en cada servidor web para instalar el archivo DLL en GAC de cada servidor.

Después de ejecutar el comando Enable, siga estos pasos:
1. Vaya al directorio de configuración compartida y busque el archivo applicationHost.config.
2. Agregue esta línea a la sección de módulos de la configuración:
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```
## <a name="troubleshooting"></a>solución de problemas
    
### <a name="troubleshooting-powershell"></a>Solución de problemas de PowerShell

#### <a name="determine-which-modules-are-available"></a>Determinar qué módulos están disponibles
Puede usar el `Get-Module -ListAvailable` comando para determinar qué módulos se instalan.

#### <a name="import-a-module-into-the-current-session"></a>Importar un módulo en la sesión actual
Si no se ha cargado un módulo en una sesión de PowerShell, manualmente puede cargar mediante el `Import-Module <path to psd1>` comando.


### <a name="troubleshooting-the-status-monitor-v2-module"></a>El módulo de v2 del Monitor de estado de la solución de problemas

#### <a name="list-the-commands-available-in-the-status-monitor-v2-module"></a>Mostrar los comandos disponibles en el módulo de v2 del Monitor de estado
Ejecute el comando `Get-Command -Module Az.ApplicationMonitor` para obtener los comandos disponibles:

```
Command type     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Disable-ApplicationInsightsMonitoring              0.2.1      Az.ApplicationMonitor
Cmdlet          Disable-InstrumentationEngine                      0.2.1      Az.ApplicationMonitor
Cmdlet          Enable-ApplicationInsightsMonitoring               0.2.1      Az.ApplicationMonitor
Cmdlet          Enable-InstrumentationEngine                       0.2.1      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.2.1      Az.ApplicationMonitor
Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
```

#### <a name="determine-the-current-version-of-the-status-monitor-v2-module"></a>Determinar la versión actual del módulo de v2 de Monitor de estado
Ejecute el `Get-ApplicationInsightsMonitoringStatus` comando para mostrar la siguiente información sobre el módulo:
   - Versión del módulo de PowerShell
   - Versión de Application Insights SDK
   - Rutas de acceso de archivo del módulo de PowerShell
    
Revise el [referencia de API](status-monitor-v2-api-get-status.md) para obtener una descripción detallada de cómo usar este cmdlet.


### <a name="troubleshooting-running-processes"></a>Solución de problemas de los procesos en ejecución

Puede inspeccionar los procesos en el equipo instrumentado para determinar si se cargan todos los archivos DLL.
Si funciona la supervisión, se debe cargar al menos 12 dll.

Use el `Get-ApplicationInsightsMonitoringStatus -InspectProcess` comando para comprobar los archivos DLL.

Revise el [referencia de API](status-monitor-v2-api-get-status.md) para obtener una descripción detallada de cómo usar este cmdlet.


### <a name="collect-etw-logs-by-using-perfview"></a>Recopilar registros ETW mediante el uso de PerfView

#### <a name="setup"></a>Configuración

1. Descargar PerfView.exe y PerfView64.exe desde [GitHub](https://github.com/Microsoft/perfview/releases).
2. Iniciar PerfView64.exe.
3. Expanda **opciones avanzadas**.
4. Desactive estas casillas de verificación:
    - **Zip**
    - **Merge**
    - **Colección de símbolos de .NET**
5. Establecer estos **proveedores adicionales**: `61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>Recopilación de registros

1. En una consola de comandos con privilegios de administrador, ejecute el `iisreset /stop` comando para desactivar IIS y todas las aplicaciones web.
2. En PerfView, seleccione **iniciar colección**.
3. En una consola de comandos con privilegios de administrador, ejecute el `iisreset /start` comando para iniciar IIS.
4. Intente ir a la aplicación.
5. Cuando se carga la aplicación, vuelva a PerfView y seleccione **Detener colección**.



## <a name="next-steps"></a>Pasos siguientes

- Revise el [referencia de API](status-monitor-v2-overview.md#powershell-api-reference) para obtener información acerca de los parámetros que se podría haber pasado por alto.
- Si encuentra un problema que no aparece aquí, puede en contacto con nosotros en [GitHub](https://github.com/Microsoft/ApplicationInsights-Home/issues).
