---
title: Matriz de módulo y versión de Ansible para Azure | Microsoft Docs
description: Matriz de módulo y versión de Ansible para Azure
keywords: ansible, roles, matriz, versión, azure, devops
ms.topic: reference
ms.service: ansible
author: tomarchermsft
manager: jeconnoc
ms.author: tarcher
ms.date: 04/30/2019
ms.openlocfilehash: b3396b2f4639cc7298b77810dbaafadd308d6b24
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65230979"
---
# <a name="ansible-module-and-version-matrix"></a>Matriz de módulo y versión de Ansible

Ansible incluye un conjunto de módulos para su uso en el aprovisionamiento y configuración de los recursos de Azure. Estos recursos incluyen máquinas virtuales, conjuntos de escalado, servicios y los servicios de contenedor de la red. En este artículo se enumera los distintos módulos de Ansible para Azure y las versiones de Ansible en el que se incluyen.

## <a name="ansible-modules-for-azure"></a>Módulos de Ansible para Azure

Los siguientes módulos se pueden ejecutar directamente en los hosts remotos o mediante guiones de procedimientos.

Estos módulos están disponibles desde el lanzamiento oficial de Ansible y desde los siguientes roles de cuaderno de estrategias de Microsoft.

| Módulo Ansible para Azure                   |  Ansible 2.4 |  Ansible 2.5 |  Ansible 2.6 | Ansible 2.7 | Ansible 2.8 | Rol de Ansible | 
|---------------------------------------------|--------------|--------------|-----------------------------|-------------------------------------|--------------|--------------| 
| **Proceso**                    |           |                          |                          |                            |           |           |
| azure_rm_availabilityset                    | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_availabilityset_facts              | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_deployment                         | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_deployment_facts                   | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_functionapp                        | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_functionapp_facts                  | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_image                              | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_image_facts                        | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_resource                           | -            | -                           | Sí          | Sí          | Sí          | Sí          |
| azure_rm_resource_facts                     | -            | -                           | Sí          | Sí          | Sí          | Sí          |
| azure_rm_resourcegroup                      | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_resourcegroup_facts                | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachine                     | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachine_facts               | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_virtualmachineextension           | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachineextension_facts      | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_virtualmachineimage_facts          | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachinescaleset            | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachinescaleset_facts      | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualmachinescalesetextension    | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_virtualmachinescalesetextension_facts | -            | -                        | -            | -            | Sí          | Sí          |
| azure_rm_virtualmachinescalesetinstance     | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_virtualmachinescalesetinstance_facts | -            | -                         | -            | -            | Sí          | Sí          |
| **Redes**                              |              |                             |              |              |              |              |
| azure_rm_appgateway                         | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_appgwroute                         | -            | -                           | -            | -            | -          | Sí          |
| azure_rm_appgwroute_facts                   | -            | -                           | -            | -            | -          | Sí          |
| azure_rm_appgwroutetable                    | -            | -                           | -            | -            | -          | Sí          |
| azure_rm_appgwroutetable_facts              | -            | -                           | -            | -            | -          | Sí          |
| azure_rm_applicationsecuritygroup           | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_applicationsecuritygroup_facts     | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_cdnendpoint                        | -            | -                         | -          | -            | Sí          | Sí          |
| azure_rm_cdnendpoint_facts                  | -            | -                         | -          | -            | Sí          | Sí          |
| azure_rm_cdnprofile                         | -            | -                         | -          | -            | Sí          | Sí          |
| azure_rm_cdnprofile_facts                   | -            | -                         | -          | -            | Sí          | Sí          |
| azure_rm_dnsrecordset                       | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_dnsrecordset_facts                 | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_dnszone                            | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_dnszone_facts                      | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_loadbalancer                       | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_loadbalancer_facts                 | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_networkinterface                   | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_networkinterface_facts             | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_publicipaddress                    | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_publicipaddress_facts              | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_route                              | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_routetable                         | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_routetable_facts                   | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_securitygroup                      | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_subnet                             | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_subnet_facts                       | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_trafficmanagerendpoint             | -            | -                         | -          | Sí          | Sí          | Sí          |
| azure_rm_trafficmanagerendpoint_facts       | -            | -                         | -          | Sí          | Sí          | Sí          |
| azure_rm_trafficmanagerprofile              | -            | -                         | -          | Sí          | Sí          | Sí          |
| azure_rm_trafficmanagerprofile_facts        | -            | -                         | -          | Sí          | Sí          | Sí          |
| azure_rm_virtualnetwork                     | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualnetwork_facts               | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_virtualnetworkpeering              | -            | -                         | -          | -            | Sí          | Sí          |
| **Storage**                    |           |                          |                          |                            |           |           |
| azure_rm_manageddisk                        | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_manageddisk_facts                  | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_storageaccount                     | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_storageaccount_facts               | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_storageblob                        | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| **Web**                    |           |                          |                          |                             |           |           |
| azure_rm_appserviceplan                     | -            | -                         | -          | Sí          | Sí          | Sí          |
| azure_rm_appserviceplan_facts               | -            | -                         | -          | Sí          | Sí          | Sí          |
| azure_rm_webapp                             | -            | -                         | -          | Sí          | Sí          | Sí          |
| azure_rm_webapp_facts                       | -            | -                         | -          | Sí          | Sí          | Sí          |
| azure_rm_webappslot                         | -            | -                         | -          | -            | Sí          | Sí          |
| **Contenedores**                    |           |                          |                          |                            |           |           |
| azure_rm_acs                                | Sí          | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_aks                                | -            | -                           | Sí          | Sí          | Sí          | Sí          |
| azure_rm_aks_facts                          | -            | -                           | Sí          | Sí          | Sí          | Sí          |
| azure_rm_aksversion_facts                   | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_containerinstance                  | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_containerinstance_facts            | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_containerregistry                  | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_containerregistry_facts            | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_containerregistryreplication       | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_containerregistryreplication_facts | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_containerregistrywebhook           | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_containerregistrywebhook_facts     | -            | -                           | -            | -            | Sí          | Sí          |
| **Bases de datos**                    |           |                          |                          |                             |           |           |
| azure_rm_cosmosdbaccount                    | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_cosmosdbaccount_facts              | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mariadbconfiguration               | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mariadbconfiguration_facts         | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mariadbdatabase                    | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mariadbdatabase_facts              | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mariadbfirewallrule                | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mariadbfirewallrule_facts          | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mariadbserver                      | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mariadbserver_facts                | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mysqlconfiguration                 | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mysqlconfiguration_facts           | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mysqldatabase                      | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_mysqldatabase_facts                | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_mysqlfirewallrule                  | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mysqlfirewallrule_facts            | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_mysqlserver                        | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_mysqlserver_facts                  | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_postgresqlconfiguration            | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_postgresqlconfiguration_facts      | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_postgresqldatabase                 | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_postgresqldatabase_facts           | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_postgresqlfirewallrule             | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_postgresqlfirewallrule_facts       | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_postgresqlserver                   | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_postgresqlserver_facts             | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_rediscache                         | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_rediscache_facts                   | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_rediscachefirewallrule             | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_sqldatabase                        | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_sqldatabase_facts                  | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_sqlelasticpool                     | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_sqlelasticpool_facts               | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_sqlfirewallrule                    | -            | -                           | -            | Sí          | Sí          | Sí          |
| azure_rm_sqlfirewallrule_facts              | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_sqlserver                          | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_sqlserver_facts                    | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| **Analytics**                    |           |                          |                          |                             |           |           |
| azure_rm_hdinsightcluster                   | -            | -                           | -            | -            | Sí          | Sí          |
| **Integración**                    |           |                          |                          |                             |           |           |
| azure_rm_servicebus                         | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_servicebus_facts                   | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_servicebusqueue                    | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_servicebussaspolicy                | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_servicebustopic                    | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_servicebustopicsubscription        | -            | -                           | -            | -            | Sí          | Sí          |
| **Seguridad**                    |           |                          |                          |                             |           |           |
| azure_rm_keyvault                           | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_keyvault_facts                     | -            | -                           | -              | -          | Sí          | Sí          |
| azure_rm_keyvaultkey                        | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_keyvaultsecret                     | -            | Sí                         | Sí          | Sí          | Sí          | Sí          |
| azure_rm_roleassignment                     | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_roleassignment_facts               | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_roledefinition                     | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_roledefinition_facts               | -            | -                           | -            | -            | Sí          | Sí          |
| **DevOps**               |           |                          |                          |                             |           |           |
| azure_rm_devtestlab                         | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlab_facts                   | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabarmtemplate_facts        | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabartifact_facts           | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabartifactsource           | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabartifactsource_facts     | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabcustomimage              | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabenvironment              | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabpolicy                   | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabschedule                 | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabvirtualmachine           | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabvirtualmachine_facts | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabvirtualnetwork           | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_devtestlabvirtualnetwork_facts     | -            | -                           | -            | -            | Sí          | Sí          |
| **Azure Monitor**          |           |                          |                          |                             |           |           |
| azure_rm_autoscale                  | -            | -                         | -          | Sí          | Sí          | Sí          |
| azure_rm_autoscale_facts            | -            | -                         | -          | Sí          | Sí          | Sí          |
| azure_rm_loganalyticsworkspace              | -            | -                           | -            | -            | Sí          | Sí          |
| azure_rm_loganalyticsworkspace_facts        | -            | -                           | -            | -            | Sí          | Sí          |

## <a name="introduction-to-playbook-role-for-azure"></a>Introducción al rol de cuaderno de estrategias de Azure

El [rol azure_preview_module](https://galaxy.ansible.com/Azure/azure_preview_modules/) incluye todos los módulos de Azure más recientes. Las actualizaciones y correcciones de errores se realizan de manera más oportuna que en la versión oficial de Ansible. Si usa Ansible para aprovisionamiento de recursos de Azure, le recomendamos que instale el `azure_preview_module` rol.

El `azure_preview_module` rol se publica cada tres semanas.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de los roles de guión de procedimientos, consulte [crear playbooks reutilizables](https://docs.ansible.com/ansible/latest/playbooks_reuse.html). 
