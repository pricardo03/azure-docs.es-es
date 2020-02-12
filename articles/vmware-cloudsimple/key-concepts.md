---
title: Conceptos clave para administrar Azure VMware Solutions (AVS)
description: Describe los conceptos clave para administrar Azure VMware Solutions (AVS)
titleSuffix: Azure VMware Solutions (AVS)
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: c581ffbba7fbaefc8fbd0ffc7e92350bd69802b8
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025238"
---
# <a name="key-concepts-for-administration-of-azure-vmware-solutions-avs"></a>Conceptos clave para la administración de Azure VMware Solutions (AVS)

La administración de Azure VMware Solutions (AVS) requiere entender los conceptos siguientes:

* Servicio de AVS, que se muestra como Azure VMware Solutions (AVS): servicio
* Nodo de AVS, que se muestra como Azure VMware Solutions (AVS): nodo
* Nube privada de AVS
* Redes de servicio
* Máquina virtual de AVS, que se muestra como Azure VMware Solutions (AVS): máquina virtual

## <a name="avs-service"></a>Servicio de AVS

Con el servicio de AVS, puede crear y administrar todos los recursos asociados con VMware Solutions (AVS) desde Azure Portal. Cree un recurso de servicio en todas las regiones donde piense usar el servicio.

Obtenga más información sobre el [servicio de AVS](cloudsimple-service.md).

## <a name="avs-node"></a>Nodo de AVS

Un nodo de AVS es un host de proceso y almacenamiento hiperconvergido, sin sistema operativo y dedicado en el que se implementa el hipervisor ESXi de VMware. Después, este nodo se incorpora a las plataformas VMware vSphere, vCenter, vSAN y NSX. También se habilitan los servicios de red de AVS y los de red perimetral. Cada nodo actúa como una unidad de capacidad de proceso y almacenamiento que puede aprovisionar para crear [nubes privadas de AVS](cloudsimple-private-cloud.md). Los nodos se aprovisionan o reservan en una región donde está disponible el servicio de AVS.

Obtenga más información sobre los [nodos de AVS](cloudsimple-node.md).

## <a name="avs-private-cloud"></a>Nube privada de AVS

Una nube privada de AVS es un entorno de pila de VMware aislada administrado por un servidor vCenter en su propio dominio de administración. La pila VMware incluye hosts ESXi, vSphere, vCenter, vSAN y NSX. La pila se ejecuta en nodos dedicados (hardware sin sistema operativo dedicado y aislado) y los usuarios la consumen a través de herramientas nativas de VMware que incluyen vCenter y NSX Manager. Los nodos dedicados se implementan en ubicaciones de Azure y son administrados por Azure. Cada nube privada de AVS se puede segmentar y proteger mediante servicios de red, como VLAN, subredes y tablas de firewall. Las conexiones al entorno local y la red de Azure se crean mediante VPN privadas y seguras, y conexiones de Azure ExpressRoute.

Obtenga más información sobre la [nube privada de AVS](cloudsimple-private-cloud.md).

## <a name="service-networking"></a>Redes de servicio

El servicio de AVS proporciona una red por región donde se implementa el servicio de AVS. La red es un único espacio de direcciones TCP de nivel 3 con el enrutamiento habilitado de forma predeterminada. Todas las nubes privadas de AVS y las subredes creadas en esta región se comunican entre sí sin ninguna configuración adicional. En vCenter, los grupos de puertos distribuidos se crean mediante VLAN. Puede usar las características de red siguientes para configurar y proteger los recursos de carga de trabajo en la nube privada de AVS:

* [VLAN y subredes](cloudsimple-vlans-subnets.md)
* [Tablas de firewall](cloudsimple-firewall-tables.md)
* [Puertas de enlace de VPN](cloudsimple-vpn-gateways.md)
* [Dirección IP pública](cloudsimple-public-ip-address.md)
* [Conexión de red de Azure](cloudsimple-azure-network-connection.md)

## <a name="avs-virtual-machine"></a>Máquina virtual de AVS

Con el servicio de AVS, puede administrar máquinas virtuales de VMware desde Azure Portal. Se pueden asignar uno o más clústeres o grupos de recursos del entorno de vSphere a la suscripción en la que se crea el servicio.

Más información sobre:

* [Máquinas virtuales de AVS](cloudsimple-virtual-machines.md)
* [Asignación de suscripciones de Azure](https://docs.azure.cloudsimple.com/azure-subscription-mapping/)
