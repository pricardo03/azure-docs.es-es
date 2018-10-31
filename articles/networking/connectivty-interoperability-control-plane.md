---
title: 'Interoperabilidad de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET - Análisis del plano de control: interoperabilidad de las características de conectividad de back-end de Azure | Microsoft Docs'
description: En esta página se proporciona el análisis del plano de control de la configuración de prueba creada para analizar la interoperabilidad de las características de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: ee887da18b5666e61bc25365791b2e7dffb925e0
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947159"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---control-plane-analysis"></a>Interoperabilidad de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET: Análisis del plano de control

En este artículo, se va a analizar el análisis del plano de control de la configuración de prueba. Si quiere revisar la configuración de prueba, vea la [configuración de prueba][Setup]. Para revisar los detalles de la configuración de prueba, vea [Configuración de la prueba][Configuration].

En esencia, el análisis del plano de control examina las rutas que se intercambian entre las redes dentro de una topología. El análisis del plano de control ayuda a la forma que tiene cada red de ver la topología.

##<a name="hub-and-spoke-vnet-perspective"></a>Perspectiva de red virtual de tipo hub-and-spoke

En el diagrama siguiente se ilustra la red desde la perspectiva de la red virtual del concentrador y la red virtual de radio (resaltada en azul). En el diagrama también se muestra el número de sistema autónomo (ASN) de las redes y rutas que se intercambian entre las distintas redes. 

[![1]][1]

Tenga en cuenta que el ASN de la puerta de enlace de ExpressRoute de la red virtual es diferente al de los enrutadores de Microsoft Enterprise Edge (MSEE). La puerta de enlace de ExpressRoute usa un ASN privado (65515) y los MSEE usan un ASN público (12076) de forma global. Al configurar el emparejamiento de ExpressRoute, como MSEE es del mismo nivel, se usa 12076 como el ASN del mismo nivel. En el lado de Azure, MSEE establece emparejamiento de EBGP con la puerta de enlace de ExpressRoute. El emparejamiento EBGP dual que el MSEE establece para cada emparejamiento de ExpressRoute es transparente en el nivel del plano de control. Por tanto, cuando se ve una tabla de enrutamiento de ExpressRoute, se ve el ASN de la puerta de enlace de ExpressRoute de la red virtual para los prefijos de la red virtual. A continuación se muestra una captura de pantalla de una tabla de rutas de ExpressRoute de ejemplo: 

[![5]][5]

Dentro de Azure, el ASN solo es significativo desde una perspectiva de emparejamiento. De forma predeterminada, el ASN de la puerta de enlace de ExpressRoute y el de la de VPN es 65515.

##<a name="on-premises-location-1-and-remote-vnet-perspective-via-expressroute-1"></a>Perspectiva de la Ubicación 1 local y la red virtual remota a través de ExpressRoute 1

La Ubicación 1 local y la red virtual remota están conectadas a la red virtual del concentrador a través de ExpressRoute 1 y, por tanto, comparten la misma perspectiva de la topología, como se muestra en el diagrama siguiente.

[![2]][2]

##<a name="on-premises-location-1-and-branch-vnet-perspective-via-site-to-site-vpn"></a>Perspectiva de la Ubicación 1 local y la red virtual de sucursal a través de VPN de sitio a sitio

La Ubicación 1 local y la red virtual de sucursal están conectadas a la puerta de enlace de la red virtual del concentrador a través de conexiones VPN de sitio a sitio y, por tanto, comparten la misma perspectiva de la topología, como se muestra en el diagrama siguiente.

[![3]][3]

##<a name="on-premises-location-2-perspective"></a>Perspectiva de la Ubicación 2 local

La Ubicación 2 local está conectada a la red virtual del concentrador a través del emparejamiento privado de ExpressRoute 2. 

[![4]][4]

## <a name="further-reading"></a>Lecturas adicionales

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Uso conjunto de ExpressRoute y la conectividad VPN de sitio a sitio

####  <a name="site-to-site-vpn-over-expressroute"></a>VPN de sitio a sitio a través de ExpressRoute

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

Para el análisis del plano de datos de la configuración de prueba y las vistas de las características de supervisión de red de Azure, vea [Data-Plane Analysis][Data-Analysis] (Análisis del plano de datos).

Para obtener información sobre cuántos circuitos de ExpressRoute se pueden conectar a una puerta de enlace de ExpressRoute, cuántas puertas de enlace de ExpressRoute se pueden conectar a un circuito de ExpressRoute, o bien para obtener información sobre otros límites de escala de ExpressRoute, vea [P+F de ExpressRoute][ExR-FAQ].


<!--Image References-->
[1]: ./media/backend-interoperability/HubView.png "Perspectiva de red virtual de tipo hub-and-spoke de la topología"
[2]: ./media/backend-interoperability/Loc1ExRView.png "Perspectiva de la topología de la Ubicación 1 y la red virtual remota a través de ExpressRoute 1"
[3]: ./media/backend-interoperability/Loc1VPNView.png "Perspectiva de la topología de la Ubicación 1 y la red virtual de sucursal a través de VPN de sitio a sitio"
[4]: ./media/backend-interoperability/Loc2View.png "Perspectiva de la topología de la Ubicación 2"
[5]: ./media/backend-interoperability/ExR1-RouteTable.png "Tabla de rutas de ExpressRoute 1"

<!--Link References-->
[Setup]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-preface
[Configuration]: https://docs.microsoft.com/azure/networking/connectivty-interoperability-config
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




