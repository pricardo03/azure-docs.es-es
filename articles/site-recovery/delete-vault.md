---
title: Eliminación de un almacén de Recovery Services configurado para el servicio Azure Site Recovery
description: Aprenda a eliminar un almacén de Recovery Services configurado para Azure Site Recovery.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: rajanaki
ms.openlocfilehash: a13dee2010688b02fd86fb05900826470a7d7a08
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "67876039"
---
# <a name="delete-a-site-recovery-services-vault"></a>Eliminación de un almacén de Site Recovery Services

Las dependencias pueden evitar la eliminación de un almacén de Azure Site Recovery. Las acciones que hay que llevar a cabo varían en función del escenario de Site Recovery. Para eliminar un almacén de Azure Backup, consulte el artículo sobre la [eliminación de un almacén de Azure Backup](../backup/backup-azure-delete-vault.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="delete-a-site-recovery-vault"></a>Eliminación del almacén de Site Recovery 
Para eliminar el almacén, siga los pasos recomendados para su escenario.
### <a name="azure-vms-to-azure"></a>Máquinas virtuales de Azure en Azure

1. Siga los pasos descritos en el artículo sobre la [deshabilitación de la protección para VMware](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-azure-vm-azure-to-azure) para eliminar todas las máquinas virtuales protegidas.
2. Elimine el almacén.

### <a name="vmware-vms-to-azure"></a>Máquinas virtuales de VMware en Azure

1. Siga los pasos descritos en el artículo sobre la [deshabilitación de la protección para VMware](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure) para eliminar todas las máquinas virtuales protegidas.

2. Siga los pasos descritos en [Eliminación de una directiva de aplicación](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy) para eliminar todas las directivas de replicación.

3. Siga los pasos descritos en el artículo de [eliminación de un servidor vCenter](vmware-azure-manage-vcenter.md#delete-a-vcenter-server) para eliminar las referencias a vCenter.

4. Siga los pasos descritos en el artículo de [retirada de un servidor de configuración](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server) para eliminar el servidor de configuración.

5. Elimine el almacén.


### <a name="hyper-v-vms-with-vmm-to-azure"></a>Máquinas virtuales de Hyper-V (con VMM) en Azure
1. Siga los pasos descritos en [Deshabilitación de la protección para una máquina virtual de Hyper-V que se replica mediante el escenario de System Center VMM a Azure](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario) para eliminar todas las máquinas virtuales protegidas.

2. Desasocie y elimine todas las directivas de replicación examinando su Almacén -> **Infraestructura de Site Recovery** -> **Para System Center VMM** -> **Directivas de replicación**.

3.  Siga los pasos descritos en [Quitar servidores y deshabilitar la protección](site-recovery-manage-registration-and-protection.md##unregister-a-vmm-server) para eliminar las referencias a los servidores Virtual Machine Manager.

4.  Elimine el almacén.

### <a name="hyper-v-vms-without-virtual-machine-manager-to-azure"></a>Máquinas virtuales de Hyper-V (sin Virtual Machine Manager) a Azure
1. Elimine todas las máquinas virtuales protegidas siguiendo los pasos de la sección [Deshabilitación de la protección para una máquina virtual de Hyper-V (Hyper-V a Azure)](site-recovery-manage-registration-and-protection.md#disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure).

2. Desasocie y elimine todas las directivas de replicación examinando su Almacén -> **Infraestructura de Site Recovery** -> **Para Hyper-V Sites** -> **Directivas de replicación**.

3. Siga los pasos del artículo sobre la [anulación del registro de un host de Hyper-V](site-recovery-manage-registration-and-protection.md#unregister-a-hyper-v-host-in-a-hyper-v-site) para eliminar las referencias a los servidores Hyper-V.

4. Elimine el sitio de Hyper-V.

5. Elimine el almacén.


## <a name="use-powershell-to-force-delete-the-vault"></a>Uso de PowerShell para forzar la eliminación del almacén 

> [!Important]
> Si va a probar el producto y no le importa perder datos, puede usar el método de eliminación forzada para eliminar el almacén y todas sus dependencias rápidamente.
> El comando de PowerShell eliminará todo el contenido del almacén y la acción **es irreversible**.

Para eliminar el almacén de Site Recovery aunque haya elementos protegidos, use estos comandos:

    Connect-AzAccount

    Select-AzSubscription -SubscriptionName "XXXXX"

    $vault = Get-AzRecoveryServicesVault -Name "vaultname"

    Remove-AzRecoveryServicesVault -Vault $vault

Obtenga más información sobre [Get-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault) y [Remove-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/remove-azrecoveryservicesvault).
