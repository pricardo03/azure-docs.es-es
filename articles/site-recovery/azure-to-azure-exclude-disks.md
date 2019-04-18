---
title: Azure Site Recovery - exclusión de discos durante la replicación de máquinas virtuales de Azure mediante Azure PowerShell | Microsoft Docs
description: Obtenga información sobre cómo excluir discos de máquinas virtuales de Azure durante la recuperación del sitio de Azure mediante Azure PowerShell.
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 02/18/2019
ms.author: asgang
ms.openlocfilehash: 54a32d7f7aa4bcab73f5828da3e7eba9d25276be
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678282"
---
# <a name="exclude-disks-from-powershell-replication-of-azure-vms"></a>Excluir discos de la replicación de máquinas virtuales de Azure PowerShell

En este artículo se describe cómo excluir discos al replicar máquinas virtuales de Azure. Puede excluir discos para optimizar el ancho de banda consumido con la replicación o los recursos del lado de destino que usan esos discos. Actualmente, esta funcionalidad está disponible únicamente a través de Azure PowerShell.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar:

- Asegúrese de que comprende el [componentes y arquitectura de recuperación ante desastres](azure-to-azure-architecture.md).
- Revise los [requisitos de compatibilidad](azure-to-azure-support-matrix.md) de todos los componentes.
- Asegúrese de que dispone de AzureRm PowerShell "Az" módulo. Para instalar o actualizar PowerShell, consulte [instalar el módulo Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).
- Asegúrese de que ha creado un almacén de recovery services y máquinas virtuales protegidas al menos una vez. Si aún no ha realizado estas acciones, siga el proceso en [Configurar recuperación ante desastres para máquinas virtuales de Azure con Azure PowerShell](azure-to-azure-powershell.md).

## <a name="why-exclude-disks-from-replication"></a>¿Por qué excluir discos de replicación
Es posible que deba excluir discos de replicación porque:

- La máquina virtual ha alcanzado [frecuencias de cambio de los límites de Azure Site Recovery para replicar datos](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-support-matrix).

- Los datos que se renovados en el disco excluido no es importantes o no necesitan replicarse.

- Desea guardar los recursos de red y almacenamiento por no replica los datos.

## <a name="how-to-exclude-disks-from-replication"></a>Cómo excluir discos de replicación

En nuestro ejemplo, se replican una máquina virtual que tiene un sistema operativo y los tres discos de datos que en la región Este de Estados Unidos en la región Oeste de Estados Unidos 2. El nombre de la máquina virtual es *AzureDemoVM*. Nos excluir el disco 1 y mantenga los discos 2 y 3.

## <a name="get-details-of-the-virtual-machines-to-replicate"></a>Obtener detalles de las máquinas virtuales para replicar

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

Obtener detalles acerca de los discos de la máquina virtual. Esta información se usará más adelante al iniciar la replicación de la máquina virtual.

```azurepowershell
$OSDiskVhdURI = $VM.StorageProfile.OsDisk.Vhd
$DataDisk1VhdURI = $VM.StorageProfile.DataDisks[0].Vhd
```

## <a name="replicate-an-azure-virtual-machine"></a>Replicar una máquina virtual de Azure

El ejemplo siguiente, se supone que ya tiene una cuenta de almacenamiento en caché, la directiva de replicación y asignaciones. Si no tiene estas cosas, siga el proceso en [Configurar recuperación ante desastres para máquinas virtuales de Azure con Azure PowerShell](azure-to-azure-powershell.md).

Replicar una máquina virtual de Azure con *discos administrados*.

```azurepowershell

#Get the resource group that the virtual machine must be created in when failed over.
$RecoveryRG = Get-AzResourceGroup -Name "a2ademorecoveryrg" -Location "West US 2"

#Specify replication properties for each disk of the VM that is to be replicated (create disk replication configuration).

#OsDisk
$OSdiskId =  $vm.StorageProfile.OsDisk.ManagedDisk.Id
$RecoveryOSDiskAccountType = $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType
$RecoveryReplicaDiskAccountType =  $vm.StorageProfile.OsDisk.ManagedDisk.StorageAccountType

$OSDiskReplicationConfig = New-AzRecoveryServicesAsrAzureToAzureDiskReplicationConfig -ManagedDisk -LogStorageAccountId $EastUSCacheStorageAccount.Id `
         -DiskId $OSdiskId -RecoveryResourceGroupId  $RecoveryRG.ResourceId -RecoveryReplicaDiskAccountType  $RecoveryReplicaDiskAccountType `
         -RecoveryTargetDiskAccountType $RecoveryOSDiskAccountType

# Data Disk 1 i.e StorageProfile.DataDisks[0] is excluded, so we will provide it during the time of replication. 

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


#Start replication by creating a replication protected item. Using a GUID for the name of the replication protected item to ensure uniqueness of name.
$TempASRJob = New-ASRReplicationProtectedItem -AzureToAzure -AzureVmId $VM.Id -Name (New-Guid).Guid -ProtectionContainerMapping $EusToWusPCMapping -AzureToAzureDiskReplicationConfiguration $diskconfigs -RecoveryResourceGroupId $RecoveryRG.ResourceId
```

Cuando la operación de replicación inicial se realiza correctamente, los datos de la máquina virtual se replican en la región de recuperación.

Puede ir al portal de Azure y ver las máquinas virtuales replicadas en "elementos replicados".

Se inicia el proceso de replicación mediante la propagación de una copia de los discos de replicación de la máquina virtual en la región de recuperación. Esta fase se denomina la fase de la replicación inicial.

Una vez finalizada la replicación inicial, la replicación pasa a la fase de sincronización de la copia diferencial. En este momento, la máquina virtual está protegida. Seleccione la máquina virtual protegida para ver si se excluyen todos los discos.

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre [ejecuta una conmutación por error de prueba](site-recovery-test-failover-to-azure.md).
