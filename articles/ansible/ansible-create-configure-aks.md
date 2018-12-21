---
title: Creación y configuración de clústeres de Azure Kubernetes Service en Azure mediante Ansible
description: Aprenda a usar Ansible para crear y administrar un clúster de Azure Kubernetes Service en Azure
ms.service: ansible
keywords: ansible, azure, devops, bash, cloudshell, cuaderno de estrategias, aks, contenedor, Kubernetes
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 08/23/2018
ms.openlocfilehash: f17998957db6fca5b109b5a05aa624e238af8dca
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "53257546"
---
# <a name="create-and-configure-azure-kubernetes-service-clusters-in-azure-using-ansible"></a>Creación y configuración de clústeres de Azure Kubernetes Service en Azure mediante Ansible
Ansible permite automatizar la implementación y la configuración de recursos en un entorno. Puede usar Ansible para administrar Azure Kubernetes Service (AKS). En este artículo se muestra cómo usar Ansible para crear y configurar un clúster de Azure Kubernetes Service.

## <a name="prerequisites"></a>Requisitos previos
- **Suscripción a Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- **Entidad de servicio de Azure**: al [crear la entidad de servicio](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#create-the-service-principal), tenga en cuenta los siguientes valores: **appId**, **displayName**, **password** y **tenant**.

- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Ansible 2.6 es necesario para ejecutar los siguientes cuadernos de estrategias de ejemplo de este tutorial. 

## <a name="create-a-managed-aks-cluster"></a>Creación de un clúster de AKS administrado
El siguiente cuaderno de estrategias de Ansible de ejemplo crea un grupo de recursos y un clúster de AKS que reside en dicho grupo:

  ```yaml
  - name: Create Azure Kubernetes Service
    hosts: localhost
    connection: local
    vars:
      resource_group: myResourceGroup
      location: eastus
      aks_name: myAKSCluster
      username: azureuser
      ssh_key: "your_ssh_key"
      client_id: "your_client_id"
      client_secret: "your_client_secret"
    tasks:
    - name: Create resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"
    - name: Create a managed Azure Container Services (AKS) cluster
      azure_rm_aks:
        name: "{{ aks_name }}"
        location: "{{ location }}"
        resource_group: "{{ resource_group }}"
        dns_prefix: "{{ aks_name }}"
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 2
            vm_size: Standard_D2_v2
        tags:
          Environment: Production
  ```

Las siguientes viñetas ayudan a explicar el código del cuaderno de estrategias de Ansible anterior:
- En la primera sección dentro de las **tareas** se define un grupo de recursos llamado **myResourceGroup** dentro de la ubicación **eastus**. 
- En la segunda sección dentro de las **tareas** se define un clúster de AKS llamado **myAKSCluster** dentro del grupo de recursos **myResourceGroup**. 

Para crear el clúster de AKS con Ansible, guarde el cuaderno de estrategias de ejemplo anterior como `azure_create_aks.yml` y ejecútelo con el siguiente comando:

  ```bash
  ansible-playbook azure_create_aks.yml
  ```

La salida del comando **ansible-playbook* es similar a la siguiente, que muestra que el clúster de AKS se ha creado correctamente:

  ```Output
  PLAY [Create AKS] ****************************************************************************************

  TASK [Gathering Facts] ********************************************************************************************
  ok: [localhost]

  TASK [Create resource group] **************************************************************************************
  changed: [localhost]

  TASK [Create a Azure Container Services (AKS) cluster] ***************************************************
  changed: [localhost]

  PLAY RECAP *********************************************************************************************************
  localhost                  : ok=3    changed=2    unreachable=0    failed=0
  ```

## <a name="scale-aks-nodes"></a>Escalado de nodos de AKS

El cuaderno de estrategias de ejemplo de la sección anterior define dos nodos. Si necesita menos o más cargas de trabajo de contenedor en el clúster, puede ajustar fácilmente el número de nodos. El cuaderno de estrategias de ejemplo de esta sección aumenta el número de nodos de dos a tres. La modificación del número de nodos se lleva a cabo cambiando el valor **count** en el bloque **agent_pool_profiles**. 

Escriba sus propios valores `ssh_key`, `client_id` y `client_secret` en el bloque **service_principal**:

```yaml
- name: Scale AKS cluster
  hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    location: eastus
    aks_name: myAKSCluster
    username: azureuser
    ssh_key: "your_ssh_key"
    client_id: "your_client_id"
    client_secret: "your_client_secret"
  tasks:
  - name: Scaling an existed AKS cluster
    azure_rm_aks:
        name: "{{ aks_name }}"    
        location: "{{ location }}"
        resource_group: "{{ resource_group }}" 
        dns_prefix: "{{ aks_name }}" 
        linux_profile:
          admin_username: "{{ username }}"
          ssh_key: "{{ ssh_key }}"
        service_principal:
          client_id: "{{ client_id }}"
          client_secret: "{{ client_secret }}"
        agent_pool_profiles:
          - name: default
            count: 3
            vm_size: Standard_D2_v2
```

Para escalar el clúster de Azure Kubernetes Service con Ansible, guarde el cuaderno de estrategias anterior como *azure_configure_aks.yml* y ejecútelo de la manera siguiente:

  ```bash
  ansible-playbook azure_configure_aks.yml
  ```

La siguiente salida muestra que el clúster de AKS se ha creado correctamente:

  ```Output
  PLAY [Scale AKS cluster] ***************************************************************

  TASK [Gathering Facts] ******************************************************************
  ok: [localhost]

  TASK [Scaling an existed AKS cluster] **************************************************
  changed: [localhost]

  PLAY RECAP ******************************************************************************
  localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```
## <a name="delete-a-managed-aks-cluster"></a>Eliminación de un clúster de AKS administrado

La siguiente sección del cuaderno de Ansible de ejemplo muestra cómo eliminar un clúster de AKS:

  ```yaml
  - name: Delete a managed Azure Container Services (AKS) cluster
    hosts: localhost
    connection: local
    vars:
      resource_group: myResourceGroup
      aks_name: myAKSCluster
    tasks:
    - name: 
      azure_rm_aks:
        name: "{{ aks_name }}"
        resource_group: "{{ resource_group }}"
        state: absent
   ```

Para eliminar el clúster de Azure Kubernetes Service con Ansible, guarde el cuaderno anterior como *azure_delete_aks.yml* y ejecútelo de la manera siguiente:

  ```bash
  ansible-playbook azure_delete_aks.yml
  ```

La siguiente salida muestra que el clúster de AKS se ha eliminado correctamente:
  ```Output
PLAY [Delete a managed Azure Container Services (AKS) cluster] ****************************

TASK [Gathering Facts] ********************************************************************
ok: [localhost]

TASK [azure_rm_aks] *********************************************************************

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0
  ```
  
## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Tutorial: Escalado de una aplicación en Azure Kubernetes Service (AKS)](https://docs.microsoft.com/azure/aks/tutorial-kubernetes-scale)
