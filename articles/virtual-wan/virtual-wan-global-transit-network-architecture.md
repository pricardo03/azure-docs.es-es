---
title: Arquitectura de red de tránsito global de Azure Virtual WAN | Microsoft Docs
description: Obtenga información sobre la arquitectura de red de tránsito global de Virtual WAN.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 07/23/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand global transit network architecture as it relates to Virtual WAN.
ms.openlocfilehash: 2376c77ecc328788c842e045aafb618cbad39b0e
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2019
ms.locfileid: "68421431"
---
# <a name="global-transit-network-architecture-and-virtual-wan"></a>Arquitectura de red de tránsito global y Virtual WAN

Las empresas están adoptando la arquitectura de red de tránsito global para consolidar, conectar y controlar la huella de una TI empresarial moderna centrada en la nube. En una empresa moderna centrada en la nube, no es necesario devolver el tráfico de red a la sede central. La arquitectura de red de tránsito global se basa tanto en conceptos de red conocidos como en conceptos nuevos que son únicos para las arquitecturas de nube y basadas en la nube.

![arquitectura](./media/virtual-wan-global-transit-network-architecture/architecture2.png)

**Figura 1: Red de tránsito global con Virtual WAN**

Las empresas modernas requieren de una conectividad ubicua entre las aplicaciones, los datos y los usuarios hiperdistribuidos en la nube y en el entorno local. Azure Virtual WAN permite una arquitectura de red de tránsito global al habilitar una conectividad ubicua universal entre conjuntos distribuidos globalmente de redes virtuales, sitios, aplicaciones y usuarios. Azure Virtual WAN es un servicio administrado de Microsoft. Microsoft hospeda y administra todos los componentes de red que conforman este servicio. Para más información sobre Virtual WAN, consulte el artículo de [información general sobre Virtual WAN](virtual-wan-about.md).

En la arquitectura de Azure Virtual WAN, las regiones de Azure actúan como centros a los que puede elegir conectar sus sucursales. Una vez que las sucursales están conectadas, puede aprovechar la red troncal de Azure para establecer la conectividad tanto entre la sucursal y la red virtual como, opcionalmente, entre distintas sucursales.

Puede establecer una WAN virtual mediante la creación de un centro único de Virtual WAN en la región con el mayor número de radios (sucursales, redes virtuales, usuarios) y, luego, la conexión con el centro de radios que están en otras regiones. Como alternativa, si las radios están distribuidas geográficamente, también puede crear instancias de los centros regionales e interconectar los centros. Todos los centros son parte de la misma WAN virtual, pero se pueden asociar con distintas directivas regionales.

## <a name="hub"></a>Tránsito radial

La arquitectura de red de tránsito global se basa en un modelo de conectividad radial clásica donde el "centro" de la red que se hospeda en la nube permite la conectividad transitiva entre puntos de conexión que pueden estar distribuidos en distintos tipos de "radios".
  
En este modelo, una radio puede ser:

* Red virtual (redes virtuales)
* Sitio de sucursal física
* Usuario remoto
* Internet

![diagrama de tránsito global radial](./media/virtual-wan-global-transit-network-architecture/architecture.png)

**Figura 2: Radial**

En la figura 2 se muestra la vista lógica de la red global donde los usuarios, los sitios físicos y las redes virtuales distribuidos geográficamente están interconectados a través de un centro de redes hospedado en la nube. Esta arquitectura permite la conectividad de tránsito de un salto lógico entre los puntos de conexión de red. Las radios se conectan al centro a través de varios servicios de red de Azure, como ExpressRoute o VPN sitio a sitio para las sucursales físicas, emparejamiento de red virtual para las redes virtuales y VPN punto a sitio para los usuarios remotos.

## <a name="crossregion"></a>Conectividad entre regiones

Para una empresa, una huella de la nube por lo general sigue la huella física. La mayoría de las empresas accede a la nube desde una región más cercana a su sitio físico y a sus usuarios. Uno de los principios clave de la arquitectura de red global es habilitar la conectividad entre regiones entre los puntos de conexión y las entidades de red. Una huella de la nube puede abarcar varias regiones. Esto significa que el tráfico de una rama que está conectada a la nube en una región puede llegar a otra rama o a una red virtual en otra región mediante la conectividad entre transformadores, que actualmente se encuentra en nuestro plan de desarrollo.

## <a name="any"></a>Conectividad universal

La arquitectura de red de tránsito global permite la *conectividad universal* a través de un centro de redes central. Esta arquitectura elimina o disminuye la necesidad de modelos de conectividad de malla parcial o de malla completa que son más complejos de crear y mantener. Además, el enrutamiento de control entre las redes radiales y las redes de malla es más fácil de configurar y mantener.

La conectividad universal, en el contexto de una arquitectura global, permite que una empresa con usuarios, sucursales, centros de datos, redes virtuales y aplicaciones distribuidos globalmente se conecten entre sí a través del centro de tránsito. El centro de tránsito actúa como el sistema de tránsito global.

![rutas de tráfico](./media/virtual-wan-global-transit-network-architecture/trafficpath.png)

**Figura 3: Rutas de tráfico de Virtual WAN**

Azure Virtual WAN admite las siguientes rutas de conectividad de tránsito global. Las letras entre paréntesis hacen referencia a la figura 3.

* Sucursal a red virtual (a)  
* Sucursal a sucursal (b)
* Usuario remoto a red virtual (c)
* Usuario remoto a sucursal (d)
* Red virtual a red virtual con emparejamiento de red virtual (e)
* Global Reach de ExpressRoute 

### <a name="branchvnet"></a>Sucursal a red virtual

Sucursal a red virtual es la ruta principal admitida por Azure Virtual WAN. Esta ruta le permite conectar sucursales a las cargas de trabajo empresariales de IaaS de Azure que están implementadas en redes virtuales de Azure. Las sucursales se pueden conectar a la WAN virtual a través de ExpressRoute o de VPN de sitio a sitio. El tráfico transita a las redes virtuales que están conectadas a los centros de Virtual WAN a través de conexiones de red virtual.

### <a name="branchbranch"></a>Sucursal a sucursal

Las sucursales se pueden conectar a un centro de Azure Virtual WAN mediante circuitos de ExpressRoute o conexiones de VPN de sitio a sitio. Puede conectar las sucursales al centro de Virtual WAN que se encuentra en la región más cercana a la sucursal.

Esta opción permite que las empresas usen la red troncal de Azure para conectar las sucursales. Sin embargo, aunque esta funcionalidad esté disponible, debe ponderar los beneficios de conectar las sucursales a través de Azure Virtual WAN en comparación con usar una WAN privada.

### <a name="usertovnet"></a>Usuario remoto a red virtual

Puede habilitar el acceso remoto seguro y directo a Azure mediante conexiones de punto a sitio desde un cliente de usuario remoto a una WAN virtual. Los usuarios empresariales remotos ya no tienen que conectarse a la nube a través de una VPN corporativa.

### <a name="usertobranch"></a>Usuario remoto a sucursal

La ruta de usuario remoto a sucursal permite que los usuarios remotos que usan una conexión de punto a sitio a Azure accedan a aplicaciones y cargas de trabajo locales mediante el tránsito a través de la nube. Esta ruta brinda a los usuarios remotos la flexibilidad para acceder a las cargas de trabajo que están implementadas en Azure y en el entorno local. Las empresas pueden habilitar el servicio central de acceso remoto seguro basado en la nube en Azure Virtual WAN.

### <a name="vnetvnet"></a>Tránsito de red virtual a red virtual con emparejamiento de red virtual

Para conectar redes virtuales entre sí y admitir aplicaciones de varios niveles implementadas en varias redes virtuales, use el emparejamiento de red virtual. Actualmente no se admite un escenario de tránsito de red virtual a red virtual en Azure Virtual WAN, pero sí está en la hoja de ruta de Azure. Conectar las redes virtuales a través del emparejamiento de red virtual es la solución recomendada para las redes virtuales que se deben conectar entre sí. El [tránsito de puerta de enlace](../virtual-network/virtual-network-peering-overview.md#gateways-and-on-premises-connectivity) (en el contexto del emparejamiento de red virtual) no es necesario para Virtual WAN, porque Virtual WAN habilita automáticamente el tránsito de puerta de enlace.

### <a name="globalreach"></a>Global Reach de ExpressRoute

ExpressRoute es una forma privada y resistente de conectar las redes locales a la nube de Microsoft. Global Reach ExpressRoute es una característica de complemento para ExpressRoute. Con Global Reach, puede vincular los circuitos de ExpressRoute para crear una red privada entre las redes locales. Las sucursales que se conectan a Azure Virtual WAN a través de ExpressRoute requieren que Global Reach de ExpressRoute se comuniquen entre sí.

En este modelo, cada sucursal que se conecta al centro de Virtual WAN mediante ExpressRoute se puede conectar a las redes virtuales mediante la ruta de sucursal a red virtual. El tráfico de sucursal a sucursal no atravesará el centro, porque Global Reach de ExpressRoute permite una ruta más óptima a través de Azure WAN.

## <a name="security"></a>Seguridad y control de directivas

El centro de redes virtuales interconecta todo el tráfico de tránsito y, probablemente, lo ve todo. Puede ser el lugar para hospedar servicios y funciones de red como un enrutamiento de nube, seguridad y directiva de red y el control de acceso de Internet.

## <a name="next-steps"></a>Pasos siguientes

Cree una conexión mediante Virtual WAN.

* [Creación de una conexión de sitio a sitio mediante Azure Virtual WAN](virtual-wan-site-to-site-portal.md)
* [Conexiones de ExpressRoute con Virtual WAN](virtual-wan-expressroute-portal.md)
