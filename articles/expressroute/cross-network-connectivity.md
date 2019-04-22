---
title: Conectividad entre redes de Azure | Microsoft Docs
description: Esta página describe un escenario de aplicación para entre la conectividad de red y la solución basada en características de red de Azure.
documentationcenter: na
services: networking
author: rambk
manager: tracsman
ms.service: expressroute
ms.topic: article
ms.workload: infrastructure-services
ms.date: 04/03/2019
ms.author: rambala
ms.openlocfilehash: 4923c7f2048b7368af6314d5e2288216115bc3bc
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59784746"
---
# <a name="cross-network-connectivity"></a>Conectividad entre redes

Fabrikam Inc. tiene una presencia física importante e implementación de Azure en el Este de EE. UU. Fabrikam tiene conectividad de back-end entre sus servidores locales y las implementaciones de Azure a través de ExpressRoute. De forma similar, Contoso Ltd. tiene una presencia y la implementación de Azure en el oeste de Estados Unidos. Contoso tiene conectividad de back-end entre sus servidores locales y las implementaciones de Azure a través de ExpressRoute.  

Fabrikam Inc. adquiere Contoso Ltd. Después de la fusión, Fabrikam desea interconectar las redes. En la siguiente ilustración se muestra este escenario:

 [![1]][1]

Las flechas discontinuas en medio de la ilustración anterior, indican las interconexiones de red que desee. En concreto, hay tres tipos de conexiones cruzadas deseadas: Conectan Fabrikam y Contoso VNets entre (1), 2) entre regionales locales y redes virtuales entre se conecta (es decir, la conexión de red local de Fabrikam a Contoso VNet y la conexión de red local de Contoso para Fabrikam VNet) y (3) Fabrikam y Contoso conectar la red local entre. 

En la tabla siguiente se muestra la tabla de enrutamiento del emparejamiento privado de la ExpressRoute de Contoso Ltd., antes de la fusión.

[![2]][2]

La siguiente tabla muestra las rutas eficaces de una máquina virtual en la suscripción de Contoso, antes de la fusión. Según la tabla, la máquina virtual en la red virtual es tener en cuenta el espacio de direcciones de red virtual y la red local de Contoso, aparte de los valores predeterminados. 

[![4]][4]

En la tabla siguiente se muestra la tabla de enrutamiento del emparejamiento privado de ExpressRoute de Fabrikam Inc., antes de la fusión.

[![3]][3]

La siguiente tabla muestra las rutas eficaces de una máquina virtual en la suscripción de Fabrikam, antes de la fusión. Según la tabla, la máquina virtual en la red virtual es tener en cuenta el espacio de direcciones de red virtual y la red local de Fabrikam, aparte de los valores predeterminados.

[![5]][5]

En este artículo, vamos a través de paso a paso y discutir cómo lograr las conexiones cruzadas deseadas utilizando las siguientes características de red de Azure:

* [Emparejamiento de redes virtuales][Virtual network peering] 
* [Conexión de ExpressRoute de red virtual][connection]
* [Alcance global][Global Reach] 

## <a name="cross-connecting-vnets"></a>Cross conectar redes virtuales

Emparejamiento de redes virtuales (emparejamiento de red virtual) proporciona más óptima y el mejor rendimiento de red cuando se conecta dos redes virtuales. Emparejamiento de VNet es compatible con dos emparejadas tanto en la misma región de Azure (comúnmente denominado el emparejamiento de VNet) en dos regiones diferentes (comúnmente denominado el emparejamiento de VNet Global). 

Vamos a configurar el emparejamiento entre las redes virtuales en suscripciones de Contoso y Fabrikam Azure Global de VNet. Para crear la red virtual de emparejamiento entre dos de las redes virtuales, consulte [crear un emparejamiento de redes virtuales] [ Configure VNet peering] artículo.

La siguiente imagen muestra la arquitectura de red después de configurar el emparejamiento de VNet Global.

[![6]][6]

La siguiente tabla muestra las rutas conocidas a la máquina virtual de la suscripción de Contoso. Preste atención a la última entrada de la tabla. Esta entrada es el resultado de cross conectan las redes virtuales.

[![7]][7]

La siguiente tabla muestra las rutas conocidas a la máquina virtual de la suscripción de Fabrikam. Preste atención a la última entrada de la tabla. Esta entrada es el resultado de cross conectan las redes virtuales.

[![8]][8]

Directamente el emparejamiento de VNet vincula dos redes virtuales (consulte no hay ningún salto para *VNetGlobalPeering* entrada en las dos tablas anteriores)

## <a name="cross-connecting-vnets-to-the-on-premises-networks"></a>Cross conectar redes virtuales a las redes locales

Un circuito ExpressRoute podemos conectarnos a varias redes virtuales. Consulte [subscripciones y límites de servicio] [ Subscription limits] para el número máximo de redes virtuales que se pueden conectar a un circuito ExpressRoute. 

Vamos a conectar circuito de Fabrikam ExpressRoute para la suscripción de Contoso red virtual y lo mismo circuito Contoso ExpressRoute a la red virtual para habilitar la conectividad cruzada entre redes virtuales y las redes locales de suscripción de Fabrikam. Para conectar una red virtual a un circuito de ExpressRoute en una suscripción diferente, es necesario crear y utilizar una autorización.  Consulte el artículo: [Conectar una red virtual a un circuito ExpressRoute][Connect-ER-VNet].

La siguiente imagen muestra la arquitectura de red después de configurar ExpressRoute entre la conectividad a las redes virtuales.

[![9]][9]

En la tabla siguiente se muestra la tabla de rutas del emparejamiento privado de ExpressRoute de Contoso Ltd., después entre la conexión de redes virtuales a las redes locales a través de ExpressRoute. Observe que la tabla de rutas tiene las rutas que pertenecen a ambas redes virtuales.

[![10]][10]

En la tabla siguiente se muestra la tabla de rutas del emparejamiento privado de ExpressRoute de Fabrikam Inc., una vez entre la conexión de redes virtuales a las redes locales a través de ExpressRoute. Observe que la tabla de rutas tiene las rutas que pertenecen a ambas redes virtuales.

[![11]][11]

La siguiente tabla muestra las rutas conocidas a la máquina virtual de la suscripción de Contoso. Preste atención a *puerta de enlace de red Virtual* entradas de la tabla. La máquina virtual ve rutas para ambas redes locales.

[![12]][12]

La siguiente tabla muestra las rutas conocidas a la máquina virtual de la suscripción de Fabrikam. Preste atención a *puerta de enlace de red Virtual* entradas de la tabla. La máquina virtual ve rutas para ambas redes locales.

[![13]][13]

>[!NOTE]
>En cualquiera de las suscripciones de Fabrikam y Contoso que también puede tener radios redes virtuales al concentrador respectivo red virtual (no se muestra un diseño de concentrador y radio en los diagramas de arquitectura de este artículo). Las conexiones cruzadas entre las puertas de enlace de red virtual de concentrador a ExpressRoute también permitirá la comunicación entre este y oeste de concentradores y radios.
>

## <a name="cross-connecting-on-premises-networks"></a>Cross conectar redes locales

Alcance Global de ExpressRoute proporciona conectividad entre redes locales que están conectadas a circuitos de ExpressRoute. Vamos a configurar el alcance Global entre Contoso y Fabrikam ExpressRoute circuitos. Dado que son los circuitos ExpressRoute en distintas suscripciones, es necesario crear y utilizar una autorización. Consulte [configurar ExpressRoute Global alcance] [ Configure Global Reach] artículo para obtener instrucciones paso a paso.

La siguiente imagen muestra la arquitectura de red después de configurar el alcance Global.

[![14]][14]

En la tabla siguiente se muestra la tabla de enrutamiento del emparejamiento privado de ExpressRoute de Contoso Ltd., después de configurar el alcance Global. Observe que la tabla de rutas tiene las rutas que pertenecen a ambas redes locales. 

[![15]][15]

En la tabla siguiente se muestra la tabla de enrutamiento del emparejamiento privado de ExpressRoute de Fabrikam Inc., después de configurar el alcance Global. Observe que la tabla de rutas tiene las rutas que pertenecen a ambas redes locales.

[![16]][16]

## <a name="next-steps"></a>Pasos siguientes

Consulte [P+F de virtual network][VNet-FAQ], para preguntas aún más en la red virtual y el emparejamiento de redes virtuales. Consulte [P+F de ExpressRoute] [ ER-FAQ] para conectividad de red de alguna pregunta sobre ExpressRoute y virtuales.

Global Reach se está dando a conocer país a país. Para ver si Global Reach está disponible en los países que desea, consulte [ExpressRoute Global Reach][Global Reach].

<!--Image References-->
[1]: ./media/cross-network-connectivity/premergerscenario.png "Escenario de aplicación"
[2]: ./media/cross-network-connectivity/contosoexr-rt-premerger.png "tabla de rutas de Contoso ExpressRoute antes de fusión"
[3]: ./media/cross-network-connectivity/fabrikamexr-rt-premerger.png "tabla de rutas de Fabrikam ExpressRoute antes de fusión"
[4]: ./media/cross-network-connectivity/contosovm-routes-premerger.png "enruta Contoso VM antes de fusión"
[5]: ./media/cross-network-connectivity/fabrikamvm-routes-premerger.png "Fabrikam VM enruta antes de fusión"
[6]: ./media/cross-network-connectivity/vnet-peering.png "la arquitectura después de emparejamiento de VNet"
[7]: ./media/cross-network-connectivity/contosovm-routes-peering.png "enruta Contoso VM después de emparejamiento de VNet"
[8]: ./media/cross-network-connectivity/fabrikamvm-routes-peering.png "enruta Fabrikam VM después de emparejamiento de VNet"
[9]: ./media/cross-network-connectivity/exr-x-connect.png "la arquitectura de ExpressRoutes la conexión cruzada"
[10]: ./media/cross-network-connectivity/contosoexr-rt-xconnect.png "tabla de rutas de Contoso ExpressRoute después cross conecta ExR y redes virtuales"
[11]: ./media/cross-network-connectivity/fabrikamexr-rt-xconnect.png "Fabrikam ExpressRoute la tabla de rutas después cross conecta ExR y redes virtuales"
[12]: ./media/cross-network-connectivity/contosovm-routes-xconnect.png "rutas Contoso VM después de cruzar conecta ExR y redes virtuales"
[13]: ./media/cross-network-connectivity/fabrikamvm-routes-xconnect.png "rutas Fabrikam VM después de cruzar conecta ExR y redes virtuales"
[14]: ./media/cross-network-connectivity/globalreach.png "la arquitectura después de configurar el alcance Global"
[15]: ./media/cross-network-connectivity/contosoexr-rt-gr.png "tabla de rutas de Contoso ExpressRoute después alcance Global"
[16]: ./media/cross-network-connectivity/fabrikamexr-rt-gr.png "tabla de rutas de Fabrikam ExpressRoute después alcance Global"

<!--Link References-->
[Virtual network peering]: https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview
[connection]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager
[Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-global-reach
[Configure VNet peering]: https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions
[Configure Global Reach]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-set-global-reach
[Subscription limits]: https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits
[Connect-ER-VNet]: https://docs.microsoft.com/azure/expressroute/expressroute-howto-linkvnet-portal-resource-manager
[ER-FAQ]: https://docs.microsoft.com/azure/expressroute/expressroute-faqs
[VNet-FAQ]: https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq