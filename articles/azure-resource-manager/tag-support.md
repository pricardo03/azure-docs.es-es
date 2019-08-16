---
title: Compatibilidad de etiquetas de Azure Resource Manager para los recursos
description: Muestra los tipos de recursos de Azure que admiten etiquetas. Proporciona detalles de todos los servicios de Azure.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: reference
ms.date: 08/05/2019
ms.author: tomfitz
ms.openlocfilehash: 0e8df116a82f73af2348651a91aee3c34a595c45
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68814086"
---
# <a name="tag-support-for-azure-resources"></a>Compatibilidad de etiquetas de los recursos de Azure
En este artículo se describe si un tipo de recurso admite [etiquetas](resource-group-using-tags.md). La columna con la etiqueta **Admite etiquetas** indica si el tipo de recurso tiene una propiedad para la etiqueta. La columna con la etiqueta **Etiqueta en el informe de costos** indica si ese tipo de recurso pasa la etiqueta al informe de costos.

Para obtener los mismos datos como un archivo de valores separados por comas, descargue [tag-support.csv](https://github.com/tfitzmac/resource-capabilities/blob/master/tag-support.csv).

Vaya a un espacio de nombres del proveedor de recursos:
> [!div class="op_single_selector"]
> - [Microsoft.AAD](#microsoftaad)
> - [Microsoft.AADDomainServices](#microsoftaaddomainservices)
> - [Microsoft.Addons](#microsoftaddons)
> - [Microsoft.ADHybridHealthService](#microsoftadhybridhealthservice)
> - [Microsoft.Advisor](#microsoftadvisor)
> - [Microsoft.AlertsManagement](#microsoftalertsmanagement)
> - [Microsoft.AnalysisServices](#microsoftanalysisservices)
> - [Microsoft.ApiManagement](#microsoftapimanagement)
> - [Microsoft.AppConfiguration](#microsoftappconfiguration)
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
> - [Microsoft.BizTalkServices](#microsoftbiztalkservices)
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
> - [Microsoft.ContentModerator](#microsoftcontentmoderator)
> - [Microsoft.CortanaAnalytics](#microsoftcortanaanalytics)
> - [Microsoft.CostManagement](#microsoftcostmanagement)
> - [Microsoft.CustomerInsights](#microsoftcustomerinsights)
> - [Microsoft.CustomerLockbox](#microsoftcustomerlockbox)
> - [Microsoft.CustomProviders](#microsoftcustomproviders)
> - [Microsoft.DataBox](#microsoftdatabox)
> - [Microsoft.DataBoxEdge](#microsoftdataboxedge)
> - [Microsoft.Databricks](#microsoftdatabricks)
> - [Microsoft.DataCatalog](#microsoftdatacatalog)
> - [Microsoft.DataConnect](#microsoftdataconnect)
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
> - [Microsoft.ManagedLab](#microsoftmanagedlab)
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
> - [Microsoft.Network](#microsoftnetwork)
> - [Microsoft.NotificationHubs](#microsoftnotificationhubs)
> - [Microsoft.OffAzure](#microsoftoffazure)
> - [Microsoft.OperationalInsights](#microsoftoperationalinsights)
> - [Microsoft.OperationsManagement](#microsoftoperationsmanagement)
> - [Microsoft.Peering](#microsoftpeering)
> - [Microsoft.PolicyInsights](#microsoftpolicyinsights)
> - [Microsoft.Portal](#microsoftportal)
> - [Microsoft.PowerBI](#microsoftpowerbi)
> - [Microsoft.PowerBIDedicated](#microsoftpowerbidedicated)
> - [Microsoft.RecoveryServices](#microsoftrecoveryservices)
> - [Microsoft.Relay](#microsoftrelay)
> - [Microsoft.RemoteApp](#microsoftremoteapp)
> - [Microsoft.ResourceGraph](#microsoftresourcegraph)
> - [Microsoft.ResourceHealth](#microsoftresourcehealth)
> - [Microsoft.Resources](#microsoftresources)
> - [Microsoft.SaaS](#microsoftsaas)
> - [Microsoft.Scheduler](#microsoftscheduler)
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
> - [Microsoft.Web](#microsoftweb)
> - [Microsoft.WindowsDefenderATP](#microsoftwindowsdefenderatp)
> - [Microsoft.WindowsIoT](#microsoftwindowsiot)
> - [Microsoft.WorkloadMonitor](#microsoftworkloadmonitor)

## <a name="microsoftaad"></a>Microsoft.AAD

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | DomainServices | Sí |Sí |
> | DomainServices/oucontainer | Sin |Sin |
> | DomainServices/ReplicaSets | Sí |Sí |

## <a name="microsoftaaddomainservices"></a>Microsoft.AADDomainServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | domains | Sin |Sin |

## <a name="microsoftaddons"></a>Microsoft.Addons

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | supportProviders | Sin |Sin |

## <a name="microsoftadhybridhealthservice"></a>Microsoft.ADHybridHealthService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | aadsupportcases | Sin |Sin |
> | addsservices | Sin |Sin |
> | agents | Sin |Sin |
> | anonymousapiusers | Sin |Sin |
> | configuración | Sin |Sin |
> | logs | Sin |Sin |
> | reports | Sin |Sin |
> | servicehealthmetrics | Sin |Sin |
> | services | Sin |Sin |

## <a name="microsoftadvisor"></a>Microsoft.Advisor

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | configuraciones | Sin |Sin |
> | generateRecommendations | Sin |Sin |
> | metadata | Sin |Sin |
> | de películas | Sin |Sin |
> | suppressions | Sin |Sin |

## <a name="microsoftalertsmanagement"></a>Microsoft.AlertsManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | actionRules | Sí |Sí |
> | alerts | Sin |Sin |
> | alertsList | Sin |Sin |
> | alertsMetaData | Sin |Sin |
> | alertsSummary | Sin |Sin |
> | alertsSummaryList | Sin |Sin |
> | feedback | Sin |Sin |
> | smartDetectorAlertRules | Sin |Sin |
> | smartDetectorRuntimeEnvironments | Sin |Sin |
> | smartGroups | Sin |Sin |

## <a name="microsoftanalysisservices"></a>Microsoft.AnalysisServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | servers | Sí |Sí |

## <a name="microsoftapimanagement"></a>Microsoft.ApiManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | reportFeedback | Sin |Sin |
> | service | SÍ |Sí |
> | validateServiceName | Sin |Sin |

## <a name="microsoftappconfiguration"></a>Microsoft.AppConfiguration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | configurationStores | Sí |Sí |
> | configurationStores/eventGridFilters | Sin |Sin |

## <a name="microsoftattestation"></a>Microsoft.Attestation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | attestationProviders | Sin |Sin |

## <a name="microsoftauthorization"></a>Microsoft.Authorization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | classicAdministrators | Sin |Sin |
> | dataAliases | Sin |Sin |
> | denyAssignments | Sin |Sin |
> | elevateAccess | Sin |Sin |
> | locks | Sin |Sin |
> | permisos | Sin |Sin |
> | policyAssignments | Sin |Sin |
> | policyDefinitions | Sin |Sin |
> | policySetDefinitions | Sin |Sin |
> | providerOperations | Sin |Sin |
> | roleAssignments | Sin |Sin |
> | roleDefinitions | Sin |Sin |

## <a name="microsoftautomation"></a>Microsoft.Automation

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | automationAccounts | Sí |Sí |
> | automationAccounts/configurations | Sí |Sí |
> | automationAccounts/jobs | Sin |Sin |
> | automationAccounts/runbooks | Sí |Sí |
> | automationAccounts/softwareUpdateConfigurations | Sin |Sin |
> | automationAccounts/webhooks | Sin |Sin |

## <a name="microsoftazconfig"></a>Microsoft.Azconfig

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | configurationStores | Sí |Sí |
> | configurationStores/eventGridFilters | Sin |Sin |

## <a name="microsoftazuregeneva"></a>Microsoft.Azure.Geneva

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | environments | Sin |Sin |
> | environments/accounts | Sin |Sin |
> | environments/accounts/namespaces | Sin |Sin |
> | environments/accounts/namespaces/configurations | Sin |Sin |

## <a name="microsoftazureactivedirectory"></a>Microsoft.AzureActiveDirectory

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | b2cDirectories | Sí |Sin |
> | b2ctenants | Sin |Sin |

## <a name="microsoftazuredata"></a>Microsoft.AzureData

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | sqlServerRegistrations | Sí |Sí |
> | sqlServerRegistrations/sqlServers | Sin |Sin |

## <a name="microsoftazurestack"></a>Microsoft.AzureStack

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | registrations | Sí |Sí |
> | registrations/customerSubscriptions | Sin |Sin |
> | registrations/products | Sin |Sin |

## <a name="microsoftbatch"></a>Microsoft.Batch

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | batchAccounts | Sí |Sí |

## <a name="microsoftbilling"></a>Microsoft.Billing

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | billingAccounts | Sin |Sin |
> | billingAccounts/agreements | Sin |Sin |
> | billingAccounts/billingProfiles | Sin |Sin |
> | billingAccounts/billingProfiles/billingSubscriptions | Sin |Sin |
> | billingAccounts/billingProfiles/invoices | Sin |Sin |
> | billingAccounts/billingProfiles/invoices/pricesheet | Sin |Sin |
> | billingAccounts/billingProfiles/invoiceSections | Sin |Sin |
> | billingAccounts/BillingProfiles/patchOperations | Sin |Sin |
> | billingAccounts/billingProfiles/paymentMethods | Sin |Sin |
> | billingAccounts/billingProfiles/policies | Sin |Sin |
> | billingAccounts/billingProfiles/pricesheet | Sin |Sin |
> | billingAccounts/billingProfiles/pricesheetDownloadOperations | Sin |Sin |
> | billingAccounts/billingProfiles/products | Sin |Sin |
> | billingAccounts/billingProfiles/transactions | Sin |Sin |
> | billingAccounts/billingSubscriptions | Sin |Sin |
> | billingAccounts/createInvoiceSectionOperations | Sin |Sin |
> | billingAccounts/customers | Sin |Sin |
> | billingAccounts/customers/billingSubscriptions | Sin |Sin |
> | billingAccounts/departments | Sin |Sin |
> | billingAccounts/enrollmentAccounts | Sin |Sin |
> | billingAccounts/invoices | Sin |Sin |
> | billingAccounts/invoiceSections | Sin |Sin |
> | billingAccounts/invoiceSections/billingSubscriptionMoveOperations | Sin |Sin |
> | billingAccounts/invoiceSections/billingSubscriptions | Sin |Sin |
> | billingAccounts/invoiceSections/billingSubscriptions/transfer | Sin |Sin |
> | billingAccounts/invoiceSections/elevate | Sin |Sin |
> | billingAccounts/invoiceSections/initiateTransfer | Sin |Sin |
> | billingAccounts/invoiceSections/patchOperations | Sin |Sin |
> | billingAccounts/invoiceSections/productMoveOperations | Sin |Sin |
> | billingAccounts/invoiceSections/products | Sin |Sin |
> | billingAccounts/invoiceSections/products/transfer | Sin |Sin |
> | billingAccounts/invoiceSections/products/updateAutoRenew | Sin |Sin |
> | billingAccounts/invoiceSections/transactions | Sin |Sin |
> | billingAccounts/invoiceSections/transfers | Sin |Sin |
> | billingAccounts/lineOfCredit | Sin |Sin |
> | billingAccounts/patchOperations | Sin |Sin |
> | billingAccounts/paymentMethods | Sin |Sin |
> | billingAccounts/products | Sin |Sin |
> | billingAccounts/transactions | Sin |Sin |
> | billingPeriods | Sin |Sin |
> | billingPermissions | Sin |Sin |
> | billingProperty | Sin |Sin |
> | billingRoleAssignments | Sin |Sin |
> | billingRoleDefinitions | Sin |Sin |
> | createBillingRoleAssignment | Sin |Sin |
> | departments | Sin |Sin |
> | enrollmentAccounts | Sin |Sin |
> | invoices | Sin |Sin |
> | transfers | Sin |Sin |
> | transfers/acceptTransfer | Sin |Sin |
> | transfers/declineTransfer | Sin |Sin |
> | transfers/operationStatus | Sin |Sin |
> | validateAddress | Sin |Sin |

## <a name="microsoftbingmaps"></a>Microsoft.BingMaps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | mapApis | Sí |Sí |
> | updateCommunicationPreference | Sin |Sin |

## <a name="microsoftbiztalkservices"></a>Microsoft.BizTalkServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | BizTalk | Sí |Sí |

## <a name="microsoftblockchain"></a>Microsoft.Blockchain

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | blockchainMembers | Sí |Sí |
> | watchers | Sí |Sí |

## <a name="microsoftblueprint"></a>Microsoft.Blueprint

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | blueprintAssignments | Sin |Sin |
> | blueprintAssignments/assignmentOperations | Sin |Sin |
> | blueprintAssignments/operations | Sin |Sin |
> | blueprints | Sin |Sin |
> | blueprints/artifacts | Sin |Sin |
> | blueprints/versions | Sin |Sin |
> | blueprints/versions/artifacts | Sin |Sin |

## <a name="microsoftbotservice"></a>Microsoft.BotService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | botServices | Sí |Sí |
> | botServices/channels | Sin |Sin |
> | botServices/connections | Sin |Sin |
> | languages | Sin |Sin |
> | plantillas | Sin |Sin |

## <a name="microsoftcache"></a>Microsoft.Cache

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | Redis | Sí |Sí |
> | RedisConfigDefinition | Sin |Sin |

## <a name="microsoftcapacity"></a>Microsoft.Capacity

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | appliedReservations | Sin |Sin |
> | calculateExchange | Sin |Sin |
> | calculatePrice | Sin |Sin |
> | calculatePurchasePrice | Sin |Sin |
> | catalogs | Sin |Sin |
> | commercialReservationOrders | Sin |Sin |
> | cambio | Sin |Sin |
> | placePurchaseOrder | Sin |Sin |
> | reservationOrders | Sin |Sin |
> | reservationOrders/calculateRefund | Sin |Sin |
> | reservationOrders/merge | Sin |Sin |
> | reservationOrders/reservations | Sin |Sin |
> | reservationOrders/reservations/revisions | Sin |Sin |
> | reservationOrders/return | Sin |Sin |
> | reservationOrders/split | Sin |Sin |
> | reservationOrders/swap | Sin |Sin |
> | reservations | Sin |Sin |
> | resources | Sin |Sin |
> | validateReservationOrder | Sin |Sin |

## <a name="microsoftcdn"></a>Microsoft.Cdn

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | CdnWebApplicationFirewallManagedRuleSets | Sin |Sin |
> | CdnWebApplicationFirewallPolicies | Sí |Sí |
> | edgenodes | Sin |Sin |
> | profiles | Sí |Sí |
> | profiles/endpoints | Sí |Sí |
> | profiles/endpoints/customdomains | Sin |Sin |
> | profiles/endpoints/origins | Sin |Sin |
> | validateProbe | Sin |Sin |

## <a name="microsoftcertificateregistration"></a>Microsoft.CertificateRegistration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | certificateOrders | Sí |Sí |
> | certificateOrders/certificates | Sin |Sin |
> | validateCertificateRegistrationInformation | Sin |Sin |

## <a name="microsoftclassiccompute"></a>Microsoft.ClassicCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | capabilities | Sin |Sin |
> | domainNames | Sí |Sí |
> | domainNames/capabilities | Sin |Sin |
> | domainNames/internalLoadBalancers | Sin |Sin |
> | domainNames/serviceCertificates | Sin |Sin |
> | domainNames/slots | Sin |Sin |
> | domainNames/slots/roles | Sin |Sin |
> | domainNames/slots/roles/metricDefinitions | Sin |Sin |
> | domainNames/slots/roles/metrics | Sin |Sin |
> | moveSubscriptionResources | Sin |Sin |
> | operatingSystemFamilies | Sin |Sin |
> | operatingSystems | Sin |Sin |
> | quotas | Sin |Sin |
> | resourceTypes | Sin |Sin |
> | validateSubscriptionMoveAvailability | Sin |Sin |
> | virtualMachines | Sí |Sí |
> | virtualMachines/diagnosticSettings | Sin |Sin |
> | virtualMachines/metricDefinitions | Sin |Sin |
> | virtualMachines/metrics | Sin |Sin |

## <a name="microsoftclassicinfrastructuremigrate"></a>Microsoft.ClassicInfrastructureMigrate

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | classicInfrastructureResources | Sin |Sin |

## <a name="microsoftclassicnetwork"></a>Microsoft.ClassicNetwork

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | capabilities | Sin |Sin |
> | expressRouteCrossConnections | Sin |Sin |
> | expressRouteCrossConnections/peerings | Sin |Sin |
> | gatewaySupportedDevices | Sin |Sin |
> | networkSecurityGroups | Sí |Sí |
> | quotas | Sin |Sin |
> | reservedIps | Sí |Sí |
> | virtualNetworks | Sí |Sí |
> | virtualNetworks/remoteVirtualNetworkPeeringProxies | Sin |Sin |
> | virtualNetworks/virtualNetworkPeerings | Sin |Sin |

## <a name="microsoftclassicstorage"></a>Microsoft.ClassicStorage

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | capabilities | Sin |Sin |
> | disks | Sin |Sin |
> | images | Sin |Sin |
> | osImages | Sin |Sin |
> | osPlatformImages | Sin |Sin |
> | publicImages | Sin |Sin |
> | quotas | Sin |Sin |
> | storageAccounts | Sí |Sí |
> | storageAccounts/metricDefinitions | Sin |Sin |
> | storageAccounts/metrics | Sin |Sin |
> | storageAccounts/services | Sin |Sin |
> | storageAccounts/services/diagnosticSettings | Sin |Sin |
> | storageAccounts/services/metricDefinitions | Sin |Sin |
> | storageAccounts/services/metrics | Sin |Sin |
> | storageAccounts/vmImages | Sin |Sin |
> | vmImages | Sin |Sin |

## <a name="microsoftcognitiveservices"></a>Microsoft.CognitiveServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | accounts | Sí |Sí |

## <a name="microsoftcommerce"></a>Microsoft.Commerce

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | RateCard | Sin |Sin |
> | UsageAggregates | Sin |Sin |

## <a name="microsoftcompute"></a>Microsoft.Compute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | availabilitySets | Sí |Sí |
> | diskEncryptionSets | Sí |Sí |
> | disks | Sí |Sí |
> | galleries | Sí |Sí |
> | galleries/applications | Sí |Sí |
> | galleries/applications/versions | Sí |Sí |
> | galleries/images | Sí |Sí |
> | galleries/images/versions | Sí |Sí |
> | hostGroups | Sí |Sí |
> | hostGroups/hosts | Sí |Sí |
> | images | Sí |Sí |
> | proximityPlacementGroups | Sí |Sí |
> | restorePointCollections | Sí |Sí |
> | restorePointCollections/restorePoints | Sin |Sin |
> | sharedVMImages | Sí |Sí |
> | sharedVMImages/versions | Sí |Sí |
> | snapshots | Sí |Sí |
> | virtualMachines | Sí |Sí |
> | virtualMachines/extensions | Sí |Sí |
> | virtualMachines/metricDefinitions | Sin |Sin |
> | virtualMachines/scriptJobs | Sin |Sin |
> | virtualMachines/softwareUpdateDeployments | Sin |Sin |
> | virtualMachineScaleSets | Sí |Sí |
> | virtualMachineScaleSets/extensions | Sin |Sin |
> | virtualMachineScaleSets/networkInterfaces | Sin |Sin |
> | virtualMachineScaleSets/publicIPAddresses | Sin |Sin |
> | virtualMachineScaleSets/virtualMachines | Sin |Sin |
> | virtualMachineScaleSets/virtualMachines/networkInterfaces | Sin |Sin |

## <a name="microsoftconsumption"></a>Microsoft.Consumption

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | AggregatedCost | Sin |Sin |
> | Saldos | Sin |Sin |
> | Presupuestos | Sin |Sin |
> | Charges | Sin |Sin |
> | CostTags | Sin |Sin |
> | credits | Sin |Sin |
> | events | Sin |Sin |
> | Previsiones | Sin |Sin |
> | lots | Sin |Sin |
> | Catálogos de soluciones | Sin |Sin |
> | Pricesheets | Sin |Sin |
> | products | Sin |Sin |
> | ReservationDetails | Sin |Sin |
> | ReservationRecommendations | Sin |Sin |
> | ReservationSummaries | Sin |Sin |
> | ReservationTransactions | Sin |Sin |
> | Etiquetas | Sin |Sin |
> | tenants | Sin |Sin |
> | Términos | Sin |Sin |
> | UsageDetails | Sin |Sin |

## <a name="microsoftcontainerinstance"></a>Microsoft.ContainerInstance

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | containerGroups | Sí |Sí |
> | serviceAssociationLinks | Sin |Sin |

## <a name="microsoftcontainerregistry"></a>Microsoft.ContainerRegistry

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | registries | Sí |Sí |
> | registries/builds | Sin |Sin |
> | registries/builds/cancel | Sin |Sin |
> | registries/builds/getLogLink | Sin |Sin |
> | registries/buildTasks | Sí |Sí |
> | registries/buildTasks/steps | Sin |Sin |
> | registries/eventGridFilters | Sin |Sin |
> | registries/getBuildSourceUploadUrl | Sin |Sin |
> | registries/GetCredentials | Sin |Sin |
> | registries/importImage | Sin |Sin |
> | registries/queueBuild | Sin |Sin |
> | registries/regenerateCredential | Sin |Sin |
> | registries/regenerateCredentials | Sin |Sin |
> | registries/replications | Sí |Sí |
> | registries/runs | Sin |Sin |
> | registries/runs/cancel | Sin |Sin |
> | registries/scheduleRun | Sin |Sin |
> | registries/tasks | Sí |Sí |
> | registries/updatePolicies | Sin |Sin |
> | registries/webhooks | Sí |Sí |
> | registries/webhooks/getCallbackConfig | Sin |Sin |
> | registries/webhooks/ping | Sin |Sin |

## <a name="microsoftcontainerservice"></a>Microsoft.ContainerService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | containerServices | Sí |Sí |
> | managedClusters | Sí |Sí |
> | openShiftManagedClusters | Sí |Sí |

## <a name="microsoftcontentmoderator"></a>Microsoft.ContentModerator

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | applications | Sí |Sí |
> | updateCommunicationPreference | Sin |Sin |

## <a name="microsoftcortanaanalytics"></a>Microsoft.CortanaAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | accounts | Sí |Sí |

## <a name="microsoftcostmanagement"></a>Microsoft.CostManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | Alertas | Sin |Sin |
> | BillingAccounts | Sin |Sin |
> | Presupuestos | Sin |Sin |
> | CloudConnectors | Sin |Sin |
> | Conectores | Sí |Sí |
> | Departments | Sin |Sin |
> | Dimensiones | Sin |Sin |
> | EnrollmentAccounts | Sin |Sin |
> | Exports | Sin |Sin |
> | ExternalBillingAccounts | Sin |Sin |
> | ExternalBillingAccounts/Alerts | Sin |Sin |
> | ExternalBillingAccounts/Dimensions | Sin |Sin |
> | ExternalBillingAccounts/Forecast | Sin |Sin |
> | ExternalBillingAccounts/Query | Sin |Sin |
> | ExternalSubscriptions | Sin |Sin |
> | ExternalSubscriptions/Alerts | Sin |Sin |
> | ExternalSubscriptions/Dimensions | Sin |Sin |
> | ExternalSubscriptions/Forecast | Sin |Sin |
> | ExternalSubscriptions/Query | Sin |Sin |
> | Forecast | Sin |Sin |
> | Consultar | Sin |Sin |
> | registro | Sin |Sin |
> | Reportconfigs | Sin |Sin |
> | Informes | Sin |Sin |
> | Configuración | Sin |Sin |
> | showbackRules | Sin |Sin |
> | Vistas | Sin |Sin |

## <a name="microsoftcustomerinsights"></a>Microsoft.CustomerInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | hubs | Sí |Sí |
> | hubs/authorizationPolicies | Sin |Sin |
> | hubs/connectors | Sin |Sin |
> | hubs/connectors/mappings | Sin |Sin |
> | hubs/interactions | Sin |Sin |
> | hubs/kpi | Sin |Sin |
> | hubs/links | Sin |Sin |
> | hubs/profiles | Sin |Sin |
> | hubs/roleAssignments | Sin |Sin |
> | hubs/roles | Sin |Sin |
> | hubs/suggestTypeSchema | Sin |Sin |
> | hubs/views | Sin |Sin |
> | hubs/widgetTypes | Sin |Sin |

## <a name="microsoftcustomerlockbox"></a>Microsoft.CustomerLockbox

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | Solicitudes | Sin |Sin |

## <a name="microsoftcustomproviders"></a>Microsoft.CustomProviders

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | associations | Sin |Sin |
> | resourceProviders | Sí |Sí |

## <a name="microsoftdatabox"></a>Microsoft.DataBox

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | jobs | Sí |Sí |

## <a name="microsoftdataboxedge"></a>Microsoft.DataBoxEdge

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | DataBoxEdgeDevices | Sí |Sí |

## <a name="microsoftdatabricks"></a>Microsoft.Databricks

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | workspaces | Sí |Sin |
> | workspaces/virtualNetworkPeerings | Sin |Sin |

## <a name="microsoftdatacatalog"></a>Microsoft.DataCatalog

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | catalogs | Sí |Sí |
> | datacatalogs | Sí |Sí |
> | datacatalogs/scantargets | Sin |Sin |
> | datacatalogs/scantargets/datasets | Sin |Sin |

## <a name="microsoftdataconnect"></a>Microsoft.DataConnect

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | connectionManagers | Sí |Sí |

## <a name="microsoftdatafactory"></a>Microsoft.DataFactory

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | dataFactories | Sí |Sin |
> | dataFactories/diagnosticSettings | Sin |Sin |
> | dataFactories/metricDefinitions | Sin |Sin |
> | dataFactorySchema | Sin |Sin |
> | factories | Sí |Sin |
> | factories/integrationRuntimes | Sin |Sin |

## <a name="microsoftdatalakeanalytics"></a>Microsoft.DataLakeAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | accounts | Sí |Sí |
> | accounts/dataLakeStoreAccounts | Sin |Sin |
> | accounts/storageAccounts | Sin |Sin |
> | accounts/storageAccounts/containers | Sin |Sin |
> | accounts/transferAnalyticsUnits | Sin |Sin |

## <a name="microsoftdatalakestore"></a>Microsoft.DataLakeStore

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | accounts | Sí |Sí |
> | accounts/eventGridFilters | Sin |Sin |
> | accounts/firewallRules | Sin |Sin |

## <a name="microsoftdatamigration"></a>Microsoft.DataMigration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | services | Sí |Sí |
> | services/projects | Sí |Sí |
> | slots | Sí |Sí |

## <a name="microsoftdatashare"></a>Microsoft.DataShare

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | accounts | Sí |Sí |
> | accounts/shares | Sin |Sin |
> | accounts/shares/datasets | Sin |Sin |
> | accounts/shares/invitations | Sin |Sin |
> | accounts/shares/providersharesubscriptions | Sin |Sin |
> | accounts/shares/synchronizationSettings | Sin |Sin |
> | accounts/sharesubscriptions | Sin |Sin |
> | accounts/sharesubscriptions/consumerSourceDataSets | Sin |Sin |
> | accounts/sharesubscriptions/datasetmappings | Sin |Sin |
> | accounts/sharesubscriptions/triggers | Sin |Sin |

## <a name="microsoftdbformariadb"></a>Microsoft.DBforMariaDB

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | servers | Sí |Sí |
> | servers/advisors | Sin |Sin |
> | servers/queryTexts | Sin |Sin |
> | servers/recoverableServers | Sin |Sin |
> | servers/topQueryStatistics | Sin |Sin |
> | servers/virtualNetworkRules | Sin |Sin |
> | servers/waitStatistics | Sin |Sin |

## <a name="microsoftdbformysql"></a>Microsoft.DBforMySQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | servers | Sí |Sí |
> | servers/advisors | Sin |Sin |
> | servers/queryTexts | Sin |Sin |
> | servers/recoverableServers | Sin |Sin |
> | servers/topQueryStatistics | Sin |Sin |
> | servers/virtualNetworkRules | Sin |Sin |
> | servers/waitStatistics | Sin |Sin |

## <a name="microsoftdbforpostgresql"></a>Microsoft.DBforPostgreSQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | serverGroups | Sí |Sí |
> | servers | Sí |Sí |
> | servers/advisors | Sin |Sin |
> | servers/queryTexts | Sin |Sin |
> | servers/recoverableServers | Sin |Sin |
> | servers/topQueryStatistics | Sin |Sin |
> | servers/virtualNetworkRules | Sin |Sin |
> | servers/waitStatistics | Sin |Sin |
> | serversv2 | Sí |Sí |

## <a name="microsoftdeploymentmanager"></a>Microsoft.DeploymentManager

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | artifactSources | Sí |Sí |
> | rollouts | Sí |Sí |
> | serviceTopologies | Sí |Sí |
> | serviceTopologies/services | Sí |Sí |
> | serviceTopologies/services/serviceUnits | Sí |Sí |
> | steps | Sí |Sí |

## <a name="microsoftdesktopvirtualization"></a>Microsoft.DesktopVirtualization

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | applicationgroups | Sí |Sí |
> | applicationgroups/applications | Sin |Sin |
> | applicationgroups/assignedusers | Sin |Sin |
> | applicationgroups/startmenuitems | Sin |Sin |
> | hostpools | Sí |Sí |
> | hostpools/sessionhosts | Sin |Sin |
> | hostpools/sessionhosts/usersessions | Sin |Sin |
> | hostpools/usersessions | Sin |Sin |
> | workspaces | Sí |Sí |

## <a name="microsoftdevices"></a>Microsoft.Devices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | ElasticPools | Sí |Sí |
> | ElasticPools/IotHubTenants | Sí |Sí |
> | IotHubs | Sí |Sí |
> | IotHubs/eventGridFilters | Sin |Sin |
> | ProvisioningServices | Sí |Sí |
> | usages | Sin |Sin |

## <a name="microsoftdevops"></a>Microsoft.DevOps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | pipelines | Sí |Sí |

## <a name="microsoftdevspaces"></a>Microsoft.DevSpaces

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | controllers | Sí |Sí |

## <a name="microsoftdevtestlab"></a>Microsoft.DevTestLab

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | labcenters | Sí |Sí |
> | labs | Sí |Sí |
> | labs/environments | Sí |Sí |
> | labs/serviceRunners | Sí |Sí |
> | labs/virtualMachines | Sí |Sí |
> | schedules | Sí |Sí |

## <a name="microsoftdocumentdb"></a>Microsoft.DocumentDB

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | databaseAccountNames | Sin |Sin |
> | databaseAccounts | Sí |Sí |

## <a name="microsoftdomainregistration"></a>Microsoft.DomainRegistration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | domains | Sí |Sí |
> | domains/domainOwnershipIdentifiers | Sin |Sin |
> | generateSsoRequest | Sin |Sin |
> | topLevelDomains | Sin |Sin |
> | validateDomainRegistrationInformation | Sin |Sin |

## <a name="microsoftdynamicslcs"></a>Microsoft.DynamicsLcs

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | lcsprojects | Sin |Sin |
> | lcsprojects/clouddeployments | Sin |Sin |
> | lcsprojects/connectors | Sin |Sin |

## <a name="microsoftenterpriseknowledgegraph"></a>Microsoft.EnterpriseKnowledgeGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | services | Sí |Sí |

## <a name="microsofteventgrid"></a>Microsoft.EventGrid

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | domains | Sí |Sí |
> | domains/topics | Sin |Sin |
> | eventSubscriptions | Sin |Sin |
> | extensionTopics | Sin |Sin |
> | topics | Sí |Sí |
> | topicTypes | Sin |Sin |

## <a name="microsofteventhub"></a>Microsoft.EventHub

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | clusters | Sí |Sí |
> | namespaces | Sí |Sí |
> | namespaces/authorizationrules | Sin |Sin |
> | namespaces/disasterrecoveryconfigs | Sin |Sin |
> | namespaces/eventhubs | Sin |Sin |
> | namespaces/eventhubs/authorizationrules | Sin |Sin |
> | namespaces/eventhubs/consumergroups | Sin |Sin |
> | namespaces/networkrulesets | Sin |Sin |

## <a name="microsoftfeatures"></a>Microsoft.Features

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | features | Sin |Sin |
> | providers | Sin |Sin |

## <a name="microsoftgallery"></a>Microsoft.Gallery

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | enroll | Sin |Sin |
> | galleryitems | Sin |Sin |
> | generateartifactaccessuri | Sin |Sin |
> | myareas | Sin |Sin |
> | myareas/areas | Sin |Sin |
> | myareas/areas/areas | Sin |Sin |
> | myareas/areas/areas/galleryitems | Sin |Sin |
> | myareas/areas/galleryitems | Sin |Sin |
> | myareas/galleryitems | Sin |Sin |
> | registro | Sin |Sin |
> | resources | Sin |Sin |
> | retrieveresourcesbyid | Sin |Sin |

## <a name="microsoftgenomics"></a>Microsoft.Genomics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | accounts | Sí |Sí |

## <a name="microsoftguestconfiguration"></a>Microsoft.GuestConfiguration

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | guestConfigurationAssignments | Sin |Sin |
> | software | Sin |Sin |
> | softwareUpdateProfile | Sin |Sin |
> | softwareUpdates | Sin |Sin |

## <a name="microsofthanaonazure"></a>Microsoft.HanaOnAzure

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | hanaInstances | Sí |Sí |
> | sapMonitors | Sí |Sí |

## <a name="microsofthardwaresecuritymodules"></a>Microsoft.HardwareSecurityModules

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | dedicatedHSMs | Sí |Sí |

## <a name="microsofthdinsight"></a>Microsoft.HDInsight

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | clusters | Sí |Sí |
> | clusters/applications | Sin |Sin |

## <a name="microsofthealthcareapis"></a>Microsoft.HealthcareApis

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | services | Sí |Sí |

## <a name="microsofthybridcompute"></a>Microsoft.HybridCompute

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | machines | Sí |Sí |

## <a name="microsofthybriddata"></a>Microsoft.HybridData

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | dataManagers | Sí |Sí |

## <a name="microsofthydra"></a>Microsoft.Hydra

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | components | Sí |Sí |
> | networkScopes | Sí |Sí |

## <a name="microsoftimportexport"></a>Microsoft.ImportExport

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | jobs | Sí |Sí |

## <a name="microsoftintune"></a>Microsoft.Intune

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Sin |Sin |
> | diagnosticSettingsCategories | Sin |Sin |

## <a name="microsoftiotcentral"></a>Microsoft.IoTCentral

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | appTemplates | Sin |Sin |
> | IoTApps | Sí |Sí |

## <a name="microsoftiotspaces"></a>Microsoft.IoTSpaces

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | Grafo | Sí |Sí |

## <a name="microsoftkeyvault"></a>Microsoft.KeyVault

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | deletedVaults | Sin |Sin |
> | hsmPools | Sí |Sí |
> | vaults | Sí |Sí |
> | vaults/accessPolicies | Sin |Sin |
> | vaults/eventGridFilters | Sin |Sin |
> | vaults/secrets | Sin |Sin |

## <a name="microsoftkusto"></a>Microsoft.Kusto

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | clusters | Sí |Sí |
> | clusters/attacheddatabaseconfigurations | Sin |Sin |
> | clusters/databases | Sin |Sin |
> | clusters/databases/dataconnections | Sin |Sin |
> | clusters/databases/eventhubconnections | Sin |Sin |

## <a name="microsoftlabservices"></a>Microsoft.LabServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | labaccounts | Sí |Sí |
> | users | Sin |Sin |

## <a name="microsoftlogic"></a>Microsoft.Logic

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | hostingEnvironments | Sí |Sí |
> | integrationAccounts | Sí |Sí |
> | integrationServiceEnvironments | Sí |Sí |
> | isolatedEnvironments | Sí |Sí |
> | workflows | Sí |Sí |

## <a name="microsoftmachinelearning"></a>Microsoft.MachineLearning

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | commitmentPlans | Sí |Sí |
> | webServices | Sí |Sí |
> | Áreas de trabajo | Sí |Sí |

## <a name="microsoftmachinelearningservices"></a>Microsoft.MachineLearningServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | workspaces | Sí |Sí |
> | workspaces/computes | Sin |Sin |

## <a name="microsoftmanagedidentity"></a>Microsoft.ManagedIdentity

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | Identities | Sin |Sin |
> | userAssignedIdentities | Sí |Sí |

## <a name="microsoftmanagedlab"></a>Microsoft.ManagedLab

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | labaccounts | Sí |Sí |

## <a name="microsoftmanagedservices"></a>Microsoft.ManagedServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | marketplaceRegistrationDefinitions | Sin |Sin |
> | registrationAssignments | Sin |Sin |
> | registrationDefinitions | Sin |Sin |

## <a name="microsoftmanagement"></a>Microsoft.Management

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | getEntities | Sin |Sin |
> | managementGroups | Sin |Sin |
> | resources | Sin |Sin |
> | startTenantBackfill | Sin |Sin |
> | tenantBackfillStatus | Sin |Sin |

## <a name="microsoftmaps"></a>Microsoft.Maps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | accounts | Sí |Sí |
> | accounts/eventGridFilters | Sin |Sin |

## <a name="microsoftmarketplace"></a>Microsoft.Marketplace

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | offers | Sin |Sin |
> | offerTypes | Sin |Sin |
> | offerTypes/publishers | Sin |Sin |
> | offerTypes/publishers/offers | Sin |Sin |
> | offerTypes/publishers/offers/plans | Sin |Sin |
> | offerTypes/publishers/offers/plans/agreements | Sin |Sin |
> | offerTypes/publishers/offers/plans/configs | Sin |Sin |
> | offerTypes/publishers/offers/plans/configs/importImage | Sin |Sin |
> | privategalleryitems | Sin |Sin |
> | products | Sin |Sin |
> | publishers | Sin |Sin |
> | publishers/offers | Sin |Sin |
> | publishers/offers/amendments | Sin |Sin |

## <a name="microsoftmarketplaceapps"></a>Microsoft.MarketplaceApps

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | classicDevServices | Sí |Sí |
> | updateCommunicationPreference | Sin |Sin |

## <a name="microsoftmarketplaceordering"></a>Microsoft.MarketplaceOrdering

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | agreements | Sin |Sin |
> | offertypes | Sin |Sin |

## <a name="microsoftmedia"></a>Microsoft.Media

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | mediaservices | Sí |Sí |
> | mediaservices/accountFilters | Sin |Sin |
> | mediaservices/assets | Sin |Sin |
> | mediaservices/assets/assetFilters | Sin |Sin |
> | mediaservices/contentKeyPolicies | Sin |Sin |
> | mediaservices/eventGridFilters | Sin |Sin |
> | mediaservices/liveEventOperations | Sin |Sin |
> | mediaservices/liveEvents | Sí |Sí |
> | mediaservices/liveEvents/liveOutputs | Sin |Sin |
> | mediaservices/liveOutputOperations | Sin |Sin |
> | mediaservices/streamingEndpointOperations | Sin |Sin |
> | mediaservices/streamingEndpoints | Sí |Sí |
> | mediaservices/streamingLocators | Sin |Sin |
> | mediaservices/streamingPolicies | Sin |Sin |
> | mediaservices/transforms | Sin |Sin |
> | mediaservices/transforms/jobs | Sin |Sin |

## <a name="microsoftmicroservices4spring"></a>Microsoft.Microservices4Spring

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | appClusters | Sí |Sí |

## <a name="microsoftmigrate"></a>Microsoft.Migrate

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | assessmentProjects | Sí |Sí |
> | migrateprojects | Sí |Sí |
> | projects | Sí |Sí |

## <a name="microsoftmixedreality"></a>Microsoft.MixedReality

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | remoteRenderingAccounts | Sí |Sí |
> | spatialAnchorsAccounts | Sí |Sí |

## <a name="microsoftnetapp"></a>Microsoft.NetApp

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | netAppAccounts | Sí |Sí |
> | netAppAccounts/capacityPools | Sí |Sí |
> | netAppAccounts/capacityPools/volumes | Sí |Sí |
> | netAppAccounts/capacityPools/volumes/mountTargets | Sí |Sí |
> | netAppAccounts/capacityPools/volumes/snapshots | Sí |Sí |

## <a name="microsoftnetwork"></a>Microsoft.Network

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | applicationGateways | Sí |Sí |
> | applicationGatewayWebApplicationFirewallPolicies | Sí |Sí |
> | applicationSecurityGroups | Sí |Sí |
> | azureFirewallFqdnTags | Sin |Sin |
> | azureFirewalls | Sí |Sí |
> | bastionHosts | Sí |Sí |
> | bgpServiceCommunities | Sin |Sin |
> | connections | Sí |Sí |
> | ddosCustomPolicies | Sí |Sí |
> | ddosProtectionPlans | Sí |Sí |
> | dnsOperationStatuses | Sin |Sin |
> | dnszones | Sí |Sí |
> | dnszones/A | Sin |Sin |
> | dnszones/AAAA | Sin |Sin |
> | dnszones/all | Sin |Sin |
> | dnszones/CAA | Sin |Sin |
> | dnszones/CNAME | Sin |Sin |
> | dnszones/MX | Sin |Sin |
> | dnszones/NS | Sin |Sin |
> | dnszones/PTR | Sin |Sin |
> | dnszones/recordsets | Sin |Sin |
> | dnszones/SOA | Sin |Sin |
> | dnszones/SRV | Sin |Sin |
> | dnszones/TXT | Sin |Sin |
> | expressRouteCircuits | Sí |Sí |
> | expressRouteCrossConnections | Sí |Sí |
> | expressRouteGateways | Sí |Sí |
> | expressRoutePorts | Sí |Sí |
> | expressRouteServiceProviders | Sin |Sin |
> | firewallPolicies | Sí |Sí |
> | frontdoors | Sí, pero con límites (consulte la [nota siguiente](#frontdoor)) |Sí |
> | frontdoorWebApplicationFirewallManagedRuleSets | Sí, pero con límites (consulte la [nota siguiente](#frontdoor)) |Sin |
> | frontdoorWebApplicationFirewallPolicies | Sí, pero con límites (consulte la [nota siguiente](#frontdoor)) |Sí |
> | getDnsResourceReference | Sin |Sin |
> | internalNotify | Sin |Sin |
> | loadBalancers | Sí |Sin |
> | localNetworkGateways | Sí |Sí |
> | natGateways | Sí |Sí |
> | networkIntentPolicies | Sí |Sí |
> | networkInterfaces | Sí |Sí |
> | networkProfiles | Sí |Sí |
> | networkSecurityGroups | Sí |Sí |
> | networkWatchers | Sí |Sin |
> | networkWatchers/connectionMonitors | Sí |Sin |
> | networkWatchers/lenses | Sí |Sin |
> | networkWatchers/pingMeshes | Sí |Sin |
> | p2sVpnGateways | Sí |Sí |
> | privateDnsOperationStatuses | Sin |Sin |
> | privateDnsZones | Sí |Sí |
> | privateDnsZones/A | Sin |Sin |
> | privateDnsZones/AAAA | Sin |Sin |
> | privateDnsZones/all | Sin |Sin |
> | privateDnsZones/CNAME | Sin |Sin |
> | privateDnsZones/MX | Sin |Sin |
> | privateDnsZones/PTR | Sin |Sin |
> | privateDnsZones/SOA | Sin |Sin |
> | privateDnsZones/SRV | Sin |Sin |
> | privateDnsZones/TXT | Sin |Sin |
> | privateDnsZones/virtualNetworkLinks | Sí |Sí |
> | privateEndpoints | Sí |Sí |
> | privateLinkServices | Sí |Sí |
> | publicIPAddresses | Sí |Sí |
> | publicIPPrefixes | Sí |Sí |
> | routeFilters | Sí |Sí |
> | routeTables | Sí |Sí |
> | secureGateways | Sí |Sí |
> | serviceEndpointPolicies | Sí |Sí |
> | trafficManagerGeographicHierarchies | Sin |Sin |
> | trafficmanagerprofiles | Sí |Sí |
> | trafficmanagerprofiles/heatMaps | Sin |Sin |
> | trafficManagerUserMetricsKeys | Sin |Sin |
> | virtualHubs | Sí |Sí |
> | virtualNetworkGateways | Sí |Sí |
> | virtualNetworks | Sí |Sí |
> | virtualNetworkTaps | Sí |Sí |
> | virtualWans | Sí |Sí |
> | vpnGateways | Sí |Sin |
> | vpnSites | Sí |Sí |
> | webApplicationFirewallPolicies | Sí |Sí |

<a id="frontdoor" />

> [!NOTE]
> Para Azure Front Door Service, puede aplicar etiquetas al crear el recurso, pero no se admite actualmente la actualización o la adición de etiquetas.

## <a name="microsoftnotificationhubs"></a>Microsoft.NotificationHubs

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | namespaces | Sí |Sin |
> | namespaces/notificationHubs | Sí |Sin |

## <a name="microsoftoffazure"></a>Microsoft.OffAzure

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | HyperVSites | Sí |Sí |
> | ImportSites | Sí |Sí |
> | ServerSites | Sí |Sí |
> | VMwareSites | Sí |Sí |

## <a name="microsoftoperationalinsights"></a>Microsoft.OperationalInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | devices | Sin |Sin |
> | linkTargets | Sin |Sin |
> | storageInsightConfigs | Sin |Sin |
> | workspaces | Sí |Sí |
> | workspaces/dataSources | Sin |Sin |
> | workspaces/linkedServices | Sin |Sin |
> | workspaces/query | Sin |Sin |

## <a name="microsoftoperationsmanagement"></a>Microsoft.OperationsManagement

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | managementassociations | Sin |Sin |
> | managementconfigurations | Sí |Sí |
> | solutions | Sí |SÍ |
> | views | SÍ |Sí |

## <a name="microsoftpeering"></a>Microsoft.Peering

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | legacyPeerings | Sin |Sin |
> | peerAsns | Sin |Sin |
> | peerings | Sí |Sí |

## <a name="microsoftpolicyinsights"></a>Microsoft.PolicyInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | policyEvents | Sin |Sin |
> | policyStates | Sin |Sin |
> | policyTrackedResources | Sin |Sin |
> | remediations | Sin |Sin |

## <a name="microsoftportal"></a>Microsoft.Portal

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | consoles | Sin |Sin |
> | dashboards | Sí |Sí |
> | userSettings | Sin |Sin |

## <a name="microsoftpowerbi"></a>Microsoft.PowerBI

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | workspaceCollections | Sí |Sí |

## <a name="microsoftpowerbidedicated"></a>Microsoft.PowerBIDedicated

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | capacities | Sí |Sí |

## <a name="microsoftrecoveryservices"></a>Microsoft.RecoveryServices

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | backupProtectedItems | Sin |Sin |
> | vaults | Sí |Sí |

## <a name="microsoftrelay"></a>Microsoft.Relay

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | namespaces | Sí |Sí |
> | namespaces/authorizationrules | Sin |Sin |
> | namespaces/hybridconnections | Sin |Sin |
> | namespaces/hybridconnections/authorizationrules | Sin |Sin |
> | namespaces/wcfrelays | Sin |Sin |
> | namespaces/wcfrelays/authorizationrules | Sin |Sin |

## <a name="microsoftremoteapp"></a>Microsoft.RemoteApp

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | accounts | Sin |Sin |
> | collections | Sí |Sí |
> | collections/applications | Sin |Sin |
> | collections/securityprincipals | Sin |Sin |
> | templateImages | Sin |Sin |

## <a name="microsoftresourcegraph"></a>Microsoft.ResourceGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | Consultas | Sí |Sí |
> | resourceChangeDetails | Sin |Sin |
> | resourceChanges | Sin |Sin |
> | resources | Sin |Sin |
> | resourcesHistory | Sin |Sin |
> | subscriptionsStatus | Sin |Sin |

## <a name="microsoftresourcehealth"></a>Microsoft.ResourceHealth

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | availabilityStatuses | Sin |Sin |
> | childAvailabilityStatuses | Sin |Sin |
> | childResources | Sin |Sin |
> | events | Sin |Sin |
> | impactedResources | Sin |Sin |
> | metadata | Sin |Sin |
> | Notificaciones | Sin |Sin |

## <a name="microsoftresources"></a>Microsoft.Resources

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | deployments | Sin |Sin |
> | deployments/operations | Sin |Sin |
> | links | Sin |Sin |
> | notifyResourceJobs | Sin |Sin |
> | providers | Sin |Sin |
> | resourceGroups | Sin |Sin |
> | resources | Sin |Sin |
> | subscriptions | Sin |Sin |
> | subscriptions/providers | Sin |Sin |
> | subscriptions/resourceGroups | Sin |Sin |
> | subscriptions/resourcegroups/resources | Sin |Sin |
> | subscriptions/resources | Sin |Sin |
> | subscriptions/tagnames | Sin |Sin |
> | subscriptions/tagNames/tagValues | Sin |Sin |
> | etiquetas | Sin |Sin |
> | tenants | Sin |Sin |

## <a name="microsoftsaas"></a>Microsoft.SaaS

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | applications | Sí |Sí |
> | saasresources | Sin |Sin |

## <a name="microsoftscheduler"></a>Microsoft.Scheduler

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | flows | Sí |Sí |
> | jobcollections | Sí |Sí |

## <a name="microsoftsearch"></a>Microsoft.Search

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | resourceHealthMetadata | Sin |Sin |
> | searchServices | Sí |Sí |

## <a name="microsoftsecurity"></a>Microsoft.Security

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | adaptiveNetworkHardenings | Sin |Sin |
> | advancedThreatProtectionSettings | Sin |Sin |
> | alerts | Sin |Sin |
> | allowedConnections | Sin |Sin |
> | applicationWhitelistings | Sin |Sin |
> | assessmentMetadata | Sin |Sin |
> | assessments | Sin |Sin |
> | AutoProvisioningSettings | Sin |Sin |
> | Compliances | Sin |Sin |
> | dataCollectionAgents | Sin |Sin |
> | deviceSecurityGroups | Sin |Sin |
> | discoveredSecuritySolutions | Sin |Sin |
> | externalSecuritySolutions | Sin |Sin |
> | InformationProtectionPolicies | Sin |Sin |
> | iotSecuritySolutions | Sí |Sí |
> | iotSecuritySolutions/analyticsModels | Sin |Sin |
> | iotSecuritySolutions/analyticsModels/aggregatedAlerts | Sin |Sin |
> | iotSecuritySolutions/analyticsModels/aggregatedRecommendations | Sin |Sin |
> | jitNetworkAccessPolicies | Sin |Sin |
> | playbookConfigurations | Sí |Sí |
> | directivas | Sin |Sin |
> | pricings | Sin |Sin |
> | regulatoryComplianceStandards | Sin |Sin |
> | regulatoryComplianceStandards/regulatoryComplianceControls | Sin |Sin |
> | regulatoryComplianceStandards/regulatoryComplianceControls/regulatoryComplianceAssessments | Sin |Sin |
> | securityContacts | Sin |Sin |
> | securitySolutions | Sin |Sin |
> | securitySolutionsReferenceData | Sin |Sin |
> | securityStatuses | Sin |Sin |
> | securityStatusesSummaries | Sin |Sin |
> | serverVulnerabilityAssessments | Sin |Sin |
> | configuración | Sin |Sin |
> | tareas | Sin |Sin |
> | topologies | Sin |Sin |
> | workspaceSettings | Sin |Sin |

## <a name="microsoftsecuritygraph"></a>Microsoft.SecurityGraph

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Sin |Sin |
> | diagnosticSettingsCategories | Sin |Sin |

## <a name="microsoftsecurityinsights"></a>Microsoft.SecurityInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | aggregations | Sin |Sin |
> | alertRules | Sin |Sin |
> | bookmarks | Sin |Sin |
> | cases | Sin |Sin |
> | dataConnectors | Sin |Sin |
> | entities | Sin |Sin |
> | entityQueries | Sin |Sin |
> | officeConsents | Sin |Sin |
> | configuración | Sin |Sin |

## <a name="microsoftservicebus"></a>Microsoft.ServiceBus

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | namespaces | Sí |Sin |
> | namespaces/authorizationrules | Sin |Sin |
> | namespaces/disasterrecoveryconfigs | Sin |Sin |
> | namespaces/eventgridfilters | Sin |Sin |
> | namespaces/networkrulesets | Sin |Sin |
> | namespaces/queues | Sin |Sin |
> | namespaces/queues/authorizationrules | Sin |Sin |
> | namespaces/topics | Sin |Sin |
> | namespaces/topics/authorizationrules | Sin |Sin |
> | namespaces/topics/subscriptions | Sin |Sin |
> | namespaces/topics/subscriptions/rules | Sin |Sin |
> | premiumMessagingRegions | Sin |Sin |

## <a name="microsoftservicefabric"></a>Microsoft.ServiceFabric

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | applications | Sí |Sí |
> | clusters | Sí |Sí |
> | clusters/applications | Sin |Sin |
> | containerGroups | Sí |Sí |
> | containerGroupSets | Sí |Sí |
> | edgeclusters | Sí |Sí |
> | edgeclusters/applications | Sin |Sin |
> | networks | Sí |Sí |
> | secretstores | Sí |Sí |
> | secretstores/certificates | Sin |Sin |
> | secretstores/secrets | Sin |Sin |
> | volumes | Sí |Sí |

## <a name="microsoftservicefabricmesh"></a>Microsoft.ServiceFabricMesh

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | applications | Sí |Sí |
> | containerGroups | Sí |Sí |
> | gateways | Sí |Sí |
> | networks | Sí |Sí |
> | secrets | Sí |Sí |
> | volumes | Sí |Sí |

## <a name="microsoftservices"></a>Microsoft.Services

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | providerRegistrations | Sin |Sin |
> | providerRegistrations/resourceTypeRegistrations | Sin |Sin |
> | rollouts | Sí |Sí |

## <a name="microsoftsignalrservice"></a>Microsoft.SignalRService

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | SignalR | Sí |Sí |
> | SignalR/eventGridFilters | Sin |Sin |

## <a name="microsoftsiterecovery"></a>Microsoft.SiteRecovery

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | SiteRecoveryVault | Sí |Sí |

## <a name="microsoftsoftwareplan"></a>Microsoft.SoftwarePlan

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | hybridUseBenefits | Sin |Sin |

## <a name="microsoftsolutions"></a>Microsoft.Solutions

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | applicationDefinitions | Sí |Sí |
> | applications | Sí |Sí |
> | jitRequests | Sí |Sí |

## <a name="microsoftsql"></a>Microsoft.SQL

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | managedInstances | Sí | Sí |
> | managedInstances/databases | Yes (see [note below](#sqlnote)) | Sí |
> | managedInstances/databases/backupShortTermRetentionPolicies | Sin | Sin |
> | managedInstances/databases/schemas/tables/columns/sensitivityLabels | Sin | Sin |
> | managedInstances/databases/vulnerabilityAssessments | Sin | Sin |
> | managedInstances/databases/vulnerabilityAssessments/rules/baselines | Sin | Sin |
> | managedInstances/encryptionProtector | Sin | Sin |
> | managedInstances/keys | Sin | Sin |
> | managedInstances/restorableDroppedDatabases/backupShortTermRetentionPolicies | Sin | Sin |
> | managedInstances/vulnerabilityAssessments | Sin | Sin |
> | servers | Sí | Sí |
> | servers/administrators | Sin | Sin |
> | servers/communicationLinks | Sin | Sin |
> | servers/databases | Yes (see [note below](#sqlnote)) | Sí |
> | servers/encryptionProtector | Sin | Sin |
> | servers/firewallRules | Sin | Sin |
> | servers/keys | Sin | Sin |
> | servers/restorableDroppedDatabases | Sin | Sin |
> | servers/serviceobjectives | Sin | Sin |
> | servers/tdeCertificates | Sin | Sin |

<a id="sqlnote" />

> [!NOTE]
> La base de datos maestra no admite etiquetas, pero otras bases de datos, como las de Azure SQL Data Warehouse, sí lo hacen. Las bases de datos de Azure SQL Data Warehouse deben estar en estado activo (no en pausa).

## <a name="microsoftsqlvirtualmachine"></a>Microsoft.SqlVirtualMachine

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | SqlVirtualMachineGroups | Sí |Sí |
> | SqlVirtualMachineGroups/AvailabilityGroupListeners | Sin |Sin |
> | SqlVirtualMachines | Sí |Sí |

## <a name="microsoftstorage"></a>Microsoft.Storage

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | storageAccounts | Sí |Sí |
> | storageAccounts/blobServices | Sin |Sin |
> | storageAccounts/fileServices | Sin |Sin |
> | storageAccounts/queueServices | Sin |Sin |
> | storageAccounts/services | Sin |Sin |
> | storageAccounts/services/metricDefinitions | Sin |Sin |
> | storageAccounts/tableServices | Sin |Sin |
> | usages | Sin |Sin |

## <a name="microsoftstoragecache"></a>Microsoft.StorageCache

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | caches | Sí |Sí |
> | caches/storageTargets | Sin |Sin |
> | usageModels | Sin |Sin |

## <a name="microsoftstoragereplication"></a>Microsoft.StorageReplication

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | replicationGroups | Sin |Sin |

## <a name="microsoftstoragesync"></a>Microsoft.StorageSync

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Sí |Sí |
> | storageSyncServices/registeredServers | Sin |Sin |
> | storageSyncServices/syncGroups | Sin |Sin |
> | storageSyncServices/syncGroups/cloudEndpoints | Sin |Sin |
> | storageSyncServices/syncGroups/serverEndpoints | Sin |Sin |
> | storageSyncServices/workflows | Sin |Sin |

## <a name="microsoftstoragesyncdev"></a>Microsoft.StorageSyncDev

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Sí |Sí |
> | storageSyncServices/registeredServers | Sin |Sin |
> | storageSyncServices/syncGroups | Sin |Sin |
> | storageSyncServices/syncGroups/cloudEndpoints | Sin |Sin |
> | storageSyncServices/syncGroups/serverEndpoints | Sin |Sin |
> | storageSyncServices/workflows | Sin |Sin |

## <a name="microsoftstoragesyncint"></a>Microsoft.StorageSyncInt

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | storageSyncServices | Sí |Sí |
> | storageSyncServices/registeredServers | Sin |Sin |
> | storageSyncServices/syncGroups | Sin |Sin |
> | storageSyncServices/syncGroups/cloudEndpoints | Sin |Sin |
> | storageSyncServices/syncGroups/serverEndpoints | Sin |Sin |
> | storageSyncServices/workflows | Sin |Sin |

## <a name="microsoftstorsimple"></a>Microsoft.StorSimple

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | managers | Sí |Sí |

## <a name="microsoftstreamanalytics"></a>Microsoft.StreamAnalytics

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | streamingjobs | Sí (vea la nota siguiente) |Sí |

> [!NOTE]
> No se pueden agregar etiquetas cuando hay instancias de streamingjobs en ejecución. Detenga el recurso si desea agregar una etiqueta.

## <a name="microsoftsubscription"></a>Microsoft.Subscription

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | cancel | Sin |Sin |
> | CreateSubscription | Sin |Sin |
> | rename | Sin |Sin |
> | SubscriptionDefinitions | Sin |Sin |
> | SubscriptionOperations | Sin |Sin |

## <a name="microsofttimeseriesinsights"></a>Microsoft.TimeSeriesInsights

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | environments | Sí |Sin |
> | environments/accessPolicies | Sin |Sin |
> | environments/eventsources | Sí |Sin |
> | environments/referenceDataSets | Sí |Sin |

## <a name="microsoftvmwarecloudsimple"></a>Microsoft.VMwareCloudSimple

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | dedicatedCloudNodes | Sí |Sí |
> | dedicatedCloudServices | Sí |Sí |
> | virtualMachines | Sí |Sí |

## <a name="microsoftweb"></a>Microsoft.Web

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | apiManagementAccounts | Sin |Sin |
> | apiManagementAccounts/apiAcls | Sin |Sin |
> | apiManagementAccounts/apis | Sin |Sin |
> | apiManagementAccounts/apis/apiAcls | Sin |Sin |
> | apiManagementAccounts/apis/connectionAcls | Sin |Sin |
> | apiManagementAccounts/apis/connections | Sin |Sin |
> | apiManagementAccounts/apis/connections/connectionAcls | Sin |Sin |
> | apiManagementAccounts/apis/localizedDefinitions | Sin |Sin |
> | apiManagementAccounts/connectionAcls | Sin |Sin |
> | apiManagementAccounts/connections | Sin |Sin |
> | billingMeters | Sin |Sin |
> | certificates | Sí |Sí |
> | connectionGateways | Sí |Sí |
> | connections | Sí |Sí |
> | customApis | Sí |Sí |
> | deletedSites | Sin |Sin |
> | functions | Sin |Sin |
> | hostingEnvironments | Sí |Sí |
> | hostingEnvironments/multiRolePools | Sin |Sin |
> | hostingEnvironments/workerPools | Sin |Sin |
> | publishingUsers | Sin |Sin |
> | de películas | Sin |Sin |
> | resourceHealthMetadata | Sin |Sin |
> | runtimes | Sin |Sin |
> | serverFarms | Sí |Sí |
> | serverFarms/eventGridFilters | Sin |Sin |
> | sites | Sí |Sí |
> | sites/config  | Sin | Sin |
> | sites/eventGridFilters | Sin |Sin |
> | sites/hostNameBindings | Sin |Sin |
> | sites/networkConfig | Sin |Sin |
> | sites/premieraddons | Sí |Sí |
> | sites/slots | Sí |Sí |
> | sites/slots/eventGridFilters | Sin |Sin |
> | sites/slots/hostNameBindings | Sin |Sin |
> | sites/slots/networkConfig | Sin |Sin |
> | sourceControls | Sin |Sin |
> | validate | Sin |Sin |
> | verifyHostingEnvironmentVnet | Sin |Sin |

## <a name="microsoftwindowsdefenderatp"></a>Microsoft.WindowsDefenderATP

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | diagnosticSettings | Sin |Sin |
> | diagnosticSettingsCategories | Sin |Sin |

## <a name="microsoftwindowsiot"></a>Microsoft.WindowsIoT

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | DeviceServices | Sí |Sí |

## <a name="microsoftworkloadmonitor"></a>Microsoft.WorkloadMonitor

> [!div class="mx-tableFixed"]
> | Tipo de recurso | Compatible con las etiquetas | Etiqueta en el informe de costos |
> | ------------- | ----------- | ----------- |
> | components | Sin |Sin |
> | componentsSummary | Sin |Sin |
> | monitorInstances | Sin |Sin |
> | monitorInstancesSummary | Sin |Sin |
> | monitors | Sin |Sin |
> | notificationSettings | Sin |Sin |

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo aplicar etiquetas a recursos, consulte [Uso de etiquetas para organizar los recursos de Azure](resource-group-using-tags.md).
