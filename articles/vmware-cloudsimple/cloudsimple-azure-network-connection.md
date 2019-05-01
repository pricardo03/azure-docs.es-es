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
ms.openlocfilehash: 8ea98d6493b824bfa232ef8193388e93b97c506b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577006"
---
# <a name="azure-network-connection-overview"></a>Información general de conexión de red de Azure

Cuando se crea un servicio CloudSimple en una región,:

* Crea el circuito de ExpressRoute de Azure y lo asocia al servicio en esa región
* Se conecta la red de área CloudSimple a su red virtual de Azure o su red local mediante Azure ExpressRoute
* Proporciona servicios de acceso que se ejecutan en su suscripción de Azure, o a la red local, desde el entorno de nube privada

Esta conexión es el ancho de banda alto con una latencia baja.

## <a name="benefits"></a>Ventajas

Conexión de red de Azure le permite:

* Uso de Azure como destino de copia de seguridad para máquinas virtuales en su propia nube privada.
* Implemente servidores KMS en su suscripción de Azure para cifrar el almacén de datos de nube privada vSAN.
* Use las aplicaciones híbridas donde el nivel web de la aplicación se ejecuta en la nube pública mientras la aplicación y los niveles de base de datos que se ejecuten en su propia nube privada.

## <a name="expressroute-connection-to-on-premises-network"></a>Conexión de ExpressRoute a la red local

Puede conectar el circuito de ExpressRoute de Azure existente a la región CloudSimple. Característica alcance Global de ExpressRoute se utiliza para conectar los dos circuitos entre sí.  Se establece una conexión entre locales y los circuitos CloudSimple ExpressRoute.

Este método establece una conexión entre los dos entornos es:

* Protección
* Privada
* Ancho de banda alto
* Baja latencia

Para crear una conexión ExpressRoute a una red local, [póngase en contacto con soporte técnico](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Pasos siguientes

* [Configurar conexión de red virtual](https://docs.azure.cloudsimple.com/virtual-network-connection)