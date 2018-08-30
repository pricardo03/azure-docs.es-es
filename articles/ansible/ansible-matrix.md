---
title: Matriz de módulo y versión de Ansible para Azure
description: Matriz de módulo y versión de Ansible para Azure
ms.service: ansible
keywords: ansible, roles, matriz, versión, azure, devops
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 07/02/2018
ms.topic: article
ms.openlocfilehash: 8bbb2563991b45fe7a20ce243751c1c1a1f75b99
ms.sourcegitcommit: f6e2a03076679d53b550a24828141c4fb978dcf9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43094914"
---
# <a name="ansible-module-and-version-matrix"></a>Matriz de módulo y versión de Ansible

## <a name="ansible-modules-for-azure"></a>Módulos de Ansible para Azure
Ansible se suministra con una serie de módulos que se pueden ejecutar directamente en los hosts remotos o mediante guiones de procedimientos.
En este artículo se enumeran los módulos de Ansible para Azure que pueden aprovisionar recursos en la nube de Azure, como máquinas virtuales, redes y servicios de contenedor. Puede obtener estos módulos desde la versión oficial de Ansible o desde los siguientes roles de guion de procedimientos publicados por Microsoft.

| Módulo Ansible para Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 |  Rol de guion de procedimientos [azure_preview_module](#introduction-to-azurepreviewmodule) | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------| 
| **Proceso**                    |           |                          |                          |                                  | 
| azure_rm_availabilityset                    | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_availabilityset_facts              | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_deployment                         | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_resource                           | -            | -                           | SÍ          | SÍ                                 | 
| azure_rm_resource_facts                     | -            | -                           | SÍ          | SÍ                                 | 
| azure_rm_virtualmachine_scaleset_facts      | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_virtualmachineimage_facts          | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_resourcegroup                      | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_resourcegroup_facts                | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_virtualmachine                     | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_virtualmachine_extension           | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_virtualmachine_scaleset            | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_image                              |              | SÍ                         | Sí          | SÍ                                 | 
| **Redes**                    |           |                          |                          |                                  | 
| azure_rm_virtualnetwork                     | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_virtualnetwork_facts               | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_subnet                             | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_networkinterface                   | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_networkinterface_facts             | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_publicipaddress                    | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_publicipaddress_facts              | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_dnsrecordset                       | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_dnsrecordset_facts                 | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_dnszone                            | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_dnszone_facts                      | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_loadbalancer                       | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_loadbalancer_facts                 | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_appgw                              | -            | -                           | -            | SÍ                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | SÍ                                 | 
| azure_rm_appgwroute                         | -            | -                           | -            | SÍ                                 |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | SÍ                                 |
| azure_rm_appgwroutetable                    | -            | -                           | -            | SÍ                                 |
| azure_rm_securitygroup                      | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | SÍ                                 | 
| **Storage**                    |           |                          |                          |                                  | 
| azure_rm_storageaccount                     | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_storageaccount_facts               | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_storageblob                        | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_managed_disk                       | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_managed_disk_facts                 | SÍ          | Sí                         | Sí          | SÍ                                 | 
| **Contenedores**                    |           |                          |                          |                                  | 
| azure_rm_aks                                | -            | -                           | SÍ          | SÍ                                 | 
| azure_rm_aks_facts                          | -            | -                           | SÍ          | SÍ                                 | 
| azure_rm_acs                                | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_containerinstance                  | -            | SÍ                         | Sí          | SÍ                                 | 
| azure_rm_containerinstance_facts            | -            | -                           | -            | SÍ                                 | 
| azure_rm_containerregistry                  | -            | SÍ                         | Sí          | SÍ                                 | 
| azure_rm_containerregistry_facts            | -            | -                           | -            | SÍ                                 | 
| azure_rm_containerregistryreplication       | -            | -                           | -            | SÍ                                 | 
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | SÍ                                 | 
| azure_rm_containerregistrywebhook           | -            | -                           | -            | SÍ                                 | 
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | SÍ                                 | 
| **Azure Functions**                    |           |                          |                          |                                  | 
| azure_rm_functionapp                        | SÍ          | Sí                         | Sí          | SÍ                                 | 
| azure_rm_functionapp_facts                  | SÍ          | Sí                         | Sí          | SÍ                                 | 
| **Bases de datos**                    |           |                          |                          |                                  | 
| azure_rm_sqlserver                          | -            | SÍ                         | Sí          | SÍ                                 | 
| azure_rm_sqlserver_facts                    | -            | SÍ                         | Sí          | SÍ                                 | 
| azure_rm_sqldatabase                        | -            | SÍ                         | Sí          | SÍ                                 | 
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | SÍ                                 | 
| azure_rm_sqlelasticpool                     | -            | -                           | -            | SÍ                                 | 
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | SÍ                                 | 
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | SÍ                                 | 
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | SÍ                                 | 
| azure_rm_mysqlserver                        | -            | SÍ                         | Sí          | SÍ                                 | 
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | SÍ                                 | 
| azure_rm_mysqldatabase                      | -            | SÍ                         | Sí          | SÍ                                 | 
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | SÍ                                 | 
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | SÍ                                 | 
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | SÍ                                 | 
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | SÍ                                 | 
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | SÍ                                 | 
| azure_rm_postgresqlserver                   | -            | SÍ                         | Sí          | SÍ                                 | 
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | SÍ                                 | 
| azure_rm_postgresqldatabase                 | -            | SÍ                         | Sí          | SÍ                                 | 
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | SÍ                                 | 
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | SÍ                                 | 
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | SÍ                                 | 
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | SÍ                                 | 
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | SÍ                                 | 
| **Key Vault**                    |           |                          |                          |                                  | 
| azure_rm_keyvault                           | -            | SÍ                         | Sí          | SÍ                                 |
| azure_rm_keyvault_facts                     | -            | -                           | -            | SÍ                                 |
| azure_rm_keyvaultkey                        | -            | SÍ                         | Sí          | SÍ                                 |
| azure_rm_keyvaultsecret                     | -            | SÍ                         | Sí          | SÍ                                 |


## <a name="introduction-to-playbook-role-for-azure"></a>Introducción al rol de cuaderno de estrategias de Azure
El [rol de guion de procedimientos azure_preview_module](https://galaxy.ansible.com/Azure/azure_preview_modules/) es el rol más completo e incluye todos los módulos más recientes de Azure. Las actualizaciones y correcciones de errores se realizan de manera más oportuna que en la versión oficial de Ansible. Si usa Ansible para el aprovisionamiento de recursos de Azure, se recomienda instalar el rol azure_preview_module.

El rol de guion de procedimientos azure_preview_module se publica cada tres semanas.

## <a name="next-steps"></a>Pasos siguientes
Para más información relacionada con los roles de guion de procedimientos, consulte [Creating Reusable Playbooks](http://docs.ansible.com/ansible/latest/playbooks_reuse.html) (Creación de guiones de procedimientos reutilizables). 
