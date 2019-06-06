---
title: 'Referencia de API v2 de Azure Monitor de estado: Establecer la configuración | Microsoft Docs'
description: Referencia de API v2 de Monitor de estado. Set-ApplicationInsightsMonitoringConfig. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: 562ce8a4267370be9b049e3b56f213f82deb89c0
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734985"
---
# <a name="status-monitor-v2-api-set-applicationinsightsmonitoringconfig-v021-alpha"></a>API de v2 del Monitor de estado: Set-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

Este documento describe un cmdlet que forme parte del [módulo de Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no es aconsejable para cargas de trabajo de producción. Podrían no admitir algunas características, y algunas pueden tener funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIPCIÓN

Establece el archivo de configuración sin tener que realizar una reinstalación completa.
Reinicie IIS para que los cambios surtan efecto.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.


## <a name="examples"></a>Ejemplos

### <a name="example-with-a-single-instrumentation-key"></a>Ejemplo con una única clave de instrumentación
En este ejemplo, todas las aplicaciones en el equipo actual se asignará una única clave de instrumentación.

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
    - 'ComputerName' coincidirá con solo los equipos con el nombre especificado.
- **AppFilter** es obligatoria C# regex del equipo o el nombre de la máquina virtual.
    - ". *" coincide con todos
    - 'ApplicationName' hará coincidir solo las aplicaciones IIS con el nombre especificado.
- **InstrumentationKey** se requiere para habilitar la supervisión de las aplicaciones que coinciden con los dos filtros anteriores.
    - Deje este valor null si desea definir reglas para excluir de la supervisión.


### <a name="-verbose"></a>-Verbose
**Parámetro común.** Utilice este modificador para mostrar los registros detallados.


## <a name="output"></a>Salida

De forma predeterminada, ninguna salida.

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkey"></a>Ejemplo de salida desde la configuración en el archivo de configuración a través de la clave de instrumentación - verbose

```
VERBOSE: Operation: InstallWithIkey
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
```

#### <a name="example-verbose-output-from-setting-the-config-file-via--instrumentationkeymap"></a>Ejemplo de salida desde la configuración en el archivo de configuración a través de InstrumentationKeyMap - verbose

```
VERBOSE: Operation: InstallWithIkeyMap
VERBOSE: InstrumentationKeyMap parsed:
Filters:
0)InstrumentationKey:  AppFilter: WebAppExclude MachineFilter: .*
1)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx2 AppFilter: WebAppTwo MachineFilter: .*
2)InstrumentationKey: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxdefault AppFilter: .* MachineFilter: .*
VERBOSE: set config file
VERBOSE: Config File Path:
C:\Program Files\WindowsPowerShell\Modules\Az.ApplicationMonitor\content\applicationInsights.ikey.config
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
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar seguimiento y registrar las llamadas
 
 Hacer más con el Monitor de estado v2:
 - Use nuestra guía para [solucionar](status-monitor-v2-troubleshoot.md) v2 del Monitor de estado.
 - [Obtener la configuración](status-monitor-v2-api-get-config.md) para confirmar que la configuración se registraron correctamente.
 - [Obtener el estado](status-monitor-v2-api-get-status.md) para inspeccionar la supervisión.
