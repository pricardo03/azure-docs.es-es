---
title: Compatibilidad con el movimiento de recursos de Azure entre regiones
description: Enumera los tipos de recursos de Azure que se pueden transferir entre regiones de Azure.
author: rayne-wiselman
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 11/21/2019
ms.author: raynew
ms.openlocfilehash: 60b8708458e081d66514e092edc9ef9af7c33494
ms.sourcegitcommit: 8a2949267c913b0e332ff8675bcdfc049029b64b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74308083"
---
# <a name="support-for-moving-azure-resources-across-regions"></a>Compatibilidad con el movimiento de recursos de Azure entre regiones

En este artículo se confirma si un tipo de recurso de Azure es compatible para el movimiento a otra región de Azure. 

Vaya a un espacio de nombres del proveedor de recursos:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [microsoft.aadiam](#microsoftaadiam)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppService](#microsoftappservice)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.BatchAI](#microsoftbatchai)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Container](#microsoftcontainer)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
> - [Microsoft.DataExchange](#microsoftdataexchange)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLake](#microsoftdatalake)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [microsoft.insights](#microsoftinsights)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.LocationBasedServices](#microsoftlocationbasedservices)
> - [Microsoft.LocationServices](#microsoftlocationservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningCompute](#microsoftmachinelearningcompute)
> - [Microsoft.MachineLearningExperimentation](#microsoftmachinelearningexperimentation)
> - [Microsoft.MachineLearningModelManagement](#microsoftmachinelearningmodelmanagement)
> - [Microsoft.MachineLearningOperationalization](#microsoftmachinelearningoperationalization)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PortalSdk](#microsoftportalsdk)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectOxford](#microsoftprojectoxford)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Scheduler](#microsoftscheduler)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.ServerManagement](#microsoftservermanagement)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.Sql](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.SqlVM](#microsoftsqlvm)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.StreamAnalyticsExplorer](#microsoftstreamanalyticsexplorer)
> - [Microsoft.TerraformOSS](#microsoftterraformoss)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.Token](#microsofttoken)
> - [Microsoft.VirtualMachineImages](#microsoftvirtualmachineimages)
> - [microsoft.visualstudio](#microsoftvisualstudio)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WindowsVirtualDesktop](#microsoftwindowsvirtualdesktop)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- | 
> | domainservices | Sin | 
> | domainservices/replicasets | Sin | 

## <a name="microsoftaadiam"></a>microsoft.aadiam

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | tenants | Sin |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | actionRules | Sin | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | servers | Sin |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | service | Sin |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | configurationstores | Sin | 

## <a name="microsoftappservice"></a>Microsoft.AppService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | apiapps | Sin | 
> | appidentities | Sin | 
> | gateways | Sin | 


## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | policyassignments | Sin |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | automationaccounts | Sin | 
> | automationaccounts/configurations | Sin | 
> | automationaccounts/runbooks | Sin | 



## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | b2cdirectories | Sin | 

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | sqlserverregistrations | Sin |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | registrations | Sin | 

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | batchaccounts | Sin |

## <a name="microsoftbatchai"></a>Microsoft.BatchAI

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | clusters | Sin | 
> | fileservers | Sin | 
> | jobs | Sin | 
> | workspaces | Sin | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | mapapis | Sin | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | biztalk | Sin | 

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | blockchainmembers | Sin |
> | watchers | Sin | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | blueprintassignments | Sin | 

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | botservices | Sin | 

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | redis | Sin | 


## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | cdnwebapplicationfirewallpolicies | Sin |
> | profiles | Sin | 
> | profiles/endpoints | Sin | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | certificateorders | Sin | 


## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | domainnames | Sin |  
> | virtualmachines | Sin | 



## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | networksecuritygroups | Sin |
> | reservedips | Sin | 
> | virtualnetworks | Sin | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | storageaccounts | Sí |  


## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | availabilitysets | Sin | 
> | diskencryptionsets | Sin | 
> | disks | Sin | 
> | galleries | Sin | 
> | galleries/images | Sin | 
> | galleries/images/versions | Sin | 
> | hostgroups | Sin | 
> | hostgroups/hosts | Sin | 
> | images | Sin | 
> | proximityplacementgroups | Sin | 
> | restorepointcollections | Sin | 
> | sharedvmimages | Sin | 
> | sharedvmimages/versions | Sin | 
> | snapshots | Sin | 
> | virtualmachines | Sí | 
> | virtualmachines/extensions | Sin | 
> | virtualmachinescalesets | Sin | 

## <a name="microsoftcontainer"></a>Microsoft.Container

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | containergroups | Sin | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | containergroups | Sin | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | registries | Sin |  
> | registries/buildtasks | Sin |  
> | registries/replications | Sin | 
> | registries/tasks | Sin |  
> | registries/webhooks | Sin | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | containerservices | Sin | 
> | managedclusters | Sin | 
> | openshiftmanagedclusters | Sin | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applications | Sin | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | conectores | Sin |  

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | hubs | Sin |  

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | resourceproviders | Sin | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | jobs | Sin | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | databoxedgedevices | Sin | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | workspaces | Sin | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | catalogs | Sin | 
> | datacatalogs | Sin | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | connectionmanagers | Sin | 

## <a name="microsoftdataexchange"></a>Microsoft.DataExchange

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | packages | Sin | 
> | plans | Sin | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | datafactories | Sin | 
> | factories | Sin |  

## <a name="microsoftdatalake"></a>Microsoft.DataLake

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | datalakeaccounts | Sin | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | services | Sin | 
> | services/projects | Sin | 
> | slots | Sin | 

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | servers | Sin |  

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | servers | Sin |  

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | servergroups | Sin | 
> | servers | Sin |  
> | serversv2 | Sin | 

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | artifactsources | Sin | 
> | rollouts | Sin |  
> | servicetopologies | Sin | 
> | servicetopologies/services | Sin |  
> | servicetopologies/services/serviceunits | Sin | 
> | steps | Sin | 

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | elasticpools | Sin | 
> | elasticpools/iothubtenants | Sin | 
> | iothubs | Sí | 
> | provisioningservices | Sin | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | controllers | Sin | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | labcenters | Sin | 
> | labs | Sin | 
> | labs/environments | Sin |  
> | labs/servicerunners | Sin | 
> | labs/virtualmachines | Sin |  
> | schedules | Sin |  

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | databaseaccounts | Sin | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | domains | Sin | 

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | services | Sin |  

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | domains | Sin |  
> | topics | Sin | 

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | clusters | Sin |  
> | namespaces | Sin | 

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | hanainstances | Sin | 
> | sapmonitors | Sin |  

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | clusters | Sin | 

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | services | Sin |  

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | machines | Sin | 

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | datamanagers |  Sin | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | jobs |  Sin | 

## <a name="microsoftinsights"></a>microsoft.insights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 
> | actiongroups |  Sin | 
> | activitylogalerts | Sin | 
> | alertrules |  Sin | 
> | autoscalesettings |  Sin | 
> | components |  Sin |  
> | guestdiagnosticsettings | Sin | 
> | metricalerts | Sin | 
> | notificationgroups | Sin | 
> | notificationrules | Sin | 
> | scheduledqueryrules |  Sin | 
> | webtests |  Sin | 
> | workbooks |  Sin |  


## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | iotapps |  Sin |  

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | checknameavailability |  Sin |  
> | graph |  Sin | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | hsmpools | Sin | 
> | vaults |  Sin | 


## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | clusters |  Sin |  

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | labaccounts | Sin | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | hostingenvironments | Sin | 
> | integrationaccounts |  Sin |  
> | integrationserviceenvironments | Sin | 
> | isolatedenvironments | Sin | 
> | workflows |  Sin |  

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | commitmentplans |  Sin | 
> | webservices |  Sin | 
> | workspaces |  Sin | 

## <a name="microsoftmachinelearningcompute"></a>Microsoft.MachineLearningCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | operationalizationclusters |  Sin | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 
> | accounts/workspaces | Sin | 
> | accounts/workspaces/projects | Sin | 
> | teamaccounts | Sin | 
> | teamaccounts/workspaces | Sin | 
> | teamaccounts/workspaces/projects | Sin | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 

## <a name="microsoftmachinelearningoperationalization"></a>Microsoft.MachineLearningOperationalization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | hostingaccounts | Sin | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | workspaces | Sin | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | userassignedidentities | Sin | 

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts |  Sin |  

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | classicdevservices | Sin | 

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | mediaservices |  Sin | 
> | mediaservices/liveevents |  Sin | 
> | mediaservices/streamingendpoints |  Sin | 

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | appclusters | Sin | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | assessmentprojects | Sin | 
> | migrateprojects | Sin | 
> | projects | Sin | 

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | netappaccounts | Sin | 
> | netappaccounts/capacitypools | Sin | 
> | netappaccounts/capacitypools/volumes | Sin | 
> | netappaccounts/capacitypools/volumes/mounttargets | Sin | 
> | netappaccounts/capacitypools/volumes/snapshots | Sin | 

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applicationgateways | Sin | 
> | applicationgatewaywebapplicationfirewallpolicies | Sin | 
> | applicationsecuritygroups |  Sin |  
> | azurefirewalls |  Sin |  
> | bastionhosts | Sin | 
> | connections |  Sin | 
> | ddoscustompolicies |  Sin | 
> | ddosprotectionplans | Sin | 
> | dnszones |  Sin | 
> | expressroutecircuits | Sin | 
> | expressroutecrossconnections | Sin | 
> | expressroutegateways | Sin | 
> | expressrouteports | Sin | 
> | frontdoors | Sin | 
> | frontdoorwebapplicationfirewallpolicies | Sin | 
> | loadbalancers | Sí: SKU básico<br>No: SKU estándar | Sí: SKU básico<br> Sí: SKU estándar |
> | localnetworkgateways |  Sin | 
> | natgateways |  Sin | 
> | networkintentpolicies |  Sin | 
> | networkinterfaces | Sí | 
> | networkprofiles | Sin | 
> | networksecuritygroups | Sí | 
> | networkwatchers |  Sin |  
> | networkwatchers/connectionmonitors |  Sin | 
> | networkwatchers/lenses |  Sin | 
> | networkwatchers/pingmeshes |  Sin | 
> | p2svpngateways | Sin | 
> | privatednszones |  Sin |  
> | privatednszones/virtualnetworklinks |  Sin |  
> | privateendpoints | Sin | 
> | privatelinkservices | Sin | 
> | publicipaddresses | Sí: SKU básico<br>No: SKU estándar | Sí: SKU básico<br>No: SKU estándar |
> | publicipprefixes | Sin | 
> | routefilters | Sin | 
> | routetables |  Sin | 
> | serviceendpointpolicies |  Sin | 
> | trafficmanagerprofiles |  Sin | 
> | virtualhubs | Sin | 
> | virtualnetworkgateways |  Sin |  
> | virtualnetworks |  Sin | 
> | virtualnetworktaps | Sin | 
> | virtualwans | Sin | 
> | vpngateways (Virtual WAN) | Sin | 
> | vpnsites (Virtual WAN) | Sin | 
> | webapplicationfirewallpolicies |  Sin | 


## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | namespaces |  Sin | 
> | namespaces/notificationhubs |  Sin |  

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | workspaces |  Sin | 



## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | managementconfigurations |  Sin | 
> | views |  Sin | 

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | peerings | Sin | 

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | dashboards | Sin | 

## <a name="microsoftportalsdk"></a>Microsoft.PortalSdk

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | rootresources | Sin | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | workspacecollections |  Sin | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | capacities |  Sin | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | accounts | Sin | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | vaults | Sí (¿para almacenes de copia de seguridad?) | 


## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | namespaces |  Sin | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | Consultas |  Sin |  

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applications |  Sin | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | flows |  Sin |  
> | jobcollections |  Sin | 

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | searchservices |  Sin | 


## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | iotsecuritysolutions |  Sin | 
> | playbookconfigurations | Sin | 

## <a name="microsoftservermanagement"></a>Microsoft.ServerManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | gateways | Sin | 
> | nodes | Sin | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | namespaces |  Sin | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applications | Sin | 
> | clusters |  Sin | 
> | clusters/applications | Sin | 
> | containergroups | Sin | 
> | containergroupsets | Sin | 
> | edgeclusters | Sin | 
> | networks | Sin | 
> | secretstores | Sin | 
> | volumes | Sin | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applications |  Sin | 
> | containergroups | Sin | 
> | gateways |  Sin | 
> | networks |  Sin | 
> | secrets |  Sin | 
> | volumes |  Sin |  

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | signalr |  Sin |  

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | appliancedefinitions | Sin | 
> | appliances | Sin | 
> | applicationdefinitions | Sin | 
> | applications | Sin | 
> | jitrequests | Sin | 

## <a name="microsoftsql"></a>Microsoft.Sql

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | instancepools | Sin | 
> | managedinstances | Sí | 
> | managedinstances/databases | Sí | 
> | servers | Sí | 
> | servers/databases | Sí | 
> | servers/elasticpools | Sí | 
> | virtualclusters | Sí | 

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | sqlvirtualmachinegroups |  Sin |  
> | sqlvirtualmachines |  Sin |  

## <a name="microsoftsqlvm"></a>Microsoft.SqlVM

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | dwvm | Sin | 

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | storageaccounts | ¿Sí? | 

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | caches | Sin | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | storagesyncservices |  Sin | 

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | storagesyncservices | Sin | 

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | storagesyncservices | Sin | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | managers | Sin | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | streamingjobs |  Sin |  


## <a name="microsoftstreamanalyticsexplorer"></a>Microsoft.StreamAnalyticsExplorer

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | environments | Sin | 
> | environments/eventsources | Sin | 
> | instances | Sin | 
> | instances/environments | Sin | 
> | instances/environments/eventsources | Sin | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | providerregistrations | Sin | 
> | resources | Sin | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | environments |  Sin | 
> | environments/eventsources |  Sin |  
> | environments/referencedatasets |  Sin | 

## <a name="microsofttoken"></a>Microsoft.Token

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | almacenes | Sin | 

## <a name="microsoftvirtualmachineimages"></a>Microsoft.VirtualMachineImages

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | imagetemplates | Sin | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | account |  Sin | 
> | account/extension |  Sin | 
> | account/project |  Sin | 



## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | dedicatedcloudnodes | Sin | 
> | dedicatedcloudservices | Sin | 
> | virtualmachines | Sin | 

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | certificates | Sin | 
> | connectiongateways |  Sin |  
> | connections |  Sin |  
> | customapis |  Sin | 
> | hostingenvironments | Sin | 
> | serverfarms |  Sin |  
> | sites |  Sin | 
> | sites/premieraddons |  Sin |  
> | sites/slots |  Sin |  


## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | deviceservices | Sin | 

## <a name="microsoftwindowsvirtualdesktop"></a>Microsoft.WindowsVirtualDesktop

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Movimiento de región | 
> | ------------- | ----------- |
> | applicationgroups | Sin | 
> | hostpools | Sin | 
> | workspaces | Sin | 

## <a name="third-party-services"></a>Servicios de terceros

Actualmente los servicios de terceros no son compatibles con la operación de traslado.
