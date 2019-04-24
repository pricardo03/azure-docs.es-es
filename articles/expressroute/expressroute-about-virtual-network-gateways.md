---
title: 'Acerca de las puertas de enlace de red virtual de ExpressRoute: Azure| Microsoft Docs'
description: Conozca sobre las puertas de enlace de red virtual para ExpressRoute. En este artículo incluye información sobre los tipos y las SKU de puerta de enlace.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: mialdrid
ms.custom: seodec18
ms.openlocfilehash: d9c607114d6c6c56c25303a88dcc11f4ab804eb4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60367981"
---
# <a name="about-virtual-network-gateways-for-expressroute"></a>Acerca de las puertas de enlace de red virtual para ExpressRoute
Una puerta de enlace de red virtual se usa para enviar tráfico de red entre redes virtuales y ubicaciones locales de Azure. Puede usar una puerta de enlace de red virtual para tráfico de ExpressRoute o tráfico VPN. Este artículo se centra en las puertas de enlace de red virtual de ExpressRoute y contiene información sobre las SKU, el rendimiento estimado por SKU y los tipos de puerta de enlace.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="gateway-types"></a>Tipos de puerta de enlace

Al crear una puerta de enlace de red virtual, debe especificar varios valores de configuración. Uno de los valores de configuración necesarios, "-GatewayType", especifica si la puerta de enlace se usará para el tráfico VPN o de ExpressRoute. Los dos tipos de puerta de enlace son los siguientes:

* **Vpn**: para enviar tráfico cifrado a través de una conexión a Internet pública, use la puerta de enlace de tipo "Vpn". Este tipo también se conoce como VPN Gateway. Las conexiones de sitio a sitio, de punto a sitio y de red virtual a red virtual utilizan una puerta de enlace de VPN.

* **ExpressRoute**: para enviar tráfico de red en una conexión privada, use la puerta de enlace de tipo "ExpressRoute". Este tipo también se conoce como puerta de enlace de ExpressRoute y es el tipo de puerta de enlace que se utiliza al configurar ExpressRoute.

Cada red virtual tiene una única puerta de enlace de red virtual por cada tipo de puerta de enlace. Por ejemplo, puede tener una puerta de enlace de una red virtual que use -GatewayType Vpn y otra que use -GatewayType ExpressRoute.

## <a name="gwsku"></a>SKU de puerta de enlace
[!INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

Si desea actualizar la puerta de enlace a una SKU de puerta de enlace más eficaz, en la mayoría de los casos puede usar el cmdlet de PowerShell 'Resize-AzVirtualNetworkGateway'. Esto funcionará para las actualizaciones de SKU Standard y HighPerformance. Sin embargo, para actualizar a la SKU UltraPerformance, debe volver a crear la puerta de enlace. Volver a crear una puerta de enlace provoca un tiempo de inactividad.

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
