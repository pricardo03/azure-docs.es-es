---
title: Recursos de Azure sin límite de recuento de 800
description: Aquí se enumeran los tipos de recursos de Azure que pueden tener más de 800 instancias en un grupo de recursos.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 10/4/2019
ms.author: tomfitz
ms.openlocfilehash: 6677290999049dead40f39e9a840735810c7c763
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973324"
---
# <a name="resources-not-limited-to-800-instances-per-resource-group"></a>Recursos no limitados a 800 instancias por grupo de recursos

De forma predeterminada, puede implementar hasta 800 instancias de un tipo de recurso en cada grupo de recursos. Sin embargo, algunos tipos de recursos están exentos del límite de 800 instancias. En este artículo se enumeran los tipos de recursos de Azure que pueden tener más de 800 instancias en un grupo de recursos. Todos los demás tipos de recursos se limitan a 800 instancias.

Para usar algunos tipos de recursos, debe ponerse en contacto con el servicio de soporte técnico para que se elimine el límite de 800 instancias. Esos tipos de recursos se indican en este artículo.


## <a name="microsoftautomation"></a>Microsoft.Automation

* automationAccounts

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

* registrations
* registrations/customerSubscriptions
* registrations/products

## <a name="microsoftbotservice"></a>Microsoft.BotService

* botServices: póngase en contacto con el soporte técnico para ampliar el límite.

## <a name="microsoftcompute"></a>Microsoft.Compute

* disks
* images
* snapshots
* virtualMachines

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

* containerGroups

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

* registries/buildTasks
* registries/buildTasks/listSourceRepositoryProperties
* registries/buildTasks/steps
* registries/buildTasks/steps/listBuildArguments
* registries/eventGridFilters
* registries/replications
* registries/tasks
* registries/webhooks

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

* servers

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

* servers

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

* serverGroups
* servers
* serversv2

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

* services

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

* guestConfigurationAssignments
* software
* softwareUpdateProfile
* softwareUpdates

## <a name="microsoftlogic"></a>Microsoft.Logic

* integrationAccounts
* workflows

## <a name="microsoftnetapp"></a>Microsoft.NetApp

* netAppAccounts
* netAppAccounts/capacityPools
* netAppAccounts/capacityPools/volumes
* netAppAccounts/capacityPools/volumes/mountTargets
* netAppAccounts/capacityPools/volumes/snapshots

## <a name="microsoftnetwork"></a>Microsoft.Network

* applicationGatewayWebApplicationFirewallPolicies
* applicationSecurityGroups
* bastionHosts
* ddosProtectionPlans
* dnszones
* dnszones/A
* dnszones/AAAA
* dnszones/CAA
* dnszones/CNAME
* dnszones/MX
* dnszones/NS
* dnszones/PTR
* dnszones/SOA
* dnszones/SRV
* dnszones/TXT
* dnszones/all
* dnszones/recordsets
* networkIntentPolicies
* networkInterfaces
* privateDnsZones
* privateDnsZones/A
* privateDnsZones/AAAA
* privateDnsZones/CNAME
* privateDnsZones/MX
* privateDnsZones/PTR
* privateDnsZones/SOA
* privateDnsZones/SRV
* privateDnsZones/TXT
* privateDnsZones/all
* privateDnsZones/virtualNetworkLinks
* privateEndpoints
* privateLinkServices
* publicIPAddresses: póngase en contacto con el soporte técnico para ampliar el límite.
* serviceEndpointPolicies
* trafficmanagerprofiles
* virtualNetworkTaps

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk

* rootResources

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

* workspaceCollections: póngase en contacto con el soporte técnico para ampliar el límite.

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

* jobcollections

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

* applications
* containerGroups
* gateways
* networks
* secrets
* volumes

## <a name="microsoftstorage"></a>Microsoft.Storage

* storageAccounts

## <a name="microsoftweb"></a>Microsoft.Web

* apiManagementAccounts/apis
* sites

## <a name="next-steps"></a>Pasos siguientes

Para obtener una lista completa de las cuotas y los límites, consulte [Suscripción de Azure y límites de servicio, cuotas y restricciones](../azure-subscription-service-limits.md).
