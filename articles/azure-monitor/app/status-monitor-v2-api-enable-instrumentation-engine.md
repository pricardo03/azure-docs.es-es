---
title: 'Referencia de API de estado de Azure Monitor v2: Habilitar el motor de instrumentación | Microsoft Docs'
description: Habilitación de referencia de API-InstrumentationEngine de estado Monitor v2. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: e993378634262de25449975431c0a9e3145ca9fb
ms.sourcegitcommit: 25a60179840b30706429c397991157f27de9e886
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/28/2019
ms.locfileid: "66255246"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v021-alpha"></a>API de v2 del Monitor de estado: Enable-InstrumentationEngine (v0.2.1-alpha)

Este documento describe un cmdlet que se distribuye como un miembro de la [módulo de Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para obtener más información, consulte [términos de uso complementarios para vistas previas de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>DESCRIPCIÓN

Este cmdlet permitirá el motor de instrumentación estableciendo algunas claves del registro.
Reinicie IIS para que estos cambios surtan efecto.

El motor de instrumentación puede complementar los datos recopilados por el SDK. NET.
Eventos y mensajes se recopilarán que describen la ejecución de un proceso administrado. Incluyendo pero sin limitarse a códigos de resultado de la dependencia, verbos HTTP y texto de comando SQL. 

Permitir que el motor de instrumentación si:
- Ya se ha habilitado la supervisión mediante el cmdlet Enable pero no ha habilitado la InstrumentationEngine.
- Manual se ha instrumentado la aplicación con el SDK de .NET y desea recopilar datos de telemetría adicionales.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.

> [!NOTE] 
> Este cmdlet, deberá revisar y aceptar la licencia y declaración de privacidad.

> [!NOTE] 
> El motor de instrumentación agrega sobrecarga adicional y está desactivada de forma predeterminada.

## <a name="examples"></a>Ejemplos

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parámetros 

### <a name="-acceptlicense"></a>-AcceptLicense
**Opcional.** Use este modificador para aceptar la licencia y declaración de privacidad en las instalaciones sin periféricos.

### <a name="-verbose"></a>-Verbose
**Parámetro común.** Use este modificador para enviar registros detallados.

## <a name="output"></a>Salida


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Ejemplo de salida de habilitar correctamente el motor de instrumentación

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
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
