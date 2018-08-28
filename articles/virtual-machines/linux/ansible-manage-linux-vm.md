---
title: Uso de Ansible para administrar una máquina virtual Linux en Azure
description: Obtenga información sobre cómo usar Ansible para administrar una máquina virtual Linux en Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 08/21/2018
ms.openlocfilehash: 66106346b298fae22cce47081916a6c8eec8fd40
ms.sourcegitcommit: 76797c962fa04d8af9a7b9153eaa042cf74b2699
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/21/2018
ms.locfileid: "40250676"
---
# <a name="use-ansible-to-manage-a-linux-virtual-machine-in-azure"></a>Uso de Ansible para administrar una máquina virtual Linux en Azure
Ansible permite automatizar la implementación y la configuración de recursos en un entorno. Puede usar Ansible para administrar máquinas virtuales de Azure al igual que podría hacerlo con cualquier otro recurso. Este artículo muestra cómo utilizar un cuaderno de Ansible para iniciar y detener una máquina virtual Linux. 

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción de Azure**: si no tiene una suscripción de Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- **Configurar Azure Cloud Shell** o **instalar y configurar Ansible en una máquina virtual Linux**

  **Configuración de Azure Cloud Shell**

  1. **Configuración de Azure Cloud Shell**: si es la primera vez que usa Azure Cloud Shell, el artículo [Guía de inicio rápido para Bash en Azure Cloud Shell](/azure/cloud-shell/quickstart) muestra cómo iniciar y configurar Cloud Shell. 

  1. **Máquina virtual Linux**: si no tiene acceso a una máquina virtual Linux, puede [crear una máquina virtual con Ansible](ansible-create-vm.md).

  **O bien,**

  **Instalar y configurar Ansible en una máquina virtual Linux**

  1. **Instalación de Ansible**: instale Ansible en una [plataforma Linux compatible](/azure/virtual-machines/linux/ansible-install-configure#install-ansible-on-an-azure-linux-virtual-machine).

  1. **Configuración de Ansible** - [Creación de credenciales de Azure y configuración de Ansible](/azure/virtual-machines/linux/ansible-install-configure#create-azure-credentials)

## <a name="use-ansible-to-deallocate-stop-an-azure-virtual-machine"></a>Uso de Ansible para desasignar (detener) una máquina virtual de Azure
Esta sección muestra cómo usar Ansible para desasignar (detener) una máquina virtual de Azure

1. Inicie sesión en el [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Abra [Cloud Shell](/azure/cloud-shell/overview).

1. Cree un archivo (para que contenga el cuaderno) llamado `azure_vm_stop.yml` y ábralo en el editor VI como se indica a continuación:

  ```azurecli-interactive
  vi azure_vm_stop.yml
  ```

1. Seleccione la tecla **I** para acceder al modo de inserción.

1. Pegue el siguiente código de ejemplo en el editor:

    ```yaml
    - name: Stop Azure VM
    hosts: localhost
    connection: local
    tasks:
    - name: Deallocate the virtual machine
        azure_rm_virtualmachine:
            resource_group: myResourceGroup
            name: myVM
            allocated: no 
    ```

1. Seleccione la tecla **Esc** para acceder al modo de inserción.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

1. Ejecute el cuaderno de Ansible de ejemplo.

  ```bash
  ansible-playbook azure_vm_stop.yml
  ```

1. La salida es similar a la del siguiente ejemplo, en el que se muestra que la máquina virtual se ha desasignado (detenido) correctamente:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Deallocate the Virtual Machine] ***************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="use-ansible-to-start-a-deallocated-stopped-azure-virtual-machine"></a>Uso de Ansible para iniciar una máquina virtual de Azure desasignada (detenida)
Esta sección muestra cómo usar Ansible para iniciar una máquina virtual de Azure desasignada (detenida)

1. Inicie sesión en el [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Abra [Cloud Shell](/azure/cloud-shell/overview).

1. Cree un archivo (para que contenga el cuaderno) llamado `azure_vm_start.yml` y ábralo en el editor VI como se indica a continuación:

  ```azurecli-interactive
  vi azure_vm_start.yml
  ```

1. Seleccione la tecla **I** para acceder al modo de inserción.

1. Pegue el siguiente código de ejemplo en el editor:

    ```yaml
    - name: Start Azure VM
    hosts: localhost
    connection: local
    tasks:
    - name: Start the virtual machine
        azure_rm_virtualmachine:
            resource_group: myResourceGroup
            name: myVM
    ```

1. Seleccione la tecla **Esc** para acceder al modo de inserción.

1. Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

1. Ejecute el cuaderno de Ansible de ejemplo.

  ```bash
  ansible-playbook azure_vm_start.yml
  ```

1. La salida es similar a la del siguiente ejemplo, que muestra que la máquina virtual se ha iniciado correctamente:

    La salida es similar a la del siguiente ejemplo, que muestra que la máquina virtual se ha iniciado correctamente:

    ```bash
    PLAY [Stop Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Uso de Ansible para administrar los inventarios dinámicos de Azure](../../ansible/ansible-manage-azure-dynamic-inventories.md)