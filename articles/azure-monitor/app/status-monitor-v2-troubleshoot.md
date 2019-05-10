---
title: Problemas conocidos y solución de Azure Monitor de estado v2 | Microsoft Docs
description: Los problemas conocidos de Monitor de estado v2 y ejemplos de solución de problemas. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: d640206fd72b4eb89afe5ed1750627823bca9637
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65415871"
---
# <a name="troubleshooting-status-monitor-v2"></a>Solución de problemas de estado supervisar v2

Cuando se habilita la supervisión, puede experimentar problemas que impiden la recopilación de datos. Este documento enumeran todos los problemas conocidos y solución de problemas de ejemplos.
Si encuentra un problema no aparece aquí, es posible que en contacto con nosotros [aquí](https://github.com/Microsoft/ApplicationInsights-Home/issues).


> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para obtener más información, consulte [términos de uso complementarios para vistas previas de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="known-issues"></a>Problemas conocidos

### <a name="conflicting-dlls-in-an-applications-bin-directory"></a>Archivos DLL en conflicto en el directorio bin de la aplicación

Si cualquiera de estas DLL están presente en el directorio bin, puede fallar la supervisión.

- Microsoft.ApplicationInsights.dll
- Microsoft.AspNet.TelemetryCorrelation.dll
- System.Diagnostics.DiagnosticSource.dll

Algunos de estos archivos DLL se incluyen en las plantillas de aplicación de Visual Studio de forma predeterminada, incluso si la aplicación no los use.
Comportamiento síntoma se puede ver mediante las herramientas de solución de problemas:

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

- iisreset + aplicación cargue (TELEMETRÍA NO). Investigue con Sysinternals (Handle.exe y ListDLLs.exe)
    ```
    .\handle64.exe -p w3wp | findstr /I "InstrumentationEngine AI. ApplicationInsights"
    E54: File  (R-D)   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Runtime\Microsoft.ApplicationInsights.RedfieldIISModule.dll

    .\Listdlls64.exe w3wp | findstr /I "InstrumentationEngine AI ApplicationInsights"
    0x0000000009be0000  0x127000  C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\MicrosoftInstrumentationEngine_x64.dll
    0x0000000009b90000  0x4f000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.ExtensionsHost_x64.dll
    0x0000000004d20000  0xb2000   C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\Instrumentation64\Microsoft.ApplicationInsights.Extensions.Base_x64.dll
    ```

### <a name="conflict-with-iis-shared-configuration"></a>Conflicto con la configuración compartida de IIS

Si tiene un clúster de servidores web, puede que esté usando un [configuración compartida](https://docs.microsoft.com/iis/web-hosting/configuring-servers-in-the-windows-web-platform/shared-configuration_211). Automáticamente, no podemos inyectar nuestro HttpModule en esta configuración compartida. Cada servidor web en primer lugar debe ejecutar el comando Enable para instalar la DLL en la GAC.

Después de ejecutar el comando Enable, 
1. Vaya a su directorio de configuración compartida y encontrar su `applicationHost.config` archivo.
2. Agregue esta línea a la configuración de la sección de módulos:
    ```
    <modules>
        <!-- Registered global managed http module handler. The 'Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.dll' must be installed in the GAC before this config is applied. -->
        <add name="ManagedHttpModuleHelper" type="Microsoft.AppInsights.IIS.ManagedHttpModuleHelper.ManagedHttpModuleHelper, Microsoft.AppInsights.IIS.ManagedHttpModuleHelper, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35" preCondition="managedHandler,runtimeVersionv4.0" />
    </modules>
    ```
    


## <a name="troubleshooting"></a>solución de problemas
    
### <a name="troubleshooting-powershell"></a>Solución de problemas de PowerShell

#### <a name="how-to-inspect-what-modules-are-available"></a>¿Para inspeccionar qué módulos están disponibles?
Puede auditar los módulos instalados mediante el comando: `Get-Module -ListAvailable`

#### <a name="how-to-import-a-module-into-the-current-session"></a>¿Cómo importar un módulo en la sesión actual?
Si no se ha cargado el módulo en una sesión de PowerShell, puede cargar manualmente mediante el comando: `Import-Module <path to psd1>`


### <a name="troubleshooting-the-status-monitor-v2-module"></a>El módulo de v2 del Monitor de estado de la solución de problemas

#### <a name="how-to-review-what-commands-are-available-in-the-status-monitor-v2-module"></a>¿Cómo revisar qué comandos están disponibles en el módulo de supervisión de estado v2?
- Ejecute el comando: `Get-Command -Module Az.ApplicationMonitor` para obtener los comandos disponibles:

    ```
    CommandType     Name                                               Version    Source
    -----------     ----                                               -------    ------
    Cmdlet          Disable-ApplicationInsightsMonitoring              0.2.1      Az.ApplicationMonitor
    Cmdlet          Disable-InstrumentationEngine                      0.2.1      Az.ApplicationMonitor
    Cmdlet          Enable-ApplicationInsightsMonitoring               0.2.1      Az.ApplicationMonitor
    Cmdlet          Enable-InstrumentationEngine                       0.2.1      Az.ApplicationMonitor
    Cmdlet          Get-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
    Cmdlet          Get-ApplicationInsightsMonitoringStatus            0.2.1      Az.ApplicationMonitor
    Cmdlet          Set-ApplicationInsightsMonitoringConfig            0.2.1      Az.ApplicationMonitor
    ```

#### <a name="what-is-the-current-version-of-the-status-monitor-v2-module"></a>¿Qué es la versión actual del módulo de supervisión de estado v2?
- Ejecute el comando: `Get-ApplicationInsightsMonitoringStatus` para obtener un resultado de la información sobre este módulo:
    - Versión del módulo de PowerShell
    - Versión de Application Insights SDK
    - Rutas de acceso de archivo del módulo de PowerShell
    
Revise nuestra [referencia de API](status-monitor-v2-api-get-status.md) para obtener una descripción detallada de cómo usar este cmdlet.



### <a name="troubleshooting-running-processes"></a>Solución de problemas de los procesos en ejecución

Puede inspeccionar el proceso en el equipo para ver si se cargan todos los archivos DLL instrumentado.
Si funciona la supervisión, se debe cargar al menos 12 dll.

- Cmd: `Get-ApplicationInsightsMonitoringStatus -InspectProcess`

Revise nuestra [referencia de API](status-monitor-v2-api-get-status.md) para obtener una descripción detallada de cómo usar este cmdlet.


### <a name="collect-etw-logs-with-perfview"></a>Recopilación de registros ETW con PerfView

#### <a name="setup"></a>Configuración

1. Descargar PerfView.exe y PerfView64.exe desde https://github.com/Microsoft/perfview/releases
2. Launch PerfView64.exe
3. Expanda "Opciones avanzadas"
4. Desactive la opción:
    - Zip
    - Combinar
    - Colección de símbolos de .NET
5. Conjunto de proveedores adicionales: `61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,925fa42b-9ef6-5fa7-10b8-56449d7a2040,f7d60e07-e910-5aca-bdd2-9de45b46c560,7c739bb9-7861-412e-ba50-bf30d95eae36,61f6ca3b-4b5f-5602-fa60-759a2a2d1fbd,323adc25-e39b-5c87-8658-2c1af1a92dc5,252e28f4-43f9-5771-197a-e8c7e750a984`


#### <a name="collecting-logs"></a>Recopilación de registros en curso

1. En una consola cmd con privilegios de administrador, ejecute `iisreset /stop` para desactivar todas las aplicaciones web y de IIS.
2. En PerfView, haga clic en "Iniciar colección"
3. En una consola cmd con privilegios de administrador, ejecute `iisreset /start` para iniciar IIS.
4. Intente ir a la aplicación.
5. Después de la aplicación termine de cargarse, vuelva a PerfView y haga clic en "Detener colección"



## <a name="next-steps"></a>Pasos siguientes

- Revise nuestra [referencia de API](status-monitor-v2-overview.md#powershell-api-reference) para encontrar un parámetro podría haber pasado por alto.
- Si encuentra un problema no aparece aquí, es posible que en contacto con nosotros [aquí](https://github.com/Microsoft/ApplicationInsights-Home/issues).
