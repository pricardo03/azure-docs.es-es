---
title: Configuración de un servidor de procesos en Azure para conmutación por recuperación de máquinas virtuales y servidores físicos de VMware con Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo configurar un servidor de procesos en Azure para la conmutación por recuperación de máquinas virtuales de Azure a VMware.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 5e0e4646b14a5bff5c40d0817ab8ecccf1046ea9
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49077772"
---
# <a name="set-up-additional-process-servers-for-scalability"></a>Configuración de servidores de procesos adicionales con escalabilidad

De forma predeterminada, al replicar servidores físicos o máquinas virtuales de VMware en Azure con [Site Recovery](site-recovery-overview.md), un servidor de procesos se instala en la máquina del servidor de configuración y se usa para coordinar la transferencia de datos entre Site Recovery y la infraestructura local. Para aumentar la capacidad y el escalado horizontal de la implementación de replicación, puede agregar servidores de procesos independientes adicionales. En este artículo se describe cómo hacerlo.

## <a name="before-you-start"></a>Antes de comenzar

### <a name="capacity-planning"></a>Planificación de capacidad

Asegúrese de que ha realizado la [planeación de la capacidad](site-recovery-plan-capacity-vmware.md) para la replicación de VMware. Esto le ayudará a identificar cómo y cuándo debe implementar servidores de proceso adicionales.

### <a name="sizing-requirements"></a>Requisitos de tamaño 

Compruebe los requisitos de tamaño que se resumen en la tabla. En general, si debe escalar horizontalmente la implementación a más de 200 máquinas de origen o si la tasa de renovación diaria total supera los 2 TB, necesitará servidores de procesos adicionales para controlar el volumen del tráfico.

| **Servidores de procesos adicionales** | **Tamaño del disco de caché** | **Frecuencia de cambio de datos** | **Máquinas protegidas** |
| --- | --- | --- | --- |
|4 vCPU (2 sockets * 2 núcleos a 2,5 GHz), 8 GB de memoria |< 300 GB |250 GB o menos |Replicar 85 máquinas o menos. |
|8 vCPU (2 sockets * 4 núcleos a 2,5 GHz), 12 GB de memoria |600 GB |250 GB a 1 TB |Replicar entre 85 y 150 máquinas. |
|12 vCPU (2 sockets * 6 núcleos a 2,5 GHz), 24 GB de memoria |1 TB |1 TB a 2 TB |Replicar entre 150 y 225 máquinas. |

Donde cada máquina de origen protegida está configurada con 3 discos de 100 GB cada uno.

### <a name="prerequisites"></a>Requisitos previos

En la tabla siguiente se resumen los requisitos previos para el servidor de procesos adicional.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]


## <a name="download-installation-file"></a>Descarga del archivo de instalación

Descargue el archivo de instalación del servidor de procesos de la manera siguiente:

1. Inicie sesión en Azure Portal y busque el almacén de Recovery Services.
2. Abra **Site Recovery Infrastructure** (Infraestructura de Site Recovery)  > **VMWare and Physical Machines** (Máquinas físicas y VMware)  > **Servidores de configuración** (en For VMware & Physical Machines [Para máquinas físicas y VMware]).
3. Seleccione el servidor de configuración para explorar en profundidad su página de detalles. Luego haga clic en **Servidor de procesos**.
4. En **Agregar servidor de procesos** >  **Elegir dónde quiere implementar el servidor de procesos**, seleccione **Implementar un servidor de procesos de escalado horizontal local**.

  ![Página Agregar servidores](./media/vmware-azure-set-up-process-server-scale/add-process-server.png)
1. Haga clic en **Download the Microsoft Azure Site Recovery Unified Setup** (Descargar la instalación unificada de Microsoft Azure Site Recovery). Se descarga la versión más reciente del archivo de instalación.

  > [!WARNING]
  La versión de instalación del servidor de procesos debe ser igual o anterior a la versión del servidor de configuración que se esté ejecutando. Una forma sencilla de garantizar la compatibilidad de versiones consiste en usar el mismo instalador que usó recientemente para instalar o actualizar el servidor de configuración.

## <a name="install-from-the-ui"></a>Instalar desde la interfaz de usuario

Haga la instalación de la siguiente manera. Después de configurar el servidor, debe migrar las máquinas de origen para que lo utilicen.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-add-process-server.md)]


## <a name="install-from-the-command-line"></a>Instalación desde la línea de comandos

Instálelos ejecutando el comando siguiente:

```
UnifiedSetup.exe [/ServerMode <CS/PS>] [/InstallDrive <DriveLetter>] [/MySQLCredsFilePath <MySQL credentials file path>] [/VaultCredsFilePath <Vault credentials file path>] [/EnvType <VMWare/NonVMWare>] [/PSIP <IP address to be used for data transfer] [/CSIP <IP address of CS to be registered with>] [/PassphraseFilePath <Passphrase file path>]
```

Los parámetros de la línea de comandos son los siguientes:

[!INCLUDE [site-recovery-unified-setup-parameters](../../includes/site-recovery-unified-installer-command-parameters.md)]

Por ejemplo: 

```
MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /xC:\Temp\Extracted
cd C:\Temp\Extracted
UNIFIEDSETUP.EXE /AcceptThirdpartyEULA /servermode "PS" /InstallLocation "D:\" /EnvType "VMWare" /CSIP "10.150.24.119" /PassphraseFilePath "C:\Users\Administrator\Desktop\Passphrase.txt" /DataTransferSecurePort 443
```
### <a name="create-a-proxy-settings-file"></a>Creación de un archivo de configuración de proxy

Si tiene que configurar un servidor proxy, el parámetro ProxySettingsFilePath toma un archivo como entrada. Puede crear el archivo de la manera siguiente y pasarlo como parámetro de entrada ProxySettingsFilePath.

```
* [ProxySettings]
* ProxyAuthentication = "Yes/No"
* Proxy IP = "IP Address"
* ProxyPort = "Port"
* ProxyUserName="UserName"
* ProxyPassword="Password"
```

## <a name="next-steps"></a>Pasos siguientes
Más información sobre la [administración de configuración de servidores de procesos](vmware-azure-manage-process-server.md)
