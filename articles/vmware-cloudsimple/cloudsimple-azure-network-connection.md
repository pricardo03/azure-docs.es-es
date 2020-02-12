---
title: 'VMware Solutions (AVS): conexiones de red de Azure'
description: 'Aprenda a conectar su red virtual de Azure con la red de su región de AVS:'
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: bf11d4e2676179e8b71d3a03f8ed3cbcb4cfba9d
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025085"
---
# <a name="azure-network-connections-overview"></a>Introducción a las conexiones de red de Azure

Cuando se crea un servicio de AVS en una región y se crean nodos, se puede:

* Solicitar un circuito de Azure ExpressRoute y asociarlo a la red de AVS de esa región.
* Conectar la red de la región de AVS a la red virtual de Azure o la red local mediante Azure ExpressRoute.
* Proporcionar acceso a servicios que se ejecutan en la suscripción de Azure o en la red local desde el entorno de nube privada.

La conexión de ExpressRoute es de ancho de banda alto con baja latencia.

## <a name="benefits"></a>Ventajas

La conexión de red de Azure le permite:

* Uso de Azure como destino de copia de seguridad para máquinas virtuales en su nube privada.
* Implementar servidores KMS en su suscripción de Azure para cifrar el almacén de datos vSAN de la nube privada.
* Usar aplicaciones híbridas donde el nivel de web de la aplicación se ejecuta en la nube pública de AVS mientras los niveles de base de datos y la aplicación se ejecutan en su nube privada.

## <a name="azure-virtual-network-connection"></a>Conexión de red virtual de Azure

Las nubes privadas de AVS se pueden conectar a los recursos de Azure mediante ExpressRoute. La conexión de ExpressRoute permite acceder a recursos que se ejecutan en la suscripción de Azure desde la nube privada de AVS. Esta conexión permite extender la red de nube privada de AVS a la red virtual de Azure. Las rutas de la red de AVS se intercambian con la red virtual de Azure a través de BGP. Si tiene configurado el emparejamiento de red virtual, se puede acceder a todas las redes virtuales emparejadas desde la red de AVS.

![Conexión de Azure ExpressRoute a la red virtual](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>Conexión de ExpressRoute a la red local

Puede conectar el circuito de Azure ExpressRoute existente a su región de AVS. La característica Global Reach de ExpressRoute se utiliza para conectar los dos circuitos entre sí. Se establece una conexión entre el circuito local y el circuito de ExpressRoute de AVS. Esta conexión le permite ampliar las redes locales a una red de nube privada de AVS. Las rutas de la red de AVS se intercambian a través de BGP con la red local.

![Conexión de ExpressRoute local - Global Reach](media/cloudsimple-global-reach-connection.png)

## <a name="connection-to-on-premises-network-and-azure-virtual-network"></a>Conexión a la red local y a la red virtual de Azure

Las conexiones a la red local y a la red virtual de Azure pueden coexistir desde la red AVS. La conexión usa BGP para intercambiar rutas entre la red local, la red virtual de Azure y la red de AVS. Al conectar la red de AVS a la red virtual de Azure en presencia de una conexión de Global Reach, las rutas de la red virtual de Azure estarán visibles en la red local. El intercambio de ruta se produce en Azure entre los enrutadores perimetrales.

![Conexión local de ExpressRoute con conexión de red virtual de Azure](media/cloudsimple-global-reach-and-vnet-connection.png)

### <a name="important-considerations"></a>Consideraciones importantes

La conexión a una red de AVS desde la red local o desde la red virtual de Azure permite el intercambio de rutas entre todas las redes.

* Una red virtual de Azure será visible desde la red local y desde una red de AVS.
* Si se ha conectado a la red virtual de Azure desde una red local, la conexión a una red de AVS mediante Global Reach permitirá el acceso a redes virtuales desde la red de AVS.
* Las direcciones de subred **no deben** superponerse entre ninguna de las redes conectadas.
* AVS **no** anunciará una ruta predeterminada para las conexiones de ExpressRoute.
* Si el enrutador local anuncia la ruta predeterminada, el tráfico de la red de AVS y de la red virtual de Azure usará la ruta predeterminada anunciada. Como resultado, no se puede acceder a las máquinas virtuales de Azure mediante direcciones IP públicas.

## <a name="next-steps"></a>Pasos siguientes

* [Conexión de la red virtual de Azure a AVS mediante ExpressRoute](virtual-network-connection.md)
* [Conexión desde una red local a AVS mediante ExpressRoute](on-premises-connection.md)
