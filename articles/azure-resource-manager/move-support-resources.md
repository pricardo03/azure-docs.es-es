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
ms.openlocfilehash: dfed8ce93a97b31380b44f0ac7057c0cff60901e
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55497288"
---
# <a name="move-operation-support-for-resources"></a>Compatibilidad con la operación de traslado para recursos

En este artículo se indica si un tipo de recurso de Azure es compatible con la operación de traslado. Aunque un tipo de recurso sea compatible con la operación de traslado, puede haber condiciones que impidan que el recurso se traslade. Para obtener información sobre las condiciones que afectan a las operaciones de desplazamiento, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](resource-group-move-resources.md).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="find-resource-provider-and-resource-type"></a>Buscar el proveedor de recursos y el tipo de recurso

Para determinar si se puede mover un recurso, debe buscar su proveedor de recursos y el tipo de recurso.

Para PowerShell, use:

```azurepowershell-interactive
Get-AzResource -ResourceGroupName demogroup | Select Name, ResourceType | Format-table
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
| servers | Sí | Sí |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | --------------- | ----------- |
| service | SÍ | Sí |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | --------------- | ----------- |
| policyassignments | Sin  | Sin  |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| automationaccounts | Sí | Sí |
| automationaccounts/configurations | Sí | Sí |
| automationaccounts/runbooks | Sí | Sí |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| b2cdirectories | Sí | Sí |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| registrations | Sí | Sí |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| backupvault | Sin  | Sin  |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| batchaccounts | Sí | Sí |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| mapapis | Sin  | Sin  |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| biztalk | Sí | Sí |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| blueprintassignments | Sin  | Sin  |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| botservices | Sí | Sí |

## <a name="microsoftcache"></a>Microsoft.Cache
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| redis | Sí | Sí |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| perfiles | Sí | Sí |
| profiles/endpoints | Sí | Sí |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| certificateorders | Sí | Sí |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| domainnames | Sí | Sin  |
| virtualmachines | Sí | Sin  |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| networksecuritygroups | Sin  | Sin  |
| reservedips | Sin  | Sin  |
| virtualnetworks | Sin  | Sin  |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| storageaccounts | Sí | Sin  |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | Sí | Sí |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| availabilitysets | Sí | Sí |
| disks | Sí | Sí |
| galleries | Sin  | Sin  |
| galleries/images | Sin  | Sin  |
| galleries/images/versions | Sin  | Sin  |
| images | Sí | Sí |
| restorepointcollections | Sin  | Sin  |
| sharedvmimages | Sin  | Sin  |
| sharedvmimages/versions | Sin  | Sin  |
| snapshots | Sí | Sí |
| virtualmachines | Sí | Sí |
| virtualmachines/extensions | Sí | Sí |
| virtualmachinescalesets | Sí | Sí |

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
| registries | Sí | Sí |
| registries/buildtasks | Sí | Sí |
| registries/replications | Sin  | Sin  |
| registries/tasks | Sí | Sí |
| registries/webhooks | Sí | Sí |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| containerservices | Sin  | Sin  |
| managedclusters | Sin  | Sin  |
| openshiftmanagedclusters | Sin  | Sin  |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| applications | Sí | Sí |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| conectores | Sí | Sí |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| hubs | Sí | Sí |

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
| workspaces | Sin  | Sin  |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| catalogs | Sí | Sí |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| datafactories | Sí | Sí |
| factories | Sí | Sí |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| datalakeaccounts | Sin  | Sin  |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | Sí | Sí |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | Sí | Sí |

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
| servers | Sí | Sí |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| servergroups | Sin  | Sin  |
| servers | Sí | Sí |

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
| iothubs | Sí | Sí |
| provisioningservices | Sí | Sí |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| labcenters | Sin  | Sin  |
| labs | Sí | Sin  |
| labs/servicerunners | Sí | Sí |
| labs/virtualmachines | Sí | Sin  |
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
| databaseaccounts | Sí | Sí |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| domains | Sí | Sí |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| topics | Sí | Sí |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| clusters | Sí | Sí |
| namespaces | Sí | Sí |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| hanainstances | Sí | Sí |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| dedicatedhsms | Sin  | Sin  |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| clusters | Sí | Sí |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| jobs | Sí | Sí |

## <a name="microsoftinsights"></a>microsoft.insights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| actiongroups | Sí | Sí |
| activitylogalerts | Sin  | Sin  |
| alertrules | Sí | Sí |
| autoscalesettings | Sí | Sí |
| components | Sí | Sí |
| metricalerts | Sin  | Sin  |
| scheduledqueryrules | Sí | Sí |
| webtests | Sí | Sí |
| workbooks | Sí | Sí |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| iotapps | Sí | Sí |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| vaults | Sí | Sí |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| clusters | Sí | Sí |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| labaccounts | Sí | Sí |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | Sí | Sí |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | Sí | Sí |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| integrationaccounts | Sí | Sí |
| workflows | Sí | Sí |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| commitmentplans | Sí | Sí |
| webservices | Sí | Sin  |
| workspaces | Sí | Sí |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| operationalizationclusters | Sí | Sí |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | Sí | Sí |
| accounts/workspaces | Sí | Sí |
| accounts/workspaces/projects | Sí | Sí |
| teamaccounts | Sí | Sí |
| teamaccounts/workspaces | Sí | Sí |
| teamaccounts/workspaces/projects | Sí | Sí |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | Sí | Sí |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| workspaces | Sí | Sí |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| userassignedidentities | Sí | Sí |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| accounts | Sí | Sí |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| classicdevservices | Sin  | Sin  |

## <a name="microsoftmedia"></a>Microsoft.Media
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| mediaservices | Sí | Sí |
| mediaservices/liveevents | Sí | Sí |
| mediaservices/streamingendpoints | Sí | Sí |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| projects | Sin  | Sin  |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| applicationgateways | Sin  | Sin  |
| applicationsecuritygroups | Sí | Sí |
| azurefirewalls | Sin  | Sin  |
| connections | Sí | Sí |
| ddosprotectionplans | Sin  | Sin  |
| dnszones | Sí | Sí |
| expressroutecircuits | Sin  | Sin  |
| expressroutecrossconnections | Sin  | Sin  |
| expressroutegateways | Sin  | Sin  |
| expressrouteports | Sin  | Sin  |
| frontdoors | Sí | Sí |
| frontdoorwebapplicationfirewallpolicies | Sí | Sí |
| interfaceendpoints | Sin  | Sin  |
| loadbalancers | Sí | Sí |
| localnetworkgateways | Sí | Sí |
| networkintentpolicies | Sí | Sí |
| networkinterfaces | Sí | Sí |
| networkprofiles | Sin  | Sin  |
| networksecuritygroups | Sí | Sí |
| networkwatchers | Sí | Sí |
| networkwatchers/connectionmonitors | Sí | Sí |
| networkwatchers/lenses | Sí | Sí |
| networkwatchers/pingmeshes | Sí | Sí |
| publicipaddresses | Sí | Sí |
| publicipprefixes | Sí | Sí |
| routefilters | Sin  | Sin  |
| routetables | Sí | Sí |
| serviceendpointpolicies | Sí | Sí |
| trafficmanagerprofiles | Sí | Sí |
| virtualhubs | Sí | Sí |
| virtualnetworkgateways | Sí | Sí |
| virtualnetworks | Sí | Sí |
| virtualnetworktaps | Sin  | Sin  |
| virtualwans | Sí | Sí |
| vpngateways | Sí | Sí |
| vpnsites | Sí | Sí |
| webapplicationfirewallpolicies | Sí | Sí |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| namespaces | Sí | Sí |
| namespaces/notificationhubs | Sí | Sí |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| workspaces | Sí | Sí |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| managementconfigurations | Sí | Sí |
| solutions | Sí | SÍ |
| views | SÍ | Sí |

## <a name="microsoftportal"></a>Microsoft.Portal
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| dashboards | Sí | Sí |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| workspacecollections | Sí | Sí |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| capacities | Sí | Sí |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| vaults | Sí | Sí |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| namespaces | Sí | Sí |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| applications | Sí | Sin  |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| flows | Sí | Sí |
| jobcollections | Sí | Sí |

## <a name="microsoftsearch"></a>Microsoft.Search
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| searchservices | Sí | Sí |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| namespaces | Sí | Sí |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| clusters | Sí | Sí |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| applications | Sí | Sí |
| networks | Sí | Sí |
| volumes | Sí | Sí |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| signalr | Sí | Sí |

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
| applications | Sin  | Sin  |

## <a name="microsoftsql"></a>Microsoft.Sql
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| managedinstances | Sí | Sí |
| managedinstances/databases | Sí | Sí |
| servers | Sí | Sí |
| servers/databases | Sí | Sí |
| servers/elasticpools | Sí | Sí |
| virtualclusters | Sí | Sí |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| storageaccounts | Sí | Sí |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| storagesyncservices | Sí | Sí |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| managers | Sin  | Sin  |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| streamingjobs | Sí | Sí |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| environments | Sí | Sí |
| environments/eventsources | Sí | Sí |
| environments/referencedatasets | Sí | Sí |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| account | SÍ | Sí |
| account/extension | Sí | Sí |
| account/project | Sí | Sí |

## <a name="microsoftweb"></a>Microsoft.Web
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| certificates | Sin  | Sí |
| classicmobileservices | Sin  | Sin  |
| connectiongateways | Sí | Sí |
| connections | Sí | Sí |
| customapis | Sí | Sí |
| hostingenvironments | Sin  | Sin  |
| serverfarms | Sí | Sí |
| sites | Sí | Sí |
| sites/premieraddons | Sí | Sí |
| sites/slots | Sí | Sí |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | -------------- | ------------ |
| deviceservices | Sí | Sí |


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
