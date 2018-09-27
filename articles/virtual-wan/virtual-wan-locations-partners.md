---
title: Ubicaciones y asociados de Azure Virtual WAN | Microsoft Docs
description: En este artículo se incluyen listas de ubicaciones de concentradores y de asociados de Azure Virtual WAN
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: 76542392522ff14642ed7ccc6de8ed543d66513f
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46985000"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Ubicaciones de concentradores virtuales y asociados de Azure Virtual WAN

En este artículo se proporciona información sobre las regiones admitidas de Virtual WAN y los asociados preferidos para la conectividad con concentradores virtuales.

Azure Virtual WAN es un servicio de redes que ofrece conectividad de rama a rama automatizada y optimizada mediante Azure. Azure Virtual WAN permite conectar y configurar los dispositivos de rama para comunicarse con Azure. Esto puede realizarse manualmente o con dispositivos de proveedores preferidos mediante un asociado preferido de Virtual WAN. El uso de dispositivos de asociados preferidos permite la administración de la configuración, facilita el uso y simplifica la conectividad. La conectividad desde el dispositivo local se establece de forma automática para el concentrador virtual. Un concentrador virtual es una red virtual administrada por Microsoft. El concentrador contiene varios puntos de conexión de servicio para habilitar la conectividad de la red local (vpnsite). Solo se puede tener un concentrador por región.

## <a name="regions"></a>Regiones

La lista de regiones admitidas y disponibles es la siguiente:

|Región geopolítica | Regiones de Azure|
|---|---|
|Norteamérica | Este de EE. UU., oeste de EE. UU., este de EE. UU. 2, oeste de EE.UU. 2, centro de EE. UU., centro-sur de EE. UU., centro-norte de EE. UU., centro-oeste de EE. UU., centro de Canadá y este de Canadá |
|Sudamérica |Sur de Brasil |
| Europa | Centro de Francia, Sur de Francia, Europa del Norte, Europa Occidental, Oeste de Reino Unido, Sur de Reino Unido |
| Asia | Este de Asia y Sudeste de Asia |
| Japón  | Oeste de Japón y Este de Japón |
| Australia | Este de Australia y Sudeste de Australia | 
| Gobierno de Australia | Centro de Australia, Centro de Australia 2 |
| India | India occidental, India central, India del Sur |
| Corea del Sur | Corea Central, Corea del Sur | 

## <a name="automation-from-connectivity-partners"></a>Automatización de asociados de conectividad

En esta sección se describen los detalles de alto nivel de la automatización de los proveedores de conectividad.

Los dispositivos que se conectan a Azure Virtual WAN tienen automatización integrada para conectarse. Normalmente, esto se configura en la interfaz de usuario de administración de dispositivos (o equivalente), donde se configura la administración de configuración y de conectividad entre el dispositivo de la rama VPN a un punto de conexión de VPN de concentrador virtual de Azure (puerta de enlace VPN).

La automatización de alto nivel siguiente se configura en la consola del dispositivo/centro de administración:

* Permisos adecuados para que el dispositivo acceda al grupo de recursos de Azure Virtual WAN
* Carga del dispositivo de la rama en Azure Virtual WAN
* Descarga automática de la información de conectividad de Azure 
* Configuración del dispositivo de rama local 

Algunos asociados de conectividad pueden extender la automatización para incluir la creación de la red virtual del concentrador virtual de Azure y VPN Gateway. Si quiere obtener más información acerca de la automatización, consulte [Configuración de la automatización: Asociados de WAN](virtual-wan-configure-automation-providers.md).

## <a name="connectivity-through-preferred-partners"></a>Conectividad a través de asociados preferidos

Si su asociado de dispositivo de rama no aparece en la sección siguiente, póngase en contacto con su proveedor de dispositivos de rama y pídale que se ponga en contacto con nosotros por correo electrónico a azurevirtualwan@microsoft.com.

Puede comprobar los vínculos siguientes para recopilar más información sobre los servicios ofrecidos por los asociados preferidos. 

|Asociados preferidos|
|---|
|[Barracuda Networks](https://www.barracuda.com/AzurevWAN)|
| [Check Point](https://www.checkpoint.com/solutions/microsoft-azure-virtual-wan/) |
| [Citrix](https://www.citrix.com/global-partners/microsoft/sd-wan-for-azure-virtual-wan.html)|
|[Netfoundry](https://netfoundry.io/solutions/netfoundry-for-microsoft-azure-virtual-wan/)|
|[Palo Alto Networks](https://researchcenter.paloaltonetworks.com/2018/09/azure-vwan-integration/) |
|[Riverbed Technology](https://www.riverbed.com/go/steelconnect-azurewan.html)|
|[128 Technology](https://www.128technology.com/partners/azure) |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre Virtual WAN, consulte [Preguntas más frecuentes sobre Virtual WAN](virtual-wan-faq.md).

Para obtener más información acerca de cómo automatizar la conectividad a Azure Virtual WAN, consulte [Configuración de la automatización: Asociados de WAN](virtual-wan-configure-automation-providers.md).
