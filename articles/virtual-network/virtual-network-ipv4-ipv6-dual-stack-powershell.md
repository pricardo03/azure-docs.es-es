---
title: Implementar una aplicación de pila doble de IPv6 en Azure virtual network - PowerShell
titlesuffix: Azure Virtual Network
description: En este artículo se muestra cómo implementa una aplicación de pila doble de IPv6 en Azure virtual network mediante Azure Powershell.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/22/2019
ms.author: kumud
ms.openlocfilehash: a9f8066896134072665c3f5b325e033b638bf094
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66001008"
---
# <a name="deploy-an-ipv6-dual-stack-application-in-azure---powershell-preview"></a>Implementar una aplicación de pila doble de IPv6 en Azure: PowerShell (versión preliminar)

Este artículo muestra cómo implementar una aplicación de doble pila (IPv4 + IPv6) en Azure que incluya una red virtual de pila doble y una subred, un equilibrador de carga con configuraciones de front-end dual (IPv4 + IPv6), las máquinas virtuales con NIC que tienen una configuración de IP dual, red grupo de seguridad y las direcciones IP públicas.

> [!Important]
> Compatibilidad con IPv6 para Azure Virtual Network está actualmente en versión preliminar pública. Esta versión preliminar se ofrece sin contrato de nivel de servicio y no es aconsejable usarla para cargas de trabajo de producción. Es posible que algunas características no sean compatibles o que tengan sus funcionalidades limitadas. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Si decide instalar y usar PowerShell localmente, este artículo requiere la versión del módulo 6.9.0 de Azure PowerShell o una versión posterior. Ejecute `Get-Module -ListAvailable Az` para buscar la versión instalada. Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-Az-ps). Si PowerShell se ejecuta localmente, también debe ejecutar `Connect-AzAccount` para crear una conexión con Azure.

## <a name="prerequisites"></a>Requisitos previos
Antes de implementar una aplicación de pila doble en Azure, debe configurar su suscripción para esta característica de vista previa con Azure PowerShell siguientes:

Registrar la manera siguiente:
```azurepowershell
Register-AzProviderFeature -FeatureName AllowIPv6VirtualNetwork -ProviderNamespace Microsoft.Network
```
Se tarda hasta 30 minutos en completar el registro de características. Puede comprobar su estado de registro ejecutando el siguiente comando de PowerShell de Azure: Compruebe en el registro como sigue:
```azurepowershell
Get-AzProviderFeature -FeatureName AllowIPv6VirtualNetwork -ProviderNamespace Microsoft.Network
```
Una vez completado el registro, ejecute el siguiente comando:

```azurepowershell
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Para poder crear la red virtual de doble pila, debe crear un grupo de recursos con [New AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). En el ejemplo siguiente se crea un grupo de recursos denominado *myRGDualStack* en el *east us* ubicación:

```azurepowershell-interactive
   $rg = New-AzResourceGroup `
  -ResourceGroupName "dsRG1"  `
  -Location "east us"
```

## <a name="create-ipv4-and-ipv6-public-ip-addresses"></a>Creación de direcciones IP públicas IPv4 e IPv6
Para obtener acceso a las máquinas virtuales desde Internet, necesita direcciones IP públicas IPv4 e IPv6 para el equilibrador de carga. Crear direcciones IP públicas con [New AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress). En el ejemplo siguiente se crea IPv4 e IPv6 dirección IP pública denominada *dsPublicIP_v4* y *dsPublicIP_v6* en el *dsRG1* grupo de recursos:

```azurepowershell-interactive
$PublicIP_v4 = New-AzPublicIpAddress `
  -Name "dsPublicIP_v4" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Dynamic `
  -IpAddressVersion IPv4
  
$PublicIP_v6 = New-AzPublicIpAddress `
  -Name "dsPublicIP_v6" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Dynamic `
  -IpAddressVersion IPv6
```
Para obtener acceso a las máquinas virtuales mediante una conexión RDP, cree un direcciones IP públicas de IPV4 para las máquinas virtuales con [New AzPublicIpAddress](/powershell/module/az.network/new-azpublicipaddress).

```azurepowershell-interactive
  $RdpPublicIP_1 = New-AzPublicIpAddress `
  -Name "RdpPublicIP_1" `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -AllocationMethod Dynamic `
  -IpAddressVersion IPv4
  
  $RdpPublicIP_2 = New-AzPublicIpAddress `
   -Name "RdpPublicIP_2" `
   -ResourceGroupName $rg.ResourceGroupName `
   -Location $rg.Location  `
   -AllocationMethod Dynamic `
   -IpAddressVersion IPv4
```

## <a name="create-basic-load-balancer"></a>Creación de un equilibrador de carga básico

En esta sección, configurará dos front-end IP (IPv4 e IPv6) y el grupo de direcciones de back-end del equilibrador de carga y, a continuación, crear un equilibrador de carga básico.

### <a name="create-front-end-ip"></a>Creación de una dirección IP de front-end

Cree una dirección IP de front-end con [New-AzLoadBalancerFrontendIpConfig](/powershell/module/az.network/new-azloadbalancerfrontendipconfig). El ejemplo siguiente crea el front-end de IPv4 e IPv6 configuraciones de IP denominadas *dsLbFrontEnd_v4* y *dsLbFrontEnd_v6*:

```azurepowershell-interactive
$frontendIPv4 = New-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v4" `
  -PublicIpAddress $PublicIP_v4

$frontendIPv6 = New-AzLoadBalancerFrontendIpConfig `
  -Name "dsLbFrontEnd_v6" `
  -PublicIpAddress $PublicIP_v6

```

### <a name="configure-back-end-address-pool"></a>Configuración del grupo de direcciones de back-end

Cree un grupo de direcciones de back-end con [New-AzLoadBalancerBackendAddressPoolConfig](/powershell/module/az.network/new-azloadbalancerbackendaddresspoolconfig). En el resto de los pasos, las máquinas virtuales se conectan a este grupo back-end. El ejemplo siguiente crea los grupos de direcciones de back-end llamados *dsLbBackEndPool_v4* y *dsLbBackEndPool_v6* para incluir las máquinas virtuales con configuraciones de IPV4 e IPv6 NIC:

```azurepowershell-interactive
$backendPoolv4 = New-AzLoadBalancerBackendAddressPoolConfig `
-Name "dsLbBackEndPool_v4"

$backendPoolv6 = New-AzLoadBalancerBackendAddressPoolConfig `
-Name "dsLbBackEndPool_v6"
```

### <a name="create-a-load-balancer-rule"></a>Creación de una regla de equilibrador de carga

Las reglas de equilibrador de carga se utilizan para definir cómo se distribuye el tráfico a las máquinas virtuales. Defina la configuración de la IP de front-end para el tráfico entrante y el grupo de IP de back-end para el tráfico entrante, junto con los puertos de origen y destino requeridos. Para que correcto está seguro de que solo las máquinas virtuales reciben tráfico, también puede definir un sondeo de estado. Equilibrador de carga básico usa un sondeo de IPv4 para evaluar el estado de los puntos de conexión IPv4 e IPv6 en las máquinas virtuales. Equilibrador de carga estándar incluye compatibilidad con sondeos de estado de IPv6 explícitamente.

Cree una regla del equilibrador de carga con [Add-AzLoadBalancerRuleConfig](/powershell/module/az.network/add-azloadbalancerruleconfig). En el ejemplo siguiente se crea reglas de equilibrador de carga denominadas *dsLBrule_v4* y *dsLBrule_v6* y equilibra el tráfico en *TCP* puerto *80* a las configuraciones de IP front-end de IPv4 e IPv6:

```azurepowershell-interactive
$lbrule_v4 = New-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v4" `
  -FrontendIpConfiguration $frontendIPv4 `
  -BackendAddressPool $backendPoolv4 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80

$lbrule_v6 = New-AzLoadBalancerRuleConfig `
  -Name "dsLBrule_v6" `
  -FrontendIpConfiguration $frontendIPv6 `
  -BackendAddressPool $backendPoolv6 `
  -Protocol Tcp `
  -FrontendPort 80 `
  -BackendPort 80
```

### <a name="create-load-balancer"></a>Crear equilibrador de carga

Cree un equilibrador de carga básico con [New-AzLoadBalancer](/powershell/module/az.network/new-azloadbalancer). En el ejemplo siguiente se crea un público básico equilibrador de carga denominado *myLoadBalancer* con la dirección IP de front-end IPv4 e IPv6 configuraciones, grupos de back-end y reglas de equilibrio de carga que creó en los pasos anteriores:

```azurepowershell-interactive
$lb = New-AzLoadBalancer `
-ResourceGroupName $rg.ResourceGroupName `
-Location $rg.Location  `
-Name "MyLoadBalancer" `
-Sku "Basic" `
-FrontendIpConfiguration $frontendIPv4,$frontendIPv6 `
-BackendAddressPool $backendPoolv4,$backendPoolv6 `
-LoadBalancingRule $lbrule_v4,$lbrule_v6

```

## <a name="create-network-resources"></a>Crear recursos de red
Antes de implementar algunas máquinas virtuales y poder probar el equilibrador, debe crear los recursos de red auxiliares - conjunto de disponibilidad, el grupo de seguridad de red, la red virtual y NIC virtuales. 
### <a name="create-an-availability-set"></a>Crear un conjunto de disponibilidad
Para mejorar la alta disponibilidad de la aplicación, coloque las máquinas virtuales en un conjunto de disponibilidad.

Cree un conjunto de disponibilidad con [New-AzAvailabilitySet](/powershell/module/az.compute/new-azavailabilityset). En el ejemplo siguiente se crea un conjunto de disponibilidad denominado *myAvailabilitySet*:

```azurepowershell-interactive
$avset = New-AzAvailabilitySet `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsAVset" `
  -PlatformFaultDomainCount 2 `
  -PlatformUpdateDomainCount 2 `
  -Sku aligned
```

### <a name="create-network-security-group"></a>Crear grupo de seguridad de red

Cree un grupo de seguridad de red para las reglas que rige la comunicación entrante y saliente de la red virtual.

#### <a name="create-a-network-security-group-rule-for-port-3389"></a>Creación de una regla de grupo de seguridad de red para el puerto 3389

Cree una regla de grupo de seguridad de red para permitir conexiones RDP en el puerto 3389 con [New-AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell-interactive
$rule1 = New-AzNetworkSecurityRuleConfig `
-Name 'myNetworkSecurityGroupRuleRDP' `
-Description 'Allow RDP' `
-Access Allow `
-Protocol Tcp `
-Direction Inbound `
-Priority 100 `
-SourceAddressPrefix * `
-SourcePortRange * `
-DestinationAddressPrefix * `
-DestinationPortRange 3389
```
#### <a name="create-a-network-security-group-rule-for-port-80"></a>Creación de una regla de grupo de seguridad de red para el puerto 80

Crear una regla de grupo de seguridad de red para permitir conexiones a internet a través del puerto 80 con [New AzNetworkSecurityRuleConfig](/powershell/module/az.network/new-aznetworksecurityruleconfig).

```azurepowershell-interactive
$rule2 = New-AzNetworkSecurityRuleConfig `
  -Name 'myNetworkSecurityGroupRuleHTTP' `
  -Description 'Allow HTTP' `
  -Access Allow `
  -Protocol Tcp `
  -Direction Inbound `
  -Priority 200 `
  -SourceAddressPrefix * `
  -SourcePortRange 80 `
  -DestinationAddressPrefix * `
  -DestinationPortRange 80
```
#### <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red

Cree un grupo de seguridad de red con [New-AzNetworkSecurityGroup](/powershell/module/az.network/new-aznetworksecuritygroup).

```azurepowershell-interactive
$nsg = New-AzNetworkSecurityGroup `
-ResourceGroupName $rg.ResourceGroupName `
-Location $rg.Location  `
-Name "dsNSG1"  `
-SecurityRules $rule1,$rule2
```
### <a name="create-a-virtual-network"></a>Creación de una red virtual

Cree una red virtual con [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork). En el ejemplo siguiente se crea una red virtual denominada con una subred llamada *myVnet* con *mySubnet*:

```azurepowershell-interactive
# Create dual stack subnet
$subnet = New-AzVirtualNetworkSubnetConfig `
-Name "dsSubnet" `
-AddressPrefix "10.0.0.0/24","ace:cab:deca:deed::/64"

# Create the virtual network
$vnet = New-AzVirtualNetwork `
  -ResourceGroupName $rg.ResourceGroupName `
  -Location $rg.Location  `
  -Name "dsVnet" `
  -AddressPrefix "10.0.0.0/16","ace:cab:deca::/48"  `
  -Subnet $subnet
```

### <a name="create-nics"></a>Creación de tarjetas NIC

Cree tarjetas NIC virtuales con [New AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface). En el ejemplo siguiente se crea dos NIC virtuales ambos con configuraciones de IPv4 e IPv6. (Una NIC virtual para cada máquina virtual que cree para la aplicación en los pasos siguientes).

```azurepowershell-interactive
  $Ip4Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp4Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv4 `
    -LoadBalancerBackendAddressPool $backendPoolv4 `
    -PublicIpAddress  $RdpPublicIP_1
    
  $Ip6Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp6Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv6 `
    -LoadBalancerBackendAddressPool $backendPoolv6
    
  $NIC_1 = New-AzNetworkInterface `
    -Name "dsNIC1" `
    -ResourceGroupName $rg.ResourceGroupName `
    -Location $rg.Location  `
    -NetworkSecurityGroupId $nsg.Id `
    -IpConfiguration $Ip4Config,$Ip6Config 
    
  $Ip4Config=New-AzNetworkInterfaceIpConfig `
    -Name dsIp4Config `
    -Subnet $vnet.subnets[0] `
    -PrivateIpAddressVersion IPv4 `
    -LoadBalancerBackendAddressPool $backendPoolv4 `
    -PublicIpAddress  $RdpPublicIP_2  

  $NIC_2 = New-AzNetworkInterface `
    -Name "dsNIC2" `
    -ResourceGroupName $rg.ResourceGroupName `
    -Location $rg.Location  `
    -NetworkSecurityGroupId $nsg.Id `
    -IpConfiguration $Ip4Config,$Ip6Config 

```

### <a name="create-virtual-machines"></a>Creación de máquinas virtuales

Establezca un nombre de usuario de administrador y una contraseña para las máquinas virtuales con [Get-Credential](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.security/Get-Credential):

```azurepowershell-interactive
$cred = get-credential -Message "DUAL STACK VNET SAMPLE:  Please enter the Administrator credential to log into the VMs."
```

Ahora puede crear las máquinas virtuales con [New-AzVM](/powershell/module/az.compute/new-azvm). En el ejemplo siguiente, se crean dos máquinas virtuales y los componentes de red virtual necesarios, si aún no existen. 

```azurepowershell-interactive
$vmsize = "Standard_A2"
$ImagePublisher = "MicrosoftWindowsServer"
$imageOffer = "WindowsServer"
$imageSKU = "2016-Datacenter"

$vmName= "dsVM1"
$VMconfig1 = New-AzVMConfig -VMName $vmName -VMSize $vmsize -AvailabilitySetId $avset.Id 3> $null | Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent 3> $null | Set-AzVMSourceImage -PublisherName $ImagePublisher -Offer $imageOffer -Skus $imageSKU -Version "latest" 3> $null | Set-AzVMOSDisk -Name "$vmName.vhd" -CreateOption fromImage  3> $null | Add-AzVMNetworkInterface -Id $NIC_1.Id  3> $null 
$VM1 = New-AzVM -ResourceGroupName $rg.ResourceGroupName  -Location $rg.Location  -VM $VMconfig1 

$vmName= "dsVM2"
$VMconfig2 = New-AzVMConfig -VMName $vmName -VMSize $vmsize -AvailabilitySetId $avset.Id 3> $null | Set-AzVMOperatingSystem -Windows -ComputerName $vmName -Credential $cred -ProvisionVMAgent 3> $null | Set-AzVMSourceImage -PublisherName $ImagePublisher -Offer $imageOffer -Skus $imageSKU -Version "latest" 3> $null | Set-AzVMOSDisk -Name "$vmName.vhd" -CreateOption fromImage  3> $null | Add-AzVMNetworkInterface -Id $NIC_2.Id  3> $null 
$VM2 = New-AzVM -ResourceGroupName $rg.ResourceGroupName  -Location $rg.Location  -VM $VMconfig2
```

## <a name="determine-ip-addresses-of-the-ipv4-and-ipv6-endpoints"></a>Determinar las direcciones IP de los puntos de conexión IPv4 e IPv6
Obtener todos los objetos de interfaz de red en el grupo de recursos para resumir las direcciones IP usado en esta implementación con `get-AzNetworkInterface`. Asimismo, obtenga las direcciones de front-end del equilibrador de carga de los puntos de conexión IPv4 e IPv6 con `get-AzpublicIpAddress`.

```azurepowershell-interactive
$rgName= "dsRG1"
$NICsInRG= get-AzNetworkInterface -resourceGroupName $rgName 
write-host `nSummary of IPs in this Deployment: 
write-host ******************************************
foreach ($NIC in $NICsInRG) {
 
    $VMid= $NIC.virtualmachine.id 
    $VMnamebits= $VMid.split("/") 
    $VMname= $VMnamebits[($VMnamebits.count-1)] 
    write-host `nPrivate IP addresses for $VMname 
    $IPconfigsInNIC= $NIC.IPconfigurations 
    foreach ($IPconfig in $IPconfigsInNIC) {
 
        $IPaddress= $IPconfig.privateipaddress 
        write-host "    "$IPaddress 
        IF ($IPconfig.PublicIpAddress.ID) {
 
            $IDbits= ($IPconfig.PublicIpAddress.ID).split("/")
            $PipName= $IDbits[($IDbits.count-1)]
            $PipObject= get-azPublicIpAddress -name $PipName -resourceGroup $rgName
            write-host "    "RDP address:  $PipObject.IpAddress
                 }
         }
 }
 
 
 
  write-host `nPublic IP addresses on Load Balancer:
 
  (get-AzpublicIpAddress -resourcegroupname $rgName | where { $_.name -notlike "RdpPublicIP*" }).IpAddress
```
La siguiente ilustración muestra una salida de ejemplo que muestra las direcciones privadas de IPv4 e IPv6 de las dos máquinas virtuales y las direcciones IP IPv4 e IPv6 de front-end del equilibrador de carga.

![Resumen IP de la implementación de aplicaciones de doble pila (IPv4/IPv6) en Azure](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-application-summary.png)

## <a name="view-ipv6-dual-stack-virtual-network-in-azure-portal"></a>Ver la red virtual de pila doble de IPv6 en Azure portal
Puede ver la red virtual de pila doble de IPv6 en Azure portal como sigue:
1. En la barra de búsqueda, escriba *dsVnet*.
2. Cuando aparezca la opción **myVirtualNetwork** en los resultados de la búsqueda, selecciónela. Esto inicia el **Introducción** página de la red virtual de pila doble llamada *dsVnet*. La red virtual de la pila dual muestra las dos NIC con las configuraciones de IPv4 e IPv6 ubicadas en la subred de pila doble denominada *dsSubnet*.

  ![Red virtual de pila doble de IPv6 en Azure](./media/virtual-network-ipv4-ipv6-dual-stack-powershell/dual-stack-vnet.png)

> [!NOTE]
> IPv6 para Azure virtual network está disponible en el portal de Azure en solo lectura para esta versión preliminar.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando ya no se necesiten, puede usar el comando [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurepowershell-interactive
Remove-AzResourceGroup -Name dsRG1
```

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado un equilibrador de carga básica con una configuración de IP de frontend dual (IPv4 e IPv6). También creó un dos máquinas virtuales que se incluyen las NIC con configuraciones de IP duales (IPV4 + IPv6) que se han agregado al grupo de back-end del equilibrador de carga. Para más información sobre la compatibilidad con IPv6 en redes virtuales de Azure, consulte [¿qué es IPv6 para Azure Virtual Network?](ipv6-overview.md)