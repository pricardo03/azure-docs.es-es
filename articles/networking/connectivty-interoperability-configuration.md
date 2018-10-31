---
title: 'Interoperabilidad de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET - Detalles de configuración: interoperabilidad de las características de conectividad de back-end de Azure | Microsoft Docs'
description: En esta página se proporcionan los detalles de la configuración de prueba que se usa para analizar la interoperabilidad de las características de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: d94900b764331c6fff0e0384e6edbebc88ac938b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947154"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---test-configuration-details"></a>Interoperabilidad de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET: Detalles de la configuración de prueba

En este artículo, se van a analizar los detalles de la configuración de prueba. Para revisar la configuración de prueba, vea la [configuración de prueba][Setup]. 

##<a name="spoke-vnet-connectivity-using-vnet-peering"></a>Conectividad de red virtual de radio mediante el emparejamiento de VNET

En la captura de pantalla siguiente de Azure Portal se muestran los detalles del emparejamiento de red virtual de radio. Para obtener instrucciones paso a paso para configurar el emparejamiento de red virtual entre dos redes virtuales, vea [Crear, cambiar o eliminar un emparejamiento de red virtual][VNet-Config]. Si quiere que la red virtual de radio use las puertas de enlace conectadas a la red virtual del concentrador, tendrá que consultar *Uso de puertas de enlace remotas*.

[![1]][1]

En la captura de pantalla siguiente de Azure Portal se muestran los detalles del emparejamiento de red virtual del concentrador. Si quiere que la red virtual del concentrador permita que la red virtual de radio use sus puertas de enlace, tendrá que consultar *Uso de puertas de enlace remotas*.

[![2]][2]

##<a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Conectividad de red virtual de sucursal mediante VPN de sitio a sitio

La conectividad VPN de sitio a sitio entre las redes virtuales de concentrador y de sucursal se configura mediante las puertas de enlace VPN. De forma predeterminada, las puertas de enlace VPN y ExpressRoute se configuran con el valor ASN privado de 65515. La puerta de enlace de VPN permite cambiar el valor ASN. En la configuración de prueba, como se muestra en la captura de pantalla siguiente de Azure Portal, el valor ASN de la puerta de enlace VPN de red virtual de sucursal se cambia a 65516 para habilitar el enrutamiento EBGP entre las redes virtuales de concentrador y de sucursal.


[![3]][3]


##<a name="location-1-on-premises-connectivity-using-expressroute-and-site-to-site-vpn"></a>Conectividad local de Ubicación 1 con ExpressRoute y VPN de sitio a sitio

###<a name="expressroute1-configuration-details"></a>Detalles de configuración de ExpressRoute1

En la captura de pantalla siguiente del portal se muestra la configuración del circuito ExpressRoute de la región 1 de Azure hacia los enrutadores CE locales de Ubicación 1.

[![4]][4]

En la captura de pantalla siguiente del portal se muestra la configuración de conexión entre el circuito ExpressRoute1 y la red virtual del concentrador.

[![5]][5]

La configuración siguiente es el listado de la configuración de enrutadores CE principales (en la configuración de prueba se usan enrutadores Cisco ASR1000 como enrutadores CE) relacionados con la conectividad de emparejamiento privado de ExpressRoute. Cuando VPN de sitio a sitio y el circuito de ExpressRoute se configuran en paralelo para conectar una red local a Azure, Azure prefiere el circuito de ExpressRoute de forma predeterminada. Para evitar el enrutamiento asimétrico, en la red local también se debe preferir ExpressRoute a VPN de sitio a sitio para las rutas recibidas a través de ExpressRoute y VPN de sitio a sitio. Esto se logra en la configuración siguiente con el atributo local-preference de BGP. 

    interface TenGigabitEthernet0/0/0.300
     description Customer 30 private peering to Azure
     encapsulation dot1Q 30 second-dot1q 300
     ip vrf forwarding 30
     ip address 192.168.30.17 255.255.255.252
    !
    interface TenGigabitEthernet1/0/0.30
     description Customer 30 to south bound LAN switch
     encapsulation dot1Q 30
     ip vrf forwarding 30
     ip address 192.168.30.0 255.255.255.254
     ip ospf network point-to-point
    !
    router ospf 30 vrf 30
     router-id 10.2.30.253
     redistribute bgp 65021 subnets route-map BGP2OSPF
     network 192.168.30.0 0.0.0.1 area 0.0.0.0
    default-information originate always
     default-metric 10
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 192.168.30.18 remote-as 12076
      neighbor 192.168.30.18 activate
      neighbor 192.168.30.18 next-hop-self
      neighbor 192.168.30.18 soft-reconfiguration inbound
      neighbor 192.168.30.18 route-map prefer-ER-over-VPN in
      neighbor 192.168.30.18 prefix-list Cust30_to_Private out
     exit-address-family
    !
    route-map prefer-ER-over-VPN permit 10
     set local-preference 200
    !
    ip prefix-list Cust30_to_Private seq 10 permit 10.2.30.0/25
    !

###<a name="site-to-site-vpn-configuration-details"></a>Detalles de configuración de VPN de sitio a sitio

Este es el listado de la configuración de enrutador CE principal relacionado con la conectividad VPN de sitio a sitio:

    crypto ikev2 proposal Cust30-azure-proposal
     encryption aes-cbc-256 aes-cbc-128 3des
     integrity sha1
     group 2
    !
    crypto ikev2 policy Cust30-azure-policy
     match address local 66.198.12.106
     proposal Cust30-azure-proposal
    !
    crypto ikev2 keyring Cust30-azure-keyring
     peer azure
      address 52.168.162.84
      pre-shared-key local IamSecure123
      pre-shared-key remote IamSecure123
    !
    crypto ikev2 profile Cust30-azure-profile
     match identity remote address 52.168.162.84 255.255.255.255
     identity local address 66.198.12.106
     authentication local pre-share
     authentication remote pre-share
     keyring local Cust30-azure-keyring
    !
    crypto ipsec transform-set Cust30-azure-ipsec-proposal-set esp-aes 256 esp-sha-hmac
     mode tunnel
    !
    crypto ipsec profile Cust30-azure-ipsec-profile
     set transform-set Cust30-azure-ipsec-proposal-set
     set ikev2-profile Cust30-azure-profile
    !
    interface Loopback30
     ip address 66.198.12.106 255.255.255.255
    !
    interface Tunnel30
     ip vrf forwarding 30
     ip address 10.2.30.125 255.255.255.255
     tunnel source Loopback30
     tunnel mode ipsec ipv4
     tunnel destination 52.168.162.84
     tunnel protection ipsec profile Cust30-azure-ipsec-profile
    !
    router bgp 65021
     !
     address-family ipv4 vrf 30
      network 10.2.30.0 mask 255.255.255.128
      neighbor 10.10.30.254 remote-as 65515
      neighbor 10.10.30.254 ebgp-multihop 5
      neighbor 10.10.30.254 update-source Tunnel30
      neighbor 10.10.30.254 activate
      neighbor 10.10.30.254 soft-reconfiguration inbound
     exit-address-family
    !
    ip route vrf 30 10.10.30.254 255.255.255.255 Tunnel30

##<a name="location-2-on-premises-connectivity-using-expressroute"></a>Conectividad local de Ubicación 2 con ExpressRoute

Un segundo circuito de ExpressRoute, más próximo a la Ubicación 2 en el entorno local, conecta la Ubicación 2 a la red virtual del concentrador. En la captura de pantalla siguiente del portal se muestra la segunda configuración de ExpressRoute.

[![6]][6]

En la captura de pantalla siguiente del portal se muestra la configuración de conexión entre el segundo circuito de ExpressRoute y la red virtual del concentrador.

[![7]][7]

ExpressRoute1 también conecta la red virtual del concentrador y la Ubicación 1 en el entorno local a una red virtual remota en otra región de Azure.

[![8]][8]

## <a name="further-reading"></a>Lecturas adicionales

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Uso conjunto de ExpressRoute y la conectividad VPN de sitio a sitio

#### <a name="site-to-site-vpn-over-expressroute"></a>VPN de sitio a sitio a través de ExpressRoute

VPN de sitio a sitio se puede configurar a través del emparejamiento de Microsoft de ExpressRoute para intercambiar datos de forma privada entre la red local y las redes virtuales de Azure con confidencialidad, antirreproducción, autenticidad e integridad. Para obtener más información sobre cómo configurar VPN IPSec de sitio a sitio en modo de túnel a través del emparejamiento de Microsoft de ExpressRoute, vea [Configuración de una VPN de sitio a sitio a través del emparejamiento de Microsoft de ExpressRoute][S2S-Over-ExR]. 

La limitación principal de la configuración de VPN de sitio a sitio a través del emparejamiento de Microsoft es el rendimiento. El rendimiento a través del túnel IPSec está limitado por la capacidad de la puerta de enlace de VPN. El rendimiento de la puerta de enlace de VPN es menor en comparación con el rendimiento de ExpressRoute. En estos escenarios, el uso del túnel IPSec para el tráfico de seguridad alta y el emparejamiento privado para el tráfico restante ayudaría a optimizar el uso de ancho de banda de ExpressRoute.

#### <a name="site-to-site-vpn-as-a-secure-failover-path-for-expressroute"></a>VPN de sitio a sitio como ruta de acceso de conmutación por error para ExpressRoute
ExpressRoute se ofrece como par de circuito redundante para garantizar la alta disponibilidad. Puede configurar la conectividad de ExpressRoute con redundancia geográfica en diferentes regiones de Azure. Además, como se ha hecho en la configuración de prueba, dentro de una región determinada de Azure, si quiere una ruta de acceso de conmutación por error para la conectividad de ExpressRoute, puede hacerlo mediante VPN de sitio a sitio. Cuando se anuncian los mismos prefijos a través de ExpressRoute y VPN de sitio a sitio, Azure prefiere ExpressRoute a VPN de sitio a sitio. Para evitar el enrutamiento asimétrico entre ExpressRoute y VPN de sitio a sitio, de forma recíproca en la configuración de la red local también debe ser preferible ExpressRoute a la conectividad VPN de sitio a sitio.

Para obtener más información sobre cómo configurar conexiones coexistentes de ExpressRoute y VPN de sitio a sitio, vea [Configuración de conexiones ExpressRoute y de sitio a sitio coexistentes][ExR-S2S-CoEx].

### <a name="extending-backend-connectivity-to-spoke-vnets-and-branch-locations"></a>Ampliación de la conectividad de back-end a redes virtuales de radio y ubicaciones de sucursales

#### <a name="spoke-vnet-connectivity-using-vnet-peering"></a>Conectividad de red virtual de radio mediante el emparejamiento de VNET

La arquitectura de red virtual de tipo hub-and-spoke se usa ampliamente. El concentrador (hub) es una red virtual (VNet) en Azure que actúa como un punto central de conectividad entre las redes virtuales de radio (spoke) y la red local. Los radios son redes virtuales que se emparejan con el concentrador y que se pueden usar para aislar las cargas de trabajo. El tráfico fluye entre el centro de datos local y el concentrador a través de una conexión de ExpressRoute o VPN. Para obtener más información sobre la arquitectura, vea [Implementación de una topología de red en estrella tipo hub-and-spoke en Azure][Hub-n-Spoke].

El emparejamiento de VNET dentro de una región permite que las redes virtuales de radio usen las puertas de enlace de la red virtual del concentrador (tanto de VPN como de ExpressRoute) para comunicarse con las redes remotas.

#### <a name="branch-vnet-connectivity-using-site-to-site-vpn"></a>Conectividad de red virtual de sucursal mediante VPN de sitio a sitio

Si quiere que las redes virtuales de sucursal (en distintas regiones) y las redes locales se comuniquen entre sí a través de una red virtual de concentrador, la solución de Azure nativa es la conectividad VPN de sitio a sitio mediante VPN. Una opción alternativa consiste en usar una Aplicación virtual de red (NVA) para el enrutamiento en el concentrador.

Para configurar puertas de enlace de VPN, vea [Configuración de una instancia de VPN Gateway][VPN]. Para implementar aplicaciones virtuales de red de alta disponibilidad, vea [Implementación de aplicaciones virtuales de red de alta disponibilidad][Deploy-NVA].

## <a name="next-steps"></a>Pasos siguientes

Para el análisis del plano de control de la configuración de prueba y comprender las vistas de otras redes virtuales y VLAN de la topología, vea [Control-Plane Analysis][Control-Analysis] (Análisis del plano de control).

Para el análisis del plano de datos de la configuración de prueba y las vistas de las características de supervisión de red de Azure, vea [Data-Plane Analysis][Data-Analysis] (Análisis del plano de datos).

Para obtener información sobre cuántos circuitos de ExpressRoute se pueden conectar a una puerta de enlace de ExpressRoute, cuántas puertas de enlace de ExpressRoute se pueden conectar a un circuito de ExpressRoute, o bien para obtener información sobre otros límites de escala de ExpressRoute, vea [P+F de ExpressRoute][ExR-FAQ].


<!--Image References-->
[1]: ./media/backend-interoperability/SpokeVNet_peering.png "Emparejamiento de VNET de la red virtual de radio"
[2]: ./media/backend-interoperability/HubVNet-peering.png "Emparejamiento de VNET de la red virtual del concentrador"
[3]: ./media/backend-interoperability/BranchVNet-VPNGW.png "Configuración de puerta de enlace de VPN de la red virtual de sucursal"
[4]: ./media/backend-interoperability/ExR1.png "Configuración de ExpressRoute1"
[5]: ./media/backend-interoperability/ExR1-Hub-Connection.png "Configuración de conexión de ExpressRoute1 a la puerta de enlace ExR de la red virtual del concentrador"
[6]: ./media/backend-interoperability/ExR2.png "Configuración de ExpressRoute2"
[7]: ./media/backend-interoperability/ExR2-Hub-Connection.png "Configuración de conexión de ExpressRoute2 a la puerta de enlace ExR de la red virtual del concentrador"
[8]: ./media/backend-interoperability/ExR2-Remote-Connection.png "Configuración de conexión de ExpressRoute2 a la puerta de enlace ExR de la red virtual remota"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/networking/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha
[VNet-Config]: https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering




