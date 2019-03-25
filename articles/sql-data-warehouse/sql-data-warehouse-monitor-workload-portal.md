---
title: 'Carga de trabajo del Monitor: portal de Azure | Microsoft Docs'
description: Supervisar Azure SQL Data Warehouse mediante Azure portal
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 03/22/2019
ms.author: kevin
ms.reviewer: jrasnick
ms.openlocfilehash: 6c8ce090039e3d5cc85c86d920710294de2165f9
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58405788"
---
# <a name="monitor-workload---azure-portal"></a>Carga de trabajo del Monitor: portal de Azure

En este artículo se describe cómo usar el portal de Azure para supervisar la carga de trabajo. Esto incluye la configuración de registros de Azure Monitor para investigar las tendencias de ejecución y la carga de trabajo de consulta con log analytics para [Azure SQL Data Warehouse](https://azure.microsoft.com/blog/workload-insights-with-sql-data-warehouse-delivered-through-azure-monitor-diagnostic-logs-pass/).

## <a name="prerequisites"></a>Requisitos previos

- Suscripción de Azure: Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.
- Azure SQL Data Warehouse: Se recopilarán los registros de SQL data warehouse. Si no tienes un aprovisionado en SQL data warehouse, consulte las instrucciones de [crear SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-tutorial).

## <a name="create-a-log-analytics-workspace"></a>Creación de un área de trabajo de Log Analytics

Vaya a la hoja examinar las áreas de trabajo de Log Analytics y crear un área de trabajo 

![Áreas de trabajo de Log Analytics](media/sql-data-warehouse-monitor/log_analytics_workspaces.png)

![Agregar área de trabajo de análisis](media/sql-data-warehouse-monitor/add_analytics_workspace.png)

![Agregar área de trabajo de análisis](media/sql-data-warehouse-monitor/add_analytics_workspace_2.png)

Para obtener más detalles sobre las áreas de trabajo, visite el siguiente [documentación](https://docs.microsoft.com/azure/azure-monitor/platform/manage-access#create-a-workspace).

## <a name="turn-on-diagnostic-logs"></a>Activar los registros de diagnóstico 

Configurar opciones de diagnóstico para emitir registros de SQL data warehouse. Los registros constan de las vistas de datos de telemetría de su almacén de datos equivalente en el rendimiento más utilizado de solución de problemas de las DMV para SQL Data Warehouse. Actualmente se admiten las siguientes vistas:

- [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_dms_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql?view=aps-pdw-2016-au7)
- [sys.dm_pdw_sql_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql?view=aps-pdw-2016-au7)


![Habilitación de registros de diagnóstico](media/sql-data-warehouse-monitor/enable_diagnostic_logs.png)

Los registros se pueden emitir a Azure Storage, Stream Analytics o Log Analytics. Para este tutorial, seleccione Log Analytics.

![Especificar los registros](media/sql-data-warehouse-monitor/specify_logs.png)

## <a name="run-queries-against-log-analytics"></a>Ejecutar consultas en Log Analytics

Desplácese hasta el área de trabajo de Log Analytics donde se puede hacer lo siguiente:

- Análisis de registros mediante consultas de registros y guardar las consultas para reutilizarlas
- Guardar las consultas para reutilizarlas
- Crear alertas del registro
- Resultados de la consulta de PIN a un panel

Para obtener más información sobre las capacidades de las consultas de registro, visite el siguiente [documentación](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language).

![Editor de área de trabajo de log Analytics](media/sql-data-warehouse-monitor/log_analytics_workspace_editor.png)



![Registrar consultas de área de trabajo de análisis](media/sql-data-warehouse-monitor/log_analytics_workspace_queries.png)

## <a name="sample-log-queries"></a>Consultas de registro de ejemplo



```Kusto
//List all queries 
AzureDiagnostics
| where Category contains "ExecRequests"
| project TimeGenerated, StartTime_t, EndTime_t, Status_s, Command_s, ResourceClass_s, duration=datetime_diff('millisecond',EndTime_t, StartTime_t)
```
```Kusto
//Chart the most active resource classes
AzureDiagnostics
| where Category contains "ExecRequests"
| where Status_s == "Completed"
| summarize totalQueries = dcount(RequestId_s) by ResourceClass_s
| render barchart 
```
```Kusto
//Count of all queued queries
AzureDiagnostics
| where Category contains "waits" 
| where Type_s == "UserConcurrencyResourceType"
| summarize totalQueuedQueries = dcount(RequestId_s)
```
## <a name="next-steps"></a>Pasos siguientes

Ahora que se ha instalado y configurado los registros de Azure monitor, [personalizar los paneles de Azure](https://docs.microsoft.com/azure/azure-portal/azure-portal-dashboards) compartir a través de su equipo.
