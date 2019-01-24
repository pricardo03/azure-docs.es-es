---
title: Compatibilidad con la operación de traslado por tipo de recurso de Azure | Microsoft Docs
description: Enumera los tipos de recursos de Azure que se pueden trasladar a un nuevo grupo de recursos o suscripción.
services: azure-resource-manager
documentationcenter: ''
author: tfitzmac
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: tomfitz
ms.openlocfilehash: c16a0eeb674e712ec1c3678a2e0002a8ddcfc329
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54464721"
---
# <a name="move-operation-support-for-resources"></a>Compatibilidad con la operación de traslado para recursos

En este artículo se indica si un tipo de recurso de Azure es compatible con la operación de traslado. Aunque un tipo de recurso sea compatible con la operación de traslado, puede haber condiciones que impidan que el recurso se traslade. Para obtener información sobre las condiciones que afectan a las operaciones de desplazamiento, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](resource-group-move-resources.md).

## <a name="find-resource-provider-and-resource-type"></a>Buscar el proveedor de recursos y el tipo de recurso

Para determinar si se puede mover un recurso, debe buscar su proveedor de recursos y el tipo de recurso.

Para PowerShell, use:

```azurepowershell-interactive
Get-AzureRmResource -ResourceGroupName demogroup | Select Name, ResourceType | Format-table
```

Para la CLI de Azure, utilice:

```azurecli-interactive
az resource list -g demogroup --query '[].{name:name, resourceType:type}' --output table
```

Se devuelve el tipo de recurso en formato `<resource-provider>/<resource-type-name>`. Por lo tanto, el valor `Microsoft.OperationalInsights/workspaces` tiene como proveedor de recursos **Microsoft.OperationalInsights** y como nombre de tipo de recurso **áreas de trabajo**.

Después de encontrar el proveedor de recursos y el tipo de recurso, use las tablas que se incluyen en este artículo para determinar si el tipo de recurso es compatible con la operación de traslado.

## <a name="microsoftaad"></a>Microsoft.AAD

| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | --------------- | ----------- |
| domainservices | Sin  | Sin  |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| servers | SÍ | SÍ |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | --------------- | ----------- |
| Azure | SÍ | SÍ |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | --------------- | ----------- |
| policyassignments | Sin  | Sin  |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| automationaccounts | SÍ | SÍ |
| automationaccounts/configurations | SÍ | SÍ |
| automationaccounts/runbooks | SÍ | SÍ |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| b2cdirectories | SÍ | SÍ |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| registrations | SÍ | SÍ |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| backupvault | Sin  | Sin  |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| batchaccounts | SÍ | SÍ |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| mapapis | Sin  | Sin  |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| biztalk | SÍ | SÍ |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| blueprintassignments | Sin  | Sin  |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| botservices | SÍ | SÍ |

## <a name="microsoftcache"></a>Microsoft.Cache
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| redis | SÍ | SÍ |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| perfiles | SÍ | SÍ |
| profiles/endpoints | SÍ | SÍ |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| certificateorders | SÍ | SÍ |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| domainnames | SÍ | Sin  |
| virtualmachines | SÍ | Sin  |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| networksecuritygroups | Sin  | Sin  |
| reservedips | Sin  | Sin  |
| virtualnetworks | Sin  | Sin  |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| storageaccounts | SÍ | Sin  |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | SÍ | SÍ |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| availabilitysets | SÍ | SÍ |
| disks | SÍ | SÍ |
| galleries | Sin  | Sin  |
| galleries/images | Sin  | Sin  |
| galleries/images/versions | Sin  | Sin  |
| images | SÍ | SÍ |
| restorepointcollections | Sin  | Sin  |
| sharedvmimages | Sin  | Sin  |
| sharedvmimages/versions | Sin  | Sin  |
| snapshots | SÍ | SÍ |
| virtualmachines | SÍ | SÍ |
| virtualmachines/extensions | SÍ | SÍ |
| virtualmachinescalesets | SÍ | SÍ |

## <a name="microsoftcontainer"></a>Microsoft.Container
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| containergroups | Sin  | Sin  |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| containergroups | Sin  | Sin  |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| registries | SÍ | SÍ |
| registries/buildtasks | SÍ | SÍ |
| registries/replications | Sin  | Sin  |
| registries/tasks | SÍ | SÍ |
| registries/webhooks | SÍ | SÍ |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| containerservices | Sin  | Sin  |
| managedclusters | Sin  | Sin  |
| openshiftmanagedclusters | Sin  | Sin  |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| aplicaciones de escala de web | SÍ | SÍ |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| conectores | SÍ | SÍ |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| hubs | SÍ | SÍ |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| jobs | Sin  | Sin  |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| databoxedgedevices | Sin  | Sin  |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| áreas de trabajo | Sin  | Sin  |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| catalogs | SÍ | SÍ |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| datafactories | SÍ | SÍ |
| factories | SÍ | SÍ |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| datalakeaccounts | Sin  | Sin  |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | SÍ | SÍ |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | SÍ | SÍ |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| services | Sin  | Sin  |
| services/projects | Sin  | Sin  |
| slots | Sin  | Sin  |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| servers | Sin  | Sin  |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| servers | SÍ | SÍ |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| servergroups | Sin  | Sin  |
| servers | SÍ | SÍ |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| artifactsources | Sin  | Sin  |
| rollouts | Sin  | Sin  |
| servicetopologies | Sin  | Sin  |
| servicetopologies/services | Sin  | Sin  |
| servicetopologies/services/serviceunits | Sin  | Sin  |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| iothubs | SÍ | SÍ |
| provisioningservices | SÍ | SÍ |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| labcenters | Sin  | Sin  |
| labs | SÍ | Sin  |
| labs/servicerunners | SÍ | SÍ |
| labs/virtualmachines | SÍ | Sin  |
| schedules | Sin  | Sin  |

## <a name="microsoftdns"></a>microsoft.dns
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| dnszones | Sin  | Sin  |
| dnszones/a | Sin  | Sin  |
| dnszones/aaaa | Sin  | Sin  |
| dnszones/cname | Sin  | Sin  |
| dnszones/mx | Sin  | Sin  |
| dnszones/ptr | Sin  | Sin  |
| dnszones/srv | Sin  | Sin  |
| dnszones/txt | Sin  | Sin  |
| trafficmanagerprofiles | Sin  | Sin  |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| databaseaccounts | SÍ | SÍ |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| domains | SÍ | SÍ |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| topics | SÍ | SÍ |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| clusters | SÍ | SÍ |
| namespaces | SÍ | SÍ |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| hanainstances | SÍ | SÍ |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| dedicatedhsms | Sin  | Sin  |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| clusters | SÍ | SÍ |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| jobs | SÍ | SÍ |

## <a name="microsoftinsights"></a>microsoft.insights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| actiongroups | SÍ | SÍ |
| activitylogalerts | Sin  | Sin  |
| alertrules | SÍ | SÍ |
| autoscalesettings | SÍ | SÍ |
| components | SÍ | SÍ |
| metricalerts | Sin  | Sin  |
| scheduledqueryrules | SÍ | SÍ |
| webtests | SÍ | SÍ |
| workbooks | SÍ | SÍ |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| iotapps | SÍ | SÍ |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| vaults | SÍ | SÍ |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| clusters | SÍ | SÍ |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| labaccounts | SÍ | SÍ |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | SÍ | SÍ |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | SÍ | SÍ |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| integrationaccounts | SÍ | SÍ |
| workflows | SÍ | SÍ |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| commitmentplans | SÍ | SÍ |
| webservices | SÍ | Sin  |
| áreas de trabajo | SÍ | SÍ |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| operationalizationclusters | SÍ | SÍ |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | SÍ | SÍ |
| accounts/workspaces | SÍ | SÍ |
| accounts/workspaces/projects | SÍ | SÍ |
| teamaccounts | SÍ | SÍ |
| teamaccounts/workspaces | SÍ | SÍ |
| teamaccounts/workspaces/projects | SÍ | SÍ |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | SÍ | SÍ |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| áreas de trabajo | SÍ | SÍ |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| userassignedidentities | SÍ | SÍ |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | SÍ | SÍ |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| classicdevservices | Sin  | Sin  |

## <a name="microsoftmedia"></a>Microsoft.Media
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| mediaservices | SÍ | SÍ |
| mediaservices/liveevents | SÍ | SÍ |
| mediaservices/streamingendpoints | SÍ | SÍ |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| projects | Sin  | Sin  |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| applicationgateways | Sin  | Sin  |
| applicationsecuritygroups | SÍ | SÍ |
| azurefirewalls | Sin  | Sin  |
| connections | SÍ | SÍ |
| ddosprotectionplans | Sin  | Sin  |
| dnszones | SÍ | SÍ |
| expressroutecircuits | Sin  | Sin  |
| expressroutecrossconnections | Sin  | Sin  |
| expressroutegateways | Sin  | Sin  |
| expressrouteports | Sin  | Sin  |
| frontdoors | SÍ | SÍ |
| frontdoorwebapplicationfirewallpolicies | SÍ | SÍ |
| interfaceendpoints | Sin  | Sin  |
| loadbalancers | SÍ | SÍ |
| localnetworkgateways | SÍ | SÍ |
| networkintentpolicies | SÍ | SÍ |
| networkinterfaces | SÍ | SÍ |
| networkprofiles | Sin  | Sin  |
| networksecuritygroups | SÍ | SÍ |
| networkwatchers | SÍ | SÍ |
| networkwatchers/connectionmonitors | SÍ | SÍ |
| networkwatchers/lenses | SÍ | SÍ |
| networkwatchers/pingmeshes | SÍ | SÍ |
| publicipaddresses | SÍ | SÍ |
| publicipprefixes | SÍ | SÍ |
| routefilters | Sin  | Sin  |
| routetables | SÍ | SÍ |
| serviceendpointpolicies | SÍ | SÍ |
| trafficmanagerprofiles | SÍ | SÍ |
| virtualhubs | SÍ | SÍ |
| virtualnetworkgateways | SÍ | SÍ |
| virtualnetworks | SÍ | SÍ |
| virtualnetworktaps | Sin  | Sin  |
| virtualwans | SÍ | SÍ |
| vpngateways | SÍ | SÍ |
| vpnsites | SÍ | SÍ |
| webapplicationfirewallpolicies | SÍ | SÍ |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| namespaces | SÍ | SÍ |
| namespaces/notificationhubs | SÍ | SÍ |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| áreas de trabajo | SÍ | SÍ |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| managementconfigurations | SÍ | SÍ |
| solutions | SÍ | SÍ |
| vistas | SÍ | SÍ |

## <a name="microsoftportal"></a>Microsoft.Portal
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| dashboards | SÍ | SÍ |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| workspacecollections | SÍ | SÍ |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| capacities | SÍ | SÍ |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| vaults | SÍ | SÍ |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| namespaces | SÍ | SÍ |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| aplicaciones de escala de web | SÍ | Sin  |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| flows | SÍ | SÍ |
| jobcollections | SÍ | SÍ |

## <a name="microsoftsearch"></a>Microsoft.Search
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| searchservices | SÍ | SÍ |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| namespaces | SÍ | SÍ |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| clusters | SÍ | SÍ |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| aplicaciones de escala de web | SÍ | SÍ |
| networks | SÍ | SÍ |
| volumes | SÍ | SÍ |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| signalr | SÍ | SÍ |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| siterecoveryvault | Sin  | Sin  |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| appliancedefinitions | Sin  | Sin  |
| appliances | Sin  | Sin  |
| applicationdefinitions | Sin  | Sin  |
| aplicaciones de escala de web | Sin  | Sin  |

## <a name="microsoftsql"></a>Microsoft.Sql
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| managedinstances | SÍ | SÍ |
| managedinstances/databases | SÍ | SÍ |
| servers | SÍ | SÍ |
| servers/databases | SÍ | SÍ |
| servers/elasticpools | SÍ | SÍ |
| virtualclusters | SÍ | SÍ |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| storageaccounts | SÍ | SÍ |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| storagesyncservices | SÍ | SÍ |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| managers | Sin  | Sin  |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| streamingjobs | SÍ | SÍ |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| environments | SÍ | SÍ |
| environments/eventsources | SÍ | SÍ |
| environments/referencedatasets | SÍ | SÍ |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| cuenta | SÍ | SÍ |
| account/extension | SÍ | SÍ |
| account/project | SÍ | SÍ |

## <a name="microsoftweb"></a>Microsoft.Web
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| certificates | Sin  | SÍ |
| classicmobileservices | Sin  | Sin  |
| connectiongateways | SÍ | SÍ |
| connections | SÍ | SÍ |
| customapis | SÍ | SÍ |
| hostingenvironments | Sin  | Sin  |
| serverfarms | SÍ | SÍ |
| sites | SÍ | SÍ |
| sites/premieraddons | SÍ | SÍ |
| sites/slots | SÍ | SÍ |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| deviceservices | SÍ | SÍ |


## <a name="third-party-services"></a>Servicios de terceros

Actualmente los servicios de terceros no son compatibles con la operación de traslado. Estos proveedores de recursos son:

* 84codes.CloudAMQP
* AppDynamics.APM
* Aspera.Transfers
* Auth0.Cloud
* Citrix.Cloud
* Citrix.Services
* CloudSimple.PrivateCloudIaaS
* Cloudyn.Analytics
* Conexlink.MyCloudIT
* Crypteron.DataSecurity
* Dynatrace.DynatraceSaaS
* Dynatrace.Ruxit
* LiveArena.Broadcast
* Lombiq.DotNest
* Mailjet.Email
* Myget.PackageManagement
* NewRelic.APM
* nuubit.nextgencdn
* Paraleap.CloudMonix
* Pokitdok.Platform
* RavenHq.Db
* Raygun.CrashReporting
* RevAPM.MobileCDN
* Sendgrid.Email
* Sparkpost.Basic
* stackify.retrace
* SuccessBricks.ClearDB
* TrendMicro.DeepSecurity
* U2uconsult.TheIdentityHub


## <a name="next-steps"></a>Pasos siguientes

* Para ver los comandos para trasladar recursos, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](resource-group-move-resources.md).
