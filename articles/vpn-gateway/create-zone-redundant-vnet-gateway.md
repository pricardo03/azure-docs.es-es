---
title: Creación de una puerta de enlace de red virtual con redundancia de zona en Azure Availability Zones (versión preliminar) | Microsoft Docs
description: Implementación de puertas de enlace de VPN Gateway y ExpressRoute en Availability Zones (versión preliminar).
services: vpn-gateway
documentationcenter: na
author: cherylmc
Customer intent: As someone with a basic network background, I want to understand how to create zone-redundant gateways.
ms.service: vpn-gateway
ms.topic: article
ms.date: 06/28/2018
ms.author: cherylmc
ms.openlocfilehash: c484358bf98f0121cfc3ce270b162b01c75b5b09
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37096240"
---
# <a name="create-a-zone-redundant-virtual-network-gateway-in-azure-availability-zones---preview"></a>Creación de una puerta de enlace de red virtual con redundancia de zona en Azure Availability Zones (versión preliminar)

Puede implementar puertas de enlace de VPN Gateway y ExpressRoute en [Azure Availability Zones](../availability-zones/az-overview.md). Esto aporta una mayor disponibilidad, escalabilidad y resistencia a las puertas de enlace de red virtual. Implementar puertas de enlace en Azure Availability Zones separa de forma física y lógica las puertas de enlace dentro de una región, y protege la conectividad de red local en Azure de errores de nivel de zona.

Las puertas de enlace zonales y con redundancia de zona ofrecen mejoras de rendimiento sustanciales frente a las puertas de enlace de red virtual normales. Además, crear una puerta de enlace de red virtual zonal o con redundancia de zona es más rápido que crear otras puertas de enlace. En lugar de 45 minutos, se tarda aproximadamente 15 minutos para una puerta de enlace de ExpressRoute y 19 minutos para una puerta de enlace de VPN Gateway.

### <a name="zrgw"></a>Puertas de enlace con redundancia de zona

Para implementar automáticamente las puertas de enlace de red virtual en distintas zonas de disponibilidad, puede usar puertas de enlace de red virtual con redundancia de zona. Las puertas de enlace con redundancia de zona permiten aprovechar el SLA del 99,99 % de tiempo de actividad en las versiones de disponibilidad general para tener acceso a los servicios de Azure críticos para la misión y escalables.

<br>
<br>

![gráfico de puertas de enlace con redundancia de zona](./media/create-zone-redundant-vnet-gateway/zonered.png)

### <a name="zgw"></a>Puertas de enlace zonales

Para implementar puertas de enlace en una zona específica, use puertas de enlace zonales. Al implementar una puerta de enlace zonal, ambas instancias de la puerta de enlace se implementan en la misma zona de disponibilidad.

<br>
<br>

![gráfico de puertas de enlace zonales](./media/create-zone-redundant-vnet-gateway/zonal.png)

## <a name="gwskus"></a>SKU de puerta de enlace

Las puertas de enlace zonales y con redundancia de zona deben usar las nuevas SKU de puerta de enlace. Después de [inscribirse automáticamente en la versión preliminar](#enroll), verá las nuevas SKU de puerta de enlace de red virtual en todas las regiones de Azure AZ. Estas SKU son similares a las SKU correspondientes para ExpressRoute y VPN Gateway, salvo que son específicas para puertas de enlace zonales y con redundancia de zona.

Las nuevas SKU de puerta de enlace son:

### <a name="vpn-gateway"></a>VPN Gateway

* VpnGw1AZ
* VpnGw2AZ
* VpnGw3AZ

### <a name="expressroute"></a>ExpressRoute

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

## <a name="pipskus"></a>SKU de IP pública

Las puertas de enlace con redundancia de zona y las puertas de enlace zonales se basan en el recurso de IP pública de Azure SKU *Estándar*. La configuración del recurso de IP pública de Azure determina si la puerta de enlace que se implementa tiene redundancia de zona o es zonal. Si crea un recurso de IP pública con una SKU *Básica*, la puerta de enlace no tendrá redundancia de zona y los recursos de puerta de enlace serán regionales.

### <a name="pipzrg"></a>Puertas de enlace con redundancia de zona

Cuando se crea una dirección IP pública con la SKU de IP pública **Estándar** sin especificar una zona, el comportamiento es diferente según si la puerta de enlace es una puerta de enlace de VPN o una puerta de enlace de ExpressRoute. 

* En el caso de las puertas de enlace de VPN, las dos instancias de la puerta de enlace se implementarán en dos de estas tres zonas para proporcionar redundancia de zona. 
* En el caso de las puertas de enlace de ExpressRoute, como puede haber más de dos instancias, la puerta de enlace puede abarcar las tres zonas.

### <a name="pipzg"></a>Puertas de enlace zonales

Cuando se crea una dirección IP pública con la SKU de IP pública **Estándar** y se especifica la zona (1, 2 o 3), todas las instancias de la puerta de enlace se implementarán en la misma zona.

### <a name="piprg"></a>Puertas de enlace regionales

Cuando se crea una dirección IP pública con la SKU de IP pública **Básica**, la puerta de enlace se implementa como una puerta de enlace regional y no se agrega redundancia de zona a la puerta de enlace.

## <a name="before-you-begin"></a>Antes de empezar

Puede usar una instalación local de PowerShell en su equipo o Azure Cloud Shell. Si decide instalar y usar PowerShell de forma local, esta característica requiere la versión más reciente del módulo de PowerShell.

[!INCLUDE [Cloud shell](../../includes/vpn-gateway-cloud-shell-powershell.md)]

### <a name="to-use-powershell-locally"></a>Para usar PowerShell de forma local

Si está usando PowerShell localmente en el equipo en lugar de usar Cloud Shell, debe instalar el módulo de PowerShell 6.1.1 o versiones posteriores. Para comprobar qué versión de PowerShell tiene instalada, use el siguiente comando:

```azurepowershell
Get-Module AzureRM -ListAvailable | Select-Object -Property Name,Version,Path
```

Si necesita actualizarla, consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-azurerm-ps).

[!INCLUDE [PowerShell login](../../includes/vpn-gateway-ps-login-include.md)]

## <a name="enroll"></a>1. Inscripción en la versión preliminar

Para poder configurar una puerta de enlace zonal o con redundancia de zona, primero debe inscribir su suscripción en la versión preliminar. Después de aprovisionar la suscripción, empezará a ver las nuevas SKU de puerta de enlace en todas las regiones de Azure AZ. 

Asegúrese de iniciar sesión con su cuenta de Azure y de utilizar la suscripción que desea a agregar a la lista de permitidos para esta versión preliminar. Para inscribirse, utilice el ejemplo siguiente:

```azurepowershell-interactive
Register-AzureRmProviderFeature -FeatureName AllowVMSSVirtualNetworkGateway -ProviderNamespace Microsoft.Network
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
```

Use el siguiente comando para comprobar que la característica "AllowVMSSVirtualNetworkGateway" está registrada en su suscripción:

```azurepowershell-interactive
Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network
```

El resultado será similar al siguiente ejemplo:

![aprovisionado](./media/create-zone-redundant-vnet-gateway/verifypreview.png)

## <a name="variables"></a>2. Declaración de las variables

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

## <a name="configure"></a>3. Crear la red virtual

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

## <a name="gwsub"></a>4. Adición de la subred de puerta de enlace

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
## <a name="publicip"></a>5. Solicitar una dirección IP pública
 
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
## <a name="gwipconfig"></a>6. Creación de la configuración de IP

```azurepowershell-interactive
$getvnet = Get-AzureRmVirtualNetwork -ResourceGroupName $RG1 -Name $VNet1
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name $GwSubnet1 -VirtualNetwork $getvnet
$gwipconf1 = New-AzureRmVirtualNetworkGatewayIpConfig -Name $GwIPConf1 -Subnet $subnet -PublicIpAddress $pip1
```

## <a name="gwconfig"></a>7. Creación de la puerta de enlace

Cree la puerta de enlace de red virtual.

>[!NOTE]
>En este momento, no puede especificar la SKU de puerta de enlace. De forma predeterminada, se seleccionará la SKU ErGw1AZ para ExpressRoute y VpnGw1AZ para VPN Gateway.
>

### <a name="for-expressroute"></a>Para ExpressRoute

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType ExpressRoute
```

### <a name="for-vpn-gateway"></a>Para VPN Gateway

```azurepowershell-interactive
New-AzureRmVirtualNetworkGateway -ResourceGroup $RG1 -Location $Location1 -Name $Gw1 -IpConfigurations $GwIPConf1 -GatewayType Vpn -VpnType RouteBased
```

## <a name="feedback"></a>Cómo enviar sus comentarios

Agradeceríamos su participación. Envíe un correo electrónico a aznetworkgateways@microsoft.com para notificar posibles problemas o proporcionar comentarios (positivos o negativos) acerca de las puertas de enlace zonales y con redundancia de zona de VPN Gateway y ExpressRoute. Incluya el nombre de su compañía en la línea de asunto "[ ]". Incluya también su identificador de suscripción si está notificando un problema.

## <a name="faq"></a>P+F

### <a name="how-do-i-sign-up-for-the-preview"></a>¿Cómo me puedo suscribir a la versión preliminar?

También puede [inscribirse automáticamente](#enroll) con los comandos de PowerShell indicados en este artículo.

### <a name="what-will-change-when-i-enroll"></a>¿Qué cambiará cuando me inscriba?

Desde su perspectiva, durante la versión preliminar, puede implementar puertas de enlace con redundancia de zona. Esto significa que todas las instancias de las puertas de enlace se implementarán en distintas zonas de disponibilidad de Azure, y cada zona de disponibilidad es un dominio de error y actualización diferente. Por ello, las puertas de enlace son más confiables, disponibles y resistentes a los errores de la zona.

### <a name="what-regions-are-available-for-the-preview"></a>¿Qué regiones están disponibles para la versión preliminar?

Las puertas de enlace zonales y con redundancia de zona están disponibles en las regiones de producción o pública de Azure.

### <a name="will-i-be-billed-for-participating-in-this-preview"></a>¿Se me cobrarán cargos por participar en esta versión preliminar?

No se le facturarán las puertas de enlace durante la versión preliminar. Sin embargo, no hay ningún SLA asociado con su implementación. Estamos muy interesados en recibir sus comentarios.

> [!NOTE]
> En el caso de las puertas de enlace de ExpressRoute, no se facturará la puerta de enlace ni realizarán cargos por ella. Sin embargo, se facturará el circuito en sí (no la puerta de enlace).

### <a name="what-regions-are-available-for-me-to-try-this-in"></a>¿Qué regiones están disponibles para probar esto?

La versión preliminar pública está disponible en las regiones Centro de EE. UU. y Centro de Francia (regiones de Azure con zonas de disponibilidad disponibles con carácter general). En el futuro, las puertas de enlace con redundancia de zona estarán disponibles en otras regiones públicas de Azure.

### <a name="can-i-change-my-existing-virtual-network-gateways-to-zone-redundant-or-zonal-gateways"></a>¿Puedo convertir mis puertas de enlace de red virtual existentes en puertas de enlace zonales o con redundancia de zona?

Actualmente no se permite migrar las puertas de enlace de red virtual existentes a puertas de enlace zonales o con redundancia de zona. Sin embargo, puede eliminar la puerta de enlace existente y volver a crear una puerta de enlace zonal o con redundancia de zona.

### <a name="can-i-deploy-both-vpn-and-express-route-gateways-in-same-virtual-network"></a>¿Puedo implementar puertas de enlace de VPN y ExpressRoute en la misma red virtual?

Se admite la coexistencia de puertas de enlace de VPN y ExpressRoute en la misma red virtual durante la versión preliminar pública. Sin embargo, tenga en cuenta los siguientes requisitos y limitaciones:

* Reserve un intervalo de direcciones IP /27 para la subred de puerta de enlace.
* Las puertas de enlace de ExpressRoute zonales o con redundancia de zona solo pueden coexistir con puertas de enlace de VPN zonales o con redundancia de zona.
* Implemente las puertas de enlace de ExpressRoute zonales o con redundancia de zona antes de implementar las puertas de enlace de VPN zonales o con redundancia de zona.
* Una puerta de enlace de ExpressRoute zonal o con redundancia de zona se puede conectar a un máximo de 4 circuitos.

## <a name="next-steps"></a>Pasos siguientes

Agradeceríamos su participación. Envíe un correo electrónico a aznetworkgateways@microsoft.com para notificar posibles problemas o proporcionar comentarios (positivos o negativos) acerca de las puertas de enlace zonales y con redundancia de zona de VPN Gateway y ExpressRoute. Incluya el nombre de su compañía en la línea de asunto "[ ]". Incluya también su identificador de suscripción si está notificando un problema.
