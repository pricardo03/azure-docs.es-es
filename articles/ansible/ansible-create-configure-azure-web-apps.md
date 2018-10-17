---
title: Creación de aplicaciones Azure Web Apps con Ansible (versión preliminar)
description: Aprenda a usar Ansible para crear una aplicación web con Java 8 y el entorno de ejecución de contenedor de Tomcat en App Service en Linux.
ms.service: ansible
keywords: ansible, azure, devops, bash, playbook, Azure App Service, Web App, Java
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.topic: tutorial
ms.date: 09/20/2018
ms.openlocfilehash: 1899b1fc1e0a38d859fb3a7ce2153585579650f3
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47586712"
---
# <a name="create-azure-app-service-web-apps-using-ansible-preview"></a>Creación de Azure App Service Web Apps con Ansible (versión preliminar)
[Azure App Service Web Apps](https://docs.microsoft.com/azure/app-service/app-service-web-overview) (o simplemente Web Apps) es un servicio para hospedar aplicaciones web, API REST y servidores back-end para dispositivos móviles. Puede desarrollarlo en su lenguaje preferido, ya sea. NET, .NET Core, Java, Ruby, Node.js, PHP o Python.

Ansible permite automatizar la implementación y la configuración de recursos en un entorno. En este artículo se muestra cómo usar Ansible para crear una aplicación web con el entorno de ejecución de Java. 

## <a name="prerequisites"></a>Requisitos previos
- **Suscripción a Azure**: si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) antes de empezar.
- [!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation1.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation1.md)][!INCLUDE [ansible-prereqs-for-cloudshell-use-or-vm-creation2.md](../../includes/ansible-prereqs-for-cloudshell-use-or-vm-creation2.md)]

> [!Note]
> Se requiere Ansible 2.7 para ejecutar los siguientes cuadernos de estrategias de ejemplo en este tutorial. Podría instalar la versión RC de Ansible 2.7 mediante la ejecución de `sudo pip install ansible[azure]==2.7.0rc2`. Ansible 2.7 se publicará en octubre de 2018. Después de eso, no será necesario especificar aquí la versión porque la versión predeterminada será 2.7. 

## <a name="create-a-simple-app-service"></a>Creación de un servicio de aplicación simple
En esta sección se presenta un cuaderno de estrategias de Ansible de ejemplo que define los siguientes recursos:
- El grupo de recursos, donde se implementará el plan de App Service y la aplicación web.
- La aplicación web, una aplicación web con Java 8 y el entorno de ejecución de contenedor de Tomcat en App Service en Linux.

```
- hosts: localhost
  connection: local
  vars:
    resource_group: myfirstResourceGroup
    webapp_name: myfirstWebApp
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
          name: myappplan
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
Guarde el cuaderno de estrategias anterior como firstwebapp.yml.

Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook firstwebapp.yml
```

La salida de la ejecución del cuaderno de estrategias de Ansible muestra que la aplicación web se ha creado correctamente:

```
TASK [Create a resource group] *************************************************
changed: [localhost]

TASK [Create App Service on Linux with Java Runtime] ******************************
 [WARNING]: Azure API profile latest does not define an entry for
WebSiteManagementClient
changed: [localhost]

PLAY RECAP *********************************************************************
localhost                  : ok=2    changed=2    unreachable=0    failed=0   
```

## <a name="create-app-service-with-traffic-manager"></a>Creación de App Service con Traffic Manager
Puede usar [Azure Traffic Manager](https://docs.microsoft.com/azure/app-service/web-sites-traffic-manager) para controlar la manera en que se distribuyen solicitudes de clientes web a aplicaciones de Azure App Service. Cuando se agregan puntos de conexión de App Service a un perfil de Azure Traffic Manager, este hace un seguimiento del estado de las aplicaciones de App Service (en ejecución, detenidas o eliminadas) para decidir cuáles de esos puntos de conexión debe recibir tráfico.

En App Service, cada aplicación se ejecuta en un [plan de App Service](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview
). Un plan de App Service define un conjunto de recursos de proceso para que una aplicación web se ejecute. Puede administrar el plan de App Service y la aplicación web en diferentes grupos.

En esta sección se presenta un cuaderno de estrategias de Ansible de ejemplo que define los siguientes recursos:
- Grupo de recursos, donde se implementará su plan de App Service.
- Plan de servicio de aplicación
- Grupo de recursos secundario, donde se implementará la aplicación web.
- La aplicación web, una aplicación web con Java 8 y el entorno de ejecución de contenedor de Tomcat en App Service en Linux.
- Perfil del Administrador de tráfico
- Punto de conexión de Traffic Manager mediante el sitio web creado

```
- hosts: localhost
  connection: local
  vars:
    resource_group: myResourceGroup
    plan_resource_group: planResourceGroup
    app_name: myLinuxWebApp
    location: eastus
    linux_plan_name: myAppServicePlan
    traffic_manager_profile_name: myTrafficManagerProfile
    traffic_manager_endpoint_name: myTrafficManagerEndpoint

  tasks:
  - name: Create resource group
    azure_rm_resourcegroup:
        name: "{{ resource_group }}"
        location: "{{ location }}"

  - name: Create secondary resource group
    azure_rm_resourcegroup:
        name: "{{ plan_resource_group }}"
        location: "{{ location }}"

  - name: Create App Service Plan
    azure_rm_appserviceplan:
      resource_group: "{{ plan_resource_group }}"
      name: "{{ linux_plan_name }}"
      location: "{{ location }}"
      is_linux: true
      sku: S1
      number_of_workers: 1

  - name: Create App Service on Linux with Java Runtime
    azure_rm_webapp:
        resource_group: "{{ resource_group }}"
        name: "{{ app_name }}"
        plan:
          resource_group: "{{ plan_resource_group }}"
          name: "{{ linux_plan_name }}"
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
      resource_group: "{{ resource_group }}"
      name: "{{ app_name }}"
    register: webapp
    
  - name: Create Traffic Manager Profile
    azure_rm_trafficmanagerprofile:
      resource_group: "{{ resource_group }}"
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
      resource_group: "{{ resource_group }}"
      profile_name: "{{ traffic_manager_profile_name }}"
      name: "{{ traffic_manager_endpoint_name }}"
      type: azure_endpoints
      location: "{{ location }}"
      target_resource_id: "{{ webapp.webapps[0].id }}"

```
Guarde el cuaderno de estrategias anterior como webapp.yml, o [descargue el cuaderno de estrategias](https://github.com/Azure-Samples/ansible-playbooks/blob/master/webapp.yml).

Para ejecutar el cuaderno de estrategias, use el comando **ansible-playbook** de la siguiente manera:
```bash
ansible-playbook webapp.yml
```

La salida de la ejecución del cuaderno de estrategias de Ansible muestra que el plan de App Service, la aplicación web, el perfil de Traffic Manager y el punto de conexión se han creado correctamente:
```
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
> [Ansible en Azure](https://docs.microsoft.com/azure/ansible/)