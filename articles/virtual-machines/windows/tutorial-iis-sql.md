---
title: 'Tutorial: Creación de máquinas virtuales que ejecuten una pila SQL, IIS y .NET en Azure | Microsoft Docs'
description: En este tutorial, aprenderá a instalar la pila Azure SQL, IIS, .NET en una máquina virtual Windows en Azure.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/27/2018
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: c5fa818a7f0d25a91ea6d272c30384de4c1765b3
ms.sourcegitcommit: b4755b3262c5b7d546e598c0a034a7c0d1e261ec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54887111"
---
# <a name="tutorial-install-the-sql-iis-net-stack-in-a-windows-vm-with-azure-powershell"></a>Tutorial: Instalación de la pila SQL, IIS y .NET en una máquina virtual Windows con Azure PowerShell

En este tutorial, vamos a instalar una pila SQL, IIS y .NET con Azure PowerShell. Esta pila está formada por dos máquinas virtuales que ejecutan Windows Server 2016, una con IIS y .NET y la otra con SQL Server.

> [!div class="checklist"]
> * Crear una VM 
> * Instalación de IIS y el SDK de .NET Core en la máquina virtual
> * Creación de una máquina virtual que ejecute SQL Server
> * Instalación de la extensión de SQL Server

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar PowerShell de forma local, para este tutorial se requiere la versión 5.7.0 del módulo AzureRM.Compute o versiones posteriores. Ejecute `Get-Module -ListAvailable AzureRM.Compute` para encontrar la versión. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).

## <a name="create-a-iis-vm"></a>Creación de una máquina virtual con IIS 

En este ejemplo, usamos el cmdlet [New-AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm) en PowerShell Cloud Shell para crear rápidamente una máquina virtual Windows Server 2016 y, a continuación, instalar IIS y .NET Framework. Las máquinas virtuales con IIS y SQL comparten un grupo de recursos y una red virtual, por lo que creamos variables para esos nombres.


```azurepowershell-interactive
$vmName = "IISVM"
$vNetName = "myIISSQLvNet"
$resourceGroup = "myIISSQLGroup"
New-AzureRmVm `
    -ResourceGroupName $resourceGroup `
    -Name $vmName `
    -Location "East US" `
    -VirtualNetworkName $vNetName `
    -SubnetName "myIISSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -AddressPrefix 192.168.0.0/16 `
    -PublicIpAddressName "myIISPublicIpAddress" `
    -OpenPorts 80,3389 
```

Instale IIS y .NET Framework utilizando la extensión de script personalizado con el cmdlet [Set-AzureRmVMExtension](/powershell/module/azurerm.compute/set-azurermvmextension).

```azurepowershell-interactive
Set-AzureRmVMExtension `
    -ResourceGroupName $resourceGroup `
    -ExtensionName IIS `
    -VMName $vmName `
    -Publisher Microsoft.Compute `
    -ExtensionType CustomScriptExtension `
    -TypeHandlerVersion 1.4 `
    -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server,Web-Asp-Net45,NET-Framework-Features"}' `
    -Location EastUS
```

## <a name="create-another-subnet"></a>Creación de otra subred

Cree una segunda subred para la máquina virtual de SQL. Obtenga la red virtual mediante [Get-AzureRmVirtualNetwork]{/powershell/module/azurerm.network/get-azurermvirtualnetwork}.

```azurepowershell-interactive
$vNet = Get-AzureRmVirtualNetwork `
   -Name $vNetName `
   -ResourceGroupName $resourceGroup
```

Cree una configuración para la subred con [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig).


```azurepowershell-interactive
Add-AzureRmVirtualNetworkSubnetConfig `
   -AddressPrefix 192.168.0.0/24 `
   -Name mySQLSubnet `
   -VirtualNetwork $vNet `
   -ServiceEndpoint Microsoft.Sql
```

Actualice la red virtual con la nueva información de subred mediante [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork).
   
```azurepowershell-interactive   
$vNet | Set-AzureRmVirtualNetwork
```

## <a name="azure-sql-vm"></a>Máquina virtual de Azure SQL

Utilice una imagen de Marketplace de Azure configurada previamente de un servidor SQL Server para crear la máquina virtual de SQL. Primero creamos la máquina virtual y, a continuación, vamos a instalar la extensión de SQL Server en la máquina virtual. 


```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName $resourceGroup `
    -Name "mySQLVM" `
    -ImageName "MicrosoftSQLServer:SQL2016SP1-WS2016:Enterprise:latest" `
    -Location eastus `
    -VirtualNetworkName $vNetName `
    -SubnetName "mySQLSubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "mySQLPublicIpAddress" `
    -OpenPorts 3389,1401 
```

Use [Set-AzureRmVMSqlServerExtension](/powershell/module/azurerm.compute/set-azurermvmsqlserverextension) para agregar la [extensión de SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension) a la máquina virtual con SQL.

```azurepowershell-interactive
Set-AzureRmVMSqlServerExtension `
   -ResourceGroupName $resourceGroup  `
   -VMName mySQLVM `
   -Name "SQLExtension" `
   -Location "EastUS"
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha instalado una pila SQL&#92;IIS&#92;.NET con Azure PowerShell. Ha aprendido a:

> [!div class="checklist"]
> * Crear una VM 
> * Instalación de IIS y el SDK de .NET Core en la máquina virtual
> * Creación de una máquina virtual que ejecute SQL Server
> * Instalación de la extensión de SQL Server

Pase al siguiente tutorial para aprender a proteger un servidor web IIS con certificados SSL.

> [!div class="nextstepaction"]
> [Protección de un servidor web IIS con certificados SSL](tutorial-secure-web-server.md)

