---
title: 'Interoperabilidad de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET - Configuración de prueba: interoperabilidad de las características de conectividad de back-end de Azure | Microsoft Docs'
description: En esta página se proporciona una configuración de prueba que se usa para analizar la interoperabilidad de las características de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: e859a0a3ac35a9d9f2dab579b7609192e599f90f
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947157"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---test-setup"></a>Interoperabilidad de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET: configuración de prueba
En este artículo, se va a identificar una configuración de prueba que se puede usar para analizar cómo interactúan las diferentes características entre sí, tanto en el nivel de plano de control como en el del plano de datos. Antes de describir la configuración de prueba, se analizará brevemente el significado de estas características de red de Azure.

ExpressRoute: mediante el emparejamiento privado de ExpressRoute se pueden conectar directamente espacios IP privados de la red local a las implementaciones de red virtual de Azure.  Con ExpressRoute puede lograr un ancho de banda mayor y conexión privada. Hay muchos asociados de ExpressRoute, que ofrecen conectividad de ExpressRoute con Acuerdo de Nivel de Servicio. Para obtener más información sobre ExpressRoute y cómo configurarlo, vea [Información general sobre ExpressRoute][ExpressRoute].

VPN de sitio a sitio: para conectar de forma segura una red local a Azure a través de Internet o ExpressRoute, dispone de la opción VPN de sitio a sitio (S2S). Para obtener información sobre cómo configurar una VPN de sitio a sitio para conectarse a Azure, vea [Configuración de VPN Gateway][VPN].

Emparejamiento de VNET: el emparejamiento de VNET está disponible para establecer la conectividad entre redes virtuales (VNet). Para obtener más información sobre el emparejamiento de VNET, vea [Conexión de redes virtuales con emparejamiento de redes virtuales][VNet].

##<a name="test-setup"></a>Prueba de configuración

En el diagrama siguiente se muestra la configuración de prueba.

[![1]][1]

La pieza central de la configuración de prueba es la red virtual del concentrador en la región 1 de Azure. La red virtual del concentrador se conecta a otras redes como se muestra a continuación:

1.  A la red virtual de radio a través del emparejamiento de VNET. La red virtual de radio tiene acceso remoto a las dos puertas de enlace en la red virtual del concentrador.
2.  A la red virtual de sucursal a través de VPN de sitio a sitio. La conectividad usa EBGP para intercambiar las rutas.
3.  A la red local de Ubicación 1 a través del emparejamiento privado de ExpressRoute como ruta de acceso principal y la conectividad VPN de sitio a sitio como ruta de acceso de respaldo. En el resto de este documento, se hará referencia a este circuito de ExpressRoute como ExpressRoute1. De forma predeterminada, los circuitos de ExpressRoute proporcionan conectividad redundante para alta disponibilidad. En ExpressRoute1, la subinterfaz del enrutador CE secundario que accede al MSEE secundario está deshabilitada. Esto se indica mediante una línea de color rojo sobre la flecha de doble línea en el diagrama anterior.
4.  A la red local de Ubicación 2 a través de otro emparejamiento privado de ExpressRoute. En el resto de este documento, se hará referencia a este segundo circuito de ExpressRoute como ExpressRoute2.
5.  ExpressRoute1 también conecta la red virtual del concentrador y la red local de Ubicación 1 a una red virtual remota en la región 2 de Azure.

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

Para obtener los detalles de configuración de la topología de prueba, vea [Detalles de configuración][Configuration].

Para el análisis del plano de control de la configuración de prueba y comprender las vistas de otras redes virtuales y VLAN de la topología, vea [Control-Plane Analysis][Control-Analysis] (Análisis del plano de control).

Para el análisis del plano de datos de la configuración de prueba y las vistas de las características de supervisión de red de Azure, vea [Data-Plane Analysis][Data-Analysis] (Análisis del plano de datos).

Para obtener información sobre cuántos circuitos de ExpressRoute se pueden conectar a una puerta de enlace de ExpressRoute, cuántas puertas de enlace de ExpressRoute se pueden conectar a un circuito de ExpressRoute, o bien para obtener información sobre otros límites de escala de ExpressRoute, vea [P+F de ExpressRoute][ExR-FAQ].



<!--Image References-->
[1]: ./media/backend-interoperability/TestSetup.png "La topología de prueba"

<!--Link References-->
[ExpressRoute]: https://docs.microsoft.com/azure/expressroute/expressroute-introduction
[VPN]: https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways
[VNet]: https://docs.microsoft.com/azure/virtual-network/tutorial-connect-virtual-networks-portal
[Configuration]: https://docs.microsoft.com/azure/connectivty-interoperability-configuration
[Control-Analysis]:https://docs.microsoft.com/azure/connectivty-interoperability-control-plane
[Data-Analysis]: https://docs.microsoft.com/azure/connectivty-interoperability-data-plane
[ExR-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[S2S-Over-ExR]: https://docs.microsoft.com/azure/expressroute/site-to-site-vpn-over-microsoft-peering
[ExR-S2S-CoEx]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-coexist-resource-manager
[Hub-n-Spoke]: https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke
[Deploy-NVA]: https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha




