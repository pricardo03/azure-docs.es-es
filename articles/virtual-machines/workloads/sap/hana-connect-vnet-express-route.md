---
title: Configuración de conectividad de la red virtual en SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Configuración de conectividad de máquinas virtuales para usar SAP HANA en Azure (instancias grandes).
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
ms.openlocfilehash: 5efdda485e4e1f5013948c6636b267f0d388f4d5
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44167634"
---
# <a name="connecting-a-vnet-to-hana-large-instance-expressroute"></a>Conexión de una red virtual a ExpressRoute de HANA Instancias grandes

Una vez definidos todos los intervalos de direcciones IP y con los datos devueltos por Microsoft, puede comenzar a conectar la red virtual creada anteriormente a HANA Instancias grandes. Una vez creada la red virtual de Azure, es necesario crear una puerta de enlace de ExpressRoute en la red virtual para vincular la red al circuito de ExpressRoute que conecta con el inquilino del cliente en la marca Instancias grandes.

> [!NOTE] 
> Este paso puede tardar hasta 30 minutos en completarse, ya que la nueva puerta de enlace se crea en la suscripción de Azure designada y, después, se conecta a la red virtual de Azure especificada.

Si ya existe una puerta de enlace, compruebe si se trata de una puerta de enlace de ExpressRoute o no. Si no lo es, debe eliminarla y volver a crearla como una puerta de enlace de ExpressRoute. Si ya hay una puerta de enlace de ExpressRoute establecida, la red virtual de Azure ya está conectada al circuito de ExpressRoute para la conectividad local; por tanto, continúe con la sección Vinculación de redes virtuales, que aparece a continuación.

- Use el (nuevo) [Azure Portal](https://portal.azure.com/) o PowerShell para crear una puerta de enlace de VPN de ExpressRoute conectada a la red virtual.
  - Si usa Azure Portal, agregue una nueva **puerta de enlace de red virtual** y seleccione **ExpressRoute** como tipo de puerta de enlace.
  - Si prefiere PowerShell, descargue y use la versión más reciente [SDK de Azure PowerShell](https://azure.microsoft.com/downloads/) para garantizar una experiencia óptima. Los siguientes comandos crean una puerta de enlace de ExpressRoute. Los textos precedidos por un signo _$_ son variables definidas por el usuario que es necesario actualizar con la información específica.

```PowerShell
# These Values should already exist, update to match your environment
$myAzureRegion = "eastus"
$myGroupName = "SAP-East-Coast"
$myVNetName = "VNet01"

# These values are used to create the gateway, update for how you wish the GW components to be named
$myGWName = "VNet01GW"
$myGWConfig = "VNet01GWConfig"
$myGWPIPName = "VNet01GWPIP"
$myGWSku = "HighPerformance" # Supported values for HANA Large Instances are: HighPerformance or UltraPerformance

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

En este ejemplo, se usó la SKU de puerta de enlace HighPerformance. Puede elegir HighPerformance o UltraPerformance, las únicas SKU de puerta de enlace que son compatibles con SAP HANA en Azure (Instancias grandes).

> [!IMPORTANT]
> Para las instancias grandes de HANA de las SKU de la clase Tipo II, el uso de la SKU de puerta de enlace UltraPerformance es obligatorio.

**Vinculación de redes virtuales**

Ahora que la red virtual de Azure tiene una puerta de enlace de ExpressRoute, utilice la información de autorización proporcionada por Microsoft para conectar la puerta de enlace de ExpressRoute al circuito de ExpressRoute de SAP HANA en Azure (Instancias grandes) creado para esta conectividad. Este paso puede realizarse mediante Azure Portal o PowerShell. Aunque se recomienda Azure Portal, las instrucciones de PowerShell son las siguientes. 

- Ejecute los siguientes comandos para cada puerta de enlace de la red virtual con un AuthGUID diferente para cada conexión. Las dos primeras entradas mostradas en el siguiente script proceden de la información proporcionada por Microsoft. Además, el AuthGUID es específico de cada red virtual y su puerta de enlace. Significa que, si quiere agregar otra red virtual de Azure, tiene que obtener otro AuthID para el circuito de ExpressRoute que conecta HANA (Instancias grandes) a Azure. 

```PowerShell
# Populate with information provided by Microsoft Onboarding team
$PeerID = "/subscriptions/9cb43037-9195-4420-a798-f87681a0e380/resourceGroups/Customer-USE-Circuits/providers/Microsoft.Network/expressRouteCircuits/Customer-USE01"
$AuthGUID = "76d40466-c458-4d14-adcf-3d1b56d1cd61"

# Your ExpressRoute Gateway Information
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

Si quiere conectar la puerta de enlace a varios circuitos de ExpressRoute asociados a la suscripción, puede que tenga que ejecutar este paso más de una vez. Por ejemplo, probablemente vaya a conectar la misma puerta de enlace de la red virtual al circuito de ExpressRoute que conecta la red virtual a la red local.

**Pasos siguientes**

- Consulte [Requisitos de red adicionales para HLI](hana-additional-network-requirements.md).