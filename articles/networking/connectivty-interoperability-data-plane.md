---
title: 'Interoperabilidad de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET - Análisis del plano de datos: interoperabilidad de las características de conectividad de back-end de Azure | Microsoft Docs'
description: En esta página se proporciona el análisis del plano de datos de la configuración de prueba creada para analizar la interoperabilidad de las características de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute,vpn-gateway,virtual-network
ms.topic: article
ms.workload: infrastructure-services
ms.date: 10/18/2018
ms.author: rambala
ms.openlocfilehash: c9f3824b1e0f44338696ba3c2e434d60eee3af8b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2018
ms.locfileid: "49947155"
---
# <a name="interoperability-of-expressroute-site-to-site-vpn-and-vnet-peering---data-plane-analysis"></a>Interoperabilidad de ExpressRoute, VPN de sitio a sitio y emparejamiento de VNET: Análisis del plano de datos

En este artículo, se va a describir el análisis del plano de datos de la configuración de prueba. Para revisar la configuración de prueba, vea la [configuración de prueba][Setup]. Para revisar los detalles de la configuración de prueba, vea [Configuración de la prueba][Configuration]. Para revisar el análisis del plano de control de la configuración de prueba, vea [Análisis del plano de control][Control-Analysis].

El análisis del plano de datos examina la ruta de acceso de los paquetes que se desplazan desde una red local (LAN o red virtual) a otra dentro de una topología. Es posible que la ruta de acceso de datos entre dos redes locales no sea necesariamente simétrica. Por tanto, en este artículo se va a analizar la ruta de reenvío desde una red local a otra de forma independiente a la ruta de acceso inversa.

##<a name="data-path-from-hub-vnet"></a>Ruta de acceso de datos desde la red virtual del concentrador

###<a name="path-to-spoke-vnet"></a>Ruta de acceso a la red virtual de radio

El emparejamiento de VNET emula la funcionalidad de puente de red entre las dos redes virtuales emparejadas. A continuación se muestra la salida de traceroute desde una red virtual de concentrador a una máquina virtual en la red virtual de radio:

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.11.30.4

    Trace complete.

El recorte de pantalla siguiente es la vista de conexión gráfica de la red virtual del concentrador y la red virtual de radio presentada por Azure Network Watcher:


[![1]][1]

###<a name="path-to-branch-vnet"></a>Ruta de acceso a la red virtual de sucursal

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms     1 ms     1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     2 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

En el comando traceroute anterior, el primer salto es la puerta de enlace de VPN de la red virtual del concentrador. El segundo salto es la puerta de enlace de VPN de la red virtual de sucursal, cuya dirección IP no se anuncia en la red virtual del concentrador. El tercer salto es la máquina virtual en la red virtual de sucursal.

El recorte de pantalla siguiente es la vista de conexión gráfica de la red virtual del concentrador y la red virtual de sucursal presentada por Azure Network Watcher:

[![2]][2]

Para la misma conexión, el recorte de pantalla siguiente es la vista de cuadrícula presentada por Azure Network Watcher:

[![3]][3]

###<a name="path-to-on-premises-location-1"></a>Ruta de acceso a la Ubicación 1 local

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     2 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

En el comando traceroute anterior, el primer salto es el punto de conexión de túnel de puerta de enlace de ExpressRoute a MSEE. El segundo y el tercer saltos son, respectivamente, las direcciones IP LAN del enrutador CE y la Ubicación 1 local, que no se anuncian en la red virtual del concentrador. El cuarto salto es la máquina virtual en la Ubicación 1 local.


###<a name="path-to-on-premises-location-2"></a>Ruta de acceso a la Ubicación 2 local

    C:\Users\rb>tracert 10.1.31.10

    Tracing route to 10.1.31.10 over a maximum of 30 hops

      1    76 ms    75 ms    75 ms  10.10.30.134
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    75 ms    75 ms    75 ms  10.1.31.10

    Trace complete.

En el comando traceroute anterior, el primer salto es el punto de conexión de túnel de puerta de enlace de ExpressRoute a MSEE. El segundo y el tercer saltos son, respectivamente, las direcciones IP LAN del enrutador CE y la Ubicación 2 local, que no se anuncian en la red virtual del concentrador. El cuarto salto es la máquina virtual en la Ubicación 2 local.

###<a name="path-to-remote-vnet"></a>Ruta de acceso a la red virtual remota

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     2 ms     2 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    69 ms  10.17.30.4

    Trace complete.

En el comando traceroute anterior, el primer salto es el punto de conexión de túnel de puerta de enlace de ExpressRoute a MSEE. El segundo salto es la dirección IP de la puerta de enlace de la red virtual remota. El intervalo IP del segundo salto no se anuncia dentro de la red virtual del concentrador. El tercer salto es la máquina virtual en la red virtual remota.

##<a name="data-path-from-spoke-vnet"></a>Ruta de acceso de datos de la red virtual de radio

Recuerde que la red virtual de radio comparte la vista de red de la red virtual del concentrador. A través del emparejamiento VNET, la red virtual de radio usa la conectividad de puerta de enlace remota de la red virtual del concentrador como si estuvieran conectadas directamente a la red virtual de radio.

###<a name="path-to-hub-vnet"></a>Ruta de acceso a la red virtual del concentrador

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.10.30.4

    Trace complete.

###<a name="path-to-branch-vnet"></a>Ruta de acceso a la red virtual de sucursal

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.10.30.142
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

En el comando traceroute anterior, el primer salto es la puerta de enlace de VPN de la red virtual del concentrador. El segundo salto es la puerta de enlace de VPN de la red virtual de sucursal, cuya dirección IP no se anuncia en la red virtual de concentrador o de radio. El tercer salto es la máquina virtual en la red virtual de sucursal.

###<a name="path-to-on-premises-location-1"></a>Ruta de acceso a la Ubicación 1 local

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

En el comando traceroute anterior, el primer salto es el punto de conexión de túnel de puerta de enlace de ExpressRoute de la red virtual del concentrador a MSEE. El segundo y el tercer saltos son, respectivamente, las direcciones IP LAN del enrutador CE y la Ubicación 1 local, que no se anuncian en la red virtual del concentrador o de radio. El cuarto salto es la máquina virtual en la Ubicación 1 local.

###<a name="path-to-on-premises-location-2"></a>Ruta de acceso a la Ubicación 2 local

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    24 ms     2 ms     3 ms  10.10.30.132
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4     3 ms     2 ms     2 ms  10.2.30.10

    Trace complete.

En el comando traceroute anterior, el primer salto es el punto de conexión de túnel de puerta de enlace de ExpressRoute de la red virtual del concentrador a MSEE. El segundo y el tercer saltos son, respectivamente, las direcciones IP LAN del enrutador CE y la Ubicación 2 local, que no se anuncian en las redes virtuales de concentrador o de radio. El cuarto salto es la máquina virtual en la Ubicación 2 local.

###<a name="path-to-remote-vnet"></a>Ruta de acceso a la red virtual remota

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1     2 ms     1 ms     1 ms  10.10.30.133
      2     *        *        *     Request timed out.
      3    71 ms    70 ms    70 ms  10.17.30.4

    Trace complete.

En el comando traceroute anterior, el primer salto es el punto de conexión de túnel de puerta de enlace de ExpressRoute de la red virtual del concentrador a MSEE. El segundo salto es la dirección IP de la puerta de enlace de la red virtual remota. El intervalo IP del segundo salto no se anuncia dentro de la red virtual de concentrador o de radio. El tercer salto es la máquina virtual en la red virtual remota.

##<a name="data-path-from-branch-vnet"></a>Ruta de acceso de datos desde la red virtual de sucursal

###<a name="path-to-hub-vnet"></a>Ruta de acceso a la red virtual del concentrador

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     3 ms  10.10.30.4

    Trace complete.

En el comando traceroute anterior, el primer salto es la puerta de enlace de VPN de la red virtual de sucursal. El segundo salto es la puerta de enlace de VPN de la red virtual del concentrador, cuya dirección IP no se anuncia en la red virtual remota. El tercer salto es la máquina virtual en la red virtual del concentrador.

###<a name="path-to-spoke-vnet"></a>Ruta de acceso a la red virtual de radio

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1     1 ms    <1 ms     1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     4 ms     3 ms     2 ms  10.11.30.4

    Trace complete.

En el comando traceroute anterior, el primer salto es la puerta de enlace de VPN de la red virtual de sucursal. El segundo salto es la puerta de enlace de VPN de la red virtual del concentrador, cuya dirección IP no se anuncia en la red virtual remota, y el tercer salto es la máquina virtual en la red virtual de radio.

###<a name="path-to-on-premises-location-1"></a>Ruta de acceso a la Ubicación 1 local

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1     1 ms    <1 ms    <1 ms  10.11.30.100
      2     *        *        *     Request timed out.
      3     3 ms     2 ms     2 ms  10.2.30.125
      4     *        *        *     Request timed out.
      5     3 ms     3 ms     3 ms  10.2.30.10

    Trace complete.

En el comando traceroute anterior, el primer salto es la puerta de enlace de VPN de la red virtual de sucursal. El segundo salto es la puerta de enlace de VPN de la red virtual del concentrador, cuya dirección IP no se anuncia en la red virtual remota. El tercer salto es el punto de terminación de túnel VPN en el enrutador CE principal. El cuarto salto es una dirección IP interna de la dirección IP LAN de la Ubicación 1 local que no se anuncia fuera del enrutador CE. El quinto salto es la máquina virtual de destino en la Ubicación 1 local.

###<a name="path-to-on-premises-location-2-and-remote-vnet"></a>Ruta de acceso a la Ubicación 2 local y la red virtual remota

Como se explicó antes en el análisis del plano de control, la red virtual de sucursal no tiene visibilidad a la Ubicación 2 local ni a la red virtual remota según la configuración de red. Los resultados de ping siguientes lo confirman. 

    C:\Users\rb>ping 10.1.31.10

    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

    C:\Users\rb>ping 10.17.30.4

    Pinging 10.17.30.4 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.17.30.4:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

##<a name="data-path-from-on-premises-location-1"></a>Ruta de acceso de datos desde la Ubicación 1 local

###<a name="path-to-hub-vnet"></a>Ruta de acceso a la red virtual del concentrador

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     2 ms     2 ms     2 ms  10.10.30.4

    Trace complete.

En el comando traceroute anterior, los dos primeros saltos forman parte de la red local. El tercer salto es la interfaz MSEE principal que accede al enrutador CE. El cuarto salto es la puerta de enlace de ExpressRoute de la red virtual del concentrador, cuyo intervalo IP no se anuncia a la red local. El quinto salto es la máquina virtual de destino.

Azure Network Watcher solo proporciona una vista centrada en Azure. Por tanto, para la vista centrada en el entorno local se ha usado Azure Network Performance Monitor (NPM). NPM proporciona agentes que pueden instalar servidores de red fuera de Azure y realizar el análisis de la ruta de acceso de datos.

El recorte de pantalla siguiente es la vista de topología de la conectividad de la máquina virtual en la ubicación 1 local a la máquina virtual en la red virtual del concentrador a través de ExpressRoute.

[![4]][4]

Recuerde que en la configuración de prueba se usa VPN de sitio a sitio como conectividad de copia de seguridad para ExpressRoute entre la Ubicación 1 local y la red virtual del concentrador. Para volver a probar la ruta de acceso de datos, se va a inducir un error de conexión de ExpressRoute entre el enrutador CE principal de la Ubicación 1 local y el MSEE correspondiente apagando la interfaz de CE que accede al MSEE.

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     3 ms  10.10.30.4

    Trace complete.

El recorte de pantalla siguiente es la vista de topología de la conectividad de la máquina virtual en la ubicación 1 local a la máquina virtual en la red virtual del concentrador a través de la conectividad VPN de sitio a sitio cuando la de ExpressRoute está inactiva.

[![5]][5]

###<a name="path-to-spoke-vnet"></a>Ruta de acceso a la red virtual de radio

Volvamos a la conectividad de ExpressRoute principal para realizar el análisis de ruta de datos hacia la red virtual de radio.

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5     3 ms     2 ms     2 ms  10.11.30.4

    Trace complete.

Se recupera la conectividad de ExpressRoute 1 principal para el resto del análisis de la ruta acceso de datos.

###<a name="path-to-branch-vnet"></a>Ruta de acceso a la red virtual de sucursal

    C:\Users\rb>tracert 10.11.30.68

    Tracing route to 10.11.30.68 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2    <1 ms    <1 ms    <1 ms  192.168.30.0
      3     3 ms     2 ms     2 ms  10.11.30.68

    Trace complete.

###<a name="path-to-on-premises-location-2"></a>Ruta de acceso a la Ubicación 2 local

Como se explicó antes en el análisis del plano de control, la Ubicación 1 local no tiene visibilidad a la Ubicación 2 local según la configuración de red. Los resultados de ping siguientes lo confirman. 

    C:\Users\rb>ping 10.1.31.10
    
    Pinging 10.1.31.10 with 32 bytes of data:

    Request timed out.
    ...
    Request timed out.

    Ping statistics for 10.1.31.10:
        Packets: Sent = 4, Received = 0, Lost = 4 (100% loss),

###<a name="path-to-remote-vnet"></a>Ruta de acceso a la red virtual remota

    C:\Users\rb>tracert 10.17.30.4

    Tracing route to 10.17.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.2.30.3
      2     2 ms     5 ms     7 ms  192.168.30.0
      3    <1 ms    <1 ms    <1 ms  192.168.30.18
      4     *        *        *     Request timed out.
      5    69 ms    70 ms    69 ms  10.17.30.4

    Trace complete.

##<a name="data-path-from-on-premises-location-2"></a>Ruta de acceso de datos desde la Ubicación 2 local

###<a name="path-to-hub-vnet"></a>Ruta de acceso a la red virtual del concentrador

    C:\Windows\system32>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    <1 ms    <1 ms    <1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.4
      3    <1 ms    <1 ms    <1 ms  192.168.31.22
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.10.30.4

    Trace complete.

###<a name="path-to-spoke-vnet"></a>Ruta de acceso a la red virtual de radio

    C:\Windows\system32>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops
      1    <1 ms    <1 ms     1 ms  10.1.31.3
      2    <1 ms    <1 ms    <1 ms  192.168.31.0
      3    <1 ms    <1 ms    <1 ms  192.168.31.18
      4     *        *        *     Request timed out.
      5    75 ms    74 ms    74 ms  10.11.30.4

    Trace complete.

###<a name="path-to-branch-vnet-on-premises-location-1-and-remote-vnet"></a>Ruta de acceso a la red virtual de sucursal, la Ubicación 1 local y la red virtual remota

Como se explicó antes en el análisis del plano de control, la Ubicación 1 local no tiene visibilidad a la red virtual de sucursal, la Ubicación 1 local y la red virtual remota según la configuración de red. 

##<a name="data-path-from-remote-vnet"></a>Ruta de acceso de datos desde la red virtual remota

###<a name="path-to-hub-vnet"></a>Ruta de acceso a la red virtual del concentrador

    C:\Users\rb>tracert 10.10.30.4

    Tracing route to 10.10.30.4 over a maximum of 30 hops

      1    65 ms    65 ms    65 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    69 ms    68 ms    68 ms  10.10.30.4

    Trace complete.

###<a name="path-to-spoke-vnet"></a>Ruta de acceso a la red virtual de radio

    C:\Users\rb>tracert 10.11.30.4

    Tracing route to 10.11.30.4 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3    71 ms    69 ms    69 ms  10.11.30.4

    Trace complete.

### <a name="path-to-branch-vnet-and-on-premises-location-2"></a>Ruta de acceso a la red virtual de sucursal y la Ubicación 2 local

Como se explicó antes en el análisis del plano de control, la red virtual remota no tiene visibilidad a la red virtual de sucursal ni a la Ubicación 2 local según la configuración de red. 


### <a name="path-to-on-premises-location-1"></a>Ruta de acceso a la Ubicación 1 local

    C:\Users\rb>tracert 10.2.30.10

    Tracing route to 10.2.30.10 over a maximum of 30 hops

      1    67 ms    67 ms    67 ms  10.17.30.36
      2     *        *        *     Request timed out.
      3     *        *        *     Request timed out.
      4    69 ms    69 ms    69 ms  10.2.30.10

    Trace complete.


## <a name="further-reading"></a>Lecturas adicionales

### <a name="using-expressroute-and-site-to-site-vpn-connectivity-in-tandem"></a>Uso conjunto de ExpressRoute y la conectividad VPN de sitio a sitio

####<a name="site-to-site-vpn-over-expressroute"></a>VPN de sitio a sitio a través de ExpressRoute

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

Para obtener información sobre cuántos circuitos de ExpressRoute se pueden conectar a una puerta de enlace de ExpressRoute, cuántas puertas de enlace de ExpressRoute se pueden conectar a un circuito de ExpressRoute, o bien para obtener información sobre otros límites de escala de ExpressRoute, vea [P+F de ExpressRoute][ExR-FAQ].


<!--Image References-->
[1]: ./media/backend-interoperability/HubVM-SpkVM.jpg "Vista de Network Watcher de la conectividad desde la red virtual del concentrador a la red virtual de radio"
[2]: ./media/backend-interoperability/HubVM-BranchVM.jpg "Vista de Network Watcher de la conectividad desde la red virtual del concentrador a la red virtual de sucursal"
[3]: ./media/backend-interoperability/HubVM-BranchVM-Grid.jpg "Vista de cuadrícula de Network Watcher de la conectividad desde la red virtual del concentrador a la red virtual de sucursal"
[4]: ./media/backend-interoperability/Loc1-HubVM.jpg "Vista de Network Performance Monitor de la conectividad desde la máquina virtual en la Ubicación 1 a la red virtual del concentrador a través de ExpressRoute 1"
[5]: ./media/backend-interoperability/Loc1-HubVM-S2S.jpg "Vista de Network Performance Monitor de la conectividad desde la máquina virtual en la Ubicación 1 a la red virtual del concentrador a través de VPN de sitio a sitio"

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




