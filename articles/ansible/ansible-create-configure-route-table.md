---
title: Creación, modificación o eliminación de una tabla de rutas de Azure mediante Ansible
description: Aprenda a usar Ansible para crear, modificar o eliminar una tabla de rutas.
ms.service: azure
keywords: ansible, azure, devops, bash, cuaderno de estrategias, redes, ruta, tabla de rutas
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/17/2018
ms.openlocfilehash: 025a8182d32a7d0d00a48795c848d356eb1c3d4e
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792453"
---
# <a name="create-change-or-delete-an-azure-route-table-using-ansible"></a>Creación, modificación o eliminación de una tabla de rutas de Azure mediante Ansible
Azure enruta automáticamente el tráfico entre redes locales, las redes virtuales y las subredes de Azure. Si desea cambiar algún enrutamiento predeterminado en Azure, debe crear una [tabla de rutas](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

Ansible permite automatizar la implementación y la configuración de recursos en el entorno. Este artículo muestra cómo crear, modificar o eliminar una tabla de rutas de Azure y, también, cómo asociar la tabla de rutas a una subred. 

## <a name="prerequisites"></a>Requisitos previos
- **Suscripción a Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Se requiere Ansible 2.7 para ejecutar los siguientes cuadernos de estrategias de ejemplo en este tutorial.

## <a name="create-a-route-table"></a>Creación de una tabla de rutas
En esta sección se presenta un cuaderno de estrategias de Ansible de ejemplo que crea una tabla de rutas. Existe un límite para la cantidad de tablas de rutas que puede crear por suscripción y ubicación de Azure. Para más información, consulte el artículo acerca de los [límites de Azure](https://docs.microsoft.com/azure/azure-subscription-service-limits?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). 

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create a route table
      azure_rm_routetable:
        name: "{{ route_table_name }}"
        resource_group: "{{ resource_group }}"
```

Guarde este cuaderno de estrategias como `route_table_create.yml`. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook route_table_create.yml
```

## <a name="associate-a-route-table-to-a-subnet"></a>Asociación de una tabla de rutas a una subred
Una subred puede tener una tabla de ruta asociada a ella o ninguna. Una tabla de rutas se puede asociar a varias subredes o a ninguna. Como las tablas de rutas no se asocian a las redes virtuales, debe asociar una tabla de rutas a cada subred con la que desea asociarla. Todo el tráfico que sale de la subred se enruta según las rutas que se crearon dentro de las tablas de rutas, las [rutas predeterminadas](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#default) y las rutas propagadas desde una red local, si la red virtual está conectada a una puerta de enlace de red virtual de Azure (ExpressRoute, o VPN, si se usa BGP con una puerta de enlace de VPN). Solo puede asociar una tabla de rutas a las subredes de las redes virtuales que existen en la misma suscripción y ubicación de Azure de la tabla de rutas.

En esta sección se muestra un cuaderno de estrategias de Ansible de ejemplo que crea una red virtual y una subred y, posteriormente, asocia una tabla de rutas a la subred.

```yml
- hosts: localhost
  vars:
    subnet_name: mySubnet
    virtual_network_name: myVirtualNetwork 
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create virtual network
      azure_rm_virtualnetwork:
        name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
        - 10.1.0.0/16
        - 172.100.0.0/16
        dns_servers:
        - 127.0.0.1
        - 127.0.0.3
    - name: Create a subnet with route table
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: "10.1.0.0/24"
        route_table: "{ route_table_name }"
```

Guarde este cuaderno de estrategias como `route_table_associate.yml`. Para ejecutar el cuaderno de estrategias de Ansible, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook route_table_associate.yml
```

## <a name="dissociate-a-route-table-from-a-subnet"></a>Desasociación de una tabla de rutas de una subred
Cuando desasocia una tabla de rutas de una subred, basta con establecer la `route_table` de una subred en `None`. A continuación se muestra un cuaderno de estrategias de Ansible de ejemplo. 

```yml
- hosts: localhost
  vars:
    subnet_name: mySubnet
    virtual_network_name: myVirtualNetwork 
    resource_group: myResourceGroup
  tasks:
    - name: Dissociate a route table
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ virtual_network_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: "10.1.0.0/24"
```

Guarde este cuaderno de estrategias como `route_table_dissociate.yml`. Para ejecutar el cuaderno de estrategias de Ansible, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook route_table_dissociate.yml
```

## <a name="create-a-route"></a>Creación de una ruta
En esta sección se presenta un cuaderno de estrategias de Ansible de ejemplo que crea una ruta en la tabla de rutas. Se define `virtual_network_gateway` como `next_hop_type` y `10.1.0.0/16` como `address_prefix`. El prefijo no se puede duplicar en más de una ruta dentro de la tabla de rutas, aunque puede estar dentro de otro prefijo. Para más información sobre cómo Azure selecciona rutas y una descripción detallada de todos los tipos de próximo salto, consulte [Introducción al enrutamiento](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview).

```yml
- hosts: localhost
  vars:
    route_name: myRoute
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create route
      azure_rm_route:
        name: "{{ route_name }}"
        resource_group: "{{ resource_group }}"
        next_hop_type: virtual_network_gateway
        address_prefix: "10.1.0.0/16"
        route_table_name: "{{ route_table_name }}"
```
Guarde este cuaderno de estrategias como `route_create.yml`. Para ejecutar el cuaderno de estrategias de Ansible, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook route_create.yml
```

## <a name="delete-a-route"></a>Eliminación de una ruta
En esta sección se presenta un cuaderno de estrategias de Ansible de ejemplo que elimina una ruta de una tabla de rutas.

```yml
- hosts: localhost
  vars:
    route_name: myRoute
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Remove route
      azure_rm_route:
        name: "{{ route_name }}"
        resource_group: "{{ resource_group }}"
        route_table_name: "{{ route_table_name }}"
        state: absent
```

Guarde este cuaderno de estrategias como `route_delete.yml`. Para ejecutar el cuaderno de estrategias de Ansible, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook route_delete.yml
```

## <a name="get-information-of-a-route-table"></a>Obtención de información de una tabla de rutas
Puede ver los detalles de una route_table mediante el módulo de Ansible denominado `azure_rm_routetable_facts`. El módulo de hechos devolverá la información de la tabla de rutas con todas las rutas asociadas a ella.
A continuación se muestra un cuaderno de estrategias de Ansible de ejemplo. 

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks: 
    - name: Get route table information
      azure_rm_routetable_facts:
         resource_group: "{{ resource_group }}"
         name: "{{ route_table_name }}"
      register: query
    
    - debug:
         var: query.route_tables[0]
```

Guarde este cuaderno de estrategias como `route_table_facts.yml`. Para ejecutar el cuaderno de estrategias de Ansible, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook route_table_facts.yml
```

## <a name="delete-a-route-table"></a>Eliminación de una tabla de rutas
Una tabla de rutas no se puede eliminar si está asociada a alguna subred. [Desasocie](#dissociate-a-route-table-from-a-subnet) una tabla de rutas de todas las subredes antes de intentar eliminarla.

Puede eliminar la tabla de rutas junto con todas las rutas. A continuación se muestra un cuaderno de estrategias de Ansible de ejemplo. 

```yml
- hosts: localhost
  vars:
    route_table_name: myRouteTable
    resource_group: myResourceGroup
  tasks:
    - name: Create a route table
      azure_rm_routetable:
        name: "{{ route_table_name }}"
        resource_group: "{{ resource_group }}"
        state: absent
```

Guarde este cuaderno de estrategias como `route_table_delete.yml`. Para ejecutar el cuaderno de estrategias de Ansible, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook route_table_delete.yml
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Ansible en Azure](https://docs.microsoft.com/azure/ansible/)
