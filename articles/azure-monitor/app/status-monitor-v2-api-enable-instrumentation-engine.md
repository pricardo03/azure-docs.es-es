---
title: 'Referencia de API v2 de Azure Monitor de estado: Habilitar el motor de instrumentación | Microsoft Docs'
description: Referencia de API v2 de Monitor de estado. Enable-InstrumentationEngine. Supervisar el rendimiento del sitio Web sin volver a implementar el sitio Web. Funciona con las aplicaciones web ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.sourcegitcommit: adb6c981eba06f3b258b697251d7f87489a5da33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66514379"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v021-alpha"></a>API de v2 del Monitor de estado: Enable-InstrumentationEngine (v0.2.1-alpha)

Este artículo describe un cmdlet que forme parte del [módulo de Az.ApplicationMonitor PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> V2 de Monitor de estado está actualmente en versión preliminar pública.
> Esta versión preliminar se proporciona sin un contrato de nivel de servicio y no es aconsejable para cargas de trabajo de producción. Podrían no admitir algunas características, y algunas pueden tener funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="description"></a>DESCRIPCIÓN

Permite que el motor de instrumentación estableciendo algunas claves del registro.
Reinicie IIS para que los cambios surtan efecto.

El motor de instrumentación puede complementar los datos recopilados por el SDK. NET.
Recopila los eventos y mensajes que describen la ejecución de un proceso administrado. Estos eventos y mensajes incluyen códigos de resultado de la dependencia, verbos HTTP y texto de comando SQL.

Permitir que el motor de instrumentación si:
- Ya se ha habilitado la supervisión con el cmdlet Enable pero no ha habilitado el motor de instrumentación.
- Manualmente se ha instrumentado la aplicación con el SDK de .NET y desea recopilar datos de telemetría adicionales.

> [!IMPORTANT] 
> Este cmdlet requiere una sesión de PowerShell con permisos de administrador.

> [!NOTE] 
> - Este cmdlet requiere que revise y acepte la licencia y declaración de privacidad.
> - El motor de instrumentación agrega sobrecarga adicional y está desactivada de forma predeterminada.

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
 - [Explorar métricas](../../azure-monitor/app/metrics-explorer.md) para supervisar el rendimiento y uso.
- [Busque eventos y registros](../../azure-monitor/app/diagnostic-search.md) para diagnosticar problemas.
- Use [analytics](../../azure-monitor/app/analytics.md) para obtener más consultas avanzadas.
- [Crear paneles](../../azure-monitor/app/overview-dashboard.md).
 
 Agregue más telemetría:
 - [Cree pruebas web](monitor-web-app-availability.md) para asegurarse de que el sitio permanece activo.
- [Agregue telemetría de cliente web](../../azure-monitor/app/javascript.md) para ver las excepciones de código de la página web y para habilitar las llamadas de seguimiento.
- [Agregue el SDK de Application Insights al código](../../azure-monitor/app/asp-net.md) para que pueda insertar seguimiento y registrar las llamadas.
 
 Hacer más con el Monitor de estado v2:
 - Use nuestra guía para [solucionar](status-monitor-v2-troubleshoot.md) v2 del Monitor de estado.
 - [Obtener la configuración](status-monitor-v2-api-get-config.md) para confirmar que la configuración se registraron correctamente.
 - [Obtener el estado](status-monitor-v2-api-get-status.md) para inspeccionar la supervisión.
