---
title: Crear una puerta de enlace de red virtual con redundancia de zona en Azure Availability Zones | Microsoft Docs
description: Implementar puertas de enlace de VPN Gateway y ExpressRoute en Availability Zones
services: vpn-gateway
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: a0a06ff79d1a48e8fbbc13a8e2410817c020d9a9
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54430856"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones"></a>Crear una puerta de enlace de red virtual con redundancia de zona en Azure Availability Zones

Puede implementar puertas de enlace de VPN Gateway y ExpressRoute en Azure Availability Zones. Esto aporta una mayor disponibilidad, escalabilidad y resistencia a las puertas de enlace de red virtual. Implementar puertas de enlace en Azure Availability Zones separa de forma física y lógica las puertas de enlace dentro de una región, y protege la conectividad de red local en Azure de errores de nivel de zona. Para obtener más información, consulte [About zone-redundant virtual network gateways](about-zone-redundant-vnet-gateways.md) (Acerca de las puertas de enlace de red virtual con redundancia de zona) y [About Azure Availability Zones](../availability-zones/az-overview.md) (Acerca de Azure Availability Zones).

## <a name="before-you-begin"></a>Antes de empezar

Puede usar una instalación local de PowerShell en su equipo o Azure Cloud Shell. Si decide instalar y usar PowerShell de forma local, esta característica requiere la versión más reciente del módulo de PowerShell.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>Para usar PowerShell de forma local

Si está usando PowerShell localmente en el equipo en lugar de usar Cloud Shell, debe instalar el módulo de PowerShell 6.1.1 o versiones posteriores. Para comprobar qué versión de PowerShell tiene instalada, use el siguiente comando:

```azurepowershell
Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
```

Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="variables"></a>1. Declaración de las variables

A continuación se muestran los valores utilizados en los pasos del ejemplo. Además, algunos de los ejemplos utilizan variables declaradas en sus pasos. Si está siguiendo estos pasos en su propio entorno, asegúrese de reemplazar estos valores por los suyos. Cuando especifique la ubicación, compruebe que se admite la región que especifique. Para más información, consulte las [preguntas más frecuentes](#faq).

```azurepowershell-interactive
$RG1         = "TestRG1"
$VNet1       = "VNet1"
$Location1   = "CentralUS"
$FESubnet1   = "FrontEnd"
$BESubnet1   = "Backend"
$GwSubnet1   = "GatewaySubnet"
$VNet1Prefix = "10.1.0.0/16"
$FEPrefix1   = "10.1.0.0/24"
$BEPrefix1   = "10.1.1.0/24"
$GwPrefix1   = "10.1.255.0/27"
$Gw1         = "VNet1GW"
$GwIP1       = "VNet1GWIP"
$GwIPConf1   = "gwipconf1"
```

## <a name="configure"></a>2. Crear la red virtual

Cree un grupo de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -ResourceGroupName $RG1 -Location $Location1
```

Cree una red virtual.

```azurepowershell-interactive
$fesub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $FESubnet1 -AddressPrefix $FEPrefix1
$besub1 = New-AzureRmVirtualNetworkSubnetConfig -Name $BESubnet1 -AddressPrefix $BEPrefix1
$vnet = New-AzureRmVirtualNetwork -Name $VNet1 -ResourceGroupName $RG1 -Location $Location1 -AddressPrefix $VNet1Prefix -Subnet $fesub1,$besub1
```

## <a name="gwsub"></a>3. Adición de la subred de puerta de enlace

La subred de puerta de enlace contiene las direcciones IP reservadas que usan los servicios de puerta de enlace de la red virtual. Utilice los siguientes ejemplos para agregar y establecer una subred de puerta de enlace:

Agregue la subred de puerta de enlace.

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name VNet1
Add-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -AddressPrefix 10.1.255.0/27 -VirtualNetwork $getvnet
```

Establezca la configuración de subred de puerta de enlace para la red virtual.

```azurepowershell-interactive
$getvnet | Set-AzureRmVirtualNetwork
```
## <a name="publicip"></a>4. Solicitar una dirección IP pública
 
En este paso, seleccione las instrucciones que se aplican a la puerta de enlace que desea crear. La selección de las zonas para la implementación de las puertas de enlace depende de las zonas especificadas para la dirección IP pública.

### <a name="ipzoneredundant"></a>Para las puertas de enlace con redundancia de zona

Solicite una dirección IP pública con una SKU de IP pública **Estándar** y no especifique ninguna zona. En este caso, la dirección IP pública Estándar creada será una IP pública con redundancia de zona.   

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard
```

### <a name="ipzonalgw"></a>Para las puertas de enlace zonales

Solicite una dirección IP pública con una SKU de IP pública **Estándar**. Especifique la zona (1, 2 ó 3). Todas las instancias de puerta de enlace se implementarán en esta zona.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Static -Sku Standard -Zone 1
```

### <a name="ipregionalgw"></a>Para las puertas de enlace regionales

Solicite una dirección IP pública con una SKU de IP pública **Básica**. En este caso, la puerta de enlace se implementa como una puerta de enlace regional y no se agrega redundancia de zona a la puerta de enlace. Las instancias de puerta de enlace se crean en las zonas respectivas.

```azurepowershell-interactive
$pip1 = New-AzureRmPublicIpAddress -ResourceGroup $RG1 -Location $Location1 -Name $GwIP1 -AllocationMethod Dynamic -Sku Basic
```
## <a name="gwipconfig"></a>5. Creación de la configuración de IP

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>6. Creación de la puerta de enlace

Cree la puerta de enlace de red virtual.

### <a name="for-expressroute"></a>Para ExpressRoute

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>Para VPN Gateway

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="faq"></a>P+F

### <a name="what-will-change-when-i-deploy-these-new-skus"></a>¿Qué cambiará cuando proceda a implementar estas SKU nuevas?

Desde su perspectiva, puede implementar puertas de enlace con redundancia de zona. Esto significa que todas las instancias de las puertas de enlace se implementarán en distintas zonas de disponibilidad de Azure, y cada zona de disponibilidad es un dominio de error y actualización diferente. Por ello, las puertas de enlace son más confiables, disponibles y resistentes a los errores de la zona.

### <a name="can-i-use-the-azure-portal"></a>¿Puedo usar Azure Portal?

Sí, puede usar Azure Portal para implementar las nuevas SKU. Sin embargo, verá estas nuevas SKU solo en las regiones de Azure que tengan Azure Availability Zones.

### <a name="what-regions-are-available-for-me-to-use-the-new-skus"></a>¿Qué regiones están disponibles para usar las nuevas SKU?

Las nuevas SKU están disponibles en las regiones de Azure que tienen Azure Availability Zones: las regiones Centro de EE. UU., Centro de Francia, Europa del Norte, Europa Occidental y Oeste de EE. UU. 2. En el futuro, las puertas de enlace con redundancia de zona estarán disponibles en otras regiones públicas de Azure.

### <a name="can-i-changemigrateupgrade-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>¿Puedo convertir/migrar/actualizar mis puertas de enlace de red virtual existentes en puertas de enlace zonales o con redundancia de zona?

Actualmente no se permite migrar las puertas de enlace de red virtual existentes a puertas de enlace zonales o con redundancia de zona. Sin embargo, puede eliminar la puerta de enlace existente y volver a crear una puerta de enlace zonal o con redundancia de zona.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>¿Puedo implementar puertas de enlace de VPN y ExpressRoute en la misma red virtual?

Se admite la coexistencia de puertas de enlace de VPN y ExpressRoute en la misma red virtual. Sin embargo, le recomendamos que reserve un intervalo de direcciones IP /27 para la subred de puerta de enlace.
