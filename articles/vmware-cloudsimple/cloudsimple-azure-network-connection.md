---
title: Solución de VMware CloudSimple - conexiones de red de Azure
description: Obtenga información sobre cómo conectar su red virtual de Azure a la red de área CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f2ab82b6c1b4b373c186019eaf96f9864861b9d9
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497626"
---
# <a name="azure-network-connections-overview"></a>Introducción a las conexiones de red de Azure

Cuando se crea un servicio CloudSimple en una región,:

* Crea un circuito ExpressRoute de Azure y lo asocia al servicio en esa región
* Permite la conexión de la red de área CloudSimple a su red virtual de Azure o la red local mediante Azure ExpressRoute
* Proporciona servicios de acceso que se ejecutan en su suscripción de Azure, o a la red local, desde el entorno de nube privada

La conexión es:

* Protección
* Privada
* Ancho de banda alto
* Baja latencia

## <a name="benefits"></a>Ventajas

Conexión de red de Azure le permite:

* Uso de Azure como destino de copia de seguridad para máquinas virtuales en su propia nube privada.
* Implemente servidores KMS en su suscripción de Azure para cifrar el almacén de datos de nube privada vSAN.
* Use las aplicaciones híbridas donde el nivel web de la aplicación se ejecuta en la nube pública mientras la aplicación y los niveles de base de datos que se ejecuten en su propia nube privada.

## <a name="azure-virtual-network-connection"></a>Conexión de red virtual de Azure

Las nubes privadas se pueden conectar a los recursos de Azure mediante ExpressRoute.  Puede usar esta conexión para tener acceso a diferentes recursos que se ejecutan en su suscripción de Azure desde su propia nube privada.  Esta conexión permite amplíe su red de nube privada a la red virtual de Azure.

![Conexión a red virtual de Azure ExpressRoute](media/cloudsimple-azure-network-connection.png)

## <a name="expressroute-connection-to-on-premises-network"></a>Conexión de ExpressRoute a la red local

Puede conectar el circuito de ExpressRoute de Azure existente a la región CloudSimple. Característica alcance Global de ExpressRoute se utiliza para conectar los dos circuitos entre sí.  Se establece una conexión entre los servidores locales y circuitos CloudSimple ExpressRoute.  Esta conexión le permite ampliar sus redes locales a la red de nube privada.

![Conexión de ExpressRoute local - alcance Global](media/cloudsimple-global-reach-connection.png)

## <a name="next-steps"></a>Pasos siguientes

* [Obtener información de emparejamiento de Azure virtual network para conexión CloudSimple](https://docs.azure.cloudsimple.com/virtual-network-connection)
* [Conectarse desde un entorno local a CloudSimple mediante ExpressRoute](https://docs.azure.cloudsimple.com/on-premises-connection)
