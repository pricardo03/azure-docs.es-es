---
title: Recursos compatibles para las alertas de métricas de Azure Monitor
description: Referencia sobre métricas y registros de soporte técnico para las alertas de métricas en Azure Monitor
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 06/29/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: be9f8079480438b194f0d5779ac0da50328b5dd3
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/18/2018
ms.locfileid: "53587460"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Recursos compatibles para las alertas de métricas de Azure Monitor

Azure Monitor ahora admite un [nuevo tipo de alerta de métrica](../../azure-monitor/platform/alerts-overview.md) que tiene ventajas considerables sobre las anteriores [alertas de métrica clásicas](../../azure-monitor/platform/alerts-classic.overview.md). Las métricas están disponibles para una [amplia lista de servicios de Azure](../../azure-monitor/platform/metrics-supported.md). Las nuevas alertas admiten un subconjunto (creciente) de los tipos de recurso. En este artículo se muestra ese subconjunto.


También puede utilizar las nuevas alertas de métricas en los registros populares de Log Analytics extraídos como métricas. Para obtener más información, consulte [Alertas de métricas para registros](../../azure-monitor/platform/alerts-metric-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST support
Actualmente, solo puede crear alertas de métricas nuevas en Azure Portal, la [API de REST](https://docs.microsoft.com/rest/api/monitor/metricalerts/) o [plantillas de Resource Manager](../../azure-monitor/platform/alerts-metric-create-templates.md). La compatibilidad con la configuración de nuevas alertas mediante las versiones 2.0 de PowerShell y la CLI de Azure estará disponible próximamente.

## <a name="metrics-and-dimensions-supported"></a>Métricas y dimensiones compatibles
Las nuevas alertas de métricas admiten alertas de métricas que utilizan dimensiones. Puede usar dimensiones para filtrar las métricas al nivel adecuado. Todas las métricas compatibles, junto con las dimensiones aplicables, se pueden explorar y visualizar en el [Explorador de métricas de Azure Monitor](../../azure-monitor/platform/metrics-charts.md).

Esta es la lista completa de los orígenes de métricas de Azure Monitor que se admiten por las nuevas alertas:

|Tipo de recurso  |Dimensiones compatibles  | Métricas disponibles|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | SÍ        | [API Management](../../azure-monitor/platform/metrics-supported.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     SÍ   | [Cuentas de Automation](../../azure-monitor/platform/metrics-supported.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/D| [Cuentas de Batch](../../azure-monitor/platform/metrics-supported.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/D     |[Azure Cache for Redis](../../azure-monitor/platform/metrics-supported.md#microsoftcacheredis)|
|Microsoft.CognitiveServices/accounts     |    N/D     | [Cognitive Services](../../azure-monitor/platform/metrics-supported.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines     |    N/D     | [Máquinas virtuales](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/D      |[Conjuntos de escalado de máquinas virtuales](../../azure-monitor/platform/metrics-supported.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | SÍ| [Grupos de contenedores](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | SÍ | [Clústeres administrados](../../azure-monitor/platform/metrics-supported.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataFactory/datafactories| SÍ| [Factorías de datos V1](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   SÍ     |[Factorías de datos V2](../../azure-monitor/platform/metrics-supported.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/D      |[DB para MySQL](../../azure-monitor/platform/metrics-supported.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/D     | [DB para PostgreSQL](../../azure-monitor/platform/metrics-supported.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  SÍ      |[Event Hubs](../../azure-monitor/platform/metrics-supported.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Sin  | [Almacenes](../../azure-monitor/platform/metrics-supported.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/D    |[Logic Apps](../../azure-monitor/platform/metrics-supported.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/D     | [Puertas de enlace de aplicaciones](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/expressRouteCircuits | N/D |  [Circuitos ExpressRoute](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/dnsZones | N/D| [Zonas DNS](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (solo para SKU estándar)| SÍ| [Equilibradores de carga](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/D       |[Direcciones IP públicas](../../azure-monitor/platform/metrics-supported.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/D | [Capacidades](../../azure-monitor/platform/metrics-supported.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Network/trafficManagerProfiles | SÍ | [Perfiles de Traffic Manager](../../azure-monitor/platform/metrics-supported.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.Search/searchServices     |   N/D      |[Servicios de búsqueda](../../azure-monitor/platform/metrics-supported.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  SÍ       |[Service Bus](../../azure-monitor/platform/metrics-supported.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    SÍ     | [Cuentas de almacenamiento](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     SÍ    | [Servicios de blob](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsblobservices), [servicios de archivo](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsfileservices), [servicios de cola](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountsqueueservices) y [servicios de tabla](../../azure-monitor/platform/metrics-supported.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/D       | [Stream Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftstreamanalyticsstreamingjobs)|
| Microsoft.Web/serverfarms | SÍ | [Planes de App Service](../../azure-monitor/platform/metrics-supported.md#microsoftwebserverfarms)  |
| Microsoft.Web/sites | SÍ | [App Services](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-excluding-functions) y [Functions](../../azure-monitor/platform/metrics-supported.md#microsoftwebsites-functions)|
| Microsoft.Web/sites/slots | SÍ | [Ranuras de App Service](../../azure-monitor/platform/metrics-supported.md#microsoftwebsitesslots)|
|Microsoft.OperationalInsights/workspaces| SÍ|[Áreas de trabajo de Log Analytics](../../azure-monitor/platform/metrics-supported.md#microsoftoperationalinsightsworkspaces)|



## <a name="payload-schema"></a>Esquema de carga

La operación POST contiene el esquema y la carga útil de JSON siguientes para todas las nuevas métricas cuando se usa un [grupo de acciones](../../azure-monitor/platform/action-groups.md) configurado correctamente:

```json
{"schemaId":"AzureMonitorMetricAlert","data":
    {
    "version": "2.0",
    "status": "Activated",
    "context": {
    "timestamp": "2018-02-28T10:44:10.1714014Z",
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/microsoft.insights/metricAlerts/StorageCheck",
    "name": "StorageCheck",
    "description": "",
    "conditionType": "SingleResourceMultipleMetricCriteria",
    "condition": {
      "windowSize": "PT5M",
      "allOf": [
        {
          "metricName": "Transactions",
          "dimensions": [
            {
              "name": "AccountResourceId",
              "value": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
            },
            {
              "name": "GeoType",
              "value": "Primary"
            }
          ],
          "operator": "GreaterThan",
          "threshold": "0",
          "timeAggregation": "PT5M",
          "metricValue": 1.0
        },
      ]
    },
    "subscriptionId": "00000000-0000-0000-0000-000000000000",
    "resourceGroupName": "Contoso",
    "resourceName": "diag500",
    "resourceType": "Microsoft.Storage/storageAccounts",
    "resourceId": "/subscriptions/1e3ff1c0-771a-4119-a03b-be82a51e232d/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500",
    "portalLink": "https://portal.azure.com/#resource//subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/Contoso/providers/Microsoft.Storage/storageAccounts/diag500"
  },
        "properties": {
                "key1": "value1",
                "key2": "value2"
        }
    }
}
```

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre la nueva [experiencia de alertas](../../azure-monitor/platform/alerts-overview.md).
* Más información sobre las [alertas de registro en Azure](../../azure-monitor/platform/alerts-unified-log.md).
* Más información sobre las [alertas en Azure](../../azure-monitor/platform/alerts-overview.md).
