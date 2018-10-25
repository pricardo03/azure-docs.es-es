---
title: Configuración de la conectividad de red virtual a SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Configuración de la conectividad de máquinas virtuales para usar SAP HANA en Azure (instancias grandes).
services: virtual-machines-linux
documentationcenter: ''
author: RicksterCDN
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/10/2018
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a64a60603cd9898386a975313afc676e3b253326
ms.sourcegitcommit: 8e06d67ea248340a83341f920881092fd2a4163c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49353604"
---
# <a name="connect-a-virtual-network-to-hana-large-instances"></a>Conexión de una red virtual a instancias grandes de HANA

Tras crear una red virtual de Azure, puede conectarla a SAP HANA en instancias grandes de Azure. Cree una puerta de enlace de ExpressRoute de Azure en la red virtual. Dicha puerta de enlace le permite vincular la red virtual al circuito de ExpressRoute que se conecta con el inquilino del cliente en el sello de instancias grandes.

> [!NOTE] 
> Este paso puede tardar hasta 30 minutos en completarse. La nueva puerta de enlace se crea en la suscripción de Azure designada y, después, se conecta a la red virtual de Azure especificada.

Si ya existe una puerta de enlace, compruebe si se trata de una puerta de enlace de ExpressRoute o no. De no ser así, elimine la puerta de enlace y vuelva a crearla como una puerta de enlace de ExpressRoute. Si ya hay una puerta de enlace de ExpressRoute establecida, consulte la sección siguiente de este artículo, "Vinculación de redes virtuales". 

- Use [Azure Portal](https://portal.azure.com/) o PowerShell para crear una puerta de enlace VPN de ExpressRoute conectada a la red virtual.
  - Si usa Azure Portal, agregue una nueva **puerta de enlace de red virtual** y seleccione **ExpressRoute** como tipo de puerta de enlace.
  - Si usa PowerShell, descargue y use la versión más reciente del [SDK de Azure PowerShell](https://azure.microsoft.com/downloads/). Los siguientes comandos crean una puerta de enlace de ExpressRoute. Los textos precedidos por un signo _$_ son variables definidas por el usuario que se deben actualizar con la información específica.

```PowerShell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA large instances are: HighPerformance or UltraPerformance

# These Commands create the Public IP and ExpressRoute Gateway
$vnet = Get-AzureRmVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzureRmPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzureRmVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

En este ejemplo, se usó la SKU de puerta de enlace HighPerformance. Puede elegir HighPerformance o UltraPerformance, las únicas SKU de puerta de enlace que son compatibles con SAP HANA en Azure (instancias grandes).

> [!IMPORTANT]
> Para las instancias grandes de HANA de las SKU de la clase Tipo II, debe usar la SKU de puerta de enlace UltraPerformance.

## <a name="link-virtual-networks"></a>Vinculación de redes virtuales

La red virtual de Azure ahora tiene una puerta de enlace de ExpressRoute. Utilice la información de autorización proporcionada por Microsoft para conectar la puerta de enlace de ExpressRoute al circuito de ExpressRoute de SAP HANA en Azure (instancias grandes). Puede usar Azure Portal o PowerShell para realizar la conexión. Se recomienda Azure Portal, pero si prefiere usar PowerShell, las instrucciones son las siguientes. 

Ejecute los siguientes comandos para cada puerta de enlace de la red virtual con un AuthGUID diferente para cada conexión. Las dos primeras entradas mostradas en el siguiente script proceden de la información proporcionada por Microsoft. Además, el AuthGUID es específico de cada red virtual y su puerta de enlace. Si quiere agregar otra red virtual de Azure, tiene que obtener otro AuthID para el circuito de ExpressRoute que conecta HANA (instancias grandes) a Azure. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway information
$myGroupName = "SAP-East-Coast"
$myGWName = "VNet01GW"
$myGWLocation = "East US"

# Define the name for your connection
$myConnectionName = "VNet01GWConnection"

# Create a new connection between the ER Circuit and your Gateway using the Authorization
$gw = Get-AzureRmVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzureRmVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID
```

Para conectar la puerta de enlace a más de un circuito de ExpressRoute asociado a la suscripción, puede que tenga que ejecutar este paso más de una vez. Por ejemplo, probablemente vaya a conectar la misma puerta de enlace de la red virtual al circuito de ExpressRoute que conecta la red virtual a la red local.

## <a name="next-steps"></a>Pasos siguientes

- [Requisitos de red adicionales para HLI](hana-additional-network-requirements.md)