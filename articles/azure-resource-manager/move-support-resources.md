---
title: Compatibilidad con la operación de traslado por tipo de recurso de Azure
description: Enumera los tipos de recursos de Azure que se pueden trasladar a un nuevo grupo de recursos o suscripción.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 6/6/2019
ms.author: tomfitz
ms.openlocfilehash: 314b28edbd5770186d96fb2a2b203f26ff27bda0
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66752355"
---
# <a name="move-operation-support-for-resources"></a>Compatibilidad con la operación de traslado para recursos
En este artículo se indica si un tipo de recurso de Azure es compatible con la operación de traslado. Aunque un tipo de recurso sea compatible con la operación de traslado, puede haber condiciones que impidan que el recurso se traslade. Para obtener información sobre las condiciones que afectan a las operaciones de desplazamiento, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](resource-group-move-resources.md).

Para obtener los mismos datos como un archivo de valores separados por comas, descargue [move-support-resources.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/move-support-resources.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| domainservices | Sin | Sin |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| tenants | Sin | Sin |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| actionrules | Sí | Sí |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| servers | Sí | Sí |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| service | SÍ | Sí |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| configurationstores | Sí | Sí |

## <a name="microsoftappservice"></a>Microsoft.AppService
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| apiapps | No | Sin |
| appidentities | Sin | Sin |
| gateways | Sin | Sin |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| policyassignments | Sin | Sin |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| automationaccounts | Sí | Sí |
| automationaccounts/configurations | Sí | Sí |
| automationaccounts/runbooks | Sí | Sí |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| b2cdirectories | Sí | Sí |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| registrations | Sí | Sí |

## <a name="microsoftbackup"></a>Microsoft.Backup
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| backupvault | No | No |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| batchaccounts | Sí | Sí |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| clusters | Sin | No |
| fileservers | No | No |
| jobs | Sin | No |
| workspaces | No | No |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| mapapis | Sin | Sin |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| biztalk | Sí | Sí |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| blockchainmembers | Sí | Sí |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| blueprintassignments | Sin | Sin |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| botservices | Sí | Sí |

## <a name="microsoftcache"></a>Microsoft.Cache
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| redis | Sí | Sí |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| perfiles | Sí | Sí |
| profiles/endpoints | Sí | Sí |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| certificateorders | Sí | Sí |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| domainnames | Sí | Sin |
| virtualmachines | Sí | Sin |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| networksecuritygroups | Sin | Sin |
| reservedips | No | Sin |
| virtualnetworks | Sin | No |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| storageaccounts | Sí | No |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Sí | Sí |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| availabilitysets | Sí | Sí |
| disks | Sí | Sí |
| galleries | Sin | Sin |
| galleries/images | No | Sin |
| galleries/images/versions | No | Sin |
| hostgroups | No | Sin |
| hostgroups/hosts | Sin | Sin |
| images | Sí | Sí |
| proximityplacementgroups | Sin | No |
| restorepointcollections | Sin | Sin |
| sharedvmimages | Sin | Sin |
| sharedvmimages/versions | Sin | No |
| snapshots | Sí | Sí |
| virtualmachines | Sí | Sí |
| virtualmachines/extensions | Sí | Sí |
| virtualmachinescalesets | Sí | Sí |

## <a name="microsoftcontainer"></a>Microsoft.Container
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| containergroups | Sin | No |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| containergroups | Sin | Sin |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| registries | Sí | Sí |
| registries/buildtasks | Sí | Sí |
| registries/replications | Sí | Sí |
| registries/tasks | Sí | Sí |
| registries/webhooks | Sí | Sí |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| containerservices | Sin | No |
| managedclusters | Sin | Sin |
| openshiftmanagedclusters | No | No |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| applications | Sí | Sí |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Sin | Sin |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| conectores | Sí | Sí |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| hubs | Sí | Sí |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| jobs | No | Sin |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| databoxedgedevices | Sin | No |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| workspaces | Sin | Sin |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| catalogs | Sí | Sí |
| datacatalogs | No | No |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| connectionmanagers | Sin | Sin |

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| packages | Sin | No |
| plans | Sin | No |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| datafactories | Sí | Sí |
| factories | Sí | Sí |

## <a name="microsoftdatalake"></a>Microsoft.DataLake
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| datalakeaccounts | Sin | Sin |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Sí | Sí |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Sí | Sí |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| services | Sin | No |
| services/projects | Sin | Sin |
| slots | No | Sin |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| servers | Sí | Sí |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| servers | Sí | Sí |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| servergroups | No | Sin |
| servers | Sí | Sí |
| serversv2 | Sí | Sí |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| artifactsources | Sí | Sí |
| rollouts | Sí | Sí |
| servicetopologies | Sí | Sí |
| servicetopologies/services | Sí | Sí |
| servicetopologies/services/serviceunits | Sí | Sí |
| steps | Sí | Sí |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| elasticpools | Sin | Sin |
| elasticpools/iothubtenants | Sin | No |
| iothubs | Sí | Sí |
| provisioningservices | Sí | Sí |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| controllers | Sin | No |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| labcenters | No | Sin |
| labs | Sí | Sin |
| laboratorios y entornos | Sí | Sí |
| labs/servicerunners | Sí | Sí |
| labs/virtualmachines | Sí | No |
| schedules | Sí | Sí |

## <a name="microsoftdns"></a>microsoft.dns
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| dnszones | Sin | Sin |
| dnszones/a | No | Sin |
| dnszones/aaaa | Sin | No |
| dnszones/cname | Sin | Sin |
| dnszones/mx | Sin | Sin |
| dnszones/ptr | Sin | Sin |
| dnszones/srv | Sin | No |
| dnszones/txt | Sin | Sin |
| trafficmanagerprofiles | Sin | No |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| databaseaccounts | Sí | Sí |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| domains | Sí | Sí |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| services | Sí | Sí |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| domains | Sí | Sí |
| topics | Sí | Sí |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| clusters | Sí | Sí |
| namespaces | Sí | Sí |

## <a name="microsoftgenomics"></a>Microsoft.Genomics
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | No | Sin |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| hanainstances | Sí | Sí |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| clusters | Sí | Sí |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| services | Sí | Sí |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| Máquinas | Sin | No |

## <a name="microsofthybriddata"></a>Microsoft.HybridData
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| datamanagers | Sí | Sí |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| jobs | Sí | Sí |

## <a name="microsoftinsights"></a>microsoft.insights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Sin | Sin |
| actiongroups | Sí | Sí |
| activitylogalerts | Sin | Sin |
| alertrules | Sí | Sí |
| autoscalesettings | Sí | Sí |
| components | Sí | Sí |
| guestdiagnosticsettings | No | Sin |
| metricalerts | Sin | No |
| notificationgroups | Sin | Sin |
| notificationrules | Sin | No |
| scheduledqueryrules | Sí | Sí |
| webtests | Sí | Sí |
| workbooks | Sí | Sí |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| iotapps | Sí | Sí |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| checknameavailability | Sí | Sí |
| graph | Sí | Sí |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| hsmpools | No | Sin |
| vaults | Sí | Sí |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| clusters | Sí | Sí |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| labaccounts | Sí | Sí |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Sí | Sí |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Sin | No |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| hostingenvironments | Sin | Sin |
| integrationaccounts | Sí | Sí |
| integrationserviceenvironments | Sin | Sin |
| isolatedenvironments | No | No |
| workflows | Sí | Sí |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| commitmentplans | Sí | Sí |
| webservices | Sí | No |
| workspaces | Sí | Sí |

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| operationalizationclusters | Sí | Sí |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Sin | Sin |
| accounts/workspaces | Sin | Sin |
| accounts/workspaces/projects | Sin | No |
| teamaccounts | Sin | Sin |
| teamaccounts/workspaces | Sin | Sin |
| teamaccounts/workspaces/projects | Sin | No |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Sí | Sí |

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| hostingaccounts | No | Sin |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| workspaces | Sin | No |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| userassignedidentities | Sin | Sin |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | Sí | Sí |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| classicdevservices | Sin | Sin |

## <a name="microsoftmedia"></a>Microsoft.Media
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| mediaservices | Sí | Sí |
| mediaservices/liveevents | Sí | Sí |
| mediaservices/streamingendpoints | Sí | Sí |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| assessmentprojects | No | Sin |
| migrateprojects | Sin | Sin |
| projects | Sin | No |

## <a name="microsoftnetapp"></a>Microsoft.NetApp
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| netappaccounts | Sin | Sin |
| netappaccounts/capacitypools | Sin | No |
| netappaccounts/capacitypools/volumes | No | Sin |
| netappaccounts/capacitypools/volumes/mounttargets | Sin | Sin |
| netappaccounts/capacitypools/volumes/snapshots | No | Sin |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| applicationgateways | No | Sin |
| applicationgatewaywebapplicationfirewallpolicies | Sin | Sin |
| applicationsecuritygroups | Sí | Sí |
| azurefirewalls | Sí | Sí |
| bastionhosts | No | Sin |
| connections | Sí | Sí |
| ddoscustompolicies | Sí | Sí |
| ddosprotectionplans | No | Sin |
| dnszones | Sí | Sí |
| expressroutecircuits | Sin | No |
| expressroutecrossconnections | No | Sin |
| expressroutegateways | Sin | Sin |
| expressrouteports | No | Sin |
| frontdoors | Sí | Sí |
| frontdoorwebapplicationfirewallpolicies | Sí | Sí |
| loadbalancers | Sí | Sí |
| localnetworkgateways | Sí | Sí |
| natgateways | Sí | Sí |
| networkintentpolicies | Sí | Sí |
| networkinterfaces | Sí | Sí |
| networkprofiles | Sin | Sin |
| networksecuritygroups | Sí | Sí |
| networkwatchers | Sí | Sí |
| networkwatchers/connectionmonitors | Sí | Sí |
| networkwatchers/lenses | Sí | Sí |
| networkwatchers/pingmeshes | Sí | Sí |
| p2svpngateways | No | Sin |
| privatednszones | Sí | Sí |
| privatednszones/virtualnetworklinks | Sí | Sí |
| privateendpoints | No | No |
| privatelinkservices | No | No |
| publicipaddresses | Sí | Sí |
| publicipprefixes | Sí | Sí |
| routefilters | No | Sin |
| routetables | Sí | Sí |
| securegateways | Sí | Sí |
| serviceendpointpolicies | Sí | Sí |
| trafficmanagerprofiles | Sí | Sí |
| virtualhubs | No | Sin |
| virtualnetworkgateways | Sí | Sí |
| virtualnetworks | Sí | Sí |
| virtualnetworktaps | No | Sin |
| virtualwans | Sin | Sin |
| vpngateways | Sin | Sin |
| vpnsites | Sin | Sin |
| webapplicationfirewallpolicies | Sí | Sí |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| namespaces | Sí | Sí |
| namespaces/notificationhubs | Sí | Sí |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| workspaces | Sí | Sí |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| managementconfigurations | Sí | Sí |
| solutions | Sí | SÍ |
| views | SÍ | Sí |

## <a name="microsoftpeering"></a>Microsoft.Peering
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| emparejamientos | Sin | No |

## <a name="microsoftportal"></a>Microsoft.Portal
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| dashboards | Sí | Sí |

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| rootresources | Sin | Sin |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| workspacecollections | Sí | Sí |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| capacities | Sí | Sí |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| accounts | No | Sin |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| vaults | Sí | Sí |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| namespaces | Sí | Sí |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| applications | Sí | No |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| flows | Sí | Sí |
| jobcollections | Sí | Sí |

## <a name="microsoftsearch"></a>Microsoft.Search
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| searchservices | Sí | Sí |

## <a name="microsoftsecurity"></a>Microsoft.Security
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| iotsecuritysolutions | Sí | Sí |

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| gateways | Sin | Sin |
| nodes | Sin | Sin |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| namespaces | Sí | Sí |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| applications | Sin | Sin |
| clusters | Sí | Sí |
| containergroups | No | Sin |
| containergroupsets | Sin | Sin |
| edgeclusters | Sin | No |
| networks | Sin | Sin |
| secretstores | Sin | No |
| volumes | Sin | Sin |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| applications | Sí | Sí |
| containergroups | Sin | No |
| gateways | Sí | Sí |
| networks | Sí | Sí |
| secrets | Sí | Sí |
| volumes | Sí | Sí |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| signalr | Sí | Sí |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| siterecoveryvault | Sin | Sin |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| appliancedefinitions | No | Sin |
| appliances | Sin | Sin |
| applicationdefinitions | Sin | Sin |
| applications | Sin | Sin |
| jitrequests | No | Sin |

## <a name="microsoftsql"></a>Microsoft.Sql
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| instancepools | Sí | Sí |
| managedinstances | Sí | Sí |
| managedinstances/databases | Sí | Sí |
| servers | Sí | Sí |
| servers/databases | Sí | Sí |
| servers/elasticpools | Sí | Sí |
| virtualclusters | Sí | Sí |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| sqlvirtualmachinegroups | Sí | Sí |
| sqlvirtualmachines | Sí | Sí |

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| dwvm | No | Sin |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| storageaccounts | Sí | Sí |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| Memorias caché | Sin | Sin |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Sí | Sí |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | Sin | No |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| storagesyncservices | No | No |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| managers | Sin | Sin |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| streamingjobs | Sí | Sí |

## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| environments | Sin | Sin |
| environments/eventsources | Sin | Sin |
| instances | Sin | Sin |
| instances/environments | No | Sin |
| instances/environments/eventsources | No | No |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| providerregistrations | Sin | Sin |
| resources | Sin | Sin |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| environments | Sí | Sí |
| environments/eventsources | Sí | Sí |
| environments/referencedatasets | Sí | Sí |

## <a name="microsofttoken"></a>Microsoft.Token
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| almacenes | Sin | No |

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| imagetemplates | No | Sin |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| account | SÍ | Sí |
| account/extension | Sí | Sí |
| account/project | Sí | Sí |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| dedicatedcloudnodes | Sí | Sí |
| dedicatedcloudservices | Sí | Sí |
| virtualmachines | Sí | Sí |

## <a name="microsoftweb"></a>Microsoft.Web
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| certificates | No | Sí |
| connectiongateways | Sí | Sí |
| connections | Sí | Sí |
| customapis | Sí | Sí |
| hostingenvironments | No | Sin |
| serverfarms | Sí | Sí |
| sites | Sí | Sí |
| sites/premieraddons | Sí | Sí |
| sites/slots | Sí | Sí |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| deviceservices | No | Sin |

## <a name="microsoftwindowsvirtualdesktop"></a>Microsoft.WindowsVirtualDesktop
| Tipo de recurso | Grupos de recursos | Subscription |
| ------------- | ----------- | ---------- |
| applicationgroups | Sin | Sin |
| hostpools | No | Sin |
| workspaces | Sin | Sin |

## <a name="third-party-services"></a>Servicios de terceros

Actualmente los servicios de terceros no son compatibles con la operación de traslado.

## <a name="next-steps"></a>Pasos siguientes
Para ver los comandos para trasladar recursos, vea [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](resource-group-move-resources.md).
