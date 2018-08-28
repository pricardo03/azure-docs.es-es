---
title: Uso de Ansible para crear una máquina virtual Linux en Azure
description: Obtenga información sobre cómo usar Ansible para crear una máquina virtual Linux en Azure
ms.service: ansible
keywords: ansible, azure, devops, máquina virtual
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/21/2018
ms.openlocfilehash: a60ba863dbbd308219f4229319fb98c72180114d
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "40250660"
---
# <a name="use-ansible-to-create-a-linux-virtual-machine-in-azure"></a>Uso de Ansible para crear una máquina virtual Linux en Azure
Mediante un lenguaje declarativo, Ansible permite automatizar la creación, configuración e implementación de recursos de Azure con los *cuadernos* de Ansible. Cada sección de este artículo muestra cómo podría ser cada sección de un cuaderno de Ansible para crear y configurar diferentes aspectos de una máquina virtual Linux. El [cuaderno de Ansible completo](#complete-sample-ansible-playbook) aparece al final de este artículo.

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción de Azure**: si no tiene una suscripción de Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- **Configurar Azure Cloud Shell** o **instalar y configurar Ansible en una máquina virtual Linux**

  **Configuración de Azure Cloud Shell**

  1. **Configuración de Azure Cloud Shell**: si es la primera vez que usa Azure Cloud Shell, el artículo [Guía de inicio rápido para Bash en Azure Cloud Shell](/azure/cloud-shell/quickstart) muestra cómo iniciar y configurar Cloud Shell. 

  **O bien,**

  **Instalar y configurar Ansible en una máquina virtual Linux**

  1. **Instalación de Ansible**: instale Ansible en una [plataforma Linux compatible](/azure/virtual-machines/linux/ansible-install-configure#install-ansible-on-an-azure-linux-virtual-machine).

  1. **Configuración de Ansible** - [Creación de credenciales de Azure y configuración de Ansible](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="create-a-resource-group"></a>Crear un grupo de recursos
Ansible necesita un grupo de recursos en el que implementará todos los recursos. En la siguiente sección, el cuaderno de Ansible de ejemplo crea un grupo de recursos llamado `myResourceGroup` en la ubicación `eastus`:

```yaml
- name: Create resource group
    azure_rm_resourcegroup:
      name: myResourceGroup
      location: eastus
```

## <a name="create-a-virtual-network"></a>Creación de una red virtual
Cuando se crea una máquina virtual de Azure, es preciso crear una [red virtual](/azure/virtual-network/virtual-networks-overview) o usar una red virtual existente. También es preciso decidir la forma en que pretende que se acceda a las máquinas virtuales de la red virtual. En la siguiente sección, el cuaderno de Ansible crea una red virtual llamada `myVnet` en el espacio de direcciones `10.0.0.0/16`:

```yaml
- name: Create virtual network
  azure_rm_virtualnetwork:
    resource_group: myResourceGroup
    name: myVnet
    address_prefixes: "10.0.0.0/16"
```

Todos los recursos de Azure implementados en una red virtual se implementan en una [subred](/azure/virtual-network/virtual-network-manage-subnet) de esa red virtual. 

En la siguiente sección, el cuaderno de Ansible de ejemplo crea una subred llamada `mySubnet` en la red virtual `myVnet`:

```yaml
- name: Add subnet
  azure_rm_subnet:
    resource_group: myResourceGroup
    name: mySubnet
    address_prefix: "10.0.1.0/24"
    virtual_network: myVnet
```

## <a name="create-a-public-ip-address"></a>Crear una dirección IP pública
Las [direcciones IP públicas](/azure/virtual-network/virtual-network-ip-addresses-overview-arm) permiten a los recursos de Internet la comunicación entrante a los recursos de Azure. También habilitan los recursos de Azure para la comunicación saliente a Internet y los servicios de Azure orientados al público con una dirección IP asignada al recurso. Hasta que cancele la asignación, la dirección estará dedicada al recurso. Si una dirección IP pública no se asigna a un recurso, este puede seguir comunicándose hacia Internet, pero Azure asigna dinámicamente una dirección IP disponible que no esté dedicada a los recursos. 

En la siguiente sección, el cuaderno de Ansible de ejemplo crea una dirección IP pública llamada `myPublicIP`:

```yaml
- name: Create public IP address
  azure_rm_publicipaddress:
    resource_group: myResourceGroup
    allocation_method: Static
    name: myPublicIP
```

## <a name="create-a-network-security-group"></a>Crear un grupo de seguridad de red
Un [grupo de seguridad de red](/azure/virtual-network/security-overview) le permite filtrar el tráfico de red hacia y desde los recursos de Azure de una red virtual de Azure. Un grupo de seguridad de red contiene reglas de seguridad que permiten o deniegan el tráfico de red entrante o el tráfico de red saliente de varios tipos de recursos de Azure. 

En la siguiente sección, el cuaderno de Ansible de ejemplo crea un grupo de seguridad de red llamado `myNetworkSecurityGroup` y define una regla para permitir el tráfico de SSH por el puerto TCP 22:

```yaml
- name: Create Network Security Group that allows SSH
  azure_rm_securitygroup:
    resource_group: myResourceGroup
    name: myNetworkSecurityGroup
    rules:
      - name: SSH
        protocol: Tcp
        destination_port_range: 22
        access: Allow
        priority: 1001
        direction: Inbound
```


## <a name="create-a-virtual-network-interface-card"></a>Creación de una tarjeta de interfaz de red virtual
Una tarjeta de interfaz de red virtual conecta la máquina virtual a una red virtual determinada, una dirección IP pública y un grupo de seguridad de red. 

En la siguiente sección, el cuaderno de Ansible de ejemplo crea una tarjeta de interfaz de red virtual llamada `myNIC` conectada a los recursos de red virtual que ha creado:

```yaml
- name: Create virtual network inteface card
  azure_rm_networkinterface:
    resource_group: myResourceGroup
    name: myNIC
    virtual_network: myVnet
    subnet: mySubnet
    public_ip_name: myPublicIP
    security_group: myNetworkSecurityGroup
```

## <a name="create-a-virtual-machine"></a>de una máquina virtual
El último paso es crear una máquina virtual que usa todos los recursos que ha creado en las secciones anteriores de este artículo. 

El cuaderno de Ansible de ejemplo presentado en esta sección crea una máquina virtual llamada `myVM` y conecta la tarjeta de interfaz de red virtual llamada `myNIC`. Reemplace el marcador de posición &lt;datos-de-clave> por sus propios datos de clave pública completos.

```yaml
- name: Create VM
  azure_rm_virtualmachine:
    resource_group: myResourceGroup
    name: myVM
    vm_size: Standard_DS1_v2
    admin_username: azureuser
    ssh_password_enabled: false
    ssh_public_keys:
      - path: /home/azureuser/.ssh/authorized_keys
        key_data: <your-key-data>
    network_interfaces: myNIC
    image:
      offer: CentOS
      publisher: OpenLogic
      sku: '7.5'
      version: latest
```

## <a name="complete-sample-ansible-playbook"></a>Cuaderno de Ansible de ejemplo completo

Esta sección enumera el cuaderno de Ansible de ejemplo completo que creó en el transcurso de este artículo. 

```yaml
- name: Create Azure VM
  hosts: localhost
  connection: local
  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
      name: myResourceGroup
      location: eastus
  - name: Create virtual network
    azure_rm_virtualnetwork:
      resource_group: myResourceGroup
      name: myVnet
      address_prefixes: "10.0.0.0/16"
  - name: Add subnet
    azure_rm_subnet:
      resource_group: myResourceGroup
      name: mySubnet
      address_prefix: "10.0.1.0/24"
      virtual_network: myVnet
  - name: Create public IP address
    azure_rm_publicipaddress:
      resource_group: myResourceGroup
      allocation_method: Static
      name: myPublicIP
    register: output_ip_address
  - name: Dump public IP for VM which will be created
    debug:
      msg: "The public IP is {{ output_ip_address.state.ip_address }}."
  - name: Create Network Security Group that allows SSH
    azure_rm_securitygroup:
      resource_group: myResourceGroup
      name: myNetworkSecurityGroup
      rules:
        - name: SSH
          protocol: Tcp
          destination_port_range: 22
          access: Allow
          priority: 1001
          direction: Inbound
  - name: Create virtual network inteface card
    azure_rm_networkinterface:
      resource_group: myResourceGroup
      name: myNIC
      virtual_network: myVnet
      subnet: mySubnet
      public_ip_name: myPublicIP
      security_group: myNetworkSecurityGroup
  - name: Create VM
    azure_rm_virtualmachine:
      resource_group: myResourceGroup
      name: myVM
      vm_size: Standard_DS1_v2
      admin_username: azureuser
      ssh_password_enabled: false
      ssh_public_keys:
        - path: /home/azureuser/.ssh/authorized_keys
          key_data: <your-key-data>
      network_interfaces: myNIC
      image:
        offer: CentOS
        publisher: OpenLogic
        sku: '7.5'
        version: latest
```

## <a name="run-the-sample-ansible-playbook"></a>Ejecución del cuaderno de Ansible de ejemplo

Esta sección le guía en la ejecución del cuaderno de Ansible de ejemplo presentada en este artículo.

1. Inicie sesión en el [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Abra [Cloud Shell](/azure/cloud-shell/overview).

1. Cree un archivo (para que contenga el cuaderno) llamado `azure_create_complete_vm.yml` y ábralo en el editor VI como se indica a continuación:

  ```azurecli-interactive
  vi azure_create_complete_vm.yml
  ```

1. Seleccione la tecla **I** para acceder al modo de inserción.

1. Pegue el [cuaderno de Ansible de ejemplo completo](#complete-sample-ansible-playbook) en el editor.

1. Seleccione la tecla **Esc** para acceder al modo de inserción.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

1. Ejecute el cuaderno de Ansible de ejemplo.

  ```bash
  ansible-playbook azure_create_complete_vm.yml
  ```

1. La salida es similar a la siguiente, en la que se muestra que la máquina virtual se ha creado correctamente:

  ```bash
  PLAY [Create Azure VM] ****************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Create resource group] *********************************************
  changed: [localhost]

  TASK [Create virtual network] *********************************************
  changed: [localhost]

  TASK [Add subnet] *********************************************************
  changed: [localhost]

  TASK [Create public IP address] *******************************************
  changed: [localhost]

  TASK [Dump public IP for VM which will be created] ********************************************************************
  ok: [localhost] => {
      "msg": "The public IP is <ip-address>."
  }

  TASK [Create Network Security Group that allows SSH] **********************
  changed: [localhost]

  TASK [Create virtual network inteface card] *******************************
  changed: [localhost]

  TASK [Create VM] **********************************************************
  changed: [localhost]

  PLAY RECAP ****************************************************************
  localhost                  : ok=8    changed=7    unreachable=0    failed=0
  ```

1. El comando SSH se usa para acceder a la máquina virtual Linux. Reemplace el marcador de posición &lt;dirección-ip> por la dirección IP del paso anterior.

  ```bash
  ssh azureuser@<ip-address>
  ```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Uso de Ansible para administrar una máquina virtual Linux en Azure](./ansible-manage-linux-vm.md)