---
title: Matriz de módulo y versión de Ansible para Azure
description: Matriz de módulo y versión de Ansible para Azure
ms.service: azure
keywords: ansible, roles, matriz, versión, azure, devops
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 09/22/2018
ms.topic: article
ms.openlocfilehash: f2f1dccfd5b91205d0673c2b82ad7cfa673f61f6
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57791824"
---
# <a name="ansible-module-and-version-matrix"></a>Matriz de módulo y versión de Ansible

## <a name="ansible-modules-for-azure"></a>Módulos de Ansible para Azure
Ansible se suministra con una serie de módulos que se pueden ejecutar directamente en los hosts remotos o mediante guiones de procedimientos.
En este artículo se enumeran los módulos de Ansible para Azure que pueden aprovisionar recursos en la nube de Azure, como máquinas virtuales, redes y servicios de contenedor. Puede obtener estos módulos desde la versión oficial de Ansible o desde los siguientes roles de guion de procedimientos publicados por Microsoft.

| Módulo Ansible para Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 | Ansible 2.7 | Rol de Ansible | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|-------------------------------------| 
| **Proceso**                    |           |                          |                          |                            |                                | 
| azure_rm_availabilityset                    | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_availabilityset_facts              | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_deployment                         | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_resource                           | -            | -                           | Sí          | Sí          | Sí                                 | 
| azure_rm_resource_facts                     | -            | -                           | Sí          | Sí          | Sí                                 | 
| azure_rm_virtualmachine_scaleset_facts      | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_virtualmachineimage_facts          | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_resourcegroup                      | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_resourcegroup_facts                | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_virtualmachine                     | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_virtualmachine_facts               | -            | -                           | -            | Sí          | Sí                                 | 
| azure_rm_virtualmachine_extension           | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_virtualmachine_scaleset            | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_image                              |              | Sí                         | Sí          | Sí          | Sí                                 | 
| **Redes**                    |           |                          |                          |                             |                               | 
| azure_rm_virtualnetwork                     | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_virtualnetwork_facts               | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_subnet                             | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_networkinterface                   | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_networkinterface_facts             | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_publicipaddress                    | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_publicipaddress_facts              | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_dnsrecordset                       | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_dnsrecordset_facts                 | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_dnszone                            | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_dnszone_facts                      | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_loadbalancer                       | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_loadbalancer_facts                 | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_appgateway                         | -            | -                           | -            | Sí          | Sí                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | -            | Sí                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | -            | Sí                                 |
| azure_rm_appgwroutetable                    | -            | -                           | -            | -            | Sí                                 |
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_securitygroup                      | Sí          | Sí                         | Sí          | Sí          | Sí                                 |
| azure_rm_route                              | -            | -                           | -            | Sí          | Sí                                 | 
| azure_rm_routetable                         | -            | -                           | -            | Sí          | Sí                                 | 
| azure_rm_routetable_facts                   | -            | -                           | -            | Sí          | Sí                                 | 
| **Storage**                    |           |                          |                          |                             |                               | 
| azure_rm_storageaccount                     | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_storageaccount_facts               | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_storageblob                        | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_managed_disk                       | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_managed_disk_facts                 | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| **Contenedores**                    |           |                          |                          |                            |                                | 
| azure_rm_aks                                | -            | -                           | Sí          | Sí          | Sí                                 | 
| azure_rm_aks_facts                          | -            | -                           | Sí          | Sí          | Sí                                 | 
| azure_rm_acs                                | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_containerinstance                  | -            | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | -              | -            | Sí                                 | 
| azure_rm_containerregistry                  | -            | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | -            | Sí          | Sí                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | -            | Sí                                 | 
| **Azure Functions**                    |           |                          |                          |                            |                                | 
| azure_rm_functionapp                        | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_functionapp_facts                  | Sí          | Sí                         | Sí          | Sí          | Sí                                 | 
| **Bases de datos**                    |           |                          |                          |                             |                               | 
| azure_rm_sqlserver                          | -            | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_sqlserver_facts                    | -            | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_sqldatabase                        | -            | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | Sí          | Sí                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_mysqlserver                        | -            | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | Sí          | Sí                                 | 
| azure_rm_mysqldatabase                      | -            | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | Sí          | Sí                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_postgresqlserver                   | -            | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | Sí          | Sí                                 | 
| azure_rm_postgresqldatabase                 | -            | Sí                         | Sí          | Sí          | Sí                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | Sí          | Sí                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | Sí                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | -            | Sí                                 | 
| **Key Vault**                    |           |                          |                          |                             |                               | 
| azure_rm_keyvault                           | -            | Sí                         | Sí          | Sí          | Sí                                 |
| azure_rm_keyvault_facts                     | -            | -                           | -              | -              | Sí                               |
| azure_rm_keyvaultkey                        | -            | Sí                         | Sí          | Sí          | Sí                                 |
| azure_rm_keyvaultsecret                     | -            | Sí                         | Sí          | Sí          | Sí                                 |
| **Aplicaciones web**                    |           |                          |                          |                             |                               | 
| azure_rm_appserviceplan                          | -            | -                         | -          | Sí          | Sí                                 | 
| azure_rm_appserviceplan_facts                    | -            | -                         | -          | Sí          | Sí                                 | 
| azure_rm_webapp                                  | -            | -                         | -          | Sí          | Sí                                 | 
| azure_rm_webapp_facts                            | -            | -                         | -          | Sí          | Sí                                 | 
| **Traffic Manager**                    |           |                          |                          |                             |                               | 
| azure_rm_trafficmanagerendpoint                  | -            | -                         | -          | Sí          | Sí                                 | 
| azure_rm_trafficmanagerendpoint_facts            | -            | -                         | -          | Sí          | Sí                                 | 
| azure_rm_trafficmanagerprofile                   | -            | -                         | -          | Sí          | Sí                                 | 
| azure_rm_trafficmanagerprofile_facts             | -            | -                         | -          | Sí          | Sí                                 | 
| **Escalabilidad automática**                    |           |                          |                          |                             |                               | 
| azure_rm_autoscale                  | -            | -                         | -          | Sí          | Sí                                 | 
| azure_rm_autoscale_facts            | -            | -                         | -          | Sí          | Sí                                 | 

## <a name="introduction-to-playbook-role-for-azure"></a>Introducción al rol de cuaderno de estrategias de Azure
El [rol de guion de procedimientos azure_preview_module](https://galaxy.ansible.com/Azure/azure_preview_modules/) es el rol más completo e incluye todos los módulos más recientes de Azure. Las actualizaciones y correcciones de errores se realizan de manera más oportuna que en la versión oficial de Ansible. Si usa Ansible para el aprovisionamiento de recursos de Azure, se recomienda instalar el rol de cuaderno de estrategias azure_preview_module.

El rol de guion de procedimientos azure_preview_module se publica cada tres semanas.

## <a name="next-steps"></a>Pasos siguientes
Para más información relacionada con los roles de guion de procedimientos, consulte [Creating Reusable Playbooks](https://docs.ansible.com/ansible/latest/playbooks_reuse.html) (Creación de guiones de procedimientos reutilizables). 
