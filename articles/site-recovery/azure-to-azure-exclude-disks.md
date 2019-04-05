---
title: 'Azure Site Recovery: Exclusión del disco durante la replicación de máquinas virtuales de Azure mediante Azure PowerShell | Microsoft Docs'
description: Aprenda excluir el disco en máquinas virtuales de Azure con Azure Site Recovery mediante Azure PowerShell.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/18/2019
ms.author: asgang
ms.openlocfilehash: 1c278d810df7e5ba8701529a59987c9bb16fa40c
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044132"
---
# <a name="exclude-disks-from-replication-of-azure-vms-to-azure-using-azure-powershell"></a>Exclusión de discos de la replicación de máquinas virtuales de Azure a Azure mediante Azure PowerShell

En este artículo se describe cómo excluir discos al replicar máquinas virtuales de Azure. Esta exclusión puede optimizar el ancho de banda consumido con la replicación u optimizar los recursos del lado del destino que utilizan estos discos. Actualmente, esta funcionalidad se expone solo mediante Azure PowerShell.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar:

- Asegúrese de entender la [arquitectura y los componentes del escenario](azure-to-azure-architecture.md).
- Revise los [requisitos de compatibilidad](azure-to-azure-support-matrix.md) de todos los componentes.
- Tiene Azure PowerShell `Az` módulo. Si necesita instalar o actualizar Azure PowerShell, siga la guía [Cómo instalar y configurar Azure PowerShell](/powershell/azure/install-az-ps).
- Ya ha creado el almacén de Recovery Services y ha realizado la protección de las máquinas virtuales al menos una vez. Si no lo hacen con la documentación mencionada [aquí](azure-to-azure-powershell.md).

## <a name="why-exclude-disks-from-replication"></a>¿Por qué excluir discos de la replicación?
A menudo es necesario excluir discos de replicación porque:

- La máquina virtual ha alcanzado los [límites de Azure Site Recovery para replicar frecuencias de cambio de datos](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix).

- Los datos renovados en el disco excluido no son importantes o no es necesario replicarlos.

- No quiere replicar la renovación para ahorrar recursos de almacenamiento y de red.


## <a name="how-to-exclude-disks-from-replication"></a>¿Cómo se excluyen discos de la replicación?

En el ejemplo de este artículo, una máquina virtual que tiene un sistema operativo y tres discos de datos de la región Este de EE. UU. se replicará en la región Oeste de EE. UU. 2. El nombre de la máquina virtual usada en el ejemplo es AzureDemoVM. Excluiremos el disco 1 y mantendremos los discos 2 y 3.

## <a name="get-details-of-the-virtual-machines-to-be-replicated"></a>Obtención de los detalles de las máquinas virtuales que se van a replicar

```azurepowershell
# Get details of the virtual machine
$VM = Get-AzVM -ResourceGroupName "A2AdemoRG" -Name "AzureDemoVM"

Write-Output $VM     
```

```
ResourceGroupName  : A2AdemoRG
Id                 : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/A2AdemoRG/providers/Microsoft.Compute/virtualMachines/AzureDemoVM
VmId               : 1b864902-c7ea-499a-ad0f-65da2930b81b
Name               : AzureDemoVM
Type               : Microsoft.Compute/virtualMachines
Location           : eastus
Tags               : {}
DiagnosticsProfile : {BootDiagnostics}
HardwareProfile    : {VmSize}
NetworkProfile     : {NetworkInterfaces}
OSProfile          : {ComputerName, AdminUsername, WindowsConfiguration, Secrets}
ProvisioningState  : Succeeded
StorageProfile     : {ImageReference, OsDisk, DataDisks}
```


Obtenga los detalles de los discos de la máquina virtual. Estos detalles se usarán posteriormente al iniciar la replicación de la máquina virtual.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="replicate-azure-virtual-machine"></a>Replicación de máquinas virtuales de Azure

En el siguiente ejemplo hemos asumido que ya tiene una cuenta de almacenamiento en caché, una directiva de replicación y asignaciones. Si no lo ha hecho, hágalo con la ayuda de la documentación mencionada [aquí](azure-to-azure-powershell.md). 


Replique la máquina virtual de Azure con **discos administrados**.

```azurepowershell

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration)

#OsDisk
$OSdiskId =  $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id `
         -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType

# Data Disk 1 i.e StorageProfile.DataDisks[0] is excluded so we will provide it during the time of replication 

# Data disk 2
$datadiskId2  = $vm.StorageProfile.DataDisks[1].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[1]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[1]. StorageAccountType

$DataDisk2ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId2 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

# Data Disk 3

$datadiskId3  = $vm.StorageProfile.DataDisks[2].ManagedDisk.id
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.DataDisks[2]. StorageAccountType
$RecoveryTargetDiskAccountType = $vm.StorageProfile.DataDisks[2]. StorageAccountType

$DataDisk3ReplicationConfig  = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $CacheStorageAccount.Id `
         -DiskId $datadiskId3 -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryTargetDiskAccountType

#Create a list of disk replication configuration objects for the disks of the virtual machine that are to be replicated.
$diskconfigs = @()
$diskconfigs += $OSDiskReplicationConfig, $DataDisk2ReplicationConfig, $DataDisk3ReplicationConfig


#Start replication by creating replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId
```

Una vez que la operación de replicación inicial se realiza correctamente, se replican los datos de la máquina virtual en la región de recuperación.

Puede ir a Azure Portal y en los elementos replicados puede ver cómo se replican las máquinas virtuales.
El proceso de replicación comienza con la propagación inicial de una copia de los discos de replicación de la máquina virtual en la región de recuperación. Esta fase se conoce como fase de replicación inicial.

Una vez completada la replicación inicial, la replicación pasa a la fase de sincronización diferencial. En este momento, la máquina virtual está protegida. Haga clic en la máquina virtual protegida > discos para ver si el disco se excluye o no.

## <a name="next-steps"></a>Pasos siguientes

[Más información](site-recovery-test-failover-to-azure.md) sobre la ejecución de una conmutación por error de prueba.
