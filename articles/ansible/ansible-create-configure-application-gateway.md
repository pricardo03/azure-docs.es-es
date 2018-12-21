---
title: Administración del tráfico web con Azure Application Gateway mediante Ansible
description: Aprenda a usar Ansible para crear y configurar una instancia de Azure Application Gateway para administrar el tráfico web
ms.service: ansible
keywords: ansible, azure, devops, bash, cuaderno de estrategias, azure application gateway, equilibrador de carga, tráfico web
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: af7f22ae5c289a01e6876d8ce586cb32383c8d3b
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53253378"
---
# <a name="manage-web-traffic-with-azure-application-gateway-by-using-ansible"></a>Administración del tráfico web con Azure Application Gateway mediante Ansible

[Azure Application Gateway](https://docs.microsoft.com/azure/application-gateway/) es un equilibrador de carga de tráfico web que permite administrar el tráfico a las aplicaciones web.

Ansible le ayuda a automatizar la implementación y la configuración de recursos en su entorno. En este artículo se muestra cómo usar Ansible para crear una puerta de enlace de aplicación. También le enseña cómo usar la puerta de enlace para administrar el tráfico a dos servidores web que se ejecutan en instancias de contenedor de Azure.

En este tutorial se muestra cómo realizar las siguientes acciones:

> [!div class="checklist"]
> * Configuración de la red
> * Creación de dos instancias de contenedor de Azure con imágenes HTTPD
> * Creación de una puerta de enlace de aplicación que funciona con las instancias de contenedor de Azure del clúster de servidores

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción a Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Se requiere Ansible 2.7 para ejecutar los siguientes cuadernos de estrategias de ejemplo en este tutorial. 

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos es un contenedor lógico en el que se implementan y se administran los recursos de Azure.  

En el ejemplo siguiente, se crea un grupo de recursos denominado **myResourceGroup** en la ubicación **eastus**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
```

Guarde este cuaderno de estrategias como *rg.yml*. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook rg.yml
```

## <a name="create-network-resources"></a>Crear recursos de red

Cree en primer lugar una red virtual para que la puerta de enlace de aplicaciones pueda comunicarse con otros recursos.

En el ejemplo siguiente se crea una red virtual llamada **myVNet**, una subred llamada **myAGSubnet** y una dirección IP pública llamada **myAGPublicIPAddress** con un dominio denominado **mydomain**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    vnet_name: myVNet 
    subnet_name: myAGSubnet 
    publicip_name: myAGPublicIPAddress
    publicip_domain: mydomain
  tasks:
    - name: Create a virtual network
      azure_rm_virtualnetwork:
        name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefixes_cidr:
            - 10.1.0.0/16
            - 172.100.0.0/16
        dns_servers:
            - 127.0.0.1
            - 127.0.0.2

    - name: Create a subnet
      azure_rm_subnet:
        name: "{{ subnet_name }}"
        virtual_network_name: "{{ vnet_name }}"
        resource_group: "{{ resource_group }}"
        address_prefix_cidr: 10.1.0.0/24

    - name: Create a public IP address
      azure_rm_publicipaddress:
        resource_group: "{{ resource_group }}" 
        allocation_method: Dynamic
        name: "{{ publicip_name }}"
        domain_name_label: "{{ publicip_domain }}"
```

Guarde este cuaderno de estrategias como *vnet_create.yml*. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook vnet_create.yml
```

## <a name="create-servers"></a>Creación de servidores

En el siguiente ejemplo se muestra cómo crear dos instancias de contenedores de Azure con imágenes HTTPD para usarlos como servidores web para la puerta de enlace de aplicación.  

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    location: eastus 
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Create a container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_1_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80

    - name: Create another container with httpd image 
      azure_rm_containerinstance:
        resource_group: "{{ resource_group }}"
        name: "{{ aci_2_name }}"
        os_type: linux
        ip_address: public
        location: "{{ location }}"
        ports:
          - 80
        containers:
          - name: mycontainer
            image: httpd
            memory: 1.5
            ports:
              - 80
```

Guarde este cuaderno de estrategias como *aci_create.yml*. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook aci_create.yml
```

## <a name="create-the-application-gateway"></a>Creación de la puerta de enlace de aplicaciones

En el ejemplo siguiente se crea una puerta de enlace de aplicaciones denominada **myAppGateway** con configuraciones para http, front-end y back-end.  

* **appGatewayIP** se define en el bloque **gateway_ip_configurations**. Se necesita una referencia de subred para la configuración IP de la puerta de enlace.
* **appGatewayBackendPool** se define en el bloque **backend_address_pools**. Una puerta de enlace de aplicaciones debe tener al menos un grupo de direcciones de servidores back-end.
* **appGatewayBackendHttpSettings** se define en el bloque **backend_http_settings_collection**. Especifica que se use el puerto 80 y un protocolo HTTP para la comunicación.
* **appGatewayHttpListener** se define en el bloque **backend_http_settings_collection**. Agente de escucha predeterminado asociado con appGatewayBackendPool.
* **appGatewayFrontendIP** se define en el bloque **frontend_ip_configurations**. Asigna myAGPublicIPAddress a appGatewayHttpListener.
* **rule1** se define en el bloque **request_routing_rules**. La regla de enrutamiento predeterminada asociada a appGatewayHttpListener.

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    vnet_name: myVNet
    subnet_name: myAGSubnet
    location: eastus
    publicip_name: myAGPublicIPAddress
    appgw_name: myAppGateway
    aci_1_name: myACI1
    aci_2_name: myACI2
  tasks:
    - name: Get info of Subnet
      azure_rm_resource_facts:
        api_version: '2018-08-01'
        resource_group: "{{ resource_group }}"
        provider: network
        resource_type: virtualnetworks
        resource_name: "{{ vnet_name }}"
        subresource:
          - type: subnets
            name: "{{ subnet_name }}"
      register: subnet

    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_1_name }}"
      register: aci_1_output
    - name: Get info of backend server 2
      azure_rm_resource_facts:
        api_version: '2018-04-01'
        resource_group: "{{ resource_group }}"
        provider: containerinstance
        resource_type: containergroups
        resource_name: "{{ aci_2_name }}"
      register: aci_2_output

    - name: Create instance of Application Gateway
      azure_rm_appgateway:
        resource_group: "{{ resource_group }}"
        name: "{{ appgw_name }}"
        sku:
          name: standard_small
          tier: standard
          capacity: 2
        gateway_ip_configurations:
          - subnet:
              id: "{{ subnet.response[0].id }}"
            name: appGatewayIP
        frontend_ip_configurations:
          - public_ip_address: "{{ publicip_name }}"
            name: appGatewayFrontendIP
        frontend_ports:
          - port: 80
            name: appGatewayFrontendPort
        backend_address_pools:
          - backend_addresses:
              - ip_address: "{{ aci_1_output.response[0].properties.ipAddress.ip }}"
              - ip_address: "{{ aci_2_output.response[0].properties.ipAddress.ip }}"
            name: appGatewayBackendPool
        backend_http_settings_collection:
          - port: 80
            protocol: http
            cookie_based_affinity: enabled
            name: appGatewayBackendHttpSettings
        http_listeners:
          - frontend_ip_configuration: appGatewayFrontendIP
            frontend_port: appGatewayFrontendPort
            name: appGatewayHttpListener
        request_routing_rules:
          - rule_type: Basic
            backend_address_pool: appGatewayBackendPool
            backend_http_settings: appGatewayBackendHttpSettings
            http_listener: appGatewayHttpListener
            name: rule1
```

Guarde este cuaderno de estrategias como *appgw_create.yml*. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook appgw_create.yml
```

La puerta de enlace de aplicaciones puede tardar varios minutos en crearse.

## <a name="test-the-application-gateway"></a>Prueba de la puerta de enlace de aplicaciones

En el cuaderno de estrategias de ejemplo para los recursos de red, el dominio llamado **mydomain** se creó en **eastus**. Vaya a `http://mydomain.eastus.cloudapp.azure.com` en el explorador. Si ve la página siguiente significa que la puerta de enlace de aplicación funciona según lo previsto.

![Prueba correcta de una puerta de enlace de aplicación que funciona](media/ansible-create-configure-application-gateway/applicationgateway.PNG)

## <a name="clean-up-resources"></a>Limpieza de recursos

Si no necesita estos recursos, puede eliminarlos mediante la ejecución del código siguiente. Este elimina un grupo de recursos denominado **myResourceGroup**.

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
  tasks:
    - name: Delete a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        state: absent
```

Guarde este cuaderno de estrategias como *rg_delete*.yml. Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook rg_delete.yml
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Ansible en Azure](https://docs.microsoft.com/azure/ansible/)
