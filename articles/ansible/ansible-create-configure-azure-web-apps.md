---
title: Creación de aplicaciones web de Azure con Ansible
description: Aprenda a usar Ansible para crear una aplicación web con Java 8 y el entorno de ejecución de contenedores Tomcat en App Service en Linux
ms.service: azure
keywords: ansible, azure, devops, bash, playbook, Azure App Service, Web App, Java
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 12/08/2018
ms.openlocfilehash: 5f67a9f7d629eec9ab1462a25940355869c1cd28
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57791229"
---
# <a name="create-azure-app-service-web-apps-by-using-ansible"></a>Creación de aplicaciones web de Azure App Service con Ansible
[Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/overview) (o simplemente Web Apps) hospeda aplicaciones web, API de REST y servidores back-end para dispositivos móviles. Puede desarrollar en su lenguaje preferido, ya sea .NET, .NET Core, Java, Ruby, Node.js, PHP o Python.

Ansible permite automatizar la implementación y la configuración de recursos en el entorno. En este artículo se muestra cómo usar Ansible para crear una aplicación web con el entorno de ejecución de Java. 

## <a name="prerequisites"></a>Requisitos previos
- **Suscripción a Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Se requiere Ansible 2.7 para ejecutar los siguientes cuadernos de estrategias de ejemplo en este tutorial.

## <a name="create-a-simple-app-service"></a>Creación de un servicio de aplicación simple
En esta sección se presenta un cuaderno de estrategias de Ansible de ejemplo que define los siguientes recursos:
- El grupo de recursos, en el que se implementarán el plan de App Service y la aplicación web
- Una aplicación web con Java 8 y el entorno de ejecución de contenedores Tomcat en App Service en Linux

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    webapp_name: myfirstWebApp
    plan_name: myAppServicePlan
    location: eastus
  tasks:
    - name: Create a resource group
      azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

    - name: Create App Service on Linux with Java Runtime
      azure_rm_webapp:
        resource_group: "{{ resource_group }}"
        name: "{{ webapp_name }}"
        plan:
          resource_group: "{{ resource_group }}"
          name: "{{ plan_name }}"
          is_linux: true
          sku: S1
          number_of_workers: 1
        frameworks:
          - name: "java"
            version: "8"
            settings:
              java_container: tomcat
              java_container_version: 8.5
```
Guarde el cuaderno de estrategias anterior como **firstwebapp.yml**.

Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook firstwebapp.yml
```

La salida de la ejecución del cuaderno de estrategias de Ansible muestra que la aplicación web se ha creado correctamente:

```Output
PLAY [localhost] *************************************************

TASK [Gathering Facts] *************************************************
ok: [localhost]

TASK [Create a resource group] *************************************************
changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] *************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

PLAY RECAP *************************************************
localhost                  : ok=3    changed=2    unreachable=0    failed=0
```

## <a name="create-an-app-service-by-using-traffic-manager"></a>Creación de un servicio de aplicaciones mediante Traffic Manager
Puede usar [Azure Traffic Manager](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager) para controlar la manera en que se distribuyen solicitudes de clientes web a aplicaciones de Azure App Service. Cuando se agregan puntos de conexión de App Service a un perfil de Azure Traffic Manager, este realiza un seguimiento del estado de las aplicaciones de App Service. Los estados incluyen en ejecución, detenido y eliminado. Traffic Manager, a continuación, puede decidir cuál de esos puntos de conexión debe recibir el tráfico.

En App Service, cada aplicación se ejecuta en un [plan de App Service](https://docs.microsoft.com/azure/app-service/overview-hosting-plans
). Un plan de App Service define un conjunto de recursos de proceso para que una aplicación web se ejecute. Puede administrar el plan de App Service y la aplicación web en grupos distintos.

En esta sección se presenta un cuaderno de estrategias de Ansible de ejemplo que define los siguientes recursos:
- Grupo de recursos, donde se implementará su plan de App Service.
- Plan de App Service
- Grupo de recursos secundario, donde se implementará la aplicación web
- Una aplicación web con Java 8 y el entorno de ejecución de contenedores Tomcat en App Service en Linux
- Perfil de Traffic Manager
- Punto de conexión de Traffic Manager con el sitio web creado

```yml
- hosts: localhost
  connection: local
  vars:
    resource_group_webapp: myResourceGroupWebapp
    resource_group: myResourceGroup
    webapp_name: myLinuxWebApp
    plan_name: myAppServicePlan
    location: eastus
    traffic_manager_profile_name: myTrafficManagerProfile
    traffic_manager_endpoint_name: myTrafficManagerEndpoint

  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group_webapp }}"
        location: "{{ location }}"

  - name: Create secondary resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

  - name: Create App Service Plan
    azure_rm_appserviceplan:
      resource_group: "{{ resource_group }}"
      name: "{{ plan_name }}"
      location: "{{ location }}"
      is_linux: true
      sku: S1
      number_of_workers: 1

  - name: Create App Service on Linux with Java Runtime
    azure_rm_webapp:
        resource_group: "{{ resource_group_webapp }}"
        name: "{{ webapp_name }}"
        plan:
          resource_group: "{{ resource_group }}"
          name: "{{ plan_name }}"
          is_linux: true
          sku: S1
          number_of_workers: 1
        app_settings:
          testkey: "testvalue"
        frameworks:
          - name: java
            version: 8
            settings:
              java_container: "Tomcat"
              java_container_version: "8.5"

  - name: Get web app facts
    azure_rm_webapp_facts:
      resource_group: "{{ resource_group_webapp }}"
      name: "{{ webapp_name }}"
    register: webapp
    
  - name: Create Traffic Manager Profile
    azure_rm_trafficmanagerprofile:
      resource_group: "{{ resource_group_webapp }}"
      name: "{{ traffic_manager_profile_name }}"
      location: global
      routing_method: performance
      dns_config:
        relative_name: "{{ traffic_manager_profile_name }}"
        ttl:  60
      monitor_config:
        protocol: HTTPS
        port: 80
        path: '/'

  - name: Add endpoint to traffic manager profile, using created web site
    azure_rm_trafficmanagerendpoint:
      resource_group: "{{ resource_group_webapp }}"
      profile_name: "{{ traffic_manager_profile_name }}"
      name: "{{ traffic_manager_endpoint_name }}"
      type: azure_endpoints
      location: "{{ location }}"
      target_resource_id: "{{ webapp.webapps[0].id }}"
```
Guarde el cuaderno de estrategias anterior como **webapp.yml** o [descargue el cuaderno de estrategias](https://github.com/Azure-Samples/ansible-playbooks/blob/master/webapp.yml).

Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook webapp.yml
```

La salida de la ejecución del cuaderno de estrategias de Ansible muestra que el plan de App Service, la aplicación web, el perfil de Traffic Manager y el punto de conexión se han creado correctamente:
```Output
PLAY [localhost] *************************************************

TASK [Gathering Facts] *************************************************
ok: [localhost]

TASK [Create resource group] ****************************************************************************
changed: [localhost]

TASK [Create resource group for app service plan] ****************************************************************************
changed: [localhost]

TASK [Create App Service Plan] ****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for WebSiteManagementClient

changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] ****************************************************************************
changed: [localhost]

TASK [Get web app facts] *****************************************************************************
ok: [localhost]

TASK [Create Traffic Manager Profile] *****************************************************************************
 [WARNING]: Azure API profile latest does not define an entry for TrafficManagerManagementClient

changed: [localhost]

TASK [Add endpoint to traffic manager profile, using the web site created above] *****************************************************************************
changed: [localhost]

TASK [Get Traffic Manager Profile facts] ******************************************************************************
ok: [localhost]

PLAY RECAP ******************************************************************************
localhost                  : ok=9    changed=6    unreachable=0    failed=0
```

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"] 
> [Escalado de aplicaciones web de Azure App Service con Ansible](https://docs.microsoft.com/azure/ansible/ansible-scale-azure-web-apps)