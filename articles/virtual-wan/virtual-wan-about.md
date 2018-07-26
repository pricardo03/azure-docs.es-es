---
title: Información general Azure Virtual WAN | Microsoft Docs
description: Obtenga información sobre la conectividad escalable y automatizada de rama a rama de Azure Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: overview
ms.date: 07/18/2018
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand what Virtual WAN is and if it is the right choice for my Azure network.
ms.openlocfilehash: 67dd6ba9b94ed9d58d91fb644ce9ee9e44ae9e45
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39159176"
---
# <a name="what-is-azure-virtual-wan-preview"></a>¿Qué es Azure Virtual WAN? (versión preliminar)

Azure Virtual WAN es un servicio de redes que ofrece conectividad de rama a rama automatizada y optimizada mediante Azure. Azure Virtual WAN permite conectar y configurar los dispositivos de rama para comunicarse con Azure. Esto puede realizarse manualmente o con dispositivos de proveedores preferidos mediante un asociado de Virtual WAN. El uso de dispositivos de proveedores preferidos permite la administración de la configuración, facilita el uso y simplifica la conectividad. El panel integrado de Azure WAN proporciona información instantánea para solucionar problemas que puede ayudarle a ahorrar tiempo y le ofrece una manera fácil de ver la conectividad de sitio a sitio y a gran escala.

> [!IMPORTANT]
> Azure Virtual WAN se encuentra actualmente en versión preliminar pública administrada. Para usar Virtual WAN, es preciso [inscribirse en la versión preliminar](#enroll).
>
> Esta versión preliminar pública se proporciona sin un contrato de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

En este artículo se proporciona una vista rápida de la conectividad de red de las cargas de trabajo de Azure y de las que no son de Azure. Virtual WAN ofrece las siguientes ventajas:

* **Soluciones integradas de conectividad en concentrador y radio:** configuración de sitio a sitio automatizada y conectividad entre sitios locales y un concentrador de Azure desde diversos orígenes, incluidas las soluciones de asociados de Virtual WAN.
* **Configuración e instalación automatizadas de radio:** conecte sin problemas las cargas de trabajo y las redes virtuales al concentrador de Azure.
* **Solución de problemas intuitiva:** puede ver el flujo de un extremo a otro dentro de Azure y usar esta información para tomar las acciones necesarias.

![Diagrama de Virtual WAN](./media/virtual-wan-about/virtualwan.png)

## <a name="vendor"></a>Trabajo con un asociado de Virtual WAN

1. El controlador del dispositivo de rama (VPN/SDWAN) se autentica para exportar información centrada en el sitio en Azure mediante el uso de una entidad de servicio de Azure.
2. El controlador de dispositivo (VPN/SDWAN) de rama obtiene la configuración de conectividad de Azure y actualiza el dispositivo local. De esta forma se automatiza la descarga de la configuración, la edición y la actualización del dispositivo VPN local.
3. Una vez que el dispositivo tiene la configuración de Azure correcta, se establece una conexión de sitio a sitio (dos túneles activos) a Azure WAN. Azure requiere el controlador de rama (VPN/SDWAN) para admitir IKEv2. BGP es opcional.

## <a name="resources"></a>Recursos de Virtual WAN

Para configurar una red Virtual WAN de extremo a otro, debe crear los siguientes recursos:

* **virtualWAN:** este recurso representa una superposición virtual de la red de Azure y es una colección de varios recursos. Contiene vínculos a todos los concentradores virtuales que le gustaría tener dentro de Virtual WAN. Los recursos de Virtual WAN están aislados entre sí y no pueden contener un concentrador común. Los concentradores virtuales a través de Virtual WAN no se comunican entre sí.

* **Sitio:** el recurso de sitio conocido como vpnsite representa el dispositivo VPN local y su configuración. Al trabajar con un asociado de Virtual WAN, tiene una solución integrada para exportar automáticamente esta información a Azure.

* **Concentrador:** un concentrador virtual es una red virtual administrada por Microsoft. El concentrador contiene varios puntos de conexión de servicio para habilitar la conectividad de la red local (vpnsite). El concentrador es el núcleo de la red en una región. Solo puede haber un único concentrador por cada región de Azure. Cuando se crea un concentrador mediante Azure Portal, se crea automáticamente una red virtual del concentrador virtual y un elemento vpngateway de concentrador virtual.

  Una puerta de enlace de concentrador no es lo mismo que la puerta de enlace de red virtual que usó para ExpressRoute y VPN Gateway. Por ejemplo, cuando se usa Virtual WAN, no se crea una conexión de sitio a sitio desde su sitio local directamente a la red virtual. En su lugar, se crea una conexión de sitio a sitio al concentrador. El tráfico siempre pasa a través de la puerta de enlace del concentrador. Esto significa que las redes virtuales no necesitan su propia puerta de enlace de red virtual. Virtual WAN permite que las redes virtuales obtengan provecho del escalado fácilmente mediante el concentrador virtual y su puerta de enlace. 

* **Conexión de red virtual del concentrador:** el recurso de conexión de red virtual se usa para conectar el concentrador sin problemas a la red virtual. En este momento, solo puede conectarse a las redes virtuales que están en la misma región central.

##<a name="enroll"></a>Inscribirse en la versión preliminar

Para poder configurar una instancia de Virtual WAN, primero hay que inscribir una suscripción en la versión preliminar. En caso contrario, no podrá trabajar con Virtual WAN en el portal. Para inscribirse, envíe un correo electrónico a <azurevirtualwan@microsoft.com> con su identificador de suscripción. Recibirá un correo electrónico una vez inscrita la suscripción.

## <a name="faq"></a>P+F

[!INCLUDE [Virtual WAN FAQ](../../includes/virtual-wan-faq-include.md)]

## <a name="feedback"></a>Comentarios acerca de la versión preliminar

Agradeceríamos su participación. Envíe un correo electrónico a <azurevirtualwan@microsoft.com> para informar sobre cualquier problema o para proporcionar comentarios (positivos o negativos) acerca de Virtual WAN. Incluya el nombre de su compañía en la línea de asunto "[ ]". Incluya también su identificador de suscripción si está notificando un problema.

## <a name="next-steps"></a>Pasos siguientes

Para crear una conexión de sitio a sitio mediante Virtual WAN, puede continuar mediante un [asociado de Virtual WAN](https://aka.ms/virtualwan) o crear la conexión manualmente. Para crear la conexión manualmente, consulte [Creación de una conexión de sitio a sitio mediante Virtual WAN](virtual-wan-site-to-site-portal.md).
