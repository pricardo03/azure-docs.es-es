---
title: Creación de máquinas virtuales Windows con el cmdlet New-AzVM simplificado en Azure Cloud Shell | Microsoft Docs
description: Aprenda rápidamente a crear máquinas virtuales Windows con el cmdlet New-AzVM simplificado en Azure Cloud Shell.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 12/12/2017
ms.author: cynthn
ROBOTS: NOINDEX
ms.openlocfilehash: cdf7385b3686c5c3f91e66c67ab5f0758935b39f
ms.sourcegitcommit: 1aefdf876c95bf6c07b12eb8c5fab98e92948000
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/06/2019
ms.locfileid: "66730137"
---
# <a name="create-a-windows-virtual-machine-with-the-simplified-new-azvm-cmdlet-in-cloud-shell"></a>Creación de una máquina virtual Windows con el cmdlet New-AzVM simplificado en Cloud Shell 

El cmdlet [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) ha agregado un conjunto simplificado de parámetros para crear una nueva VM con PowerShell. En este tema se muestra cómo usar PowerShell en Azure Cloud Shell, con la versión más reciente del cmdlet New-AzureVM preinstalada, para crear una nueva máquina virtual. Se usará un conjunto de parámetros simplificado que crea automáticamente todos los recursos necesarios con valores predeterminados inteligentes. 

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]


## <a name="create-the-vm"></a>Creación de la máquina virtual

Puede usar el cmdlet [New-AzVM](https://docs.microsoft.com/powershell/module/az.compute/new-azvm) para crear una máquina virtual con los valores predeterminados inteligentes que incluyen el uso de la imagen de Windows Server 2016 Datacenter de Azure Marketplace. Puede usar New-AzVM simplemente con el parámetro **-Name** y se utilizará ese valor para todos los nombres de recursos. En este ejemplo, establecemos el parámetro **-Name** como *myVM*. 

Asegúrese de que **PowerShell** está seleccionado en Cloud Shell y escriba:

```azurepowershell-interactive
New-AzVm -Name myVM
```

Deberá crear un nombre de usuario y una contraseña para la máquina virtual, que se usarán cuando se conecte a la máquina virtual más adelante en este tema. La contraseña debe tener entre 12 y 123 caracteres y reunir, al menos, tres de los cuatro requisitos de complejidad siguientes: contener al menos una minúscula, una mayúscula, un número y un carácter especial.

Se tarda un minuto en crear la máquina virtual y los recursos asociados. Cuando finalice, podrá ver todos los recursos que se crearon con el cmdlet [Get-AzResource](https://docs.microsoft.com/powershell/module/az.resources/get-azresource).

```azurepowershell-interactive
Get-AzResource `
    -ResourceGroupName myVMResourceGroup | Format-Table Name
```

## <a name="connect-to-the-vm"></a>Conexión a la máquina virtual

Una vez finalizada la implementación, cree una conexión del Escritorio remoto con la máquina virtual.

Use el comando [Get-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress) para devolver la dirección IP pública de la máquina virtual. Anote esta dirección IP.

```azurepowershell-interactive
Get-AzPublicIpAddress `
    -ResourceGroupName myVMResourceGroup | Select IpAddress
```

En la máquina local, abra un símbolo del sistema y use el comando **mstsc** para iniciar una sesión de escritorio remoto con la nueva máquina virtual. Reemplace el valor de &lt;publicIPAddress&gt; con la dirección IP de la máquina virtual. Cuando se le solicite, escriba el nombre de usuario y la contraseña que se le asignó a la máquina virtual durante la creación.

```
mstsc /v:<publicIpAddress>
```
## <a name="specify-different-resource-names"></a>Especificación de diferentes nombres de recursos

Puede también proporcionar nombres más descriptivos para los recursos y mantener su creación automática. Este es un ejemplo en el que hemos asignado un nombre a varios recursos para la nueva máquina virtual, incluido un nuevo grupo de recursos.

```azurepowershell-interactive
New-AzVm `
    -ResourceGroupName "myResourceGroup" `
    -Name "myVM" `
    -Location "East US" `
    -VirtualNetworkName "myVnet" `
    -SubnetName "mySubnet" `
    -SecurityGroupName "myNetworkSecurityGroup" `
    -PublicIpAddressName "myPublicIpAddress" `
    -OpenPorts 3389
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el comando [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myVM
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este tema, ha implementado una máquina virtual sencilla con New-AzVM y se ha conectado a ella mediante el protocolo de escritorio remoto. Para más información acerca de las máquinas virtuales de Azure, continúe con el tutorial de máquinas virtuales Windows.

> [!div class="nextstepaction"]
> [Tutoriales de máquinas virtuales Windows de Azure](./tutorial-manage-vm.md)
