---
title: Funciones en las consultas de registros de Azure Monitor | Microsoft Docs
description: En este artículo se describe cómo utilizar funciones para llamar a una consulta desde otra consulta de registro de Azure Monitor.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/15/2018
ms.openlocfilehash: 8d8473b88327d3d17346a0351d0a9fc510152cd8
ms.sourcegitcommit: 5acd8f33a5adce3f5ded20dff2a7a48a07be8672
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2019
ms.locfileid: "72894186"
---
# <a name="using-functions-in-azure-monitor-log-queries"></a>Uso de funciones en consultas de registros de Azure Monitor

Para usar una consulta de registro con otra consulta, puede guardarla como una función. Esto le permite simplificar las consultas complejas al dividirlas en partes y, además, le permite reutilizar código común con varias consultas.

## <a name="create-a-function"></a>Creación de una función

Para crear una función con Log Analytics en Azure Portal, haga clic en **Guardar** y, después, proporcione la información de la tabla siguiente.

| Configuración | DESCRIPCIÓN |
|:---|:---|
| NOMBRE           | Nombre para mostrar de la consulta en el **Explorador de consultas**. |
| Guardar como        | Función |
| Alias de función | Nombre corto para usar la función en otras consultas. No puede contener espacios y debe ser único. |
| Category       | Categoría para organizar las consultas y funciones guardadas en el **Explorador de consultas**. |

> [!NOTE]
> Una función de Azure Monitor no puede contener otra función.




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
