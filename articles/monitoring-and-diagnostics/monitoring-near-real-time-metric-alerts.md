---
title: Recursos compatibles para las alertas de métrica de Azure Monitor más recientes
description: Referencia sobre métricas y registro de soporte técnico para las nuevas alertas de métricas casi en tiempo real de Azure.
author: snehithm
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 04/27/2018
ms.author: snmuvva
ms.component: alerts
ms.openlocfilehash: d5eaa4dafc9c155d3e6f85bc67c578c8a12da7cf
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35264517"
---
# <a name="supported-metrics-and-creation-methods-for-new-metric-alerts"></a>Métodos de creación y métricas compatibles para las nuevas alertas de métrica
Azure Monitor ahora admite un [nuevo tipo de alerta de métrica](monitoring-overview-unified-alerts.md) que tiene ventajas considerables sobre las anteriores [alertas de métrica clásicas](insights-alerts-portal.md). Las alertas anteriores admiten una [gran lista de métricas](monitoring-supported-metrics.md). Las alertas más recientes admiten un subconjunto (creciente) de esa lista más grande. En este artículo se muestra ese subconjunto. 

## <a name="portal-powershell-cli-rest-support"></a>Portal, PowerShell, CLI, REST support
Actualmente, solo puede crear alertas de métricas nuevas en Azure Portal, la [API REST](https://docs.microsoft.com/en-us/rest/api/monitor/metricalerts/createorupdate) o [plantillas de Resource Manager](monitoring-create-metric-alerts-with-templates.md). La compatibilidad con la configuración de nuevas alertas mediante PowerShell, la interfaz de la línea de comandos de Azure (CLI de Azure 2.0) estará disponible próximamente.

## <a name="metrics-and-dimensions-supported"></a>Métricas y dimensiones compatibles
Las nuevas alertas de métricas admiten alertas de métricas que utilizan dimensiones. Puede usar dimensiones para filtrar las métricas al nivel adecuado. Todas las métricas compatibles junto con las dimensiones aplicables se pueden explorar y visualizar en el [Explorador de métricas de Azure Monitor (versión preliminar)](monitoring-metric-charts.md).

Esta es la lista completa de los orígenes de métricas de Azure Monitor que se admiten por las nuevas alertas:

|Tipo de recurso  |Dimensiones compatibles  | Métricas disponibles|
|---------|---------|----------------|
|Microsoft.ApiManagement/service     | Sí        | [API Management](monitoring-supported-metrics.md#microsoftapimanagementservice)|
|Microsoft.Automation/automationAccounts     |     Sí   | [Cuentas de Automation](monitoring-supported-metrics.md#microsoftautomationautomationaccounts)|
|Microsoft.Batch/batchAccounts | N/D| [Cuentas de Batch](monitoring-supported-metrics.md#microsoftbatchbatchaccounts)|
|Microsoft.Cache/Redis     |    N/D     |[Redis Cache](monitoring-supported-metrics.md#microsoftcacheredis)|
|Microsoft.Compute/virtualMachines     |    N/D     | [Virtual Machines](monitoring-supported-metrics.md#microsoftcomputevirtualmachines)|
|Microsoft.Compute/virtualMachineScaleSets     |   N/D      |[Conjuntos de escalado de máquina virtual](monitoring-supported-metrics.md#microsoftcomputevirtualmachinescalesets)|
|Microsoft.ContainerInstance/containerGroups | Sí| [Grupos de contenedores](monitoring-supported-metrics.md#microsoftcontainerinstancecontainergroups)|
|Microsoft.DataFactory/datafactories| Sí| [Factorías de datos V1](monitoring-supported-metrics.md#microsoftdatafactorydatafactories)|
|Microsoft.DataFactory/factories     |   Sí     |[Factorías de datos V2](monitoring-supported-metrics.md#microsoftdatafactoryfactories)|
|Microsoft.DBforMySQL/servers     |   N/D      |[DB para MySQL](monitoring-supported-metrics.md#microsoftdbformysqlservers)|
|Microsoft.DBforPostgreSQL/servers     |    N/D     | [DB para PostgreSQL](monitoring-supported-metrics.md#microsoftdbforpostgresqlservers)|
|Microsoft.EventHub/namespaces     |  Sí      |[Event Hubs](monitoring-supported-metrics.md#microsofteventhubnamespaces)|
|Microsoft.KeyVault/vaults| Sin  | [Almacenes](monitoring-supported-metrics.md#microsoftkeyvaultvaults)|
|Microsoft.Logic/workflows     |     N/D    |[Logic Apps](monitoring-supported-metrics.md#microsoftlogicworkflows) |
|Microsoft.Network/applicationGateways     |    N/D     | [Puertas de enlace de aplicaciones](monitoring-supported-metrics.md#microsoftnetworkapplicationgateways) |
|Microsoft.Network/dnsZones | N/D| [Zonas DNS](monitoring-supported-metrics.md#microsoftnetworkdnszones) |
|Microsoft.Network/loadBalancers (solo para SKU estándar)| Sí| [Equilibradores de carga](monitoring-supported-metrics.md#microsoftnetworkloadbalancers) |
|Microsoft.Network/publicipaddresses     |  N/D       |[Direcciones IP públicas](monitoring-supported-metrics.md#microsoftnetworkpublicipaddresses)|
|Microsoft.PowerBIDedicated/capacities | N/D | [Capacidades](monitoring-supported-metrics.md#microsoftpowerbidedicatedcapacities)|
|Microsoft.Search/searchServices     |   N/D      |[Servicios de búsqueda](monitoring-supported-metrics.md#microsoftsearchsearchservices)|
|Microsoft.ServiceBus/namespaces     |  Sí       |[Service Bus](monitoring-supported-metrics.md#microsoftservicebusnamespaces)|
|Microsoft.Storage/storageAccounts     |    Sí     | [Cuentas de almacenamiento](monitoring-supported-metrics.md#microsoftstoragestorageaccounts)|
|Microsoft.Storage/storageAccounts/services     |     Sí    | [Servicios de blob](monitoring-supported-metrics.md#microsoftstoragestorageaccountsblobservices), [servicios de archivo](monitoring-supported-metrics.md#microsoftstoragestorageaccountsfileservices), [servicios de cola](monitoring-supported-metrics.md#microsoftstoragestorageaccountsqueueservices) y [servicios de tabla](monitoring-supported-metrics.md#microsoftstoragestorageaccountstableservices)|
|Microsoft.StreamAnalytics/streamingjobs     |  N/D       | [Stream Analytics](monitoring-supported-metrics.md#microsoftstreamanalyticsstreamingjobs)|
|Microsoft.CognitiveServices/accounts     |    N/D     | [Cognitive Services](monitoring-supported-metrics.md#microsoftcognitiveservicesaccounts)|
|Microsoft.OperationalInsights/workspaces (versión preliminar) | Sí|[Áreas de trabajo de Log Analytics](#log-analytics-logs-as-metrics-for-alerting)|


## <a name="log-analytics-logs-as-metrics-for-alerting"></a>Registros de Log Analytics como métricas para alertas 

También puede utilizar las nuevas alertas de métricas en los registros de Log Analytics populares extraídos como métricas como parte de las métricas de versión preliminar de los registros.  
- [Contadores de rendimiento](../log-analytics/log-analytics-data-sources-performance-counters.md) para equipos con Windows y Linux
- [Registros de latidos para Agent Health](../operations-management-suite/oms-solution-agenthealth.md)
- Registros de [administración de actualizaciones](../operations-management-suite/oms-solution-update-management.md)
 
> [!NOTE]
> La métrica o dimensión específicas solo se mostrarán si los datos de ellas existen durante el período elegido. Estas métricas están disponibles para los clientes con áreas de trabajo de Este de EE. UU., Centro-oeste de EE. UU. y Europa Occidental que han participado en la versión preliminar. Si quiere formar parte de esta versión preliminar, regístrese mediante [la encuesta](https://aka.ms/MetricLogPreview).

Se admite la siguiente lista de orígenes de métricas basados en registros de Log Analytics:

Detalles/nombre de métricas  |Dimensiones compatibles  | Tipo de registro  |
|---------|---------|---------|
|Average_Avg. Segundos de disco/lecturas     |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
| Average_Avg. Segundos de disco/escrituras     |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
| Average_Current Disk Queue Length   |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
| Average_Disk Reads/sec    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
| Average_Disk Transfers/sec    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
|   Average_% Free Space    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
| Average_Available MBytes     |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
| Average_% Committed Bytes In Use    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
| Average_Bytes Received/sec    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
|  Average_Bytes Sent/sec    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
|  Average_Bytes Total/sec    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
|  Average_% Processor Time    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
|   Average_Processor Queue Length    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Windows      |
|   Average_% Free Inodes   |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Free Space   |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Used Inodes  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Used Space   |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Disk Read Bytes/sec    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Disk Reads/sec |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Disk Transfers/sec |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Disk Write Bytes/sec   |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Disk Writes/sec    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Free Megabytes |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Logical Disk Bytes/sec |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Available Memory |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Available Swap Space |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Used Memory  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Used Swap Space  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Available MBytes Memory    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Available MBytes Swap  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Page Reads/sec |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Page Writes/sec    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Pages/sec  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Used MBytes Swap Space |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Used Memory MBytes |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Total Bytes Transmitted    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Total Bytes Received   |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Total Bytes    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Total Packets Transmitted  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Total Packets Received |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Total Rx Errors    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Total Tx Errors    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Total Collisions   |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Avg. Segundos de disco/lecturas |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Avg. Segundos de disco/transferencias |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Avg. Segundos de disco/escrituras    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Physical Disk Bytes/sec    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Pct Privileged Time    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Pct User Time  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Used Memory kBytes |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Virtual Shared Memory  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% DPC Time |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Idle Time    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Interrupt Time   |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% IO Wait Time |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Nice Time    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Privileged Time  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% Processor Time   |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_% User Time    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Free Physical Memory   |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Free Space in Paging Files |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Free Virtual Memory    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Processes  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Size Stored In Paging Files    |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Uptime |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Average_Users  |     Sí: Computer, ObjectName, InstanceName, CounterPath y SourceSystem    |   Contador de rendimiento de Linux      |
|    Latido  |     Sí: Computer, OSType, Version y SourceComputerId    |   Registros de latidos |
|    Actualizar |     Sí: Computer, Product, Classification, UpdateState, Optional y Approved    |   Administración de actualizaciones |



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

* Más información sobre la nueva [experiencia de alertas](monitoring-overview-unified-alerts.md).
* Más información sobre las [alertas de registro en Azure](monitor-alerts-unified-log.md).
* Más información sobre las [alertas en Azure](monitoring-overview-alerts.md).
