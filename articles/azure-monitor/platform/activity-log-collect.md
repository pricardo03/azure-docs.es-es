---
title: Recopilar y analizar los registros de actividad de Azure en el área de trabajo de Log Analytics | Microsoft Docs
description: Recopilar el registro de actividad de Azure en registros de Azure Monitor y usar la solución de supervisión para analizar y buscar en el registro de actividad de Azure en todas las suscripciones de Azure.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: dbac4c73-0058-4191-a906-e59aca8e2ee0
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 05/19/2019
ms.author: bwren
ms.openlocfilehash: 5839fd40a128097e400f13acbe4fb6ef90c656b7
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66248135"
---
# <a name="collect-and-analyze-azure-activity-logs-in-log-analytics-workspace-in-azure-monitor"></a>Recopilar y analizar los registros de actividad de Azure en el área de trabajo de Log Analytics en Azure Monitor
El [Azure Activity Log](activity-logs-overview.md) proporciona información sobre los eventos de nivel de suscripción que se han producido en su suscripción de Azure. Este artículo describe cómo recopilar el registro de actividad en un área de trabajo de Log Analytics y cómo usar el análisis de registro de actividad [solución de supervisión](../insights/solutions.md), que proporciona vistas y consultas de registros para analizar estos datos. 

Conectar el registro de actividad a un área de trabajo de Log Analytics proporciona las siguientes ventajas:

- Consolidar el registro de actividad de varias suscripciones de Azure en una ubicación para el análisis.
- Store entradas del registro de actividad durante más de 90 días.
- Correlacionar datos de registro de actividad con otros datos de supervisión recopilados por Azure Monitor.
- Use [registrar consultas](../log-query/log-query-overview.md) para realizar un análisis complejo y obtener información detallada sobre las entradas del registro de actividad.

## <a name="connect-to-log-analytics-workspace"></a>Conectarse al área de trabajo de Log Analytics
Un registro de actividad se puede conectar a solo un área de trabajo, pero una sola área de trabajo se puede conectar al registro de actividad para varias suscripciones en el mismo inquilino de Azure. Para la colección a través de varios inquilinos, consulte [recopilar los registros de actividad de Azure en un área de trabajo de Log Analytics entre suscripciones en diferentes Azure Active Directory a los inquilinos](activity-log-collect-tenants.md).

Use el procedimiento siguiente para conectar el registro de actividad en el área de trabajo de Log Analytics:

1. Desde el **las áreas de trabajo de Log Analytics** menú en el portal de Azure, seleccione el área de trabajo para recopilar el registro de actividad.
1. En el **orígenes de datos del área de trabajo** sección del menú del área de trabajo, seleccione **registro de actividad de Azure**.
1. Haga clic en la suscripción que desea conectarse.

    ![Áreas de trabajo](media/activity-log-export/workspaces.png)

1. Haga clic en **Connect** para conectar el registro de actividad en la suscripción al área de trabajo seleccionado. Si la suscripción ya está conectada a otra área de trabajo, haga clic en **desconexión** primero para desconectarla.

    ![Conectarse a las áreas de trabajo](media/activity-log-export/connect-workspace.png)

## <a name="analyze-in-log-analytics-workspace"></a>Analizar en el área de trabajo de Log Analytics
Cuando se conecta a un registro de actividad a un área de trabajo de Log Analytics, las entradas se escribirán en el área de trabajo en una tabla denominada **AzureActivity** que se pueden recuperar con una [consulta de registro](../log-query/log-query-overview.md). La estructura de esta tabla varía en función de la [categoría de entrada del registro](activity-logs-overview.md#categories-in-the-activity-log). Consulte [esquema de eventos de registro de actividad de Azure](activity-log-schema.md) para obtener una descripción de cada categoría.

## <a name="activity-logs-analytics-monitoring-solution"></a>Solución de supervisión de análisis de registros de actividad
La solución de supervisión de Azure Log Analytics incluye varias consultas de registros y vistas para analizar las entradas del registro de actividad en el área de trabajo de Log Analytics.

### <a name="install-the-solution"></a>Instalar la solución
Use el procedimiento de [instalar una solución de supervisión](../insights/solutions.md#install-a-monitoring-solution) para instalar el **Activity Log Analytics** solución. No es necesaria ninguna configuración adicional.

### <a name="use-the-solution"></a>Uso de la solución
Soluciones de supervisión se obtiene acceso desde el **Monitor** menú en el portal de Azure. Seleccione **más** en el **Insights** sección para abrir el **Introducción** página con los iconos de la solución. El **los registros de actividad de Azure** icono muestra un recuento del número de **AzureActivity** registros en el área de trabajo.

![Icono Registros de actividad de Azure](media/collect-activity-logs/azure-activity-logs-tile.png)


Haga clic en el **los registros de actividad de Azure** icono para abrir el **los registros de actividad de Azure** vista. La vista incluye los elementos de visualización en la tabla siguiente. Cada parte muestra hasta 10 elementos que coinciden con los criterios de esa partes para el intervalo de tiempo especificado. Puede ejecutar una consulta de registros que devuelve todos los registros coincidentes, haga clic en **ver todo** en la parte inferior de la parte.

![Panel Registros de actividad de Azure](media/collect-activity-logs/activity-log-dash.png)

| Elemento de visualización | DESCRIPCIÓN |
| --- | --- |
| Entradas del registro de actividad de Azure | Muestra un gráfico de barras de la primera entrada de registro de actividad de Azure totales de registro para el intervalo de fechas que ha seleccionado y muestra una lista de los llamadores de 10 actividad principales. Haga clic en el gráfico de barras para ejecutar una búsqueda de registros de `AzureActivity`. Haga clic en un elemento de llamador para ejecutar una búsqueda de registros que devuelva todas las entradas de registro de actividad para ese elemento. |
| Registros de actividad por estado | Muestra un gráfico de anillos para el estado de registro de actividad de Azure para el intervalo de fechas seleccionado y una lista de los registros de diez estado principales. Haga clic en el gráfico para ejecutar una consulta de registro para `AzureActivity | summarize AggregatedValue = count() by ActivityStatus`. Haga clic en un elemento de estado para ejecutar una búsqueda de registros que devuelva todas las entradas de registro de actividad para ese registro de estado. |
| Registros de actividad por recurso | Muestra el número total de recursos con los registros de actividad y se enumeran las principales recuentos de los diez recursos con el registro para cada recurso. Haga clic en el área total para ejecutar una búsqueda de registros de `AzureActivity | summarize AggregatedValue = count() by Resource`, que muestra todos los recursos de Azure disponibles para la solución. Haga clic en un recurso para ejecutar una consulta de registros que devuelva todos los registros de actividad para ese recurso. |
| Registros de actividad por proveedor de recursos | Muestra el número total de los proveedores de recursos que generan registros de actividad y enumera los diez principales. Haga clic en el área total para ejecutar una consulta de registro para `AzureActivity | summarize AggregatedValue = count() by ResourceProvider`, que muestra todos los proveedores de recursos de Azure. Haga clic en un proveedor de recursos para ejecutar una consulta de registros que devuelva todos los registros de actividad para el proveedor. |

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre la [registro de actividad](activity-logs-overview.md).
- Obtenga más información sobre la [plataforma de datos de Azure Monitor](data-platform.md).
- Use [registrar consultas](../log-query/log-query-overview.md) para ver información detallada del registro de actividad.
