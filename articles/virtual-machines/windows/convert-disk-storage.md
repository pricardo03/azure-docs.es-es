---
title: Convertir Azure managed disks el almacenamiento de estándar a Premium a estándar o Premium | Microsoft Docs
description: Cómo convertir Azure managed disks de estándar a Premium a estándar o Premium con Azure PowerShell.
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
ms.date: 02/22/2019
ms.author: ramankum
ms.subservice: disks
ms.openlocfilehash: f97140ffeed9115a0308215ea082baee611501fb
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58009874"
---
# <a name="update-the-storage-type-of-a-managed-disk"></a>Actualizar el tipo de almacenamiento de un disco administrado

Hay cuatro opciones para Azure managed disks: Estándar, Premium SSD, SSD estándar y almacenamiento en disco Ultra Azure HDD. Puede cambiar entre estos tipos de almacenamiento según sus necesidades de rendimiento con poco tiempo de inactividad. Esta funcionalidad no se admite para discos no administrados. Pero le resultará muy fácil [convertir un disco no administrado en un disco administrado](convert-unmanaged-to-managed-disks.md) para poder cambiar entre los tipos de disco.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="prerequisites"></a>Requisitos previos

* Dado que la conversión requiere un reinicio de la máquina virtual (VM), debe programar la migración de su almacenamiento en disco durante una ventana de mantenimiento existente previamente.
* Si el disco no está administrado, en primer lugar [convertirlo en un disco administrado](convert-unmanaged-to-managed-disks.md) por lo que puede alternar entre las opciones de almacenamiento.

## <a name="switch-all-managed-disks-of-a-vm-between-premium-and-standard"></a>Cambiar todos los discos administrados de una máquina virtual entre estándar y Premium

En este ejemplo se muestra cómo convertir todos los discos de la máquina virtual de estándar a Premium storage o Premium a Standard storage. Para usar Managed Disks premium, la máquina virtual debe usar un [Tamaño de máquina virtual](sizes.md) que admita Premium Storage. En este ejemplo también se pasa a un tamaño que admite Premium Storage:

```azurepowershell-interactive
# Name of the resource group that contains the VM
$rgName = 'yourResourceGroup'

# Name of the your virtual machine
$vmName = 'yourVM'

# Choose between Standard_LRS and Premium_LRS based on your scenario
$storageType = 'Premium_LRS'

# Premium capable size
# Required only if converting storage from Standard to Premium
$size = 'Standard_DS2_v2'

# Stop and deallocate the VM before changing the size
Stop-AzVM -ResourceGroupName $rgName -Name $vmName -Force

$vm = Get-AzVM -Name $vmName -resourceGroupName $rgName

# Change the VM size to a size that supports Premium storage
# Skip this step if converting storage from Premium to Standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Get all disks in the resource group of the VM
$vmDisks = Get-AzDisk -ResourceGroupName $rgName 

# For disks that belong to the selected VM, convert to Premium storage
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

## <a name="switch-individual-managed-disks-between-standard-and-premium"></a>Cambiar los discos administrados individuales entre estándar y Premium

La carga de trabajo de desarrollo y pruebas, podría una mezcla de discos estándar y Premium para reducir los costos. Puede elegir actualizar sólo esos discos que tienen un mejor rendimiento. En este ejemplo se muestra cómo convertir un único disco de máquina virtual de estándar a Premium storage o Premium a Standard storage. Para usar Managed Disks premium, la máquina virtual debe usar un [Tamaño de máquina virtual](sizes.md) que admita Premium Storage. En este ejemplo también muestra cómo se cambia a un tamaño que admita Premium storage:

```azurepowershell-interactive

$diskName = 'yourDiskName'
# resource group that contains the managed disk
$rgName = 'yourResourceGroupName'
# Choose between Standard_LRS and Premium_LRS based on your scenario
$storageType = 'Premium_LRS'
# Premium capable size 
$size = 'Standard_DS2_v2'

$disk = Get-AzDisk -DiskName $diskName -ResourceGroupName $rgName

# Get parent VM resource
$vmResource = Get-AzResource -ResourceId $disk.ManagedBy

# Stop and deallocate the VM before changing the storage type
Stop-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name -Force

$vm = Get-AzVM -ResourceGroupName $vmResource.ResourceGroupName -Name $vmResource.Name 

# Change the VM size to a size that supports Premium storage
# Skip this step if converting storage from Premium to Standard
$vm.HardwareProfile.VmSize = $size
Update-AzVM -VM $vm -ResourceGroupName $rgName

# Update the storage type
$diskUpdateConfig = New-AzDiskUpdateConfig -AccountType $storageType -DiskSizeGB $disk.DiskSizeGB
Update-AzDisk -DiskUpdate $diskUpdateConfig -ResourceGroupName $rgName `
-DiskName $disk.Name

Start-AzVM -ResourceGroupName $vm.ResourceGroupName -Name $vm.Name
```

## <a name="convert-managed-disks-from-standard-to-premium-in-the-azure-portal"></a>Conversión de managed disks de estándar a Premium en Azure portal

Siga estos pasos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Seleccione la máquina virtual en la lista de **máquinas virtuales** en el portal.
3. Si no se ha detenido la máquina virtual, seleccione **detener** en la parte superior de la máquina virtual **Overview** panel y espere a que la máquina virtual a detener.
3. En el panel de la máquina virtual, seleccione **discos** en el menú.
4. Seleccione el disco que se va a convertir.
5. Seleccione **configuración** en el menú.
6. Cambiar el **tipo de cuenta** desde **HDD estándar** a **SSD Premium**.
7. Haga clic en **guardar**y cerrar el panel de disco.

La conversión de tipos de disco es instantánea. Puede reiniciar la máquina virtual después de la conversión.

## <a name="switch-managed-disks-between-standard-hdd-and-standard-ssd"></a>Cambiar los discos administrados entre estándar HDD y SSD estándar 

En este ejemplo se muestra cómo convertir un único disco de máquina virtual de estándar de HDD a SSD estándar o de estándar SSD a HDD estándar:

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
