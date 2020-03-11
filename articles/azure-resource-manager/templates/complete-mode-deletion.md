---
title: Eliminación del modo completo
description: Muestra cómo los tipos de recursos controlan la eliminación en modo completo en las plantillas de Azure Resource Manager.
ms.topic: conceptual
ms.date: 02/26/2020
ms.openlocfilehash: 5f797974212636460306c6a17869d6b8380545ab
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77664413"
---
# <a name="deletion-of-azure-resources-for-complete-mode-deployments"></a>Eliminación de recursos de Azure en implementaciones en modo completo

En este artículo, se describe cómo se eliminan los tipos de recursos cuando no están en una plantilla implementada en modo completo.

Los tipos de recursos con la marca **Sí** se eliminan cuando el tipo no está en la plantilla que se implementó en modo completo.

Los tipos de recursos con la marca **No** no se eliminan automáticamente cuando no están en la plantilla; sin embargo, sí lo hacen cuando se elimina el recurso principal. Para ver una descripción completa de este comportamiento, consulte [Modos de implementación de Azure Resource Manager](deployment-modes.md).

Si implementa en [más de un grupo de recursos en una plantilla](cross-resource-group-deployment.md), se pueden eliminar los recursos del grupo de recursos especificado en la operación de implementación. Los recursos de los grupos de recursos secundarios no se eliminarán.

Vaya a un espacio de nombres del proveedor de recursos:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
> - [Microsoft.AppPlatform](#microsoftappplatform)
> - [Microsoft.Attestation](#microsoftattestation)
> - [Microsoft.Authorization](#microsoftauthorization)
> - [Microsoft.Automation](#microsoftautomation)
> - [Microsoft.Azconfig](#microsoftazconfig)
> - [Microsoft.Azure.Geneva](#microsoftazuregeneva)
> - [Microsoft.AzureActiveDirectory](#microsoftazureactivedirectory)
> - [Microsoft.AzureData](#microsoftazuredata)
> - [Microsoft.AzureStack](#microsoftazurestack)
> - [Microsoft.Batch](#microsoftbatch)
> - [Microsoft.Billing](#microsoftbilling)
> - [Microsoft.BingMaps](#microsoftbingmaps)
> - [Microsoft.Blockchain](#microsoftblockchain)
> - [Microsoft.Blueprint](#microsoftblueprint)
> - [Microsoft.BotService](#microsoftbotservice)
> - [Microsoft.Cache](#microsoftcache)
> - [Microsoft.Capacity](#microsoftcapacity)
> - [Microsoft.Cdn](#microsoftcdn)
> - [Microsoft.CertificateRegistration](#microsoftcertificateregistration)
> - [Microsoft.ClassicCompute](#microsoftclassiccompute)
> - [Microsoft.ClassicInfrastructureMigrate](#microsoftclassicinfrastructuremigrate)
> - [Microsoft.ClassicNetwork](#microsoftclassicnetwork)
> - [Microsoft.ClassicStorage](#microsoftclassicstorage)
> - [Microsoft.CognitiveServices](#microsoftcognitiveservices)
> - [Microsoft.Commerce](#microsoftcommerce)
> - [Microsoft.Compute](#microsoftcompute)
> - [Microsoft.Consumption](#microsoftconsumption)
> - [Microsoft.ContainerInstance](#microsoftcontainerinstance)
> - [Microsoft.ContainerRegistry](#microsoftcontainerregistry)
> - [Microsoft.ContainerService](#microsoftcontainerservice)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataFactory](#microsoftdatafactory)
> - [Microsoft.DataLakeAnalytics](#microsoftdatalakeanalytics)
> - [Microsoft.DataLakeStore](#microsoftdatalakestore)
> - [Microsoft.DataMigration](#microsoftdatamigration)
> - [Microsoft.DataShare](#microsoftdatashare)
> - [Microsoft.DBforMariaDB](#microsoftdbformariadb)
> - [Microsoft.DBforMySQL](#microsoftdbformysql)
> - [Microsoft.DBforPostgreSQL](#microsoftdbforpostgresql)
> - [Microsoft.DeploymentManager](#microsoftdeploymentmanager)
> - [Microsoft.DesktopVirtualization](#microsoftdesktopvirtualization)
> - [Microsoft.Devices](#microsoftdevices)
> - [Microsoft.DevOps](#microsoftdevops)
> - [Microsoft.DevSpaces](#microsoftdevspaces)
> - [Microsoft.DevTestLab](#microsoftdevtestlab)
> - [Microsoft.DocumentDB](#microsoftdocumentdb)
> - [Microsoft.DomainRegistration](#microsoftdomainregistration)
> - [Microsoft.DynamicsLcs](#microsoftdynamicslcs)
> - [Microsoft.EnterpriseKnowledgeGraph](#microsoftenterpriseknowledgegraph)
> - [Microsoft.EventGrid](#microsofteventgrid)
> - [Microsoft.EventHub](#microsofteventhub)
> - [Microsoft.Features](#microsoftfeatures)
> - [Microsoft.Gallery](#microsoftgallery)
> - [Microsoft.Genomics](#microsoftgenomics)
> - [Microsoft.GuestConfiguration](#microsoftguestconfiguration)
> - [Microsoft.HanaOnAzure](#microsofthanaonazure)
> - [Microsoft.HardwareSecurityModules](#microsofthardwaresecuritymodules)
> - [Microsoft.HDInsight](#microsofthdinsight)
> - [Microsoft.HealthcareApis](#microsofthealthcareapis)
> - [Microsoft.HybridCompute](#microsofthybridcompute)
> - [Microsoft.HybridData](#microsofthybriddata)
> - [Microsoft.Hydra](#microsofthydra)
> - [Microsoft.ImportExport](#microsoftimportexport)
> - [Microsoft.Intune](#microsoftintune)
> - [Microsoft.IoTCentral](#microsoftiotcentral)
> - [Microsoft.IoTSpaces](#microsoftiotspaces)
> - [Microsoft.KeyVault](#microsoftkeyvault)
> - [Microsoft.Kusto](#microsoftkusto)
> - [Microsoft.LabServices](#microsoftlabservices)
> - [Microsoft.Logic](#microsoftlogic)
> - [Microsoft.MachineLearning](#microsoftmachinelearning)
> - [Microsoft.MachineLearningServices](#microsoftmachinelearningservices)
> - [Microsoft.ManagedIdentity](#microsoftmanagedidentity)
> - [Microsoft.ManagedServices](#microsoftmanagedservices)
> - [Microsoft.Management](#microsoftmanagement)
> - [Microsoft.Maps](#microsoftmaps)
> - [Microsoft.Marketplace](#microsoftmarketplace)
> - [Microsoft.MarketplaceApps](#microsoftmarketplaceapps)
> - [Microsoft.MarketplaceOrdering](#microsoftmarketplaceordering)
> - [Microsoft.Media](#microsoftmedia)
> - [Microsoft.Microservices4Spring](#microsoftmicroservices4spring)
> - [Microsoft.Migrate](#microsoftmigrate)
> - [Microsoft.MixedReality](#microsoftmixedreality)
> - [Microsoft.NetApp](#microsoftnetapp)
> - [Microsoft.Notebooks](#microsoftnotebooks)
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.ObjectStore](#microsoftobjectstore)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.ProjectBabylon](#microsoftprojectbabylon)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.RemoteApp](#microsoftremoteapp)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Search](#microsoftsearch)
> - [Microsoft.Security](#microsoftsecurity)
> - [Microsoft.SecurityGraph](#microsoftsecuritygraph)
> - [Microsoft.SecurityInsights](#microsoftsecurityinsights)
> - [Microsoft.ServiceBus](#microsoftservicebus)
> - [Microsoft.ServiceFabric](#microsoftservicefabric)
> - [Microsoft.ServiceFabricMesh](#microsoftservicefabricmesh)
> - [Microsoft.Services](#microsoftservices)
> - [Microsoft.SignalRService](#microsoftsignalrservice)
> - [Microsoft.SiteRecovery](#microsoftsiterecovery)
> - [Microsoft.SoftwarePlan](#microsoftsoftwareplan)
> - [Microsoft.Solutions](#microsoftsolutions)
> - [Microsoft.SpoolService](#microsoftspoolservice)
> - [Microsoft.SQL](#microsoftsql)
> - [Microsoft.SqlVirtualMachine](#microsoftsqlvirtualmachine)
> - [Microsoft.Storage](#microsoftstorage)
> - [Microsoft.StorageCache](#microsoftstoragecache)
> - [Microsoft.StorageReplication](#microsoftstoragereplication)
> - [Microsoft.StorageSync](#microsoftstoragesync)
> - [Microsoft.StorageSyncDev](#microsoftstoragesyncdev)
> - [Microsoft.StorageSyncInt](#microsoftstoragesyncint)
> - [Microsoft.StorSimple](#microsoftstorsimple)
> - [Microsoft.StreamAnalytics](#microsoftstreamanalytics)
> - [Microsoft.Subscription](#microsoftsubscription)
> - [Microsoft.TimeSeriesInsights](#microsofttimeseriesinsights)
> - [Microsoft.VMwareCloudSimple](#microsoftvmwarecloudsimple)
> - [Microsoft.VnfManager](#microsoftvnfmanager)
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsDefenderATP](#microsoftwindowsdefenderatp)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | DomainServices | Sí |
> | DomainServices/oucontainer | Sin |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | supportProviders | Sin |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | aadsupportcases | Sin |
> | addsservices | Sin |
> | agents | Sin |
> | anonymousapiusers | Sin |
> | configuración | Sin |
> | logs | Sin |
> | reports | Sin |
> | servicehealthmetrics | Sin |
> | services | Sin |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | configuraciones | Sin |
> | generateRecommendations | Sin |
> | metadata | Sin |
> | de películas | Sin |
> | suppressions | Sin |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | actionRules | Sí |
> | alerts | Sin |
> | alertsList | Sin |
> | alertsMetaData | Sin |
> | alertsSummary | Sin |
> | alertsSummaryList | Sin |
> | feedback | Sin |
> | smartDetectorAlertRules | Sí |
> | smartDetectorRuntimeEnvironments | Sin |
> | smartGroups | Sin |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | servers | Sí |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | reportFeedback | Sin |
> | service | Sí |
> | validateServiceName | Sin |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | configurationStores | Sí |
> | configurationStores/eventGridFilters | Sin |

## <a name="microsoftappplatform"></a>Microsoft.AppPlatform

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | Spring | Sí |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | attestationProviders | Sin |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | classicAdministrators | Sin |
> | dataAliases | Sin |
> | denyAssignments | Sin |
> | elevateAccess | Sin |
> | findOrphanRoleAssignments | Sin |
> | locks | Sin |
> | permisos | Sin |
> | policyAssignments | Sin |
> | policyDefinitions | Sin |
> | policySetDefinitions | Sin |
> | providerOperations | Sin |
> | roleAssignments | Sin |
> | roleAssignmentsUsageMetrics | Sin |
> | roleDefinitions | Sin |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | automationAccounts | Sí |
> | automationAccounts/configurations | Sí |
> | automationAccounts/jobs | Sin |
> | automationAccounts / privateEndpointConnectionProxies | Sin |
> | automationAccounts / privateEndpointConnections | Sin |
> | automationAccounts / privateLinkResources | Sin |
> | automationAccounts/runbooks | Sí |
> | automationAccounts/softwareUpdateConfigurations | Sin |
> | automationAccounts/webhooks | Sin |

## <a name="microsoftazconfig"></a>Microsoft.Azconfig

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | configurationStores | Sí |
> | configurationStores/eventGridFilters | Sin |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | environments | Sin |
> | environments/accounts | Sin |
> | environments/accounts/namespaces | Sin |
> | environments/accounts/namespaces/configurations | Sin |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | b2cDirectories | Sí |
> | b2ctenants | Sin |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | hybridDataManagers | Sí |
> | postgresInstances | Sí |
> | sqlBigDataClusters | Sí |
> | sqlInstances | Sí |
> | sqlServerRegistrations | Sí |
> | sqlServerRegistrations/sqlServers | Sin |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | registrations | Sí |
> | registrations/customerSubscriptions | Sin |
> | registrations/products | Sin |
> | verificationKeys | Sin |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | batchAccounts | Sí |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | billingAccounts | Sin |
> | billingAccounts/agreements | Sin |
> | billingAccounts/billingPermissions | Sin |
> | billingAccounts/billingProfiles | Sin |
> | billingAccounts/billingProfiles/billingPermissions | Sin |
> | billingAccounts/billingProfiles/billingRoleAssignments | Sin |
> | billingAccounts/billingProfiles/billingRoleDefinitions | Sin |
> | billingAccounts/billingProfiles/billingSubscriptions | Sin |
> | billingAccounts/billingProfiles/createBillingRoleAssignment | Sin |
> | billingAccounts/billingProfiles/customers | Sin |
> | billingAccounts / billingProfiles / instructions | Sin |
> | billingAccounts/billingProfiles/invoices | Sin |
> | billingAccounts/billingProfiles/invoices/pricesheet | Sin |
> | billingAccounts/billingProfiles/invoices/transactions | Sin |
> | billingAccounts/billingProfiles/invoiceSections | Sin |
> | billingAccounts/billingProfiles/invoiceSections/billingPermissions | Sin |
> | billingAccounts/billingProfiles/invoiceSections/billingRoleAssignments | Sin |
> | billingAccounts/billingProfiles/invoiceSections/billingRoleDefinitions | Sin |
> | billingAccounts/billingProfiles/invoiceSections/billingSubscriptions | Sin |
> | billingAccounts/billingProfiles/invoiceSections/createBillingRoleAssignment | Sin |
> | billingAccounts/billingProfiles/invoiceSections/initiateTransfer | Sin |
> | billingAccounts/billingProfiles/invoiceSections/products | Sin |
> | billingAccounts/billingProfiles/invoiceSections/products/transfer | Sin |
> | billingAccounts/billingProfiles/invoiceSections/products/updateAutoRenew | Sin |
> | billingAccounts/billingProfiles/invoiceSections/transactions | Sin |
> | billingAccounts/billingProfiles/invoiceSections/transfers | Sin |
> | billingAccounts/BillingProfiles/patchOperations | Sin |
> | billingAccounts/billingProfiles/paymentMethods | Sin |
> | billingAccounts/billingProfiles/policies | Sin |
> | billingAccounts/billingProfiles/pricesheet | Sin |
> | billingAccounts/billingProfiles/pricesheetDownloadOperations | Sin |
> | billingAccounts/billingProfiles/products | Sin |
> | billingAccounts/billingProfiles/transactions | Sin |
> | billingAccounts/billingRoleAssignments | Sin |
> | billingAccounts/billingRoleDefinitions | Sin |
> | billingAccounts/billingSubscriptions | Sin |
> | billingAccounts/billingSubscriptions/invoices | Sin |
> | billingAccounts/createBillingRoleAssignment | Sin |
> | billingAccounts/createInvoiceSectionOperations | Sin |
> | billingAccounts/customers | Sin |
> | billingAccounts/customers/billingPermissions | Sin |
> | billingAccounts/customers/billingSubscriptions | Sin |
> | billingAccounts/customers/initiateTransfer | Sin |
> | billingAccounts/customers/policies | Sin |
> | billingAccounts/customers/products | Sin |
> | billingAccounts/customers/transactions | Sin |
> | billingAccounts/customers/transfers | Sin |
> | billingAccounts/departments | Sin |
> | billingAccounts/enrollmentAccounts | Sin |
> | billingAccounts/invoices | Sin |
> | billingAccounts/invoiceSections | Sin |
> | billingAccounts/invoiceSections/billingSubscriptionMoveOperations | Sin |
> | billingAccounts/invoiceSections/billingSubscriptions | Sin |
> | billingAccounts/invoiceSections/billingSubscriptions/transfer | Sin |
> | billingAccounts/invoiceSections/elevate | Sin |
> | billingAccounts/invoiceSections/initiateTransfer | Sin |
> | billingAccounts/invoiceSections/patchOperations | Sin |
> | billingAccounts/invoiceSections/productMoveOperations | Sin |
> | billingAccounts/invoiceSections/products | Sin |
> | billingAccounts/invoiceSections/products/transfer | Sin |
> | billingAccounts/invoiceSections/products/updateAutoRenew | Sin |
> | billingAccounts/invoiceSections/transactions | Sin |
> | billingAccounts/invoiceSections/transfers | Sin |
> | billingAccounts/lineOfCredit | Sin |
> | billingAccounts/patchOperations | Sin |
> | billingAccounts/paymentMethods | Sin |
> | billingAccounts/products | Sin |
> | billingAccounts/transactions | Sin |
> | billingPeriods | Sin |
> | billingPermissions | Sin |
> | billingProperty | Sin |
> | billingRoleAssignments | Sin |
> | billingRoleDefinitions | Sin |
> | createBillingRoleAssignment | Sin |
> | departments | Sin |
> | enrollmentAccounts | Sin |
> | invoices | Sin |
> | transfers | Sin |
> | transfers/acceptTransfer | Sin |
> | transfers/declineTransfer | Sin |
> | transfers/operationStatus | Sin |
> | transfers/validateTransfer | Sin |
> | validateAddress | Sin |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | mapApis | Sí |
> | updateCommunicationPreference | Sin |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | blockchainMembers | Sí |
> | cordaMembers | Sí |
> | watchers | Sí |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | blueprintAssignments | Sin |
> | blueprintAssignments/assignmentOperations | Sin |
> | blueprintAssignments/operations | Sin |
> | blueprints | Sin |
> | blueprints/artifacts | Sin |
> | blueprints/versions | Sin |
> | blueprints/versions/artifacts | Sin |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | botServices | Sí |
> | botServices/channels | Sin |
> | botServices/connections | Sin |
> | languages | Sin |
> | plantillas | Sin |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | Redis | Sí |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | appliedReservations | Sin |
> | autoQuotaIncrease | Sin |
> | calculateExchange | Sin |
> | calculatePrice | Sin |
> | calculatePurchasePrice | Sin |
> | catalogs | Sin |
> | commercialReservationOrders | Sin |
> | cambio | Sin |
> | placePurchaseOrder | Sin |
> | reservationOrders | Sin |
> | reservationOrders/calculateRefund | Sin |
> | reservationOrders/merge | Sin |
> | reservationOrders/reservations | Sin |
> | reservationOrders/reservations/revisions | Sin |
> | reservationOrders/return | Sin |
> | reservationOrders/split | Sin |
> | reservationOrders/swap | Sin |
> | reservations | Sin |
> | resourceProviders | Sin |
> | resources | Sin |
> | validateReservationOrder | Sin |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Sin |
> | CdnWebApplicationFirewallPolicies | Sí |
> | edgenodes | Sin |
> | profiles | Sí |
> | profiles/endpoints | Sí |
> | profiles/endpoints/customdomains | Sin |
> | profiles/endpoints/origins | Sin |
> | validateProbe | Sin |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | certificateOrders | Sí |
> | certificateOrders/certificates | Sin |
> | validateCertificateRegistrationInformation | Sin |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | capabilities | Sin |
> | domainNames | Sí |
> | domainNames/capabilities | Sin |
> | domainNames/internalLoadBalancers | Sin |
> | domainNames/serviceCertificates | Sin |
> | domainNames/slots | Sin |
> | domainNames/slots/roles | Sin |
> | domainNames/slots/roles/metricDefinitions | Sin |
> | domainNames/slots/roles/metrics | Sin |
> | moveSubscriptionResources | Sin |
> | operatingSystemFamilies | Sin |
> | operatingSystems | Sin |
> | quotas | Sin |
> | resourceTypes | Sin |
> | validateSubscriptionMoveAvailability | Sin |
> | virtualMachines | Sí |
> | virtualMachines/diagnosticSettings | Sin |
> | virtualMachines/metricDefinitions | Sin |
> | virtualMachines/metrics | Sin |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | classicInfrastructureResources | Sin |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | capabilities | Sin |
> | expressRouteCrossConnections | Sin |
> | expressRouteCrossConnections/peerings | Sin |
> | gatewaySupportedDevices | Sin |
> | networkSecurityGroups | Sí |
> | quotas | Sin |
> | reservedIps | Sí |
> | virtualNetworks | Sí |
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | Sin |
> | virtualNetworks/virtualNetworkPeerings | Sin |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | capabilities | Sin |
> | disks | Sin |
> | images | Sin |
> | osImages | Sin |
> | osPlatformImages | Sin |
> | publicImages | Sin |
> | quotas | Sin |
> | storageAccounts | Sí |
> | storageAccounts/blobServices | Sin |
> | storageAccounts/fileServices | Sin |
> | storageAccounts/metricDefinitions | Sin |
> | storageAccounts/metrics | Sin |
> | storageAccounts/queueServices | Sin |
> | storageAccounts/services | Sin |
> | storageAccounts/services/diagnosticSettings | Sin |
> | storageAccounts/services/metricDefinitions | Sin |
> | storageAccounts/services/metrics | Sin |
> | storageAccounts/tableServices | Sin |
> | storageAccounts/vmImages | Sin |
> | vmImages | Sin |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | accounts | Sí |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | RateCard | Sin |
> | UsageAggregates | Sin |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | availabilitySets | Sí |
> | diskEncryptionSets | Sí |
> | disks | Sí |
> | galleries | Sí |
> | galleries/applications | Sin |
> | galleries/applications/versions | Sin |
> | galleries/images | Sin |
> | galleries/images/versions | Sin |
> | hostGroups | Sí |
> | hostGroups/hosts | Sí |
> | images | Sí |
> | proximityPlacementGroups | Sí |
> | restorePointCollections | Sí |
> | restorePointCollections/restorePoints | Sin |
> | sharedVMImages | Sí |
> | sharedVMImages/versions | Sin |
> | snapshots | Sí |
> | virtualMachines | Sí |
> | virtualMachines/extensions | Sí |
> | virtualMachines/metricDefinitions | Sin |
> | virtualMachineScaleSets | Sí |
> | virtualMachineScaleSets/extensions | Sin |
> | virtualMachineScaleSets/networkInterfaces | Sin |
> | virtualMachineScaleSets/publicIPAddresses | Sin |
> | virtualMachineScaleSets/virtualMachines | Sin |
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | Sin |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | AggregatedCost | Sin |
> | Saldos | Sin |
> | Presupuestos | Sin |
> | Charges | Sin |
> | CostTags | Sin |
> | credits | Sin |
> | events | Sin |
> | Previsiones | Sin |
> | lots | Sin |
> | Catálogos de soluciones | Sin |
> | Pricesheets | Sin |
> | products | Sin |
> | ReservationDetails | Sin |
> | ReservationRecommendations | Sin |
> | ReservationSummaries | Sin |
> | ReservationTransactions | Sin |
> | Etiquetas | Sin |
> | tenants | Sin |
> | Términos | Sin |
> | UsageDetails | Sin |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | containerGroups | Sí |
> | serviceAssociationLinks | Sin |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | registries | Sí |
> | registries/builds | Sin |
> | registries/builds/cancel | Sin |
> | registries/builds/getLogLink | Sin |
> | registries/buildTasks | Sí |
> | registries/buildTasks/steps | Sin |
> | registries/eventGridFilters | Sin |
> | registries/generateCredentials | Sin |
> | registries/getBuildSourceUploadUrl | Sin |
> | registries/GetCredentials | Sin |
> | registries/importImage | Sin |
> | registries/privateEndpointConnectionProxies | Sin |
> | registries/privateEndpointConnectionProxies/validate | Sin |
> | registries / privateEndpointConnections | Sin |
> | registries/privateLinkResources | Sin |
> | registries/queueBuild | Sin |
> | registries/regenerateCredential | Sin |
> | registries/regenerateCredentials | Sin |
> | registries/replications | Sí |
> | registries/runs | Sin |
> | registries/runs/cancel | Sin |
> | registries/scheduleRun | Sin |
> | registries/scopeMaps | Sin |
> | registries/taskRuns | Sí |
> | registries/tasks | Sí |
> | registries/tokens | Sin |
> | registries/updatePolicies | Sin |
> | registries/webhooks | Sí |
> | registries/webhooks/getCallbackConfig | Sin |
> | registries/webhooks/ping | Sin |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | containerServices | Sí |
> | managedClusters | Sí |
> | openShiftManagedClusters | Sí |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | accounts | Sí |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | Alertas | Sin |
> | BillingAccounts | Sin |
> | Presupuestos | Sin |
> | CloudConnectors | Sin |
> | Conectores | Sí |
> | Departments | Sin |
> | Dimensions | Sin |
> | EnrollmentAccounts | Sin |
> | Exports | Sin |
> | ExternalBillingAccounts | Sin |
> | ExternalBillingAccounts/Alerts | Sin |
> | ExternalBillingAccounts/Dimensions | Sin |
> | ExternalBillingAccounts/Forecast | Sin |
> | ExternalBillingAccounts/Query | Sin |
> | ExternalSubscriptions | Sin |
> | ExternalSubscriptions/Alerts | Sin |
> | ExternalSubscriptions/Dimensions | Sin |
> | ExternalSubscriptions/Forecast | Sin |
> | ExternalSubscriptions/Query | Sin |
> | Forecast | Sin |
> | Consultar | Sin |
> | registro | Sin |
> | Reportconfigs | Sin |
> | Informes | Sin |
> | Configuración | Sin |
> | showbackRules | Sin |
> | Vistas | Sin |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | Solicitudes | Sin |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | associations | Sin |
> | resourceProviders | Sí |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | jobs | Sí |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | DataBoxEdgeDevices | Sí |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | workspaces | Sí |
> | workspaces / dbWorkspaces | Sin |
> | workspaces / storageEncryption | Sin |
> | workspaces/virtualNetworkPeerings | Sin |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | catalogs | Sí |
> | datacatalogs | Sí |
> | datacatalogs/datasources | Sin |
> | datacatalogs/datasources/scans | Sin |
> | datacatalogs/datasources/scans/datasets | Sin |
> | datacatalogs/datasources/scans/triggers | Sin |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | dataFactories | Sí |
> | dataFactories/diagnosticSettings | Sin |
> | dataFactories/metricDefinitions | Sin |
> | dataFactorySchema | Sin |
> | factories | Sí |
> | factories/integrationRuntimes | Sin |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | accounts | Sí |
> | accounts/dataLakeStoreAccounts | Sin |
> | accounts/storageAccounts | Sin |
> | accounts/storageAccounts/containers | Sin |
> | accounts/transferAnalyticsUnits | Sin |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | accounts | Sí |
> | accounts/eventGridFilters | Sin |
> | accounts/firewallRules | Sin |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | services | Sí |
> | services/projects | Sí |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | accounts | Sí |
> | accounts/shares | Sin |
> | accounts/shares/datasets | Sin |
> | accounts/shares/invitations | Sin |
> | accounts/shares/providersharesubscriptions | Sin |
> | accounts/shares/synchronizationSettings | Sin |
> | accounts/sharesubscriptions | Sin |
> | accounts/sharesubscriptions/consumerSourceDataSets | Sin |
> | accounts/sharesubscriptions/datasetmappings | Sin |
> | accounts/sharesubscriptions/triggers | Sin |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | servers | Sí |
> | servers/advisors | Sin |
> | servers/keys | Sin |
> | servers/privateEndpointConnectionProxies | Sin |
> | servers/privateEndpointConnections | Sin |
> | servers/privateLinkResources | Sin |
> | servers/queryTexts | Sin |
> | servers/recoverableServers | Sin |
> | servers/topQueryStatistics | Sin |
> | servers/virtualNetworkRules | Sin |
> | servers/waitStatistics | Sin |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | servers | Sí |
> | servers/advisors | Sin |
> | servers/keys | Sin |
> | servers/privateEndpointConnectionProxies | Sin |
> | servers/privateEndpointConnections | Sin |
> | servers/privateLinkResources | Sin |
> | servers/queryTexts | Sin |
> | servers/recoverableServers | Sin |
> | servers/topQueryStatistics | Sin |
> | servers/virtualNetworkRules | Sin |
> | servers/waitStatistics | Sin |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | serverGroups | Sí |
> | servers | Sí |
> | servers/advisors | Sin |
> | servers/keys | Sin |
> | servers/privateEndpointConnectionProxies | Sin |
> | servers/privateEndpointConnections | Sin |
> | servers/privateLinkResources | Sin |
> | servers/queryTexts | Sin |
> | servers/recoverableServers | Sin |
> | servers/topQueryStatistics | Sin |
> | servers/virtualNetworkRules | Sin |
> | servers/waitStatistics | Sin |
> | serversv2 | Sí |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | artifactSources | Sí |
> | rollouts | Sí |
> | serviceTopologies | Sí |
> | serviceTopologies/services | Sí |
> | serviceTopologies/services/serviceUnits | Sí |
> | steps | Sí |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | applicationgroups | Sí |
> | applicationgroups/applications | Sin |
> | applicationgroups/desktops | Sin |
> | applicationgroups/startmenuitems | Sin |
> | hostpools | Sí |
> | hostpools/sessionhosts | Sin |
> | hostpools/sessionhosts/usersessions | Sin |
> | hostpools/usersessions | Sin |
> | workspaces | Sí |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | ElasticPools | Sí |
> | ElasticPools/IotHubTenants | Sí |
> | ElasticPools / IotHubTenants / securitySettings | Sin |
> | IotHubs | Sí |
> | IotHubs/eventGridFilters | Sin |
> | IotHubs / securitySettings | Sin |
> | ProvisioningServices | Sí |
> | usages | Sin |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | pipelines | Sí |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | controllers | Sí |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | labcenters | Sí |
> | labs | Sí |
> | labs/environments | Sí |
> | labs/serviceRunners | Sí |
> | labs/virtualMachines | Sí |
> | schedules | Sí |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | databaseAccountNames | Sin |
> | databaseAccounts | Sí |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | domains | Sí |
> | domains/domainOwnershipIdentifiers | Sin |
> | generateSsoRequest | Sin |
> | topLevelDomains | Sin |
> | validateDomainRegistrationInformation | Sin |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | lcsprojects | Sin |
> | lcsprojects/clouddeployments | Sin |
> | lcsprojects/connectors | Sin |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | services | Sí |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | domains | Sí |
> | domains/topics | Sin |
> | eventSubscriptions | Sin |
> | extensionTopics | Sin |
> | partnerNamespaces | Sí |
> | partnerNamespaces / eventChannels | Sin |
> | partnerRegistrations | Sí |
> | partnerTopics | Sí |
> | partnerTopics / eventSubscriptions | Sin |
> | systemTopics | Sí |
> | systemTopics / eventSubscriptions | Sin |
> | topics | Sí |
> | topicTypes | Sin |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | clusters | Sí |
> | espacios de nombres | Sí |
> | namespaces/authorizationrules | Sin |
> | namespaces/disasterrecoveryconfigs | Sin |
> | namespaces/eventhubs | Sin |
> | namespaces/eventhubs/authorizationrules | Sin |
> | namespaces/eventhubs/consumergroups | Sin |
> | namespaces/networkrulesets | Sin |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | features | Sin |
> | providers | Sin |

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | enroll | Sin |
> | galleryitems | Sin |
> | generateartifactaccessuri | Sin |
> | myareas | Sin |
> | myareas/areas | Sin |
> | myareas/areas/areas | Sin |
> | myareas/areas/areas/galleryitems | Sin |
> | myareas/areas/galleryitems | Sin |
> | myareas/galleryitems | Sin |
> | registro | Sin |
> | resources | Sin |
> | retrieveresourcesbyid | Sin |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | accounts | Sí |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | autoManagedAccounts | Sí |
> | autoManagedVmConfigurationProfiles | Sí |
> | configurationProfileAssignments | Sin |
> | guestConfigurationAssignments | Sin |
> | software | Sin |
> | softwareUpdateProfile | Sin |
> | softwareUpdates | Sin |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | hanaInstances | Sí |
> | sapMonitors | Sí |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | dedicatedHSMs | Sí |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | clusters | Sí |
> | clusters/applications | Sin |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | services | Sí |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | machines | Sí |
> | machines/extensions | Sí |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | dataManagers | Sí |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | components | Sí |
> | networkScopes | Sí |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | jobs | Sí |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | diagnosticSettings | Sin |
> | diagnosticSettingsCategories | Sin |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | appTemplates | Sin |
> | IoTApps | Sí |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | Grafo | Sí |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | deletedVaults | Sin |
> | hsmPools | Sí |
> | vaults | Sí |
> | vaults/accessPolicies | Sin |
> | vaults/eventGridFilters | Sin |
> | vaults/secrets | Sin |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | clusters | Sí |
> | clusters/attacheddatabaseconfigurations | Sin |
> | clusters/databases | Sin |
> | clusters/databases/dataconnections | Sin |
> | clusters/databases/eventhubconnections | Sin |
> | clusters / databases / principalassignments | Sin |
> | clusters / principalassignments | Sin |
> | clusters/sharedidentities | Sin |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | labaccounts | Sí |
> | users | Sin |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | hostingEnvironments | Sí |
> | integrationAccounts | Sí |
> | integrationServiceEnvironments | Sí |
> | integrationServiceEnvironments/managedApis | Sí |
> | isolatedEnvironments | Sí |
> | workflows | Sí |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | commitmentPlans | Sí |
> | webServices | Sí |
> | Áreas de trabajo | Sí |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | workspaces | Sí |
> | workspaces/computes | Sin |
> | workspaces/eventGridFilters | Sin |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | Identities | Sin |
> | userAssignedIdentities | Sí |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | marketplaceRegistrationDefinitions | Sin |
> | registrationAssignments | Sin |
> | registrationDefinitions | Sin |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | getEntities | Sin |
> | managementGroups | Sin |
> | managementGroups / settings | Sin |
> | resources | Sin |
> | startTenantBackfill | Sin |
> | tenantBackfillStatus | Sin |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | accounts | Sí |
> | accounts/eventGridFilters | Sin |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | offers | Sin |
> | offerTypes | Sin |
> | offerTypes/publishers | Sin |
> | offerTypes/publishers/offers | Sin |
> | offerTypes/publishers/offers/plans | Sin |
> | offerTypes/publishers/offers/plans/agreements | Sin |
> | offerTypes/publishers/offers/plans/configs | Sin |
> | offerTypes/publishers/offers/plans/configs/importImage | Sin |
> | privategalleryitems | Sin |
> | privateStoreClient | Sin |
> | products | Sin |
> | publishers | Sin |
> | publishers/offers | Sin |
> | publishers/offers/amendments | Sin |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | classicDevServices | Sí |
> | updateCommunicationPreference | Sin |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | agreements | Sin |
> | offertypes | Sin |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | mediaservices | Sí |
> | mediaservices/accountFilters | Sin |
> | mediaservices/assets | Sin |
> | mediaservices/assets/assetFilters | Sin |
> | mediaservices/contentKeyPolicies | Sin |
> | mediaservices/eventGridFilters | Sin |
> | mediaservices/liveEventOperations | Sin |
> | mediaservices/liveEvents | Sí |
> | mediaservices/liveEvents/liveOutputs | Sin |
> | mediaservices/liveOutputOperations | Sin |
> | mediaservices/mediaGraphs | Sin |
> | mediaservices/streamingEndpointOperations | Sin |
> | mediaservices/streamingEndpoints | Sí |
> | mediaservices/streamingLocators | Sin |
> | mediaservices/streamingPolicies | Sin |
> | mediaservices/transforms | Sin |
> | mediaservices/transforms/jobs | Sin |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | appClusters | Sí |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | assessmentProjects | Sí |
> | migrateprojects | Sí |
> | moveCollections | Sí |
> | projects | Sí |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | holographicsBroadcastAccounts | Sí |
> | objectUnderstandingAccounts | Sí |
> | remoteRenderingAccounts | Sí |
> | spatialAnchorsAccounts | Sí |
> | surfaceReconstructionAccounts | Sí |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | netAppAccounts | Sí |
> | netAppAccounts/capacityPools | Sí |
> | netAppAccounts/capacityPools/volumes | Sí |
> | netAppAccounts/capacityPools/volumes/mountTargets | Sí |
> | netAppAccounts/capacityPools/volumes/snapshots | Sí |

## <a name="microsoftnotebooks"></a>Microsoft.Notebooks

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | NotebookProxies | Sin |
## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | applicationGateways | Sí |
> | applicationGatewayWebApplicationFirewallPolicies | Sí |
> | applicationSecurityGroups | Sí |
> | azureFirewallFqdnTags | Sin |
> | azureFirewalls | Sí |
> | bastionHosts | Sí |
> | bgpServiceCommunities | Sin |
> | connections | Sí |
> | ddosCustomPolicies | Sí |
> | ddosProtectionPlans | Sí |
> | dnsOperationStatuses | Sin |
> | dnszones | Sí |
> | dnszones/A | Sin |
> | dnszones/AAAA | Sin |
> | dnszones/all | Sin |
> | dnszones/CAA | Sin |
> | dnszones/CNAME | Sin |
> | dnszones/MX | Sin |
> | dnszones/NS | Sin |
> | dnszones/PTR | Sin |
> | dnszones/recordsets | Sin |
> | dnszones/SOA | Sin |
> | dnszones/SRV | Sin |
> | dnszones/TXT | Sin |
> | expressRouteCircuits | Sí |
> | expressRouteCrossConnections | Sí |
> | expressRouteGateways | Sí |
> | expressRoutePorts | Sí |
> | expressRouteServiceProviders | Sin |
> | firewallPolicies | Sí |
> | frontdoors | Sí |
> | frontdoorWebApplicationFirewallManagedRuleSets | Sin |
> | frontdoorWebApplicationFirewallPolicies | Sí |
> | getDnsResourceReference | Sin |
> | internalNotify | Sin |
> | loadBalancers | Sí |
> | localNetworkGateways | Sí |
> | natGateways | Sí |
> | networkIntentPolicies | Sí |
> | networkInterfaces | Sí |
> | networkProfiles | Sí |
> | networkSecurityGroups | Sí |
> | networkWatchers | Sí |
> | networkWatchers/connectionMonitors | Sí |
> | networkWatchers/lenses | Sí |
> | networkWatchers/pingMeshes | Sí |
> | p2sVpnGateways | Sí |
> | privateDnsOperationStatuses | Sin |
> | privateDnsZones | Sí |
> | privateDnsZones/A | Sin |
> | privateDnsZones/AAAA | Sin |
> | privateDnsZones/all | Sin |
> | privateDnsZones/CNAME | Sin |
> | privateDnsZones/MX | Sin |
> | privateDnsZones/PTR | Sin |
> | privateDnsZones/SOA | Sin |
> | privateDnsZones/SRV | Sin |
> | privateDnsZones/TXT | Sin |
> | privateDnsZones/virtualNetworkLinks | Sí |
> | privateEndpoints | Sí |
> | privateLinkServices | Sí |
> | publicIPAddresses | Sí |
> | publicIPPrefixes | Sí |
> | routeFilters | Sí |
> | routeTables | Sí |
> | serviceEndpointPolicies | Sí |
> | trafficManagerGeographicHierarchies | Sin |
> | trafficmanagerprofiles | Sí |
> | trafficmanagerprofiles/heatMaps | Sin |
> | trafficManagerUserMetricsKeys | Sin |
> | virtualHubs | Sí |
> | virtualNetworkGateways | Sí |
> | virtualNetworks | Sí |
> | virtualNetworkTaps | Sí |
> | virtualWans | Sí |
> | vpnGateways | Sí |
> | vpnSites | Sí |
> | webApplicationFirewallPolicies | Sí |

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | espacios de nombres | Sí |
> | namespaces/notificationHubs | Sí |

## <a name="microsoftobjectstore"></a>Microsoft.ObjectStore

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | osNamespaces | Sí |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | HyperVSites | Sí |
> | ImportSites | Sí |
> | ServerSites | Sí |
> | VMwareSites | Sí |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | clusters | Sí |
> | linkTargets | Sin |
> | storageInsightConfigs | Sin |
> | workspaces | Sí |
> | workspaces / dataExports | Sin |
> | workspaces/dataSources | Sin |
> | workspaces/linkedServices | Sin |
> | workspaces / privateEndpointConnectionProxies | Sin |
> | workspaces / privateEndpointConnections | Sin |
> | workspaces / privateLinkResources | Sin |
> | workspaces/query | Sin |
> | workspaces/scopedPrivateLinkProxies | Sin |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | managementassociations | Sin |
> | managementconfigurations | Sí |
> | solutions | Sí |
> | views | Sí |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | legacyPeerings | Sin |
> | peerAsns | Sin |
> | peerings | Sí |
> | peeringServiceCountries | Sin |
> | peeringServiceProviders | Sin |
> | peeringServices | Sí |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | policyEvents | Sin |
> | policyMetadata | Sin |
> | policyStates | Sin |
> | policyTrackedResources | Sin |
> | remediations | Sin |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | consoles | Sin |
> | dashboards | Sí |
> | userSettings | Sin |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | workspaceCollections | Sí |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | capacities | Sí |

## <a name="microsoftprojectbabylon"></a>Microsoft.ProjectBabylon

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | accounts | Sí |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | backupProtectedItems | Sin |
> | vaults | Sí |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | espacios de nombres | Sí |
> | namespaces/authorizationrules | Sin |
> | namespaces/hybridconnections | Sin |
> | namespaces/hybridconnections/authorizationrules | Sin |
> | namespaces/wcfrelays | Sin |
> | namespaces/wcfrelays/authorizationrules | Sin |

## <a name="microsoftremoteapp"></a>Microsoft.RemoteApp

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | accounts | Sin |
> | collections | Sí |
> | collections/applications | Sin |
> | collections/securityprincipals | Sin |
> | templateImages | Sin |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | Consultas | Sí |
> | resourceChangeDetails | Sin |
> | resourceChanges | Sin |
> | resources | Sin |
> | resourcesHistory | Sin |
> | subscriptionsStatus | Sin |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | availabilityStatuses | Sin |
> | childAvailabilityStatuses | Sin |
> | childResources | Sin |
> | emergingissues | Sin |
> | events | Sin |
> | impactedResources | Sin |
> | metadata | Sin |
> | Notificaciones | Sin |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | deployments | Sin |
> | deployments/operations | Sin |
> | deploymentScripts | Sí |
> | deploymentScripts/logs | Sin |
> | vínculos | Sin |
> | notifyResourceJobs | Sin |
> | providers | Sin |
> | resourceGroups | Sin |
> | subscriptions | Sin |
> | tenants | Sin |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | applications | Sí |
> | saasresources | Sin |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | resourceHealthMetadata | Sin |
> | searchServices | Sí |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | adaptiveNetworkHardenings | Sin |
> | advancedThreatProtectionSettings | Sin |
> | alerts | Sin |
> | allowedConnections | Sin |
> | applicationWhitelistings | Sin |
> | assessmentMetadata | Sin |
> | assessments | Sin |
> | autoDismissAlertsRules | Sin |
> | automations | Sí |
> | AutoProvisioningSettings | Sin |
> | Compliances | Sin |
> | dataCollectionAgents | Sin |
> | deviceSecurityGroups | Sin |
> | discoveredSecuritySolutions | Sin |
> | externalSecuritySolutions | Sin |
> | InformationProtectionPolicies | Sin |
> | iotSecuritySolutions | Sí |
> | iotSecuritySolutions/analyticsModels | Sin |
> | iotSecuritySolutions/analyticsModels/aggregatedAlerts | Sin |
> | iotSecuritySolutions/analyticsModels/aggregatedRecommendations | Sin |
> | jitNetworkAccessPolicies | Sin |
> | networkData | Sin |
> | directivas | Sin |
> | pricings | Sin |
> | regulatoryComplianceStandards | Sin |
> | regulatoryComplianceStandards/regulatoryComplianceControls | Sin |
> | regulatoryComplianceStandards/regulatoryComplianceControls/regulatoryComplianceAssessments | Sin |
> | securityContacts | Sin |
> | securitySolutions | Sin |
> | securitySolutionsReferenceData | Sin |
> | securityStatuses | Sin |
> | securityStatusesSummaries | Sin |
> | serverVulnerabilityAssessments | Sin |
> | configuración | Sin |
> | subAssessments | Sin |
> | tareas | Sin |
> | topologies | Sin |
> | workspaceSettings | Sin |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | diagnosticSettings | Sin |
> | diagnosticSettingsCategories | Sin |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | aggregations | Sin |
> | alertRules | Sin |
> | alertRuleTemplates | Sin |
> | bookmarks | Sin |
> | cases | Sin |
> | dataConnectors | Sin |
> | dataConnectorsCheckRequirements | Sin |
> | entities | Sin |
> | entityQueries | Sin |
> | incidents | Sin |
> | officeConsents | Sin |
> | configuración | Sin |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | espacios de nombres | Sí |
> | namespaces/authorizationrules | Sin |
> | namespaces/disasterrecoveryconfigs | Sin |
> | namespaces/eventgridfilters | Sin |
> | namespaces/networkrulesets | Sin |
> | namespaces/queues | Sin |
> | namespaces/queues/authorizationrules | Sin |
> | namespaces/topics | Sin |
> | namespaces/topics/authorizationrules | Sin |
> | namespaces/topics/subscriptions | Sin |
> | namespaces/topics/subscriptions/rules | Sin |
> | premiumMessagingRegions | Sin |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | applications | Sí |
> | clusters | Sí |
> | clusters/applications | Sin |
> | containerGroups | Sí |
> | containerGroupSets | Sí |
> | edgeclusters | Sí |
> | edgeclusters/applications | Sin |
> | managedclusters | Sí |
> | managedclusters/nodetypes | Sin |
> | networks | Sí |
> | secretstores | Sí |
> | secretstores/certificates | Sin |
> | secretstores/secrets | Sin |
> | volumes | Sí |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | applications | Sí |
> | containerGroups | Sí |
> | gateways | Sí |
> | networks | Sí |
> | secrets | Sí |
> | volumes | Sí |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | providerRegistrations | Sin |
> | providerRegistrations/resourceTypeRegistrations | Sin |
> | rollouts | Sí |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | SignalR | Sí |
> | SignalR/eventGridFilters | Sin |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | SiteRecoveryVault | Sí |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | hybridUseBenefits | Sin |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | applicationDefinitions | Sí |
> | applications | Sí |
> | jitRequests | Sí |

## <a name="microsoftspoolservice"></a>Microsoft.SpoolService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | registeredSubscriptions | Sin |
> | spools | Sí |

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | managedInstances | Sí |
> | managedInstances/databases | Sí |
> | managedInstances/databases/backupShortTermRetentionPolicies | Sin |
> | managedInstances/databases/schemas/tables/columns/sensitivityLabels | Sin |
> | managedInstances/databases/vulnerabilityAssessments | Sin |
> | managedInstances/databases/vulnerabilityAssessments/rules/baselines | Sin |
> | managedInstances/encryptionProtector | Sin |
> | managedInstances/keys | Sin |
> | managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Sin |
> | managedInstances/vulnerabilityAssessments | Sin |
> | servers | Sí |
> | servers/administrators | Sin |
> | servers/communicationLinks | Sin |
> | servers/databases | Sí |
> | servers/encryptionProtector | Sin |
> | servers/firewallRules | Sin |
> | servers/keys | Sin |
> | servers/restorableDroppedDatabases | Sin |
> | servers/serviceobjectives | Sin |
> | servers/tdeCertificates | Sin |
> | virtualClusters | Sin |

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | SqlVirtualMachineGroups | Sí |
> | SqlVirtualMachineGroups/AvailabilityGroupListeners | Sin |
> | SqlVirtualMachines | Sí |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | storageAccounts | Sí |
> | storageAccounts/blobServices | Sin |
> | storageAccounts/fileServices | Sin |
> | storageAccounts/queueServices | Sin |
> | storageAccounts/services | Sin |
> | storageAccounts/services/metricDefinitions | Sin |
> | storageAccounts/tableServices | Sin |
> | usages | Sin |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | caches | Sí |
> | caches/storageTargets | Sin |
> | usageModels | Sin |

## <a name="microsoftstoragereplication"></a>Microsoft.StorageReplication

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | replicationGroups | Sin |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | storageSyncServices | Sí |
> | storageSyncServices/registeredServers | Sin |
> | storageSyncServices/syncGroups | Sin |
> | storageSyncServices/syncGroups/cloudEndpoints | Sin |
> | storageSyncServices/syncGroups/serverEndpoints | Sin |
> | storageSyncServices/workflows | Sin |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | storageSyncServices | Sí |
> | storageSyncServices/registeredServers | Sin |
> | storageSyncServices/syncGroups | Sin |
> | storageSyncServices/syncGroups/cloudEndpoints | Sin |
> | storageSyncServices/syncGroups/serverEndpoints | Sin |
> | storageSyncServices/workflows | Sin |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | storageSyncServices | Sí |
> | storageSyncServices/registeredServers | Sin |
> | storageSyncServices/syncGroups | Sin |
> | storageSyncServices/syncGroups/cloudEndpoints | Sin |
> | storageSyncServices/syncGroups/serverEndpoints | Sin |
> | storageSyncServices/workflows | Sin |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | managers | Sí |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | streamingjobs | Sí |

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | cancel | Sin |
> | CreateSubscription | Sin |
> | enable | Sin |
> | rename | Sin |
> | SubscriptionDefinitions | Sin |
> | SubscriptionOperations | Sin |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | environments | Sí |
> | environments/accessPolicies | Sin |
> | environments/eventsources | Sí |
> | environments/referenceDataSets | Sí |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | dedicatedCloudNodes | Sí |
> | dedicatedCloudServices | Sí |
> | virtualMachines | Sí |

## <a name="microsoftvnfmanager"></a>Microsoft.VnfManager

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | devices | Sí |
> | registeredSubscriptions | Sin |
> | vendors | Sin |
> | vendors / skus | Sin |
> | vendors/vnfs | Sin |
> | vnfs | Sí |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | apiManagementAccounts | Sin |
> | apiManagementAccounts/apiAcls | Sin |
> | apiManagementAccounts/apis | Sin |
> | apiManagementAccounts/apis/apiAcls | Sin |
> | apiManagementAccounts/apis/connectionAcls | Sin |
> | apiManagementAccounts/apis/connections | Sin |
> | apiManagementAccounts/apis/connections/connectionAcls | Sin |
> | apiManagementAccounts/apis/localizedDefinitions | Sin |
> | apiManagementAccounts/connectionAcls | Sin |
> | apiManagementAccounts/connections | Sin |
> | billingMeters | Sin |
> | certificates | Sí |
> | connectionGateways | Sí |
> | connections | Sí |
> | customApis | Sí |
> | deletedSites | Sin |
> | hostingEnvironments | Sí |
> | hostingEnvironments / eventGridFilters | Sin |
> | hostingEnvironments/multiRolePools | Sin |
> | hostingEnvironments/workerPools | Sin |
> | publishingUsers | Sin |
> | de películas | Sin |
> | resourceHealthMetadata | Sin |
> | runtimes | Sin |
> | serverFarms | Sí |
> | serverFarms/eventGridFilters | Sin |
> | sites | Sí |
> | sites/config  | Sin |
> | sites/eventGridFilters | Sin |
> | sites/hostNameBindings | Sin |
> | sites/networkConfig | Sin |
> | sites/premieraddons | Sí |
> | sites/slots | Sí |
> | sites/slots/eventGridFilters | Sin |
> | sites/slots/hostNameBindings | Sin |
> | sites/slots/networkConfig | Sin |
> | sourceControls | Sin |
> | staticSites | Sí |
> | validate | Sin |
> | verifyHostingEnvironmentVnet | Sin |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | diagnosticSettings | Sin |
> | diagnosticSettingsCategories | Sin |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | DeviceServices | Sí |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Eliminación del modo completo |
> | ------------- | ----------- |
> | components | Sin |
> | componentsSummary | Sin |
> | monitorInstances | Sin |
> | monitorInstancesSummary | Sin |
> | monitors | Sin |
> | notificationSettings | Sin |

## <a name="next-steps"></a>Pasos siguientes

Para obtener los mismos datos que un archivo de valores separados por comas, descargue [complete-mode-deletion.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/complete-mode-deletion.csv).
