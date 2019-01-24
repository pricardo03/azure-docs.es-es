---
title: Conexión de un disco de datos a una VM Windows en Azure con PowerShell | Microsoft Docs
description: Cómo conectar un disco de datos nuevo o existente a una VM Windows mediante PowerShell con el modelo de implementación de Resource Manager.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/16/2018
ms.author: cynthn
ms.component: disks
ms.openlocfilehash: b293a60240622b5c8e417bf61de83ae8817e203f
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54477131"
---
# <a name="attach-a-data-disk-to-a-windows-vm-with-powershell"></a>Conexión de un disco a una VM Windows con PowerShell

En este artículo se explica cómo conectar discos nuevos y existentes a una máquina virtual Windows con PowerShell. 

En primer lugar, revise estas sugerencias:
* El tamaño de la máquina virtual controla cuántos discos de datos puede conectar. Para más información, consulte [Tamaños de las máquinas virtuales Linux en Azure](sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Para usar Premium Storage, necesitará una VM con Premium Storage habilitado con un tipo como el de las VM de las series DS o GS. Para más información, consulte [Premium Storage: Almacenamiento de alto rendimiento para cargas de trabajo de la máquina virtual de Azure](premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Para instalar y usar PowerShell de forma local, en este tutorial se requiere la versión 6.0.0 del módulo de Azure PowerShell, o cualquier versión posterior. Ejecute ` Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps). Si PowerShell se ejecuta localmente, también deberá ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.


## <a name="add-an-empty-data-disk-to-a-virtual-machine"></a>Incorporación de un disco de datos vacío a una máquina virtual

En este ejemplo se muestra cómo agregar un disco de datos vacío a una máquina virtual existente.

### <a name="using-managed-disks"></a>Uso de discos administrados

```azurepowershell-interactive
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

### <a name="using-managed-disks-in-an-availability-zone"></a>Uso de discos administrados en una zona de disponibilidad
Para crear un disco en una zona de disponibilidad, use [New-AzureRmDiskConfig](/powershell/module/azurerm.compute/new-azurermdiskconfig) con el parámetro `-Zone`. En el siguiente ejemplo se crea un disco en la zona *1*.


```powershell
$rgName = 'myResourceGroup'
$vmName = 'myVM'
$location = 'East US 2' 
$storageType = 'Premium_LRS'
$dataDiskName = $vmName + '_datadisk1'

$diskConfig = New-AzureRmDiskConfig -SkuName $storageType -Location $location -CreateOption Empty -DiskSizeGB 128 -Zone 1
$dataDisk1 = New-AzureRmDisk -DiskName $dataDiskName -Disk $diskConfig -ResourceGroupName $rgName

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -CreateOption Attach -ManagedDiskId $dataDisk1.Id -Lun 1

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```


### <a name="initialize-the-disk"></a>Initialize the disk

Después de agregar un disco vacío, debe inicializarlo. Para hacerlo, puede iniciar sesión en una VM y usar la administración de discos. Si habilitó [WinRM](https://docs.microsoft.com/windows/desktop/WinRM/portal) y un certificado en la VM cuando la creó, puede usar PowerShell remoto para inicializar el disco. También puede utilizar una extensión de script personalizada: 

```azurepowershell-interactive
    $location = "location-name"
    $scriptName = "script-name"
    $fileName = "script-file-name"
    Set-AzureRmVMCustomScriptExtension -ResourceGroupName $rgName -Location $locName -VMName $vmName -Name $scriptName -TypeHandlerVersion "1.4" -StorageAccountName "mystore1" -StorageAccountKey "primary-key" -FileName $fileName -ContainerName "scripts"
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
$disk = Get-AzureRmDisk -ResourceGroupName $rgName -DiskName $dataDiskName 

$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName 

$vm = Add-AzureRmVMDataDisk -CreateOption Attach -Lun 0 -VM $vm -ManagedDiskId $disk.Id

Update-AzureRmVM -VM $vm -ResourceGroupName $rgName
```

## <a name="next-steps"></a>Pasos siguientes

Crear una [instantánea](snapshot-copy-managed-disk.md).
