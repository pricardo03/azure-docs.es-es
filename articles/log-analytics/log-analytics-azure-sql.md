---
title: Solución Azure SQL Analytics de Log Analytics | Microsoft Docs
description: La solución Azure SQL Analytics le ayuda a administrar las instancias de Azure SQL Database.
services: log-analytics
documentationcenter: ''
author: danimir
manager: carmonm
ms.reviewer: carlrab
ms.assetid: b2712749-1ded-40c4-b211-abc51cc65171
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/03/2018
ms.author: v-daljep
ms.component: na
ms.openlocfilehash: 82845f475857f9a911febd496e86eb2a60f69c25
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782250"
---
# <a name="monitor-azure-sql-databases-using-azure-sql-analytics-preview"></a>Supervisar instancias de Azure SQL Database con Azure SQL Analytics (versión preliminar)

![Símbolo de Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-symbol.png)

Azure SQL Analytics es una solución de supervisión en la nube para supervisar el rendimiento de las instancias de Azure SQL Database, los grupos elásticos y las instancias administradas a escala entre varias suscripciones. Recopila y visualiza métricas importantes del rendimiento de Azure SQL Database con inteligencia integrada para solucionar problemas de rendimiento.

Con las métricas recopiladas con la solución, puede crear alertas y reglas de supervisión personalizadas. La solución también le ayuda a identificar los problemas de cada capa de la pila de la aplicación. Usa las métricas de diagnóstico de Azure junto con las vistas de Log Analytics para presentar datos sobre todas sus instancias de Azure SQL Database, sus grupos elásticos y las bases de datos en las instancias administradas en una sola área de trabajo de Log Analytics. Log Analytics le ayuda a recopilar, correlacionar y visualizar datos estructurados y no estructurados.

Actualmente, la versión preliminar de esta solución admite hasta 200 000 instancias de Azure SQL Database y 5000 grupos elásticos de SQL por área de trabajo.

Para obtener información general práctica acerca del uso de la solución Azure SQL Analytics y escenarios de uso habituales, vea el vídeo incrustado:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

## <a name="connected-sources"></a>Orígenes conectados

Azure SQL Analytics es una solución de supervisión de uso solo en la nube que admite el streaming de telemetría de diagnósticos para grupos elásticos, instancias de SQL Azure Database e instancias administradas. Debido a que no utiliza agentes para conectarse al servicio Log Analytics, la solución no admite la supervisión de instancias locales de SQL Server o máquinas virtuales, consulte la siguiente tabla de compatibilidad.

| Origen conectado | Compatible | DESCRIPCIÓN |
| --- | --- | --- |
| **[Azure Diagnostics](log-analytics-azure-storage.md)** | **Sí** | Azure envía directamente los datos de métricas y registros de Azure a Log Analytics. |
| [Cuenta de Almacenamiento de Azure](log-analytics-azure-storage.md) | Sin  | Log Analytics no lee los datos de una cuenta de almacenamiento. |
| [Agentes de Windows](log-analytics-windows-agent.md) | Sin  | La solución no utiliza agentes directos de Windows. |
| [Agentes de Linux](log-analytics-linux-agents.md) | Sin  | La solución no utiliza agentes directos de Linux. |
| [Grupo de administración de SCOM](log-analytics-om-agents.md) | No | La solución no utiliza una conexión directa entre el agente de SCOM y Log Analytics. |

## <a name="configuration"></a>Configuración

Realice los pasos siguientes para agregar la solución Azure SQL Analytics al área de trabajo.

1. Agregue la solución Azure SQL Analytics al área de trabajo desde [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. En Azure Portal, haga clic en **+ Crear un recurso** y, a continuación, busque **Azure SQL Analytics**.  
    ![Supervisión + Administración](./media/log-analytics-azure-sql/monitoring-management.png)
3. Seleccione **Azure SQL Analytics (versión preliminar)** en la lista.
4. En el área **Azure SQL Analytics (versión preliminar)**, haga clic en **Crear**.  
    ![Creación](./media/log-analytics-azure-sql/portal-create.png)
5. En el área **Crear nueva solución**, cree una nueva área de trabajo o seleccione una existente, a la que quiera agregar la solución y luego haga clic en **Crear**.  
    ![Agregar a área de trabajo](./media/log-analytics-azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-and-elastic-pools-to-stream-diagnostics-telemetry"></a>Configurar las instancias de Azure SQL Database y los grupos elásticos para la telemetría de diagnósticos de secuencia

Una vez haya creado la solución Azure SQL Analytics en el área de trabajo, con el fin de supervisar el rendimiento de las instancias de Azure SQL Database o los grupos elásticos, deberá **configurar cada** instancia de Azure SQL Database y grupo elástico que quiera supervisar para transmitir su telemetría de diagnósticos a la solución.

- Habilite Diagnósticos de Azure para sus instancias de Azure SQL Database y para los grupos elásticos y [configúrelo para que envíe sus datos a Log Analytics](../sql-database/sql-database-metrics-diag-logging.md).

### <a name="to-configure-multiple-azure-subscriptions"></a>Configuración de varias suscripciones de Azure

Para admitir varias suscripciones, use el script de PowerShell de [Enable Azure resource metrics logging using PowerShell](https://blogs.technet.microsoft.com/msoms/2017/01/17/enable-azure-resource-metrics-logging-using-powershell/) (Habilitar registro de métricas de recursos de Azure mediante PowerShell). Proporcione el identificador de recurso del área de trabajo como un parámetro al ejecutar el script para enviar los datos de diagnóstico de los recursos de una suscripción de Azure a un área de trabajo de otra suscripción de Azure.

**Ejemplo**

```
PS C:\> $WSID = "/subscriptions/<subID>/resourcegroups/oms/providers/microsoft.operationalinsights/workspaces/omsws"
```

```
PS C:\> .\Enable-AzureRMDiagnostics.ps1 -WSID $WSID
```

## <a name="using-the-solution"></a>Uso de la solución

Cuando la solución se agrega al área de trabajo, el icono de Azure SQL Analytics se agrega al área de trabajo y aparece en la introducción. El icono muestra el número de bases de datos y grupos elásticos de Azure SQL a los que está conectada la solución.

![Icono de Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-tile.png)

### <a name="viewing-azure-sql-analytics-data"></a>Visualización de los datos de Azure SQL Analytics

Haga clic en el icono de **Azure SQL Analytics** para que se abra el panel de Azure SQL Analytics. El panel incluye la información general de todas las bases de datos que se supervisan a través de distintas perspectivas. Para que estas distintas perspectivas funcionen, debe habilitar las métricas o los registros apropiados en los recursos de SQL para que se transmitan al área de trabajo de Azure Log Analytics.

![Información general de Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-overview.png)

Si selecciona cualquiera de los iconos, se abre un informe detallado de la perspectiva específica. Una vez que se ha seleccionado la perspectiva, se abre el informe de la exploración en profundidad.

![Tiempos de expiración de Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-metrics.png)

Cada perspectiva proporciona resúmenes en el nivel de suscripción, servidor, grupo elástico y base de datos. Además, cada perspectiva muestra a la derecha una perspectiva específica del informe. Seleccione suscripción, servidor, grupo o base de datos en la lista para seguir obteniendo los detalles.

| Perspectiva | Descripción |
| --- | --- |
| Recurso por tipo | Perspectiva que considera todos los recursos supervisados. El informe detallado proporciona el resumen de las métricas de GB y DTU. |
| Información detallada | Ofrece un informe detallado jerárquico de Intelligent Insights. Más información sobre Intelligent Insights. |
| Errors | Ofrece un informe detallado jerárquico de los errores de SQL que se produjeron en las bases de datos. |
| Tiempos de expiración | Ofrece un informe detallado jerárquico de los tiempos de expiración de SQL que se produjeron en las bases de datos. |
| Bloqueos | Ofrece un informe detallado jerárquico de los bloqueos de SQL que se produjeron en las bases de datos. |
| Esperas de la base de datos | Ofrece un informe detallado jerárquico de las estadísticas de espera de SQL en el nivel de base de datos. Incluye resúmenes del tiempo de espera total y el tiempo de espera por tipo de espera. |
| Duración de la consulta | Ofrece un informe detallado jerárquico de las estadísticas de ejecución de consulta, como la duración de la consulta, el uso de CPU, el uso de E/S de datos y el uso de E/S de registro. |
| Esperas de consulta | Ofrece un informe detallado jerárquico de las estadísticas de espera de consulta por categoría de espera. |

### <a name="intelligent-insights-report"></a>Informe de Intelligent Insights

Azure SQL Database [Intelligent Insights](../sql-database/sql-database-intelligent-insights.md) le permite saber lo que ocurre con el rendimiento de la base de datos. Todas las instancias de Intelligent Insights que se recopilan se pueden visualizar y acceder a través de la perspectiva de información detallada.

![Información detallada de Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Informes de grupos elásticos y bases de datos

Tanto los grupos elásticos como las bases de datos tienen sus propios informes específicos que muestran cómo se recopilan todos los datos para el recurso en el tiempo especificado.

![Base de datos Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-database.png)

![Grupo elástico de Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Informes de consulta

Con las perspectivas de duración de consulta y esperas de consulta, puede correlacionar el rendimiento de cualquier consulta a través del informe de consulta. En este informe se compara el rendimiento de la consulta en distintas bases de datos y facilita el proceso de identificar las bases de datos que se completan de manera correcta la consulta seleccionada en lugar de las lentas.

![Consultas de Azure SQL Analytics](./media/log-analytics-azure-sql/azure-sql-sol-queries.png)

### <a name="analyze-data-and-create-alerts"></a>Análisis de datos y creación de alertas

Las [alertas se pueden crear fácilmente](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md) con los datos procedentes de los recursos de Azure SQL Database. Estas son algunas consultas de [búsqueda de registros](log-analytics-log-searches.md) útiles que puede usar para las alertas de registros:

*CPU alta en Azure SQL Database*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/DATABASES/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - El requisito previo para configurar esta alerta es que las bases de datos supervisadas transmitan las métricas de diagnóstico (opción "Todas las métricas") a la solución.
> - Reemplace el valor de MetricName cpu_percent por dtu_consumption_percent para obtener resultados DTU altos.

*CPU alta en los grupos elásticos de Azure SQL Database*

```
AzureMetrics 
| where ResourceProvider=="MICROSOFT.SQL"
| where ResourceId contains "/ELASTICPOOLS/"
| where MetricName=="cpu_percent" 
| summarize AggregatedValue = max(Maximum) by bin(TimeGenerated, 5m)
| render timechart
```

> [!NOTE]
> - El requisito previo para configurar esta alerta es que las bases de datos supervisadas transmitan las métricas de diagnóstico (opción "Todas las métricas") a la solución.
> - Reemplace el valor de MetricName cpu_percent por dtu_consumption_percent para obtener resultados DTU altos.

*Almacenamiento de Azure SQL Database en promedio por encima del 95 % en la última hora*

```
let time_range = 1h;
let storage_threshold = 95;
AzureMetrics
| where ResourceId contains "/DATABASES/"
| where MetricName == "storage_percent"
| summarize max_storage = max(Average) by ResourceId, bin(TimeGenerated, time_range)
| where max_storage > storage_threshold
| distinct ResourceId
```

> [!NOTE]
> - El requisito previo para configurar esta alerta es que las bases de datos supervisadas transmitan las métricas de diagnóstico (opción "Todas las métricas") a la solución.
> - Esta consulta requiere que se configure una regla de alerta para activar una alerta cuando existen resultados (> 0 resultados) de la consulta, lo que indica que la condición existe en algunas bases de datos. La salida es una lista de recursos de la base de datos que superan el valor storage_threshold dentro del valor de time_range definido.
> - La salida es una lista de recursos de la base de datos que superan el valor storage_threshold dentro del valor de time_range definido.

*Alerta sobre Intelligent Insights*

```
let alert_run_interval = 1h;
let insights_string = "hitting its CPU limits";
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| where TimeGenerated > ago(alert_run_interval)
| where rootCauseAnalysis_s contains insights_string
| distinct ResourceId
```

> [!NOTE]
> - El requisito previo para configurar esta alerta es que las bases de datos supervisadas transmitan el registro de diagnóstico de SQLInsights a la solución.
> - Esta consulta requiere que se configure una regla de alerta para que se ejecute con la misma frecuencia que alert_run_interval para evitar resultados duplicados. La regla debe configurarse para activar la alerta cuando existen resultados (> 0 resultados) de la consulta.
> - Personalice alert_run_interval para especificar el intervalo de tiempo para comprobar si la condición ha ocurrido en bases de datos configuradas para transmitir el registro de SQLInsights a la solución.
> - Personalice el valor de insights_string para capturar la salida del texto de análisis de la causa principal de Insights. Este es el mismo texto que se muestra en la interfaz de usuario de la solución y que puede utilizar a partir de las conclusiones existentes. Como alternativa, puede utilizar la consulta siguiente para ver el texto de todas las conclusiones generadas en la suscripción. Utilice la salida de la consulta para recopilar las distintas cadenas para configurar alertas sobre Insights.

```
AzureDiagnostics
| where Category == "SQLInsights" and status_s == "Active" 
| distinct rootCauseAnalysis_s
```

## <a name="next-steps"></a>Pasos siguientes

- Use [Búsquedas de registros](log-analytics-log-searches.md) en Log Analytics para ver datos detallados de Azure SQL.
- [Cree sus propios paneles](log-analytics-dashboards.md) que muestren datos de Azure SQL.
- [Cree alertas](log-analytics-alerts.md) cuando se produzcan eventos específicos de Azure SQL.
