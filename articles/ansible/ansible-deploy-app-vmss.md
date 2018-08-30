---
title: Implementación de aplicaciones en conjuntos de escalado de máquinas virtuales de Azure mediante Ansible
description: Aprenda a usar Ansible para configurar un conjunto de escalado de máquinas virtuales e implementar una aplicación en el conjunto de escalado de máquinas virtuales de Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, cuaderno de estrategias, máquina virtual, conjunto de escalado de máquinas virtuales, vmss
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/24/2018
ms.openlocfilehash: 762c14b5b6e30f6410a8d572d69651c803f079c2
ms.sourcegitcommit: ebb460ed4f1331feb56052ea84509c2d5e9bd65c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42918093"
---
# <a name="deploy-applications-to-virtual-machine-scale-sets-in-azure-using-ansible"></a>Implementación de aplicaciones en conjuntos de escalado de máquinas virtuales de Azure mediante Ansible
Ansible permite automatizar la implementación y la configuración de recursos en un entorno. Puede usar Ansible para implementar sus aplicaciones en Azure. En este artículo se muestra cómo implementar una aplicación Java en un conjunto de escalado de máquinas virtuales (VMSS) de Azure.  

## <a name="prerequisites"></a>Requisitos previos
- **Suscripción a Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- **Conjunto de escalado de máquinas virtuales**: si aún no tiene un conjunto de escalado de máquinas virtuales, puede [crear un conjunto de escalado de máquinas virtuales con Ansible](ansible-create-configure-vmss.md). 
- **git** - [git](https://git-scm.com) se usa para descargar un ejemplo de Java utilizado en este tutorial.
- **Java SE Development Kit (JDK)**: el JDK se usa para compilar el proyecto de Java de ejemplo.
- **Herramientas de compilación Maven de Apache**: las [herramientas de compilación Maven de Apache](https://maven.apache.org/download.cgi) se usan para compilar el proyecto de Java de ejemplo.

> [!Note]
> Ansible 2.6 es necesario para ejecutar los siguientes cuadernos de estrategias de ejemplo de este tutorial. 

## <a name="get-host-information"></a>Obtención de información sobre host

En esta sección se muestra cómo usar Ansible para recuperar información sobre host para un grupo de máquinas virtuales de Azure. A continuación se muestra un cuaderno de estrategias de Ansible de ejemplo. El código obtiene las direcciones IP públicas y el equilibrador de carga en el grupo de recursos especificado, y crea un grupo host llamado **saclesethosts** en el inventario. 

Guarde el siguiente cuaderno de estrategias de ejemplo como `get-hosts-tasks.yml`: 

  ```yaml
  - name: Get facts for all Public IPs within a resource groups
    azure_rm_publicipaddress_facts:
      resource_group: "{{ resource_group }}"
    register: output_ip_address

  - name: Get loadbalancer info
    azure_rm_loadbalancer_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ loadbalancer_name }}"
    register: output

  - name: Add all hosts
    add_host:
      groups: scalesethosts
      hostname: "{{ output_ip_address.ansible_facts.azure_publicipaddresses[0].properties.ipAddress }}_{{ item.properties.frontendPort }}"
      ansible_host: "{{ output_ip_address.ansible_facts.azure_publicipaddresses[0].properties.ipAddress }}"
      ansible_port: "{{ item.properties.frontendPort }}"
      ansible_ssh_user: "{{ admin_username }}"
      ansible_ssh_pass: "{{ admin_password }}"
    with_items:
      - "{{ output.ansible_facts.azure_loadbalancers[0].properties.inboundNatRules }}"
  ```

## <a name="prepare-an-application-for-deployment"></a>Preparación de una aplicación para la implementación  

En esta sección, usa git para clonar un proyecto de ejemplo de Java desde GitHub y compilar el proyecto. Guarde el siguiente cuaderno de estrategias como `app.yml`:

  ```yaml
  - hosts: localhost
    vars:
      repo_url: https://github.com/spring-guides/gs-spring-boot.git
      workspace: ~/src/helloworld

    tasks: 
    - name: Git Clone sample app
      git:
        repo: "{{ repo_url }}"
        dest: "{{ workspace }}"

    - name: Build sample app
      shell: mvn package chdir="{{ workspace }}/complete"
  ```

Ejecute el cuaderno de estrategias de Ansible de ejemplo con el siguiente comando:

  ```bash
  ansible-playbook app.yml
  ```

La salida del comando ansible-playbook es similar a la siguiente, donde verá que ha compilado la aplicación de ejemplo clonada desde GitHub:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Git Clone sample app] ***************************************************************************
  changed: [localhost]

  TASK [Build sample app] ***************************************************
  changed: [localhost]

  PLAY RECAP ***************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0

  ```

## <a name="deploy-the-application-to-vmss"></a>Implementación de la aplicación en VMSS

En la siguiente sección de un cuaderno de estrategias de Ansible, se instala el JRE (Java Runtime Environment) en un grupo host llamado **saclesethosts** y se implementa la aplicación Java en un grupo host llamado **saclesethosts**: 

(cambie `admin_password` por su propia contraseña).

  ```yaml
  - hosts: localhost
    vars:
      resource_group: myResourceGroup
      scaleset_name: myVMSS
      loadbalancer_name: myVMSSlb
      admin_username: azureuser
      admin_password: "your_password"
    tasks:   
    - include: get-hosts-tasks.yml

  - name: Install JRE on VMSS
    hosts: scalesethosts
    become: yes
    vars:
      workspace: ~/src/helloworld
      admin_username: azureuser

    tasks:
    - name: Install JRE
      apt:
        name: default-jre
        update_cache: yes

    - name: Copy app to Azure VM
      copy:
        src: "{{ workspace }}/complete/target/gs-spring-boot-0.1.0.jar"
        dest: "/home/{{ admin_username }}/helloworld.jar"
        force: yes
        mode: 0755

    - name: Start the application
      shell: java -jar "/home/{{ admin_username }}/helloworld.jar" >/dev/null 2>&1 &
      async: 5000
      poll: 0
  ```

Puede guardar el cuaderno de estrategias de Ansible de ejemplo anterior como `vmss-setup-deploy.yml`, o bien [descargar todo el cuaderno de estrategias de ejemplo](https://github.com/Azure-Samples/ansible-playbooks/blob/master/vmss). 

Para usar el tipo de conexión ssh con contraseñas, debe instalar el programa sshpass. 
  - En Ubuntu 16.04, ejecute el comando `apt-get install sshpass`.
  - En CentOS 7.4, ejecute el comando `yum install sshpass`.

Puede ver un error como **Using an SSH password instead of a key is not possible because Host Key checking is enabled and sshpass does not support this. Add this host's fingerprint to your known_hosts file to manage this host (El uso de una contraseña SSH en lugar de una clave no es posible, ya que la comprobación de clave de host está habilitada y sshpass no lo admite. Agregue la huella digital de este host a su archivo known_hosts para administrar este host).** Si ve este error, puede deshabilitar la comprobación de clave de host agregando la siguiente línea al archivo `/etc/ansible/ansible.cfg` o al archivo `~/.ansible.cfg`:
  ```bash
  [defaults]
  host_key_checking = False
  ```

Ejecute el cuaderno de estrategias con el siguiente comando:

  ```bash
  ansible-playbook vmss-setup-deploy.yml
  ```

El resultado de la ejecución del comando ansible-playbook indica que la aplicación Java de ejemplo se ha instalado en el grupo host del conjunto de escalado de máquinas virtuales:

  ```bash
  PLAY [localhost] **********************************************************

  TASK [Gathering Facts] ****************************************************
  ok: [localhost]

  TASK [Get facts for all Public IPs within a resource groups] **********************************************
  ok: [localhost]

  TASK [Get loadbalancer info] ****************************************************************************
  ok: [localhost]

  TASK [Add all hosts] *****************************************************************************
  changed: [localhost] ...

  PLAY [Install JRE on VMSS] *****************************************************************************

  TASK [Gathering Facts] *****************************************************************************
  ok: [40.114.30.145_50000]
  ok: [40.114.30.145_50003]

  TASK [Copy app to Azure VM] *****************************************************************************
  changed: [40.114.30.145_50003]
  changed: [40.114.30.145_50000]

  TASK [Start the application] ********************************************************************
  changed: [40.114.30.145_50000]
  changed: [40.114.30.145_50003]

  PLAY RECAP ************************************************************************************************
  40.114.30.145_50000        : ok=4    changed=3    unreachable=0    failed=0
  40.114.30.145_50003        : ok=4    changed=3    unreachable=0    failed=0
  localhost                  : ok=4    changed=1    unreachable=0    failed=0
  ```

Felicidades. Su aplicación se ejecuta en Azure ahora. Ahora puede ir a la dirección URL del equilibrador de carga para su conjunto de escalado de máquinas virtuales:

![Aplicación Java que se ejecuta en un conjunto de escalado de máquinas virtuales de Azure.](media/ansible-deploy-app-vmss/ansible-deploy-app-vmss.png)

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Cuaderno de estrategias de ejemplo de Ansible para VMSS](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss)