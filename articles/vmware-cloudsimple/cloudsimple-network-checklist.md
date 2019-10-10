---
title: 'Azure VMware Solution by CloudSimple: lista de comprobación de red'
description: Lista de comprobación para la asignación de CIDR de red en Azure VMware Solution by CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 09/25/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c31942b198697ddc913f3732fd41409334ff0ef6
ms.sourcegitcommit: 80da36d4df7991628fd5a3df4b3aa92d55cc5ade
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/02/2019
ms.locfileid: "71817395"
---
# <a name="networking-prerequisites-for-azure-vmware-solution-by-cloudsimple"></a>Requisitos previos de redes para Azure VMware Solution by CloudSimple

Azure VMware Solution by CloudSimple ofrece un entorno de nube privada de VMware que es accesible para los usuarios y las aplicaciones desde entornos locales, desde dispositivos administrados por la empresa y desde recursos de Azure. La conectividad se proporciona a través de servicios de red, como las VPN y las conexiones ExpressRoute.  Algunos de los servicios de red requieren para su habilitación que se especifiquen los intervalos de direcciones de red.  En las tablas de este artículo se describe el conjunto de intervalos de direcciones y los servicios correspondientes que usan las direcciones especificadas.  Algunas direcciones son obligatorias y otras dependen de los servicios que desea implementar.  Estos espacios de direcciones no deben superponerse con ninguna de las subredes locales, las subredes de Azure Virtual Network o las subredes de carga de trabajo de CloudSimple planeadas.

## <a name="network-address-ranges-required-for-creating-a-private-cloud"></a>Intervalos de direcciones de red necesarios para crear una nube privada

Durante la creación del servicio CloudSimple y una nube privada, se necesita el siguiente intervalo de CIDR de red.

| Nombre/usado para     | DESCRIPCIÓN                                                                                                                            | Intervalo de direcciones            |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------|--------------------------|
| CIDR de puerta de enlace      | Se requiere para los servicios perimetrales (puertas de enlace de VPN).  Este CIDR es necesario durante la creación del servicio CloudSimple y debe proceder del espacio RFC 1918. | /28                      |
| CIDR de vSphere/vSAN | Se requiere para las redes de administración de VMware. Este CIDR debe especificarse durante la creación de la nube privada.                                    | /24 o /23 o /22 o /21 |

## <a name="network-address-range-required-for-azure-network-connection-to-on-premises-network"></a>Intervalo de direcciones de red necesario para la conexión de red de Azure a la red local

La conexión desde la [red local a la red de nube privada mediante ExpressRoute](on-premises-connection.md) establece una conexión Global Reach.  La conexión intercambiará las rutas a través de BGP entre la red local, la red de nube privada y las redes de Azure.

| Nombre/usado para             | DESCRIPCIÓN                                                                                                                                                                             | Intervalo de direcciones |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------|
| CIDR de emparejamiento de ExpressRoute | Se requiere cuando se usa Global Reach de ExpressRoute para la conectividad local. Este CIDR debe proporcionarse cuando se realiza una solicitud de conexión Global Reach a través de una incidencia de soporte técnico. | /29           |

## <a name="network-address-range-required-for-using-site-to-site-vpn-connection-to-on-premises-network"></a>Intervalo de direcciones de red necesario para usar la conexión VPN de sitio a sitio a la red local

La conexión desde la [red local a la red de nube privada mediante la VPN de sitio a sitio](vpn-gateway.md) requiere las siguientes direcciones IP, red local e identificadores. 

| Dirección/intervalo de direcciones | DESCRIPCIÓN                                                                                                                                                                                                                                                           |
|-----------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Dirección IP de par               | Dirección IP pública de la puerta de enlace de VPN local. Se requiere para establecer una conexión VPN de sitio a sitio entre el centro de datos local y la región de servicio de CloudSimple. Esta dirección IP es necesaria durante la creación de una puerta de enlace de VPN de sitio a sitio.                                         |
| Identificador de par       | Identificador de par de la puerta de enlace de VPN local. Normalmente, es lo mismo que la **dirección IP de par**.  Si se especifica un identificador único en la puerta de enlace de VPN local, debe especificarse el identificador.  Este identificador es necesario durante la creación de la puerta de enlace de VPN de sitio a sitio.   |
| Redes locales   | Prefijos de redes locales que necesitan acceso a redes de CloudSimple de la región.  Indique todos los prefijos de redes locales que tendrán acceso a la red de CloudSimple, incluida la red de cliente desde la que accederán los usuarios.                                         |

## <a name="network-address-range-required-for-using-point-to-site-vpn-connections"></a>Intervalo de direcciones de red necesario para usar conexiones VPN de punto a sitio.

La conexión VPN de punto a sitio permite el acceso a la red de CloudSimple desde un equipo cliente.  La [configuración de VPN de punto a sitio](vpn-gateway.md) requiere que se especifique el siguiente intervalo de direcciones de red.

| Dirección/intervalo de direcciones | DESCRIPCIÓN                                                                                                                                                                                                                                                                                                  |
|-----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Subred de cliente         | Las direcciones DHCP se proporcionarán desde la subred de cliente cuando se conecte mediante VPN de punto a sitio. Esta subred es necesaria al crear una puerta de enlace de VPN de punto a sitio en el portal de CloudSimple.  La red se dividirá en dos subredes: una se usará para la conexión UDP y otra para las conexiones TCP. |

## <a name="next-steps"></a>Pasos siguientes

* [Inicio rápido: Creación del servicio CloudSimple](quickstart-create-cloudsimple-service.md)
* [Inicio rápido: Creación de una nube privada](quickstart-create-private-cloud.md)
* Más información sobre las [conexiones de red de Azure](cloudsimple-azure-network-connection.md)
* Más información sobre las [puertas de enlace de VPN](cloudsimple-vpn-gateways.md)
