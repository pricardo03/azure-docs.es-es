---
title: Escalado automático de un conjunto de escalado de máquinas virtuales en Azure con Ansible
description: Aprenda a usar Ansible para escalar un conjunto de escalado de máquinas virtuales con la escalabilidad automática en Azure.
ms.service: azure
keywords: ansible, azure, devops, bash, playbook, scale, autoscale, virtual machine, virtual machine scale set, vmss
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/10/2018
ms.openlocfilehash: 578ad3207f62e74805be056ca11d3bd9b46513da
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792436"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-in-azure-using-ansible"></a>Escalado automático de un conjunto de escalado de máquinas virtuales en Azure con Ansible
Ansible permite automatizar la implementación y la configuración de recursos en un entorno. Puede usar Ansible para administrar su conjunto de escalado de máquinas virtuales (VMSS) de Azure, al igual que podría hacerlo con cualquier otro recurso de Azure. 

Al crear un conjunto de escalado, puede definir el número de instancias de máquina virtual que quiere ejecutar. A medida que cambia la demanda de las aplicaciones, puede aumentar o reducir automáticamente el número de estas instancias. La posibilidad de realizar el escalado automático le permite satisfacer la demanda del cliente o responder a los cambios de rendimiento de la aplicación a lo largo del ciclo de vida de esta. En este artículo, creará una configuración de escalabilidad automática y la asociará a un conjunto de escalado de máquinas virtuales existente. En la configuración de la escalabilidad automática, puede configurar una regla para escalar o reducir horizontalmente según sus preferencias.

## <a name="prerequisites"></a>Requisitos previos
- **Suscripción a Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- Un conjunto de escalado de máquinas virtuales de Azure ya existente. - Si no tiene uno, consulte [Creación de conjuntos de escalado de máquinas virtuales de Azure mediante Ansible](https://docs.microsoft.com/azure/ansible/ansible-create-configure-vmss).

> [!Note]
> Se requiere Ansible 2.7 para ejecutar los siguientes cuadernos de estrategias de ejemplo en este tutorial. 

## <a name="auto-scale-based-on-a-schedule"></a>Escalación automática según una programación   
Para habilitar el escalado automático en un conjunto de escalado, primero debe definir un perfil de escalado automático. Este perfil define la capacidad predeterminada, mínima y máxima del conjunto de escalado. Estos límites le permiten controlar el costo al no crear continuamente instancias de máquina virtual, y equilibrar un rendimiento aceptable con un número mínimo de instancias que permanecen en un evento de reducción horizontal. 

Puede reducir y escalar horizontalmente los Virtual Machine Scale Sets siguiendo una programación periódica o en una fecha determinada. En esta sección se presenta un ejemplo de cuaderno de estrategias de Ansible que crea una configuración de escalabilidad automática por la que se aumenta el número de instancias de máquina virtual a tres en los conjuntos de escalado a las 10:00 todos los lunes, zona horaria del Pacífico. 

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myVMSS
    name: autoscalesetting
  tasks: 
    - name: Create auto scaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         target:
           namespace: "Microsoft.Compute"
           types: "virtualMachineScaleSets"
           name: "{{ vmss_name }}"
         enabled: true
         profiles:
         - count: '3'
           min_count: '3'
           max_count: '3'
           name: Auto created scale condition
           recurrence_timezone: Pacific Standard Time
           recurrence_frequency: Week
           recurrence_days:
              - Monday
           recurrence_mins:
              - '0'
           recurrence_hours:
              - '10'
```

Guarde este cuaderno de estrategias como *vmss-auto-scale.yml*. Para ejecutar el cuaderno de estrategias de Ansible, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook vmss-auto-scale.yml
```

## <a name="auto-scale-based-on-performance-data"></a>Escalado automático basado en datos de rendimiento
Si aumenta la demanda de la aplicación, la carga de las instancias de máquina virtual de los conjuntos de escalado aumenta. Si este aumento de la carga es continuado, en lugar de ser algo puntual, puede configurar reglas de escalado automático para aumentar el número de instancias de máquina virtual en el conjunto de escalado. Cuando se crean estas instancias de máquina virtual y se implementan las aplicaciones, el conjunto de escalado empieza a distribuir el tráfico entre ellas mediante el equilibrador de carga. Puede controlar qué métricas se deben supervisar como, por ejemplo, la CPU o el disco, cuánto tiempo debe cumplir la carga de la aplicación un límite determinado y cuántas instancias de máquina virtual se deben agregar al conjunto de escalado.

Puede reducir y escalar horizontalmente los Virtual Machine Scale Sets según umbrales de métricas de rendimiento, siguiendo una programación periódica o por una fecha determinada. En esta sección se presenta un ejemplo de cuaderno de estrategias de Ansible que comprueba la carga de trabajo de los últimos 10 minutos a las 18:00 todos los lunes, zona horaria del Pacífico, y escala horizontalmente el número de instancias de máquinas virtuales en los conjuntos de escalado a cuatro o reduce horizontalmente a una instancia según la métrica del porcentaje de CPU. 

```yml
---
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    vmss_name: myVMSS
    name: autoscalesetting
  tasks:
  - name: Get facts of the resource group
    azure_rm_resourcegroup_facts:
      name: "{{ resource_group }}"
    register: rg

  - name: Get VMSS resource uri
    set_fact:
      vmss_id: "{{ rg.ansible_facts.azure_resourcegroups[0].id }}/providers/Microsoft.Compute/virtualMachineScaleSets/{{ vmss_name }}"
    
  - name: Create auto scaling
    azure_rm_autoscale:
      resource_group: "{{ resource_group }}"
      name: "{{ name }}"
      target: "{{ vmss_id }}"
      enabled: true
      profiles:
      - count: '1'
        max_count: '1'
        min_count: '1'
        name: 'This scale condition is executed when none of the other scale condition(s) match'
        recurrence_days:
          - Monday
        recurrence_frequency: Week
        recurrence_hours:
          - 18
        recurrence_mins:
          - 0
        recurrence_timezone: Pacific Standard Time
      - count: '1'
        min_count: '1'
        max_count: '4'
        name: Auto created scale condition
        recurrence_days:
          - Monday
        recurrence_frequency: Week
        recurrence_hours:
          - 18
        recurrence_mins:
          - 0
        recurrence_timezone: Pacific Standard Time
        rules:
          - cooldown: 5
            direction: Increase
            metric_name: Percentage CPU
            metric_resource_uri: "{{ vmss_id }}"
            operator: GreaterThan
            statistic: Average
            threshold: 70
            time_aggregation: Average
            time_grain: 1
            time_window: 10
            type: ChangeCount
            value: '1'
          - cooldown: 5
            direction: Decrease
            metric_name: Percentage CPU
            metric_resource_uri: "{{ vmss_id }}"
            operator: LessThan
            statistic: Average
            threshold: 30
            time_aggregation: Average
            time_grain: 1
            time_window: 10
            type: ChangeCount
            value: '1'
```

Guarde este cuaderno de estrategias como *vmss-auto-scale-metrics.yml*. Para ejecutar el cuaderno de estrategias de Ansible, use el comando **ansible-playbook** de la siguiente manera:

```bash
ansible-playbook vmss-auto-scale-metrics.yml
```

## <a name="get-information-for-existing-autoscale-settings"></a>Información sobre la configuración de escalabilidad automática existente
Puede obtener detalles de cualquier configuración de escalabilidad automática a través del módulo *azure_rm_autoscale_facts* con el cuaderno de estrategias del modo siguiente:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Retrieve auto scale settings information
      azure_rm_autoscale_facts:
        resource_group: "{{ resource_group }}"
        name: "{{ name }}"
      register: autoscale_query
    
    - debug:
        var: autoscale_query.autoscales[0]
```

## <a name="disable-the-autoscale-settings"></a>Deshabilitación de la configuración de escalabilidad automática
Puede deshabilitar la configuración de la escalabilidad automática cambiando `enabled: true` a `enabled: false`, o eliminando la configuración de escalabilidad automática con el cuaderno de estrategias del modo siguiente:

```yml
- hosts: localhost
  vars:
    resource_group: myResourceGroup
    name: autoscalesetting
  tasks: 
    - name: Delete auto scaling
      azure_rm_autoscale:
         resource_group: "{{ resource_group }}"
         name: "{{ name }}"
         state: absent
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Ansible sample playbook for virtual machine scale sets](https://github.com/Azure-Samples/ansible-playbooks/tree/master/vmss) (Cuaderno de estrategias de ejemplo de Ansible para conjuntos de escalado de máquinas virtuales)