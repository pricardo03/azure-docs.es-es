---
title: Creación de gráficos y diagramas de las consultas de Azure Log Analytics | Microsoft Docs
description: Describe las distintas visualizaciones en Azure Log Analytics para mostrar los datos de distintas maneras.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.openlocfilehash: 049e50f5800194c4126003f7e5ff7ae60b3de768
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53186232"
---
# <a name="creating-charts-and-diagrams-from-log-analytics-queries"></a>Creación de gráficos y diagramas de las consultas de Log Analytics

> [!NOTE]
> Debe completar la lección [Agregaciones avanzadas en consultas de Log Analytics](advanced-aggregations.md) antes de completar esta lección.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

En este artículo se escriben las distintas visualizaciones en Azure Log Analytics para mostrar los datos de distintas maneras.

## <a name="charting-the-results"></a>Crear gráficos de los resultados
Comience por revisar cuántos equipos hay por sistema operativo, durante la última hora:

```Kusto
Heartbeat
| where TimeGenerated > ago(1h)
| summarize count(Computer) by OSType  
```

De forma predeterminada, los resultados se muestran como una tabla:

![Tabla](media/charts/table-display.png)

Para obtener una mejor perspectiva, seleccione **Gráfico** y elija la opción **Circular** para visualizar los resultados:

![Gráfico circular](media/charts/charts-and-diagrams-pie.png)


## <a name="timecharts"></a>Gráficos de tiempo
Muestran los percentiles 50 y 95 promedio del tiempo de procesador en intervalos de 1 hora. La consulta genera varias series y puede seleccionar qué series mostrar en el gráfico de tiempo:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
```

Seleccione la opción de visualización de gráfico de **líneas**:

![Gráfico de líneas](media/charts/charts-and-diagrams-multiSeries.png)

### <a name="reference-line"></a>Línea de referencia

Una línea de referencia puede ayudarlo a identificar fácilmente si la métrica superó un umbral específico. Para agregar una línea a un gráfico, amplíe el conjunto de datos con una columna de constante:

```Kusto
Perf
| where TimeGenerated > ago(1d) 
| where CounterName == "% Processor Time" 
| summarize avg(CounterValue), percentiles(CounterValue, 50, 95)  by bin(TimeGenerated, 1h)
| extend Threshold = 20
```

![Línea de referencia](media/charts/charts-and-diagrams-multiSeriesThreshold.png)

## <a name="multiple-dimensions"></a>Varias dimensiones
Varias expresiones en la cláusula `by` de `summarize` crean varias filas en los resultados, una para cada combinación de valores.

```Kusto
SecurityEvent
| where TimeGenerated > ago(1d)
| summarize count() by tostring(EventID), AccountType, bin(TimeGenerated, 1h)
```

Cuando ve los resultados como un gráfico, usa la primera columna de la cláusula `by`. En el ejemplo siguiente se muestra un gráfico de columnas apiladas con el _EventID._ Las dimensiones deben ser del tipo `string`, por lo que en este ejemplo, _EventID_ se convierte en cadena. 

![EventID del gráfico de barras](media/charts/charts-and-diagrams-multiDimension1.png)

Para cambiar, seleccione la lista desplegable con el nombre de la columna. 

![AccountType del gráfico de barras](media/charts/charts-and-diagrams-multiDimension2.png)

## <a name="next-steps"></a>Pasos siguientes
Vea otras lecciones para usar el lenguaje de consulta de Log Analytics:

- [Operaciones de cadena](string-operations.md)
- [Operaciones de fecha y hora](datetime-operations.md)
- [Funciones de agregación](aggregations.md)
- [Agregaciones avanzadas](advanced-aggregations.md)
- [JSON y estructuras de datos](json-data-structures.md)
- [Escritura de consultas avanzadas](advanced-query-writing.md)
- [Combinaciones](joins.md)