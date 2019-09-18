---
layout: LandingPage
description: Aprenda a solucionar problemas en implementaciones de máquinas virtuales.
title: Documentación sobre solución de problemas en Azure Virtual Machines | Microsoft Docs
services: virtual-machines
author: genlin
manager: dcscontentpm
ms.assetid: ''
ms.service: virtual-machines
ms.tgt_pltfrm: na
ms.topic: landing-page
ms.date: 10/3/2018
ms.author: genli
ms.openlocfilehash: d7ceb3acb1d2e3d174f3b665ec6210d3ddac9970
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71059157"
---
# <a name="troubleshooting-azure-virtual-machines"></a>Solución de problemas de máquinas virtuales de Azure

## <a name="tools-for-troubleshooting"></a>Herramientas de solución de problemas

- [Consola serie](serial-console-windows.md)
- [Diagnósticos de arranque](boot-diagnostics.md)
- [Máquina virtual Windows: Conexión del disco del sistema operativo con otra máquina virtual para solucionar problemas](troubleshoot-recovery-disks-portal-windows.md)
- [Máquina virtual Linux: Conexión del disco del sistema operativo con otra máquina virtual para solucionar problemas](troubleshoot-recovery-disks-portal-linux.md)

## <a name="cant-connect-to-the-vm"></a>No puedo conectarme a la máquina virtual

### <a name="windows"></a>Windows

**Solución habitual**

- [Restablecimiento de RDP](reset-rdp.md)
- [Solución de problemas de RDP](troubleshoot-rdp-connection.md)
- [Solución de problemas detallada de RDP](detailed-troubleshoot-rdp.md)

**Errores de RDP**

- [No hay servidor de licencias](troubleshoot-rdp-no-license-server.md)
- [Un ](Troubleshoot-rdp-internal-error.md) interno
- [errores de autenticación](troubleshoot-authentication-error-rdp-vm.md)
- [Solución de errores específicos](troubleshoot-specific-rdp-errors.md)

**Errores al arrancar la máquina virtual**

* [Errores de arranque de BitLocker](troubleshoot-bitlocker-boot-error.md) 
* [Windows muestra "Comprobando el sistema de archivos" durante el arranque](troubleshoot-check-disk-boot-error.md)
* [Errores de pantalla azul](troubleshoot-common-blue-screen-error.md)
* [El inicio de la máquina virtual está detenido en "Preparando Windows](troubleshoot-vm-boot-configure-update.md)
* [Pantalla azul con el mensaje "ERROR CRÍTICO DEL SERVICIO"](troubleshoot-critical-service-failed-boot-error.md)
* [Problema de bucle de reinicio](troubleshoot-reboot-loop.md)
* [El inicio de la máquina virtual está atascado en una fase de actualización de Windows](troubleshoot-stuck-updating-boot-error.md)
* [La máquina virtual arranca en modo seguro](troubleshoot-rdp-safe-mode.md)

**Otros**
- [Restablezca la contraseña de máquina virtual de la máquina virtual Windows sin conexión](reset-local-password-without-agent.md)
- [Restablezca la NIC después de una configuración incompleta](reset-network-interface.md)

### <a name="linux"></a>Linux

- [Solución de problemas de SSH](troubleshoot-ssh-connection.md)
- [Solución de problemas detallada de SSH](detailed-troubleshoot-ssh-connection.md)
- [Mensajes comunes de error](error-messages.md)
- [Restablezca la contraseña de máquina virtual de la máquina virtual Linux sin conexión](reset-password.md)

## <a name="vm-deployment-issues"></a>Problemas de implementación de máquinas virtuales

- [Errores de asignación](allocation-failure.md)
- Reimplementación de una máquina virtual
    - [Linux](redeploy-to-new-node-linux.md)
    - [Windows](redeploy-to-new-node-windows.md)
- Solución de problemas de implementaciones
    - [Linux](troubleshoot-deploy-vm-linux.md)
    - [Windows](troubleshoot-deploy-vm-windows.md)
- [Se cambian los nombres de dispositivos](troubleshoot-device-names-problems.md)
- [Instalación del agente de máquina virtual Windows sin conexión](install-vm-agent-offline.md)
- [Reinicio o cambio de tamaño de una máquina virtual](restart-resize-error-troubleshooting.md)

## <a name="vm-performance-issue"></a>Problema de rendimiento de la máquina virtual
- [Problemas de rendimiento con máquinas virtuales](performance-diagnostics.md)
- Windows
    - [Uso de PerfInsights](how-to-use-perfinsights.md)
    - [Extensión de diagnóstico de rendimiento](performance-diagnostics-vm-extension.md)
- Linux
    - [Uso de PerfInsights](how-to-use-perfinsights-linux.md)