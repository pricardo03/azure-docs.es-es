---
title: Uso de los grupos de selección de ubicación de proximidad para máquinas virtuales Windows | Microsoft Docs
description: Obtenga información sobre la creación y el uso de los grupos de selección de ubicación de proximidad para máquinas virtuales Windows en Azure.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 07/01/2019
ms.author: cynthn
ms.openlocfilehash: af75b3f98232d6507fc8b0fda179bebc75828086
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70088836"
---
# <a name="preview-deploy-vms-to-proximity-placement-groups-using-powershell"></a>Vista previa: Implementación de máquinas virtuales en grupos de selección de ubicación de proximidad con PowerShell


Para acercar las máquinas virtuales lo máximo posible con la menor latencia, debe implementarlas dentro de un [grupo de selección de ubicación de proximidad](co-location.md#preview-proximity-placement-groups).

Un grupo de selección de ubicación de proximidad es una agrupación lógica que se usa para asegurarse de que los recursos de proceso de Azure se encuentran físicamente cercanos entre sí. Los grupos de selección de ubicación de proximidad son útiles para las cargas de trabajo en las que la latencia baja es un requisito.

> [!IMPORTANT]
> Los grupos de selección de ubicación de proximidad se encuentran actualmente en versión preliminar pública.
> Esta versión preliminar se ofrece sin Acuerdo de Nivel de Servicio y no se recomienda para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las Versiones Preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Los grupos de selección de ubicación de proximidad no están disponibles en estas regiones durante la versión preliminar: **Japón Oriental**, **Este de Australia** e **India central**.


## <a name="create-a-proximity-placement-group"></a>Creación de un grupo de selección de ubicación por proximidad
Cree un grupo de selección de ubicación de proximidad con el cmdlet [New-AzProximityPlacementGroup](https://docs.microsoft.com/powershell/module/az.compute/new-azproximityplacementgroup). 

```azurepowershell-interactive
$resourceGroup = "myPPGResourceGroup"
$location = "East US"
$ppgName = "myPPG"
New-AzResourceGroup -Name $resourceGroup -Location $location
$ppg = New-AzProximityPlacementGroup `
   -Location $location `
   -Name $ppgName `
   -ResourceGroupName $resourceGroup `
   -ProximityPlacementGroupType Standard
```

## <a name="list-proximity-placement-groups"></a>Enumeración de los grupos de selección de ubicación de proximidad

Puede enumerar todos los grupos de selección de ubicación de proximidad mediante el cmdlet [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup).

```azurepowershell-interactive
Get-AzProximityPlacementGroup
```


## <a name="create-a-vm"></a>Crear una VM

Cree una máquina virtual en el grupo de selección de ubicación de proximidad con `-ProximityPlacementGroup $ppg.Id` para hacer referencia al identificador de grupo de selección de ubicación de proximidad cuando use [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) para crear la máquina virtual.

```azurepowershell-interactive
$vmName = "myVM"

New-AzVm `
  -ResourceGroupName $resourceGroup `
  -Name $vmName `
  -Location $location `
  -OpenPorts 3389 `
  -ProximityPlacementGroup $ppg.Id
```

Puede ver la máquina virtual en el grupo de selección de ubicación mediante [Get-AzProximityPlacementGroup](/powershell/module/az.compute/get-azproximityplacementgroup).

```azurepowershell-interactive
Get-AzProximityPlacementGroup -ResourceId $ppg.Id |
    Format-Table -Property VirtualMachines -Wrap
```

## <a name="availability-sets"></a>Conjuntos de disponibilidad
También puede crear un conjunto de disponibilidad en el grupo de selección de ubicación de proximidad. Use el mismo parámetro `-ProximityPlacementGroup` con el cmdlet [New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset) para crear un conjunto de disponibilidad, y todas las máquinas virtuales del conjunto de disponibilidad también se crearán en el mismo grupo de selección de ubicación de proximidad.

## <a name="scale-sets"></a>Conjuntos de escalado

También puede crear un conjunto de escalado en el grupo de selección de ubicación de proximidad. Use el mismo parámetro `-ProximityPlacementGroup` con [New-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/new-azvmss) para crear un conjunto de escalado, y todas las instancias se crearán en el mismo grupo de selección de ubicación de proximidad.

## <a name="next-steps"></a>Pasos siguientes

También puede usar la [CLI de Azure](../linux/proximity-placement-groups.md) para crear grupos de selección de ubicación de proximidad.
