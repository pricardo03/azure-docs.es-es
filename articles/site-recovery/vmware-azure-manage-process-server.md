---
title: Administrar un servidor de procesos utilizado para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: Este artículo se describe la administración de un servidor de procesos configurado para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure con Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/28/2019
ms.author: ramamill
ms.openlocfilehash: 2c27779719c73adf4d7fc1a61a0c77d03df71815
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925596"
---
# <a name="manage-process-servers"></a>Administración de servidores de proceso

En este artículo se describe tareas comunes para administrar el servidor de procesos de Site Recovery.

El servidor de procesos se usa para recibir, optimizar y enviar datos de replicación en Azure. También realiza una instalación de inserción de Mobility service en máquinas virtuales de VMware y servidores físicos que desea replicar, y realiza la detección automática de máquinas locales. Para replicar máquinas virtuales de VMware locales o servidores físicos en Azure, el servidor de procesos se instala de forma predeterminada en la máquina del servidor de configuración. 

- Para implementaciones de gran tamaño, es posible que necesite servidores de procesos locales adicionales para escalar la capacidad.
- La conmutación por recuperación desde Azure a local, debe configurar un servidor de procesos temporal en Azure. Esta máquina virtual se puede eliminar cuando finalice la conmutación por recuperación. 

Más información sobre el servidor de procesos.


## <a name="upgrade-a-process-server"></a>Actualizar un servidor de procesos

Al implementar un servidor de procesos de forma local o como una máquina virtual de Azure para la conmutación por recuperación, se instala la versión más reciente del servidor de proceso. Los equipos de Site Recovery publican periódicamente correcciones y mejoras y se recomienda mantener actualizados los servidores de procesos. Puede actualizar un servidor de procesos como sigue:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]


## <a name="move-vms-to-balance-the-process-server-load"></a>Mover las máquinas virtuales para equilibrar la carga del servidor de proceso

Equilibrar la carga moviendo las máquinas virtuales entre dos servidores de procesos, como sigue:

1. En el almacén, en **administrar** haga clic en **infraestructura de Site Recovery**. En **For VMware & Physical machines**, haga clic en **servidores de configuración**.
2. Haga clic en el servidor de configuración con la que se registran los servidores de procesos.
3. Haga clic en el servidor de procesos para el que desea equilibrar la carga.

    ![LoadBalance](media/vmware-azure-manage-process-server/LoadBalance.png)

4. Haga clic en **equilibrar la carga**, seleccione el servidor de procesos de destino al que desea mover las máquinas. A continuación, haga clic en **Aceptar**

    ![LoadPS](media/vmware-azure-manage-process-server/LoadPS.PNG)

2. Haga clic en **seleccionar máquinas**y elija las máquinas que desee mover desde la actual al servidor de procesos de destino. Los detalles del cambio promedio de datos se muestran en cada máquina virtual. A continuación, haga clic en **Aceptar**. 
3. En el almacén, supervisar el progreso del trabajo en **supervisión** > **trabajos de Site Recovery**.

Se tardará unos 15 minutos para que los cambios se reflejan en el portal. Para obtener un efecto más rápido, [actualizar el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server).

## <a name="switch-an-entire-workload-to-another-process-server"></a>Cambie una carga de trabajo completo a otro servidor de procesos

Mover toda la carga de trabajo controlado por un servidor de proceso a un servidor de procesos diferentes, como se indica a continuación:

1. En el almacén, en **administrar** haga clic en **infraestructura de Site Recovery**. En **For VMware & Physical machines**, haga clic en **servidores de configuración**.
2. Haga clic en el servidor de configuración con la que se registran los servidores de procesos.
3. Haga clic en el servidor de procesos desde el que desea cambiar la carga de trabajo.
4. Haga clic en **conmutador**, seleccione el servidor de procesos de destino al que desea mover la carga de trabajo. A continuación, haga clic en **Aceptar**

    ![Switch](media/vmware-azure-manage-process-server/Switch.PNG)

5. En el almacén, supervisar el progreso del trabajo en **supervisión** > **trabajos de Site Recovery**.

Se tardará unos 15 minutos para que los cambios se reflejan en el portal. Para obtener un efecto más rápido, [actualizar el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server).



## <a name="reregister-a-process-server"></a>Volver a registrar un servidor de procesos

Vuelva a registrar un servidor de procesos que se ejecutan en local o en una máquina virtual de Azure con el servidor de configuración como sigue:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Después de guardar la configuración, haga lo siguiente:

1. En el servidor de procesos, abra un símbolo del sistema de administrador.
2. Vaya a la carpeta **%PROGRAMDATA%\ASR\Agent** y ejecute el comando:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Modificar la configuración de proxy para un servidor de procesos local

Si un servidor de procesos local usa a un proxy para conectarse a Azure, puede modificar la configuración de proxy como sigue:

1. Inicie sesión en la máquina del servidor de procesos. 
2. Abra la ventana de comandos de administrador de PowerShell y ejecute el comando siguiente:
   ```powershell
   $pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
   net stop obengine
   net start obengine
   ```
2. Vaya a la carpeta **%PROGRAMDATA%\ASR\Agent**, y ejecute este comando:
   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```

## <a name="remove-a-process-server"></a>Eliminar un servidor de procesos

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="exclude-folders-from-anti-virus-software"></a>Excluir carpetas del software antivirus

Si el software antivirus se está ejecutando en un servidor de procesos de escalado horizontal (o el servidor de destino maestro), excluir las siguientes carpetas de las operaciones de antivirus:


- C:\Archivos de programa\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- Directorio de instalación del servidor de proceso. Por ejemplo:  C:\Program Files (x86)\Microsoft Azure Site Recovery