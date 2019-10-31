---
title: Configuración de Windows Update para trabajar con Azure Update Management
description: En este artículo se describen los parámetros de Windows Update que se configuran para trabajar con Azure Update Management.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 10/02/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 813d34f9c07e6c2909c483f040d4f3bf09b3ad24
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72690837"
---
# <a name="configure-windows-update-settings-for-update-management"></a>Configuración de los parámetros de Windows Update para Update Management

Azure Update Management se basa en Windows Update para descargar e instalar las actualizaciones de Windows. Como resultado, Update Management respeta muchas de las configuraciones que usa Windows Update. Si usa la configuración para habilitar las actualizaciones que no son de Windows, Update Management también administrará dichas actualizaciones. Si quiere habilitar la descarga de actualizaciones antes de que se lleve a cabo una implementación de actualizaciones, las implementaciones de actualizaciones pueden ejecutarse de manera más rápida y eficaz, con menos probabilidades de exceder la ventana de mantenimiento.

## <a name="pre-download-updates"></a>Actualizaciones previas a la descarga

Para configurar la descarga automática de actualizaciones en la directiva de grupo, establezca la opción [Configurar actualizaciones automáticas](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) en **3**. Esta configuración habilita las descargas de las actualizaciones necesarias en segundo plano, pero no las instala. De este modo, Update Management mantiene el control de las programaciones, pero las actualizaciones se pueden descargar fuera de la ventana de mantenimiento de Update Management. Este comportamiento evita errores de tipo "Ventana de mantenimiento superada" en Update Management.

También puede activar esta configuración mediante la ejecución del siguiente comando de PowerShell en un sistema que quiera configurar para la descarga automática de actualizaciones:

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

## <a name="disable-automatic-installation"></a>Deshabilitar la instalación automática

De forma predeterminada, en las máquinas virtuales (VM) de Azure, la instalación automática de actualizaciones está habilitada. Esto puede provocar que las actualizaciones se instalen antes de que programe su instalación por medio de Update Management. Puede deshabilitar este comportamiento estableciendo la clave del Registro `NoAutoUpdate` en `1`. El siguiente fragmento de código de PowerShell muestra una forma de hacerlo:

```powershell
$AutoUpdatePath = "HKLM:SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-ItemProperty -Path $AutoUpdatePath -Name NoAutoUpdate -Value 1
```

## <a name="configure-reboot-settings"></a>Configuración de las opciones de reinicio

Las claves del Registro que se enumeran en [Configuración de actualizaciones automáticas mediante la edición del Registro](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry) y [Claves del Registro usadas para administrar reinicios](/windows/deployment/update/waas-restart#registry-keys-used-to-manage-restart) pueden hacer que las máquinas se reinicien, incluso si ha especificado **No reiniciar nunca** en la configuración de **Implementación de actualizaciones**. Debe configurar estas claves del Registro como desee para su entorno.

## <a name="enable-updates-for-other-microsoft-products"></a>Habilitar actualizaciones de otros productos de Microsoft

De forma predeterminada, Windows Update solo proporciona actualizaciones para Windows. Si habilita la opción **Ofrecerme actualizaciones para otros productos de Microsoft cuando actualice Windows**, también recibirá actualizaciones para otros productos, incluidas las revisiones de seguridad para Microsoft SQL Server u otro software de Microsoft. Esta opción no se puede configurar mediante la directiva de grupo. Ejecute el siguiente comando de PowerShell en los sistemas en los que quiere habilitar otras actualizaciones de Microsoft. Update Management se ajustará a esta configuración.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="wsus-configuration-settings"></a>Valores de configuración de WSUS

Update Management cumple con la configuración de Windows Server Update Services (WSUS). A continuación se muestra la lista de opciones de WSUS que puede configurar para trabajar con Update Management.

### <a name="intranet-microsoft-update-service-location"></a>Ubicación del servicio Microsoft Update en la intranet

Puede especificar orígenes para examinar y descargar actualizaciones en [Ubicación del servicio Microsoft Update en la intranet](/windows/deployment/update/waas-wu-settings#specify-intranet-microsoft-update-service-location).

## <a name="next-steps"></a>Pasos siguientes

Después de configurar los parámetros de Windows Update, puede programar una implementación de actualizaciones siguiendo las instrucciones de [Administración de actualizaciones y revisiones para las máquinas virtuales de Azure](automation-tutorial-update-management.md).