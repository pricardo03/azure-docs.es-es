---
title: Conexión de la red local a Azure | Microsoft Docs
description: Explica y compara los distintos métodos disponibles para conectarse a servicios de nube de Microsoft como Azure, Office 365 y Dynamics CRM Online.
services: ''
documentationcenter: na
author: cherylmc
manager: timlt
editor: ''
tags: ''
ms.assetid: 294d39ad-40e0-476a-a362-57911b1172b7
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2016
ms.author: cherylmc
ms.openlocfilehash: 9361c0b2ee84d7459bd432f5a8ce13fc5fd85ef8
ms.sourcegitcommit: f331186a967d21c302a128299f60402e89035a8d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58114381"
---
# <a name="connecting-your-on-premises-network-to-azure"></a>Conexión de la red local a Azure
Microsoft proporciona varios tipos de servicios en la nube. Aunque puede conectarse a todos los servicios a través de Internet, también puede conectarse a algunos de los servicios mediante un túnel de red privada virtual (VPN) a través de Internet o a través de una conexión privada directa a Microsoft. Este artículo le ayudará a determinar qué opción de conectividad mejor se adapta a sus necesidades en función de los tipos de servicios de nube de Microsoft que consume. La mayoría de las organizaciones usan varios tipos de conexión que se describe a continuación.

## <a name="connecting-over-the-public-internet"></a>Conectarse a través de Internet
Este tipo de conexión proporciona acceso a servicios en la nube de Microsoft directamente a través de Internet, como se muestra a continuación.

![Internet](./media/guidance-connecting-your-on-premises-network-to-azure/internet.png "Internet")

Esta conexión normalmente es el primer tipo que se usa para conectarse a servicios en la nube de Microsoft. En la tabla siguiente enumera las ventajas y desventajas de este tipo de conexión.

| **Ventajas** | **Consideraciones** |
| --- | --- |
| No requiere efectuar ninguna modificación a la red local, siempre y cuando todos los dispositivos cliente tienen acceso ilimitado a todas las direcciones IP y puertos en Internet. |Aunque el tráfico a menudo se cifra mediante HTTPS, puede interceptar en tránsito, ya que lo atraviesa la Internet pública. |
| Puede conectarse a todos los servicios de nube de Microsoft expuestos a Internet pública. |Una latencia porque la conexión atraviesa Internet. |
| Usa la conexión a Internet existente. | |
| No requiere administración de los dispositivos de conectividad. | |

Esta conexión no tiene conectividad o los costos de ancho de banda, ya que usar la conexión a Internet existente.

## <a name="connecting-with-a-point-to-site-connection"></a>Conectar con una conexión de punto a sitio
Este tipo de conexión proporciona acceso a algunos servicios de nube de Microsoft a través de un túnel del protocolo de túnel de sockets seguros (SSTP) a través de Internet, como se muestra a continuación.

![P2S](./media/guidance-connecting-your-on-premises-network-to-azure/p2s.png "conexiónpunto a sitio")

La conexión se realiza a través de la conexión a Internet existente, pero requiere el uso de una puerta de enlace de VPN de Azure. En la tabla siguiente enumera las ventajas y desventajas de este tipo de conexión.

| **Ventajas** | **Consideraciones** |
| --- | --- |
| No requiere efectuar ninguna modificación a la red local, siempre y cuando todos los dispositivos cliente tienen acceso ilimitado a todas las direcciones IP y puertos en Internet. |Aunque el tráfico se cifra con IPSec, puede interceptar en tránsito, ya que lo atraviesa la Internet pública. |
| Usa la conexión a Internet existente. |Una latencia porque la conexión atraviesa Internet. |
| Rendimiento de hasta 200 Mb/s por puerta de enlace. |Requiere la creación y administración de conexiones independientes entre cada dispositivo en la red local y en cada dispositivo necesita conectarse a cada puerta de enlace. |
| Puede utilizarse para conectarse a servicios de Azure que se pueden conectar a una Azure Virtual Network (VNet), como las máquinas virtuales de Azure y Azure Cloud Services. |Requiere muy poca administración continua de una puerta de enlace de VPN de Azure. |
| No se puede usar para conectarse a Microsoft Office 365 o Dynamics CRM Online. | |
| No se puede usar para conectarse a servicios de Azure que no se puede conectar a una red virtual. | |

Obtenga más información sobre la [puerta de enlace VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md) servicio, su [precios](https://azure.microsoft.com/pricing/details/vpn-gateway)y la transferencia de datos de salida [precios](https://azure.microsoft.com/pricing/details/data-transfers).

## <a name="connecting-with-a-site-to-site-connection"></a>Conectar con una conexión de sitio a sitio
Este tipo de conexión proporciona acceso a algunos servicios de nube de Microsoft a través de un túnel IPSec a través de Internet, como se muestra a continuación.

![S2S](./media/guidance-connecting-your-on-premises-network-to-azure/s2s.png "conexión sitio a sitio")

La conexión se realiza a través de la conexión a Internet existente, pero requiere el uso de una puerta de enlace de VPN de Azure con su asociado de precios y los precios de transferencia de datos de salida. En la tabla siguiente enumera las ventajas y desventajas de este tipo de conexión.

| **Ventajas** | **Consideraciones** |
| --- | --- |
| Todos los dispositivos de la red local pueden comunicarse con servicios de Azure conectados a una red virtual, por lo que no es necesario configurar las conexiones individuales para cada dispositivo. |Aunque el tráfico se cifra con IPSec, puede interceptar en tránsito, ya que lo atraviesa la Internet pública. |
| Usa la conexión a Internet existente. |Una latencia porque la conexión atraviesa Internet. |
| Puede utilizarse para conectarse a servicios de Azure que se pueden conectar a una red virtual, como las máquinas virtuales y servicios en la nube. |Debe configurar y administrar un dispositivo VPN validado * en el entorno local. |
| Rendimiento de hasta 200 Mb/s por puerta de enlace. |Requiere muy poca administración continua de una puerta de enlace de VPN de Azure. |
| Puede forzar el tráfico saliente iniciado desde las máquinas virtuales de nube a través de la red local para la inspección y registro mediante las rutas definidas por el usuario o el protocolo de puerta de enlace de borde (BGP) **. |No se puede usar para conectarse a Microsoft Office 365 o Dynamics CRM Online. |
| No se puede usar para conectarse a servicios de Azure que no se puede conectar a una red virtual. | |
| Si usa los servicios que inician las conexiones a los dispositivos de forma local y lo requieren las directivas de seguridad, necesita un firewall entre la red local y Azure. | |

* * Ver una lista de [dispositivos VPN validados](../vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable).
* ** Obtener más información sobre el uso de [las rutas definidas por el usuario](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) o [BGP](../vpn-gateway/vpn-gateway-bgp-overview.md) para forzar el enrutamiento de redes virtuales de Azure a un dispositivo de forma local.

## <a name="connecting-with-a-dedicated-private-connection"></a>Conectar con una conexión privada dedicada
Este tipo de conexión proporciona acceso a todos los servicios de nube de Microsoft a través de una conexión privada dedicada a Microsoft que no atraviesa Internet, como se muestra a continuación.

![ER](./media/guidance-connecting-your-on-premises-network-to-azure/er.png "conexión de ExpressRoute")

La conexión requiere el uso del servicio de ExpressRoute y una conexión a un proveedor de conectividad. En la tabla siguiente enumera las ventajas y desventajas de este tipo de conexión.

| **Ventajas** | **Consideraciones** |
| --- | --- |
| El tráfico no se puede interceptar en tránsito a través de Internet ya que se usa una conexión dedicada a través de un proveedor de servicios. |Requiere la administración del enrutador local. |
| Ancho de banda de hasta 10 Gb/s por circuito de ExpressRoute y el rendimiento de hasta 2 Gb/s para cada puerta de enlace. |Requiere una conexión dedicada a un proveedor de conectividad. |
| Latencia previsible porque es una conexión dedicada a Microsoft que no atraviesa Internet. |Puede requerir la mínima administración continuada de uno o más puertas de enlace de la VPN de Azure (si se conecta el circuito a redes virtuales). |
| No requiere comunicación cifrada, aunque puede cifrar el tráfico, si lo desea. |Si usa servicios en la nube que inician conexiones a los dispositivos de forma local, necesita un firewall entre la red local y Azure. |
| Puede conectarse directamente a todos los servicios de nube de Microsoft, con algunas excepciones *. |Requiere la traducción de direcciones de red (NAT) de direcciones IP local escribir los centros de datos de Microsoft para los servicios que no se puede conectar a VNet.* * |
| Puede forzar el tráfico saliente iniciado desde las máquinas virtuales de nube a través de la red local para la inspección y registro de uso de BGP. | |

* * Para ver una [lista de servicios](../expressroute/expressroute-faqs.md#supported-services) que no se puede usar con ExpressRoute. Su suscripción de Azure debe estar aprobada para conectarse a Office 365.  Consulte la [Azure ExpressRoute para Office 365](https://support.office.com/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd?ui=en-US&rs=en-US&ad=US&fromAR=1) artículo para obtener más información.
* ** Obtener más información sobre ExpressRoute [NAT](../expressroute/expressroute-nat.md) requisitos.

Obtenga más información sobre [ExpressRoute](../expressroute/expressroute-introduction.md), su asociado [precios](https://azure.microsoft.com/pricing/details/expressroute), y [proveedores de conectividad](../expressroute/expressroute-locations.md#locations).

## <a name="additional-considerations"></a>Consideraciones adicionales
* Varias de las opciones anteriores tienen distintos límites máximos que se permiten para las conexiones de red virtual, las conexiones de puerta de enlace y otros criterios. Se recomienda que revise Azure [límites de redes](../azure-subscription-service-limits.md#networking-limits) para comprender si cualquiera de ellos afecta a los tipos de conectividad opta por usar.
* Si tiene previsto conectar una puerta de enlace de una conexión VPN de sitio a sitio a la misma red virtual como una puerta de enlace de ExpressRoute, debe familiarizarse primero con limitaciones importantes. Consulte la [conexiones coexistentes de sitio a sitio y de configurar ExpressRoute](../expressroute/expressroute-howto-coexist-resource-manager.md#limits-and-limitations) artículo para obtener más detalles.

## <a name="next-steps"></a>Pasos siguientes
Los recursos siguientes explican cómo implementar los tipos de conexión que se tratan en este artículo.

* [Implementar una conexión punto a sitio](../vpn-gateway/vpn-gateway-howto-point-to-site-rm-ps.md)
* [Implementar una conexión de sitio a sitio](guidance-hybrid-network-vpn.md)
* [Implementar una conexión privada dedicada](guidance-hybrid-network-expressroute.md)
* [Implementar una conexión privada dedicada con una conexión de sitio a sitio para la alta disponibilidad](guidance-hybrid-network-expressroute-vpn-failover.md)
