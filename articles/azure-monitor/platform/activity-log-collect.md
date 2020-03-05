---
title: Recopilación y análisis de los registros de actividad de Azure en un área de trabajo de Log Analytics | Microsoft Docs
description: Recopile el registro de actividad de Azure en los registros de Azure Monitor y utilice la solución de supervisión para analizar tal registro y hacer búsquedas en él en todas las suscripciones de Azure.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/30/2019
ms.openlocfilehash: 044f974d83eba098820639e67412110329d5ad7d
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77668985"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics-workspace-in-azure-monitor"></a>Recopilación y análisis de los registros de actividad de Azure en un área de trabajo de Log Analytics en Azure Monitor

> [!WARNING]
> Ahora puede recopilar el Registro de actividad en un área de trabajo Log Analytics mediante una configuración de diagnóstico similar a la de la recopilación de registros de recursos. Consulte [Collect and analyze Azure activity logs in Log Analytics workspace in Azure Monitor](diagnostic-settings-legacy.md) (Recopilación y análisis de registros de actividad en un área de trabajo de Log Analytics en Azure Monitor).

El [registro de actividad de Azure](platform-logs-overview.md) proporciona información de los eventos de nivel de suscripción que se han producido en la suscripción de Azure. Este artículo describe cómo recopilar el registro de actividad de un área de trabajo de Log Analytics y cómo usar la [solución de supervisión](../insights/solutions.md) de Activity Log Analytics, que proporciona vistas y consultas de registros para analizar estos datos. 

La conexión del registro de actividad a un área de trabajo de Log Analytics proporciona las siguientes ventajas:

- Consolidar el registro de actividad de varias suscripciones de Azure en una ubicación para su análisis.
- Almacenar las entradas del registro de actividad durante más de 90 días.
- Correlacionar los datos del registro de actividad con otros datos de supervisión recopilados por Azure Monitor.
- Usar las [consultas de registro](../log-query/log-query-overview.md) para realizar un análisis complejo y obtener información detallada sobre las entradas del registro de actividad.

## <a name="connect-to-log-analytics-workspace"></a>Conexión al área de trabajo de Log Analytics
Una sola área de trabajo se puede conectar al registro de actividad de varias suscripciones en el mismo inquilino de Azure. Para obtener información sobre la recopilación en varios inquilinos, consulte [Collect Azure Activity Logs into a Log Analytics workspace across subscriptions in different Azure Active Directory tenants](activity-log-collect-tenants.md) (Recopilación de registros de actividad de Azure en un área de trabajo de Log Analytics entre suscripciones en diferentes inquilinos de Azure Active Directory).

> [!IMPORTANT]
> Es posible que reciba un error con el siguiente procedimiento si los proveedores de recursos Microsoft.OperationalInsights y Microsoft.OperationsManagement no están registrados para su suscripción. Para registrar estos proveedores, consulte [Tipos y proveedores de recursos de Azure](../../azure-resource-manager/management/resource-providers-and-types.md).

Use el procedimiento siguiente para conectar el registro de actividad a su área de trabajo de Log Analytics:

1. Desde el menú **Áreas de trabajo de Log Analytics** en Azure Portal, seleccione el área de trabajo para recopilar el registro de actividad.
1. En la sección **Orígenes de datos del área de trabajo** del menú del área de trabajo, seleccione **Registro de actividad de Azure**.
1. Haga clic en la suscripción que desea conectar.

    ![Áreas de trabajo](media/activity-log-export/workspaces.png)

1. Haga clic en **Conectar** para conectar el registro de actividad en la suscripción al área de trabajo seleccionada. Si la suscripción ya está conectada a otra área de trabajo, haga clic en **Desconectar** primero para desconectarla.

    ![Conexión de áreas de trabajo](media/activity-log-export/connect-workspace.png)

## <a name="analyze-in-log-analytics-workspace"></a>Análisis en el área de trabajo de Log Analytics
Cuando conecta un registro de actividad a una área de trabajo de Log Analytics, las entradas se escribirán en el área de trabajo en una tabla denominada **AzureActivity** que puede recuperar con una [consulta de registro](../log-query/log-query-overview.md). La estructura de esta tabla varía en función de la [categoría de la entrada de registro](activity-log-view.md#categories-in-the-activity-log). Consulte [Esquema de eventos del registro de actividad de Azure](activity-log-schema.md) para obtener una descripción de cada categoría.

## <a name="activity-logs-analytics-monitoring-solution"></a>Solución de supervisión de Activity Log Analytics
La solución de supervisión de Azure Log Analytics incluye varias vistas y consultas de registros para analizar las entradas del registro de actividad en el área de trabajo de Log Analytics.

### <a name="install-the-solution"></a>Instalar la solución
Use el procedimiento [Instalación de una solución de supervisión](../insights/solutions.md#install-a-monitoring-solution) para instalar la solución **Activity Log Analytics**. No se necesita ninguna configuración adicional.

### <a name="use-the-solution"></a>Uso de la solución
A las soluciones de supervisión se accede desde el menú **Supervisar** de Azure Portal. Seleccione **Más** en la sección **Información detallada** para abrir la página **Información general** con los iconos de la solución. El icono **Registros de actividad de Azure** muestra un recuento del número de registros **AzureActivity** del área de trabajo.

![Icono Registros de actividad de Azure](media/collect-activity-logs/azure-activity-logs-tile.png)


Haga clic en el icono **Registros de actividad de Azure** para abrir la vista **Registros de actividad de Azure**. La vista incluye los elementos de visualización de la tabla siguiente. Cada uno de ellos muestra hasta otros diez elementos que coinciden con sus criterios para el intervalo de tiempo especificado. Puede ejecutar una consulta de registros que devuelve todos los registros coincidentes; para ello, haga clic en **Ver todo** en la parte inferior del elemento.

![Panel Registros de actividad de Azure](media/collect-activity-logs/activity-log-dash.png)

| Elemento de visualización | Descripción |
| --- | --- |
| Entradas del registro de actividad de Azure | Muestra un gráfico de barras de los principales totales del registro de entrada del registro de actividad del intervalo de fechas seleccionado y una lista de los 10 principales llamadores de actividad. Haga clic en el gráfico de barras para ejecutar una búsqueda de registros de `AzureActivity`. Haga clic en un elemento de llamador para ejecutar una búsqueda de registros que devuelva todas las entradas de registro de actividad de ese elemento. |
| Registros de actividad por estado | Muestra un gráfico de anillos del estado del registro de actividad de Azure para el intervalo de fechas seleccionado y una lista de los diez principales registros de estado. Haga clic en el gráfico para ejecutar una consulta de registros de `AzureActivity | summarize AggregatedValue = count() by ActivityStatus`. Haga clic en un elemento de estado para ejecutar una búsqueda de registros que devuelva todas las entradas de registro de actividad de ese registro de estado. |
| Registros de actividad por recurso | Muestra el número total de recursos con registros de actividad y enumera los diez principales recursos con recuentos de registro para cada recurso. Haga clic en el área total para ejecutar una búsqueda de registros de `AzureActivity | summarize AggregatedValue = count() by Resource`, que muestra todos los recursos de Azure disponibles para la solución. Haga clic en un recurso para ejecutar una consulta de registros que devuelva todos los registros de actividad de ese recurso. |
| Registros de actividad por proveedor de recursos | Muestra el número total de proveedores de recursos que producen registros de actividad y enumera los diez principales. Haga clic en el área total para ejecutar una consulta de registros de `AzureActivity | summarize AggregatedValue = count() by ResourceProvider`, que muestra todos los proveedores de recursos de Azure. Haga clic en un proveedor de recursos para ejecutar una consulta de registros que devuelva todos los registros de actividad para el proveedor. |

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre el [registro de actividad](platform-logs-overview.md).
- Más información sobre la [plataforma de datos de Azure Monitor](data-platform.md).
- Use las [consultas de registros](../log-query/log-query-overview.md) para ver información detallada de los registros de actividad.
