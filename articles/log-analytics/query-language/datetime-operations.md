---
title: Trabajar con valores de fecha y hora en consultas de Azure Log Analytics | Microsoft Docs
description: Describe cómo trabajar con datos de fecha y hora en consultas de Log Analytics.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/16/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 3a0e2b78de8cea3929ac457bab3d5e07a2b85401
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603386"
---
# <a name="working-with-date-time-values-in-log-analytics-queries"></a>Trabajar con valores de fecha y hora en consultas de Log Analytics

> [!NOTE]
> Debe completar la [Introducción al portal de Analytics](get-started-analytics-portal.md) y la [Introducción a las consultas en Log Analytics](get-started-queries.md) antes de completar esta lección.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

En este artículo se describe cómo trabajar con datos de fecha y hora en consultas de Log Analytics.


## <a name="date-time-basics"></a>Conceptos básicos de fecha y hora
El lenguaje de consulta de Log Analytics tiene dos tipos de datos principales asociados con las fechas y horas: datetime y timespan. Todas las fechas se expresan en formato UTC. Aunque se admiten varios formatos de datetime, se prefiere el formato ISO8601. 

Los valores timespan se expresan como un decimal seguido de una unidad de tiempo:

|Abreviatura   | Unidad de tiempo    |
|:---|:---|
|d           | day          |
|h           | hour         |
|m           | minuto       |
|s           | segundo       |
|ms          | milisegundo  |
|microsegundo | microsegundo  |
|tic        | nanosegundo   |

Los valores datetime se pueden crear mediante la conversión de una cadena con el operador `todatetime`. Por ejemplo, para revisar los latidos de máquina virtual enviados en un período de tiempo específico, puede usar el [operador between](https://docs.loganalytics.io/docs/Language-Reference/Scalar-operators/between-operator) que es útil para especificar un intervalo de tiempo.

```KQL
Heartbeat
| where TimeGenerated between(datetime("2018-06-30 22:46:42") .. datetime("2018-07-01 00:57:27"))
```

Otro escenario común es comparar un valor datetime hasta el presente. Por ejemplo, para ver todos los latidos durante los últimos dos minutos, puede usar el operador `now` junto con un valor timespan que represente dos minutos:

```KQL
Heartbeat
| where TimeGenerated > now() - 2m
```

También está disponible un acceso directo para esta función:
```KQL
Heartbeat
| where TimeGenerated > now(-2m)
```

Sin embargo, el método más corto y legible es usar el operador `ago`:
```KQL
Heartbeat
| where TimeGenerated > ago(2m)
```

Supongamos que en lugar de saber la hora de inicio y finalización, conoce la hora de inicio y la duración. Puede volver a escribir la consulta como sigue:

```KQL
let startDatetime = todatetime("2018-06-30 20:12:42.9");
let duration = totimespan(25m);
Heartbeat
| where TimeGenerated between(startDatetime .. (startDatetime+duration) )
| extend timeFromStart = TimeGenerated - startDatetime
```

## <a name="converting-time-units"></a>Conversión de unidades de tiempo
Puede ser útil expresar un valor datetime o timespan en una unidad de tiempo distinta de la predeterminada. Por ejemplo, supongamos que está revisando los eventos de error de los últimos 30 minutos y necesita una columna calculada que muestre cuánto tiempo hace que se produjo el evento:

```KQL
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated 
```

Puede ver que la columna _timeAgo_ contiene valores, como: "00:09:31.5118992", lo que significa que tienen el formato hh:mm:ss.fffffff. Si desea dar formato a estos valores en _numver_ de minutos desde la hora de inicio, solo tiene que dividir ese valor por "1 minuto":

```KQL
Event
| where TimeGenerated > ago(30m)
| where EventLevelName == "Error"
| extend timeAgo = now() - TimeGenerated
| extend timeAgoMinutes = timeAgo/1m 
```


## <a name="aggregations-and-bucketing-by-time-intervals"></a>Agregaciones y creación de cubos por intervalos de tiempo
Otro escenario muy común es la necesidad de obtener estadísticas de un período de tiempo determinado en un intervalo de agregación determinado. Para ello, se puede usar un operador `bin` como parte de una cláusula summarize.

Use la siguiente consulta para obtener el número de eventos que se produjeron cada 5 minutos durante la última media hora:

```KQL
Event
| where TimeGenerated > ago(30m)
| summarize events_count=count() by bin(TimeGenerated, 5m) 
```

Esto genera la tabla siguiente:  
|TimeGenerated(UTC)|events_count|
|--|--|
|2018-08-01T09:30:00.000|54|
|2018-08-01T09:35:00.000|41|
|2018-08-01T09:40:00.000|42|
|2018-08-01T09:45:00.000|41|
|2018-08-01T09:50:00.000|41|
|2018-08-01T09:55:00.000|16|

Otra forma de crear cubos de resultados es usar funciones, como `startofday`:

```KQL
Event
| where TimeGenerated > ago(4d)
| summarize events_count=count() by startofday(TimeGenerated) 
```

Esto genera los resultados siguientes:

|timestamp|count_|
|--|--|
|2018-07-28T00:00:00.000|7136|
|2018-07-29T00:00:00.000|12 315|
|2018-07-30T00:00:00.000|16 847|
|2018-07-31T00:00:00.000|12 616|
|2018-08-01T00:00:00.000|5416  |


## <a name="time-zones"></a>Zonas horarias
Puesto que todos los valores datetime se expresan en formato UTC, a menudo resulta útil convertirlos al formato de la zona horaria local. Por ejemplo, use este cálculo para convertir la hora UTC a la hora estándar del Pacífico:

```KQL
Event
| extend localTimestamp = TimeGenerated - 8h
```

## <a name="related-functions"></a>Funciones relacionadas

| Categoría | Función |
|:---|:---|
| Convertir tipos de datos | [todatetime](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/todatetime())  [totimespan](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/totimespan())  |
| Redondear valor al tamaño de bin | [bin](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/bin()) |
| Obtener una fecha u hora especificas | [ago](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/ago()) [now](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/now())   |
| Obtener parte del valor | [datetime_part](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/datetime_part()) [getmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/getmonth()) [monthofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/monthofyear()) [getyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/getyear()) [dayofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofmonth()) [dayofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofweek()) [dayofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/dayofyear()) [weekofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/weekofyear()) |
| Obtener una fecha relativa al valor  | [endofday](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofday()) [endofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofweek()) [endofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofmonth()) [endofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/endofyear()) [startofday](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofday()) [startofweek](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofweek()) [startofmonth](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofmonth()) [startofyear](https://docs.loganalytics.io/docs/Language-Reference/Scalar-functions/startofyear()) |

## <a name="next-steps"></a>Pasos siguientes
Vea otras lecciones para usar el lenguaje de consulta de Log Analytics:

- [Operaciones de cadena](string-operations.md)
- [Funciones de agregación](aggregations.md)
- [Agregaciones avanzadas](advanced-aggregations.md)
- [JSON y estructuras de datos](json-data-structures.md)
- [Escritura de consultas avanzadas](advanced-query-writing.md)
- [Combinaciones](joins.md)
- [Gráficos](charts.md)