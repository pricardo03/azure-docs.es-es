---
title: Crear una máquina virtual con una dirección privada estática de IP - PowerShell de Azure | Microsoft Docs
description: Obtenga información sobre cómo crear una máquina virtual con una dirección IP privada mediante PowerShell.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
ms.assetid: d5f18929-15e3-40a2-9ee3-8188bc248ed8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/07/2019
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 9115386b0543e1ac840aec29fc7f57e7c98c03bb
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64685345"
---
# <a name="create-a-virtual-machine-with-a-static-private-ip-address-using-powershell"></a>Crear una máquina virtual con una dirección IP privada estática mediante PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Puede crear una máquina virtual (VM) con una dirección IP privada estática. Asignar una dirección IP privada estática en lugar de una dirección dinámica, si desea seleccionar qué dirección de una subred se asigna a una máquina virtual. Obtenga más información sobre [direcciones IP privadas estáticas](virtual-network-ip-addresses-overview-arm.md#allocation-method). Para cambiar una dirección IP privada asignada a una máquina virtual existente de dinámica a estática, o para trabajar con direcciones IP públicas, consulte [Add, cambio o eliminación de direcciones IP](virtual-network-network-interface-addresses.md).

## <a name="create-a-virtual-machine"></a>de una máquina virtual

Puede realizar los pasos siguientes desde el equipo local o mediante Azure Cloud Shell. Para usar el equipo local, asegúrese de que tiene [instalado Azure PowerShell](/powershell/azure/install-az-ps?toc=%2fazure%2fvirtual-network%2ftoc.json). Para usar Azure Cloud Shell, seleccione **Probar** en la esquina superior derecha de cualquier cuadro de comando que sigue. Cloud Shell inicia su sesión en Azure.

1. Si usa Cloud Shell, continúe al paso 2. Abra una sesión de comandos e inicie sesión en Azure con `Connect-AzAccount`.
2. Cree un grupo de recursos con el comando [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). En el siguiente ejemplo se crea un grupo de recursos en la región Este de EE. UU. de Azure:

   ```azurepowershell-interactive
   $RgName = "myResourceGroup"
   $Location = "eastus"
   New-AzResourceGroup -Name $RgName -Location $Location
   ```

3. Crear una configuración de subred y red virtual con el [New AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig) y [New AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork) comandos:

   ```azurepowershell-interactive
   # Create a subnet configuration
   $SubnetConfig = New-AzVirtualNetworkSubnetConfig `
   -Name MySubnet `
   -AddressPrefix 10.0.0.0/24

   # Create a virtual network
   $VNet = New-AzVirtualNetwork `
   -ResourceGroupName $RgName `
   -Location $Location `
   -Name MyVNet `
   -AddressPrefix 10.0.0.0/16 `
   -Subnet $subnetConfig

   # Get the subnet object for use in a later step.
   $Subnet = Get-AzVirtualNetworkSubnetConfig -Name $SubnetConfig.Name -VirtualNetwork $VNet
   ```

4. Crear una interfaz de red en la red virtual y asignar una dirección IP privada de la subred a la interfaz de red con el [New AzNetworkInterfaceIpConfig](/powershell/module/Az.Network/New-AzNetworkInterfaceIpConfig) y [New AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) comandos:

   ```azurepowershell-interactive
   $IpConfigName1 = "IPConfig-1"
   $IpConfig1     = New-AzNetworkInterfaceIpConfig `
     -Name $IpConfigName1 `
     -Subnet $Subnet `
     -PrivateIpAddress 10.0.0.4 `
     -Primary

   $NIC = New-AzNetworkInterface `
     -Name MyNIC `
     -ResourceGroupName $RgName `
     -Location $Location `
     -IpConfiguration $IpConfig1
   ```

5. Cree una configuración de máquina virtual con [New AzVMConfig](/powershell/module/Az.Compute/New-AzVMConfig)y, a continuación, cree la máquina virtual con [New-AzVM](/powershell/module/az.Compute/New-azVM). Cuando se le solicite, proporcione un nombre de usuario y una contraseña que se usará como la credenciales de inicio de sesión para la máquina virtual:

   ```azurepowershell-interactive
   $VirtualMachine = New-AzVMConfig -VMName MyVM -VMSize "Standard_DS3"
   $VirtualMachine = Set-AzVMOperatingSystem -VM $VirtualMachine -Windows -ComputerName MyServerVM -ProvisionVMAgent -EnableAutoUpdate
   $VirtualMachine = Add-AzVMNetworkInterface -VM $VirtualMachine -Id $NIC.Id
   $VirtualMachine = Set-AzVMSourceImage -VM $VirtualMachine -PublisherName 'MicrosoftWindowsServer' -Offer 'WindowsServer' -Skus '2012-R2-Datacenter' -Version latest
   New-AzVM -ResourceGroupName $RgName -Location $Location -VM $VirtualMachine -Verbose
   ```

> [!WARNING]
> Aunque puede agregar la configuración de direcciones IP privadas para el sistema operativo, se recomienda no hacerlo después de la lectura [agregar una dirección IP privada a un sistema operativo](virtual-network-network-interface-addresses.md#private).
> 
> 
> <a name = "change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface"></a>
> 
> [!IMPORTANT]
> Para obtener acceso a la máquina virtual desde internet, debe asignar una dirección IP pública a la máquina virtual. También puede cambiar una asignación dinámica de direcciones IP privada para una asignación estática. Para obtener más información, consulte [agregar o cambiar las direcciones IP](virtual-network-network-interface-addresses.md). Además, se recomienda que limite el tráfico de red a la máquina virtual mediante la asociación de un grupo de seguridad de red a la interfaz de red, la subred que creó en la interfaz de red o ambos. Para obtener más información, consulte [administrar grupos de seguridad de red](manage-network-security-group.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no lo necesite, puede usar [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene:

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre [direcciones IP privadas](virtual-network-ip-addresses-overview-arm.md#private-ip-addresses) y asignar un [dirección IP privada estática](virtual-network-network-interface-addresses.md#add-ip-addresses) a una máquina virtual de Azure.
- Más información acerca de cómo crear [Linux](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) y [Windows](../virtual-machines/windows/tutorial-manage-vm.md?toc=%2fazure%2fvirtual-network%2ftoc.json) máquinas virtuales.
