---
title: VMware Solution by CloudSimple - Conexiones de red de Azure
description: 'Aprenda a conectar su red virtual de Azure con la red de su región de CloudSimple:'
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a8e99da05f71cb01744111b41c301b11a0969057
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812711"
---
# <a name="azure-network-connections-overview"></a>Introducción a las conexiones de red de Azure

Cuando se crea un servicio CloudSimple en una región, este:

* Crea un circuito de Azure ExpressRoute y lo asocia al servicio en esa región.
* Permite la conexión desde su red de la región de CloudSimple a su red virtual de Azure o su red local mediante Azure ExpressRoute.
* Proporciona servicios de acceso que se ejecutan en su suscripción de Azure o su red local desde su entorno de nube privada.

Características de la conexión:

* Seguridad
* Privada
* Ancho de banda elevado
* Baja latencia

## <a name="benefits"></a>Ventajas

La conexión de red de Azure le permite:

* Uso de Azure como destino de copia de seguridad para máquinas virtuales en su nube privada.
* Implementar servidores KMS en su suscripción de Azure para cifrar el almacén de datos vSAN de la nube privada.
* Usar aplicaciones híbridas donde el nivel de web de la aplicación se ejecuta en la nube pública mientras la aplicación y los niveles de base de datos se ejecutan en su nube privada.

## <a name="azure-virtual-network-connection"></a>Conexión de red virtual de Azure

Las nubes privadas se pueden conectar a los recursos de Azure mediante ExpressRoute.  Puede usar esta conexión para tener acceso a diferentes recursos que se ejecutan en la suscripción de Azure desde su nube privada.  Esta conexión le permite ampliar su red de nube privada a la red virtual de Azure.

![Conexión de Azure ExpressRoute a la red virtual](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>Conexión de ExpressRoute a la red local

Puede conectar el circuito de Azure ExpressRoute existente a su región de CloudSimple. La característica Global Reach de ExpressRoute se utiliza para conectar los dos circuitos entre sí.  Se establece una conexión entre el circuito local y el circuito de ExpressRoute de CloudSimple.  Esta conexión le permite ampliar su red local a la red de nube privada.

![Conexión de ExpressRoute local - Global Reach](media/cloudsimple-global-reach-connection.png)

## <a name="next-steps"></a>Pasos siguientes

* [Obtención de información del mismo nivel para una conexión de red virtual de Azure a CloudSimple](https://docs.azure.cloudsimple.com/virtual-network-connection)
* [Conexión desde una red local a CloudSimple mediante ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection)
