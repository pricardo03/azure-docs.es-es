---
title: Configuración de Windows Update para trabajar con Azure Update Management
description: En este artículo se describe los parámetros de Windows Update que se configuran para trabajar con Update Management.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 10/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f50ca9515f12e8c9b5943904c4d0226f2ca3353c
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72377393"
---
# <a name="configure-windows-update-settings-for-update-management"></a>Configuración de los parámetros de Windows Update para Update Management

Update Management se basa en Windows Update para descargar e instalar las actualizaciones de Windows. Como resultado, se respetan muchas de las configuraciones usadas por Windows Update. Si usa la configuración para habilitar las actualizaciones que no son de Windows, Update Management también administrará dichas actualizaciones. Si desea habilitar la descarga de actualizaciones antes de que se lleve a cabo la implementación de la actualización, las implementaciones de las actualizaciones pueden ejecutarse más rápido con menos probabilidades de exceder la ventana de mantenimiento.

## <a name="pre-download-updates"></a>Actualizaciones de descarga previa

Para configurar la descarga automática de actualizaciones en la directiva de grupo, puede establecer la opción [Configurar actualizaciones automáticas](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) en **3**. De esta forma se descargan las actualizaciones necesarias en segundo plano, pero no se instalan. Esto permite que Update Management controle las programaciones, pero permite que las actualizaciones se descarguen fuera de la ventana de mantenimiento de Update Management. Esto puede evitar errores **Ventana de mantenimiento superada** en Update Management.

También puede configurar esto con PowerShell, con la ejecución del siguiente comando de PowerShell en un sistema en el que desee descargar las actualizaciones automáticamente.

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="disable-automatic-installation"></a>Deshabilitar la instalación automática

Las máquinas virtuales de Azure tienen la instalación automática habilitada de forma predeterminada. Esto puede provocar que las actualizaciones se instalen antes de que programe su instalación por medio de Update Management. Puede deshabilitar este comportamiento estableciendo la clave del Registro `NoAutoUpdate` en `1`. El siguiente fragmento de código de PowerShell muestra una forma de hacerlo.

```powershell
$AutoUpdatePath = "HKLM:SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-ItemProperty -Path $AutoUpdatePath -Name NoAutoUpdate -Value 1
```

## <a name="configure-reboot-settings"></a>Configuración de las opciones de reinicio

Las claves del Registro que se enumeran en [Configuración de actualizaciones automáticas mediante la edición del Registro](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-rej7uijui7jgistry) y [Claves del Registro usadas para administrar reinicios](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) pueden hacer que las máquinas se reinicien, incluso si ha especificado **No reiniciar nunca** en la configuración de Implementación de actualizaciones. Debe configurar estas claves del Registro como desee para su entorno.

## <a name="enable-updates-for-other-microsoft-products"></a>Habilitar actualizaciones de otros productos de Microsoft

De forma predeterminada, Windows Update solo proporciona actualizaciones para Windows. Si habilita **Ofrecerme actualizaciones para otros productos de Microsoft cuando actualice Windows**, se proporcionan las actualizaciones para otros productos, incluidas las revisiones de seguridad de SQL Server u otro software propio. Esta opción no se puede configurar mediante la directiva de grupo. Ejecute el siguiente comando de PowerShell en los sistemas en los que desea habilitar otras revisiones propias, y Update Management respetará esta configuración.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="wsus-configuration-settings"></a>Valores de configuración de WSUS

**Update Management** respeta la configuración de WSUS. A continuación se muestra la lista de opciones de WSUS que puede configurar para trabajar con Update Management.

### <a name="intranet-microsoft-update-service-location"></a>Ubicación del servicio Microsoft Update en la intranet

Puede especificar orígenes para examinar y descargar actualizaciones en la [ubicación del servicio Microsoft Update en la intranet](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location).

## <a name="next-steps"></a>Pasos siguientes

Después de configurar los parámetros de Windows Update, puede programar una implementación de actualizaciones siguiendo los pasos descritos en [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).