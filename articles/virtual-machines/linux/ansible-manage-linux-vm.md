---
title: Uso de Ansible para administrar una máquina virtual Linux en Azure
description: Obtenga información sobre cómo usar Ansible para administrar una máquina virtual Linux en Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, playbook, bash
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: quickstart
ms.date: 09/27/2018
ms.openlocfilehash: 06a0f2416ecff9e2f9a21877e26487b56452d0a0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251328"
---
# <a name="use-ansible-to-manage-a-linux-virtual-machine-in-azure"></a>Uso de Ansible para administrar una máquina virtual Linux en Azure
Ansible permite automatizar la implementación y la configuración de recursos en un entorno. Puede usar Ansible para administrar máquinas virtuales de Azure al igual que podría hacerlo con cualquier otro recurso. Este artículo muestra cómo utilizar un cuaderno de Ansible para iniciar y detener una máquina virtual Linux. 

## <a name="prerequisites"></a>Requisitos previos

- **Suscripción de Azure**: si no tiene una suscripción de Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).

- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

## <a name="use-ansible-to-deallocate-stop-an-azure-virtual-machine"></a>Uso de Ansible para desasignar (detener) una máquina virtual de Azure
Esta sección muestra cómo usar Ansible para desasignar (detener) una máquina virtual de Azure

1.  Inicie sesión en el [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1.  Abra [Cloud Shell](/azure/cloud-shell/overview).

1.  Cree un archivo (para que contenga el cuaderno) llamado `azure-vm-stop.yml` y ábralo en el editor VI como se indica a continuación:

    ```azurecli-interactive
    vi azure-vm-stop.yml
    ```

1.  Seleccione la tecla **I** para acceder al modo de inserción.

1.  Pegue el siguiente código de ejemplo en el editor:

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

1.  Seleccione la tecla **Esc** para acceder al modo de inserción.

1.  Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

1.  Ejecute el cuaderno de Ansible de ejemplo.

    ```bash
    ansible-playbook azure-vm-stop.yml
    ```

1.  La salida es similar a la del siguiente ejemplo, en el que se muestra que la máquina virtual se ha desasignado (detenido) correctamente:

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

1.  Inicie sesión en el [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1.  Abra [Cloud Shell](/azure/cloud-shell/overview).

1.  Cree un archivo (para que contenga el cuaderno) llamado `azure-vm-start.yml` y ábralo en el editor VI como se indica a continuación:

    ```azurecli-interactive
    vi azure-vm-start.yml
    ```

1.  Seleccione la tecla **I** para acceder al modo de inserción.

1.  Pegue el siguiente código de ejemplo en el editor:

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

1.  Seleccione la tecla **Esc** para acceder al modo de inserción.

1.  Guarde el archivo y salga del editor vi, para lo que debe escribir el siguiente comando:

    ```bash
    :wq
    ```

1.  Ejecute el cuaderno de Ansible de ejemplo.

    ```bash
    ansible-playbook azure-vm-start.yml
    ```

1.  La salida es similar a la del siguiente ejemplo, que muestra que la máquina virtual se ha iniciado correctamente:

    ```bash
    PLAY [Start Azure VM] ********************************************************

    TASK [Gathering Facts] ******************************************************
    ok: [localhost]

    TASK [Start the Virtual Machine] ********************************************
    changed: [localhost]

    PLAY RECAP ******************************************************************
    localhost                  : ok=2    changed=1    unreachable=0    failed=0
    ```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Uso de Ansible para administrar los inventarios dinámicos de Azure](/articles/ansible/ansible-manage-azure-dynamic-inventories)