---
title: Solución Azure SQL Analytics de Log Analytics | Microsoft Docs
description: La solución Azure SQL Analytics le ayuda a administrar las instancias de Azure SQL Database.
services: log-analytics
ms.service: log-analytics
ms.subservice: performance
ms.custom: ''
ms.topic: conceptual
author: danimir
ms.reviewer: carlrab
manager: craigg
ms.date: 11/26/2018
ms.author: v-daljep
ms.openlocfilehash: bf2cbdb57276fccd31af61f2df1b76eeba816e66
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341633"
---
# <a name="monitor-azure-sql-database-using-azure-sql-analytics-preview"></a>Supervisión de instancias de Azure SQL Database con Azure SQL Analytics (versión preliminar)

![Símbolo de Azure SQL Analytics](./media/azure-sql/azure-sql-symbol.png)

Azure SQL Analytics es una solución en la nube que permite supervisar desde un panel único el rendimiento de bases de datos, grupos elásticos e instancias administradas de Azure SQL a escala y entre varias suscripciones. Recopila y visualiza métricas importantes del rendimiento de Azure SQL Database con inteligencia integrada para solucionar problemas de rendimiento.

Con las métricas recopiladas con la solución, puede crear alertas y reglas de supervisión personalizadas. La solución también le ayuda a identificar los problemas de cada capa de la pila de la aplicación. Usa las métricas de diagnóstico de Azure junto con las vistas de Log Analytics para presentar datos sobre todas sus instancias de Azure SQL Database, sus grupos elásticos y las bases de datos en las instancias administradas en una sola área de trabajo de Log Analytics. Log Analytics le ayuda a recopilar, correlacionar y visualizar datos estructurados y no estructurados.

Para obtener información general práctica acerca del uso de la solución Azure SQL Analytics y escenarios de uso habituales, vea el vídeo incrustado:

>[!VIDEO https://www.youtube.com/embed/j-NDkN4GIzg]
>

## <a name="connected-sources"></a>Orígenes conectados

Azure SQL Analytics es una solución de supervisión de uso solo en la nube que admite el streaming de telemetría de diagnósticos para instancias de Azure SQL Database: bases de datos únicas, agrupadas y de instancia administrada. Debido a que la solución no utiliza agentes para conectarse al servicio Log Analytics, la solución no admite la supervisión de instancias de SQL Server hospedadas localmente o en máquinas virtuales, consulte la siguiente tabla de compatibilidad.

| Origen conectado | Compatible | DESCRIPCIÓN |
| --- | --- | --- |
| [Diagnóstico de Azure](../../azure-monitor/platform/collect-azure-metrics-logs.md) | **Sí** | Azure envía directamente los datos de métricas y registros de Azure a Log Analytics. |
| [Cuenta de Almacenamiento de Azure](../../azure-monitor/platform/collect-azure-metrics-logs.md) | Sin  | Log Analytics no lee los datos de una cuenta de almacenamiento. |
| [Agentes de Windows](../../azure-monitor/platform/agent-windows.md) | Sin  | La solución no utiliza agentes directos de Windows. |
| [Agentes de Linux](../../azure-monitor/learn/quick-collect-linux-computer.md) | Sin  | La solución no utiliza agentes directos de Linux. |
| [Grupo de administración de System Center Operations](../platform/om-agents.md) | Sin  | La solución no utiliza ninguna conexión directa entre el agente de Operations Manager y Log Analytics. |

## <a name="configuration"></a>Configuración

Realice los pasos siguientes para agregar la solución Azure SQL Analytics al panel de Azure.

1. Agregue la solución Azure SQL Analytics al área de trabajo desde [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/Microsoft.AzureSQLAnalyticsOMS?tab=Overview).
2. En Azure Portal, haga clic en **+ Crear un recurso** y, a continuación, busque **Azure SQL Analytics**.  
    ![Supervisión + Administración](./media/azure-sql/monitoring-management.png)
3. Seleccione **Azure SQL Analytics (versión preliminar)** en la lista.
4. En el área **Azure SQL Analytics (versión preliminar)**, haga clic en **Crear**.  
    ![Creación](./media/azure-sql/portal-create.png)
5. En el área **Crear nueva solución**, cree una nueva área de trabajo o seleccione una existente, a la que quiera agregar la solución y luego haga clic en **Crear**.

    ![Agregar a área de trabajo](./media/azure-sql/add-to-workspace.png)

### <a name="configure-azure-sql-databases-elastic-pools-and-managed-instances-to-stream-diagnostics-telemetry"></a>Configuración de instancias de Azure SQL Database, grupos elásticos e instancias administradas para la telemetría de diagnósticos de secuencia

Cuando haya creado la solución Azure SQL Analytics en el área de trabajo, tendrá que **configurar cada uno** de los recursos que quiere supervisar para transmitir en secuencias su telemetría de diagnósticos a la solución. Siga las instrucciones detalladas en esta página:

- Habilite Azure Diagnostics en la instancia de Azure SQL Database para [transmitir en secuencias la telemetría de diagnósticos a Azure SQL Analytics](../../sql-database/sql-database-metrics-diag-logging.md).

En la página anterior, también se proporcionan instrucciones sobre cómo habilitar la compatibilidad para supervisar varias suscripciones de Azure desde una única área de trabajo de Azure SQL Analytics como un panel único.

## <a name="using-the-solution"></a>Uso de la solución

Cuando la solución se agrega al área de trabajo, el icono de Azure SQL Analytics se agrega al área de trabajo y aparece en la introducción. El icono muestra el número de bases de datos de Azure SQL Database, grupos elásticos, instancias administradas y bases de datos de instancias administradas de las que la solución recibe datos de telemetría de diagnósticos.

![Icono de Azure SQL Analytics](./media/azure-sql/azure-sql-sol-tile.png)

La solución proporciona dos vistas independientes: una para supervisar las bases de datos y los grupos elásticos de Azure SQL Database y la otra para supervisar Instancia administrada y las bases de datos de esta.

Para ver el panel de supervisión de Azure SQL Analytics de las bases de datos y grupos elásticos de Azure SQL Database, haga clic en la parte superior del icono. Para ver el panel de supervisión de Azure SQL Analytics para Instancia administrada y las bases de datos de esta, haga clic en la parte inferior del icono.

### <a name="viewing-azure-sql-analytics-data"></a>Visualización de los datos de Azure SQL Analytics

El panel incluye la información general de todas las bases de datos que se supervisan a través de distintas perspectivas. Para que estas distintas perspectivas funcionen, debe habilitar las métricas o los registros apropiados en los recursos de SQL para que se transmitan al área de trabajo de Azure Log Analytics.

Tenga en cuenta que si no se transmiten algunas métricas o registros en Azure Log Analytics, los iconos en la solución no se rellenarán con la información de supervisión.

### <a name="azure-sql-database-and-elastic-pool-view"></a>Vista de Azure SQL Database y grupo elástico

Una vez que se selecciona el icono de Azure SQL Analytics para la base de datos, aparece el panel de supervisión.

![Información general de Azure SQL Analytics](./media/azure-sql/azure-sql-sol-overview.png)

Si selecciona cualquiera de los iconos, se abre un informe detallado de la perspectiva específica. Una vez que se ha seleccionado la perspectiva, se abre el informe de la exploración en profundidad.

![Tiempos de expiración de Azure SQL Analytics](./media/azure-sql/azure-sql-sol-metrics.png)

Cada perspectiva de esta vista proporciona resúmenes en el nivel de suscripción, servidor, grupo elástico y base de datos. Además, cada perspectiva muestra a la derecha una perspectiva específica del informe. Seleccione suscripción, servidor, grupo o base de datos en la lista para seguir obteniendo los detalles.

### <a name="managed-instance-and-databases-in-managed-instance-view"></a>Instancia administrada y bases de datos en la vista de Instancia administrada

Una vez que se selecciona el icono de Azure SQL Analytics para las bases de datos, aparece el panel de supervisión.

![Información general de Azure SQL Analytics](./media/azure-sql/azure-sql-sol-overview-mi.png)

Si selecciona cualquiera de los iconos, se abre un informe detallado de la perspectiva específica. Una vez que se ha seleccionado la perspectiva, se abre el informe de la exploración en profundidad.

Si selecciona la vista de Instancia administrada, aparecerán los detalles de uso de Instancia administrada, las bases de datos que contiene y los datos de telemetría de las consultas que se han ejecutado en la instancia.

![Tiempos de expiración de Azure SQL Analytics](./media/azure-sql/azure-sql-sol-metrics-mi.png)

### <a name="perspectives"></a>Perspectivas

La siguiente tabla describe las perspectivas compatibles de dos versiones del panel, una para las bases de datos y grupos elásticos de Azure SQL Database y la otra para Instancia administrada.

| Perspectiva | DESCRIPCIÓN | Compatibilidad de instancias de SQL Database y grupos elásticos | Compatibilidad de Instancia administrada |
| --- | ------- | ----- | ----- |
| Recurso por tipo | Perspectiva que considera todos los recursos supervisados. | SÍ | SÍ |
| Información detallada | Proporciona un informe detallado jerárquico del rendimiento de Intelligent Insights. | SÍ | SÍ |
| Errors | Ofrece un informe detallado jerárquico de los errores de SQL que se produjeron en las bases de datos. | SÍ | SÍ |
| Tiempos de expiración | Ofrece un informe detallado jerárquico de los tiempos de expiración de SQL que se produjeron en las bases de datos. | SÍ | Sin  |
| Bloqueos | Ofrece un informe detallado jerárquico de los bloqueos de SQL que se produjeron en las bases de datos. | SÍ | Sin  |
| Esperas de la base de datos | Ofrece un informe detallado jerárquico de las estadísticas de espera de SQL en el nivel de base de datos. Incluye resúmenes del tiempo de espera total y el tiempo de espera por tipo de espera. |SÍ | SÍ |
| Duración de la consulta | Ofrece un informe detallado jerárquico de las estadísticas de ejecución de consulta, como la duración de la consulta, el uso de CPU, el uso de E/S de datos y el uso de E/S de registro. | SÍ | SÍ |
| Esperas de consulta | Ofrece un informe detallado jerárquico de las estadísticas de espera de consulta por categoría de espera. | SÍ | SÍ |

### <a name="intelligent-insights-report"></a>Informe de Intelligent Insights

Azure SQL Database [Intelligent Insights](../../sql-database/sql-database-intelligent-insights.md) le permite saber lo que ocurre con el rendimiento de todas las instancias de Azure SQL Database. Todas las instancias de Intelligent Insights que se recopilan se pueden visualizar y acceder a través de la perspectiva de información detallada.

![Información detallada de Azure SQL Analytics](./media/azure-sql/azure-sql-sol-insights.png)

### <a name="elastic-pool-and-database-reports"></a>Informes de grupos elásticos y bases de datos

Tanto los grupos elásticos como las bases de datos de SQL Database tienen sus propios informes específicos que muestran cómo se recopilan todos los datos para el recurso en el tiempo especificado.

![Base de datos Azure SQL Analytics](./media/azure-sql/azure-sql-sol-database.png)

![Grupo elástico de Azure SQL](./media/azure-sql/azure-sql-sol-pool.png)

### <a name="query-reports"></a>Informes de consulta

Con las perspectivas de duración de consulta y esperas de consulta, puede correlacionar el rendimiento de cualquier consulta a través del informe de consulta. En este informe se compara el rendimiento de la consulta en distintas bases de datos y facilita el proceso de identificar las bases de datos que se completan de manera correcta la consulta seleccionada en lugar de las lentas.

![Consultas de Azure SQL Analytics](./media/azure-sql/azure-sql-sol-queries.png)

## <a name="permissions"></a>Permisos

Para usar Azure SQL Analytics, los usuarios necesitan que se les conceda un permiso mínimo del rol Lector en Azure. Pero este rol no permite a los usuarios ver el texto de la consulta ni realizar acciones automáticas de optimización. Otros roles más permisivos en Azure que permiten usar todas las funciones de la solución son Propietario, Colaborador, Colaborador de base de datos SQL o Colaborador de SQL Server. También puede crear un rol personalizado en el portal con permisos específicos necesarios solo para usar Azure SQL Analytics, sin acceso para administrar otros recursos.

### <a name="creating-a-custom-role-in-portal"></a>Crear un rol personalizado en el portal

Al reconocer que algunas organizaciones exigen la implementación de controles estrictos de permisos en Azure, hemos diseñado el siguiente script de PowerShell que permite crear el rol personalizado "Operador de supervisión de SQL Analytics" en Azure Portal con los permisos mínimos de lectura y escritura necesarios para usar todas las funciones de Azure SQL Analytics.

Reemplace "{SubscriptionId}" en el script siguiente por el identificador de su suscripción de Azure y ejecute el script después de iniciar sesión con el rol Propietario o Colaborador en Azure.

   ```powershell
    Connect-AzureRmAccount
    Select-AzureRmSubscription {SubscriptionId}
    $role = Get-AzureRmRoleDefinition -Name Reader
    $role.Name = "SQL Analytics Monitoring Operator"
    $role.Description = "Lets you monitor database performance with Azure SQL Analytics as a reader. Does not allow change of resources."
    $role.IsCustom = $true
    $role.Actions.Add("Microsoft.SQL/servers/databases/read");
    $role.Actions.Add("Microsoft.SQL/servers/databases/topQueries/queryText/*");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/read");
    $role.Actions.Add("Microsoft.Sql/servers/databases/automaticTuning/write");
    $role.Actions.Add("Microsoft.Sql/servers/databases/*");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/write");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/read");
    $role.Actions.Add("Microsoft.Sql/servers/advisors/recommendedActions/write");
    $role.Actions.Add("Microsoft.Resources/deployments/write");
    $role.AssignableScopes = "/subscriptions/{SubscriptionId}"
    New-AzureRmRoleDefinition $role
   ```

Después de crear el rol, asígnelo a cada usuario al que necesite conceder permisos personalizados para usar Azure SQL Analytics.

## <a name="analyze-data-and-create-alerts"></a>Análisis de datos y creación de alertas

El análisis de datos en Azure SQL Analytics se basa en el [lenguaje de Log Analytics](../log-query/get-started-queries.md) para los informes y consultas personalizados. Encontrará una descripción de los datos disponibles recopilados del recurso de base de datos para consultas personalizadas en [Métricas y registros disponibles](../../sql-database/sql-database-metrics-diag-logging.md#metrics-and-logs-available).

Para configurar alertas automáticas en la solución, se escribe una consulta de Log Analytics que desencadena una alerta cuando se cumple una condición. Abajo encontrará varios ejemplos de consultas de Log Analytics en las que se pueden configurar alertas en la solución.

### <a name="creating-alerts-for-azure-sql-database"></a>Creación de alertas para una instancia de Azure SQL Database

Las [alertas se pueden crear fácilmente](../../azure-monitor/platform/alerts-metric.md) con los datos procedentes de los recursos de Azure SQL Database. Estas son algunas [consultas de registros](../../azure-monitor/log-query/log-query-overview.md) útiles que puede usar para las alertas de registros:

#### <a name="high-cpu-on-azure-sql-database"></a>CPU alta en Azure SQL Database

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

#### <a name="high-cpu-on-azure-sql-database-elastic-pools"></a>CPU alta en los grupos elásticos de Azure SQL Database

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

#### <a name="azure-sql-database-storage-in-average-above-95-in-the-last-1-hr"></a>Almacenamiento promedio de Azure SQL Database superior al 95 % en la última hora

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

#### <a name="alert-on-intelligent-insights"></a>Alerta sobre Intelligent Insights

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

### <a name="creating-alerts-for-managed-instance"></a>Creación de alertas para Instancia administrada

#### <a name="managed-instance-storage-is-above-90"></a>El almacenamiento de Instancia administrada es superior al 90 %

```
let storage_percentage_threshold = 90;
AzureDiagnostics
| where Category =="ResourceUsageStats"
| summarize (TimeGenerated, calculated_storage_percentage) = arg_max(TimeGenerated, todouble(storage_space_used_mb_s) *100 / todouble (reserved_storage_mb_s))
   by ResourceId
| where calculated_storage_percentage > storage_percentage_threshold
```

> [!NOTE]
> - Como requisito previo para configurar esta alerta, la Instancia administrada supervisada necesita tener habilitada la transmisión del registro ResourceUsageStats a la solución.
> - Esta consulta requiere que se configure una regla de alerta para activar una alerta cuando existen resultados (> 0 resultados) de la consulta, lo que indica que la condición existe en la instancia administrada. La salida es el consumo de porcentaje de almacenamiento en la instancia administrada.

#### <a name="managed-instance-cpu-average-consumption-is-above-95-in-the-last-1-hr"></a>El promedio de consumo de CPU de instancia administrada es superior al 95 % en la última hora

```
let cpu_percentage_threshold = 95;
let time_threshold = ago(1h);
AzureDiagnostics
| where Category == "ResourceUsageStats" and TimeGenerated > time_threshold
| summarize avg_cpu = max(todouble(avg_cpu_percent_s)) by ResourceId
| where avg_cpu > cpu_percentage_threshold
```

> [!NOTE]
> - Como requisito previo para configurar esta alerta, la Instancia administrada supervisada necesita tener habilitada la transmisión del registro ResourceUsageStats a la solución.
> - Esta consulta requiere que se configure una regla de alerta para activar una alerta cuando existen resultados (> 0 resultados) de la consulta, lo que indica que la condición existe en la instancia administrada. El resultado es el porcentaje de consumo de uso promedio de CPU en un período definido en la instancia administrada.

### <a name="pricing"></a>Precios

Aunque la solución puede usarse de forma gratuita, se aplicarán cargos por el uso de telemetría de diagnósticos que supere las unidades gratuitas de ingesta de datos asignadas cada mes; para obtener más información, vea [Precios de Log Analytics](https://azure.microsoft.com/en-us/pricing/details/monitor). Las unidades gratuitas de ingesta de datos especificadas habilitan la supervisión gratuita de varias bases de datos al mes. Tenga en cuenta que las bases de datos más activas con cargas de trabajo más pesadas ingieren más datos que las bases de datos inactivas. Puede supervisar fácilmente su ingesta de datos en la solución; para hacerlo, seleccione el área de trabajo de OMS en el menú de navegación de Azure SQL Analytics y, después, seleccione Uso y costos estimados.

## <a name="next-steps"></a>Pasos siguientes

- Use [Búsquedas de registros](../../azure-monitor/log-query/log-query-overview.md) en Log Analytics para ver datos detallados de Azure SQL.
- [Cree sus propios paneles](../../azure-monitor/platform/dashboards.md) que muestren datos de Azure SQL.
- [Cree alertas](../../azure-monitor/platform/alerts-overview.md) cuando se produzcan eventos específicos de Azure SQL.
