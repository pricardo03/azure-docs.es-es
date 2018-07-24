---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 07/10/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8c3f727c6154a0364f151d22000d2684c361676a
ms.sourcegitcommit: 04fc1781fe897ed1c21765865b73f941287e222f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39037217"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>¿Cuál es la diferencia entre una puerta de enlace de red virtual de Azure (VPN Gateway) y un elemento vpngateway de Azure Virtual WAN?

Virtual WAN proporciona conectividad de sitio a sitio y a gran escala, y se ha creado para mejorar el rendimiento, la escalabilidad y la facilidad de uso. Los dispositivos de rama CPE realizan un aprovisionamiento automático y se conectan en Azure Virtual WAN. Estos dispositivos están disponibles en un creciente ecosistema de asociados de VPN y SD-WAN.

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>¿Qué proveedores de dispositivos (asociados de Virtual WAN) se admiten desde el primer momento? 

En este momento, Citrix y Riverbed admiten la experiencia de Virtual WAN totalmente automatizada. Más asociados se incorporarán en los próximos meses, como son Nokia Nuage, Palo Alto y Checkpoint. Para más información, consulte [Asociados de Virtual WAN](https://aka.ms/virtualwan).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>¿Debo usar un dispositivo de asociado preferido?

No. Puede usar cualquier dispositivo compatible con VPN que cumpla los requisitos de la versión preliminar para la compatibilidad con IKEv2 IPsec.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>¿Cómo automatizan la conectividad con Azure Virtual WAN los asociados de Virtual WAN?

Las soluciones de conectividad definidas por software suelen administrar sus dispositivos de rama con un controlador o un centro de aprovisionamiento de dispositivos. El controlador puede usar las API de Azure para automatizar la conectividad a Azure Virtual WAN. Para más información acerca de cómo funciona esto, consulte [Automatización de asociados de Virtual WAN](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>¿Cambia Virtual WAN alguna de las características de conectividad existentes?   

No hay ningún cambio en las características de conectividad de Azure.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>¿Hay disponibles nuevos recursos de Resource Manager para Virtual WAN?
  
Sí, Virtual WAN introduce nuevos recursos de Resource Manager. Para más información, consulte la [Información general](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="are-there-any-special-requirements-to-join-the-preview"></a>¿Hay algún requisito especial para unirse a la versión preliminar? 

Para poder configurar una instancia de Azure Virtual WAN, primero debe inscribir su suscripción en la versión preliminar. Puede hacerlo enviando un correo electrónico a <azurevirtualwan@microsoft.com> con su identificador de suscripción. Recibirá un correo electrónico una vez inscrita la suscripción. Hasta que reciba la confirmación de que su suscripción se ha inscrito, no podrá trabajar con Virtual WAN en el portal.

Consideraciones:

* La vista preliminar está limitada solo a las regiones de Azure.
* Se admiten hasta 100 conexiones por concentrador virtual. Cada conexión consta de dos túneles que están en una configuración activo-activo. Los túneles terminan en un elemento vpngateway del concentrador virtual de Azure.
* Considere el uso de esta versión preliminar si:
  * Desea implementar ancho de banda agregado menor de 1 Gbps por concentrador virtual.
  * Tiene un dispositivo VPN que admite la configuración basada en enrutamiento y la conectividad IKEv2 IPsec.
  * Desea explorar el uso de dispositivos de rama operativa y SD-WAN en los asociados iniciales (Riverbed y Citrix).<br>o<br>Desea configurar la conectividad de rama a rama y de rama a Azure que incluya la administración de la configuración del dispositivo de forma local. (Configuración automática)

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>¿Se admite el emparejamiento de VNet global con Azure Virtual WAN? 

 No.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>¿Pueden las redes virtuales radiales conectadas a un concentrador virtual comunicarse entre sí?

Sí. Para hacer directamente el emparejamiento de las redes virtuales entre los radios que están conectados a un concentrador virtual. Para más información, consulte [Emparejamiento de redes virtuales](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>¿Puedo implementar y usar mi dispositivo virtual de red favorito (en una red virtual de NVA) con Azure Virtual WAN?

Sí, puede conectar su VNet de aplicación virtual de red (NVA) favorita a Azure Virtual WAN, pero requerirá funcionalidades de enrutamiento en el concentrador que se encontrarán en disponibilidad general. Además, todos los radios conectados a la red virtual de NVA deben estar conectados al concentrador virtual. 

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>¿Puede una red virtual de NVA tener una puerta de enlace de red virtual?

No. La red virtual de NVA no puede tener una puerta de enlace de red virtual si se conecta al concentrador virtual. 

### <a name="is-there-support-for-bgp"></a>¿Hay compatibilidad con BGP?

Sí, BGP se admite. Para asegurarse de que las rutas de una red virtual de NVA se anuncian correctamente, los radios deben deshabilitar BGP si están conectados a una red virtual de NVA que, a su vez, está conectada a un concentrador virtual. Además, puede conectar las redes virtuales de radios al concentrador virtual.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>¿Se puede dirigir el tráfico mediante UDR en el concentrador virtual?

La funcionalidad de enrutamiento y UDR estará disponible mediante la disponibilidad general.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>¿Hay información de precios o licencias para Virtual WAN?
 
No hay ningún cargo adicional durante la versión preliminar. El [precio de salida y de Azure VPN](https://azure.microsoft.com/pricing/details/vpn-gateway/) permanece vigente durante la versión preliminar.