---
title: Conceptos clave para administrar la solución de VMware de Azure por CloudSimple
description: Describe los conceptos clave para administrar la solución de VMware de Azure por CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0d890553ee145ca6aafed5a34d158c6a34d9af36
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358174"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solution-by-cloudsimple"></a>Conceptos clave para la administración de la solución de VMware de Azure por CloudSimple

Administrar soluciones de VMware de Azure por CloudSimple, es necesario comprender los conceptos siguientes.

* CloudSimple Service (se muestra como solución de VMware de Azure por CloudSimple - servicio)
* Nodo CloudSimple (se muestra como solución de VMware de Azure por CloudSimple - nodo)
* CloudSimple Private Cloud
* Red de servicio
* Máquina Virtual de CloudSimple (se muestra como solución de VMware de Azure por CloudSimple - Máquina Virtual)

## <a name="cloudsimple-service"></a>Servicio CloudSimple

El servicio CloudSimple permite crear y administrar todos los recursos asociados con soluciones de VMware mediante CloudSimple desde el portal de Azure. Crear un recurso de servicio en todas las regiones donde piensa usar el servicio. 

Obtenga más información sobre la [CloudSimple Service](cloudsimple-service.md)

## <a name="cloudsimple-node"></a>Nodo CloudSimple

Un nodo CloudSimple es una dedicado, bare metal, el host de proceso y almacenamiento hiperconvergido en que se implementa el hipervisor de VMware ESXi. Este nodo, a continuación, se incorpora a la de VMware vSphere, vCenter, vSAN y plataformas NSX. También están habilitados los servicios de red CloudSimple y servicios de red perimetral. Cada nodo que actúa como una unidad de capacidad de proceso y almacenamiento que se puede comprar para crear [nubes privadas CloudSimple](cloudsimple-private-cloud.md). Comprar o reserva de nodos en una región donde está disponible el servicio CloudSimple.


Obtenga más información sobre [CloudSimple nodos](cloudsimple-node.md)

## <a name="cloudsimple-private-cloud"></a>CloudSimple Private Cloud

Una nube privada CloudSimple es un entorno aislado de pila de VMware administrado por un servidor de vCenter en su propio dominio de administración. Pila VMware incluye hosts ESXi, vSphere, vCenter, vSAN y NSX.  Las ejecuciones de la pila en (dedicado y aislado hardware sin sistema operativo) de nodos dedicados y consumido por los usuarios a través de las herramientas nativas de VMware que incluyen vCenter y NSX Manager. Nodos dedicados se implementan en las ubicaciones de Azure y son administrados por Azure. Cada nube privada se puede segmentar y protege mediante servicios como VLAN y subredes de red y las tablas del firewall.  Se crean las conexiones a su entorno local y la red de Azure mediante VPN, privada y segura y las conexiones de ExpressRoute de Azure.

Obtenga más información sobre [CloudSimple privada en la nube](cloudsimple-private-cloud.md)

## <a name="service-networking"></a>Redes de servicio

El servicio de CloudSimple proporciona una red por región donde se implementa el servicio CloudSimple. La red es un único espacio de dirección TCP capa 3 con el enrutamiento habilitado de forma predeterminada. Todas las nubes privadas y las subredes creadas en esta región se comuniquen entre sí sin ninguna configuración adicional. Crear grupos de puertos distribuido en el servidor de vCenter con las VLAN.  Puede usar las siguientes características de red para configurar y proteger los recursos de la carga de trabajo en su propia nube privada.

* [VLAN y subredes](cloudsimple-vlans-subnets.md)
* [Tablas de Firewall](cloudsimple-firewall-tables.md)
* [Puertas de enlace VPN](cloudsimple-vpn-gateways.md)
* [Dirección IP pública](cloudsimple-public-ip-address.md)
* [Conexión de red de Azure](cloudsimple-azure-network-connection.md)

## <a name="cloudsimple-virtual-machine"></a>CloudSimple virtual machine

El servicio CloudSimple permite administrar máquinas virtuales de VMware desde Azure portal. Uno o más clústeres o grupos de recursos de su entorno de vSphere pueden asignarse a la suscripción en el que se crea el servicio.

Más información sobre:

* [CloudSimple virtual machines](cloudsimple-virtual-machines.md)
* [Asignación de suscripción de Azure](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
