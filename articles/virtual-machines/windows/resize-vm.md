---
title: Uso de PowerShell para cambiar el tamaño de una máquina virtual Windows en Azure | Microsoft Docs
description: Cambie el tamaño de una máquina virtual Windows creada con el modelo de implementación de Resource Manager utilizando Azure PowerShell.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/30/2018
ms.author: cynthn
ms.openlocfilehash: 0c942056e95812dfbbe6e3b1e8963799088273fb
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2019
ms.locfileid: "55981203"
---
# <a name="resize-a-windows-vm"></a>Cambio de tamaño de una máquina virtual Windows

En este artículo se muestra cómo mover una máquina virtual a otro [tamaño de máquina virtual](sizes.md) diferente con Azure PowerShell.

Después de crear una máquina virtual, puede escalarla o reducirla verticalmente cambiando su tamaño. En algunos casos, hay que desasignarla antes. Esto puede suceder si el nuevo tamaño no está disponible en el clúster de hardware que hospeda la actualmente la máquina virtual.

Si la máquina virtual usa Premium Storage, asegúrese de elegir una versión **s** del tamaño para obtener compatibilidad con este nivel de almacenamiento. Por ejemplo, elija Standard_E4**s**_v3 en lugar de Standard_E4_v3.

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Cambio de tamaño de una máquina virtual Windows que no está en un conjunto de disponibilidad

Establezca algunas variables. Reemplace los valores por su propia información.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Muestre los tamaños de máquina virtual que están disponibles en el clúster de hardware donde se hospeda la máquina virtual. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Si se muestra el tamaño deseado, ejecute los comandos siguientes para cambiar el tamaño de la máquina virtual. Si el tamaño deseado no aparece, vaya al paso 3.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMsize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```

Si el tamaño deseado no se muestra, ejecute los siguientes comandos para desasignar la máquina virtual, cambiar su tamaño y reiniciarla. Sustituya **<newVMsize>** por el tamaño que desee.
   
```powershell
Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName
$vm.HardwareProfile.VmSize = "<newVMSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
```

> [!WARNING]
> Al desasignar la máquina virtual, se liberan todas las direcciones IP dinámicas asignadas a ella. Esto no afecta a los discos del SO y de datos. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Cambio de tamaño de una máquina virtual Windows que está en un conjunto de disponibilidad

Si el nuevo tamaño de una máquina virtual de un conjunto de disponibilidad no está disponible en el clúster de hardware que hospeda la máquina virtual, habrá que desasignar todas las máquinas virtuales del conjunto de disponibilidad para cambiar el tamaño de la máquina virtual. También tendrá que actualizar el tamaño de otras máquinas virtuales del conjunto de disponibilidad después de cambiar el tamaño de una máquina virtual. Para cambiar el tamaño de una máquina virtual de un conjunto de disponibilidad, siga estos pasos.

```powershell
$resourceGroup = "myResourceGroup"
$vmName = "myVM"
```

Muestre los tamaños de máquina virtual que están disponibles en el clúster de hardware donde se hospeda la máquina virtual. 
   
```powershell
Get-AzVMSize -ResourceGroupName $resourceGroup -VMName $vmName 
```

Si se muestra el tamaño deseado, ejecute el comando siguiente para cambiar el tamaño de la máquina virtual. Si no aparece, vaya a la sección siguiente.
   
```powershell
$vm = Get-AzVM -ResourceGroupName $resourceGroup -VMName $vmName 
$vm.HardwareProfile.VmSize = "<newVmSize>"
Update-AzVM -VM $vm -ResourceGroupName $resourceGroup
```
    
Si el tamaño deseado no aparece, continúe con los pasos siguientes para desasignar todas las máquinas virtuales del conjunto de disponibilidad, cambiar su tamaño y reiniciarlas.

Detenga todas las máquinas virtuales del conjunto de disponibilidad.
   
```powershell
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    Stop-AzVM -ResourceGroupName $resourceGroup -Name $vmName -Force
    } 
```

Cambie el tamaño de todas las máquinas virtuales del conjunto de disponibilidad y reinícielas.
   
```powershell
$newSize = "<newVmSize>"
$as = Get-AzAvailabilitySet -ResourceGroupName $resourceGroup
$vmIds = $as.VirtualMachinesReferences
  foreach ($vmId in $vmIDs){
    $string = $vmID.Id.Split("/")
    $vmName = $string[8]
    $vm = Get-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    $vm.HardwareProfile.VmSize = $newSize
    Update-AzVM -ResourceGroupName $resourceGroup -VM $vm
    Start-AzVM -ResourceGroupName $resourceGroup -Name $vmName
    }
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener una mayor escalabilidad, ejecute varias instancias de VM y escálelas horizontalmente. Para obtener más información, consulte el artículo sobre [escalado automático de máquinas Linux en un conjunto de escalado de máquinas virtuales](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

