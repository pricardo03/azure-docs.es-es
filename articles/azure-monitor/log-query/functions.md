---
title: Funciones en las consultas de registros de Azure Monitor | Microsoft Docs
description: En este artículo se describe cómo utilizar funciones para llamar a una consulta desde otra consulta de registro de Azure Monitor.
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
ms.date: 11/15/2018
ms.author: bwren
ms.openlocfilehash: 6c6bd31961022957ec1a09fef6058ad32476e1c7
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "56005104"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Uso de funciones en consultas de registros de Azure Monitor

> [!NOTE]
> Debe completar la [Introducción al portal de Analytics](get-started-portal.md) y la [Introducción a las consultas en Log Analytics](get-started-queries.md) antes de completar esta lección.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]


Para usar una consulta de registro con otra consulta, puede guardarla como una función. Esto le permite simplificar las consultas complejas al dividirlas en partes y, además, le permite reutilizar código común con varias consultas.

## <a name="create-a-function"></a>Creación de una función

Para crear una función de Log Analytics en Azure Portal, haga clic en **Guardar** y, después, proporcione la información de la tabla siguiente.

| Configuración | DESCRIPCIÓN |
|:---|:---|
| NOMBRE           | Nombre para mostrar de la consulta en el **Explorador de consultas**. |
| Guardar como        | Función |
| Alias de función | Nombre corto para usar la función en otras consultas. No puede contener espacios y debe ser único. |
| Categoría       | Categoría para organizar las consultas y funciones guardadas en el **Explorador de consultas**. |

> [!NOTE]
> Una función de Azure Monitor no puede contener otra función.

> [!NOTE]
> Se puede guardar una función en consultas de registro de Azure Monitor, pero actualmente no se puede para consultas de Application Insights.



## <a name="use-a-function"></a>Uso de una función
Use una función al incluir su alias en otra consulta. Se puede usar como cualquier otra tabla.

## <a name="example"></a>Ejemplo
La siguiente consulta de ejemplo devuelve todas las actualizaciones de seguridad faltantes que se notificaron en el último día. Guarde esta consulta como función con el alias _security_updates_last_day_. 

```Kusto
Update
| where TimeGenerated > ago(1d) 
| where Classification == "Security Updates" 
| where UpdateState == "Needed"
```

Cree otra consulta y llame a la función _security_updates_last_day_ para buscar las actualizaciones de seguridad relacionadas con SQL necesarias.

```Kusto
security_updates_last_day | where Title contains "SQL"
```

## <a name="next-steps"></a>Pasos siguientes
Consulte otras lecciones para escribir consultas de registro de Azure Monitor:

- [Operaciones de cadena](string-operations.md)
- [Operaciones de fecha y hora](datetime-operations.md)
- [Funciones de agregación](aggregations.md)
- [Agregaciones avanzadas](advanced-aggregations.md)
- [JSON y estructuras de datos](json-data-structures.md)
- [Combinaciones](joins.md)
- [Gráficos](charts.md)
