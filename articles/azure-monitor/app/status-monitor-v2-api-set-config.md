---
title: 'Referencia de API de estado de Azure Monitor v2: Establecer la configuración | Microsoft Docs'
description: Conjunto de referencia de API-ApplicationInsightsMonitoringConfig de estado Monitor v2. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: 2ca738d5d79fc73f892922825d4b731e8ee92b72
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870498"
---
# <a name="status-monitor-v2-api-set-applicationinsightsmonitoringconfig-v021-alpha"></a>API de v2 del Monitor de estado: Set-ApplicationInsightsMonitoringConfig (v0.2.1-alpha)

Este documento describe un cmdlet que se distribuye como un miembro de la [módulo de Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para obtener más información, consulte [términos de uso complementarios para vistas previas de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>DESCRIPCIÓN

Establecer el archivo de configuración sin repetir una reinstalación completa. Reinicie IIS para que los cambios surtan efecto.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.


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


### <a name="-verbose"></a>-Verbose
**Parámetro común.** Use este modificador para enviar registros detallados.


## <a name="output"></a>Salida

Ninguna salida predeterminada.

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
