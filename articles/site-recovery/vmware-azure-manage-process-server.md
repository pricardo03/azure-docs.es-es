---
title: Administración de un servidor de procesos para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure mediante Azure Site Recovery | Microsoft Docs
description: En este artículo se describe la administración de un servidor de procesos configurado para la recuperación ante desastres de máquinas virtuales de VMware y servidores físicos en Azure mediante Azure Site Recovery.
author: Rajeswari-Mamilla
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: d99b5d1fdca39466d5e09ca077329b7ffa8622bc
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568859"
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
  Normalmente, al usar la imagen de la galería de Azure para crear un servidor de procesos en Azure para fines de conmutación por recuperación, se estará utilizando la versión más reciente disponible. Los equipos de Site Recovery publican periódicamente correcciones y mejoras y se recomienda mantener actualizados los servidores de procesos.



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
- Directorio de instalación del servidor de procesos; por ejemplo, C:\Archivos de programa (x86)\Microsoft Azure Site Recovery

