---
title: Creación de una red virtual mediante Azure PowerShell
titlesuffix: Azure Virtual Network
description: En esta guía de inicio rápido, aprenderá a crear una red virtual mediante Azure Portal. Una red virtual permite que los recursos de Azure, como máquinas virtuales, se comunican de manera privada entre sí y con Internet.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.service: virtual-network
ms.devlang: ''
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: jdial
ms.openlocfilehash: ade8329e6e42fae9f3232617488a6d4a69f8ef1f
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54437392"
---
# <a name="quickstart-create-a-virtual-network-using-powershell"></a>Inicio rápido: Creación de una red virtual mediante PowerShell

Una red virtual permite que los recursos de Azure, como las máquinas virtuales (VM), se comuniquen entre sí de forma privada y con Internet. En esta guía de inicio rápido aprenderá a crear una red virtual. Después de crear una red virtual, implementará dos máquinas virtuales en la red virtual. Luego, conéctese a las máquinas virtuales desde Internet y, de manera privada, comuníquese a través de la red virtual.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) ahora.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-powershell.md)]

Si decide en su lugar instalar y usar PowerShell de manera local, este inicio rápido necesita que use la versión 5.4.1. o posterior del módulo de AzureRM PowerShell. Ejecute `Get-Module -ListAvailable AzureRM` para ver cuál es la versión instalada. Consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps) para ver información sobre la instalación y la actualización.

Por último, si ejecuta PowerShell de manera local, también deberá ejecutar `Connect-AzureRmAccount`. Ese comando crea una conexión con Azure.

## <a name="create-a-resource-group-and-a-virtual-network"></a>Creación de un grupo de recursos y una red virtual

Debe llevar a cabo una serie de pasos para configurar el grupo de recursos y la red virtual.

### <a name="create-the-resource-group"></a>Creación del grupo de recursos

Para poder crear una red virtual, debe crear un grupo de recursos que hospede la red virtual. Cree un grupo de recursos con [New-AzureRmResourceGroup](/powershell/module/AzureRM.Resources/New-AzureRmResourceGroup). En este ejemplo, se crea un grupo de recursos denominado *myResourceGroup* en la ubicación *eastus*:

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

### <a name="create-the-virtual-network"></a>Crear la red virtual

Cree una red virtual con [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). En este ejemplo, se crea una red virtual predeterminada denominada *myVirtualNetwork* en la ubicación *EastUS*:

```azurepowershell-interactive
$virtualNetwork = New-AzureRmVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

### <a name="add-a-subnet"></a>Incorporación de una subred

Azure implementa recursos en una subred dentro de una red virtual, por lo que es necesario crear una subred. Cree una configuración de subred denominada *default* con [Add-AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig):

```azurepowershell-interactive
$subnetConfig = Add-AzureRmVirtualNetworkSubnetConfig `
  -Name default `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork
```

### <a name="associate-the-subnet-to-the-virtual-network"></a>Asociación de la subred a la red virtual

Puede escribir la configuración de la subred en la red virtual con [Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/Set-AzureRmVirtualNetwork). Este comando crea la subred:

```azurepowershell-interactive
$virtualNetwork | Set-AzureRmVirtualNetwork
```

## <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Cree dos máquinas virtuales en la red virtual.

### <a name="create-the-first-vm"></a>Creación de la primera máquina virtual

Cree la primera máquina virtual con [New-AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm). Cuando ejecute el comando siguiente, se le pedirán las credenciales. Escriba un nombre de usuario y una contraseña para la máquina virtual:

```azurepowershell-interactive
New-AzureRmVm `
    -ResourceGroupName "myResourceGroup" `
    -Location "East US" `
    -VirtualNetworkName "myVirtualNetwork" `
    -SubnetName "default" `
    -Name "myVm1" `
    -AsJob
```

La opción `-AsJob` crea la máquina virtual en segundo plano. Puede continuar al paso siguiente.

Cuando Azure empieza a crear la máquina virtual en segundo plano, verá algo como esto:

```powershell
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
1      Long Running... AzureLongRun... Running       True            localhost            New-AzureRmVM
```

### <a name="create-the-second-vm"></a>Creación de la segunda máquina virtual

Cree la segunda máquina virtual con este comando:

```azurepowershell-interactive
New-AzureRmVm `
  -ResourceGroupName "myResourceGroup" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "default" `
  -Name "myVm2"
```

Tendrá que crear otro usuario y otra contraseña. Azure tarda unos minutos en crear la máquina virtual.

> [!IMPORTANT]
> No vaya al próximo paso hasta que Azure haya terminado.  Sabrá que terminó cuando devuelva la salida a PowerShell.

## <a name="connect-to-a-vm-from-the-internet"></a>Conexión a una máquina virtual desde Internet

Use [Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress) para devolver la dirección IP pública de una máquina virtual. Este ejemplo devuelve la dirección IP pública de la máquina virtual *myVm1*:

```azurepowershell-interactive
Get-AzureRmPublicIpAddress `
  -Name myVm1 `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Abra un símbolo del sistema en el equipo local. Ejecute el comando `mstsc`. Reemplace `<publicIpAddress>` por la dirección IP pública que se devolvió en el último paso:

> [!NOTE]
> Si ejecutó estos comandos desde un símbolo del sistema de PowerShell en el equipo local y usa la versión 5.4.1 o posterior del módulo de AzureRM PowerShell, puede seguir en esa interfaz.

```cmd
mstsc /v:<publicIpAddress>
```

Un archivo de Protocolo de escritorio remoto (*.rdp*) se descarga en el equipo y se abre el Escritorio remoto.

1. Cuando se le pida, seleccione **Conectar**.

1. Escriba el nombre de usuario y la contraseña que especificó al crear la máquina virtual.

    > [!NOTE]
    > Es posible que tenga que seleccionar **Más opciones** > **Usar otra cuenta** para especificar las credenciales que escribió al crear la máquina virtual.

1. Seleccione **Aceptar**.

1. Puede que reciba una advertencia de certificado. Si la recibe, seleccione **Sí** o **Continuar**.

## <a name="communicate-between-vms"></a>Comunicarse entre máquinas virtuales

1. En el Escritorio remoto de *myVm1*, abra PowerShell.

1. Escriba `ping myVm2`.

    Verá algo similar a lo siguiente:

    ```powershell
    PS C:\Users\myVm1> ping myVm2

    Pinging myVm2.ovvzzdcazhbu5iczfvonhg2zrb.bx.internal.cloudap
    Request timed out.
    Request timed out.
    Request timed out.
    Request timed out.

    Ping statistics for 10.0.0.5:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),
    ```

    Se produce un error de ping, porque usa el Protocolo de mensajes de control de Internet (ICMP). De manera predeterminada, el protocolo ICMP no puede atravesar el Firewall de Windows.

1. Para permitir que *myVm2* haga ping a *myVm1* en un paso posterior, escriba este comando:

    ```powershell
    New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
    ```

    Ese comando permite una conexión entrante de ICMP a través del Firewall de Windows.

1. Cierre la conexión de Escritorio remoto a *myVm1*.

1. Repita los pasos que aparecen en [Conexión a una máquina virtual desde Internet](#connect-to-a-vm-from-the-internet). Esta vez, conéctese a *myVm2*.

1. En un símbolo del sistema de la máquina virtual *myVm2*, escriba `ping myvm1`.

    Verá algo similar a lo siguiente:

    ```cmd
    C:\windows\system32>ping myVm1

    Pinging myVm1.e5p2dibbrqtejhq04lqrusvd4g.bx.internal.cloudapp.net [10.0.0.4] with 32 bytes of data:
    Reply from 10.0.0.4: bytes=32 time=2ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128
    Reply from 10.0.0.4: bytes=32 time<1ms TTL=128

    Ping statistics for 10.0.0.4:
        Packets: Sent = 4, Received = 4, Lost = 0 (0% loss),
    Approximate round trip times in milli-seconds:
        Minimum = 0ms, Maximum = 2ms, Average = 0ms
    ```

    Recibe respuestas de *myVm1*, porque permitió ICMP a través del Firewall de Windows en la máquina virtual *myVm1* en un paso anterior.

1. Cierre la conexión de Escritorio remoto a *myVm2*.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado con la red virtual y las máquinas virtuales, use [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos y todos los recursos que tiene:

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

En esta guía de inicio rápido, ha creado una red virtual predeterminada y dos máquinas virtuales. Se ha conectado a una máquina virtual desde Internet y se ha comunicado de forma privada entre las dos máquinas virtuales. Para más información sobre la configuración de red virtual, consulte [Administración de una red virtual](manage-virtual-network.md).

Azure permite que haya comunicación privada sin restricciones entre las máquinas virtuales. De manera predeterminada, Azure, solo permite conexiones de Escritorio remoto entrantes a las máquinas virtuales Windows desde Internet. Para más información sobre cómo configurar distintos tipos de comunicaciones de red de VM, vaya al tutorial sobre el [filtrado del tráfico de red](tutorial-filter-network-traffic.md).
