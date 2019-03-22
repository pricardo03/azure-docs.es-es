---
title: Administración de un servidor de procesos para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure mediante Azure Site Recovery | Microsoft Docs
description: En este artículo se describe la administración de un servidor de procesos configurado para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure mediante Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: ramamill
ms.openlocfilehash: ba80c8ce57495eaa46e915cb0c472eb4aabcee57
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57863634"
---
# <a name="manage-process-servers"></a>Administración de servidores de proceso

De forma predeterminada, el servidor de procesos utilizado al replicar máquinas virtuales de VMware o servidores físicos en Azure se instala en el equipo del servidor de configuración local. Existen un par de casos en los que es necesario configurar un servidor de procesos independiente:

- Para implementaciones de gran tamaño, es posible que necesite servidores de procesos locales adicionales para escalar la capacidad.
- Para conmutación por recuperación, necesitará un servidor de procesos temporal configurado en Azure. Esta máquina virtual se puede eliminar cuando finalice la conmutación por recuperación. 

En este artículo se resumen las tareas de administración habituales para estos servidores de procesos adicionales.

## <a name="upgrade-a-process-server"></a>Actualizar un servidor de procesos

Un servidor de procesos que se ejecuta de forma local o en Azure (para fines de conmutación por recuperación) se actualiza como se indica a continuación:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
>   Normalmente, al usar la imagen de la galería de Azure para crear un servidor de procesos en Azure para fines de conmutación por recuperación, se estará utilizando la versión más reciente disponible. Los equipos de Site Recovery publican periódicamente correcciones y mejoras y se recomienda mantener actualizados los servidores de procesos.

## <a name="balance-the-load-on-process-server"></a>Equilibrar la carga en el servidor de procesos

Para equilibrar la carga entre dos servidores de procesos:

1. Navegue a **Recovery Services Vault** > **Manage** > **Site Recovery Infrastructure** > **For VMware & Physical machines** > **Configuration Servers** (Almacén de Recovery Services > Administrar > Infraestructura de Site Recovery > Para VMWare y máquinas físicas > Servidores de configuración).
2. Haga clic en el servidor de configuración en el que están registrados los servidores de procesos.
3. Lista de los servidores de procesos registrados en los servidores de configuración disponibles en la página.
4. Haga clic en el servidor de procesos en el que quiere modificar la carga de trabajo.

    ![LoadBalance](media/vmware-azure-manage-process-server/LoadBalance.png)

5. Puede usar las opciones **Load Balance** (Equilibrar la carga) o **Switch** (Cambiar), tal como se explica a continuación, según los requisitos.

### <a name="load-balance"></a>Equilibrador de carga

A través de esta opción, puede seleccionar una o más máquinas virtuales y transferirlas a otro servidor de procesos.

1. Haga clic en **Load Balance** (Equilibrar la carga), y seleccione el servidor de procesos de destino en la lista desplegable. Haga clic en **Aceptar**

    ![LoadPS](media/vmware-azure-manage-process-server/LoadPS.PNG)

2. Haga clic en **Seleccionar máquinas**, y elija las máquinas virtuales que quiere mover desde el servidor de procesos actual para el servidor de procesos de destino. Los detalles del cambio promedio de datos se muestran en cada máquina virtual.
3. Haga clic en **OK**. Supervise el progreso del trabajo en **Almacén de Recovery Services** > **Supervisión** > **Trabajos de Site Recovery**.
4. Los cambios tardan 15 minutos en reflejarse después de que finalice correctamente esta operación o también puede [actualizar el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server) para un efecto inmediato.

### <a name="switch"></a>Switch

A través de esta opción, toda la carga de trabajo protegida en un servidor de procesos se traslada a un servidor de procesos diferente.

1. Haga clic en **Switch** (Cambiar), seleccione el servidor de procesos de destino y haga clic en **Aceptar**.

    ![Switch](media/vmware-azure-manage-process-server/Switch.PNG)

2. Supervise el progreso del trabajo en **Almacén de Recovery Services** > **Supervisión** > **Trabajos de Site Recovery**.
3. Los cambios tardan 15 minutos en reflejarse después de que finalice correctamente esta operación o también puede [actualizar el servidor de configuración](vmware-azure-manage-configuration-server.md#refresh-configuration-server) para un efecto inmediato.

## <a name="reregister-a-process-server"></a>Volver a registrar un servidor de procesos

Si necesita volver a registrar un servidor de procesos que se ejecute de forma local o en Azure con el servidor de configuración, haga lo siguiente:

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

Si el servidor de procesos usa un proxy para conectarse a Site Recovery en Azure, utilice este procedimiento si necesita modificar la configuración de proxy existente.

1. Inicie sesión en el equipo del servidor de procesos. 
2. Abra la ventana de comandos de administrador de PowerShell y ejecute el comando siguiente:
   ```powershell
   $pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
   net stop obengine
   net start obengine
   ```
2. Vaya a la carpeta **%PROGRAMDATA%\ASR\Agent** y ejecute el siguiente comando:
   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```


## <a name="remove-a-process-server"></a>Eliminar un servidor de procesos

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="manage-anti-virus-software-on-process-servers"></a>Administración de software antivirus en servidores de procesos

Si hay un software antivirus activo en un servidor de destino maestro o en un servidor de procesos independiente, excluya las siguientes carpetas de las operaciones del programa antivirus:


- C:\Archivos de programa\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site Recovery
- El directorio de instalación del servidor del proceso; por ejemplo: C:\Archivos de programa (x86)\Microsoft Azure Site Recovery

