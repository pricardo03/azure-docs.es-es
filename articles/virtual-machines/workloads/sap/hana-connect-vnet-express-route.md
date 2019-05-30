---
title: Conectividad configurada de la red virtual al SAP HANA en Azure (instancias grandes) | Microsoft Docs
description: Conectividad configurada de red virtual para usar SAP HANA en Azure (instancias grandes).
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
ms.date: 05/25/2019
ms.author: rclaus
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9cd290f9e5e7819f3dffa2dd1efea9cd0028fc2c
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239462"
---
# <a name="connect-a-virtual-network-to-hana-large-instances"></a>Conexión de una red virtual a instancias grandes de HANA

Tras crear una red virtual de Azure, puede conectarla a SAP HANA en instancias grandes de Azure. Cree una puerta de enlace de ExpressRoute de Azure en la red virtual. Esta puerta de enlace le permite vincular la red virtual al circuito de ExpressRoute que conecta con el inquilino del cliente en el sello de instancias grandes de HANA.

> [!NOTE] 
> Este paso puede tardar hasta 30 minutos en completarse. La nueva puerta de enlace se crea en la suscripción de Azure designada y, después, se conecta a la red virtual de Azure especificada.

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

Si ya existe una puerta de enlace, compruebe si se trata de una puerta de enlace de ExpressRoute o no. Si no es una puerta de enlace de ExpressRoute, elimine la puerta de enlace y volver a crearla como una puerta de enlace de ExpressRoute. Si ya hay una puerta de enlace de ExpressRoute establecida, consulte la sección siguiente de este artículo, "Vinculación de redes virtuales". 

- Use [Azure Portal](https://portal.azure.com/) o PowerShell para crear una puerta de enlace VPN de ExpressRoute conectada a la red virtual.
  - Si usa Azure Portal, agregue una nueva **puerta de enlace de red virtual** y seleccione **ExpressRoute** como tipo de puerta de enlace.
  - Si usa PowerShell, descargue y use la versión más reciente del [SDK de Azure PowerShell](https://azure.microsoft.com/downloads/). 
 
Los siguientes comandos crean una puerta de enlace de ExpressRoute. Los textos precedidos por un _$_ son variables definidas por el usuario que se deben actualizar con su información específica.

```powershell
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
$vnet = Get-AzVirtualNetwork -Name $myVNetName -ResourceGroupName $myGroupName
$subnet = Get-AzVirtualNetworkSubnetConfig -Name 'GatewaySubnet' -VirtualNetwork $vnet
New-AzPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName `
-Location $myAzureRegion -AllocationMethod Dynamic
$gwpip = Get-AzPublicIpAddress -Name $myGWPIPName -ResourceGroupName $myGroupName
$gwipconfig = New-AzVirtualNetworkGatewayIpConfig -Name $myGWConfig -SubnetId $subnet.Id `
-PublicIpAddressId $gwpip.Id

New-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName -Location $myAzureRegion `
-IpConfigurations $gwipconfig -GatewayType ExpressRoute `
-GatewaySku $myGWSku -VpnType PolicyBased -EnableBgp $true
```

En este ejemplo, se usó la SKU de puerta de enlace HighPerformance. Puede elegir HighPerformance o UltraPerformance, las únicas SKU de puerta de enlace que son compatibles con SAP HANA en Azure (instancias grandes).

> [!IMPORTANT]
> Para las instancias grandes de HANA de las SKU de la clase Tipo II, debe usar la SKU de puerta de enlace UltraPerformance.

## <a name="link-virtual-networks"></a>Vinculación de redes virtuales

La red virtual de Azure ahora tiene una puerta de enlace de ExpressRoute. Use la información de autorización proporcionada por Microsoft para conectar la puerta de enlace de ExpressRoute al circuito ExpressRoute de instancias grandes de SAP HANA. Puede usar Azure Portal o PowerShell para realizar la conexión. Las instrucciones de PowerShell son los siguientes. 

Ejecute los comandos siguientes para cada puerta de enlace de ExpressRoute mediante el uso de un AuthGUID diferente para cada conexión. Las dos primeras entradas mostradas en el siguiente script proceden de la información proporcionada por Microsoft. Además, el AuthGUID es específico de cada red virtual y su puerta de enlace. Si desea agregar otra red virtual de Azure, deberá obtener otro AuthID para el circuito de ExpressRoute que conecta las instancias grandes de HANA en Azure de Microsoft. 

```powershell
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
$gw = Get-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID -ExpressRouteGatewayBypass
```

> [!NOTE]
> El último parámetro en el comando New-AzVirtualNetworkGatewayConnection **ExpressRouteGatewayBypass** es un parámetro nuevo que permite la ruta rápida de ExpressRoute. Una funcionalidad que reduce la latencia de red entre sus unidades de instancia grande de HANA y las máquinas virtuales de Azure. Se agregó la funcionalidad en mayo de 2019. Para obtener más información, consulte el artículo [arquitectura de red de SAP HANA (instancias grandes)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-network-architecture). Asegúrese de que se está ejecutando la versión más reciente de los cmdlets de PowerShell antes de ejecutar los comandos.

Para conectar la puerta de enlace a más de un circuito de ExpressRoute asociado a la suscripción, puede que tenga que ejecutar este paso más de una vez. Por ejemplo, probablemente vaya a conectar la misma puerta de enlace de la red virtual al circuito de ExpressRoute que conecta la red virtual a la red local.

## <a name="applying-expressroute-fast-path-to-existing-hana-large-instance-expressroute-circuits"></a>Aplicar ExpressRoute rápido ruta de acceso a los circuitos de ExpressRoute de instancias grandes de HANA existentes
Hasta ahora la documentación explica cómo conectar un nuevo circuito ExpressRoute que se creó con una implementación de instancia grande de HANA a una puerta de enlace de ExpressRoute de Azure de una de las redes virtuales de Azure. Pero muchos clientes ya tienen ya su programa de instalación de circuitos de ExpressRoute y disponen de sus redes virtuales conectadas a instancias grandes de HANA ya. Como la ruta rápida de ExpressRoute nuevo es reducir la latencia de red, se recomienda que aplique el cambio para usar esta funcionalidad. Los comandos para conectar un nuevo circuito ExpreesRoute y cambiar un circuito ExpressRoute existente son los mismos. Como resultado necesario ejecutar esta secuencia de comandos de PowerShell para cambiar un circuito existente para usar 

```powershell
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
$gw = Get-AzVirtualNetworkGateway -Name $myGWName -ResourceGroupName $myGroupName
    
New-AzVirtualNetworkGatewayConnection -Name $myConnectionName `
-ResourceGroupName $myGroupName -Location $myGWLocation -VirtualNetworkGateway1 $gw `
-PeerId $PeerID -ConnectionType ExpressRoute -AuthorizationKey $AuthGUID -ExpressRouteGatewayBypass
```

Es importante agregar el último parámetro tal como se muestra anteriormente, para habilitar la funcionalidad de ruta con Fast de ExpressRoute


## <a name="expressroute-global-reach"></a>Alcance Global de ExpressRoute
Como desee habilitar el alcance Global de uno o ambos de los dos escenarios:

 - Replicación del sistema HANA sin necesidad de firewalls o servidores proxy adicionales
 - Copia las copias de seguridad entre las unidades de instancia grande de HANA en dos regiones diferentes para realizar copias del sistema o las actualizaciones del sistema

necesita tenga en cuenta:

- Debe proporcionar un intervalo de direcciones de/29 espacio de direcciones. Que el intervalo de direcciones no puede superponer con cualquiera de los otros espacio intervalos de direcciones que usa la conexión hasta el momento de instancias grandes de HANA a Azure y no se pueden superponer con cualquiera de los intervalos de direcciones IP usa en otro lugar en Azure o localmente.
- No hay ningún límite en el ASN (número de sistema autónomo) que se puede utilizar para anunciar las rutas locales para instancias grandes de HANA. Local no debe anunciar las rutas con ASN privados en el intervalo de 65000 – 65020 o 65515. 
- Para el escenario de conexión de acceso directo a nivel local a instancias grandes de HANA, debe calcular una cuota para el circuito que le conecta a Azure. Para conocer los precios, consulte los precios de [complemento alcance Global](https://azure.microsoft.com/pricing/details/expressroute/).

Para obtener uno o ambos de los escenarios que se aplica a la implementación, abra un mensaje de soporte técnico con Azure como se describe en [abrir una solicitud de soporte técnico para instancias grandes de HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-li-portal#open-a-support-request-for-hana-large-instances)

Datos que son necesarios y las palabras clave que se deben utilizar para Microsoft para que pueda enrutar y ejecutar en la solicitud, aspecto:

- Servicio: Instancia grande de SAP HANA
- Tipo de problema: Instalación y configuración
- Subtipo del problema: Mi problema no aparece aquí
- Asunto 'Modificar mi red - agregar alcance Global'
- Detalles: ' Agregar alcance Global para la instancia grande de HANA al inquilino de instancia grande de HANA o ' Agregar alcance Global a local para el inquilino de instancia grande de HANA.
- Detalles adicionales para la instancia grande de HANA al caso de inquilino de instancia grande de HANA: Debe definir el **dos regiones de Azure** donde se encuentran los dos inquilinos conectar **AND** tiene que enviar el   **/29 intervalo de direcciones IP**
- Detalles adicionales para el local en el caso de inquilinos de instancia grande de HANA: Debe definir el **región de Azure** donde se implementa el inquilino de instancia grande de HANA que desea conectarse directamente. Además deberá proporcionar el **Auth GUID** y **Id. del mismo nivel de circuito** que recibió al establecer el circuito de ExpressRoute entre local y Azure. Además, debe nombrar la **ASN**. La última entrega es un **/29 intervalo de direcciones IP** para alcance Global de ExpressRoute.

> [!NOTE]
> Si desea tener ambos controlan los casos, debe proporcionar dos diferentes/usado hasta ahora de intervalo de direcciones de 29 intervalos de direcciones IP que no se superponen con cualquier otra dirección IP. 




## <a name="next-steps"></a>Pasos siguientes

- [Requisitos de red adicionales para HLI](hana-additional-network-requirements.md)
