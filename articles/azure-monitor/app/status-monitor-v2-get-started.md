---
title: Introducción al Monitor de estado de Azure v2 | Microsoft Docs
description: Una guía de inicio rápido del Monitor de estado v2. Supervise el rendimiento de los sitios web sin volver a implementarlos. Funciona con las aplicaciones web de ASP.NET hospedadas en local, en las máquinas virtuales o en Azure.
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
ms.openlocfilehash: 4da9d8e8efd5f70718f18b2e8e35ea6b5adf6757
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66734974"
---
# <a name="get-started-with-status-monitor-v2"></a>Introducción al Monitor de estado v2

Este artículo contiene los comandos de inicio rápido que se espera que funcionen para la mayoría de los entornos.
Las instrucciones dependen de la Galería de PowerShell para distribuir las actualizaciones.
Estos comandos admiten el parámetro de PowerShell `-Proxy`.

Para obtener una explicación de estos comandos, las instrucciones de personalización e información sobre cómo solucionar problemas, consulte las [instrucciones detalladas](status-monitor-v2-detailed-instructions.md).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

> [!IMPORTANT]
> Monitor de estado v2 está actualmente en la versión preliminar pública.
> Esta versión preliminar se ofrece sin un Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas.
> Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="download-and-install-via-powershell-gallery"></a>Descarga e instalación a través de la Galería de PowerShell

### <a name="install-prerequisites"></a>Requisitos previos de instalación
Ejecute PowerShell como administrador.
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-PackageProvider -Name NuGet -MinimumVersion 2.8.5.201 -Force
Set-PSRepository -Name "PSGallery" -InstallationPolicy Trusted
Install-Module -Name PowerShellGet -Force
``` 
Cierre PowerShell.

### <a name="install-status-monitor-v2"></a>Instalación del Monitor de estado v2
Ejecute PowerShell como administrador.
```powershell   
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Install-Module -Name Az.ApplicationMonitor -AllowPrerelease -AcceptLicense
``` 

### <a name="enable-monitoring"></a>Habilitar supervisión
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope Process -Force
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```
    
        
## <a name="download-and-install-manually-offline-option"></a>Descarga e instalación manual (opción sin conexión)
### <a name="download-the-module"></a>Descara del módulo
Descargue manualmente la versión más reciente del módulo desde la [Galería de PowerShell](https://www.powershellgallery.com/packages/Az.ApplicationMonitor).

### <a name="unzip-and-install-status-monitor-v2"></a>Descomprimir e instalar el Monitor de estado v2
```powershell
$pathToNupkg = "C:\Users\t\Desktop\Az.ApplicationMonitor.0.3.0-alpha.nupkg"
$pathToZip = ([io.path]::ChangeExtension($pathToNupkg, "zip"))
$pathToNupkg | rename-item -newname $pathToZip
$pathInstalledModule = "$Env:ProgramFiles\WindowsPowerShell\Modules\Az.ApplicationMonitor"
Expand-Archive -LiteralPath $pathToZip -DestinationPath $pathInstalledModule
```
### <a name="enable-monitoring"></a>Habilitar supervisión
```powershell
Enable-ApplicationInsightsMonitoring -InstrumentationKey xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
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

- Revise las [instrucciones detalladas](status-monitor-v2-detailed-instructions.md) para obtener una explicación de los comandos que se encuentran aquí.
- Use nuestra guía para [solucionar problemas](status-monitor-v2-troubleshoot.md) del Monitor de estado v2.
