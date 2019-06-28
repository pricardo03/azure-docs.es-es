---
title: Análisis de los datos de registro en Azure Monitor | Microsoft Docs
description: Requiere una consulta de registros para recuperar datos de registro desde Azure Monitor.  Este artículo describe cómo se usan las nuevas consultas de registros en Azure Monitor y proporciona los conceptos que debe comprender antes de crear una.
services: log-analytics
author: bwren
ms.service: log-analytics
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: bwren
ms.openlocfilehash: 1cb3946a93cbeff6a9b95e0a21edbf0523b53d5e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65203616"
---
# <a name="analyze-log-data-in-azure-monitor"></a>Análisis de datos de registro en Azure Monitor

Los datos de registro que recopila Azure Monitor se almacenan en un área de trabajo de Log Analytics, que se basa en el [Explorador de datos de Azure](/azure/data-explorer). Recopila datos de telemetría desde varios orígenes y usa el [lenguaje de consulta de Kusto](/azure/kusto/query) utilizado por Data Explorer para recuperar y analizar los datos.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="log-queries"></a>Consultas de registros

Requiere una consulta de registros para recuperar todos los datos de registro desde Azure Monitor.  Si está [analizando los datos en el portal](portals.md), [configurando una regla de alertas](../platform/alerts-metric.md) para recibir una notificación sobre una condición determinada o recuperando datos mediante la [API de registros de Azure Monitor](https://dev.loganalytics.io/), usará una consulta para especificar los datos que quiere.  En este artículo se describe cómo se usan las búsquedas de registros en Azure Monitor y se proporcionan los conceptos que debe comprender antes de crear una.

## <a name="where-log-queries-are-used"></a>Dónde se usan las consultas de registros

Entre las distintas formas en que usará las consultas en Azure Monitor se incluyen las siguientes:

- **Portal.** Puede realizar análisis interactivos de los datos de registro en [Azure Portal](portals.md).  Esto le permite modificar la consulta y analizar los resultados en una gran variedad de formatos y visualizaciones.  
- **Reglas de alertas.** [Las reglas de alertas](../platform/alerts-overview.md) identifican de manera proactiva los problemas de datos del área de trabajo.  Cada regla de alertas se basa en una búsqueda de registros que se ejecuta automáticamente a intervalos regulares.  Los resultados se inspeccionan para determinar si se debe crear una alerta.
- **Paneles.** Puede anclar los resultados de cualquier consulta en un [panel de Azure](../learn/tutorial-logs-dashboards.md), que le permitirá visualizar los datos de registros y métricas en conjunto y, opcionalmente, compartirlos con otros usuarios de Azure. 
- **Vistas.**  Puede crear visualizaciones de datos que se incluyan en los paneles de usuario con el [diseñador de vistas](../platform/view-designer.md).  Las consultas de registros proporcionan los datos utilizados por [iconos](../platform/view-designer-tiles.md) y [elementos de visualización](../platform/view-designer-parts.md) en cada vista.  

- **Exportación.**  Cuando importe datos de registro de Azure Monitor en Excel o [Power BI](../platform/powerbi.md), cree una consulta de registros para definir los datos que se van a exportar.
- **PowerShell.** Puede ejecutar un script de PowerShell desde una línea de comandos o un runbook de Azure Automation que use [Get-AzOperationalInsightsSearchResults](/powershell/module/az.operationalinsights/get-azoperationalinsightssearchresult) para recuperar los datos de registro de Azure Monitor.  Este cmdlet requiere una consulta para determinar los datos que se van a recuperar.
- **API de registros de Azure Monitor.**  La [API de registros de Azure Monitor](../platform/alerts-overview.md) permite que cualquier cliente de API REST recupere datos de registro del área de trabajo.  La solicitud de API incluye una consulta que se ejecuta en Azure Monitor para determinar los datos que se van a recuperar.

![Búsqueda de registros](media/log-query-overview/queries-overview.png)

## <a name="write-a-query"></a>Escriba una consulta.
Azure Monitor usa [una versión del lenguaje de consulta de Kusto](get-started-queries.md) para recuperar y analizar los datos de registro de varias formas.  Normalmente se empieza con consultas básicas para avanzar posteriormente a funciones más avanzadas a medida que sus requisitos se hacen más complejos.

La estructura básica de una consulta consiste en una tabla de origen seguida de una serie de operadores separados por un carácter de barra vertical `|`.  Puede encadenar varios operadores para refinar los datos y realizar funciones avanzadas.

Por ejemplo, suponga que desea buscar los diez equipos con más eventos de error del día anterior.

```Kusto
Event
| where (EventLevelName == "Error")
| where (TimeGenerated > ago(1days))
| summarize ErrorCount = count() by Computer
| top 10 by ErrorCount desc
```

O bien, quizá desee buscar los equipos que no han tenido un latido en el último día.

```Kusto
Heartbeat
| where TimeGenerated > ago(7d)
| summarize max(TimeGenerated) by Computer
| where max_TimeGenerated < ago(1d)  
```

¿Qué le parece un gráfico de líneas que muestre el uso del procesador por parte de cada equipo desde la semana pasada?

```Kusto
Perf
| where ObjectName == "Processor" and CounterName == "% Processor Time"
| where TimeGenerated  between (startofweek(ago(7d)) .. endofweek(ago(7d)) )
| summarize avg(CounterValue) by Computer, bin(TimeGenerated, 5min)
| render timechart    
```

En estos ejemplos rápidos se puede ver que, independientemente del tipo de datos con el que está trabajando, la estructura de la consulta es similar.  Puede dividirla en distintos pasos en los que los datos resultantes de un comando se envían a través de la canalización al siguiente comando.

También puede consultar datos de distintas áreas de trabajo de Log Analytics de la suscripción.

```Kusto
union Update, workspace("contoso-workspace").Update
| where TimeGenerated >= ago(1h)
| summarize dcount(Computer) by Classification 
```

## <a name="how-azure-monitor-log-data-is-organized"></a>Organización de los datos de registro de Azure Monitor
Cuando compila una consulta, primero debe determinar las tablas que tienen los datos que está buscando. Los diferentes tipos de datos se dividen en tablas dedicadas en cada [área de trabajo de Log Analytics](../learn/quick-create-workspace.md).  La documentación de los distintos orígenes de datos incluye el nombre del tipo de datos que crea y una descripción de cada una de sus propiedades.  Muchas consultas solo necesitan datos de una sola tabla, pero otras pueden utilizar una variedad de opciones para incluir datos de varias tablas.

Mientras que [Application Insights](../app/app-insights-overview.md) almacena los datos de la aplicación, como las solicitudes, las excepciones, los seguimientos y el uso en registros de Azure Monitor, estos datos se almacenan en una partición distinta de los otros datos del registro. Debe usar el mismo lenguaje de consulta para obtener acceso a estos datos, pero debe utilizar la [consola de Application Insights](../app/analytics.md) o la [API REST de Application Insights](https://dev.applicationinsights.io/) para acceder a ellos. Puede usar [consultas entre recursos](../log-query/cross-workspace-query.md) para combinar datos de Application Insights con otros datos de registro de Azure Monitor.


![Tablas](media/log-query-overview/queries-tables.png)




## <a name="next-steps"></a>Pasos siguientes
- Aprenda a usar [Log Analytics para crear y editar búsquedas de registros](../log-query/portals.md).
- Consulte un [tutorial sobre cómo escribir consultas](../log-query/get-started-queries.md) mediante el nuevo lenguaje de consulta.
