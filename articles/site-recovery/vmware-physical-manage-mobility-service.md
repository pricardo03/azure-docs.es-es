---
title: Administrar el agente de movilidad en servidores para la recuperación ante desastres de máquinas virtuales VMware y servidores físicos en Azure con Azure Site Recovery | Microsoft Docs
description: Aprenda a administrar un agente de Mobility Service para la recuperación ante desastres de máquinas virtuales VMware y servidores físicos en Azure con el servicio Azure Site Recovery.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: ramamill
ms.openlocfilehash: 7cd555f66bb6f65f498f9b3e5db9bbeda0505a8f
ms.sourcegitcommit: 04ec7b5fa7a92a4eb72fca6c6cb617be35d30d0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/22/2019
ms.locfileid: "68384972"
---
# <a name="manage-mobility-agent-on-protected-machines"></a>Administrar el agente de movilidad en máquinas protegidas

Es preciso configurar un agente de movilidad en el servidor cuando se usa Azure Site Recovery para realizar tareas de recuperación ante desastres en servidores físicos y máquinas virtuales de VMware en Azure. El agente de movilidad coordina las comunicaciones entre la máquina protegida y el servidor de configuración/servidor de procesos de escalado horizontal, y administra la replicación de datos. En este artículo se resumen las tareas comunes para administrar el agente de movilidad después de que se haya implementado.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="update-mobility-service-from-azure-portal"></a>Actualizar Mobility Service desde Azure Portal

1. Antes de empezar, asegúrese de que el servidor de configuración, los servidores de procesos de escalado horizontal y los servidores de destino principales que forman parte de la implementación estén actualizados antes de actualizar Mobility Service en las máquinas protegidas.
2. En el portal, abra el almacén > **Elementos replicados**.
3. Si el servidor de configuración es la versión más reciente, verá una notificación que dice "Hay una nueva actualización del agente de replicación de Site Recovery disponible. Haga clic para instalar".

     ![Ventana Elementos replicados](./media/vmware-azure-install-mobility-service/replicated-item-notif.png)

4. Haga clic en la notificación y en **Actualización del agente**, seleccione las máquinas en las que desea actualizar Mobility Service. A continuación, haga clic en **Aceptar**.

     ![Lista de máquinas virtuales de elementos replicados](./media/vmware-azure-install-mobility-service/update-okpng.png)

5. El trabajo de actualización de Mobility Service se inicia para cada una de las máquinas seleccionadas.

## <a name="update-mobility-service-through-powershell-script-on-windows-server"></a>Actualizar Mobility Service mediante un script de PowerShell en un servidor Windows

Use el siguiente script para actualizar Mobility Service en un servidor mediante un cmdlet de PowerShell.

```azurepowershell
Update-AzRecoveryServicesAsrMobilityService -ReplicationProtectedItem $rpi -Account $fabric.fabricSpecificDetails.RunAsAccounts[0]
```

## <a name="update-account-used-for-push-installation-of-mobility-service"></a>Actualizar la cuenta usada para la instalación de inserción de Mobility Service

Al implementar Site Recovery, para habilitar la instalación de inserción de Mobility Service, especificó una cuenta que el servidor de procesos de Site Recovery utiliza para acceder a las máquinas e instalar el servicio cuando la replicación se habilita para la máquina. Si desea actualizar las credenciales para esta cuenta, siga [estas instrucciones](vmware-azure-manage-configuration-server.md#modify-credentials-for-mobility-service-installation).

## <a name="uninstall-mobility-service"></a>Desinstalar Mobility Service

### <a name="on-a-windows-machine"></a>Una máquina Windows

Desinstale desde la interfaz de usuario o desde un símbolo del sistema.

- **Desde la interfaz de usuario**: en el Panel de Control de la máquina, seleccione **Programas**. Seleccione **Microsoft Azure Site Recovery Mobility Service/Master Target server** (Mobility Service de Microsoft Azure Site Recovery/Servidor de destino maestro)  > **Desinstalar**.
- **Desde un símbolo del sistema**: abra una ventana del símbolo del sistema como administrador en la máquina. Ejecute el siguiente comando: 
    ```
    MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
    ```

### <a name="on-a-linux-machine"></a>En un equipo Linux
1. En la máquina Linux, inicie sesión como un usuario **raíz**.
2. En un terminal, vaya a /user/local/ASR.
3. Ejecute el siguiente comando:
    ```
    uninstall.sh -Y
   ```
   
## <a name="install-site-recovery-vss-provider-on-source-machine"></a>Instalación del proveedor de VSS de Site Recovery en el equipo de origen

El proveedor de VSS de Azure Site Recovery es necesario en el equipo de origen para generar puntos de coherencia de la aplicación. Si la instalación del proveedor no se realizó correctamente a través de una instalación de inserción, siga estas directrices para instalarlo manualmente.

1. Abra la ventana cmd de administrador.
2. Vaya a la ubicación de la instalación de Mobility Service (por ejemplo, C:\Archivos de programa (x86)\Microsoft Azure Site Recovery\agent).
3. Ejecute el script InMageVSSProvider_Uninstall. cmd. Se desinstalará el servicio, en caso de que ya exista.
4. Ejecute el script InMageVSSProvider_Install. cmd para instalar el proveedor de VSS manualmente.

## <a name="next-steps"></a>Pasos siguientes

- [Configuración de la recuperación ante desastres para máquinas virtuales de VMware](vmware-azure-tutorial.md)
- [Configuración de la recuperación ante desastres para servidores físicos](physical-azure-disaster-recovery.md)
