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
ms.openlocfilehash: 89b412a58291dd542b38cd0cbfa1288795024151
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613722"
---
# <a name="supported-resources-for-metric-alerts-in-azure-monitor"></a>Recursos compatibles para las alertas de métricas de Azure Monitor

Azure Monitor ahora admite un [nuevo tipo de alerta de métrica](monitoring-overview-alerts.md) que tiene ventajas considerables sobre las anteriores [alertas de métrica clásicas](monitoring-overview-alerts-classic.md). Las métricas están disponibles para una [amplia lista de servicios de Azure](monitoring-supported-metrics.md). Las nuevas alertas admiten un subconjunto (creciente) de los tipos de recurso. En este artículo se muestra ese subconjunto.


También puede utilizar las nuevas alertas de métricas en los registros populares de Log Analytics extraídos como métricas. Para obtener más información, consulte [Alertas de métricas para registros](monitoring-metric-alerts-logs.md).

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST support
Actualmente, solo puede crear alertas de métricas nuevas en Azure Portal, la [API de REST](https://docs.microsoft.com/rest/api/monitor/metricalerts/) o [plantillas de Resource Manager](monitoring-create-metric-alerts-with-templates.md). La compatibilidad con la configuración de nuevas alertas mediante las versiones 2.0 de PowerShell y la CLI de Azure estará disponible próximamente.

## <a name="metrics-and-dimensions-supported"></a>Métricas y dimensiones compatibles
Las nuevas alertas de métricas admiten alertas de métricas que utilizan dimensiones. Puede usar dimensiones para filtrar las métricas al nivel adecuado. Todas las métricas compatibles, junto con las dimensiones aplicables, se pueden explorar y visualizar en el [Explorador de métricas de Azure Monitor](monitoring-metric-charts.md).

Esta es la lista completa de los orígenes de métricas de Azure Monitor que se admiten por las nuevas alertas:

|Tipo de recurso  |Dimensiones compatibles  | Métricas disponibles|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | SÍ        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     SÍ   | [Cuentas de Automation](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/D| [Cuentas de Batch](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/D     |[Redis Cache](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.CognitiveServices/accounts     |    N/D     | [Cognitive Services](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.Compute/virtualMachines     |    N/D     | [Máquinas virtuales](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/D      |[Conjuntos de escalado de máquinas virtuales](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | SÍ| [Grupos de contenedores](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.ContainerService/managedClusters | SÍ | [Clústeres administrados](monitoring-supported-metrics.md#microsoftcontainerservicemanagedclusters)|
|Microsoft.DataFactory/datafactories| SÍ| [Factorías de datos V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   SÍ     |[Factorías de datos V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/D      |[DB para MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/D     | [DB para PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  SÍ      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Sin  | [Almacenes](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/D    |[Logic Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/D     | [Puertas de enlace de aplicaciones](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/expressRouteCircuits | N/D |  [Circuitos ExpressRoute](monitoring-supported-metrics.md#microsoftnetworkexpressroutecircuits) |
|Microsoft.Network/dnsZones | N/D| [Zonas DNS](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (solo para SKU estándar)| SÍ| [Equilibradores de carga](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/D       |[Direcciones IP públicas](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/D | [Capacidades](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Network/trafficManagerProfiles | SÍ | [Perfiles de Traffic Manager](monitoring-supported-metrics.md#microsoftnetworktrafficmanagerprofiles) |
|Microsoft.Search/searchServices     |   N/D      |[Servicios de búsqueda](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  SÍ       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    SÍ     | [Cuentas de almacenamiento](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     SÍ    | [Servicios de blob](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [servicios de archivo](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [servicios de cola](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) y [servicios de tabla](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/D       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
| Microsoft.Web/serverfarms | SÍ | [Planes de App Service](monitoring-supported-metrics.md#microsoftwebserverfarms)  |
| Microsoft.Web/sites | SÍ | [App Services](monitoring-supported-metrics.md#microsoftwebsites-excluding-functions) y [Functions](monitoring-supported-metrics.md#microsoftwebsites-functions)|
| Microsoft.Web/sites/slots | SÍ | [Ranuras de App Service](monitoring-supported-metrics.md#microsoftwebsitesslots)|
|Microsoft.OperationalInsights/workspaces| SÍ|[Áreas de trabajo de Log Analytics](monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces)|



## <a name="payload-schema"></a>Esquema de carga

La operación POST contiene el esquema y la carga útil de JSON siguientes para todas las nuevas métricas cuando se usa un [grupo de acciones](monitoring-action-groups.md) configurado correctamente:

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

* Más información sobre la nueva [experiencia de alertas](monitoring-overview-alerts.md).
* Más información sobre las [alertas de registro en Azure](monitor-alerts-unified-log.md).
* Más información sobre las [alertas en Azure](monitoring-overview-alerts.md).
