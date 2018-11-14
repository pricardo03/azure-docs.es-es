---
title: Acerca de Mobility Service para la recuperación ante desastres de máquinas virtuales VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: Aprenda acerca del agente de Mobility Service para la recuperación ante desastres de máquinas virtuales VMware y servidores físicos en Azure con el servicio Azure Site Recovery.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/04/2018
ms.author: raynew
ms.openlocfilehash: 24102593a294d60b3cc2107bef8d443e672443b3
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51019979"
---
# <a name="about-the-mobility-service-for-vmware-vms-and-physical-servers"></a>Acerca de Mobility Service para máquinas virtuales VMware y servidores físicos

Al configurar la recuperación ante desastres para máquinas virtuales VMware y servidores físicos con [Azure Site Recovery](site-recovery-overview.md), instala el servicio Site Recovery Mobility en cada máquina virtual VMware local y servidor físico.  Mobility Service captura las escrituras de datos en la máquina y las reenvía al servidor de procesos de Site Recovery. Puede implementar Mobility Service con los métodos siguientes:

[Instalación de inserción](vmware-azure-install-mobility-service.md): configure Site Recovery para realizar una instalación de inserción de Mobility Service; para ello, cuando configure la recuperación ante desastres, configure también una cuenta que el servidor de procesos de Site Recovery pueda usar para acceder a la máquina virtual o servidor físico para los fines de instalación del servicio.
[Instalación manual](vmware-physical-mobility-service-install-manual.md): puede instalar Mobility Service manualmente en cada máquina con la interfaz de usuario o el símbolo del sistema.
[Implementación automatizada](vmware-azure-mobility-install-configuration-mgr.md): puede automatizar la instalación con herramientas de implementación de software como System Center Configuration Manager.

## <a name="azure-virtual-machine-agent"></a>Agente de máquina virtual de Azure

- **Las máquinas virtuales Windows**: desde la versión 9.7.0.0 de Mobility Service, el instalador de Mobility Service instala el [agente de máquina virtual de Azure](../virtual-machines/extensions/features-windows.md#azure-vm-agent). De este forma se garantiza que, cuando un equipo conmuta por error a Azure, la máquina virtual de Azure cumple el requisito previo de instalación del agente para usar cualquier extensión de máquina virtual.
- **Máquinas virtuales Linux**: [WALinuxAgent](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent) debe instalarse manualmente en la máquina virtual Azure después de la conmutación por error.

## <a name="installer-files"></a>Archivos del instalador

En la tabla se resumen los archivos del instalador para cada máquina virtual VMware y el sistema operativo del servidor físico. Puede revisar los [sistemas operativos admitidos](vmware-physical-azure-support-matrix.md#replicated-machines) antes de empezar.


**Archivo de instalador** | **Sistema operativo (solo de 64 bits)** 
--- | ---
Microsoft-ASR\_UA\*Windows\*release.exe | Windows Server 2016; Windows Server 2012 R2; Windows Server 2012; Windows Server 2008 R2 SP1 
Microsoft-ASR\_UA\*RHEL6-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 6.* </br> CentOS 6.*
Microsoft-ASR\_UA\*RHEL7-64\*release.tar.gz | Red Hat Enterprise Linux (RHEL) 7.* </br> CentOS 7.* 
Microsoft-ASR\_UA\*SLES12-64\*release.tar.gz | SUSE Linux Enterprise Server 12 SP1, SP2, SP3 
Microsoft-ASR\_UA\*SLES11-SP3-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP3 
Microsoft-ASR\_UA\*SLES11-SP4-64\*release.tar.gz| SUSE Linux Enterprise Server 11 SP4 
Microsoft-ASR\_UA\*OL6-64\*release.tar.gz | Oracle Enterprise Linux 6.4, 6.5
Microsoft-ASR\_UA\*UBUNTU-14.04-64\*release.tar.gz | Ubuntu Linux 14.04
Microsoft-ASR\_UA\*UBUNTU-16.04-64\*release.tar.gz | Servidor Ubuntu Linux 16.04 LTS
Microsoft-ASR_UA\*DEBIAN7-64\*release.tar.gz | Debian 7 
Microsoft-ASR_UA\*DEBIAN8-64\*release.tar.gz | Debian 8


## <a name="update-the-mobility-service"></a>Actualización de Mobility Service

1. Antes de empezar, asegúrese de que el servidor de configuración, los servidores de procesos de escalado horizontal y los servidores de destino principales que forman parte de la implementación estén actualizados antes de actualizar Mobility Service en las máquinas protegidas.
2. En el portal, abra el almacén > **Elementos replicados**.
3. Si el servidor de configuración es la versión más reciente, verá una notificación que dice "Hay una nueva actualización del agente de replicación de Site Recovery disponible. Haga clic para instalar".

     ![Ventana Elementos replicados](.\media\vmware-azure-install-mobility-service\replicated-item-notif.png)

4. Haga clic en la notificación y en **Actualización del agente**, seleccione las máquinas en las que desea actualizar Mobility Service. A continuación, haga clic en **Aceptar**.

     ![Lista de máquinas virtuales de elementos replicados](.\media\vmware-azure-install-mobility-service\update-okpng.png)

5. El trabajo de actualización de Mobility Service se inicia para cada una de las máquinas seleccionadas.

## <a name="update-the-acount-used-for-push-installation-of-the-mobility-service"></a>Actualización de la cuenta para la instalación de inserción de Mobility Service

Al implementar Site Recovery, para habilitar la instalación de inserción de Mobility Service, especificó una cuenta que el servidor de procesos de Site Recovery utiliza para acceder a las máquinas e instalar el servicio cuando la replicación se habilita para la máquina. Si desea actualizar las credenciales para esta cuenta, siga [estas instrucciones](vmware-azure-manage-configuration-server.md).

## <a name="uninstall-the-mobility-service"></a>Desinstalación de Mobility Service

### <a name="on-a-windows-machine"></a>Una máquina Windows

Desinstale desde la interfaz de usuario o desde un símbolo del sistema.

- **Desde la interfaz de usuario**: en el Panel de Control de la máquina, seleccione **Programas**. Seleccione **Microsoft Azure Site Recovery Mobility Service/Master Target server** (Mobility Service de Microsoft Azure Site Recovery/Servidor de destino maestro)  > **Desinstalar**.
- **Desde un símbolo del sistema**: abra una ventana del símbolo del sistema como administrador en la máquina. Ejecute el siguiente comando: 
    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

## <a name="on-a-linux-machine"></a>En una máquina Linux
1. En la máquina Linux, inicie sesión como un usuario **raíz**.
2. En un terminal, vaya a /user/local/ASR.
3. Ejecute el siguiente comando:

    ```
    uninstall.sh -Y
    ```

## <a name="next-steps"></a>Pasos siguientes

[Configurar la instalación de Mobility Service](vmware-azure-install-mobility-service.md).