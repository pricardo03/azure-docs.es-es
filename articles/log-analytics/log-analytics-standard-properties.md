---
title: Propiedades estándar de los registros de Log Analytics de Azure Monitor | Microsoft Docs
description: Describe las propiedades comunes para varios tipos de datos de Log Analytics de Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2018
ms.author: bwren
ms.component: na
ms.openlocfilehash: 663f0b04c528c180e4130c1c157441cbc0ceb98b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955874"
---
# <a name="standard-properties-in-log-analytics-records"></a>Propiedades estándar de los registros de Log Analytics
Los datos de [Log Analytics](../log-analytics/log-analytics-queries.md) se almacenan como un conjunto de registros, cada uno con un tipo de datos determinado que tiene un único conjunto de propiedades. Muchos tipos de datos tendrán propiedades estándar que son comunes en varios tipos. En este artículo se describen estas propiedades y se proporcionan ejemplos de cómo puede usarlas en las consultas.

Algunas de estas propiedades aún se encuentran en proceso de implementación, por lo que puede que las vea en algunos tipos de datos, pero aún no en otros.


## <a name="resourceid"></a>_ResourceId
La propiedad **_ResourceId** contiene un identificador único para el recurso con el que está asociado el registro. Esto le ofrece una propiedad estándar para usarla para definir el ámbito de la consulta a solo los registros de un recurso determinado, o para unir datos relacionados en varias tablas.

En el caso de los recursos de Azure, el valor de **_ResourceId** es la [URL de id. de recurso de Azure](../azure-resource-manager/resource-group-template-functions-resource.md). La propiedad está actualmente limitada a los recursos de Azure, pero se extenderá a los recursos de fuera de Azure, por ejemplo, en equipos locales. 

### <a name="examples"></a>Ejemplos
En el ejemplo siguiente se muestra una consulta que combina datos de rendimiento y de eventos para cada equipo. Muestra todos los eventos con el identificador _101_ y un uso del procesador superior al 50 %.

```Kusto
Perf 
| where CounterName == "% User Time" and CounterValue  > 50 and _ResourceId != "" 
| join kind=inner (     
    Event 
    | where EventID == 101 
) on _ResourceId
```

En el ejemplo siguiente se muestra una consulta que une los registros _AzureActivity_ con los de _SecurityEvent_. Muestra todas las operaciones de actividad con los usuarios que iniciaron sesión en estas máquinas.

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

## <a name="next-steps"></a>Pasos siguientes

- Obtenga más información acerca de cómo [se almacenan los datos de Log Analytics](../log-analytics/log-analytics-queries.md).
- Reciba una lección sobre [la escritura de consultas en Log Analytics](../log-analytics/query-language/get-started-queries.md).
- Reciba una lección sobre [la unión de tablas en consultas en Log Analytics](../log-analytics/query-language/joins.md).