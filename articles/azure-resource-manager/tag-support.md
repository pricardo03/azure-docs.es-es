---
title: Compatibilidad de etiquetas de Azure Resource Manager para los recursos
description: Muestra los tipos de recursos de Azure que admiten etiquetas. Proporciona detalles de todos los servicios de Azure.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 11/20/2018
ms.author: tomfitz
ms.openlocfilehash: a4bb423dc5eddde0fd2d2b9b4f263ab39dbd801f
ms.sourcegitcommit: 022cf0f3f6a227e09ea1120b09a7f4638c78b3e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2018
ms.locfileid: "52284989"
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
| services | Sin  | 
| addsservices | Sin  | 
| configuración | Sin  | 
| agents | Sin  | 
| aadsupportcases | Sin  | 
| reports | Sin  | 
| servicehealthmetrics | Sin  | 
| logs | Sin  | 
| anonymousapiusers | Sin  | 

## <a name="analysis-services"></a>Analysis Services
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | SÍ | 

## <a name="api-hubs"></a>API Hubs
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| apiManagementAccounts | Sin  | 
| apiManagementAccounts/connectionProviders | Sin  | 
| apiManagementAccounts/connections | Sin  | 
| apiManagementAccounts/connectionAcls | Sin  | 
| apiManagementAccounts/connectionProviderAcls | Sin  | 
| apiManagementAccounts/apis | Sin  | 

## <a name="api-management"></a>API Management
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| Azure | SÍ | 

## <a name="automation"></a>Automation
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| automationAccounts | SÍ | 
| automationAccounts/runbooks | SÍ | 
| automationAccounts/configurations | SÍ | 
| automationAccounts/webhooks | Sin  | 
| automationAccounts/softwareUpdateConfigurations | Sin  | 
| automationAccounts/jobs | Sin  | 

## <a name="batch"></a>Batch
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| batchAccounts | SÍ | 

## <a name="batch-ai"></a>Batch AI
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| clusters | SÍ | 
| jobs | SÍ | 
| fileservers | SÍ | 
| áreas de trabajo | SÍ | 
| workspaces/clusters | Sin  | 
| workspaces/fileservers | Sin  | 
| workspaces/experiments | Sin  | 
| workspaces/experiments/jobs | Sin  | 

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
| perfiles | SÍ | 
| profiles/endpoints | SÍ | 
| profiles/endpoints/origins | Sin  | 
| profiles/endpoints/customdomains | Sin  | 
| validateProbe | Sin  | 
| edgenodes | Sin  | 

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
| virtualNetworks/virtualNetworkPeerings | Sin  | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Sin  | 

## <a name="classic-storage"></a>Almacenamiento clásico
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| storageAccounts/services | Sin  | 
| storageAccounts/services/diagnosticSettings | Sin  | 

## <a name="compute"></a>Compute
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| availabilitySets | SÍ | 
| virtualMachines | SÍ | 
| virtualMachines/extensions | SÍ | 
| virtualMachineScaleSets | SÍ | 
| virtualMachineScaleSets/extensions | Sin  | 
| virtualMachineScaleSets/virtualMachines | Sin  | 
| virtualMachineScaleSets/networkInterfaces | Sin  | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Sin  | 
| virtualMachineScaleSets/publicIPAddresses | Sin  | 
| restorePointCollections | SÍ | 
| restorePointCollections/restorePoints | Sin  | 
| virtualMachines/diagnosticSettings | Sin  | 
| virtualMachines/metricDefinitions | Sin  | 
| sharedVMImages | SÍ | 
| sharedVMImages/versions | SÍ | 
| disks | SÍ | 
| snapshots | SÍ | 
| images | SÍ | 

## <a name="container"></a>Contenedor
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| containerGroups | SÍ | 

## <a name="container-instance"></a>Instancia de contenedor
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| containerGroups | SÍ | 
| serviceAssociationLinks | Sin  | 

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
| factories | SÍ | 
| factories/integrationRuntimes | Sin  | 
| dataFactories/diagnosticSettings | Sin  | 
| dataFactories/metricDefinitions | Sin  | 
| dataFactorySchema | Sin  | 

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
| schedules | SÍ | 
| labs/virtualMachines | SÍ | 
| labs/serviceRunners | SÍ | 

## <a name="dynamics-lcs"></a>Dynamics LCS
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| lcsprojects | Sin  | 
| lcsprojects/connectors | Sin  | 
| lcsprojects/clouddeployments | Sin  | 

## <a name="event-grid"></a>Event Grid
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| eventSubscriptions | Sin  | 
| topics | SÍ | 
| domains | SÍ | 
| domains/topics | Sin  | 
| topicTypes | Sin  | 
| extensionTopics | Sin  | 

## <a name="event-hub"></a>Centro de eventos
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| namespaces | SÍ | 
| clusters | SÍ | 

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
| components | SÍ | 
| components/query | Sin  | 
| components/metrics | Sin  | 
| components/events | Sin  | 
| webtests | SÍ | 
| Consultas | Sin  | 
| scheduledqueryrules | SÍ | 
| components/pricingPlans | Sin  | 
| migrateToNewPricingModel | Sin  | 
| rollbackToLegacyPricingModel | Sin  | 
| automatedExportSettings | Sin  | 
| workbooks | SÍ | 
| myWorkbooks | Sin  | 
| logs | Sin  | 

## <a name="key-vault"></a>Key Vault
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| vaults | SÍ | 
| vaults/secrets | Sin  | 
| vaults/accessPolicies | Sin  | 
| deletedVaults | Sin  | 

## <a name="log-analytics"></a>Log Analytics
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| logs | Sin  | 

## <a name="logic"></a>Lógica
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| workflows | SÍ | 
| integrationAccounts | SÍ | 

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

## <a name="mariadb"></a>MariaDB
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | SÍ | 
| servers/recoverableServers | Sin  | 
| servers/virtualNetworkRules | Sin  | 

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
| mediaservices/assets | Sin  | 
| mediaservices/contentKeyPolicies | Sin  | 
| mediaservices/streamingLocators | Sin  | 
| mediaservices/streamingPolicies | Sin  | 
| mediaservices/eventGridFilters | Sin  | 
| mediaservices/transforms | Sin  | 
| mediaservices/transforms/jobs | Sin  | 
| mediaservices/streamingEndpoints | SÍ | 
| mediaservices/liveEvents | SÍ | 
| mediaservices/liveEvents/liveOutputs | Sin  | 
| mediaservices/streamingEndpointOperations | Sin  | 
| mediaservices/liveEventOperations | Sin  | 
| mediaservices/liveOutputOperations | Sin  | 
| mediaservices/assets/assetFilters | Sin  | 
| mediaservices/accountFilters | Sin  | 

## <a name="mysql"></a>MySQL
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | SÍ | 
| servers/recoverableServers | Sin  | 
| servers/virtualNetworkRules | Sin  | 

## <a name="network"></a>Red
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| virtualNetworks | SÍ | 
| publicIPAddresses | SÍ | 
| networkInterfaces | SÍ | 
| interfaceEndpoints | SÍ | 
| loadBalancers | SÍ | 
| networkSecurityGroups | SÍ | 
| applicationSecurityGroups | SÍ | 
| serviceEndpointPolicies | SÍ | 
| networkIntentPolicies | SÍ | 
| routeTables | SÍ | 
| publicIPPrefixes | SÍ | 
| networkWatchers | SÍ | 
| networkWatchers/connectionMonitors | SÍ | 
| networkWatchers/lenses | SÍ | 
| networkWatchers/pingMeshes | SÍ | 
| virtualNetworkGateways | SÍ | 
| localNetworkGateways | SÍ | 
| connections | SÍ | 
| applicationGateways | SÍ | 
| expressRouteCircuits | SÍ | 
| routeFilters | SÍ | 
| virtualWans | SÍ | 
| vpnSites | SÍ | 
| virtualHubs | SÍ | 
| vpnGateways | SÍ | 
| azureFirewalls | SÍ | 
| virtualNetworkTaps | SÍ | 
| privateLinkServices | SÍ | 
| ddosProtectionPlans | SÍ | 
| networkProfiles | SÍ | 
| frontdoors | SÍ | 
| frontdoorWebApplicationFirewallPolicies | SÍ | 
| webApplicationFirewallPolicies | SÍ | 

## <a name="notification-hubs"></a>Notification Hubs
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| namespaces | SÍ | 
| namespaces/notificationHubs | SÍ | 

## <a name="portal"></a>Portal
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| dashboards | SÍ | 

## <a name="portal-sdk"></a>SDK del Portal
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| rootResources | SÍ | 

## <a name="postgresql"></a>PostgreSQL
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | SÍ | 
| servers/recoverableServers | Sin  | 
| servers/virtualNetworkRules | Sin  | 
| servers/topQueryStatistics | Sin  | 
| servers/queryTexts | Sin  | 
| servers/waitStatistics | Sin  | 
| servers/advisors | Sin  | 

## <a name="power-bi"></a>Power BI
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| workspaceCollections | SÍ | 

## <a name="recovery-services"></a>Recovery Services
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| vaults | SÍ | 
| backupProtectedItems | Sin  | 

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
| searchServices | SÍ | 
| resourceHealthMetadata | Sin  | 

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

## <a name="sql-virtual-machine"></a>Máquina virtual SQL
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| DWVM | SÍ | 

## <a name="storage"></a>Storage
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| storageAccounts | SÍ | 
| storageAccounts/blobServices | Sin  | 
| storageAccounts/tableServices | Sin  | 
| storageAccounts/queueServices | Sin  | 
| storageAccounts/fileServices | Sin  | 
| storageAccounts/services | Sin  | 
| storageAccounts/services/metricDefinitions | Sin  | 

## <a name="storage-sync"></a>Sincronización de almacenamiento
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| storageSyncServices | SÍ | 
| storageSyncServices/syncGroups | Sin  | 
| storageSyncServices/syncGroups/cloudEndpoints | Sin  | 
| storageSyncServices/syncGroups/serverEndpoints | Sin  | 
| storageSyncServices/registeredServers | Sin  | 
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
| account/project | SÍ | 
| account/extension | SÍ | 
| cuenta | SÍ | 
| account/project | SÍ | 
| account/extension | SÍ | 

## <a name="web"></a>Web
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| sites/instances | Sin  | 
| sites/slots/instances | Sin  | 
| sites/instances/extensions | Sin  | 
| sites/slots/instances/extensions | Sin  | 
| publishingUsers | Sin  | 
| validate | Sin  | 
| sourceControls | Sin  | 
| sites/hostNameBindings | Sin  | 
| sites/domainOwnershipIdentifiers | Sin  | 
| sites/slots/hostNameBindings | Sin  | 
| certificates | SÍ | 
| serverFarms | SÍ | 
| serverFarms/workers | Sin  | 
| sites | SÍ | 
| sites/slots | SÍ | 
| sites/metrics | Sin  | 
| sites/slots/metrics | Sin  | 
| sites/premieraddons | SÍ | 
| hostingEnvironments | SÍ | 
| hostingEnvironments/multiRolePools | Sin  | 
| hostingEnvironments/workerPools | Sin  | 
| hostingEnvironments/metrics | Sin  | 
| functions | Sin  | 
| deletedSites | Sin  | 
| apiManagementAccounts | Sin  | 
| apiManagementAccounts/connections | Sin  | 
| apiManagementAccounts/connectionAcls | Sin  | 
| apiManagementAccounts/apis/connections/connectionAcls | Sin  | 
| apiManagementAccounts/apis/connectionAcls | Sin  | 
| apiManagementAccounts/apiAcls | Sin  | 
| apiManagementAccounts/apis/apiAcls | Sin  | 
| apiManagementAccounts/apis | Sin  | 
| apiManagementAccounts/apis/localizedDefinitions | Sin  | 
| apiManagementAccounts/apis/connections | Sin  | 
| connections | SÍ | 
| customApis | SÍ | 
| connectionGateways | SÍ | 
| billingMeters | Sin  | 
| verifyHostingEnvironmentVnet | Sin  | 

## <a name="xrm"></a>XRM
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| organizations | Sin  | 

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre cómo aplicar etiquetas a recursos, consulte [Uso de etiquetas para organizar los recursos de Azure](resource-group-using-tags.md).
