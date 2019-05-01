---
title: Enrutamiento del tráfico de red en Azure PowerShell | Microsoft Docs
description: En este artículo, aprenderá a enrutar el tráfico de red con una tabla de rutas mediante PowerShell.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: bbb614f9a540131ea25f53b8d566a4a5ee8c197b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64692274"
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>Enrutamiento del tráfico de red con una tabla de rutas mediante PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

De forma predeterminada, Azure enruta automáticamente el tráfico entre todas las subredes de una red virtual. Sin embargo, puede crear sus propias rutas para invalidar las predeterminadas de Azure. La posibilidad de crear rutas personalizadas resulta de utilidad si, por ejemplo, quiere enrutar el tráfico entre subredes por medio de una aplicación virtual de red (NVA). En este artículo, aprenderá a:

* Creación de una tabla de rutas
* Creación de una ruta
* Creación de una red virtual con varias subredes
* Asociación de una tabla de rutas a una subred
* Creación de una aplicación virtual de red para enrutar el tráfico
* Implementación de máquinas virtuales (VM) en subredes diferentes
* Enrutamiento del tráfico desde una subred a otra a través de una aplicación virtual de red

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar PowerShell localmente, este artículo requiere la versión 1.0.0 del módulo de Azure PowerShell o una versión posterior. Ejecute `Get-Module -ListAvailable Az` para buscar la versión instalada. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

## <a name="create-a-route-table"></a>Creación de una tabla de rutas

Para poder crear una tabla de rutas, cree un grupo de recursos con [New AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). En el ejemplo siguiente se crea un grupo de recursos denominado *myResourceGroup* para todos los recursos creados en este artículo.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

Crear una tabla de rutas con [New AzRouteTable](/powershell/module/az.network/new-azroutetable). En el ejemplo siguiente se crea una tabla de rutas denominada *myRouteTablePublic*.

```azurepowershell-interactive
$routeTablePublic = New-AzRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>Creación de una ruta

Cree una ruta al recuperar el objeto de tabla de ruta con [Get AzRouteTable](/powershell/module/az.network/get-azroutetable), cree una ruta con [agregar AzRouteConfig](/powershell/module/az.network/add-azrouteconfig), a continuación, escriba la configuración de ruta en la tabla de rutas con [ Conjunto AzRouteTable](/powershell/module/az.network/set-azroutetable).

```azurepowershell-interactive
Get-AzRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzRouteTable
```

## <a name="associate-a-route-table-to-a-subnet"></a>Asociación de una tabla de rutas a una subred

Para poder asociar una tabla de rutas a una subred, debe crear una red virtual y una subred. Cree una red virtual con [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). En el ejemplo siguiente se crea una red virtual denominada *myVirtualNetwork* con el prefijo de dirección *10.0.0.0/16*.

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

Cree tres subredes mediante la creación de tres configuraciones de subred con [New AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig). En el ejemplo siguiente se crean tres configuraciones de subred para las subredes *Pública*, *Privada* y *DMZ*:

```azurepowershell-interactive
$subnetConfigPublic = Add-AzVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

Escribir las configuraciones de subred en la red virtual con [conjunto AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork), que crea las subredes de la red virtual:

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

Asociar el *myRouteTablePublic* tabla de rutas a la *pública* subred con [conjunto AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) y, a continuación, escriba la configuración de subred a la red virtual con [conjunto AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork).

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzVirtualNetwork
```

## <a name="create-an-nva"></a>Creación de una aplicación virtual de red

Una aplicación virtual de red es una máquina virtual que realiza una función de red, como el enrutamiento, el firewall o la optimización de la WAN.

Antes de crear una máquina virtual, cree una interfaz de red.

### <a name="create-a-network-interface"></a>Crear una interfaz de red

Antes de crear una interfaz de red, tiene que recuperar el virtual con identificador de red [Get AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork), a continuación, el Id. de subred con [Get AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig). Cree una interfaz de red con [New AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) en el *DMZ* subred con el reenvío IP habilitado:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

### <a name="create-a-vm"></a>Crear una VM

Para crear una máquina virtual y asociarle una interfaz de red existente a ella, primero debe crear una configuración de máquina virtual con [New AzVMConfig](/powershell/module/az.compute/new-azvmconfig). La configuración incluye la interfaz de red que creó en el paso anterior. Cuando se le pida un nombre de usuario y una contraseña, seleccione el nombre de usuario y la contraseña con los que quiere iniciar sesión en la máquina virtual.

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the VM."

# Create a VM configuration.
$vmConfig = New-AzVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzVMNetworkInterface -Id $nic.Id
```

Crear la máquina virtual mediante la configuración de máquina virtual con [New-AzVM](/powershell/module/az.compute/new-azvm). En el ejemplo siguiente se crea una máquina virtual denominada *myVmNva*.

```azurepowershell-interactive
$vmNva = New-AzVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

La opción `-AsJob` crea la máquina virtual en segundo plano, así que puede continuar con el siguiente paso.

## <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Cree dos máquinas virtuales en la red virtual para que pueda validar que el tráfico que procede de la subred *Pública* se enruta a la subred *Privada* mediante la aplicación virtual de red de un paso posterior.

Crear una máquina virtual en el *pública* subred con [New-AzVM](/powershell/module/az.compute/new-azvm). En el ejemplo siguiente se crea una máquina virtual llamada *myVmPublic* en la subred *Public* de la red virtual *myVirtualNetwork*.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPublic" `
  -AsJob
```

Cree una máquina virtual en la subred *Private*.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPrivate"
```

La máquina virtual tarda en crearse unos minutos. No continúe con el paso siguiente hasta que se cree la máquina virtual y Azure devuelva la salida a PowerShell.

## <a name="route-traffic-through-an-nva"></a>Enrutamiento del tráfico a través de una aplicación virtual de red

Use [Get AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) para devolver la dirección IP pública de la *myVmPrivate* máquina virtual. En el siguiente ejemplo se devuelve la dirección IP pública de la máquina virtual *myVmPrivate*:

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

Ejecute el comando siguiente en el equipo local para crear una sesión de Escritorio remoto con la máquina virtual *myVmPrivate*. Reemplace `<publicIpAddress>` con la dirección IP que devolvió el comando anterior.

```
mstsc /v:<publicIpAddress>
```

Abra el archivo RDP descargado. Cuando se le pida, seleccione **Conectar**.

Escriba el nombre de usuario y la contraseña que especificó al crear la máquina virtual (puede que deba seleccionar **Más opciones** y luego **Usar una cuenta diferente**, para especificar las credenciales que escribió cuando creó la máquina virtual). A continuación, seleccione **Aceptar**. Puede recibir una advertencia de certificado durante el proceso de inicio de sesión. Seleccione **Sí** para continuar con la conexión.

En un paso posterior, el `tracert.exe` comando se usa para probar el enrutamiento. Tracert usa el Protocolo de mensajes de control de Internet (ICMP), que se deniega a través del Firewall de Windows. Para habilitar ICMP mediante el Firewall de Windows, escriba el comando siguiente desde PowerShell en la máquina virtual *myVmPrivate*:

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
```

Aunque en este artículo se usa trace route para probar el enrutamiento, no es recomendable permitir el uso de ICMP a través del Firewall de Windows en las implementaciones de producción.

El reenvío IP se habilitó dentro de Azure para la interfaz de red de la VM en Habilitación del reenvío IP. Dentro de la máquina virtual, también el sistema operativo o una aplicación en ejecución deben poder reenviar el tráfico de red. Habilite el reenvío IP en el sistema operativo de *myVmNva*.

Desde un símbolo del sistema de la máquina virtual *myVmPrivate*, conéctese mediante Escritorio remoto a *myVmNva*:

``` 
mstsc /v:myvmnva
```

Para habilitar el reenvío IP dentro del sistema operativo, escriba el siguiente comando en PowerShell desde la máquina virtual *myVmNva*:

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```

Reinicie la máquina virtual *myVmNva*, lo que desconectará también la sesión de Escritorio remoto.

Mientras sigue conectado a la máquina virtual *myVmPrivate* y una vez que se reinicie la máquina virtual *myVmNva*, cree una sesión de Escritorio remoto en la máquina virtual *myVmPublic*:

``` 
mstsc /v:myVmPublic
```

Para permitir el uso de ICMP a través del Firewall de Windows, escriba el comando siguiente desde PowerShell en la máquina virtual *myVmPrivate*:

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

Para probar el enrutamiento del tráfico de red desde *myVmPublic* hasta *myVmPrivate*, escriba el siguiente comando de PowerShell en la máquina virtual *myVmPublic*:

```
tracert myVmPrivate
```

La respuesta es similar al siguiente ejemplo:

```
Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:

1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4

Trace complete.
```

Puede ver que el primer salto es 10.0.2.4, que es la dirección IP privada de la aplicación virtual de red. El segundo salto es 10.0.1.4, la dirección IP privada de la máquina virtual *myVmPrivate*. La ruta agregada a la tabla de rutas *myRouteTablePublic* y asociada a la subred *Pública* hizo que Azure enrutara el tráfico mediante la NVA, en lugar de a la subred *Privada* directamente.

Cierre la sesión de Escritorio remoto a la máquina virtual *myVmPublic*, que aún le deja conectado a la máquina virtual *myVmPrivate*.

Para probar el enrutamiento del tráfico de red desde *myVmPrivate* hasta *myVmPublic*, escriba el siguiente comando en un símbolo del sistema de la máquina virtual *myVmPrivate*:

```
tracert myVmPublic
```

La respuesta es similar al siguiente ejemplo:

```
Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:

1     1 ms     1 ms     1 ms  10.0.0.4

Trace complete.
```

Puede ver si el tráfico se enruta directamente de la máquina virtual *myVmPrivate* a la máquina virtual *myVmPublic*. De forma predeterminada, Azure enruta el tráfico directamente entre subredes.

Cierre la sesión de Escritorio remoto a la máquina virtual *myVmPrivate*.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no es necesario, utilice [Remove-AzResourcegroup](/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos y todos los recursos que contiene.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, creó una tabla de rutas y la asoció a una subred. Creó una aplicación virtual de red sencilla que enrutó el tráfico desde una subred pública hasta una subred privada. Implemente una variedad de aplicaciones virtuales de red que realicen funciones de red, como firewall y optimización de la WAN, desde [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Para más información acerca del enrutamiento, consulte [Introducción al enrutamiento](virtual-networks-udr-overview.md) y [Administración de una tabla de rutas](manage-route-table.md).

Aunque puede implementar muchos recursos de Azure en una red virtual, no es el caso de los recursos de algunos servicios de PaaS de Azure. Pero puede restringir el acceso a los recursos de algunos servicios de PaaS de Azure solo al tráfico que procede de una subred de una red virtual. Para saber cómo hacerlo, consulte [Restricción del acceso de red a los recursos de PaaS](tutorial-restrict-network-access-to-resources-powershell.md).