---
title: Arquitectura de red de tránsito global red WAN Virtual Azure | Microsoft Docs
description: Obtenga información acerca de la arquitectura de red de tránsito global para la red WAN Virtual
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: article
ms.date: 05/06/2019
ms.author: cherylmc
Customer intent: As someone with a networking background, I want to understand global transit network architecture as it relates to Virtual WAN.
ms.openlocfilehash: 8cda617ca60a17fceaaa818480ff9bbaef46c3fd
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2019
ms.locfileid: "65414063"
---
# <a name="global-transit-network-architecture-and-virtual-wan"></a>Arquitectura de red de tránsito global y la red WAN Virtual

Arquitectura de red de tránsito global se adopta las empresas a consolidar, conectarse y controlar la huella en TI de empresa modernas centradas en la nube. En una empresa centrada en la nube moderna, el tráfico de red no necesita ser backhauled a la sede central. Arquitectura de red de tránsito global se basa en conceptos familiares de red y nuevos conceptos que son únicos en la nube y las arquitecturas basadas en la nube.

![arquitectura](./media/virtual-wan-global-transit-network-architecture/architecture2.png)

**Figura 1: Red de tránsito global con la red WAN Virtual**

Las empresas modernas requieren una conectividad ubicua entre aplicaciones distribuidas hyper, datos y los usuarios en la nube y locales. WAN Virtual Azure permite una arquitectura de red de tránsito global mediante la conectividad ubicua, y a cualquier entre conjuntos de distribución global de redes virtuales, sitios, aplicaciones y los usuarios. WAN Virtual Azure es un servicio administrado por Microsoft. Todos los componentes de red que está formado por este servicio están hospedados y administrados por Microsoft. Para obtener más información acerca de la red WAN Virtual, consulte el [información general de WAN Virtual](virtual-wan-about.md) artículo.

En la arquitectura de la WAN Virtual de Azure, regiones de Azure sirven de concentradores a los que puede elegir conectar sus ramas. Una vez que están conectadas las ramas, puede aprovechar la red troncal de Azure para establecer la rama a la red virtual y, opcionalmente, conectividad de la rama a la rama.

Puede establecer mediante la creación de un único centro de la red WAN Virtual en la región que tiene el mayor número de radios (ramas, las redes virtuales, los usuarios) y, a continuación, conecte los radios que se encuentran en otras regiones en el centro de una red WAN virtual. Como alternativa, si los radios están distribuidos geográficamente, también puede crear una instancia de centros regionales e interconectar los concentradores. Los concentradores son todas forman parte de la misma red WAN virtual, pero pueden estar asociadas con diferentes directivas regionales.

## <a name="hub"></a>Tránsito de concentrador y radio

La arquitectura de red de tránsito global se basa en un modelo de conectividad de concentrador y radio clásico donde la red hospedado en la nube 'central' permite transitiva conectividad entre los puntos de conexión que se puede distribuir en distintos tipos de 'radios'.
  
En este modelo, puede ser un radio:

* Red virtual (Vnet)
* Sitio de rama física
* Usuario remoto
* Internet

![diagrama de tránsito global de concentrador y radio](./media/virtual-wan-global-transit-network-architecture/architecture.png)

**Figura 2: Hub-and-spoke**

Figura 2 muestra la vista lógica de la red global donde usuarios geográficamente distribuido, sitios físicos y las redes virtuales están conectadas entre sí a través de un concentrador de red hospedado en la nube. Esta arquitectura permite la conectividad de tránsito de un salto lógico entre los puntos de conexión de red. Los radios se conectan al centro de diversas redes servicios de Azure, como VPN de sitio a sitio o ExpressRoute para ramas físicas, red virtual de emparejamiento de redes virtuales y VPN de punto a sitio para los usuarios remotos.

## <a name="crossregion"></a>Conectividad entre regiones

Para una empresa, una superficie en la nube normalmente sigue a la superficie física. La mayoría de las empresas tener acceso a la nube desde una región más cercana a su sitio físico y los usuarios. Uno de los principios claves de la arquitectura de red global es habilitar la conectividad entre regiones entre entidades de red y los puntos de conexión. Una superficie en la nube puede abarcar varias regiones. Esto significa que el tráfico de una bifurcación que está conectado a la nube en una región puede llegar a otra rama o una red virtual en una región distinta.

## <a name="any"></a>Conectividad y a cualquier

Arquitectura de red de tránsito global permite *conectividad y a cualquier* a través de un concentrador central. Esta arquitectura se elimina o reduce la necesidad de malla completa o modelos de conectividad de malla parcial que son más complejos para crear y mantener. Además, un control de enrutamiento en el concentrador y radio frente a las redes de malla es más fácil de configurar y mantener.

Cualquier conectividad, en el contexto de una arquitectura global, permite a las empresas con usuarios distribuidos globalmente, ramas, los centros de datos, las redes virtuales y las aplicaciones se conecten entre sí a través del centro de tránsito. El centro de tránsito actúa como el sistema global de tránsito.

![rutas de tráfico](./media/virtual-wan-global-transit-network-architecture/trafficpath.png)

**Figura 3: Rutas de acceso de tráfico de WAN virtual**

WAN Virtual Azure admite las siguientes rutas de conectividad de tránsito global. Las letras entre paréntesis se asignan a la figura 3.

* Rama de red (a)  
* Rama de rama (b)
* Usuario-a-red virtual remota (c)
* Usuario-a-sucursales remotas (d)
* VNet a VNet con el emparejamiento de red virtual (e)
* Alcance Global de ExpressRoute 

### <a name="branchvnet"></a>Branch-to-VNet

Rama a la red virtual es la ruta de acceso principal admitido por Azure red WAN Virtual. Esta ruta de acceso le permite conectar las ramas con cargas de trabajo empresariales de IAAS de Azure que están implementadas en redes virtuales de Azure. Las ramas se pueden conectar a la red WAN virtual a través de ExpressRoute o VPN de sitio a sitio. Tránsito de tráfico para redes virtuales que están conectados a los concentradores WAN virtuales a través de conexiones de red virtual.

### <a name="branchbranch"></a>Rama de rama

Las ramas se pueden conectar a un centro de WAN Virtual de Azure mediante los circuitos de ExpressRoute o conexiones VPN de sitio a sitio. Puede conectar las ramas en el concentrador de red WAN Virtual que se encuentra en la región más cercana a la rama.

Esta opción permite a las empresas aprovechar la red troncal de Azure para conectar las ramas. Sin embargo, aunque esta funcionalidad está disponible, debe sopesar las ventajas de la conexión ramas a través de WAN Virtual de Azure frente a una red privada WAN.

### <a name="usertovnet"></a>Usuario-a-red virtual remota

Puede habilitar el acceso remoto directa y segura a Azure mediante conexiones de punto a sitio desde un cliente de usuario remoto a una red WAN virtual. Los usuarios remotos de empresa ya no tienen que hairpin a la nube mediante una VPN corporativa.

### <a name="usertobranch"></a>Usuario sucursales remotas

La ruta de acceso de usuario rama remota permite a los usuarios remotos que usan una conexión de punto a sitio a aplicaciones y cargas de trabajo de acceso de Azure en el entorno local si el tránsito a través de la nube. Esta ruta de acceso ofrece la flexibilidad de los usuarios remotos para las cargas de trabajo de acceso que están implementados en Azure y locales. Las empresas pueden habilitar el servicio de acceso remoto seguro basado en la nube central en WAN Virtual de Azure.

### <a name="vnetvnet"></a>Tránsito de la red virtual a red virtual mediante emparejamiento de VNet

Para conectar redes virtuales entre sí con el fin de admitir aplicaciones de niveles múltiples que se implementan a través de varias redes virtuales, utilice el emparejamiento de VNet. No se admite actualmente un escenario de tránsito de red virtual a red virtual a través de WAN Virtual de Azure, pero está en el plan de Azure. Conexión de redes virtuales a través del emparejamiento de red virtual es la solución recomendada para las redes virtuales que deben estar conectados entre sí. Para obtener más información sobre el emparejamiento de red virtual, consulte [información general sobre el emparejamiento de red virtual](../virtual-network/virtual-network-peering-overview.md).

### <a name="globalreach"></a>Alcance Global de ExpressRoute

ExpressRoute es una forma privada y resistente para conectar sus redes locales a Microsoft Cloud. Alcance Global de ExpressRoute es una característica del complemento de ExpressRoute. Con alcance Global, puede vincular los circuitos de ExpressRoute para realizar una red privada entre las redes locales. Las ramas que están conectadas a Azure red WAN Virtual mediante ExpressRoute requieren el alcance Global de ExpressRoute se comuniquen entre sí.

En este modelo, cada bifurcación que está conectado a la red WAN Virtual concentrador mediante ExpressRoute puede conectarse a redes virtuales mediante la ruta de acceso de la rama a la red virtual. Tráfico de la rama de rama no tránsito del concentrador porque alcance Global de ExpressRoute permite una ruta más óptima a través de WAN de Azure.

## <a name="security"></a>Seguridad y control de directivas

El concentrador de red virtual interconexiones y potencialmente ve todo el tráfico en tránsito. Puede ser el lugar para las funciones de red host central y servicios, como tales un enrutamiento en la nube y directivas de redes y seguridad, control de acceso a Internet.

## <a name="next-steps"></a>Pasos siguientes

Crear una conexión con la red WAN Virtual.

* [Conexiones de sitio a sitio mediante una red WAN Virtual](virtual-wan-site-to-site-portal.md)
* [Conexiones de punto a sitio mediante una red WAN Virtual](virtual-wan-point-to-site-portal.md)
* [Conexiones de ExpressRoute con la red WAN Virtual](virtual-wan-expressroute-portal.md)
