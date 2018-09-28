---
title: Información general Azure Virtual WAN | Microsoft Docs
description: Obtenga información sobre la conectividad escalable y automatizada de rama a rama de Azure Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 09/21/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: a21b97f603fc28cdbba8983547ce958a34df0774
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968601"
---
# <a name="what-is-azure-virtual-wan"></a>¿Qué es Azure Virtual WAN?

Azure Virtual WAN es un servicio de redes que ofrece conectividad de rama a rama automatizada y optimizada mediante Azure. Azure Virtual WAN permite conectar y configurar los dispositivos de rama para comunicarse con Azure. Esto puede realizarse manualmente o con dispositivos de asociados preferidos mediante un asociado de Virtual WAN. Consulte el artículo [Asociados preferidos](https://go.microsoft.com/fwlink/p/?linkid=2019615) para obtener más información. El uso de dispositivos de asociados preferidos permite la administración de la configuración, facilita el uso y simplifica la conectividad. El panel integrado de Azure WAN proporciona información instantánea para solucionar problemas que puede ayudarle a ahorrar tiempo y le ofrece una manera fácil de ver la conectividad a gran escala.

![Diagrama de Virtual WAN](./media/virtual-wan-about/virtualwan.png)

En este artículo se proporciona una vista rápida de la conectividad de red de las cargas de trabajo de Azure y de las que no son de Azure. Virtual WAN ofrece las siguientes ventajas:

* **Soluciones integradas de conectividad en concentrador y radio:** configuración de sitio a sitio automatizada y conectividad entre sitios locales y un concentrador de Azure.
* **Configuración e instalación automatizadas de radio:** conecte sin problemas las cargas de trabajo y las redes virtuales al concentrador de Azure.
* **Solución de problemas intuitiva:** puede ver el flujo de un extremo a otro dentro de Azure y usar esta información para tomar las acciones necesarias.

## <a name="s2s"></a>Conexiones de sitio a sitio

Para crear una conexión de sitio a sitio mediante Virtual WAN, puede continuar mediante un [asociado de Virtual WAN](https://go.microsoft.com/fwlink/p/?linkid=2019615) o crear la conexión manualmente.

### <a name="s2spartner"></a>Trabajo con un asociado de Virtual WAN

Cuando se trabaja con un asociado de Virtual WAN, el proceso es el siguiente:

1. El controlador del dispositivo de rama (VPN/SDWAN) se autentica para exportar información centrada en el sitio en Azure mediante el uso de una [entidad de servicio de Azure](../azure-resource-manager/resource-group-create-service-principal-portal.md).
2. El controlador de dispositivo (VPN/SDWAN) de rama obtiene la configuración de conectividad de Azure y actualiza el dispositivo local. De esta forma se automatiza la descarga de la configuración, la edición y la actualización del dispositivo VPN local.
3. Una vez que el dispositivo tiene la configuración de Azure correcta, se establece una conexión de sitio a sitio (dos túneles activos) a Azure WAN. Azure admite tanto IKEv1 como IKEv2. BGP es opcional.


Si no desea usar un asociado preferido, puede configurar la conexión manualmente y consultar [Creación de una conexión de sitio a sitio mediante Virtual WAN ](virtual-wan-site-to-site-portal.md).

## <a name="p2s"></a>Conexiones de punto a sitio (versión preliminar)

Para crear la conexión manualmente, consulte [Create a point-to-site connection using Virtual WAN](https://go.microsoft.com/fwlink/p/?linkid=2020051&clcid) (Creación de una conexión de punto a sitio mediante Virtual WAN).

## <a name="er"></a>Conexiones de ExpressRoute (versión preliminar)

Para crear la conexión manualmente, consulte [Create an ExpressRoute connection using Virtual WAN](https://go.microsoft.com/fwlink/p/?linkid=2020148&clcid) (Creación de una conexión de ExpressRoute mediante Virtual WAN).


## <a name="resources"></a>Recursos de Virtual WAN

Para configurar una red Virtual WAN de extremo a otro, debe crear los siguientes recursos:

* **virtualWAN:** este recurso representa una superposición virtual de la red de Azure y es una colección de varios recursos. Contiene vínculos a todos los concentradores virtuales que le gustaría tener dentro de Virtual WAN. Los recursos de Virtual WAN están aislados entre sí y no pueden contener un concentrador común. Los concentradores virtuales a través de Virtual WAN no se comunican entre sí. La propiedad "Permitir el tráfico de rama a rama" permite el tráfico entre sitios VPN, así como VPN en sitios habilitados para ExpressRoute. Tenga en cuenta que ExpressRoute en Azure Virtual WAN está actualmente en versión preliminar.

* **Sitio:** el recurso de sitio conocido como vpnsite representa el dispositivo VPN local y su configuración. Al trabajar con un asociado de Virtual WAN, tiene una solución integrada para exportar automáticamente esta información a Azure.

* **Concentrador:** un concentrador virtual es una red virtual administrada por Microsoft. El concentrador contiene varios puntos de conexión de servicio para habilitar la conectividad de la red local (vpnsite). El concentrador es el núcleo de la red en una región. Solo puede haber un único concentrador por cada región de Azure. Cuando se crea un concentrador mediante Azure Portal, se crea una red virtual del concentrador virtual y un elemento vpngateway de concentrador virtual.

  Una puerta de enlace de concentrador no es lo mismo que la puerta de enlace de red virtual que usó para ExpressRoute y VPN Gateway. Por ejemplo, cuando se usa Virtual WAN, no se crea una conexión de sitio a sitio desde su sitio local directamente a la red virtual. En su lugar, se crea una conexión de sitio a sitio al concentrador. El tráfico siempre pasa a través de la puerta de enlace del concentrador. Esto significa que las redes virtuales no necesitan su propia puerta de enlace de red virtual. Virtual WAN permite que las redes virtuales obtengan provecho del escalado fácilmente mediante el concentrador virtual y su puerta de enlace. 

* **Conexión de red virtual del concentrador:** el recurso de conexión de red virtual se usa para conectar el concentrador sin problemas a la red virtual. En este momento, solo puede conectarse a las redes virtuales que están en la misma región central.

* **Tabla de rutas de concentrador:** puede crear una ruta de concentrador virtual y aplicarla a la tabla de rutas de concentrador virtual. Puede aplicar varias rutas a la tabla de rutas del concentrador virtual.

## <a name="faq"></a>P+F

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]


## <a name="next-steps"></a>Pasos siguientes

* Consulte [Ubicaciones y asociados de Virtual WAN](https://aka.ms/virtualwan)