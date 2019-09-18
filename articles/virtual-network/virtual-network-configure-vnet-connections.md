---
title: Configuración y validación de las conexiones de red virtual o VPN
description: Guía paso a paso para configurar y validar diversas implementaciones de VPN y red virtual de Azure
services: virtual-network
documentationcenter: na
author: v-miegge
manager: dcscontentpm
editor: ''
ms.assetid: 0433a4f4-b5a0-476d-b398-1506c57eafa2
ms.service: virtual-network
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/28/2019
ms.author: kaushika
ms.openlocfilehash: fc4b649ce8d082d8d854c4c19b617c088ff3141c
ms.sourcegitcommit: 3e7646d60e0f3d68e4eff246b3c17711fb41eeda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/11/2019
ms.locfileid: "70901436"
---
# <a name="configure-and-validate-vnet-or-vpn-connections"></a>Configuración y validación de las conexiones de red virtual o VPN

En este tutorial guiado se proporcionan instrucciones paso a paso para configurar y validar diversas implementaciones de VPN y red virtual de Azure en escenarios como el enrutamiento de tránsito, conexiones de red virtual a red virtual, BGP, multisitio, de punto a sitio, etc.

Las instancias de VPN Gateway de Azure permiten flexibilidad en la organización de casi cualquier tipo de topología de Virtual Network (VNet) conectada en Azure: puede conectar VNet entre regiones, entre diferentes tipos de redes virtuales (Azure Resource Manager frente a una red virtual clásica), dentro de Azure o con un entorno híbrido local, en distintas suscripciones, etc. 

## <a name="scenario-1-vnet-to-vnet-vpn-connection"></a>Escenario 1: Conexión VPN de red virtual a red virtual

La conexión de una red virtual a otra a través de VPN es muy parecida a la conexión de una red virtual a una ubicación de un sitio local. Ambos tipos de conectividad usan una puerta de enlace de VPN para proporcionar un túnel seguro con **IPsec/IKE**. Las redes virtuales pueden estar en la misma región o en distintas, así como pertenecer a una única suscripción o a varias.

![IMAGEN](./media/virtual-network-configure-vnet-connections/4034386_en_2.png)
 
Figura 1: conexión de red virtual a red virtual con IPsec

Si las redes virtuales están en la misma región, considere la posibilidad de conectarlas mediante emparejamiento de VNET, que no usa una puerta de enlace de VPN, aumenta el rendimiento y reduce la latencia. Seleccione **Configure and validate VNet Peering** (Configurar y validar emparejamiento de VNET) para configurar una conexión de emparejamiento de VNET.

Si las redes virtuales se crean con el modelo de implementación de Azure Resource Manager, seleccione **Configure and validate a Resource Manager VNet to a Resource Manager VNet connection** (Configurar y validar una red virtual de Resource Manager en una conexión de red virtual de Resource Manager) para configurar una conexión VPN.

Si una de las redes virtuales se crea mediante el modelo de implementación clásica de Azure, la otra la crea Resource Manager. Seleccione **Configure and validate a classic VNet to a Resource Manager VNet connection** (Configurar y validar una red virtual clásica en una conexión de red virtual de Resource Manager) para configurar una conexión VPN.

### <a name="configuration-1-configure-vnet-peering-for-two-vnets-in-the-same-region"></a>Configuración 1: Configuración del emparejamiento de VNET para dos redes virtuales en la misma región

Antes de iniciar la implementación de emparejamiento de VNET de Azure, asegúrese de que cumple los siguientes requisitos previos para configurar el emparejamiento de VNET:

* Las redes virtuales emparejadas deben existir en la misma región de Azure.
* Deben tener además espacios de direcciones IP que no se superpongan.
* El emparejamiento de red virtual se realiza entre dos redes virtuales. No hay ninguna relación transitiva derivada entre emparejamientos. Por ejemplo, si VNetA está emparejada con VNetB y VNetB está emparejada con VNetC, VNetA *no* está emparejada con VNetC.

Cuando cumpla los requisitos, puede seguir [Tutorial: Conexión de redes virtuales con emparejamiento de redes virtuales usando Azure Portal](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) para crear y configurar el emparejamiento de VNET.

Para comprobar la configuración del emparejamiento de VNET, use los métodos siguientes:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta que disponga de los [roles y permisos](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions) necesarios.
2. En el cuadro que contiene el texto *Buscar recursos*, en la parte superior de Azure Portal, escriba *redes virtuales*. Cuando **redes virtuales** aparezca en los resultados de búsqueda, haga clic en él.
3. En la hoja **Redes virtuales** que aparece, haga clic en la red virtual para la que quiere crear un emparejamiento.
4. En el panel que aparece para la red virtual seleccionada, haga clic en **Emparejamientos** en la sección **Configuración**.
5. Haga clic en el emparejamiento del que quiera comprobar la configuración.

![IMAGEN](./media/virtual-network-configure-vnet-connections/4034496_en_1.png)
 
Con Azure PowerShell, ejecute el comando [Get-AzureRmVirtualNetworkPeering](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?view=azurermps-4.1.0) para obtener el emparejamiento de red virtual, por ejemplo:

```
PS C:\Users\User1> Get-AzureRmVirtualNetworkPeering -VirtualNetworkName Vnet10-01 -ResourceGroupName dev-vnets
Name                             : LinkToVNET10-02
Id                               : /subscriptions/GUID/resourceGroups/dev-vnets/providers/Microsoft.Network/virtualNetworks/VNET10-01/virtualNetworkPeerings/LinkToVNET10-0
2
Etag                             : W/"GUID"
ResourceGroupName                : dev-vnets
VirtualNetworkName               : vnet10-01
ProvisioningState                : Succeeded
RemoteVirtualNetwork             : {
                                  "Id": "/subscriptions/GUID/resourceGroups/DEV-VNET
                                   s/providers/Microsoft.Network/virtualNetworks/VNET10-02"
                                   }
AllowVirtualNetworkAccess        : True
AllowForwardedTraffic            : False
AllowGatewayTransit              : False
UseRemoteGateways                : False
RemoteGateways                   : null
RemoteVirtualNetworkAddressSpace : null
```

### <a name="connection-type-1-connect-a-resource-manager-vnet-to-anther-resource-manager-vnet-azure-resource-manager-to-azure-resource-manager"></a>Tipo de conexión 1: Conexión de una red virtual de Resource Manager a otra (Azure Resource Manager a Azure Resource Manager)

Puede configurar una conexión de una red virtual de Resource Manager a otra red virtual de Resource Manager directamente o configurar la conexión con IPsec.

### <a name="configuration-2-configure-vpn-connection-between-resource-manager-vnets"></a>Configuración 2: Configuración de la conexión VPN entre redes virtuales de Resource Manager

Para configurar una conexión entre redes virtuales de Resource Manager sin IPSec, consulte [Configuración de una conexión de VPN Gateway de red virtual a red virtual mediante Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal).

Para configurar una conexión con IPSec entre dos redes virtuales de Resource Manager, siga los pasos del 1 al 5 que se indican en [Creación de una conexión de sitio a sitio mediante Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) para cada red virtual.

> [!Note]
> Estos pasos solo funcionan para las redes virtuales en la misma suscripción. Si las redes virtuales se encuentran en distintas suscripciones, debe usar PowerShell para realizar la conexión. Vea el artículo [PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps).

### <a name="validate-vpn-connection-between-resource-manager-vnets"></a>Validación de la conexión VPN entre redes virtuales de Resource Manager

![IMAGEN](./media/virtual-network-configure-vnet-connections/4034493_en_2.png)

Figura 4: conexión de red virtual clásica a una red virtual de Azure Resource Manager

Para comprobar que la conexión VPN está configurada correctamente, siga las instrucciones:

> [!Note]
> El número que aparece después de los componentes de red virtual, como "Vnet 1" en los pasos siguientes, se corresponde con los números de la figura 4.

1. Compruebe los espacios de direcciones superpuestos en las redes virtuales conectadas.

   > [!Note]
   > No puede haber espacios de direcciones superpuestos en la red virtual 1 ni en la red virtual 6. 

2. Compruebe que el intervalo de direcciones de la red virtual 1 de Azure Resource Manager se defina con precisión en el **objeto de conexión** 4.
3. Compruebe que el intervalo de direcciones de la red virtual 6 de Azure Resource Manager se defina con precisión en el **objeto de conexión** 3.
4. Compruebe que las claves previamente compartidas coincidan con los objetos de conexión 3 y 4.
5. Compruebe que la IP virtual 2 de VNet Gateway de Azure Resource Manager se defina con precisión en el **objeto de conexión** 4.
6. Compruebe que la IP virtual 5 de VNet Gateway de Azure Resource Manager se defina con precisión en el **objeto de conexión** 3.

### <a name="connection-type-2-connect-a-classic-vnet-to-a-resource-manager-vnet"></a>Tipo de conexión 2: Conexión de una red virtual clásica a una red virtual de Resource Manager

Puede crear una conexión entre redes virtuales que estén en diferentes suscripciones y en diferentes regiones. También puede conectar redes virtuales que ya tengan conexiones a redes locales, siempre que el tipo de puerta de enlace se hayan configurado como basada en ruta.

Para configurar una conexión entre una red virtual clásica y una red virtual de Resource Manager, consulte [Conexión de redes virtuales a partir de diferentes modelos de implementación con Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-connect-different-deployment-models-portal) para obtener más información.

![IMAGEN](./media/virtual-network-configure-vnet-connections/4034389_en_2.png)

Figura 5: conexión de red virtual clásica a una red virtual de Azure Resource Manager

Para comprobar la configuración cuando conecte una red virtual clásica a una red virtual de Azure Resource Manager, siga las instrucciones:

> [!Note]
> El número que aparece después de los componentes de red virtual, como "Vnet 1" en los pasos siguientes, se corresponde con los números de la figura 5.

1. Compruebe los espacios de direcciones superpuestos en las redes virtuales conectadas.

   > [!Note]
   > No puede haber espacios de direcciones superpuestos en la red virtual 1 ni en la red virtual 6.

2. Compruebe que el intervalo de direcciones de la red virtual 6 de Azure Resource Manager se defina con precisión en la definición de red local clásica 3.
3. Compruebe que el intervalo de direcciones de la red virtual 1 clásica se defina con precisión en el **objeto de conexión** 4 de Azure Resource Manager.
4. Compruebe que la IP virtual de VNet Gateway 2 clásica se defina con precisión en el **objeto de conexión** 4 de Azure Resource Manager.
5. Compruebe que la IP virtual de VNet Gateway 5 de Azure Resource Manager se defina con precisión en la **definición de red local** clásica 3.
6. Compruebe que las claves previamente compartidas coincidan en ambas redes virtuales:
   1. Red virtual clásica: definición de red local 3
   2. Red virtual de Azure Resource Manager: objeto de conexión 4

## <a name="scenario-2-point-to-site-vpn-connection"></a>Escenario 2: Conexión VPN de punto a sitio

Una configuración de punto a sitio (P2S) permite crear una conexión segura entre un equipo cliente individual y una red virtual. Las conexiones de punto a sitio son útiles cuando desea conectarse a su red virtual desde una ubicación remota, como desde casa o desde una conferencia, o si tiene pocos clientes que necesiten conectarse a una red virtual. La conexión VPN P2S se inicia desde el equipo cliente utilizando el cliente de VPN de Windows nativo. En la conexión de clientes, se usan certificados para la autenticación.

![IMAGEN](./media/virtual-network-configure-vnet-connections/4034387_en_3.png)

Figura 2: Conexión de punto a sitio

Las conexiones de punto a sitio no requieren ningún dispositivo VPN. P2S crea la conexión VPN sobre SSTP (Protocolo de túnel de sockets seguros). Puede conectar una conexión de punto a sitio a una red virtual mediante el uso de diferentes modelos y herramientas de implementación:

* [Configuración de una conexión de punto a sitio a una red virtual mediante Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)
* [Configuración de una conexión de punto a sitio en una red virtual con Azure Portal (clásico)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-classic-azure-portal)
* [Configuración de una conexión de punto a sitio a una red virtual mediante PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps)

### <a name="validate-your-point-to-site-connections"></a>Validación de las conexiones de punto a sitio

En el artículo [Solución de problemas: problemas de conexión de punto a sitio de Azure](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems) se describen problemas comunes con las conexiones de punto a sitio.

## <a name="scenario-3-multi-site-vpn-connection"></a>Escenario 3: Conexión VPN multisitio

Puede agregar una conexión de sitio a sitio (S2S) a una red virtual que ya tenga una conexión S2S, una conexión de punto a sitio o una conexión de red virtual a red virtual. Con frecuencia, este tipo de conexión se conoce como configuración **multisitio**. 

![IMAGEN](./media/virtual-network-configure-vnet-connections/4034497_en_2.png)

Figura 3: conexión multisitio

Azure actualmente funciona con dos modelos de implementación: el de Resource Manager y el clásico. Los dos modelos no son totalmente compatibles entre sí. Para configurar la conexión **multisitio** con distintos modelos, consulte los siguientes artículos:

* [Agregar una conexión de sitio a sitio a una red virtual con una conexión de VPN Gateway existente](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal)
* [Agregar una conexión de sitio a sitio a una red virtual con una conexión de VPN Gateway existente (clásico)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-multi-site)

> [!Note]
> Estos pasos no se aplican a las configuraciones de conexión coexistentes de ExpressRoute y sitio a sitio. Para obtener información sobre las conexiones coexistentes, consulte [Conexiones ExpressRoute/S2S y de sitio a sitio coexistentes](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager).

## <a name="scenario-4-configure-transit-routing"></a>Escenario 4: Configuración del enrutamiento de tránsito

El enrutamiento transitivo es un escenario de enrutamiento específico en el que se conectan varias redes en una topología de "cadena de margarita". Este enrutamiento permite que los recursos de las redes virtuales de cualquier extremo de la "cadena" se comuniquen entre sí a través de redes virtuales entre ellos. Sin el enrutamiento transitivo, las redes o los dispositivos emparejados a través de un centro de conectividad no se pueden conectar entre sí.

### <a name="configuration-1-configure-transit-routing-in-a-point-to-site-connection"></a>Configuración 1: Configuración del enrutamiento de tránsito en una conexión de punto a sitio

En este escenario, va a configurar una conexión VPN de sitio a sitio entre VNetA y VNetB, además de configurar una VPN de punto a sitio para que el cliente se conecte a la puerta de enlace de VNetA. A continuación, debe habilitar el enrutamiento de tránsito para que los clientes de punto a sitio se conecten a VNetB, que pasa a través de VNetA. Este escenario se admite cuando BGP está habilitado en la VPN de sitio a sitio entre VNetA y VNetB. Para más información, consulte [Información sobre el enrutamiento de VPN de punto a sitio](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-point-to-site-routing).

### <a name="configuration-2-configure-transit-routing-in-an-expressroute-connection"></a>Configuración 2: Configuración del enrutamiento de tránsito en una conexión de ExpressRoute

Microsoft Azure ExpressRoute le permite ampliar sus redes de local en la nube de Microsoft a través de una conexión privada y dedicada que facilita un proveedor de conectividad. Con ExpressRoute, se pueden establecer conexiones con servicios en la nube de Microsoft, como Microsoft Azure, Office 365 y Dynamics 365.  Para más información, consulte [Información general sobre ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction).

![IMAGEN](./media/virtual-network-configure-vnet-connections/4034395_en_1.png)

Figura 6: conexión de "emparejamiento privado" de ExpressRoute a redes virtuales de Azure

> [!Note]
> Se recomienda que, si VNetA y VNetB se encuentren en la misma región geográfica, [vincule ambas redes virtuales al circuito de ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-arm) en lugar de configurar el enrutamiento transitivo. Si las redes virtuales se encuentran en distintas regiones geopolíticas, también puede vincularlas directamente al circuito si tiene [ExpressRoute Premium](https://docs.microsoft.com/azure/expressroute/expressroute-faqs#expressroute-premium). 

Si tiene coexistencia de sitio a sitio y ExpressRoute, no se admite el enrutamiento de tránsito. Para más información, consulte [Configuración de conexiones ExpressRoute y de sitio a sitio coexistentes](https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager).

Si ha habilitado ExpressRoute para conectar las redes locales a una red virtual de Azure, puede habilitar el emparejamiento de VNET entre las redes virtuales que quiere que tengan enrutamiento transitivo. Para permitir que las redes locales se conecten a la red virtual remota, debe configurar el [emparejamiento de redes virtuales](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview#gateways-and-on-premises-connectivity). 

> [!Note]
> El emparejamiento de redes virtuales solo está disponible para redes virtuales de la misma región.

Para comprobar si ha configurado la ruta de tránsito para el emparejamiento de VNET, siga estas instrucciones:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta que disponga de los [roles y permisos](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions) necesarios.
2. [Cree un emparejamiento entre la red virtual A y B](https://docs.microsoft.com/azure/virtual-network/virtual-network-create-peering) como en el diagrama anterior (figura 6). 
3. En el panel que aparece para la red virtual seleccionada, haga clic en **Emparejamientos** en la sección **Configuración**.
4. Haga clic en el emparejamiento que quiere ver y en **Configuración** para validar que ha habilitado **Permitir tránsito de puerta de enlace** en la red virtual A conectada al circuito ExpressRoute y **Usar puerta de enlace remota** en la red virtual B no conectado al circuito ExpressRoute.

### <a name="configuration-3-configure-transit-routing-in-a-vnet-peering-connection"></a>Configuración 3: Configuración del enrutamiento de tránsito en una conexión de emparejamiento de VNET

Cuando las redes virtuales están emparejadas, también puede configurar la puerta de enlace de la red virtual emparejada como punto de tránsito a una red local. Para configurar la ruta de tránsito en el emparejamiento de VNET, seleccione [Conexiones de red virtual a red virtual](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps?toc=/azure/virtual-network/toc.json).

> [!Note]
> No se admite el tránsito de puerta de enlace en la relación de emparejamiento entre las redes virtuales creadas mediante modelos de implementación diferentes. Para que funcione el tránsito de puerta de enlace, ambas redes virtuales de la relación de emparejamiento se deben haber creado mediante Resource Manager.

Para comprobar si ha configurado la ruta de tránsito para el emparejamiento de VNET, siga estas instrucciones:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con una cuenta que disponga de los [roles y permisos](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering#roles-permissions) necesarios.
2. En el cuadro que contiene el texto Buscar recursos, en la parte superior de Azure Portal, escriba *redes virtuales*. Cuando **redes virtuales** aparezca en los resultados de búsqueda, haga clic en él.
3. En la hoja **Redes virtuales** que aparece, haga clic en la red virtual para la que quiere comprobar la configuración de emparejamiento.
4. En el panel que aparece para la red virtual seleccionada, haga clic en **Emparejamientos** en la sección **Configuración**.
5. Haga clic en el emparejamiento que quiere ver y compruebe que ha habilitado **Permitir tránsito de puerta de enlace** y **Usar puerta de enlace remota** en **Configuración**.

![IMAGEN](./media/virtual-network-configure-vnet-connections/4035414_en_1.png)

### <a name="configuration-4-configure-transit-routing-in-a-vnet-to-vnet-connection"></a>Configuración 4: Configuración del enrutamiento de tránsito en una conexión de red virtual a red virtual

Para configurar el enrutamiento de tránsito entre redes virtuales, debe habilitar BGP en todas las conexiones de red virtual a red virtual intermedias mediante el modelo de implementación de Resource Manager y PowerShell. Para ver las instrucciones, consulte [Configuración de BGP para Azure VPN Gateway con PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

El tráfico en tránsito a través de Azure VPN Gateway es posible mediante el modelo de implementación clásica, pero se basa en espacios de direcciones definidos estáticamente en el archivo de configuración de red. BGP aún no se admite con instancias de Azure Virtual Network y VPN Gateway mediante el modelo de implementación clásica. Sin BGP, definir manualmente los espacios de direcciones de tránsito es difícil de hacer sin errores y no se recomienda.

> [!Note]
> Las conexiones de red virtual a red virtual clásicas se configuran mediante Azure Portal (clásico) o mediante un archivo de configuración de red en el portal clásico. No puede crear ni modificar una red virtual clásica mediante el modelo de implementación de Azure Resource Manager ni Azure Portal. Para obtener más información sobre el enrutamiento de tránsito para redes virtuales clásicas, consulte [Hub & Spoke, Daisy-Chain, and Full-Mesh VNET topologies in Azure ARM using VPN (V1)](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/) (Topologías VNET de red en estrella tipo hub-and-spoke, de cadena de margarita y de malla completa en Azure ARM mediante VPN [V1]).

### <a name="configuration-5-configure-transit-routing-in-a-site-to-site-connection"></a>Configuración 5: Configuración del enrutamiento de tránsito en una conexión de sitio a sitio

Para configurar el enrutamiento de tránsito entre la red local y una red virtual con una conexión de sitio a sitio, debe habilitar BGP en todas las conexiones de sitio a sitio intermedias mediante el modelo de implementación de Resource Manager y PowerShell. Para obtener instrucciones, consulte [Configuración de BGP para Azure VPN Gateway con PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

El tráfico en tránsito a través de Azure VPN Gateway es posible mediante el modelo de implementación clásica, pero se basa en espacios de direcciones definidos estáticamente en el archivo de configuración de red. BGP aún no se admite con instancias de Azure Virtual Network y VPN Gateway mediante el modelo de implementación clásica. Sin BGP, definir manualmente los espacios de direcciones de tránsito es difícil de hacer sin errores y no se recomienda.

> [!Note]
> Las conexiones de sitio a sitio clásicas se configuran mediante Azure Portal (clásico) o mediante un archivo de configuración de red en el portal clásico. No puede crear ni modificar una red virtual clásica mediante el modelo de implementación de Azure Resource Manager ni Azure Portal. Para obtener más información sobre el enrutamiento de tránsito para redes virtuales clásicas, consulte [Hub & Spoke, Daisy-Chain, and Full-Mesh VNET topologies in Azure ARM using VPN (V1)](https://blogs.msdn.microsoft.com/igorpag/2015/10/01/hubspoke-daisy-chain-and-full-mesh-vnet-topologies-in-azure-arm-using-vpn-v1/) (Topologías VNET de red en estrella tipo hub-and-spoke, de cadena de margarita y de malla completa en Azure ARM mediante VPN [V1]).

## <a name="scenario-5-configure-bgp-for-a-vpn-gateway"></a>Escenario 5: Configuración de BGP para una instancia de VPN Gateway

BGP es el protocolo de enrutamiento estándar usado en Internet para intercambiar información de enrutamiento y disponibilidad entre dos o más redes. Cuando se usa BGP en el contexto de Azure Virtual Network, BGP habilita Azure VPN Gateway y los dispositivos VPN locales, denominados vecinos o pares BGP. Intercambian "rutas" que informan a ambas puertas de enlace sobre la disponibilidad y accesibilidad de los prefijos para pasar por las puertas de enlace o los enrutadores pertinentes. BGP también puede permitir el enrutamiento del tránsito entre varias redes mediante la propagación de las rutas que una puerta de enlace de BGP aprende de un par BGP a todos los demás pares BGP. Para obtener más información, consulte [Acerca de BGP con Azure VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview).

### <a name="configure-bgp-for-a-vpn-connection"></a>Configuración de BGP para una conexión VPN

Para configurar una conexión VPN que usa BGP, consulte [Configuración de BGP para Azure VPN Gateway con PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-resource-manager-ps).

> [!Note]
> 1. Habilite BGP en la puerta de enlace de red virtual mediante la creación de un número de sistema autónomo (AS) para ella. Las puertas de enlace básicas no admiten BGP. Para comprobar la SKU de la puerta de enlace, vaya a la sección de información general de la hoja VPN Gateway en Azure Portal. Si la SKU es **básica**, tendrá que cambiarla ([cambiar el tamaño de la puerta de enlace](https://docs.microsoft.com/powershell/module/azurerm.network/resize-azurermvirtualnetworkgateway?view=azurermps-4.1.0&viewFallbackFrom=azurermps-4.0.0)) a la SKU **VpnGw1**. La comprobación de la SKU producirá un tiempo de inactividad de 20 a 30 minutos. Tan pronto como la puerta de enlace tenga la SKU correcta, se puede agregar el sistema autónomo (AS) a través del cmdlet [Set-AzureRmVirtualNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgateway?view=azurermps-3.8.0) de PowerShell. Después de configurar el número de AS, se proporcionará automáticamente una dirección IP del par BGP para la puerta de enlace.
> 2. LocalNetworkGateway debe proporcionarse **manualmente** con un número de AS y una dirección del par BGP. Puede establecer los valores **ASN** y **-BgpPeeringAddress** mediante los cmdlet [New-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermlocalnetworkgateway?view=azurermps-4.1.0) o [Set-AzureRmLocalNetworkGateway](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermlocalnetworkgateway?view=azurermps-4.1.0) de PowerShell. Algunos números de AS se reservan para Azure y no se pueden usar según se describe en [Acerca de BGP con Azure VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-bgp-overview#bgp-faq).
> 3. Por último, el objeto de conexión debe tener BGP habilitado. Puede establecer el valor **-EnableBGP** en `$True` mediante [New-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/new-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0) o [Set-AzureRmVirtualNetworkGatewayConnection](https://docs.microsoft.com/powershell/module/azurerm.network/set-azurermvirtualnetworkgatewayconnection?view=azurermps-4.1.0).

### <a name="validate-the-bgp-configuration"></a>Validación de la configuración de BGP

Para comprobar si BGP se ha configurado correctamente, puede ejecutar los cmdlet `get-AzureRmVirtualNetworkGateway` y `get-AzureRmLocalNetworkGateway` y, a continuación, observará la salida relacionada con BGP en la parte BgpSettingsText. Por ejemplo:  BgpSettingsText:

```
{

"Asn": AsnNumber,

"BgpPeeringAddress": "IP address",

"PeerWeight": 0

}
```

## <a name="scenario-6-highly-available-active-active-vpn-connection"></a>Escenario 6: Conexión VPN activo-activo de alta disponibilidad

Las diferencias clave entre las puertas de enlace activo-activo y activo-en espera:

* Debe crear dos configuraciones IP de puerta de enlace con dos direcciones IP públicas.
* Debe establecer la marca *EnableActiveActiveFeature*.
* La SKU de puerta de enlace debe ser VpnGw1, VpnGw2, VpnGw3.

Para lograr una alta disponibilidad en la conectividad de red virtual a red virtual y entre entornos locales, debe implementar varias puertas de enlace VPN y establecer varias conexiones en paralelo entre sus redes y Azure. Para obtener información general de las opciones de conectividad y la topología, consulte [Conectividad de alta disponibilidad entre locales y de red virtual a red virtual](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable).

Para crear conexiones activo-activo entre locales y de red virtual a red virtual, siga las instrucciones de [Configuración de conexiones VPN S2S activo-activo con Azure VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-activeactive-rm-powershell) para configurar Azure VPN Gateway en modo activo/activo.

> [!Note]  
> 1. Al agregar direcciones a la puerta de enlace de red local para BGP habilitado, el modo activo/activo *solo agrega las direcciones /32 de los pares BGP*. Si agrega más direcciones, se considerarán rutas estáticas y tendrán prioridad sobre las rutas BGP.
> 2. Debe usar ASN de BGP diferentes para las redes locales que se conectan a Azure. (Si son iguales, tiene que cambiar el ASN de la red virtual si el dispositivo VPN local ya utiliza el ASN para emparejarse con otros vecinos de BGP).

## <a name="scenario-7-change-an-azure-vpn-gateway-type-after-deployment"></a>Escenario 7: Cambio de un tipo de instancia de Azure VPN Gateway tras la implementación

No se puede cambiar un tipo de puerta de enlace de red virtual de Azure basada en directivas a una basada en el enrutamiento o viceversa directamente. Debe eliminar la puerta de enlace, tras lo cual la dirección IP y la clave precompartida (PSK) no se conservarán. Después, puede crear una nueva puerta de enlace del tipo deseado. Para eliminar y crear una puerta de enlace, siga estos pasos:

1. Elimine también las conexiones asociadas a la puerta de enlace original.
2. Elimine la puerta de enlace mediante Azure Portal, PowerShell o PowerShell clásico: 
   * [Eliminación de una puerta de enlace de red virtual mediante Azure Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-portal)
   * [Eliminación de una puerta de enlace de red virtual mediante PowerShell](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-powershell)
   * [Eliminación de una puerta de enlace de red virtual mediante PowerShell (clásico)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-delete-vnet-gateway-classic-powershell)
3. Siga los pasos descritos en [Creación de la puerta de enlace VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#a-namevnetgatewaya4-create-the-vpn-gateway) para crear la nueva puerta de enlace del tipo deseado y completar la configuración de VPN.

> [!Note]
> Este proceso tardará unos 60 minutos.

## <a name="next-steps"></a>Pasos siguientes

* [Solución de problemas de conectividad entre máquinas virtuales de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-troubleshoot-connectivity-problem-between-vms)

