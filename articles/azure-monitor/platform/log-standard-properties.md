---
title: Propiedades estándar de los registros de Azure Monitor | Microsoft Docs
description: Describe las propiedades comunes para varios tipos de datos de los registros de Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/20/2019
ms.author: bwren
ms.openlocfilehash: c01cdb967fd7f9516b4403aa4f0c76f2577d5050
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60394530"
---
# <a name="standard-properties-in-azure-monitor-log-records"></a>Propiedades estándar de los registros de Azure Monitor
Los datos de los registros de Azure Monitor [se almacenan como un conjunto de registros](../log-query/log-query-overview.md), cada uno con un tipo de datos determinado que tiene un único conjunto de propiedades. Muchos tipos de datos tendrán propiedades estándar que son comunes en varios tipos. En este artículo se describen estas propiedades y se proporcionan ejemplos de cómo puede usarlas en las consultas.

Algunas de estas propiedades aún se encuentran en proceso de implementación, por lo que puede que las vea en algunos tipos de datos, pero aún no en otros.

## <a name="timegenerated"></a>TimeGenerated
La propiedad **TimeGenerated** contiene la fecha y hora en la que se creó el registro. Proporciona una propiedad común que se utiliza para filtrar o resumir por hora. Cuando se selecciona un intervalo de tiempo para una vista o panel en Azure Portal, se utiliza TimeGenerated para filtrar los resultados.

### <a name="examples"></a>Ejemplos

La consulta siguiente devuelve el número de eventos de error creados para cada día de la semana anterior.

```Kusto
Event
| where EventLevelName == "Error" 
| where TimeGenerated between(startofweek(ago(7days))..endofweek(ago(7days))) 
| summarize count() by bin(TimeGenerated, 1day) 
| sort by TimeGenerated asc 
```

## <a name="type"></a>Type
La propiedad **Type** contiene el nombre de la tabla de la que se recuperó el registro, de la que también se puede pensar que es el tipo de registro. Esta propiedad es útil en las consultas que combinan registros de varias tablas, como las que utilizan el operador `search`, para distinguir entre registros de diferentes tipos. **$table** puede utilizarse en lugar de **Type** en algunos lugares.

### <a name="examples"></a>Ejemplos
La siguiente consulta devuelve el número de registros por tipo recopilados durante la última hora.

```Kusto
search * 
| where TimeGenerated > ago(1h) 
| summarize count() by Type 
```

## <a name="resourceid"></a>\_ResourceId
La propiedad **\_ResourceId** contiene un identificador único para el recurso con el que está asociado el registro. Esto le ofrece una propiedad estándar para usarla para definir el ámbito de la consulta a solo los registros de un recurso determinado, o para unir datos relacionados en varias tablas.

En el caso de los recursos de Azure, el valor de **_ResourceId** es la [URL de id. de recurso de Azure](../../azure-resource-manager/resource-group-template-functions-resource.md). La propiedad está actualmente limitada a los recursos de Azure, pero se extenderá a los recursos de fuera de Azure, por ejemplo, en equipos locales.

> [!NOTE]
> Algunos tipos de datos ya tienen campos que contienen el identificador de recurso de Azure o al menos partes de él como el identificador de suscripción. Aunque estos campos se guardan por compatibilidad con versiones anteriores, se recomienda utilizar _ResourceId para realizar la correlación cruzada, ya que será más coherente.

### <a name="examples"></a>Ejemplos
La consulta siguiente combina datos de rendimiento y de eventos para cada equipo. Muestra todos los eventos con el identificador _101_ y un uso del procesador superior al 50 %.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

La consulta siguiente combina los registros _AzureActivity_ con los de _SecurityEvent_. Muestra todas las operaciones de actividad con los usuarios que iniciaron sesión en estas máquinas.

```Kusto
AzureActivity 
| where  
    OperationName in ("Restart Virtual Machine", "Create or Update Virtual Machine", "Delete Virtual Machine")  
    and ActivityStatus == "Succeeded"  
| join kind= leftouter (    
   SecurityEvent 
   | where EventID == 4624  
   | summarize LoggedOnAccounts = makeset(Account) by _ResourceId 
) on _ResourceId  
```

La siguiente consulta se analiza **_ResourceId** y agregados facturan los volúmenes de datos por suscripción de Azure.

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| parse tolower(_ResourceId) with "/subscriptions/" subscriptionId "/resourcegroups/" 
    resourceGroup "/providers/" provider "/" resourceType "/" resourceName   
| summarize Bytes=sum(_BilledSize) by subscriptionId | sort by Bytes nulls last 
```

Use estas consultas `union withsource = tt *` con moderación, ya que la ejecución de exámenes entre tipos de datos es costosa.

## <a name="isbillable"></a>\_IsBillable
La propiedad **\_IsBillable** especifica si los datos ingeridos son facturables. Los datos con **\_IsBillable** igual a _false_ se recopilan de forma gratuita y no se facturan en su cuenta de Azure.

### <a name="examples"></a>Ejemplos
Para obtener una lista de equipos que envían los tipos de datos de facturación, use la siguiente consulta:

> [!NOTE]
> Use las consultas con `union withsource = tt *` con moderación, ya que la ejecución de exámenes entre tipos de datos es costosa. 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize TotalVolumeBytes=sum(_BilledSize) by computerName
```

Dichas consultas pueden ampliarse para devolver el número de equipos por hora que están enviando tipos de datos facturados:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| extend computerName = tolower(tostring(split(Computer, '.')[0]))
| where computerName != ""
| summarize dcount(computerName) by bin(TimeGenerated, 1h) | sort by TimeGenerated asc
```

## <a name="billedsize"></a>\_BilledSize
La propiedad **\_BilledSize** especifica el tamaño en bytes de los datos que se facturarán a su cuenta de Azure si **\_IsBillable** es true.

### <a name="examples"></a>Ejemplos
Para ver el tamaño de los eventos facturables ingeridos por equipo, use la propiedad `_BilledSize`, que proporciona el tamaño en bytes:

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize Bytes=sum(_BilledSize) by  Computer | sort by Bytes nulls last 
```

Para ver el recuento de eventos ingeridos por equipo, use la consulta siguiente:

```Kusto
union withsource = tt *
| summarize count() by Computer | sort by count_ nulls last
```

Para ver el recuento de eventos facturables ingeridos por equipo, use la consulta siguiente: 

```Kusto
union withsource = tt * 
| where _IsBillable == true 
| summarize count() by Computer  | sort by count_ nulls last
```

Si quiere ver recuentos de tipos de datos facturables que envían datos a un equipo específico, use la consulta siguiente:

```Kusto
union withsource = tt *
| where Computer == "computer name"
| where _IsBillable == true 
| summarize count() by tt | sort by count_ nulls last 
```


## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información sobre cómo se almacenan los [datos del registro de Azure Monitor](../log-query/log-query-overview.md).
- Reciba una lección sobre [escritura de consultas de registro](../../azure-monitor/log-query/get-started-queries.md).
- Reciba una lección sobre [la unión de tablas en consultas de registro](../../azure-monitor/log-query/joins.md).
