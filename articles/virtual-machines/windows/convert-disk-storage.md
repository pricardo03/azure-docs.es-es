---
title: Conversión de almacenamiento de Azure Managed Disks de estándar a premium, y viceversa | Microsoft Docs
description: Conversión de Azure Managed Disks de estándar a premium, y viceversa, mediante Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: ramankumarlive
manager: kavithag
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: ramankum
ms.subservice: disks
ms.openlocfilehash: 94482666d0db3157b0c18c0b47f9937457172521
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56116004"
---
# <a name="update-the-storage-type-of-a-managed-disk"></a>Actualizar el tipo de almacenamiento de un disco administrado

Azure Managed Disks ofrece tres opciones de tipo de almacenamiento: [SSD Premium](../windows/premium-storage.md), [SSD estándar](../windows/disks-standard-ssd.md) y [HDD estándar](../windows/standard-storage.md). En función de sus necesidades de rendimiento, puede cambiar un disco administrado entre tipos de almacenamiento con un tiempo de inactividad mínimo. En un disco no administrado, no se puede cambiar de un tipo de almacenamiento a otro. Sin embargo, le resultará muy fácil [convertir un disco no administrado en un disco administrado](convert-unmanaged-to-managed-disks.md).

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="prerequisites"></a>Requisitos previos

* Debido a que la conversión requiere reiniciar la máquina virtual, debería programar la migración del almacenamiento de discos durante una ventana de mantenimiento existente previamente. 
* Si usa un disco no administrado, en primer lugar [conviértalo en un disco administrado](convert-unmanaged-to-managed-disks.md) para permitirle cambiar entre tipos de almacenamiento. 
* Los ejemplos de este artículo requieren la versión 6.0.0 o posterior del módulo de Azure PowerShell. Ejecute `Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps). Ejecute [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) para crear una conexión con Azure.


## <a name="convert-all-the-managed-disks-of-a-vm-from-standard-to-premium"></a>Conversión de todos los discos administrados de una máquina virtual de estándar a prémium

En el ejemplo siguiente se muestra cómo cambiar todos los discos de una máquina virtual del almacenamiento estándar al premium. Para usar discos administrados prémium, la máquina virtual debe usar un [Tamaño de máquina virtual](sizes.md) que admita Premium Storage. En este ejemplo también se pasa a un tamaño que admite Premium Storage:

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'

# Premium capable size
# Required only if converting storage from standard to premium
$size = 'Standard_DS2_v2'

# Stop and deallocate the VM before changing the size
Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to premium storage
foreach ($disk in $vmDisks)
{
    if ($disk.ManagedBy -eq $vm.Id)
    {
        $diskUpdateConfig = New-AzDiskUpdateConfig –AccountType $storageType
        Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
        -DiskName $disk.Name
    }
}

Start-AzVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="convert-a-managed-disk-from-standard-to-premium"></a>Conversión de un disco administrado de estándar a prémium

Puede que, en el caso de la carga de trabajo de desarrollo y pruebas, prefiera tener una combinación de discos estándar y prémium para reducir el costo. Para ello, puede actualizar a Premium Storage solo los discos que requieren mejor rendimiento. En el ejemplo siguiente se muestra cómo cambiar un solo disco de una máquina virtual del almacenamiento estándar al premium, y viceversa. Para usar discos administrados prémium, la máquina virtual debe usar un [Tamaño de máquina virtual](sizes.md) que admita Premium Storage. En este ejemplo se muestra cómo pasar a un tamaño que admite Premium Storage:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between StandardLRS and PremiumLRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Change the VM size to a size that supports premium storage
# Skip this step if converting storage from premium to standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="convert-a-managed-disk-from-standard-hdd-to-standard-ssd"></a>Conversión de un disco administrado de HDD estándar a SSD estándar

En el ejemplo siguiente se muestra cómo cambiar un solo disco de una máquina virtual de HDD estándar a SSD estándar, y viceversa:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and StandardSSD_LRS based on your scenario
$storageType = 'StandardSSD_LRS'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="next-steps"></a>Pasos siguientes

Realice una copia de solo lectura de una máquina virtual mediante una [instantánea](snapshot-copy-managed-disk.md).

