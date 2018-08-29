---
title: Consultas avanzadas en Azure Log Analytics | Microsoft Docs
description: En este artículo se proporciona un tutorial de uso del portal de Analytics para escribir consultas en Log Analytics.
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
ms.openlocfilehash: 72c151fec0637822411f8cac44f4e13a8df96445
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "40191156"
---
# <a name="writing-advanced-queries-in-log-analytics"></a>Escribir consultas avanzadas en Log Analytics

> [!NOTE]
> Debe completar la [Introducción al portal de Analytics](get-started-analytics-portal.md) y la [Introducción a las consultas en Log Analytics](get-started-queries.md) antes de completar esta lección.

## <a name="reusing-code-with-let"></a>Reutilización de código con let
Use el elemento `let` para asignar los resultados a una variable y hacer referencia a ella más adelante en la consulta:

```OQL
// get all events that have level 2 (indicates warning level)
let warning_events=
Event
| where EventLevel == 2;
// count the number of warning events per computer
warning_events
| summarize count() by Computer 
```

También puede asignar valores constantes a variables. Esto es compatible con un método para configurar los parámetros para los campos que necesite cambiar cada vez que ejecute la consulta. Modifique los parámetros según sea necesario. Por ejemplo, para calcular el espacio libre en disco y la memoria libre (en percentiles), en un período de tiempo determinado:

```OQL
let startDate = datetime(2018-08-01T12:55:02);
let endDate = datetime(2018-08-02T13:21:35);
let FreeDiskSpace =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Logical Disk" and CounterName=="Free Megabytes"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
let FreeMemory =
Perf
| where TimeGenerated between (startDate .. endDate)
| where ObjectName=="Memory" and CounterName=="Available MBytes Memory"
| summarize percentiles(CounterValue, 50, 75, 90, 99);
union FreeDiskSpace, FreeMemory
```

Facilita el cambio de la hora de inicio y finalización la próxima vez que ejecute la consulta.

### <a name="local-functions-and-parameters"></a>Funciones y parámetros locales
Use instrucciones `let` para crear funciones que puedan usarse en la misma consulta. Por ejemplo, defina una función que tome un campo de fecha y hora (en formato UTC) y lo convierta en un formato estándar de Estados Unidos. 

```OQL
let utc_to_us_date_format = (t:datetime)
{
  strcat(getmonth(t), "/", dayofmonth(t),"/", getyear(t), " ",
  bin((t-1h)%12h+1h,1s), iff(t%24h<12h, "AM", "PM"))
};
Event 
| where TimeGenerated > ago(1h) 
| extend USTimeGenerated = utc_to_us_date_format(TimeGenerated)
| project TimeGenerated, USTimeGenerated, Source, Computer, EventLevel, EventData 
```

## <a name="functions"></a>Functions
Puede guardar una consulta con un alias de función, de modo que otras consultas puedan hacerle referencia. Por ejemplo, la siguiente consulta estándar devuelve todas las actualizaciones de seguridad que faltan notificadas en el último día:

```OQL
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

Puede guardar esta consulta como función y asignarle un alias como _security_updates_last_day_. A continuación, puede usarla en otra consulta para buscar las actualizaciones de seguridad necesarias relacionadas con SQL:

```OQL
security_updates_last_day | where Title contains "SQL"
```

Para guardar una consulta como función, seleccione el botón **Guardar** en el portal y cambie **Guardar como** a _Función_. El alias de la función puede contener letras, dígitos o caracteres de subrayado, pero debe empezar por una letra o un carácter de subrayado.

> [!NOTE]
> Se puede guardar una función en consultas de Log Analytics, pero actualmente no se puede para consultas de Application Insights.


## <a name="print"></a>Print
La función `print` devolverá una tabla con una sola columna y una sola fila, que muestra el resultado de un cálculo. Se suele usar en los casos en los que es necesario un cálculo simple. Por ejemplo, para buscar la hora actual en la hora estándar del Pacífico y agregar una columna con EST:

```OQL
print nowPst = now()-8h
| extend nowEst = nowPst+3h
```

## <a name="datatable"></a>Datatable
La función `datatable` permite definir un conjunto de datos. Se proporciona un esquema y un conjunto de valores y, a continuación, se canaliza la tabla a cualquier otro elemento de consulta. Por ejemplo, para crear una tabla de utilización de RAM y calcular su valor promedio por hora:

```OQL
datatable (TimeGenerated: datetime, usage_percent: double)
[
  "2018-06-02T15:15:46.3418323Z", 15.5,
  "2018-06-02T15:45:43.1561235Z", 20.2,
  "2018-06-02T16:16:49.2354895Z", 17.3,
  "2018-06-02T16:46:44.9813459Z", 45.7,
  "2018-06-02T17:15:41.7895423Z", 10.9,
  "2018-06-02T17:44:23.9813459Z", 24.7,
  "2018-06-02T18:14:59.7283023Z", 22.3,
  "2018-06-02T18:45:12.1895483Z", 25.4
]
| summarize avg(usage_percent) by bin(TimeGenerated, 1h)
```

Las construcciones de Datatable también son muy útiles al crear una tabla de búsqueda. Por ejemplo, para asignar datos de tabla, como los id. de eventos, de la tabla _SecurityEvent_ a tipos de evento enumerados en otro lugar, cree una tabla de búsqueda con los tipos de eventos mediante la función `datatable` y combine esta datatable con los datos de  _SecurityEvent_:

```OQL
let eventCodes = datatable (EventID: int, EventType:string)
[
    4625, "Account activity",
    4688, "Process action",
    4634, "Account activity",
    4672, "Access",
    4624, "Account activity",
    4799, "Access management",
    4798, "Access management",
    5059, "Key operation",
    4648, "A logon was attempted using explicit credentials",
    4768, "Access management",
    4662, "Other",
    8002, "Process action",
    4904, "Security event management",
    4905, "Security event management",
];
SecurityEvent
| where TimeGenerated > ago(1h) 
| join kind=leftouter (
  eventCodes
) on EventID
| project TimeGenerated, Account, AccountType, Computer, EventType
```

## <a name="next-steps"></a>Pasos siguientes
Vea otras lecciones para usar el lenguaje de consulta de Log Analytics:

- [Operaciones de cadena](string-operations.md)
- [Operaciones de fecha y hora](datetime-operations.md)
- [Funciones de agregación](aggregations.md)
- [Agregaciones avanzadas](advanced-aggregations.md)
- [JSON y estructuras de datos](json-data-structures.md)
- [Combinaciones](joins.md)
- [Gráficos](charts.md)