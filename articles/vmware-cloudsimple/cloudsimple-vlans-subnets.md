---
title: Las VLAN y subredes de la solución VMware CloudSimple - Azure
description: Obtenga información acerca de las VLAN y subredes en una nube privada CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9d29445054848d798476fed8184b89f9b6c1210f
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66497577"
---
# <a name="vlans-and-subnets-overview"></a>Información general de las VLAN y subredes

CloudSimple proporciona una red por región donde se implementa el servicio CloudSimple.  La red es un único espacio de dirección TCP capa 3 con el enrutamiento habilitado de forma predeterminada.  Todas las nubes privadas y las subredes creadas en esta región pueden comunicarse entre sí sin ninguna configuración adicional.  Puede crear grupos de puertos distribuido en el servidor de vCenter con las VLAN.

![Topología de red CloudSimple](media/cloudsimple-network-topology.png)

## <a name="vlans"></a>VLAN

VLAN (red de capa 2) se han creado por la nube privada.  El tráfico de capa 2 permanece dentro del límite de una nube privada, lo que le permite aislar el tráfico dentro de la nube privada local.  Una VLAN que creó en la nube privada puede usarse para crear grupos de puertos distribuidas solo en esa nube privada.  Una VLAN que creó en una nube privada se configura automáticamente en todos los conmutadores conectados a los hosts de una nube privada.

## <a name="subnets"></a>Subredes

Puede crear una subred al crear una VLAN, definiendo el espacio de direcciones de la subred. Se asigna una dirección IP del espacio de direcciones como una subred de puerta de enlace. Se le asigna un único espacio de direcciones privadas de nivel 3 por región y el cliente. Puede configurar cualquier espacio de direcciones no superpuestos de RFC 1918, con su red local o una red virtual de Azure, en la región de la red.

Todas las subredes pueden comunicarse entre sí de forma predeterminada, lo que reduce la configuración de la sobrecarga para el enrutamiento entre las nubes privadas. Este-oeste datos a través de los equipos en la misma región se queda en la misma red de capa 3 y transfiere a través de la infraestructura de red local dentro de la región. Salida no es necesaria para la comunicación entre las nubes privadas en una región. Este enfoque elimina cualquier reducción del rendimiento de WAN/salida en la implementación de cargas de trabajo diferentes en las nubes privadas.

## <a name="vspherevsan-subnets-cidr-range"></a>intervalo CIDR de vSphere/vSAN subredes

Se crea una nube privada como una pila VMware aislada (hosts ESXi, vCenter, vSAN y NSX) entorno administrado por un servidor vCenter.  Los componentes de administración se implementan en la red seleccionada para **vSphere/vSAN subredes CIDR**.  El intervalo de CIDR de red se divide en subredes diferentes durante la implementación.

Prefijo de intervalo de subredes de vSphere/vSAN mínimas CIDR: **/24** prefijo de intervalo de subredes de vSphere/vSAN máximo CIDR:   **/21**

> [!CAUTION]
> Las direcciones IP en el intervalo CIDR de vSphere/vSAN están reservadas para su uso por la infraestructura de nube privada. No use una dirección IP de este intervalo en cualquier máquina virtual.


### <a name="vspherevsan-subnets-cidr-range-limits"></a>límites de intervalo de subredes de vSphere/vSAN CIDR

Seleccionar el tamaño del intervalo de subredes de vSphere/vSAN CIDR tiene un impacto en el tamaño de su propia nube privada.  Tabla siguiente muestra el número máximo de nodos que puede tener en función del tamaño de las subredes de vSphere/vSAN CIDR.

| Longitud del prefijo especificadas vSphere/vSAN subredes CIDR | Número máximo de nodos |
|---------------------------------------------------|-------------------------|
| /24 | 26 |
| /23 | 58 |
| /22 | 118 |
| /21 | 220 |

### <a name="management-subnets-created-on-a-private-cloud"></a>Subredes de administración creado en una nube privada

Subredes de administración siguientes se crean al crear una nube privada. 

* **Administración del sistema** -VLAN y subred para la administración de los hosts ESXi de red, servidor DNS, servidor vCenter.
* **VMotion** -VLAN y subred de red de los hosts ESXi vMotion.
* **VSAN** -VLAN y subred de red de los hosts ESXi vSAN.
* **NsxtEdgeUplink1** -VLAN y subred para vínculos superiores VLAN a una red externa.
* **NsxtEdgeUplink2** -VLAN y subred para vínculos superiores VLAN a una red externa.
* **NsxtEdgeTransport** -VLAN y subred para las zonas de transporte controlan el alcance de las redes de capa 2 en NSX-T.
* **NsxtHostTransport** -VLAN y subred para el host de zona de transporte.

### <a name="management-network-cidr-range-breakdown"></a>Desglose de intervalo CIDR de red de administración

intervalo CIDR de subredes de vSphere/vSAN especificado se divide en varias subredes.  Tabla siguiente muestra un ejemplo del desglose de prefijos permitidos.  El ejemplo se utiliza **192.168.0.0** como el intervalo CIDR.

Ejemplo:

| Especifica las subredes de vSphere/vSAN/prefijo CIDR | 192.168.0.0/21 | 192.168.0.0/22 | 192.168.0.0/23 | 192.168.0.0/24 |
|---------------------------------|----------------|----------------|----------------|----------------|
| Administración del sistema | 192.168.0.0/24 | 192.168.0.0/24 | 192.168.0.0/25 | 192.168.0.0/26 |
| vMotion | 192.168.1.0/24 | 192.168.1.0/25 | 192.168.0.128/26 | 192.168.0.64/27 |
| vSAN | 192.168.2.0/24 | 192.168.1.128/25 | 192.168.0.192/26 | 192.168.0.96/27 |
| Transporte de Host NSX-T | 192.168.4.0/23 | 192.168.2.0/24 | 192.168.1.0/25 | 192.168.0.128/26 |
| Transporte perimetral de NSX-T | 192.168.7.208/28 | 192.168.3.208/28 | 192.168.1.208/28 | 192.168.0.208/28 |
| -T NSX Edge Uplink1 | 192.168.7.224/28 | 192.168.3.224/28 | 192.168.1.224/28 | 192.168.0.224/28 |
| -T NSX Edge uplink2 | 192.168.7.240/28 | 192.168.3.240/28 | 192.168.1.240/28 | 192.168.0.240/28 |

## <a name="next-steps"></a>Pasos siguientes

* [Crear y administrar redes VLAN y subredes](https://docs.azure.cloudsimple.com/create-vlan-subnet/)
