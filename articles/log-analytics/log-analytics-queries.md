---
title: Análisis de los datos de Log Analytics en Azure Monitor | Microsoft Docs
description: Se requiere una búsqueda de registros para recuperar los datos de Log Analytics.  Este artículo describe cómo se usan las nuevas búsquedas de registros en Log Analytics y proporciona los conceptos que debe comprender antes de crear una.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/18/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 4aeb14bd7962205b89e1207e082f551999f54558
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "52634530"
---
# <a name="analyze-log-analytics-data-in-azure-monitor"></a>Análisis de los datos de Log Analytics en Azure Monitor

Los datos de registro que recopila Azure Monitor se almacenan en un área de trabajo de Log Analytics, que se basa en el [Explorador de datos de Azure](/azure/data-explorer). Recopila datos de telemetría desde varios orígenes y usa el [lenguaje de consulta del Explorador de datos](/azure/kusto/query) para recuperar y analizar los datos.

> [!NOTE]
> Log Analytics anteriormente se trató como un servicio de Azure. Ahora se considera parte de Azure Monitor y se centra en el almacenamiento y el análisis de datos de registro mediante su lenguaje de consulta. Las características que anteriormente se consideraban como una parte de Log Analytics, como los agentes de Windows y Linux para la recopilación de datos, las vistas para consultar los datos existentes y las alertas para avisarle proactivamente de problemas, no han cambiado, pero ahora se consideran parte de Azure Monitor.



## <a name="log-queries"></a>Consultas de registros

Requiere una consulta de registros para recuperar datos de Log Analytics.  Si está [analizando los datos en el portal](log-analytics-log-search-portals.md), [configurando una regla de alertas](../monitoring-and-diagnostics/alert-metric.md) para recibir una notificación sobre una condición determinada o recuperando datos mediante la [API de Log Analytics](https://dev.loganalytics.io/), usará una consulta para especificar los datos que quiere.  En este artículo se describe cómo se usan las búsquedas de registros en Log Analytics y se proporcionan los conceptos que debe comprender antes de crear una.



## <a name="where-log-queries-are-used"></a>Dónde se usan las consultas de registros

Entre las distintas formas en que usará las consultas en Log Analytics se incluyen las siguientes:

- **Portales.** Puede realizar análisis interactivos de los datos de registro en [Azure Portal](log-analytics-log-search-portals.md).  Esto le permite modificar la consulta y analizar los resultados en una gran variedad de formatos y visualizaciones.  
- **Reglas de alertas.** [Las reglas de alertas](../monitoring-and-diagnostics/monitoring-overview-alerts.md) identifican de manera proactiva los problemas de datos del área de trabajo.  Cada regla de alertas se basa en una búsqueda de registros que se ejecuta automáticamente a intervalos regulares.  Los resultados se inspeccionan para determinar si se debe crear una alerta.
- **Paneles.** Puede anclar los resultados de cualquier consulta en un [panel de Azure](../azure-monitor/platform/dashboards.md), que le permitirá visualizar los datos de registros y métricas en conjunto y, opcionalmente, compartirlos con otros usuarios de Azure. 
- **Vistas.**  Puede crear visualizaciones de datos que se incluyan en los paneles de usuario con el [diseñador de vistas](../azure-monitor/platform/view-designer.md).  Las consultas de registros proporcionan los datos utilizados por [iconos](../azure-monitor/platform/view-designer-tiles.md) y [elementos de visualización](../azure-monitor/platform/view-designer-parts.md) en cada vista.  
- **Exportación.**  Cuando importe datos desde el área de trabajo de Log Analytics a Excel o [Power BI](log-analytics-powerbi.md), cree una consulta de registros para definir los datos que se van a exportar.
- **PowerShell.** Puede ejecutar un script de PowerShell desde una línea de comandos o un runbook de Azure Automation que utilice el cmdlet [Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/get-azurermoperationalinsightssearchresults?view=azurermps-4.0.0) para recuperar los datos de Log Analytics.  Este cmdlet requiere una consulta para determinar los datos que se van a recuperar.
- **API de Log Analytics.**  La [API de búsqueda de registros de Log Analytics](../monitoring-and-diagnostics/monitoring-overview-alerts.md) permite que cualquier cliente de API REST recupere datos del área de trabajo.  La solicitud de API incluye una consulta que se ejecuta en Log Analytics para determinar los datos que se van a recuperar.

![Búsqueda de registros](media/log-analytics-queries/queries-overview.png)

## <a name="write-a-query"></a>Escriba una consulta.
Log Analytics usa [una versión del lenguaje de consulta del Explorador de datos](query-language/get-started-queries.md) para recuperar y analizar los datos de registro de varias formas.  Normalmente se empieza con consultas básicas para avanzar posteriormente a funciones más avanzadas a medida que sus requisitos se hacen más complejos.

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

## <a name="how-log-analytics-data-is-organized"></a>Organización de los datos de Log Analytics
Cuando compila una consulta, primero debe determinar las tablas que tienen los datos que está buscando. Los diferentes tipos de datos se dividen en tablas dedicadas en cada [área de trabajo de Log Analytics](log-analytics-quick-create-workspace.md).  La documentación de los distintos orígenes de datos incluye el nombre del tipo de datos que crea y una descripción de cada una de sus propiedades.  Muchas consultas solo necesitan datos de una sola tabla, pero otras pueden utilizar una variedad de opciones para incluir datos de varias tablas.

Mientras que [Application Insights](../application-insights/app-insights-overview.md) almacena los datos de la aplicación, como las solicitudes, las excepciones, los seguimientos y el uso de Log Analytics, estos datos se almacenan en una partición distinta de los otros datos del registro. Debe usar el mismo lenguaje de consulta para obtener acceso a estos datos, pero debe utilizar la [consola de Application Insights](../application-insights/app-insights-analytics.md) o la [API REST de Application Insights](https://dev.applicationinsights.io/) para acceder a ellos. Puede usar [consultas entre recursos](log-analytics-cross-workspace-search.md) para combinar datos de Application Insights con otros datos de Log Analytics.


![Tablas](media/log-analytics-queries/queries-tables.png)







## <a name="next-steps"></a>Pasos siguientes

- Obtenga información acerca de los [portales que puede utilizar para crear y editar búsquedas de registros](log-analytics-log-search-portals.md).
- Consulte un [tutorial sobre cómo escribir consultas](query-language/get-started-queries.md) mediante el nuevo lenguaje de consulta.
