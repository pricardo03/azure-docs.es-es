---
title: Migración de una máquina virtual clásica a una máquina virtual de disco administrada con ARM | Microsoft Docs
description: Migre una VM de Azure única desde el modelo de implementación clásico a Managed Disks en el modelo de implementación de Resource Manager.
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
ms.date: 06/15/2017
ms.author: cynthn
ms.openlocfilehash: b69cc0bcff8f791e0740b30f5521a2a7b36af3b1
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54437664"
---
# <a name="migrate-a-classic-vm-to-use-a-managed-disk"></a>Migración de una VM clásica para usar Managed Disks 


Esta sección lo ayudará a migrar las máquinas virtuales de Azure existentes desde el modelo de implementación clásico a [Managed Disks](managed-disks-overview.md) en el modelo de implementación de Resource Manager.


## <a name="plan-for-the-migration-to-managed-disks"></a>Planeación de la migración a Managed Disks

Esta sección puede ayudarlo a tomar la mejor decisión sobre los tipos de discos y VM.


### <a name="location"></a>Ubicación

Elija una ubicación donde Managed Disks esté disponible. Si va a migrar a Managed Disks con almacenamiento Premium, asegúrese también de que el almacenamiento Premium esté disponible en esa región. Consulte [Servicios de Azure por región](https://azure.microsoft.com/regions/#services) para información actualizada sobre las ubicaciones disponibles.

### <a name="vm-sizes"></a>Tamaños de VM

Si va a migrar a Managed Disks con almacenamiento Premium, debe actualizar el tamaño de la VM a un tamaño compatible con Premium Storage disponible en la región donde se ubica la VM. Revise los tamaños de VM compatibles con Premium Storage. Las especificaciones de tamaño de las máquinas virtuales de Azure se muestran en [Tamaños de máquinas virtuales](sizes.md).
Repase las características de rendimiento de las máquinas virtuales que trabajan con Premium Storage y elija el tamaño de máquina virtual que se mejor se ajuste a su carga de trabajo. Procure que haya suficiente ancho de banda disponible en la máquina virtual para dirigir el tráfico de disco.

### <a name="disk-sizes"></a>Tamaños de disco

**Premium**

Hay siete tipos de almacenamiento Premium que se pueden usar con la VM, y cada uno de ellos tiene sus límites específicos de rendimiento y E/S por segundo. Considere estos límites a la hora de elegir el tipo de disco Premium para la VM según las necesidades de capacidad, rendimiento, escalabilidad y cargas máximas de la aplicación.

| Tipo de discos Premium  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Tamaño del disco           | 128 GB| 512 GB| 128 GB| 512 GB            | 1.024 GB (1 TB)    | 2048 GB (2 TB)    | 4095 GB (4 TB)    | 
| IOPS por disco       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Rendimiento de disco | 25 MB por segundo  | 50 MB por segundo  | 100 MB por segundo | 150 MB por segundo | 200 MB por segundo | 250 MB por segundo | 250 MB por segundo | 

**Estándar**

Hay siete tipos de discos estándar que se pueden usar con la VM. Cada uno de ellos tiene una capacidad distinta, pero los mismos límites de rendimiento y E/S por segundo. Elija el tipo de disco administrado Estándar según las necesidades de capacidad de la aplicación.

| Tipo de disco Estándar  | S4               | S6               | S10              | S20              | S30              | S40              | S50              | 
|---------------------|---------------------|---------------------|------------------|------------------|------------------|------------------|------------------| 
| Tamaño del disco           | 30 GB            | 64 GB            | 128 GB           | 512 GB           | 1.024 GB (1 TB)   | 2048 GB (2 TB)    | 4095 GB (4 TB)   | 
| IOPS por disco       | 500              | 500              | 500              | 500              | 500              | 500             | 500              | 
| Rendimiento de disco | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 60 MB por segundo | 


### <a name="disk-caching-policy"></a>Directiva de almacenamiento en caché de disco 

**Managed Disks Premium**

De forma predeterminada, la directiva de almacenamiento en caché de los discos es *Solo lectura* para todos los discos de datos Premium y *Lectura y Escritura* para el disco de sistema operativo Premium conectado a la máquina virtual. Recomendamos esta opción de configuración para lograr el rendimiento óptimo de E/S de la aplicación. Para discos de datos de solo escritura o de gran cantidad de escritura (por ejemplo, archivos de registro de SQL Server), deshabilite el almacenamiento en caché de disco para lograr un mejor rendimiento de la aplicación.

### <a name="pricing"></a>Precios

Revise el [precio de Managed Disks](https://azure.microsoft.com/pricing/details/managed-disks/). Los precios de los discos administrados premium son iguales que para los discos no administrados premium. Sin embargo, los precios de discos administrados estándar son distintos a los de los discos no administrados estándar.


## <a name="checklist"></a>Lista de comprobación

1.  Si va a migrar a Managed Disks Premium, asegúrese de que esté disponible en la región a la que migra.

2.  Decida la nueva serie de máquinas virtuales que se va a usar. Debe ser compatible con Premium Storage si migra a Managed Disks Premium.

3.  Decida el tamaño de VM exacto que usará, el que debe estar disponible en la región a la que migra. El tamaño de la máquina virtual debe ser suficientemente grande para admitir el número de discos de datos que tiene. Por ejemplo, si tiene cuatro discos de datos, la VM debe tener dos o más núcleos. Considere también las necesidades de potencia de procesamiento, memoria y ancho de banda de red.

4.  Tenga a mano los detalles de la máquina virtual, incluida la lista de discos y los blobs de disco duro virtual correspondientes.

Prepare la aplicación para el tiempo de inactividad. Para realizar una migración limpia, tendrá que detener todo el procesamiento del sistema actual. Solo entonces se puede obtener un estado coherente que se puede migrar a la nueva plataforma. La duración del tiempo de inactividad depende de la cantidad de datos en los discos que se van a migrar.


## <a name="migrate-the-vm"></a>Migración de la VM

Prepare la aplicación para el tiempo de inactividad. Para realizar una migración limpia, tendrá que detener todo el procesamiento del sistema actual. Solo entonces se puede obtener un estado coherente que se puede migrar a la nueva plataforma. La duración del tiempo de inactividad depende de la cantidad de datos en los discos que se van a migrar.

Este elemento requiere la versión 6.0.0 del módulo de Azure PowerShell, o cualquier versión posterior. Ejecute ` Get-Module -ListAvailable AzureRM` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps). También tiene que ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure.


Cree variables para los parámetros comunes.

```powershell
$resourceGroupName = 'yourResourceGroupName'

$location = 'your location' 

$virtualNetworkName = 'yourExistingVirtualNetworkName'

$virtualMachineName = 'yourVMName'

$virtualMachineSize = 'Standard_DS3'

$adminUserName = "youradminusername"

$adminPassword = "yourpassword" | ConvertTo-SecureString -AsPlainText -Force

$imageName = 'yourImageName'

$osVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd'

$dataVhdUri = 'https://storageaccount.blob.core.windows.net/vhdcontainer/datadisk1.vhd'

$dataDiskName = 'dataDisk1'
```

Cree un disco administrado de SO mediante el VHD de la VM clásica. Asegúrese de que proporcionó el URI completo del VHD del SO al parámetro $osVhdUri. Además, escriba **-AccountType** como **Premium_LRS** o **Standard_LRS** según el tipo de discos al que migra (premium o estándar).

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName '
   -Disk (New-AzureRmDiskConfig '
   -AccountType Premium_LRS '
   -Location $location '
   -CreateOption Import '
   -SourceUri $osVhdUri) '
   -ResourceGroupName $resourceGroupName
```

Conecte el disco de SO a la nueva VM.

```powershell
$VirtualMachine = New-AzureRmVMConfig -VMName $virtualMachineName -VMSize $virtualMachineSize
$VirtualMachine = Set-AzureRmVMOSDisk '
   -VM $VirtualMachine '
   -ManagedDiskId $osDisk.Id '
   -StorageAccountType Premium_LRS '
   -DiskSizeInGB 128 '
   -CreateOption Attach -Windows
```

Cree un disco de datos administrado desde el archivo de VHD de datos y agréguelo a la VM nueva.

```powershell
$dataDisk1 = New-AzureRmDisk '
   -DiskName $dataDiskName '
   -Disk (New-AzureRmDiskConfig '
   -AccountType Premium_LRS '
   -Location $location '
   -CreationOption Import '
   -SourceUri $dataVhdUri ) '
   -ResourceGroupName $resourceGroupName
    
$VirtualMachine = Add-AzureRmVMDataDisk '
   -VM $VirtualMachine '
   -Name $dataDiskName '
   -CreateOption Attach '
   -ManagedDiskId $dataDisk1.Id '
   -Lun 1
```

Establezca la dirección IP pública, la red virtual y la NIC para crear la VM nueva.

```powershell
$publicIp = New-AzureRmPublicIpAddress '
   -Name ($VirtualMachineName.ToLower()+'_ip') '
   -ResourceGroupName $resourceGroupName '
   -Location $location '
   -AllocationMethod Dynamic
    
$vnet = Get-AzureRmVirtualNetwork '
   -Name $virtualNetworkName '
   -ResourceGroupName $resourceGroupName
    
$nic = New-AzureRmNetworkInterface '
   -Name ($VirtualMachineName.ToLower()+'_nic') '
   -ResourceGroupName $resourceGroupName '
   -Location $location '
   -SubnetId $vnet.Subnets[0].Id '
   -PublicIpAddressId $publicIp.Id
    
$VirtualMachine = Add-AzureRmVMNetworkInterface '
   -VM $VirtualMachine '
   -Id $nic.Id
    
New-AzureRmVM -VM $VirtualMachine '
   -ResourceGroupName $resourceGroupName '
   -Location $location
```

> [!NOTE]
>Puede que en esta guía no se contemplen otros pasos necesarios para admitir su aplicación.
>
>

## <a name="next-steps"></a>Pasos siguientes

- Conexión a una máquina virtual. Para obtener instrucciones, consulte [Conexión a una máquina virtual de Azure donde se ejecuta Windows e inicio de sesión en ella](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

