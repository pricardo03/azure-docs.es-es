---
title: 'Acerca de las puertas de enlace de red virtual de ExpressRoute: Azure| Microsoft Docs'
description: Conozca sobre las puertas de enlace de red virtual para ExpressRoute. En este artículo incluye información sobre los tipos y las SKU de puerta de enlace.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: 18615cf737eedcd188fd59d2aa98482210b9333a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65991587"
---
# <a name="expressroute-virtual-network-gateway-and-fastpath"></a>Puerta de enlace de red virtual de ExpressRoute y FastPath
Para conectar una red virtual de Azure y una red local a través de ExpressRoute, primero debe crear una puerta de enlace de red virtual. Una puerta de enlace de red virtual tiene dos propósitos: intercambiar las rutas de IP entre las redes y enrutar el tráfico de red. En este artículo se explican los tipos de puerta de enlace, las SKU de puerta de enlace y el rendimiento estimado por SKU. En este artículo también se explica [FastPath](#fastpath) de ExpressRoute, una característica que permite que el tráfico de red desde la red local omita la puerta de enlace de red virtual para mejorar el rendimiento.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gateway-types"></a>Tipos de puerta de enlace

Al crear una puerta de enlace de red virtual, debe especificar varios valores de configuración. Uno de los valores de configuración necesarios, "-GatewayType", especifica si la puerta de enlace se usará para el tráfico VPN o de ExpressRoute. Los dos tipos de puerta de enlace son los siguientes:

* **Vpn**: para enviar tráfico cifrado a través de una conexión a Internet pública, use la puerta de enlace de tipo "Vpn". Este tipo también se conoce como VPN Gateway. Las conexiones de sitio a sitio, de punto a sitio y de red virtual a red virtual utilizan una puerta de enlace de VPN.

* **ExpressRoute**: para enviar tráfico de red en una conexión privada, use la puerta de enlace de tipo "ExpressRoute". Este tipo también se conoce como puerta de enlace de ExpressRoute y es el tipo de puerta de enlace que se utiliza al configurar ExpressRoute.

Cada red virtual tiene una única puerta de enlace de red virtual por cada tipo de puerta de enlace. Por ejemplo, puede tener una puerta de enlace de una red virtual que use -GatewayType Vpn y otra que use -GatewayType ExpressRoute.

## <a name="gwsku"></a>SKU de puerta de enlace
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Si desea actualizar la puerta de enlace a una SKU de puerta de enlace más eficaz, en la mayoría de los casos puede usar el cmdlet de PowerShell "Resize-AzVirtualNetworkGateway". Esto funcionará para las actualizaciones de SKU Standard y HighPerformance. Sin embargo, para actualizar a la SKU UltraPerformance, debe volver a crear la puerta de enlace. Volver a crear una puerta de enlace provoca un tiempo de inactividad.

### <a name="aggthroughput"></a>Rendimientos estimados por SKU de puerta de enlace
En la tabla siguiente se muestran los tipos de puerta de enlace y los rendimientos estimados. Esta tabla se aplica a los modelos de implementación del Administrador de recursos y clásico.

[!INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)]

> [!IMPORTANT]
> El rendimiento de la aplicación depende de varios factores, como la latencia de extremo a extremo y el número de flujos de tráfico que abre la aplicación. Los números de la tabla representan el límite superior que teóricamente la aplicación puede alcanzar en un entorno ideal.
>
>

### <a name="zrgw"></a>SKU de puerta de enlace con redundancia de zona

También puede implementar puertas de enlace de ExpressRoute en Azure Availability Zones. De este modo, se dividen física y lógicamente en distintas Availability Zones, lo que protege la conectividad de red local con Azure de errores de nivel de zona.

![Puerta de enlace de ExpressRoute con redundancia de zona](./media/expressroute-about-virtual-network-gateways/zone-redundant.png)

Las puertas de enlace con redundancia de zona utilizan nuevas SKU de puerta de enlace específicas para la puerta de enlace de ExpressRoute.

* ErGw1AZ
* ErGw2AZ
* ErGw3AZ

Las nuevas SKU de puerta de enlace admiten también otras opciones de implementación que se ajusten mejor sus necesidades. Si crea una puerta de enlace de red virtual con las nuevas SKU de puerta de enlace, tiene además la opción de implementar la puerta de enlace en una zona determinada. Esto se conoce como puerta de enlace zonal. Al implementar una puerta de enlace zonal, todas las instancias de la puerta de enlace se implementan en la misma zona de disponibilidad.

## <a name="fastpath"></a>FastPath
La puerta de enlace de red virtual de ExpressRoute está diseñada para intercambiar las rutas de red y enrutar el tráfico de red. FastPath está diseñado para mejorar el rendimiento de las rutas de acceso a los datos entre la red local y una red virtual. Cuando está habilitado, FastPath envía el tráfico de red directamente a las máquinas virtuales que están en la red, omitiendo la puerta de enlace. 

FastPath solo está disponible en [ExpressRoute Direct](expressroute-erdirect-about.md). En otras palabras, puede habilitar esta característica solo si [conecta la red virtual](expressroute-howto-linkvnet-arm.md) a un circuito de ExpressRoute creado en un puerto directo de ExpressRoute. FastPath de todos modos necesita que se cree una puerta de enlace de red virtual para intercambiar rutas entre la red virtual y una red local. La puerta de enlace de red virtual debe ser Ultrarrendimiento o ErGw3AZ.

FastPath no es compatible con estas características:
* UDR en la subred de puerta de enlace: si aplica un UDR a la subred de puerta de enlace de la red virtual, el tráfico de la red local se seguirá enviando a la puerta de enlace de red virtual.
* Emparejamiento de redes virtuales: si tiene otras redes virtuales emparejadas con la que está conectada a ExpressRoute, el tráfico de la red local a las otras redes virtuales (es decir, las llamadas redes virtuales de "radio") se seguirá enviado a la puerta de enlace de red virtual. La solución es conectar todas las redes virtuales directamente al circuito de ExpressRoute.

## <a name="resources"></a>API de REST y cmdlets de PowerShell
Para más información sobre recursos técnicos y requisitos de sintaxis específicos al usar API de REST y cmdlets de PowerShell para configuraciones de puerta de enlace de red virtual, consulte las páginas siguientes:

| **Clásico** | **Resource Manager** |
| --- | --- |
| [PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0#azure) |[PowerShell](https://docs.microsoft.com/powershell/module/az.network#networking) |
| [API DE REST](https://msdn.microsoft.com/library/jj154113.aspx) |[API DE REST](https://msdn.microsoft.com/library/mt163859.aspx) |

## <a name="next-steps"></a>Pasos siguientes
Consulte [Información técnica de ExpressRoute](expressroute-introduction.md) para más información sobre configuraciones de conexión disponibles.

Consulte [Creación de una puerta de enlace de red virtual para ExpressRoute](expressroute-howto-add-gateway-resource-manager.md) para más información sobre cómo crear puertas de enlace de ExpressRoute.

Consulte [Crear una puerta de enlace de red virtual con redundancia de zona](../../articles/vpn-gateway/create-zone-redundant-vnet-gateway.md) para obtener más información acerca de cómo configurar puertas de enlace con redundancia de zona.

Consulte el artículo sobre la [vinculación de una red virtual a ExpressRoute](expressroute-howto-linkvnet-arm.md) para más información sobre cómo habilitar FastPath. 
