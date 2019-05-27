---
title: archivo de inclusión
description: archivo de inclusión
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 03/18/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: a89a5d753eaa241b11eb4c7eed9500c9715d405d
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66150789"
---
### <a name="what-is-the-difference-between-an-azure-virtual-network-gateway-vpn-gateway-and-an-azure-virtual-wan-vpngateway"></a>¿Cuál es la diferencia entre una puerta de enlace de red virtual de Azure (VPN Gateway) y un elemento vpngateway de Azure Virtual WAN?

Virtual WAN proporciona conectividad de sitio a sitio y a gran escala, y se ha creado para mejorar el rendimiento, la escalabilidad y la facilidad de uso. ExpressRoute y la funcionalidad de conectividad de punto a sitio están actualmente en versión preliminar. Los dispositivos de la rama CPE realizan un aprovisionamiento automático y se conectan en Azure Virtual WAN. Estos dispositivos están disponibles en un creciente ecosistema de asociados de VPN y SD-WAN. Consulte la [lista de asociados preferidos](https://go.microsoft.com/fwlink/p/?linkid=2019615).

### <a name="which-device-providers-virtual-wan-partners-are-supported-at-launch-time"></a>¿Qué proveedores de dispositivos (asociados de Virtual WAN) se admiten desde el primer momento?

En este momento, muchos asociados admiten la experiencia de Virtual WAN totalmente automatizada. Para más información, consulte [Asociados de Virtual WAN](https://go.microsoft.com/fwlink/p/?linkid=2019615). 

### <a name="what-are-the-virtual-wan-partner-automation-steps"></a>¿Cuáles son los pasos de automatización de asociados de Virtual WAN?

Para conocer estos pasos, consulte [Automatización de asociados de Virtual WAN](../articles/virtual-wan/virtual-wan-configure-automation-providers.md).

### <a name="am-i-required-to-use-a-preferred-partner-device"></a>¿Debo usar un dispositivo de asociado preferido?

 No. Puede usar cualquier dispositivo compatible con VPN que cumpla los requisitos de Azure para la compatibilidad con IPsec de IKEv1 o IKEv2.

### <a name="how-do-virtual-wan-partners-automate-connectivity-with-azure-virtual-wan"></a>¿Cómo automatizan la conectividad con Azure Virtual WAN los asociados de Virtual WAN?

Las soluciones de conectividad definidas por software suelen administrar sus dispositivos de rama con un controlador o un centro de aprovisionamiento de dispositivos. El controlador puede usar las API de Azure para automatizar la conectividad a Azure Virtual WAN. Para más información, consulte Automatización de asociados de Virtual WAN.

### <a name="does-virtual-wan-change-any-existing-connectivity-features"></a>¿Cambia Virtual WAN alguna de las características de conectividad existentes?   

No hay ningún cambio en las características de conectividad de Azure.

### <a name="are-there-new-resource-manager-resources-available-for-virtual-wan"></a>¿Hay disponibles nuevos recursos de Resource Manager para Virtual WAN?
  
Sí, Virtual WAN introduce nuevos recursos de Resource Manager. Para más información, consulte la [Información general](https://go.microsoft.com/fwlink/p/?LinkId=2004389).

### <a name="how-many-vpn-devices-can-connect-to-a-single-hub"></a>¿Cuántos dispositivos VPN pueden conectarse a un único concentrador?

Se admiten hasta 1000 conexiones por concentrador virtual. Cada conexión consta de dos túneles que están en una configuración activo-activo. Los túneles terminan en un elemento vpngateway del concentrador virtual de Azure.

### <a name="can-the-on-premises-vpn-device-connect-to-multiple-hubs"></a>¿El dispositivo VPN local puede conectarse a varios concentradores?

Sí. Al principio, el flujo de tráfico sería del dispositivo local a la instancia de Microsoft Edge más cercana y, luego, al concentrador virtual.

### <a name="is-global-vnet-peering-supported-with-azure-virtual-wan"></a>¿Se admite el emparejamiento de VNet global con Azure Virtual WAN? 

  No.

### <a name="can-spoke-vnets-connected-to-a-virtual-hub-communicate-with-each-other"></a>¿Pueden las redes virtuales radiales conectadas a un concentrador virtual comunicarse entre sí?

Sí. Las redes virtuales radiales pueden comunicarse directamente a través del emparejamiento de red virtual. Sin embargo, no se admiten redes virtuales que se comuniquen de forma transitiva mediante el centro. Para más información, consulte [Emparejamiento de redes virtuales](../articles/virtual-network/virtual-network-peering-overview.md).

### <a name="can-i-deploy-and-use-my-favorite-network-virtual-appliance-in-an-nva-vnet-with-azure-virtual-wan"></a>¿Puedo implementar y usar mi dispositivo virtual de red favorito (en una red virtual de NVA) con Azure Virtual WAN?

Sí, puede conectar su aplicación virtual de red (NVA) favorita a Azure Virtual WAN. En primer lugar, conecte la red virtual de la aplicación virtual de red virtual al concentrador con una conexión de red virtual de concentrador. A continuación, cree una ruta de concentrador virtual con un próximo salto que apunte a la aplicación virtual. Puede aplicar varias rutas a la tabla de rutas del concentrador virtual. Además, los radios conectados a la red virtual del dispositivo virtual deben estar conectados al concentrador virtual para garantizar que las rutas de la red virtual del radio se propagan a los sistemas locales.

### <a name="can-an-nva-vnet-have-a-virtual-network-gateway"></a>¿Puede una red virtual de NVA tener una puerta de enlace de red virtual?

 No. La red virtual de NVA no puede tener una puerta de enlace de red virtual si se conecta al concentrador virtual. 

### <a name="is-there-support-for-bgp"></a>¿Hay compatibilidad con BGP?

Sí, BGP se admite. Al crear una VPN de sitio, puede proporcionar en ella los parámetros de BGP. Como resultado, las conexiones creadas en Azure para ese sitio estarán habilitadas para BGP. Además, si tuviera una red virtual con una NVA, y si esta red estuviera asociada a un centro Virtual WAN, para garantizar que las rutas de una de estas redes se anuncian correctamente, los radios asociados a esta red deben deshabilitar BGP. Asimismo, conecte estas redes virtuales de radio a la red virtual del concentrador virtual para garantizar que las rutas de dichas redes se propagan a los sistemas locales.

### <a name="can-i-direct-traffic-using-udr-in-the-virtual-hub"></a>¿Se puede dirigir el tráfico mediante UDR en el concentrador virtual?

Sí, puede dirigir el tráfico a una red virtual mediante la tabla de rutas del concentrador virtual.

### <a name="is-there-any-licensing-or-pricing-information-for-virtual-wan"></a>¿Hay información de precios o licencias para Virtual WAN?
 
Sí. Consulte la página [Precios](https://azure.microsoft.com/pricing/details/virtual-wan/).

### <a name="how-do-i-calculate-price-of-a-hub"></a>¿Cómo se calcula el precio de un centro?
 
Tendría que pagar para el servicio en el centro. Por ejemplo, si diez ramas o dispositivos locales necesitaran conectarse a Azure Virtual WAN, implicaría la conexión a los puntos de conexión de la VPN en el centro. Supongamos que esta VPN es de 1 unidad de escala = 500 Mbps, esto se cobraría a 0,361 USD/h. Cada conexión se cobra a 0,08 USD/h. Para 10 conexiones, el costo total de servicio/h sería de 0,361 USD + 0,08 USD/h. Se aplican cargos de datos por el tráfico que sale de Azure. 

### <a name="how-do-new-partners-that-are-not-listed-in-your-launch-partner-list-get-onboarded"></a>¿Cómo consiguen los nuevos asociados que no aparecen en la lista de asociados de partida ser incluidos en ella?

Envíe un correo electrónico a azurevirtualwan@microsoft.com. Un asociado ideal es aquel que tiene un dispositivo que se puede aprovisionar para la conectividad IPsec de IKEv1 o IKEv2.

### <a name="what-if-a-device-i-am-using-is-not-in-the-virtual-wan-partner-list-can-i-still-use-it-to-connect-to-azure-virtual-wan-vpn"></a>¿Qué pasa si el dispositivo que estoy usando no está en la lista de asociados de Virtual WAN? ¿Lo puedo usar a pesar de todo para conectarme a una VPN de Azure Virtual WAN?

Sí, siempre que el dispositivo admita IPsec IKEv1 o IKEv2. Los asociados de Virtual WAN automatizan la conectividad desde el dispositivo hasta los puntos de conexión de VPN de Azure. Esto implica la automatización de pasos como la carga de la información de rama, IPsec y configuración y conectividad. Puesto que el dispositivo no procede del ecosistema de un asociado de Virtual WAN, deberá realizar el trabajo pesado de tomar manualmente la configuración de Azure y actualizar el dispositivo para configurar la conectividad IPsec. 

### <a name="is-it-possible-to-construct-azure-virtual-wan-with-a-resource-manager-template"></a>¿Es posible construir Azure Virtual WAN con una plantilla de Resource Manager?

Se puede crear una configuración simple de una instancia de Virtual WAN con un centro y un sitio VPN mediante una [plantilla de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network). Virtual WAN es principalmente un servicio REST o basado en el portal.

### <a name="is-branch-to-branch-connectivity-allowed-in-virtual-wan"></a>¿Se permite la conectividad de rama a rama en Virtual WAN?

Sí, este tipo de conectividad está disponible en Virtual WAN para conexiones VPN y VPN a ExpressRoute. Aunque las de VPN de sitio a sitio están disponibles, las de ExpressRoute o de punto a sitio están actualmente en versión preliminar.

### <a name="does-branch-to-branch-traffic-traverse-through-the-azure-virtual-wan"></a>¿Atraviesa el tráfico de rama a rama Azure Virtual WAN?

Sí.

### <a name="how-is-virtual-wan-different-from-the-existing-azure-virtual-network-gateway"></a>¿En qué difiere Virtual WAN del actual Azure Virtual Network Gateway?

La VPN de Virtual Network Gateway se limita a 30 túneles. En las conexiones, debe usar Virtual WAN para VPN a gran escala. Puede realizar hasta 1000 conexiones de ramas con 2 Gbps en el concentrador para todas las regiones excepto la región Centro-oeste de EE. UU. Para esta región, hay 20 Gbps disponibles. Ampliaremos los 20 Gbps a más regiones próximamente. Una conexión es un túnel de activo a activo del dispositivo VPN local al concentrador virtual. Puede tener un único concentrador por región, lo que significa que puede conectar más de 1000 ramas en total con todos los concentradores.

### <a name="how-is-virtual-wan-supporting-sd-wan-devices"></a>¿Cómo es que Virtual WAN admite dispositivos SD-WAN?

Los asociados de Virtual WAN automatizan la conectividad IPsec a los puntos de conexión de VPN de Azure. Si el asociado de Virtual WAN es un proveedor de SD-WAN, se supone que el controlador de SD-WAN administra la automatización y la conectividad IPsec a los puntos de conexión de VPN de Azure. Si el dispositivo SD-WAN necesita su propio punto de conexión en lugar de la VPN de Azure para obtener funcionalidad de SD-WAN propietaria, puede implementar el punto de conexión de SD-WAN en una red virtual de Azure y coexistir con Azure Virtual WAN.

### <a name="does-this-virtual-wan-require-expressroute-from-each-site"></a>¿Esta Virtual WAN requiere ExpressRoute desde cada sitio?

No. Usa la conectividad de sitio a sitio IPsec estándar a través de vínculos de Internet desde el dispositivo a un concentrador de Azure Virtual WAN. Los sitios pueden estar conectados a una red de proveedor mediante un circuito ExpressRoute. Los sitios conectados mediante ExpressRoute en el concentrador virtual (en versión preliminar) pueden tener un flujo de tráfico de rama a rama entre ExpressRoute y VPN. 

### <a name="is-there-a-network-throughput-limit-when-using-azure-virtual-wan"></a>¿Hay un límite en el rendimiento de la red al utilizar Azure Virtual WAN?

El número de ramas se limita a 1000 conexiones por cada región o concentrador y a un total de 2 GB en el concentrador. La excepción es la región Centro-oeste de EE. UU., que dispone de un total de 20 Gbps. Ampliaremos los 20 Gbps a más regiones próximamente.

### <a name="does-virtual-wan-allow-the-on-premises-device-to-utilize-multiple-isps-in-parallel-or-is-it-always-a-single-vpn-tunnel"></a>¿Azure Virtual WAN permite al dispositivo local usar varias ISP en paralelo o es siempre un solo túnel VPN?

Una conexión que entra en una VPN de Virtual WAN siempre es un túnel activo/activo (para proporcionar resistencia en la misma región o centro) que usa un vínculo disponible en la rama. Este vínculo puede ser un vínculo de ISP en la rama local. Virtual WAN no proporciona ninguna lógica especial para configurar varios ISP en paralelo; administrar la conmutación por error en ISP en la rama es completamente una operación de red centrada en la rama. Puede usar su solución favorita SD-WAN para realizar la selección de la ruta en la rama.

### <a name="how-is-traffic-routed-on-the-azure-backbone"></a>¿Cómo se enruta el tráfico en la red troncal de Azure?

El tráfico sigue el patrón: dispositivo de rama -> ISP -> Microsoft Edge -> controlador de dominio de Microsoft (red virtual de centro) -> Microsoft Edge -> ISP -> dispositivo de rama.

### <a name="in-this-model-what-do-you-need-at-each-site-just-an-internet-connection"></a>En este modelo, ¿qué es necesario en cada sitio? ¿Solo una conexión a Internet?

Sí. Una conexión a Internet y un dispositivo físico que admita IPsec, preferiblemente de nuestros [asociados](https://go.microsoft.com/fwlink/p/?linkid=2019615) integrados. De forma opcional, puede administrar manualmente la configuración y la conectividad a Azure desde su dispositivo preferido.
