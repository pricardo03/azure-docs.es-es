---
title: Conexión de un disco de datos a una VM Windows en Azure con PowerShell | Microsoft Docs
description: Cómo conectar un disco de datos nuevo o existente a una VM Windows mediante PowerShell con el modelo de implementación de Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: roygara
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: rogarana
ms.subservice: disks
ms.openlocfilehash: 1abc3fc18de3e9c1751c01c984e15ae44f25d5af
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/24/2019
ms.locfileid: "63766154"
---
# <a name="attach-a-data-disk-to-a-windows-vm-with-powershell"></a>Conexión de un disco a una VM Windows con PowerShell

En este artículo se explica cómo conectar discos nuevos y existentes a una máquina virtual Windows con PowerShell. 

En primer lugar, revise estas sugerencias:

* El tamaño de la máquina virtual controla cuántos discos de datos puede conectar. Para más información, consulte [Tamaños de las máquinas virtuales Linux en Azure](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Para usar discos SSD Premium, necesitará un [tipo de VM con Premium Storage habilitado](sizes-memory.md), como el de las máquinas virtuales de las series DS o GS.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Incorporación de un disco de datos vacío a una máquina virtual

En este ejemplo se muestra cómo agregar un disco de datos vacío a una máquina virtual existente.

### <a name="using-managed-disks"></a>Uso de discos administrados

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Uso de discos administrados en una zona de disponibilidad

Para crear un disco en una zona de disponibilidad, use [New-AzDiskConfig](https://docs.microsoft.com/powershell/module/az.compute/new-azdiskconfig) con el parámetro `-Zone`. En el siguiente ejemplo se crea un disco en la zona *1*.

```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2'
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

### <a name="initialize-the-disk"></a>Initialize the disk

Después de agregar un disco vacío, debe inicializarlo. Para hacerlo, puede iniciar sesión en una VM y usar la administración de discos. Si habilitó [WinRM](https://docs.microsoft.com/windows/desktop/WinRM/portal) y un certificado en la VM cuando la creó, puede usar PowerShell remoto para inicializar el disco. También puede utilizar una extensión de script personalizada:

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
```

El archivo de script puede contener código para inicializar los discos, por ejemplo:

```azurepowershell-interactive
    $disks = Get-Disk | Where partitionstyle -eq 'raw' | sort number

    $letters = 70..89 | ForEach-Object { [char]$_ }
    $count = 0
    $labels = "data1","data2"

    foreach ($disk in $disks) {
        $driveLetter = $letters[$count].ToString()
        $disk | 
        Initialize-Disk -PartitionStyle MBR -PassThru |
        New-Partition -UseMaximumSize -DriveLetter $driveLetter |
        Format-Volume -FileSystem NTFS -NewFileSystemLabel $labels[$count] -Confirm:$false -Force
    $count++
    }
```

## <a name="attach-an-existing-data-disk-to-a-vm"></a>Incorporación de un disco de datos existente a una VM

También puede conectar un disco administrado existente a una máquina virtual como un disco de datos.

```azurepowershell-interactive
$rgName = "myResourceGroup"
$vmName = "myVM"
$location = "East US" 
$dataDiskName = "myDisk"
$disk = Get-AzDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Pasos siguientes

Crear una [instantánea](snapshot-copy-managed-disk.md).
