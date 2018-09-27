---
title: 'Introducción a ExpressRoute: Ampliación de la red local a Azure mediante una conexión privada | Microsoft Docs'
description: La introducción técnica sobre ExpressRoute explica cómo funciona una conexión de ExpressRoute a la hora de ampliar la red local a Azure mediante una conexión privada.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: overview
ms.date: 09/19/2018
ms.author: cherylmc
ms.openlocfilehash: f1bc447f73912ccf04f0c0cf7d6460d472bace4c
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46961916"
---
# <a name="expressroute-overview"></a>Información general sobre ExpressRoute
Microsoft Azure ExpressRoute le permite ampliar sus redes locales en la nube de Microsoft a través de una conexión privada que facilita un proveedor de conectividad. Con ExpressRoute, se pueden establecer conexiones con servicios en la nube de Microsoft, como Microsoft Azure, Office 365 y Dynamics 365.

La conectividad puede ser desde una red de conectividad universal (IP VPN), una red Ethernet de punto a punto, o una conexión cruzada virtual a través de un proveedor de conectividad en una instalación de ubicación compartida. Las conexiones ExpressRoute no pasan por la red pública de Internet. Esto permite a las conexiones de ExpressRoute ofrecer más confiabilidad, más velocidad, menor latencia y mayor seguridad que las conexiones normales a través de Internet. Para información sobre cómo conectar la red a Microsoft mediante ExpressRoute, consulte [ExpressRoute connectivity models](expressroute-connectivity-models.md) (Modelos de conectividad de ExpressRoute).

![](./media/expressroute-introduction/expressroute-connection-overview.png)

## <a name="key-benefits"></a>Ventajas principales

* Conectividad de nivel 3 entre su red local y Microsoft Cloud a través de un proveedor de conectividad. La conectividad puede ser desde una red de conectividad universal (IP VPN), una red Ethernet de punto a punto, o una conexión cruzada virtual a través de un intercambio de Ethernet.
* Conectividad de servicios en la nube de Microsoft en todas las regiones dentro de la región geopolítica.
* Conectividad global a los servicios de Microsoft en todas las regiones con el complemento ExpressRoute Premium.
* Enrutamiento dinámico entre la red y Microsoft a través de protocolos estándar del sector (BGP).
* Redundancia integrada en todas las ubicaciones de configuración entre pares para una mayor confiabilidad.
* El tiempo de actividad de conexión [SLA](https://azure.microsoft.com/support/legal/sla/).
* Compatibilidad con QoS de Skype para la empresa.

Para más información, consulte [P+F de ExpressRoute](expressroute-faqs.md).

## <a name="features"></a>Características

### <a name="layer-3-connectivity"></a>Conectividad de nivel 3
Microsoft usa el protocolo de enrutamiento dinámico estándar del sector (BGP) para intercambiar las rutas entre su red local, las instancias de Azure y las direcciones públicas de Microsoft.  Establecemos varias sesiones BGP con su red para perfiles de tráfico diferentes. Puede encontrar más información en el artículo [Circuitos y dominios de enrutamiento de ExpressRoute](expressroute-circuit-peerings.md) .

### <a name="redundancy"></a>Redundancia
Cada circuito ExpressRoute consta de dos conexiones a dos enrutadores perimetrales de Microsoft Enterprise (MSEE) desde el proveedor de conectividad o el perímetro de la red. Microsoft requiere una conexión BGP dual desde el proveedor de conectividad o su sitio, uno a cada MSEE. Si lo desea, tiene la opción de no implementar dispositivos redundantes o circuitos Ethernet en el extremo. Sin embargo, los proveedores de conectividad usan dispositivos redundantes para garantizar que las conexiones se entregan a Microsoft de forma redundante. Una configuración de conectividad redundante de nivel 3 es un requisito para nuestro [SLA](https://azure.microsoft.com/support/legal/sla/) sea válido.

### <a name="connectivity-to-microsoft-cloud-services"></a>Conectividad con los Servicios en la nube de Microsoft

Las conexiones ExpressRoute habilitan el acceso a los servicios siguientes:

* Servicios de Microsoft Azure
* Servicios de Microsoft Office 365
* Microsoft Dynamics 365

> [!NOTE]
> [!INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]
> 

También puede visitar la página [Preguntas más frecuentes de ExpressRoute](expressroute-faqs.md) para obtener una lista de servicios admitidos a través de ExpressRoute. 

### <a name="connectivity-to-all-regions-within-a-geopolitical-region"></a>Conectividad a todas las regiones dentro de una región geopolítica
Puede conectarse a Microsoft en una de nuestras [ubicaciones de configuración entre pares](expressroute-locations.md) y tener acceso a todas las regiones dentro de la región geopolíticas. 

Por ejemplo, si se conectó a Microsoft en Ámsterdam a través de ExpressRoute, tiene acceso a todos los Servicios en la nube de Microsoft hospedados en Europa septentrional y Europa occidental. Para obtener información general sobre las regiones geopolíticas, las regiones de Microsoft Cloud asociadas y las correspondientes ubicaciones de emparejamiento de ExpressRoute, consulte [Asociados de ExpressRoute y ubicaciones de emparejamiento](expressroute-locations.md).

### <a name="global-connectivity-with-expressroute-premium-add-on"></a>Conectividad global con el complemento ExpressRoute Premium
Puede habilitar la característica de complemento ExpressRoute Premium para ampliar la conectividad a través de límites geopolíticos. Por ejemplo, si se conectó a Microsoft en Ámsterdam a través de ExpressRoute, tendrá acceso a todos los servicios en la nube de Microsoft hospedados en todas las regiones del mundo (excluidas las nubes nacionales). Puede tener acceso a los servicios implementados en Sudamérica o Australia del mismo modo que accede a las regiones de Europa septentrional y occidental.

### <a name="across-on-premises-connectivity-with-expressroute-global-reach"></a>Conectividad local con Global Reach de ExpressRoute

Puede permitir que Global Reach de ExpressRoute intercambie datos entre los sitios remotos mediante la conexión de los diferentes circuitos de ExpressRoute. Por ejemplo, si tiene un centro de datos privado en California conectado a ExpressRoute en Silicon Valley, y otro centro de datos privado en Texas conectado a ExpressRoute en Dallas, con Global Reach de ExpressRoute, puede conectar los centros de datos privados entre sí a través de dos circuitos de ExpressRoute. El tráfico del centro de datos pasará a través de la red de Microsoft.

Para obtener más información, consulte [Global Reach de ExpressRoute](expressroute-global-reach.md).

### <a name="rich-connectivity-partner-ecosystem"></a>Ecosistema de socios de conectividad enriquecida
ExpressRoute tiene un ecosistema de proveedores de conectividad y asociados integradores de sistema en constante crecimiento. Para obtener la información más reciente, consulte el artículo [Ubicaciones y proveedores de servicios de ExpressRoute](expressroute-locations.md).

### <a name="connectivity-to-national-clouds"></a>Conectividad con nubes nacionales
Microsoft administra entornos de nube aislados para regiones geopolíticas y segmentos de clientes especiales. Acuda a la página [Ubicaciones y proveedores de servicios de ExpressRoute](expressroute-locations.md) para obtener una lista de nubes y proveedores nacionales.

### <a name="expressroute-direct"></a>ExpressRoute Direct

ExpressRoute Direct ofrece a los clientes la capacidad para conectarse directamente a la red global de Microsoft en ubicaciones de emparejamiento distribuidas estratégicamente por todo el mundo. ExpressRoute Direct proporciona conectividad dual de 100 Gbps, que es compatible con la conectividad activa/activa a escala. 

Algunas de las características clave que ofrece ExpressRoute Direct incluyen:

* Ingesta de datos masivos en servicios como Storage y Cosmos DB 
* Aislamiento físico para sectores regulados y que necesitaron conectividades dedicadas y aisladas, como gubernamentales, con la banca y de venta directa 
* Control granular del circuito de distribución en función de la unidad de negocio

Para obtener más información, consulte [Acerca de ExpressRoute Direct](https://go.microsoft.com/fwlink/?linkid=2022973).

### <a name="bandwidth-options"></a>Opciones de ancho de banda
Puede comprar los circuitos ExpressRoute para una amplia gama de anchos de banda. A continuación se enumera la lista de anchos de banda admitidos. Asegúrese de consultar con su proveedor de conectividad para determinar la lista de anchos de banda admitidos que proporcionan.

* 50 Mbps
* 100 Mbps
* 200 Mbps
* 500 Mbps
* 1 Gbps
* 2 Gbps
* 5 Gbps
* 10 Gbps

### <a name="dynamic-scaling-of-bandwidth"></a>Escalado dinámico del ancho de banda
Puede aumentar el ancho de banda del circuito ExpressRoute (dentro de lo posible) sin necesidad de eliminar sus conexiones. 

### <a name="flexible-billing-models"></a>Modelos flexibles de facturación
Puede elegir el modelo de facturación que mejor le convenga. Elija entre los modelos de facturación enumerados a continuación. Para más información, consulte [P+F de ExpressRoute](expressroute-faqs.md).

* **Datos ilimitados**. El circuito ExpressRoute se cobra según un precio mensual, y todas las transferencias de datos entrantes y salientes están incluidas de forma gratuita. 
* **Datos medidos**. El circuito ExpressRoute se cobra según un precio mensual. Todas las transferencias de datos de entrada son gratuitas. Las transferencias de datos de salida se cobran de acuerdo a un precio por GB de transferencia de datos. Las tasas de transferencia de datos varían según la región.
* **Complemento ExpressRoute Premium**. ExpressRoute Premium es un complemento que se agrega a un circuito ExpressRoute. El complemento ExpressRoute Premium ofrece las siguientes capacidades: 
  * Límites de ruta ampliados para las configuraciones entre pares públicos y privados de Azure, de 4.000 rutas a 10.000 rutas.
  * Conectividad global para los servicios. Un circuito ExpressRoute creado en cualquier región (excepto las nubes nacionales) tendrá acceso a los recurso de cualquier otra región del mundo. Por ejemplo, un circuito ExpressRoute aprovisionado en Silicon Valley puede acceder a una red virtual creada en Europa occidental .
  * Número de vínculos de red virtual por circuito ExpressRoute ampliado de 10 a un límite superior, que depende del ancho de banda del circuito.

## <a name="faq"></a>Preguntas más frecuentes

Para ver las preguntas más frecuentes sobre ExpressRoute, consulte [P+F de ExpressRoute](expressroute-faqs.md).

## <a name="next-steps"></a>Pasos siguientes

* Información acerca de los [Modelos de conectividad de ExpressRoute](expressroute-connectivity-models.md).
* Obtenga información acerca de las conexiones ExpressRoute y dominios de enrutamiento. Consulte [Circuitos y dominios de enrutamiento de ExpressRoute](expressroute-circuit-peerings.md).
* Busque un proveedor de servicios. Consulte [Asociados de ExpressRoute de Azure y ubicaciones de emparejamiento](expressroute-locations.md).
* Asegúrese de que se cumplen todos los requisitos previos. Consulte [Requisitos previos de ExpressRoute](expressroute-prerequisites.md).
* Consulte los requisitos de [enrutamiento](expressroute-routing.md), [NAT](expressroute-nat.md) y [QoS](expressroute-qos.md).
* Configure su conexión ExpressRoute.
  * [Creación de un circuito ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md)
  * [Configuración del emparejamiento de un circuito ExpressRoute](expressroute-howto-routing-portal-resource-manager.md)
  * [Conexión de una red virtual a un circuito ExpressRoute](expressroute-howto-linkvnet-portal-resource-manager.md)
* Obtenga información sobre las demás [funcionalidades de red](../networking/networking-overview.md) clave de Azure.
