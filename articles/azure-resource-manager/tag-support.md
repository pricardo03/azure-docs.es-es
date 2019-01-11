---
title: Compatibilidad de etiquetas de Azure Resource Manager para los recursos
description: Muestra los tipos de recursos de Azure que admiten etiquetas. Proporciona detalles de todos los servicios de Azure.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 01/02/2019
ms.author: tomfitz
ms.openlocfilehash: 50ea7a2446b5560bd208b2da128fa877068ce452
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000298"
---
# <a name="tag-support-for-azure-resources"></a>Compatibilidad de etiquetas de los recursos de Azure
En este artículo se describe si un tipo de recurso es compatible con el [etiquetado](resource-group-using-tags.md).

## <a name="aad-domain-services"></a>Servicios de dominio de AAD
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| domains | Sin  | 

## <a name="ad-hybrid-health-service"></a>Servicio de mantenimiento híbrido de AD
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| addsservices | Sin  |
| aadsupportcases | Sin  | 
| agents | Sin  | 
| anonymousapiusers | Sin  | 
| configuración | Sin  | 
| logs | Sin  | 
| reports | Sin  | 
| services | Sin  | 
| servicehealthmetrics | Sin  | 

## <a name="aks"></a>AKS
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| managedClusters | SÍ | 

## <a name="analysis-services"></a>Analysis Services
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | SÍ | 

## <a name="api-hubs"></a>API Hubs
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| apiManagementAccounts | Sin  | 
| apiManagementAccounts/apis | Sin  | 
| apiManagementAccounts/connectionAcls | Sin  | 
| apiManagementAccounts/connectionProviders | Sin  | 
| apiManagementAccounts/connectionProviderAcls | Sin  | 
| apiManagementAccounts/connections | Sin  | 

## <a name="api-management"></a>API Management
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| Azure | SÍ | 

## <a name="automation"></a>Automation
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| automationAccounts | SÍ | 
| automationAccounts/configurations | SÍ | 
| automationAccounts/jobs | Sin  | 
| automationAccounts/runbooks | SÍ | 
| automationAccounts/softwareUpdateConfigurations | Sin  | 
| automationAccounts/webhooks | Sin  | 

## <a name="azure-database-for-mariadb"></a>Azure Database for MariaDB
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | SÍ | 
| servers/configurations | Sin  |
| servers/databases | Sin  |
| servers/firewallRules | Sin  |
| servers/recoverableServers | Sin  | 
| servers/securityAlertPolicies | Sin  |
| servers/virtualNetworkRules | Sin  | 

## <a name="azure-database-for-mysql"></a>Azure Database for MySQL
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | SÍ | 
| servers/configurations | Sin  |
| servers/databases | Sin  |
| servers/firewallRules | Sin  |
| servers/recoverableServers | Sin  | 
| servers/securityAlertPolicies | Sin  |
| servers/virtualNetworkRules | Sin  | 

## <a name="azure-database-for-postgresql"></a>Azure Database for PostgreSQL
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | SÍ | 
| servers/advisors | Sin  | 
| servers/configurations | Sin  |
| servers/databases | Sin  |
| servers/firewallRules | Sin  |
| servers/queryTexts | Sin  | 
| servers/recoverableServers | Sin  | 
| servers/securityAlertPolicies | Sin  |
| servers/topQueryStatistics | Sin  | 
| servers/virtualNetworkRules | Sin  | 
| servers/waitStatistics | Sin  | 

## <a name="batch"></a>Batch
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| batchAccounts | SÍ | 

## <a name="bing-maps"></a>Mapas de Bing
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| mapApis | SÍ | 

## <a name="biztalk-services"></a>Biztalk Services
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| BizTalk | SÍ | 

## <a name="cache"></a>Memoria caché
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| Redis | SÍ | 

## <a name="cdn"></a>CDN
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| edgenodes | Sin  | 
| perfiles | SÍ | 
| profiles/endpoints | SÍ | 
| profiles/endpoints/customdomains | Sin  | 
| profiles/endpoints/origins | Sin  | 
| validateProbe | Sin  | 

## <a name="classic-compute"></a>Proceso clásico
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| domainNames | Sin  | 
| domainNames/slots | Sin  | 
| domainNames/slots/roles | Sin  | 
| virtualMachines | Sin  | 
| virtualMachines/diagnosticSettings | Sin  | 
| virtualMachines/metricDefinitions | Sin  | 
| virtualMachines/metrics | Sin  | 

## <a name="classic-infrastructure-migrate"></a>Migración de infraestructura clásica
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| classicInfrastructureResources | Sin  | 

## <a name="classic-network"></a>Red clásica
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| virtualNetworks | Sin  | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Sin  | 
| virtualNetworks/virtualNetworkPeerings | Sin  | 

## <a name="classic-storage"></a>Almacenamiento clásico
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| storageAccounts/services | Sin  | 
| storageAccounts/services/diagnosticSettings | Sin  | 

## <a name="compute"></a>Proceso
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| availabilitySets | SÍ | 
| disks | SÍ | 
| images | SÍ | 
| restorePointCollections | SÍ | 
| restorePointCollections/restorePoints | Sin  | 
| sharedVMImages | SÍ | 
| sharedVMImages/versions | SÍ | 
| snapshots | SÍ | 
| virtualMachines | SÍ | 
| virtualMachines/diagnosticSettings | Sin  | 
| virtualMachines/extensions | SÍ | 
| virtualMachines/metricDefinitions | Sin  | 
| virtualMachineScaleSets | SÍ | 
| virtualMachineScaleSets/extensions | Sin  | 
| virtualMachineScaleSets/networkInterfaces | Sin  | 
| virtualMachineScaleSets/publicIPAddresses | Sin  | 
| virtualMachineScaleSets/virtualMachines | Sin  | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Sin  | 

## <a name="container"></a>Contenedor
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| containerGroups | SÍ | 

## <a name="container-instance"></a>Instancia de contenedor
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| containerGroups | SÍ | 
| serviceAssociationLinks | Sin  | 

## <a name="container-registry"></a>Container Registry
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| registries | SÍ | 
| registries/replications | SÍ |
| registries/tasks | SÍ |
| registries/webhooks | SÍ |

## <a name="container-service"></a>Container Service
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| containerServices | SÍ | 

## <a name="cortana-analytics"></a>Cortana Analytics
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | SÍ | 

## <a name="cosmos-db"></a>Cosmos DB
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| databaseAccounts | SÍ | 
| databaseAccountNames | Sin  | 

## <a name="cost-management"></a>Administración de costos
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| Conectores | SÍ | 

## <a name="data-box"></a>Data Box
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| jobs | SÍ | 

## <a name="data-box-edge"></a>Data Box Edge
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| DataBoxEdgeDevices | SÍ | 

## <a name="data-catalog"></a>Data Catalog
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| catalogs | SÍ | 

## <a name="data-connect"></a>Data Connect
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| connectionManagers | SÍ | 

## <a name="data-factory"></a>Data Factory
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| dataFactories | SÍ | 
| dataFactories/diagnosticSettings | Sin  | 
| dataFactories/metricDefinitions | Sin  | 
| dataFactorySchema | Sin  | 
| factories | SÍ | 
| factories/integrationRuntimes | Sin  | 

## <a name="devices"></a>Dispositivos
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| IotHubs | SÍ | 
| IotHubs/eventGridFilters | Sin  | 
| ProvisioningServices | SÍ | 

## <a name="devspaces"></a>Espacios de desarrollo
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| controllers | SÍ | 

## <a name="devtest-lab"></a>DevTest Lab
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| labs | SÍ | 
| labs/artifactsources | SÍ |
| labs/costs | SÍ |
| labs/customimages | SÍ |
| labs/formulas | SÍ |
| labs/notificationchannels | SÍ |
| labs/policysets/policies | SÍ |
| labs/schedules | SÍ |
| labs/serviceRunners | SÍ | 
| labs/users | SÍ |
| labs/users/disks | SÍ |
| labs/users/environments | SÍ |
| labs/users/secrets | SÍ |
| labs/users/servicefabrics | SÍ |
| labs/users/servicefabrics/schedules | SÍ |
| labs/virtualMachines | SÍ | 
| labs/virtualmachines/schedules | SÍ |
| labs/virtualnetworks | SÍ |
| schedules | SÍ | 

## <a name="dynamics-lcs"></a>Dynamics LCS
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| lcsprojects | Sin  | 
| lcsprojects/connectors | Sin  | 
| lcsprojects/clouddeployments | Sin  | 

## <a name="event-grid"></a>Event Grid
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| domains | SÍ | 
| domains/topics | Sin  | 
| eventSubscriptions | Sin  | 
| extensionTopics | Sin  | 
| topics | SÍ | 
| topicTypes | Sin  | 

## <a name="event-hub"></a>Centro de eventos
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| clusters | SÍ | 
| namespaces | SÍ | 
| namespaces/AuthorizationRules | Sin  |
| namespaces/disasterRecoveryConfigs | Sin  |
| namespaces/eventhubs | Sin  |
| namespaces/eventhubs/authorizationRules | Sin  |
| namespaces/eventhubs/consumergroups | Sin  |

## <a name="hana-on-azure"></a>HANA en Azure
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| hanaInstances | SÍ | 

## <a name="hdinsight"></a>HDInsight
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| clusters | SÍ | 
| clusters/applications | Sin  | 

## <a name="import-export"></a>Import/Export
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| jobs | SÍ | 

## <a name="insights"></a>Información detallada
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| actionGroups | SÍ |
| activityLogAlerts | SÍ |
| alertrules | SÍ |
| automatedExportSettings | Sin  | 
| components | SÍ | 
| components/events | Sin  | 
| components/metrics | Sin  | 
| components/pricingPlans | Sin  | 
| components/query | Sin  | 
| logs | Sin  | 
| metricAlerts | SÍ |
| migrateToNewPricingModel | Sin  | 
| myWorkbooks | Sin  | 
| Consultas | Sin  | 
| rollbackToLegacyPricingModel | Sin  | 
| scheduledqueryrules | SÍ | 
| webtests | SÍ | 
| workbooks | SÍ | 

## <a name="key-vault"></a>Key Vault
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| deletedVaults | Sin  | 
| vaults | SÍ | 
| vaults/accessPolicies | Sin  | 
| vaults/secrets | Sin  | 

## <a name="log-analytics"></a>Log Analytics
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| logs | Sin  | 

## <a name="logic"></a>Lógica
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| integrationAccounts | SÍ | 
| workflows | SÍ | 

## <a name="machine-learning-services"></a>Machine Learning Services
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| áreas de trabajo | SÍ | 
| workspaces/computes | Sin  | 

## <a name="managed-identity"></a>Identidad administrada
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| Identities | Sin  | 
| userAssignedIdentities | SÍ | 

## <a name="marketplace-apps"></a>Aplicaciones de Marketplace
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| classicDevServices | SÍ | 

## <a name="marketplace-ordering"></a>Pedidos de Marketplace
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| agreements | Sin  | 
| offertypes | Sin  | 

## <a name="media"></a>Multimedia
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| mediaservices | SÍ | 
| mediaservices/accountFilters | Sin  | 
| mediaservices/assets | Sin  | 
| mediaservices/assets/assetFilters | Sin  | 
| mediaservices/contentKeyPolicies | Sin  | 
| mediaservices/eventGridFilters | Sin  | 
| mediaservices/liveEventOperations | Sin  | 
| mediaservices/liveEvents | SÍ | 
| mediaservices/liveEvents/liveOutputs | Sin  | 
| mediaservices/liveOutputOperations | Sin  | 
| mediaservices/streamingEndpoints | SÍ | 
| mediaservices/streamingEndpointOperations | Sin  | 
| mediaservices/streamingLocators | Sin  | 
| mediaservices/streamingPolicies | Sin  | 
| mediaservices/transforms | Sin  | 
| mediaservices/transforms/jobs | Sin  | 

## <a name="network"></a>Red
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| applicationGateways | SÍ | 
| applicationSecurityGroups | SÍ | 
| azureFirewalls | SÍ | 
| connections | SÍ | 
| ddosProtectionPlans | SÍ | 
| expressRouteCircuits | SÍ | 
| frontdoors | SÍ | 
| frontdoorWebApplicationFirewallPolicies | SÍ | 
| interfaceEndpoints | SÍ | 
| loadBalancers | SÍ | 
| localNetworkGateways | SÍ | 
| networkIntentPolicies | SÍ | 
| networkInterfaces | SÍ | 
| networkProfiles | SÍ | 
| networkSecurityGroups | SÍ | 
| networkWatchers | SÍ | 
| networkWatchers/connectionMonitors | SÍ | 
| networkWatchers/lenses | SÍ | 
| networkWatchers/pingMeshes | SÍ | 
| privateLinkServices | SÍ | 
| publicIPAddresses | SÍ | 
| publicIPPrefixes | SÍ | 
| routeFilters | SÍ | 
| routeTables | SÍ | 
| serviceEndpointPolicies | SÍ | 
| virtualHubs | SÍ | 
| virtualNetworks | SÍ | 
| virtualNetworkGateways | SÍ | 
| virtualNetworkTaps | SÍ | 
| virtualWans | SÍ | 
| vpnGateways | SÍ | 
| vpnSites | SÍ | 
| webApplicationFirewallPolicies | SÍ | 

## <a name="notification-hubs"></a>Notification Hubs
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| namespaces | SÍ | 
| namespaces/notificationHubs | SÍ | 

## <a name="operational-insights"></a>Operational Insights
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| áreas de trabajo | SÍ |
| workspaces/dataSources | SÍ |
| workspaces/linkedServices | SÍ |
| workspaces/savedSearches | Sin  |
| workspaces/storageInsightConfigs | SÍ |

## <a name="operations-management"></a>Operations Management
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| solutions | Sin  |

## <a name="portal"></a>Portal
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| dashboards | SÍ | 

## <a name="portal-sdk"></a>SDK del Portal
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| rootResources | SÍ | 

## <a name="power-bi"></a>Power BI
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| workspaceCollections | SÍ | 

## <a name="recovery-services"></a>Recovery Services
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| backupProtectedItems | Sin  | 
| vaults | SÍ | 

## <a name="relay"></a>Retransmisión
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| namespaces | SÍ | 

## <a name="resources"></a>Recursos
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| resourceGroups | SÍ | 
| subscriptions/resourceGroups | SÍ | 

## <a name="scheduler"></a>Scheduler
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| jobcollections | SÍ | 
| flows | SÍ | 

## <a name="search"></a>Search
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| resourceHealthMetadata | Sin  | 
| searchServices | SÍ | 

## <a name="security"></a>Seguridad
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| dataCollectionAgents | Sin  | 

## <a name="service-bus"></a>Azure Service Bus
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| namespaces | SÍ | 
| namespaces/eventgridfilters | Sin  | 

## <a name="service-fabric"></a>Service Fabric
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| clusters | SÍ | 
| clusters/applications | Sin  | 

## <a name="service-fabric-mesh"></a>Service Fabric Mesh
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| aplicaciones de escala de web | SÍ | 
| networks | SÍ | 
| volumes | SÍ | 

## <a name="signalr-service"></a>Servicio SignalR
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| SignalR | SÍ | 

## <a name="site-recovery"></a>Site Recovery
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| SiteRecoveryVault | SÍ | 

## <a name="solutions"></a>Soluciones
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| aplicaciones de escala de web | SÍ | 
| applicationDefinitions | SÍ | 
| jitRequests | SÍ | 

## <a name="sql"></a>SQL
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| locations/instanceFailoverGroups | Sin  |
| managedInstances | SÍ |
| managedInstances/databases | SÍ |
| managedInstances/databases/backupShortTermRetentionPolicies | Sin  |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | Sin  |
| managedInstances/databases/vulnerabilityAssessments | Sin  |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | Sin  |
| managedInstances/encryptionProtector | Sin  |
| managedInstances/keys | Sin  |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Sin  |
| managedInstances/vulnerabilityAssessments | Sin  |
| servers | SÍ |
| servers/administrators | Sin  |
| servers/advisors | Sin  |
| servers/auditingSettings | Sin  |
| servers/backupLongTermRetentionVaults | Sin  |
| servers/communicationLinks | Sin  |
| servers/connectionPolicies | Sin  |
| servers/databases | SÍ |
| servers/databases/advisors | Sin  |
| servers/databases/auditingSettings | Sin  |
| servers/databases/backupLongTermRetentionPolicies | Sin  |
| servers/databases/backupShortTermRetentionPolicies | Sin  |
| servers/databases/connectionPolicies | Sin  |
| servers/databases/dataMaskingPolicies | Sin  |
| servers/databases/dataMaskingPolicies/rules | Sin  |
| servers/databases/extendedAuditingSettings | Sin  |
| servers/databases/extensions | Sin  |
| servers/databases/geoBackupPolicies | Sin  |
| servers/databases/schemas/tables/columns/sensitivityLabels | Sin  |
| servers/databases/securityAlertPolicies | Sin  |
| servers/databases/syncGroups | Sin  |
| servers/databases/syncGroups/syncMembers | Sin  |
| servers/databases/transparentDataEncryption | Sin  |
| servers/databases/vulnerabilityAssessments | Sin  |
| servers/databases/vulnerabilityAssessments/rules/baselines | Sin  |
| servers/disasterRecoveryConfiguration | Sin  |
| servers/dnsAliases | Sin  |
| servers/elasticPools | SÍ |
| servers/encryptionProtector | Sin  |
| servers/extendedAuditingSettings | Sin  |
| servers/failoverGroups | SÍ |
| servers/firewallRules | Sin  |
| servers/jobAgents | SÍ |
| servers/jobAgents/credentials | Sin  |
| servers/jobAgents/jobs | Sin  |
| servers/jobAgents/jobs/executions | Sin  |
| servers/jobAgents/jobs/steps | Sin  |
| servers/jobAgents/targetGroups | Sin  |
| servers/keys | Sin  |
| servers/securityAlertPolicies | Sin  |
| servers/syncAgents | Sin  |
| servers/virtualNetworkRules | Sin  |
| servers/vulnerabilityAssessments | Sin  |

## <a name="sql-virtual-machine"></a>Máquina virtual SQL
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| DWVM | SÍ | 

## <a name="storage"></a>Storage
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| storageAccounts | SÍ | 
| storageAccounts/blobServices | Sin  | 
| storageAccounts/fileServices | Sin  | 
| storageAccounts/queueServices | Sin  | 
| storageAccounts/services | Sin  | 
| storageAccounts/services/metricDefinitions | Sin  | 
| storageAccounts/tableServices | Sin  | 

## <a name="storage-sync"></a>Sincronización de almacenamiento
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| storageSyncServices | SÍ | 
| storageSyncServices/registeredServers | Sin  | 
| storageSyncServices/syncGroups | Sin  | 
| storageSyncServices/syncGroups/cloudEndpoints | Sin  | 
| storageSyncServices/syncGroups/serverEndpoints | Sin  | 
| storageSyncServices/workflows | Sin  | 

## <a name="storsimple"></a>Storsimple
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| managers | SÍ | 

## <a name="stream-analytics"></a>Stream Analytics
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| streamingjobs | SÍ | 
| streamingjobs/diagnosticSettings | Sin  | 
| streamingjobs/metricDefinitions | Sin  | 

## <a name="subscription"></a>Subscription
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| SubscriptionDefinitions | Sin  | 
| SubscriptionOperations | Sin  | 

## <a name="support"></a>Soporte técnico
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| supporttickets | Sin  | 

## <a name="visual-studio"></a>Visual Studio
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| cuenta | SÍ | 
| account/extension | SÍ | 
| account/project | SÍ | 

## <a name="web"></a>Web
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| apiManagementAccounts | Sin  | 
| apiManagementAccounts/apiAcls | Sin  | 
| apiManagementAccounts/apis | Sin  | 
| apiManagementAccounts/apis/apiAcls | Sin  | 
| apiManagementAccounts/apis/connectionAcls | Sin  | 
| apiManagementAccounts/apis/connections | Sin  | 
| apiManagementAccounts/apis/connections/connectionAcls | Sin  | 
| apiManagementAccounts/apis/localizedDefinitions | Sin  | 
| apiManagementAccounts/connectionAcls | Sin  | 
| apiManagementAccounts/connections | Sin  | 
| billingMeters | Sin  | 
| certificates | SÍ | 
| connectionGateways | SÍ | 
| connections | SÍ | 
| customApis | SÍ | 
| deletedSites | Sin  | 
| functions | Sin  | 
| hostingEnvironments | SÍ | 
| hostingEnvironments/metrics | Sin  | 
| hostingEnvironments/multiRolePools | Sin  | 
| hostingEnvironments/workerPools | Sin  | 
| publishingUsers | Sin  | 
| serverFarms | SÍ | 
| serverFarms/workers | Sin  | 
| sites | SÍ | 
| sites/domainOwnershipIdentifiers | Sin  | 
| sites/hostNameBindings | Sin  | 
| sites/instances | Sin  | 
| sites/instances/extensions | Sin  | 
| sites/metrics | Sin  | 
| sites/premieraddons | SÍ | 
| sites/slots | SÍ | 
| sites/slots/hostNameBindings | Sin  | 
| sites/slots/instances | Sin  | 
| sites/slots/instances/extensions | Sin  | 
| sites/slots/metrics | Sin  | 
| sourceControls | Sin  | 
| validate | Sin  | 
| verifyHostingEnvironmentVnet | Sin  | 

## <a name="xrm"></a>XRM
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| organizations | Sin  | 

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre cómo aplicar etiquetas a recursos, consulte [Uso de etiquetas para organizar los recursos de Azure](resource-group-using-tags.md).
