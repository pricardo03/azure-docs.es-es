---
title: Los servicios de escritorio remoto no se inician en la máquina virtual de Azure | Microsoft Docs
description: Obtenga información sobre cómo solucionar problemas con los Servicios de Escritorio remoto al conectarse a una máquina virtual | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: 39b793e2722766f3f28829b4dc48534054abd97e
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49989062"
---
# <a name="remote-desktop-services-is-not-starting-on-an-azure-vm"></a>Los Servicios de Escritorio remoto no se inician en la máquina virtual de Azure

En este artículo se describe cómo solucionar problemas de conexión a una máquina virtual de Azure (VM) cuando los Servicios de escritorio remoto (TermService) no se inician o devuelven un error.

>[!NOTE]
>Azure tiene dos modelos de implementación diferentes para crear y trabajar con recursos: [el Administrador de recursos y el clásico](../../azure-resource-manager/resource-manager-deployment-model.md). En este artículo se describe el uso del modelo de implementación del Administrador de recursos. Se recomienda usar este modelo para las nuevas implementaciones, en lugar de usar el modelo de implementación clásica.

## <a name="symptoms"></a>Síntomas

Cuando intenta conectarse a una máquina virtual, se encuentra con los siguientes escenarios:

- En la captura de pantalla de la máquina virtual se muestra que el sistema operativo está totalmente cargado y esperando las credenciales.
- Todas las aplicaciones en la máquina virtual son accesibles y funcionan según lo esperado.
- La máquina virtual está respondiendo a la conectividad TCP en el puerto del Protocolo de escritorio remoto de Microsoft (RDP) (predeterminado: 3389).
- No se le piden las credenciales cuando intenta realizar una conexión RDP.

## <a name="cause"></a>Causa

Este problema se produce porque los Servicios de Escritorio remoto no están ejecutándose en la máquina virtual. La causa dependerá de los siguientes escenarios:

- El servicio TermService se ha establecido en **Deshabilitado**.
- El servicio TermService se bloquea.

## <a name="solution"></a>Solución

Para resolver este problema, use una de las soluciones siguientes o pruébelas una a una:

### <a name="solution-1-using-the-serial-console"></a>Solución 1: Uso de la consola serie

1. Acceda a [Serial Console](serial-console-windows.md) seleccionando **Soporte técnico y solución de problemas** > **Serial Console (versión preliminar)**. Si la característica está habilitada en la máquina virtual, puede conectar la máquina virtual correctamente.

2. Creación de un nuevo canal para una instancia CMD. Escriba **CMD** para iniciar el canal con el fin de obtener el nombre del canal.

3. Cambie al canal que ejecuta la instancia CMD, en este caso debe ser el canal 1.

   ```
   ch -si 1
   ```

4. Presione **Entrar** otra vez y escriba un nombre de usuario y una contraseña válidos (id. de dominio o local) para la máquina virtual.

5. Consulte el estado del servicio TermService.

   ```
   sc query TermService
   ```

6. Si el estado del servicio se muestra como **Detenido**, intente iniciar el servicio.

   ```
   sc start TermService
   ```

7. Envíe una consulta el servicio de nuevo para asegurarse de que el servicio se ha iniciado correctamente.

   ```
   sc query TermService
   ```

### <a name="solution-2-using-a-recovery-vm-to-enable-the-service"></a>Solución 2: Uso de una máquina virtual de recuperación para habilitar el servicio

[Realice una copia de seguridad del disco del sistema operativo](../windows/snapshot-copy-managed-disk.md) y [conecte el disco del sistema operativo a una máquina virtual de rescate](../windows/troubleshoot-recovery-disks-portal.md). A continuación, abra una instancia de CMD con privilegios elevados y ejecute los siguientes scripts en la máquina virtual de rescate:

>[!NOTE]
>Se supone que la letra de unidad que se asigna al disco del sistema operativo conectado es F. Reemplácela por el valor adecuado en la máquina virtual. Una vez hecho esto, desconecte el disco de la máquina virtual de recuperación y [vuelva a crear la máquina virtual](../windows/create-vm-specialized.md). Para solucionar otros problemas, puede usar la **Solución 1** porque se ha habilitado la consola serie.

```
reg load HKLM\BROKENSYSTEM f:\windows\system32\config\SYSTEM

REM Enable Serial Console
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} displaybootmenu yes
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} timeout 10
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /set {bootmgr} bootems yes
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /ems {<Boot Loader Identifier>} ON
bcdedit /store <Volume Letter Where The BCD Folder Is>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200

REM Get the current ControlSet from where the OS is booting
for /f "tokens=3" %x in ('REG QUERY HKLM\BROKENSYSTEM\Select /v Current') do set ControlSet=%x
set ControlSet=%ControlSet:~2,1%

REM Suggested configuration to enable OS Dump
set key=HKLM\BROKENSYSTEM\ControlSet00%ControlSet%\Control\CrashControl
REG ADD %key% /v CrashDumpEnabled /t REG_DWORD /d 2 /f
REG ADD %key% /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f
REG ADD %key% /v NMICrashDump /t REG_DWORD /d 1 /f

REM Set default values back on the broken service
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v ImagePath /t REG_EXPAND_SZ /d "<Image Path>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v ObjectName /t REG_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Process Name>" /v type /t REG_DWORD /d 16 /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v ImagePath /t REG_EXPAND_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v ObjectName /t REG_SZ /d "<Startup Account>" /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Process Name>" /v type /t REG_DWORD /d 16 /f

REM Enable default dependencies from the broken service
reg add "HKLM\BROKENSYSTEM\ControlSet001\services\<Driver/Service Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg add "HKLM\BROKENSYSTEM\ControlSet002\services\<Driver/Service Name>" /v start /t REG_DWORD /d <Startup Type> /f
reg unload HKLM\BROKENSYSTEM
```

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico

Si sigue necesitando ayuda, [póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.
