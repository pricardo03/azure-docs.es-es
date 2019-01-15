---
title: Escalado de aplicaciones web de Azure App Service con Ansible
description: Aprenda a usar Ansible para crear una aplicación web con Java 8 y el entorno de ejecución de contenedores Tomcat en App Service en Linux
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, Azure App Service, Web App, escala, Java
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/08/2018
ms.openlocfilehash: 4ef8320d3eba841ee64557e31e63b4e79ee3aa92
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/09/2019
ms.locfileid: "54159900"
---
# <a name="scale-azure-app-service-web-apps-by-using-ansible"></a>Escalado de aplicaciones web de Azure App Service con Ansible
[Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/overview) (o simplemente Web Apps) hospeda aplicaciones web, API de REST y servidores back-end para dispositivos móviles. Puede desarrollar en su lenguaje preferido, ya sea .NET, .NET Core, Java, Ruby, Node.js, PHP o Python.

Ansible permite automatizar la implementación y la configuración de recursos en el entorno. En este artículo se muestra cómo usar Ansible para escalar aplicaciones en Azure App Service.

## <a name="prerequisites"></a>Requisitos previos
- **Suscripción a Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]
- **Azure App Service Web Apps**: si aún no tiene una aplicación web de Azure App Service, puede [crear aplicaciones web de Azure mediante Ansible](ansible-create-configure-azure-web-apps.md).

## <a name="scale-up-an-app-in-app-service"></a>Escalado vertical de aplicaciones en App Service
Para escalar verticalmente, se cambia el plan de tarifa del plan de App Service al que pertenece la aplicación. En esta sección se presenta un cuaderno de estrategias de Ansible de ejemplo que define la siguiente operación:
- Obtener datos de un plan de App Service existente
- Actualizar el plan de App Service a S2 con tres roles de trabajo

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_name: myAppServicePlan
    location: eastus

  tasks:
  - name: Get facts of existing App service plan
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku

  - name: Scale up the App service plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      is_linux: true
      sku: S2
      number_of_workers: 3
      
  - name: Get facts
    azure_rm_appserviceplan_facts:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
    register: facts

  - debug: 
      var: facts.appserviceplans[0].sku
```

Guarde este cuaderno de estrategias como *webapp_scaleup.yml*.

Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook webapp_scaleup.yml
```

Tras ejecutar el cuaderno de estrategias, una salida similar a la del siguiente ejemplo muestra que el plan de App Service se ha actualizado correctamente a S2 con tres roles de trabajo:
```Output
PLAY [localhost] **************************************************************

TASK [Gathering Facts] ********************************************************
ok: [localhost]

TASK [Get facts of existing App service plan] **********************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

ok: [localhost]

TASK [debug] ******************************************************************
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 1,
        "family": "S",
        "name": "S1",
        "size": "S1",
        "tier": "Standard"
    }
}

TASK [Scale up the App service plan] *******************************************
changed: [localhost]

TASK [Get facts] ***************************************************************
ok: [localhost]

TASK [debug] *******************************************************************
ok: [localhost] => {
    "facts.appserviceplans[0].sku": {
        "capacity": 3,
        "family": "S",
        "name": "S2",
        "size": "S2",
        "tier": "Standard"
    }
}

PLAY RECAP **********************************************************************
localhost                  : ok=6    changed=1    unreachable=0    failed=0 
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Ansible en Azure](https://docs.microsoft.com/azure/ansible/)