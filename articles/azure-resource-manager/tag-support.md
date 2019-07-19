---
title: Compatibilidad de etiquetas de Azure Resource Manager para los recursos
description: Muestra los tipos de recursos de Azure que admiten etiquetas. Proporciona detalles de todos los servicios de Azure.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 06/07/2019
ms.author: tomfitz
ms.openlocfilehash: 4f1bc1415fbb875120d7b64128cae69e1e3f442c
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67339839"
---
# <a name="tag-support-for-azure-resources"></a>Compatibilidad de etiquetas de los recursos de Azure
En este artículo se describe si un tipo de recurso admite [etiquetas](resource-group-using-tags.md). La columna con la etiqueta **Admite etiquetas** indica si el tipo de recurso tiene una propiedad para la etiqueta. La columna con la etiqueta **Etiqueta en el informe de costos** indica si ese tipo de recurso pasa la etiqueta al informe de costos.

Para obtener los mismos datos como un archivo de valores separados por comas, descargue [tag-support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv).

## <a name="microsoftaad"></a>Microsoft.AAD
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| DomainServices | Sí | Sí |
| DomainServices/oucontainer | Sin | Sin |

## <a name="microsoftaadiam"></a>microsoft.aadiam
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Sin |  Sin |
| diagnosticSettingsCategories | Sin |  Sin |

## <a name="microsoftaddons"></a>Microsoft.Addons
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| supportProviders | Sin |  Sin |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| aadsupportcases | Sin |  Sin |
| addsservices | Sin |  Sin |
| agents | Sin |  Sin |
| anonymousapiusers | Sin |  Sin |
| configuración | Sin |  Sin |
| logs | Sin |  Sin |
| reports | Sin |  Sin |
| services | Sin |  Sin |

## <a name="microsoftadvisor"></a>Microsoft.Advisor
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| configuraciones | Sin |  Sin |
| generateRecommendations | Sin |  Sin |
| de películas | Sin |  Sin |
| suppressions | Sin |  Sin |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| actionRules | Sin |  Sin |
| alerts | Sin |  Sin |
| alertsList | Sin |  Sin |
| alertsSummary | Sin |  Sin |
| alertsSummaryList | Sin |  Sin |
| smartDetectorAlertRules | Sin |  Sin |
| smartDetectorRuntimeEnvironments | Sin |  Sin |
| smartGroups | Sin |  Sin |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| servers | Sí | Sí |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| reportFeedback | Sin |  Sin |
| service | SÍ | Sí |
| validateServiceName | Sin |  Sin |

## <a name="microsoftattestation"></a>Microsoft.Attestation
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| attestationProviders | Sin |  Sin |

## <a name="microsoftauthorization"></a>Microsoft.Authorization
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| classicAdministrators | Sin |  Sin |
| denyAssignments | Sin |  Sin |
| elevateAccess | Sin |  Sin |
| locks | Sin |  Sin |
| permisos | Sin |  Sin |
| policyAssignments | Sin |  Sin |
| policyDefinitions | Sin |  Sin |
| policySetDefinitions | Sin |  Sin |
| providerOperations | Sin |  Sin |
| roleAssignments | Sin |  Sin |
| roleDefinitions | Sin |  Sin |

## <a name="microsoftautomation"></a>Microsoft.Automation
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| automationAccounts | Sí | Sí |
| automationAccounts/configurations | Sí | Sí |
| automationAccounts/jobs | Sin |  Sin |
| automationAccounts/runbooks | Sí | Sí |
| automationAccounts/softwareUpdateConfigurations | Sin | Sin |
| automationAccounts/webhooks | Sin |  Sin |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| environments | Sin |  Sin |
| environments/accounts | Sin |  Sin |
| environments/accounts/namespaces | Sin |  Sin |
| environments/accounts/namespaces/configurations | Sin |  Sin |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| b2cDirectories | Sí | Sin |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| registrations | Sí | Sí |
| registrations/customerSubscriptions | Sin |  Sin |
| registrations/products | Sin |  Sin |

## <a name="microsoftbatch"></a>Microsoft.Batch
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| batchAccounts | Sí | Sí |

## <a name="microsoftbilling"></a>Microsoft.Billing
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| billingAccounts | Sin |  Sin |
| billingAccounts/billingProfiles | Sin |  Sin |
| billingAccounts/billingProfiles/billingSubscriptions | Sin |  Sin |
| billingAccounts/billingProfiles/invoices | Sin |  Sin |
| billingAccounts/billingProfiles/invoices/pricesheet | Sin |  Sin |
| billingAccounts/billingProfiles/operationStatus | Sin |  Sin |
| billingAccounts/billingProfiles/paymentMethods | Sin |  Sin |
| billingAccounts/billingProfiles/policies | Sin |  Sin |
| billingAccounts/billingProfiles/pricesheet | Sin |  Sin |
| billingAccounts/billingProfiles/products | Sin |  Sin |
| billingAccounts/billingProfiles/transactions | Sin |  Sin |
| billingAccounts/billingSubscriptions | Sin |  Sin |
| billingAccounts/departments | Sin |  Sin |
| billingAccounts/eligibleOffers | Sin |  Sin |
| billingAccounts/enrollmentAccounts | Sin |  Sin |
| billingAccounts/invoices | Sin |  Sin |
| billingAccounts/invoiceSections | Sin |  Sin |
| billingAccounts/invoiceSections/billingSubscriptions | Sin |  Sin |
| billingAccounts/invoiceSections/billingSubscriptions/transfer | Sin |  Sin |
| billingAccounts/invoiceSections/importRequests | Sin |  Sin |
| billingAccounts/invoiceSections/initiateImportRequest | Sin |  Sin |
| billingAccounts/invoiceSections/initiateTransfer | Sin |  Sin |
| billingAccounts/invoiceSections/operationStatus | Sin |  Sin |
| billingAccounts/invoiceSections/products | Sin |  Sin |
| billingAccounts/invoiceSections/transfers | Sin |  Sin |
| billingAccounts/products | Sin |  Sin |
| billingAccounts/projects | Sin |  Sin |
| billingAccounts/projects/billingSubscriptions | Sin |  Sin |
| billingAccounts/projects/importRequests | Sin |  Sin |
| billingAccounts/projects/initiateImportRequest | Sin |  Sin |
| billingAccounts/projects/operationStatus | Sin |  Sin |
| billingAccounts/projects/products | Sin |  Sin |
| billingAccounts/transactions | Sin |  Sin |
| billingPeriods | Sin |  Sin |
| BillingPermissions | Sin |  Sin |
| billingProperty | Sin |  Sin |
| BillingRoleAssignments | Sin |  Sin |
| BillingRoleDefinitions | Sin |  Sin |
| CreateBillingRoleAssignment | Sin |  Sin |
| departments | Sin |  Sin |
| enrollmentAccounts | Sin |  Sin |
| importRequests | Sin |  Sin |
| importRequests/acceptImportRequest | Sin |  Sin |
| importRequests/declineImportRequest | Sin |  Sin |
| invoices | Sin |  Sin |
| transfers | Sin |  Sin |
| transfers/acceptTransfer | Sin |  Sin |
| transfers/declineTransfer | Sin |  Sin |
| transfers/operationStatus | Sin |  Sin |
| usagePlans | Sin |  Sin |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| mapApis | Sí | Sí |
| updateCommunicationPreference | Sin |  Sin |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| BizTalk | Sí | Sí |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| blueprintAssignments | Sin |  Sin |
| blueprintAssignments/assignmentOperations | Sin |  Sin |
| blueprintAssignments/operations | Sin |  Sin |
| blueprints | Sin |  Sin |
| blueprints/artifacts | Sin |  Sin |
| blueprints/versions | Sin |  Sin |
| blueprints/versions/artifacts | Sin |  Sin |

## <a name="microsoftbotservice"></a>Microsoft.BotService
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| botServices | Sí | Sí |
| botServices/channels | Sin |  Sin |
| botServices/connections | Sin |  Sin |

## <a name="microsoftcache"></a>Microsoft.Cache
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| Redis | Sí | Sí |
| RedisConfigDefinition | Sin |  Sin |

## <a name="microsoftcapacity"></a>Microsoft.Capacity
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| appliedReservations | Sin |  Sin |
| calculatePrice | Sin |  Sin |
| catalogs | Sin |  Sin |
| commercialReservationOrders | Sin |  Sin |
| reservationOrders | Sin |  Sin |
| reservationOrders/calculateRefund | Sin |  Sin |
| reservationOrders/merge | Sin |  Sin |
| reservationOrders/reservations | Sin |  Sin |
| reservationOrders/reservations/revisions | Sin |  Sin |
| reservationOrders/return | Sin |  Sin |
| reservationOrders/split | Sin |  Sin |
| reservationOrders/swap | Sin |  Sin |
| reservations | Sin |  Sin |
| resources | Sin |  Sin |
| validateReservationOrder | Sin |  Sin |

## <a name="microsoftcdn"></a>Microsoft.Cdn
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| edgenodes | Sin |  Sin |
| perfiles | Sí | Sí |
| profiles/endpoints | Sí | Sí |
| profiles/endpoints/customdomains | Sin |  Sin |
| profiles/endpoints/origins | Sin |  Sin |
| validateProbe | Sin |  Sin |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| certificateOrders | Sí | Sí |
| certificateOrders/certificates | Sin |  Sin |
| validateCertificateRegistrationInformation | Sin |  Sin |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| capabilities | Sin |  Sin |
| domainNames | Sin |  Sin |
| domainNames/capabilities | Sin |  Sin |
| domainNames/internalLoadBalancers | Sin |  Sin |
| domainNames/serviceCertificates | Sin |  Sin |
| domainNames/slots | Sin |  Sin |
| domainNames/slots/roles | Sin |  Sin |
| moveSubscriptionResources | Sin |  Sin |
| operatingSystemFamilies | Sin |  Sin |
| operatingSystems | Sin |  Sin |
| quotas | Sin |  Sin |
| resourceTypes | Sin |  Sin |
| validateSubscriptionMoveAvailability | Sin |  Sin |
| virtualMachines | Sin |  Sin |
| virtualMachines/diagnosticSettings | Sin |  Sin |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| classicInfrastructureResources | Sin |  Sin |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| capabilities | Sin |  Sin |
| expressRouteCrossConnections | Sin |  Sin |
| expressRouteCrossConnections/peerings | Sin |  Sin |
| gatewaySupportedDevices | Sin |  Sin |
| networkSecurityGroups | Sin |  Sin |
| quotas | Sin |  Sin |
| reservedIps | Sin |  Sin |
| virtualNetworks | Sin |  Sin |
| virtualNetworks/remoteVirtualNetworkPeeringProxies | Sin |  Sin |
| virtualNetworks/virtualNetworkPeerings | Sin |  Sin |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| capabilities | Sin |  Sin |
| disks | Sin |  Sin |
| images | Sin |  Sin |
| osImages | Sin |  Sin |
| osPlatformImages | Sin |  Sin |
| publicImages | Sin |  Sin |
| quotas | Sin |  Sin |
| storageAccounts | Sin |  Sin |
| storageAccounts/services | Sin |  Sin |
| storageAccounts/services/diagnosticSettings | Sin |  Sin |
| storageAccounts/vmImages | Sin |  Sin |
| vmImages | Sin |  Sin |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| accounts | Sí | Sí |

## <a name="microsoftcommerce"></a>Microsoft.Commerce
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| RateCard | Sin |  Sin |
| UsageAggregates | Sin |  Sin |

## <a name="microsoftcompute"></a>Microsoft.Compute
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| availabilitySets | Sí | Sí |
| disks | Sí | Sí |
| images | Sí | Sí |
| restorePointCollections | Sí | Sí |
| restorePointCollections/restorePoints | Sin |  Sin |
| sharedVMImages | Sí | Sí |
| sharedVMImages/versions | Sí | Sí |
| snapshots | Sí | Sí |
| virtualMachines | Sí | Sí |
| virtualMachines/diagnosticSettings | Sin |  Sin |
| virtualMachines/extensions | Sí | Sí |
| virtualMachineScaleSets | Sí | Sí |
| virtualMachineScaleSets/extensions | Sin |  Sin |
| virtualMachineScaleSets/networkInterfaces | Sin |  Sin |
| virtualMachineScaleSets/publicIPAddresses | Sin |  Sin |
| virtualMachineScaleSets/virtualMachines | Sin |  Sin |
| virtualMachineScaleSets/virtualMachines/networkInterfaces | Sin |  Sin |

## <a name="microsoftconsumption"></a>Microsoft.Consumption
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| AggregatedCost | Sin |  Sin |
| Saldos | Sin |  Sin |
| Presupuestos | Sin |  Sin |
| Charges | Sin |  Sin |
| CostTags | Sin |  Sin |
| credits | Sin |  Sin |
| events | Sin |  Sin |
| Previsiones | Sin |  Sin |
| lots | Sin |  Sin |
| Catálogos de soluciones | Sin |  Sin |
| Pricesheets | Sin |  Sin |
| products | Sin |  Sin |
| ReservationDetails | Sin |  Sin |
| ReservationRecommendations | Sin |  Sin |
| ReservationSummaries | Sin |  Sin |
| ReservationTransactions | Sin |  Sin |
| Etiquetas | Sin |  Sin |
| Términos | Sin |  Sin |
| UsageDetails | Sin |  Sin |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| containerGroups | Sí | Sí |
| serviceAssociationLinks | Sin |  Sin |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| registries | Sí | Sí |
| registries/builds | Sin |  Sin |
| registries/builds/cancel | Sin |  Sin |
| registries/builds/getLogLink | Sin |  Sin |
| registries/buildTasks | Sí | Sí |
| registries/buildTasks/steps | Sin |  Sin |
| registries/eventGridFilters | Sin |  Sin |
| registries/getBuildSourceUploadUrl | Sin |  Sin |
| registries/GetCredentials | Sin |  Sin |
| registries/importImage | Sin |  Sin |
| registries/queueBuild | Sin |  Sin |
| registries/regenerateCredential | Sin |  Sin |
| registries/regenerateCredentials | Sin |  Sin |
| registries/replications | Sí | Sí |
| registries/runs | Sin |  Sin |
| registries/runs/cancel | Sin |  Sin |
| registries/scheduleRun | Sin |  Sin |
| registries/tasks | Sí | Sí |
| registries/updatePolicies | Sin |  Sin |
| registries/webhooks | Sí | Sí |
| registries/webhooks/getCallbackConfig | Sin |  Sin |
| registries/webhooks/ping | Sin |  Sin |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| containerServices | Sí | Sí |
| managedClusters | Sí | Sí |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| applications | Sí | Sí |
| updateCommunicationPreference | Sin |  Sin |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| accounts | Sí | Sí |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| Alertas | Sin |  Sin |
| BillingAccounts | Sin |  Sin |
| Conectores | Sí | Sí |
| Departments | Sin |  Sin |
| Dimensiones | Sin |  Sin |
| EnrollmentAccounts | Sin |  Sin |
| Consultar | Sin |  Sin |
| registro | Sin |  Sin |
| Reportconfigs | Sin |  Sin |
| Informes | Sin |  Sin |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| hubs | Sí | Sí |
| hubs/authorizationPolicies | Sin |  Sin |
| hubs/connectors | Sin |  Sin |
| hubs/connectors/mappings | Sin |  Sin |
| hubs/interactions | Sin |  Sin |
| hubs/kpi | Sin |  Sin |
| hubs/links | Sin |  Sin |
| hubs/profiles | Sin |  Sin |
| hubs/roleAssignments | Sin |  Sin |
| hubs/roles | Sin |  Sin |
| hubs/suggestTypeSchema | Sin |  Sin |
| hubs/views | Sin |  Sin |
| hubs/widgetTypes | Sin |  Sin |

## <a name="microsoftdatabox"></a>Microsoft.DataBox
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| jobs | Sí | Sí |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| DataBoxEdgeDevices | Sí | Sí |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| workspaces | Sí | Sin |
| workspaces/virtualNetworkPeerings | Sin |  Sin |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| catalogs | Sí | Sí |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| connectionManagers | Sí | Sí |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| dataFactories | Sí | Sin |
| dataFactories/diagnosticSettings | Sin |  Sin |
| dataFactorySchema | Sin |  Sin |
| factories | Sí | Sin |
| factories/integrationRuntimes | Sin |  Sin |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| accounts | Sí | Sí |
| accounts/dataLakeStoreAccounts | Sin |  Sin |
| accounts/storageAccounts | Sin |  Sin |
| accounts/storageAccounts/containers | Sin |  Sin |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| accounts | Sí | Sí |
| accounts/eventGridFilters | Sin |  Sin |
| accounts/firewallRules | Sin |  Sin |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| services | Sí | Sí |
| services/projects | Sí | Sí |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| servers | Sí | Sí |
| servers/recoverableServers | Sin |  Sin |
| servers/virtualNetworkRules | Sin |  Sin |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| servers | Sí | Sí |
| servers/recoverableServers | Sin |  Sin |
| servers/virtualNetworkRules | Sin |  Sin |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| servers | Sí | Sí |
| servers/advisors | Sin |  Sin |
| servers/queryTexts | Sin |  Sin |
| servers/recoverableServers | Sin |  Sin |
| servers/topQueryStatistics | Sin |  Sin |
| servers/virtualNetworkRules | Sin |  Sin |
| servers/waitStatistics | Sin |  Sin |

## <a name="microsoftdevices"></a>Microsoft.Devices
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| IotHubs | Sí | Sí |
| IotHubs/eventGridFilters | Sin |  Sin |
| ProvisioningServices | Sí | Sí |
| usages | Sin |  Sin |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| controllers | Sí | Sí |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| labs | Sí | Sí |
| labs/serviceRunners | Sí | Sí |
| labs/virtualMachines | Sí | Sí |
| schedules | Sí | Sí |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| databaseAccountNames | Sin |  Sin |
| databaseAccounts | Sí | Sí |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| domains | Sí | Sí |
| domains/domainOwnershipIdentifiers | Sin |  Sin |
| generateSsoRequest | Sin |  Sin |
| topLevelDomains | Sin |  Sin |
| validateDomainRegistrationInformation | Sin |  Sin |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| lcsprojects | Sin |  Sin |
| lcsprojects/clouddeployments | Sin |  Sin |
| lcsprojects/connectors | Sin |  Sin |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| domains | Sí | Sin |
| domains/topics | Sin |  Sin |
| eventSubscriptions | Sin |  Sin |
| extensionTopics | Sin |  Sin |
| topics | Sí | Sin |
| topicTypes | Sin |  Sin |

## <a name="microsofteventhub"></a>Microsoft.EventHub
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| clusters | Sí | Sin |
| namespaces | Sí | Sin |
| namespaces/authorizationrules | Sin |  Sin |
| namespaces/disasterrecoveryconfigs | Sin |  Sin |
| namespaces/eventhubs | Sin |  Sin |
| namespaces/eventhubs/authorizationrules | Sin |  Sin |
| namespaces/eventhubs/consumergroups | Sin |  Sin |

## <a name="microsoftfeatures"></a>Microsoft.Features
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| features | Sin |  Sin |
| providers | Sin |  Sin |

## <a name="microsoftgallery"></a>Microsoft.Gallery
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| enroll | Sin |  Sin |
| galleryitems | Sin |  Sin |
| generateartifactaccessuri | Sin |  Sin |
| myareas | Sin |  Sin |
| myareas/areas | Sin |  Sin |
| myareas/areas/areas | Sin |  Sin |
| myareas/areas/areas/galleryitems | Sin |  Sin |
| myareas/areas/galleryitems | Sin |  Sin |
| myareas/galleryitems | Sin |  Sin |
| registro | Sin |  Sin |
| resources | Sin |  Sin |
| retrieveresourcesbyid | Sin |  Sin |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| guestConfigurationAssignments | Sin |  Sin |
| software | Sin |  Sin |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| hanaInstances | Sí |  Sí |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| clusters | Sí | Sí |
| clusters/applications | Sin |  Sin |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| jobs | Sí | Sí |

## <a name="microsoftinformationprotection"></a>Microsoft.InformationProtection
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| labelGroups | Sin |  Sin |
| labelGroups/labels | Sin |  Sin |
| labelGroups/labels/conditions | Sin |  Sin |
| labelGroups/labels/subLabels | Sin |  Sin |
| labelGroups/labels/subLabels/conditions | Sin |  Sin |

## <a name="microsoftinsights"></a>microsoft.insights
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| actiongroups | Sí | Sí |
| activityLogAlerts | Sí | Sí |
| alertrules | Sí | Sí |
| automatedExportSettings | Sin |  Sin |
| autoscalesettings | Sí | Sí |
| baseline | Sin |  Sin |
| calculatebaseline | Sin |  Sin |
| components | Sí | Sí |
| components/events | Sin |  Sin |
| components/pricingPlans | Sin |  Sin |
| components/query | Sin |  Sin |
| diagnosticSettings | Sin |  Sin |
| diagnosticSettingsCategories | Sin |  Sin |
| eventCategories | Sin |  Sin |
| eventtypes | Sin |  Sin |
| extendedDiagnosticSettings | Sin |  Sin |
| logDefinitions | Sin |  Sin |
| logprofiles | Sin |  Sin |
| logs | Sin |  Sin |
| metricAlerts | Sí | Sí |
| migrateToNewPricingModel | Sin |  Sin |
| myWorkbooks | Sin |  Sin |
| Consultas | Sin |  Sin |
| rollbackToLegacyPricingModel | Sin |  Sin |
| scheduledqueryrules | Sí | Sí |
| vmInsightsOnboardingStatuses | Sin |  Sin |
| webtests | Sí | Sí |
| workbooks | Sí | Sí |

## <a name="microsoftintune"></a>Microsoft.Intune
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Sin |  Sin |
| diagnosticSettingsCategories | Sin |  Sin |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| IoTApps | Sí | Sí |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| Grafo | Sí | Sí |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| deletedVaults | Sin |  Sin |
| vaults | Sí | Sí |
| vaults/accessPolicies | Sin |  Sin |
| vaults/secrets | Sin |  Sin |

## <a name="microsoftkusto"></a>Microsoft.Kusto
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| clusters | Sí | Sí |
| clusters/databases | Sin |  Sin |
| clusters/databases/dataconnections | Sin |  Sin |
| clusters/databases/eventhubconnections | Sin |  Sin |

## <a name="microsoftlabservices"></a>Microsoft.LabServices
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| labaccounts | Sí | Sí |
| users | Sin |  Sin |

## <a name="microsoftlocationbasedservices"></a>Microsoft.LocationBasedServices
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| accounts | Sí | Sí |

## <a name="microsoftlocationservices"></a>Microsoft.LocationServices
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| accounts | Sí | Sí |

## <a name="microsoftloganalytics"></a>Microsoft.LogAnalytics
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| logs | Sin |  Sin |

## <a name="microsoftlogic"></a>Microsoft.Logic
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| integrationAccounts | Sí | Sí |
| workflows | Sí | Sí |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| commitmentPlans | Sí | Sí |
| webServices | Sí | Sí |
| Áreas de trabajo | Sí | Sí |

## <a name="microsoftmachinelearningexperimentation"></a>Microsoft.MachineLearningExperimentation
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| accounts | Sí | Sí |
| accounts/workspaces | Sí | Sí |
| accounts/workspaces/projects | Sí | Sí |
| teamAccounts | Sí | Sí |
| teamAccounts/workspaces | Sí | Sí |
| teamAccounts/workspaces/projects | Sí | Sí |

## <a name="microsoftmachinelearningmodelmanagement"></a>Microsoft.MachineLearningModelManagement
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| accounts | Sí | Sí |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| workspaces | Sí | Sí |
| workspaces/computes | Sin |  Sin |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| Identities | Sin |  Sin |
| userAssignedIdentities | Sí | 

## <a name="microsoftmanagement"></a>Microsoft.Management
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| getEntities | Sin |  Sin |
| managementGroups | Sin |  Sin |
| resources | Sin |  Sin |
| startTenantBackfill | Sin |  Sin |
| tenantBackfillStatus | Sin |  Sin |

## <a name="microsoftmaps"></a>Microsoft.Maps
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| accounts | Sí | Sí |
| accounts/eventGridFilters | Sin |  Sin |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| offers | Sin |  Sin |
| offerTypes | Sin |  Sin |
| offerTypes/publishers | Sin |  Sin |
| offerTypes/publishers/offers | Sin |  Sin |
| offerTypes/publishers/offers/plans | Sin |  Sin |
| offerTypes/publishers/offers/plans/agreements | Sin |  Sin |
| offerTypes/publishers/offers/plans/configs | Sin |  Sin |
| offerTypes/publishers/offers/plans/configs/importImage | Sin |  Sin |
| privategalleryitems | Sin |  Sin |
| products | Sin |  Sin |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| classicDevServices | Sí | Sí |
| updateCommunicationPreference | Sin |  Sin |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| agreements | Sin |  Sin |
| offertypes | Sin |  Sin |

## <a name="microsoftmedia"></a>Microsoft.Media
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| mediaservices | Sí | Sí |
| mediaservices/accountFilters | Sin |  Sin |
| mediaservices/assets | Sin |  Sin |
| mediaservices/assets/assetFilters | Sin |  Sin |
| mediaservices/contentKeyPolicies | Sin |  Sin |
| mediaservices/eventGridFilters | Sin |  Sin |
| mediaservices/liveEventOperations | Sin |  Sin |
| mediaservices/liveEvents | Sí | Sí |
| mediaservices/liveEvents/liveOutputs | Sin |  Sin |
| mediaservices/liveOutputOperations | Sin |  Sin |
| mediaservices/streamingEndpointOperations | Sin |  Sin |
| mediaservices/streamingEndpoints | Sí | Sí |
| mediaservices/streamingLocators | Sin |  Sin |
| mediaservices/streamingPolicies | Sin |  Sin |
| mediaservices/transforms | Sin |  Sin |
| mediaservices/transforms/jobs | Sin |  Sin |

## <a name="microsoftmigrate"></a>Microsoft.Migrate
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| projects | Sí | Sí |

## <a name="microsoftnetwork"></a>Microsoft.Network
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| applicationGateways | Sí | Sin |
| applicationSecurityGroups | Sí | Sí |
| azureFirewallFqdnTags | Sin |  Sin |
| azureFirewalls | Sí | Sin |
| bgpServiceCommunities | Sin |  Sin |
| connections | Sí | Sí |
| ddosCustomPolicies | Sí | Sí |
| ddosProtectionPlans | Sí | Sí |
| dnsOperationStatuses | Sin |  Sin |
| dnszones | Sí | Sí |
| dnszones/A | Sin |  Sin |
| dnszones/AAAA | Sin |  Sin |
| dnszones/all | Sin |  Sin |
| dnszones/CAA | Sin |  Sin |
| dnszones/CNAME | Sin |  Sin |
| dnszones/MX | Sin |  Sin |
| dnszones/NS | Sin |  Sin |
| dnszones/PTR | Sin |  Sin |
| dnszones/recordsets | Sin |  Sin |
| dnszones/SOA | Sin |  Sin |
| dnszones/SRV | Sin |  Sin |
| dnszones/TXT | Sin |  Sin |
| expressRouteCircuits | Sí  | Sin |
| expressRouteServiceProviders | Sin |  Sin |
| frontdoors | Sí, pero con límites (consulte la [nota siguiente](#frontdoor)) | Sí |
| frontdoorWebApplicationFirewallPolicies | Sí, pero con límites (consulte la [nota siguiente](#frontdoor)) | Sí |
| getDnsResourceReference | Sin |  Sin |
| interfaceEndpoints | Sí | Sí |
| internalNotify | Sin |  Sin |
| loadBalancers | Sí | Sin |
| localNetworkGateways | Sí | Sí |
| natGateways | Sí | Sí |
| networkIntentPolicies | Sí | Sí |
| networkInterfaces | Sí | Sí |
| networkProfiles | Sí | Sí |
| networkSecurityGroups | Sí | Sí |
| networkWatchers | Sí | Sin |
| networkWatchers/connectionMonitors | Sí | Sin |
| networkWatchers/lenses | Sí | Sin |
| networkWatchers/pingMeshes | Sí | Sin |
| privateLinkServices | Sí | Sí |
| publicIPAddresses | Sí | Sí |
| publicIPPrefixes | Sí | Sí |
| routeFilters | Sí | Sí |
| routeTables | Sí | Sí |
| serviceEndpointPolicies | Sí | Sí |
| trafficManagerGeographicHierarchies | Sin |  Sin |
| trafficmanagerprofiles | Sí | Sí |
| trafficmanagerprofiles/heatMaps | Sin |  Sin |
| virtualHubs | Sí | Sí |
| virtualNetworkGateways | Sí | Sin |
| virtualNetworks | Sí | Sí |
| virtualNetworks/subnets | Sin |  Sin |
| virtualNetworkTaps | Sí | Sí |
| virtualWans | Sí | Sí |
| vpnGateways | Sí | Sin |
| vpnSites | Sí | Sí |
| webApplicationFirewallPolicies | Sí | Sí |

<a id="frontdoor" />

Para Azure Front Door Service, puede aplicar etiquetas al crear el recurso, pero no se admite actualmente la actualización o la adición de etiquetas.

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| namespaces | Sí | Sin |
| namespaces/notificationHubs | Sí | Sin |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| devices | Sin |  Sin |
| linkTargets | Sin |  Sin |
| storageInsightConfigs | Sin |  Sin |
| workspaces | Sí | Sí |
| workspaces/dataSources | Sin |  Sin |
| workspaces/linkedServices | Sin |  Sin |
| workspaces/query | Sin |  Sin |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| managementassociations | Sin |  Sin |
| managementconfigurations | Sí | Sí |
| solutions | Sí | SÍ |
| views | SÍ | Sí |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| policyEvents | Sin |  Sin |
| policyStates | Sin |  Sin |
| policyTrackedResources | Sin |  Sin |
| remediations | Sin |  Sin |

## <a name="microsoftportal"></a>Microsoft.Portal
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| consoles | Sin |  Sin |
| dashboards | Sí | Sí |
| userSettings | Sin |  Sin |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| workspaceCollections | Sí | Sí |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| capacities | Sí | Sí |

## <a name="microsoftprojectoxford"></a>Microsoft.ProjectOxford
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| accounts | Sí | Sí |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| backupProtectedItems | Sin |  Sin |
| vaults | Sí | Sí |

## <a name="microsoftrelay"></a>Microsoft.Relay
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| namespaces | Sí | Sí |
| namespaces/authorizationrules | Sin |  Sin |
| namespaces/hybridconnections | Sin |  Sin |
| namespaces/hybridconnections/authorizationrules | Sin |  Sin |
| namespaces/wcfrelays | Sin |  Sin |
| namespaces/wcfrelays/authorizationrules | Sin |  Sin |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| resources | Sin |  Sin |
| subscriptionsStatus | Sin |  Sin |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| availabilityStatuses | Sin |  Sin |
| childAvailabilityStatuses | Sin |  Sin |
| childResources | Sin |  Sin |
| events | Sin |  Sin |
| impactedResources | Sin |  Sin |
| Notificaciones | Sin |  Sin |

## <a name="microsoftresources"></a>Microsoft.Resources
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| deployments | Sin |  Sin |
| deployments/operations | Sin |  Sin |
| links | Sin |  Sin |
| notifyResourceJobs | Sin |  Sin |
| providers | Sin |  Sin |
| resourceGroups | Sin |  Sin |
| resources | Sin |  Sin |
| subscriptions | Sin |  Sin |
| subscriptions/providers | Sin |  Sin |
| subscriptions/resourceGroups | Sin |  Sin |
| subscriptions/resourcegroups/resources | Sin |  Sin |
| subscriptions/resources | Sin |  Sin |
| subscriptions/tagnames | Sin |  Sin |
| subscriptions/tagNames/tagValues | Sin |  Sin |
| tenants | Sin |  Sin |

## <a name="microsoftsaas"></a>Microsoft.SaaS
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| applications | Sí | Sí |
| saasresources | Sin |  Sin |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| flows | Sí | Sí |
| jobcollections | Sí | Sí |

## <a name="microsoftsearch"></a>Microsoft.Search
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| resourceHealthMetadata | Sin |  Sin |
| searchServices | Sí | Sí |

## <a name="microsoftsecurity"></a>Microsoft.Security
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| advancedThreatProtectionSettings | Sin |  Sin |
| alerts | Sin |  Sin |
| allowedConnections | Sin |  Sin |
| appliances | Sin |  Sin |
| applicationWhitelistings | Sin |  Sin |
| AutoProvisioningSettings | Sin |  Sin |
| Compliances | Sin |  Sin |
| dataCollectionAgents | Sin |  Sin |
| discoveredSecuritySolutions | Sin |  Sin |
| externalSecuritySolutions | Sin |  Sin |
| InformationProtectionPolicies | Sin |  Sin |
| jitNetworkAccessPolicies | Sin |  Sin |
| monitoring | Sin |  Sin |
| monitoring/antimalware | Sin |  Sin |
| monitoring/baseline | Sin |  Sin |
| monitoring/patch | Sin |  Sin |
| directivas | Sin |  Sin |
| pricings | Sin |  Sin |
| securityContacts | Sin |  Sin |
| securitySolutions | Sin |  Sin |
| securitySolutionsReferenceData | Sin |  Sin |
| securityStatus | Sin |  Sin |
| securityStatus/endpoints | Sin |  Sin |
| securityStatus/subnets | Sin |  Sin |
| securityStatus/virtualMachines | Sin |  Sin |
| securityStatuses | Sin |  Sin |
| securityStatusesSummaries | Sin |  Sin |
| configuración | Sin |  Sin |
| tareas | Sin |  Sin |
| topologies | Sin |  Sin |
| workspaceSettings | Sin |  Sin |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Sin |  Sin |
| diagnosticSettingsCategories | Sin |  Sin |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| namespaces | Sí | Sin |
| namespaces/authorizationrules | Sin |  Sin |
| namespaces/disasterrecoveryconfigs | Sin |  Sin |
| namespaces/eventgridfilters | Sin |  Sin |
| namespaces/queues | Sin |  Sin |
| namespaces/queues/authorizationrules | Sin |  Sin |
| namespaces/topics | Sin |  Sin |
| namespaces/topics/authorizationrules | Sin |  Sin |
| namespaces/topics/subscriptions | Sin |  Sin |
| namespaces/topics/subscriptions/rules | Sin |  Sin |
| premiumMessagingRegions | Sin |  Sin |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| clusters | Sí | Sí |
| clusters/applications | Sin |  Sin |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| applications | Sí | Sí |
| gateways | Sí | Sí |
| networks | Sí | Sí |
| secrets | Sí | Sí |
| volumes | Sí | Sí |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| SignalR | Sí | Sí |

## <a name="microsoftsolutions"></a>Microsoft.Solutions
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| applianceDefinitions | Sí | Sí |
| appliances | Sí | Sí |
| applicationDefinitions | Sí | Sí |
| applications | Sí | Sí |
| jitRequests | Sí | Sí |

## <a name="microsoftsql"></a>Microsoft.SQL
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| managedInstances | Sí | Sí |
| managedInstances/databases | Sí (vea la nota siguiente) | Sí |
| managedInstances/databases/backupShortTermRetentionPolicies | Sin | Sin |
| managedInstances/databases/schemas/tables/columns/sensitivityLabels | Sin | Sin |
| managedInstances/databases/vulnerabilityAssessments | Sin | Sin |
| managedInstances/databases/vulnerabilityAssessments/rules/baselines | Sin | Sin |
| managedInstances/encryptionProtector | Sin | Sin |
| managedInstances/keys | Sin | Sin |
| managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Sin | Sin |
| managedInstances/vulnerabilityAssessments | Sin | Sin |
| servers | Sí | Sí |
| servers/administrators | Sin |  Sin |
| servers/communicationLinks | Sin |  Sin |
| servers/databases | Sí (vea la nota siguiente) | Sí |
| servers/encryptionProtector | Sin |  Sin |
| servers/firewallRules | Sin |  Sin |
| servers/keys | Sin |  Sin |
| servers/restorableDroppedDatabases | Sin |  Sin |
| servers/serviceobjectives | Sin |  Sin |
| servers/tdeCertificates | Sin |  Sin |

> [!NOTE]
> La base de datos maestra no admite etiquetas, pero otras bases de datos, como las de Azure SQL Data Warehouse, sí lo hacen. Las bases de datos de Azure SQL Data Warehouse deben estar en estado activo (no en pausa).


## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| SqlVirtualMachineGroups | Sí | Sí |
| SqlVirtualMachineGroups/AvailabilityGroupListeners | Sin |  Sin |
| SqlVirtualMachines | Sí | Sí |

## <a name="microsoftstorage"></a>Microsoft.Storage
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| storageAccounts | Sí | Sí |
| storageAccounts/blobServices | Sin |  Sin |
| storageAccounts/fileServices | Sin |  Sin |
| storageAccounts/queueServices | Sin |  Sin |
| storageAccounts/services | Sin |  Sin |
| storageAccounts/tableServices | Sin |  Sin |
| usages | Sin |  Sin |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| storageSyncServices | Sí | Sí |
| storageSyncServices/registeredServers | Sin |  Sin |
| storageSyncServices/syncGroups | Sin |  Sin |
| storageSyncServices/syncGroups/cloudEndpoints | Sin |  Sin |
| storageSyncServices/syncGroups/serverEndpoints | Sin |  Sin |
| storageSyncServices/workflows | Sin |  Sin |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| managers | Sí | Sí |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| streamingjobs | Sí (vea la nota siguiente) | Sí |
| streamingjobs/diagnosticSettings | Sin |  Sin |

> [!NOTE]
> No se pueden agregar etiquetas cuando hay instancias de streamingjobs en ejecución. Detenga el recurso si desea agregar una etiqueta.

## <a name="microsoftsubscription"></a>Microsoft.Subscription
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| CreateSubscription | Sin |  Sin |
| SubscriptionDefinitions | Sin |  Sin |
| SubscriptionOperations | Sin |  Sin |

## <a name="microsoftsupport"></a>microsoft.support
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| supporttickets | Sin |  Sin |

## <a name="microsoftterraformoss"></a>Microsoft.TerraformOSS
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| providerRegistrations | Sí | Sí |
| resources | Sí | Sí |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| environments | Sí | Sin |
| environments/accessPolicies | Sin |  Sin |
| environments/eventsources | Sí | Sin |
| environments/referenceDataSets | Sí | Sin |

## <a name="microsoftvisualstudio"></a>microsoft.visualstudio
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| account | SÍ | Sí |
| account/extension | Sí | Sí |
| account/project | Sí | Sí |

## <a name="microsoftweb"></a>Microsoft.Web
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| apiManagementAccounts | Sin |  Sin |
| apiManagementAccounts/apiAcls | Sin |  Sin |
| apiManagementAccounts/apis | Sin |  Sin |
| apiManagementAccounts/apis/apiAcls | Sin |  Sin |
| apiManagementAccounts/apis/connectionAcls | Sin |  Sin |
| apiManagementAccounts/apis/connections | Sin |  Sin |
| apiManagementAccounts/apis/connections/connectionAcls | Sin |  Sin |
| apiManagementAccounts/apis/localizedDefinitions | Sin |  Sin |
| apiManagementAccounts/connectionAcls | Sin |  Sin |
| apiManagementAccounts/connections | Sin |  Sin |
| billingMeters | Sin |  Sin |
| certificates | Sí | Sí |
| connectionGateways | Sí | Sí |
| connections | Sí | Sí |
| customApis | Sí | Sí |
| deletedSites | Sin |  Sin |
| functions | Sin |  Sin |
| hostingEnvironments | Sí | Sí |
| hostingEnvironments/multiRolePools | Sin |  Sin |
| hostingEnvironments/multiRolePools/instances | Sin |  Sin |
| hostingEnvironments/workerPools | Sin |  Sin |
| hostingEnvironments/workerPools/instances | Sin |  Sin |
| publishingUsers | Sin |  Sin |
| de películas | Sin |  Sin |
| resourceHealthMetadata | Sin |  Sin |
| runtimes | Sin |  Sin |
| serverFarms | Sí | Sí |
| serverFarms/workers | Sin |  Sin |
| sites | Sí | Sí |
| sites/domainOwnershipIdentifiers | Sin |  Sin |
| sites/hostNameBindings | Sin |  Sin |
| sites/instances | Sin |  Sin |
| sites/instances/extensions | Sin |  Sin |
| sites/premieraddons | Sí | Sí |
| sites/recommendations | Sin |  Sin |
| sites/resourceHealthMetadata | Sin |  Sin |
| sites/slots | Sí | Sí |
| sites/slots/hostNameBindings | Sin |  Sin |
| sites/slots/instances | Sin |  Sin |
| sites/slots/instances/extensions | Sin |  Sin |
| sourceControls | Sin |  Sin |
| validate | Sin |  Sin |
| verifyHostingEnvironmentVnet | Sin |  Sin |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| diagnosticSettings | Sin |  Sin |
| diagnosticSettingsCategories | Sin |  Sin |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| DeviceServices | Sí | Sí |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor
| Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
| ------------- | ----------- | ----------- |
| components | Sin |  Sin |
| componentsSummary | Sin |  Sin |
| monitorInstances | Sin |  Sin |
| monitorInstancesSummary | Sin |  Sin |
| monitors | Sin |  Sin |
| notificationSettings | Sin |  Sin |

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre cómo aplicar etiquetas a recursos, consulte [Uso de etiquetas para organizar los recursos de Azure](resource-group-using-tags.md).
