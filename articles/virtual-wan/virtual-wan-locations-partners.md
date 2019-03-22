---
title: Ubicaciones y asociados de Azure Virtual WAN | Microsoft Docs
description: En este artículo contiene una lista de asociados de WAN Virtual de Azure y ubicaciones de concentrador.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 03/04/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to connect find a Virtual WAN partner
ms.openlocfilehash: f38cd0565b2e90fe0803d8e815c622e22e954a18
ms.sourcegitcommit: 94305d8ee91f217ec98039fde2ac4326761fea22
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57409711"
---
# <a name="virtual-wan-partners-and-virtual-hub-locations"></a>Ubicaciones de concentradores virtuales y asociados de Azure Virtual WAN

Este artículo se proporciona información sobre la red WAN Virtual admite las regiones y los asociados para la conectividad en el concentrador Virtual.

Azure Virtual WAN es un servicio de redes que ofrece conectividad de rama a rama automatizada y optimizada mediante Azure. Azure Virtual WAN permite conectar y configurar los dispositivos de rama para comunicarse con Azure. Esto puede realizarse manualmente o mediante el uso de dispositivos del proveedor a través de un red WAN Virtual asociado. Uso de dispositivos de socio permite que facilidad de uso, la simplificación de la conectividad y administración de configuración.

La conectividad desde el dispositivo local se establece de forma automática para el concentrador virtual. Un concentrador virtual es una red virtual administrada por Microsoft. El concentrador contiene varios puntos de conexión de servicio para habilitar la conectividad de la red local (vpnsite). Solo se puede tener un concentrador por región.

## <a name="automation"></a>Automatización de los socios de conectividad

Los dispositivos que se conectan a Azure Virtual WAN tienen automatización integrada para conectarse. Normalmente, esto se configura en la interfaz de usuario de administración de dispositivos (o equivalente), donde se configura la administración de configuración y de conectividad entre el dispositivo de la rama VPN a un punto de conexión de VPN de concentrador virtual de Azure (puerta de enlace VPN).

La automatización de alto nivel siguiente se configura en la consola del dispositivo/centro de administración:

* Permisos adecuados para que el dispositivo acceda al grupo de recursos de Azure Virtual WAN
* Carga del dispositivo de la rama en Azure Virtual WAN
* Descarga automática de la información de conectividad de Azure
* Configuración del dispositivo de rama local 

Algunos asociados de conectividad pueden extender la automatización para incluir la creación de la red virtual del concentrador virtual de Azure y VPN Gateway. Si quiere obtener más información acerca de la automatización, consulte [Configuración de la automatización: Asociados de WAN](virtual-wan-configure-automation-providers.md).

## <a name="partners"></a>Conectividad a través de asociados

[!INCLUDE [partners](../../includes/virtual-wan-partners-include.md)]

## <a name="locations"></a>Ubicaciones

[!INCLUDE [regions](../../includes/virtual-wan-regions-include.md)]

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre Virtual WAN, consulte [Preguntas más frecuentes sobre Virtual WAN](virtual-wan-faq.md).

* Para obtener más información acerca de cómo automatizar la conectividad a Azure Virtual WAN, consulte [Configuración de la automatización: Asociados de WAN](virtual-wan-configure-automation-providers.md).
