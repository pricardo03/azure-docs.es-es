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
ms.openlocfilehash: 7d337c9b6b22f3abfcb4aea1c47127706ed9e9d7
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870513"
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
