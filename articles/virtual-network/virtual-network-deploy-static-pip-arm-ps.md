---
title: Creación de una máquina virtual con una dirección IP pública estática (PowerShell) | Microsoft Docs
description: Obtenga información sobre cómo crear una máquina virtual con una dirección IP pública estática mediante PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ad975ab9-d69f-45c1-9e45-0d3f0f51e87e
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/08/2018
ms.author: jdial
ms.openlocfilehash: b59157b0f17380dbe4386fbd9ac75776e22f749e
ms.sourcegitcommit: d16b7d22dddef6da8b6cfdf412b1a668ab436c1f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39713982"
---
# <a name="create-a-virtual-machine-with-a-static-public-ip-address-using-powershell"></a>Creación de una máquina virtual con una dirección IP pública estática mediante PowerShell

Puede crear una máquina virtual con una dirección IP pública estática. Una dirección IP pública le permite comunicarse con una máquina virtual desde Internet. Asigne una dirección IP pública estática, en lugar de una dirección dinámica, para garantizar que la dirección no cambie nunca. Más información sobre [direcciones IP públicas estáticas](virtual-network-ip-addresses-overview-arm.md#allocation-method). Para cambiar una dirección IP pública asignada a una máquina virtual existente de dinámica a estática, o para trabajar con direcciones IP privadas, consulte [Incorporación, cambio o eliminación de direcciones IP](virtual-network-network-interface-addresses.md). Las direcciones IP públicas tienen un [costo nominal](https://azure.microsoft.com/pricing/details/ip-addresses) y hay un [límite](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) en el número de direcciones IP públicas que se pueden usar por suscripción.

## <a name="create-a-virtual-machine"></a>de una máquina virtual

Puede realizar los pasos siguientes desde el equipo local o mediante Azure Cloud Shell. Para usar el equipo local, asegúrese de que tiene [instalado Azure PowerShell](/powershell/azure/install-azurerm-ps?toc=%2fazure%2fvirtual-network%2ftoc.json). Para usar Azure Cloud Shell, seleccione **Probar** en la esquina superior derecha de cualquier cuadro de comando que sigue. Cloud Shell inicia su sesión en Azure.

1. Si usa Cloud Shell, continúe al paso 2. Abra una sesión de comandos e inicie sesión en Azure con `Connect-AzureRmAccount`.
2. Cree un grupo de recursos con el comando [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). En el siguiente ejemplo se crea un grupo de recursos en la región Este de EE. UU. de Azure:

   ```azurepowershell-interactive
   New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
   ```

3. Cree una máquina virtual con el comando [New-AzureRmVM](/powershell/module/AzureRM.Compute/New-AzureRmVM). La opción `-AllocationMethod "Static"` asigna una dirección IP pública estática a la máquina virtual. En el ejemplo siguiente se crea una máquina virtual Windows Server con una dirección IP pública estática, con una SKU básica, denominada *myPublicIpAddress*. Cuando se le solicite, proporcione un nombre de usuario y una contraseña; se usarán como credenciales de inicio de sesión para la máquina virtual:

   ```azurepowershell-interactive
   New-AzureRmVm `
     -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "East US" `
     -PublicIpAddressName "myPublicIpAddress" `
     -AllocationMethod "Static"
   ```

   Si la dirección IP pública debe ser una SKU estándar, tiene que [crear una dirección IP pública](virtual-network-public-ip-address.md#create-a-public-ip-address), [crear una interfaz de red](virtual-network-network-interface.md#create-a-network-interface), [asignar la dirección IP pública a la interfaz de red](virtual-network-network-interface-addresses.md#add-ip-addresses) y luego [crear una máquina virtual con la interfaz de red](virtual-network-network-interface-vm.md#add-existing-network-interfaces-to-a-new-vm), en pasos independientes. Más información sobre las [SKU de dirección IP pública](virtual-network-ip-addresses-overview-arm.md#sku). Si la máquina virtual se va a agregar al grupo back-end de una instancia pública de Azure Load Balancer, la SKU de la dirección IP pública de la máquina virtual debe coincidir con la SKU de la dirección IP del equilibrador de carga. Para más información, consulte [Azure Load Balancer](../load-balancer/load-balancer-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#skus).

4. Vea la dirección IP pública asignada y confirme que se creó como una dirección estática, con [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress):

   ```azurepowershell-interactive
   Get-AzureRmPublicIpAddress `
     -ResourceGroupName "myResourceGroup" `
     -Name "myPublicIpAddress" `
     | Select "IpAddress", "PublicIpAllocationMethod" `
     | Format-Table
   ```

   Azure asigna una dirección IP pública de las direcciones usadas en la región en la que creó la máquina virtual. Puede descargar la lista de intervalos (prefijos) para las nubes de Azure [Pública](https://www.microsoft.com/download/details.aspx?id=56519), [Gobierno de Estados Unidos](https://www.microsoft.com/download/details.aspx?id=57063), [China](https://www.microsoft.com/download/details.aspx?id=57062) y [Alemania](https://www.microsoft.com/download/details.aspx?id=57064).

> [!WARNING]
No modifique la configuración de direcciones IP dentro del sistema operativo de la máquina virtual. El sistema operativo no conoce las direcciones IP públicas de Azure. Aunque puede agregar la configuración de dirección IP privada al sistema operativo, se recomienda no hacerlo a menos que sea necesario y no hasta después de haber leído [Incorporación o eliminación de direcciones IP privadas a un sistema operativo](virtual-network-network-interface-addresses.md#private).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no lo necesite, puede usar [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre [direcciones IP públicas](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses) en Azure.
- Más información sobre toda la [configuración de dirección IP pública](virtual-network-public-ip-address.md#create-a-public-ip-address).
- Más información sobre [direcciones IP privadas](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) y la asignación de una [dirección IP privada estática](virtual-network-network-interface-addresses.md#add-ip-addresses) a una máquina virtual de Azure.
- Más información sobre cómo crear máquinas virtuales [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) y [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json).