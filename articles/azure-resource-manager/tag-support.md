---
title: Compatibilidad de etiquetas de Azure Resource Manager para los recursos
description: Muestra los tipos de recursos de Azure que admiten etiquetas. Proporciona detalles de todos los servicios de Azure.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 02/13/2019
ms.author: tomfitz
ms.openlocfilehash: dd6836ef4f859ce77c1a56095d32373d8e08f468
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60388070"
---
# <a name="tag-support-for-azure-resources"></a>Compatibilidad de etiquetas de los recursos de Azure
En este artículo se describe si un tipo de recurso admite [etiquetas](resource-group-using-tags.md).

Para obtener los mismos datos como un archivo de valores separados por comas, descargue [tag-support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| DomainServices | Sí | 
| DomainServices/oucontainer | Sin  | 

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| diagnosticSettings | Sin  | 
| diagnosticSettingsCategories | Sin  | 

## <a name="microsoftaddons"></a>Microsoft.Addons
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| supportProviders | Sin  | 

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| aadsupportcases | Sin  | 
| addsservices | Sin  | 
| agents | Sin  | 
| anonymousapiusers | Sin  | 
| configuración | Sin  | 
| logs | Sin  | 
| reports | Sin  | 
| services | Sin  | 

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| configuraciones | Sin  | 
| generateRecommendations | Sin  | 
| de películas | Sin  | 
| suppressions | Sin  | 

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| actionRules | Sin  | 
| alerts | Sin  | 
| alertsList | Sin  | 
| alertsSummary | Sin  | 
| alertsSummaryList | Sin  | 
| smartDetectorAlertRules | Sin  | 
| smartDetectorRuntimeEnvironments | Sin  | 
| smartGroups | Sin  | 

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | Sí | 

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| reportFeedback | Sin  | 
| service | Sí | 
| validateServiceName | Sin  | 

## <a name="microsoftattestation"></a>Microsoft.Attestation
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| attestationProviders | Sin  | 

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| classicAdministrators | Sin  | 
| denyAssignments | Sin  | 
| elevateAccess | Sin  | 
| locks | Sin  | 
| permisos | Sin  | 
| policyAssignments | Sin  | 
| policyDefinitions | Sin  | 
| policySetDefinitions | Sin  | 
| providerOperations | Sin  | 
| roleAssignments | Sin  | 
| roleDefinitions | Sin  | 

## <a name="microsoftautomation"></a>Microsoft.Automation
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| automationAccounts | Sí | 
| automationAccounts/configurations | Sí | 
| automationAccounts/jobs | Sin  | 
| automationAccounts/runbooks | Sí | 
| automationAccounts/softwareUpdateConfigurations | Sin  | 
| automationAccounts/webhooks | Sin  | 

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| environments | Sin  | 
| environments/accounts | Sin  | 
| environments/accounts/namespaces | Sin  | 
| environments/accounts/namespaces/configurations | Sin  | 

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| b2cDirectories | Sí | 

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| registrations | Sí | 
| registrations/customerSubscriptions | Sin  | 
| registrations/products | Sin  | 

## <a name="microsoftbatch"></a>Microsoft.Batch
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| batchAccounts | Sí | 

## <a name="microsoftbilling"></a>Microsoft.Billing
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| billingAccounts | Sin  | 
| billingAccounts/billingProfiles | Sin  | 
| billingAccounts/billingProfiles/billingSubscriptions | Sin  | 
| billingAccounts/billingProfiles/invoices | Sin  | 
| billingAccounts/billingProfiles/invoices/pricesheet | Sin  | 
| billingAccounts/billingProfiles/operationStatus | Sin  | 
| billingAccounts/billingProfiles/paymentMethods | Sin  | 
| billingAccounts/billingProfiles/policies | Sin  | 
| billingAccounts/billingProfiles/pricesheet | Sin  | 
| billingAccounts/billingProfiles/products | Sin  | 
| billingAccounts/billingProfiles/transactions | Sin  | 
| billingAccounts/billingSubscriptions | Sin  | 
| billingAccounts/departments | Sin  | 
| billingAccounts/eligibleOffers | Sin  | 
| billingAccounts/enrollmentAccounts | Sin  | 
| billingAccounts/invoices | Sin  | 
| billingAccounts/invoiceSections | Sin  | 
| billingAccounts/invoiceSections/billingSubscriptions | Sin  | 
| billingAccounts/invoiceSections/billingSubscriptions/transfer | Sin  | 
| billingAccounts/invoiceSections/importRequests | Sin  | 
| billingAccounts/invoiceSections/initiateImportRequest | Sin  | 
| billingAccounts/invoiceSections/initiateTransfer | Sin  | 
| billingAccounts/invoiceSections/operationStatus | Sin  | 
| billingAccounts/invoiceSections/products | Sin  | 
| billingAccounts/invoiceSections/transfers | Sin  | 
| billingAccounts/products | Sin  | 
| billingAccounts/projects | Sin  | 
| billingAccounts/projects/billingSubscriptions | Sin  | 
| billingAccounts/projects/importRequests | Sin  | 
| billingAccounts/projects/initiateImportRequest | Sin  | 
| billingAccounts/projects/operationStatus | Sin  | 
| billingAccounts/projects/products | Sin  | 
| billingAccounts/transactions | Sin  | 
| billingPeriods | Sin  | 
| BillingPermissions | Sin  | 
| billingProperty | Sin  | 
| BillingRoleAssignments | Sin  | 
| BillingRoleDefinitions | Sin  | 
| CreateBillingRoleAssignment | Sin  | 
| departments | Sin  | 
| enrollmentAccounts | Sin  | 
| importRequests | Sin  | 
| importRequests/acceptImportRequest | Sin  | 
| importRequests/declineImportRequest | Sin  | 
| invoices | Sin  | 
| transfers | Sin  | 
| transfers/acceptTransfer | Sin  | 
| transfers/declineTransfer | Sin  | 
| transfers/operationStatus | Sin  | 
| usagePlans | Sin  | 

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| mapApis | Sí | 
| updateCommunicationPreference | Sin  | 

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| BizTalk | Sí | 

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| blueprintAssignments | Sin  | 
| blueprintAssignments/assignmentOperations | Sin  | 
| blueprintAssignments/operations | Sin  | 
| blueprints | Sin  | 
| blueprints/artifacts | Sin  | 
| blueprints/versions | Sin  | 
| blueprints/versions/artifacts | Sin  | 

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| botServices | Sí | 
| botServices/channels | Sin  | 
| botServices/connections | Sin  | 

## <a name="microsoftcache"></a>Microsoft.Cache
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| Redis | Sí | 
| RedisConfigDefinition | Sin  | 

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| appliedReservations | Sin  | 
| calculatePrice | Sin  | 
| catalogs | Sin  | 
| commercialReservationOrders | Sin  | 
| reservationOrders | Sin  | 
| reservationOrders/calculateRefund | Sin  | 
| reservationOrders/merge | Sin  | 
| reservationOrders/reservations | Sin  | 
| reservationOrders/reservations/revisions | Sin  | 
| reservationOrders/return | Sin  | 
| reservationOrders/split | Sin  | 
| reservationOrders/swap | Sin  | 
| reservations | Sin  | 
| resources | Sin  | 
| validateReservationOrder | Sin  | 

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| edgenodes | Sin  | 
| perfiles | Sí | 
| profiles/endpoints | Sí | 
| profiles/endpoints/customdomains | Sin  | 
| profiles/endpoints/origins | Sin  | 
| validateProbe | Sin  | 

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| certificateOrders | Sí | 
| certificateOrders/certificates | Sin  | 
| validateCertificateRegistrationInformation | Sin  | 

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| capabilities | Sin  | 
| domainNames | Sin  | 
| domainNames/capabilities | Sin  | 
| domainNames/internalLoadBalancers | Sin  | 
| domainNames/serviceCertificates | Sin  | 
| domainNames/slots | Sin  | 
| domainNames/slots/roles | Sin  | 
| moveSubscriptionResources | Sin  | 
| operatingSystemFamilies | Sin  | 
| operatingSystems | Sin  | 
| quotas | Sin  | 
| resourceTypes | Sin  | 
| validateSubscriptionMoveAvailability | Sin  | 
| virtualMachines | Sin  | 
| virtualMachines/diagnosticSettings | Sin  | 

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| classicInfrastructureResources | Sin  | 

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| capabilities | Sin  | 
| expressRouteCrossConnections | Sin  | 
| expressRouteCrossConnections/peerings | Sin  | 
| gatewaySupportedDevices | Sin  | 
| networkSecurityGroups | Sin  | 
| quotas | Sin  | 
| reservedIps | Sin  | 
| virtualNetworks | Sin  | 
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Sin  | 
| virtualNetworks/virtualNetworkPeerings | Sin  | 

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| capabilities | Sin  | 
| disks | Sin  | 
| images | Sin  | 
| osImages | Sin  | 
| osPlatformImages | Sin  | 
| publicImages | Sin  | 
| quotas | Sin  | 
| storageAccounts | Sin  | 
| storageAccounts/services | Sin  | 
| storageAccounts/services/diagnosticSettings | Sin  | 
| storageAccounts/vmImages | Sin  | 
| vmImages | Sin  | 

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | Sí | 

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| RateCard | Sin  | 
| UsageAggregates | Sin  | 

## <a name="microsoftcompute"></a>Microsoft.Compute
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| availabilitySets | Sí | 
| disks | Sí | 
| images | Sí | 
| restorePointCollections | Sí | 
| restorePointCollections/restorePoints | Sin  | 
| sharedVMImages | Sí | 
| sharedVMImages/versions | Sí | 
| snapshots | Sí | 
| virtualMachines | Sí | 
| virtualMachines/diagnosticSettings | Sin  | 
| virtualMachines/extensions | Sí | 
| virtualMachineScaleSets | Sí | 
| virtualMachineScaleSets/extensions | Sin  | 
| virtualMachineScaleSets/networkInterfaces | Sin  | 
| virtualMachineScaleSets/publicIPAddresses | Sin  | 
| virtualMachineScaleSets/virtualMachines | Sin  | 
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Sin  | 

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| AggregatedCost | Sin  | 
| Saldos | Sin  | 
| Presupuestos | Sin  | 
| Charges | Sin  | 
| CostTags | Sin  | 
| credits | Sin  | 
| events | Sin  | 
| Previsiones | Sin  | 
| lots | Sin  | 
| Catálogos de soluciones | Sin  | 
| Pricesheets | Sin  | 
| products | Sin  | 
| ReservationDetails | Sin  | 
| ReservationRecommendations | Sin  | 
| ReservationSummaries | Sin  | 
| ReservationTransactions | Sin  | 
| Etiquetas | Sin  | 
| Términos | Sin  | 
| UsageDetails | Sin  | 

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| containerGroups | Sí | 
| serviceAssociationLinks | Sin  | 

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| registries | Sí | 
| registries/builds | Sin  | 
| registries/builds/cancel | Sin  | 
| registries/builds/getLogLink | Sin  | 
| registries/buildTasks | Sí | 
| registries/buildTasks/steps | Sin  | 
| registries/eventGridFilters | Sin  | 
| registries/getBuildSourceUploadUrl | Sin  | 
| registries/GetCredentials | Sin  | 
| registries/importImage | Sin  | 
| registries/queueBuild | Sin  | 
| registries/regenerateCredential | Sin  | 
| registries/regenerateCredentials | Sin  | 
| registries/replications | Sí | 
| registries/runs | Sin  | 
| registries/runs/cancel | Sin  | 
| registries/scheduleRun | Sin  | 
| registries/tasks | Sí | 
| registries/updatePolicies | Sin  | 
| registries/webhooks | Sí | 
| registries/webhooks/getCallbackConfig | Sin  | 
| registries/webhooks/ping | Sin  | 

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| containerServices | Sí | 
| managedClusters | Sí | 

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| applications | Sí | 
| updateCommunicationPreference | Sin  | 

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | Sí | 

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| Alertas | Sin  | 
| BillingAccounts | Sin  | 
| Conectores | Sí | 
| Departments | Sin  | 
| Dimensiones | Sin  | 
| EnrollmentAccounts | Sin  | 
| Consultar | Sin  | 
| registro | Sin  | 
| Reportconfigs | Sin  | 
| Informes | Sin  | 

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| hubs | Sí | 
| hubs/authorizationPolicies | Sin  | 
| hubs/connectors | Sin  | 
| hubs/connectors/mappings | Sin  | 
| hubs/interactions | Sin  | 
| hubs/kpi | Sin  | 
| hubs/links | Sin  | 
| hubs/profiles | Sin  | 
| hubs/roleAssignments | Sin  | 
| hubs/roles | Sin  | 
| hubs/suggestTypeSchema | Sin  | 
| hubs/views | Sin  | 
| hubs/widgetTypes | Sin  | 

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| jobs | Sí | 

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| DataBoxEdgeDevices | Sí | 

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| workspaces | Sí | 
| workspaces/virtualNetworkPeerings | Sin  | 

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| catalogs | Sí | 

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| connectionManagers | Sí | 

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| dataFactories | Sí | 
| dataFactories/diagnosticSettings | Sin  | 
| dataFactorySchema | Sin  | 
| factories | Sí | 
| factories/integrationRuntimes | Sin  | 

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | Sí | 
| accounts/dataLakeStoreAccounts | Sin  | 
| accounts/storageAccounts | Sin  | 
| accounts/storageAccounts/containers | Sin  | 

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | Sí | 
| accounts/eventGridFilters | Sin  | 
| accounts/firewallRules | Sin  | 

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| services | Sí | 
| services/projects | Sí | 

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | Sí | 
| servers/recoverableServers | Sin  | 
| servers/virtualNetworkRules | Sin  | 

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | Sí | 
| servers/recoverableServers | Sin  | 
| servers/virtualNetworkRules | Sin  | 

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| servers | Sí | 
| servers/advisors | Sin  | 
| servers/queryTexts | Sin  | 
| servers/recoverableServers | Sin  | 
| servers/topQueryStatistics | Sin  | 
| servers/virtualNetworkRules | Sin  | 
| servers/waitStatistics | Sin  | 

## <a name="microsoftdevices"></a>Microsoft.Devices
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| IotHubs | Sí | 
| IotHubs/eventGridFilters | Sin  | 
| ProvisioningServices | Sí | 
| usages | Sin  | 

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| controllers | Sí | 

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| labs | Sí | 
| labs/serviceRunners | Sí | 
| labs/virtualMachines | Sí | 
| schedules | Sí | 

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| databaseAccountNames | Sin  | 
| databaseAccounts | Sí | 

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| domains | Sí | 
| domains/domainOwnershipIdentifiers | Sin  | 
| generateSsoRequest | Sin  | 
| topLevelDomains | Sin  | 
| validateDomainRegistrationInformation | Sin  | 

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| lcsprojects | Sin  | 
| lcsprojects/clouddeployments | Sin  | 
| lcsprojects/connectors | Sin  | 

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| domains | Sí | 
| domains/topics | Sin  | 
| eventSubscriptions | Sin  | 
| extensionTopics | Sin  | 
| topics | Sí | 
| topicTypes | Sin  | 

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| clusters | Sí | 
| namespaces | Sí | 
| namespaces/authorizationrules | Sin  | 
| namespaces/disasterrecoveryconfigs | Sin  | 
| namespaces/eventhubs | Sin  | 
| namespaces/eventhubs/authorizationrules | Sin  | 
| namespaces/eventhubs/consumergroups | Sin  | 

## <a name="microsoftfeatures"></a>Microsoft.Features
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| features | Sin  | 
| providers | Sin  | 

## <a name="microsoftgallery"></a>Microsoft.Gallery
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| enroll | Sin  | 
| galleryitems | Sin  | 
| generateartifactaccessuri | Sin  | 
| myareas | Sin  | 
| myareas/areas | Sin  | 
| myareas/areas/areas | Sin  | 
| myareas/areas/areas/galleryitems | Sin  | 
| myareas/areas/galleryitems | Sin  | 
| myareas/galleryitems | Sin  | 
| registro | Sin  | 
| resources | Sin  | 
| retrieveresourcesbyid | Sin  | 

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| guestConfigurationAssignments | Sin  | 
| software | Sin  | 

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| hanaInstances | Sí | 

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| clusters | Sí | 
| clusters/applications | Sin  | 

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| jobs | Sí | 

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| labelGroups | Sin  | 
| labelGroups/labels | Sin  | 
| labelGroups/labels/conditions | Sin  | 
| labelGroups/labels/subLabels | Sin  | 
| labelGroups/labels/subLabels/conditions | Sin  | 

## <a name="microsoftinsights"></a>microsoft.insights
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| actiongroups | Sí | 
| activityLogAlerts | Sí | 
| alertrules | Sí | 
| automatedExportSettings | Sin  | 
| autoscalesettings | Sí | 
| baseline | Sin  | 
| calculatebaseline | Sin  | 
| components | Sí | 
| components/events | Sin  | 
| components/pricingPlans | Sin  | 
| components/query | Sin  | 
| diagnosticSettings | Sin  | 
| diagnosticSettingsCategories | Sin  | 
| eventCategories | Sin  | 
| eventtypes | Sin  | 
| extendedDiagnosticSettings | Sin  | 
| logDefinitions | Sin  | 
| logprofiles | Sin  | 
| logs | Sin  | 
| metricAlerts | Sí |
| migrateToNewPricingModel | Sin  | 
| myWorkbooks | Sin  | 
| Consultas | Sin  | 
| rollbackToLegacyPricingModel | Sin  | 
| scheduledqueryrules | Sí | 
| vmInsightsOnboardingStatuses | Sin  | 
| webtests | Sí | 
| workbooks | Sí | 

## <a name="microsoftintune"></a>Microsoft.Intune
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| diagnosticSettings | Sin  | 
| diagnosticSettingsCategories | Sin  | 

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| IoTApps | Sí | 

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| Grafo | Sí | 

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| deletedVaults | Sin  | 
| vaults | Sí | 
| vaults/accessPolicies | Sin  | 
| vaults/secrets | Sin  | 

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| clusters | Sí | 
| clusters/databases | Sin  | 
| clusters/databases/dataconnections | Sin  | 
| clusters/databases/eventhubconnections | Sin  | 

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| labaccounts | Sí | 
| users | Sin  | 

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | Sí | 

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | Sí | 

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| logs | Sin  | 

## <a name="microsoftlogic"></a>Microsoft.Logic
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| integrationAccounts | Sí | 
| workflows | Sí | 

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| commitmentPlans | Sí | 
| webServices | Sí | 
| Áreas de trabajo | Sí | 

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | Sí | 
| accounts/workspaces | Sí | 
| accounts/workspaces/projects | Sí | 
| teamAccounts | Sí | 
| teamAccounts/workspaces | Sí | 
| teamAccounts/workspaces/projects | Sí | 

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | Sí | 

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| workspaces | Sí | 
| workspaces/computes | Sin  | 

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| Identities | Sin  | 
| userAssignedIdentities | Sí | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| getEntities | Sin  | 
| managementGroups | Sin  | 
| resources | Sin  | 
| startTenantBackfill | Sin  | 
| tenantBackfillStatus | Sin  | 

## <a name="microsoftmaps"></a>Microsoft.Maps
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | Sí | 
| accounts/eventGridFilters | Sin  | 

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| offers | Sin  | 
| offerTypes | Sin  | 
| offerTypes/publishers | Sin  | 
| offerTypes/publishers/offers | Sin  | 
| offerTypes/publishers/offers/plans | Sin  | 
| offerTypes/publishers/offers/plans/agreements | Sin  | 
| offerTypes/publishers/offers/plans/configs | Sin  | 
| offerTypes/publishers/offers/plans/configs/importImage | Sin  | 
| privategalleryitems | Sin  | 
| products | Sin  | 

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| classicDevServices | Sí | 
| updateCommunicationPreference | Sin  | 

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| agreements | Sin  | 
| offertypes | Sin  | 

## <a name="microsoftmedia"></a>Microsoft.Media
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| mediaservices | Sí | 
| mediaservices/accountFilters | Sin  | 
| mediaservices/assets | Sin  | 
| mediaservices/assets/assetFilters | Sin  | 
| mediaservices/contentKeyPolicies | Sin  | 
| mediaservices/eventGridFilters | Sin  | 
| mediaservices/liveEventOperations | Sin  | 
| mediaservices/liveEvents | Sí | 
| mediaservices/liveEvents/liveOutputs | Sin  | 
| mediaservices/liveOutputOperations | Sin  | 
| mediaservices/streamingEndpointOperations | Sin  | 
| mediaservices/streamingEndpoints | Sí | 
| mediaservices/streamingLocators | Sin  | 
| mediaservices/streamingPolicies | Sin  | 
| mediaservices/transforms | Sin  | 
| mediaservices/transforms/jobs | Sin  | 

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| projects | Sí | 

## <a name="microsoftnetwork"></a>Microsoft.Network
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| applicationGateways | Sí | 
| applicationSecurityGroups | Sí | 
| azureFirewallFqdnTags | Sin  | 
| azureFirewalls | Sí | 
| bgpServiceCommunities | Sin  | 
| connections | Sí | 
| ddosCustomPolicies | Sí | 
| ddosProtectionPlans | Sí | 
| dnsOperationStatuses | Sin  | 
| dnszones | Sí | 
| dnszones/A | Sin  | 
| dnszones/AAAA | Sin  | 
| dnszones/all | Sin  | 
| dnszones/CAA | Sin  | 
| dnszones/CNAME | Sin  | 
| dnszones/MX | Sin  | 
| dnszones/NS | Sin  | 
| dnszones/PTR | Sin  | 
| dnszones/recordsets | Sin  | 
| dnszones/SOA | Sin  | 
| dnszones/SRV | Sin  | 
| dnszones/TXT | Sin  | 
| expressRouteCircuits | Sí | 
| expressRouteServiceProviders | Sin  | 
| frontdoors | Sí | 
| frontdoorWebApplicationFirewallPolicies | Sí | 
| getDnsResourceReference | Sin  | 
| interfaceEndpoints | Sí | 
| internalNotify | Sin  | 
| loadBalancers | Sí | 
| localNetworkGateways | Sí | 
| natGateways | Sí | 
| networkIntentPolicies | Sí | 
| networkInterfaces | Sí | 
| networkProfiles | Sí | 
| networkSecurityGroups | Sí | 
| networkWatchers | Sí | 
| networkWatchers/connectionMonitors | Sí | 
| networkWatchers/lenses | Sí | 
| networkWatchers/pingMeshes | Sí | 
| privateLinkServices | Sí | 
| publicIPAddresses | Sí | 
| publicIPPrefixes | Sí | 
| routeFilters | Sí | 
| routeTables | Sí | 
| serviceEndpointPolicies | Sí | 
| trafficManagerGeographicHierarchies | Sin  | 
| trafficmanagerprofiles | Sí | 
| trafficmanagerprofiles/heatMaps | Sin  | 
| virtualHubs | Sí | 
| virtualNetworkGateways | Sí | 
| virtualNetworks | Sí | 
| virtualNetworkTaps | Sí | 
| virtualWans | Sí | 
| vpnGateways | Sí | 
| vpnSites | Sí | 
| webApplicationFirewallPolicies | Sí | 

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| namespaces | Sí | 
| namespaces/notificationHubs | Sí | 

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| devices | Sin  | 
| linkTargets | Sin  | 
| storageInsightConfigs | Sin  | 
| workspaces | Sí | 
| workspaces/dataSources | Sin  | 
| workspaces/linkedServices | Sin  | 
| workspaces/query | Sin  | 

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| managementassociations | Sin  | 
| managementconfigurations | Sí | 
| solutions | Sí | 
| views | Sí | 

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| policyEvents | Sin  | 
| policyStates | Sin  | 
| policyTrackedResources | Sin  | 
| remediations | Sin  | 

## <a name="microsoftportal"></a>Microsoft.Portal
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| consoles | Sin  | 
| dashboards | Sí | 
| userSettings | Sin  | 

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| workspaceCollections | Sí | 

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| capacities | Sí | 

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| accounts | Sí | 

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| backupProtectedItems | Sin  | 
| vaults | Sí | 

## <a name="microsoftrelay"></a>Microsoft.Relay
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| namespaces | Sí | 
| namespaces/authorizationrules | Sin  | 
| namespaces/hybridconnections | Sin  | 
| namespaces/hybridconnections/authorizationrules | Sin  | 
| namespaces/wcfrelays | Sin  | 
| namespaces/wcfrelays/authorizationrules | Sin  | 

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| resources | Sin  | 
| subscriptionsStatus | Sin  | 

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| availabilityStatuses | Sin  | 
| childAvailabilityStatuses | Sin  | 
| childResources | Sin  | 
| events | Sin  | 
| impactedResources | Sin  | 
| Notificaciones | Sin  | 

## <a name="microsoftresources"></a>Microsoft.Resources
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| deployments | Sin  | 
| deployments/operations | Sin  | 
| links | Sin  | 
| notifyResourceJobs | Sin  | 
| providers | Sin  | 
| resourceGroups | Sin  | 
| resources | Sin  | 
| subscriptions | Sin  | 
| subscriptions/providers | Sin  | 
| subscriptions/resourceGroups | Sin  | 
| subscriptions/resourcegroups/resources | Sin  | 
| subscriptions/resources | Sin  | 
| subscriptions/tagnames | Sin  | 
| subscriptions/tagNames/tagValues | Sin  | 
| tenants | Sin  | 

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| applications | Sí | 
| saasresources | Sin  | 

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| flows | Sí | 
| jobcollections | Sí | 

## <a name="microsoftsearch"></a>Microsoft.Search
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| resourceHealthMetadata | Sin  | 
| searchServices | Sí | 

## <a name="microsoftsecurity"></a>Microsoft.Security
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| advancedThreatProtectionSettings | Sin  | 
| alerts | Sin  | 
| allowedConnections | Sin  | 
| appliances | Sin  | 
| applicationWhitelistings | Sin  | 
| AutoProvisioningSettings | Sin  | 
| Compliances | Sin  | 
| dataCollectionAgents | Sin  | 
| discoveredSecuritySolutions | Sin  | 
| externalSecuritySolutions | Sin  | 
| InformationProtectionPolicies | Sin  | 
| jitNetworkAccessPolicies | Sin  | 
| monitoring | Sin  | 
| monitoring/antimalware | Sin  | 
| monitoring/baseline | Sin  | 
| monitoring/patch | Sin  | 
| directivas | Sin  | 
| pricings | Sin  | 
| securityContacts | Sin  | 
| securitySolutions | Sin  | 
| securitySolutionsReferenceData | Sin  | 
| securityStatus | Sin  | 
| securityStatus/endpoints | Sin  | 
| securityStatus/subnets | Sin  | 
| securityStatus/virtualMachines | Sin  | 
| securityStatuses | Sin  | 
| securityStatusesSummaries | Sin  | 
| configuración | Sin  | 
| tareas | Sin  | 
| topologies | Sin  | 
| workspaceSettings | Sin  | 

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| diagnosticSettings | Sin  | 
| diagnosticSettingsCategories | Sin  | 

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| namespaces | Sí | 
| namespaces/authorizationrules | Sin  | 
| namespaces/disasterrecoveryconfigs | Sin  | 
| namespaces/eventgridfilters | Sin  | 
| namespaces/queues | Sin  | 
| namespaces/queues/authorizationrules | Sin  | 
| namespaces/topics | Sin  | 
| namespaces/topics/authorizationrules | Sin  | 
| namespaces/topics/subscriptions | Sin  | 
| namespaces/topics/subscriptions/rules | Sin  | 
| premiumMessagingRegions | Sin  | 

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| clusters | Sí | 
| clusters/applications | Sin  | 

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| applications | Sí | 
| gateways | Sí | 
| networks | Sí | 
| secrets | Sí | 
| volumes | Sí | 

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| SignalR | Sí | 

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| applianceDefinitions | Sí | 
| appliances | Sí | 
| applicationDefinitions | Sí | 
| applications | Sí | 
| jitRequests | Sí | 

## <a name="microsoftsql"></a>Microsoft.SQL
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| managedInstances | Sí |
| managedInstances/databases | Sí (vea la nota siguiente) |
| managedInstances/databases/backupShortTermRetentionPolicies | Sin  |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | Sin  |
| managedInstances/databases/vulnerabilityAssessments | Sin  |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | Sin  |
| managedInstances/encryptionProtector | Sin  |
| managedInstances/keys | Sin  |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Sin  |
| managedInstances/vulnerabilityAssessments | Sin  |
| servers | Sí | 
| servers/administrators | Sin  | 
| servers/communicationLinks | Sin  | 
| servers/databases | Sí (vea la nota siguiente) | 
| servers/encryptionProtector | Sin  | 
| servers/firewallRules | Sin  | 
| servers/keys | Sin  | 
| servers/restorableDroppedDatabases | Sin  | 
| servers/serviceobjectives | Sin  | 
| servers/tdeCertificates | Sin  | 

> [!NOTE]
> La base de datos maestra no admite etiquetas, pero otras bases de datos, como las de Azure SQL Data Warehouse, sí lo hacen. Las bases de datos de Azure SQL Data Warehouse deben estar en estado activo (no en pausa).


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| SqlVirtualMachineGroups | Sí | 
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Sin  | 
| SqlVirtualMachines | Sí | 

## <a name="microsoftstorage"></a>Microsoft.Storage
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| storageAccounts | Sí | 
| storageAccounts/blobServices | Sin  | 
| storageAccounts/fileServices | Sin  | 
| storageAccounts/queueServices | Sin  | 
| storageAccounts/services | Sin  | 
| storageAccounts/tableServices | Sin  | 
| usages | Sin  | 

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| storageSyncServices | Sí | 
| storageSyncServices/registeredServers | Sin  | 
| storageSyncServices/syncGroups | Sin  | 
| storageSyncServices/syncGroups/cloudEndpoints | Sin  | 
| storageSyncServices/syncGroups/serverEndpoints | Sin  | 
| storageSyncServices/workflows | Sin  | 

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| managers | Sí | 

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| streamingjobs | Sí (vea la nota siguiente) | 
| streamingjobs/diagnosticSettings | Sin  | 

> [!NOTE]
> No se pueden agregar etiquetas cuando hay instancias de streamingjobs en ejecución. Detenga el recurso si desea agregar una etiqueta.

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| CreateSubscription | Sin  | 
| SubscriptionDefinitions | Sin  | 
| SubscriptionOperations | Sin  | 

## <a name="microsoftsupport"></a>microsoft.support
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| supporttickets | Sin  | 

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| providerRegistrations | Sí | 
| resources | Sí | 

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| environments | Sí | 
| environments/accessPolicies | Sin  | 
| environments/eventsources | Sí | 
| environments/referenceDataSets | Sí | 

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| account | Sí | 
| account/extension | Sí | 
| account/project | Sí | 

## <a name="microsoftweb"></a>Microsoft.Web
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
| certificates | Sí | 
| connectionGateways | Sí | 
| connections | Sí | 
| customApis | Sí | 
| deletedSites | Sin  | 
| functions | Sin  | 
| hostingEnvironments | Sí | 
| hostingEnvironments/multiRolePools | Sin  | 
| hostingEnvironments/multiRolePools/instances | Sin  | 
| hostingEnvironments/workerPools | Sin  | 
| hostingEnvironments/workerPools/instances | Sin  | 
| publishingUsers | Sin  | 
| de películas | Sin  | 
| resourceHealthMetadata | Sin  | 
| runtimes | Sin  | 
| serverFarms | Sí | 
| serverFarms/workers | Sin  | 
| sites | Sí | 
| sites/domainOwnershipIdentifiers | Sin  | 
| sites/hostNameBindings | Sin  | 
| sites/instances | Sin  | 
| sites/instances/extensions | Sin  | 
| sites/premieraddons | Sí | 
| sites/recommendations | Sin  | 
| sites/resourceHealthMetadata | Sin  | 
| sites/slots | Sí | 
| sites/slots/hostNameBindings | Sin  | 
| sites/slots/instances | Sin  | 
| sites/slots/instances/extensions | Sin  | 
| sourceControls | Sin  | 
| validate | Sin  | 
| verifyHostingEnvironmentVnet | Sin  | 

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| diagnosticSettings | Sin  | 
| diagnosticSettingsCategories | Sin  | 

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| DeviceServices | Sí | 

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| Tipo de recurso | Compatible con las etiquetas |
| ------------- | ----------- |
| components | Sin  | 
| componentsSummary | Sin  | 
| monitorInstances | Sin  | 
| monitorInstancesSummary | Sin  | 
| monitors | Sin  | 
| notificationSettings | Sin  | 

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre cómo aplicar etiquetas a recursos, consulte [Uso de etiquetas para organizar los recursos de Azure](resource-group-using-tags.md).
