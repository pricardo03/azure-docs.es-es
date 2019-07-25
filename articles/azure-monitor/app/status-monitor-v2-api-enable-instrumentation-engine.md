---
title: 'Referencia de la API del Monitor de estado de Azure v2: Habilitación del motor de instrumentación | Microsoft Docs'
description: Referencia de la API del Monitor de estado de Azure v2. Enable-InstrumentationEngine. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: 1e30490dbd51f541afd0b7036769cfc638a75877
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66514379"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v021-alpha"></a>API del Monitor de estado v2: Enable-InstrumentationEngine (v0.2.1-alpha)

Este artículo describe un cmdlet que forma parte del [módulo Az.ApplicationMonitor de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Monitor de estado v2 está actualmente en la versión preliminar pública.
> Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIPCIÓN

Habilita el motor de instrumentación mediante la definición de algunas claves del Registro.
Reinicie IIS para aplicar los cambios.

El motor de instrumentación puede complementar los datos recopilados por los SDK de .NET.
Recopila los eventos y mensajes que describen la ejecución de un proceso administrado. Estos eventos y mensajes incluyen códigos de resultado de la dependencia, verbos HTTP y texto de comando SQL.

Habilite el motor de instrumentación si:
- Ya ha habilitado la supervisión con el cmdlet Enable pero no ha habilitado el motor de instrumentación.
- Ha instrumentado la aplicación manualmente con los SDK de .NET y desea recopilar datos de telemetría adicionales.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.

> [!NOTE] 
> - Este cmdlet requiere que revise y acepte la licencia, y la declaración de privacidad.
> - El motor de instrumentación produce más sobrecarga y está desactivado de forma predeterminada.

## <a name="examples"></a>Ejemplos

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parámetros

### <a name="-acceptlicense"></a>-AcceptLicense
**Opcional.** Use este modificador para aceptar la licencia y la declaración de privacidad en las instalaciones sin periféricos.

### <a name="-verbose"></a>-Verbose
**Parámetro común.** Use este modificador para generar registros detallados.

## <a name="output"></a>Salida


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>Salida de ejemplo al habilitar correctamente el motor de instrumentación

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="next-steps"></a>Pasos siguientes

  Vea la telemetría:
 - [Explore las métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y el uso.
- [Busque en los eventos y los registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
- Use [análisis](../../azure-monitor/app/analytics.md) para consultas más avanzadas.
- [Cree paneles](../../azure-monitor/app/overview-dashboard.md).
 
 Agregue más telemetría:
 - [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar llamadas de seguimiento y registro.
 
 Hacer más con el Monitor de estado v2:
 - Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) del Monitor de estado v2.
 - [Obtenga la configuración](status-monitor-v2-api-get-config.md) para confirmar que sus opciones se registraron correctamente.
 - [Obtenga el estado](status-monitor-v2-api-get-status.md) para inspeccionar la supervisión.
