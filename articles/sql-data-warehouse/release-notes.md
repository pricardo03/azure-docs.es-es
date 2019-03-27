---
title: Notas de la versión de Azure SQL Data Warehouse | Microsoft Docs
description: Notas de la versión de Azure SQL Data Warehouse.
services: sql-data-warehouse
author: mlee3gsd
ms.author: anumjs
ms.reviewer: jrasnick
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 02/09/2019
ms.openlocfilehash: e77556ac0d6f64797906c0f3b4181f147b1668e2
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448423"
---
# <a name="azure-sql-data-warehouse-release-notes-and-documentation-updates"></a>Notas de la versión y actualizaciones de documentación de Azure SQL Data Warehouse

Azure SQL Data Warehouse (SQL DW) es un almacenamiento de datos empresarial en la nube que aprovecha el procesamiento paralelo masivo (MPP) para ejecutar rápidamente consultas complejas en petabytes de datos. SQL Data Warehouse se usa como un componente clave de una solución de macrodatos. Importe macrodatos en SQL Data Warehouse con consultas T-SQL de PolyBase simples y, después, use la potencia de MPP para realizar análisis de alto rendimiento. Al realizar la integración y el análisis, el almacenamiento de datos pasará a ser la versión única de certeza con la que puede contar su empresa para obtener información.

Haga clic en los siguientes vínculos para más información sobre las nuevas características y mejoras que puede esperar en la versión más reciente de Azure SQL Data Warehouse. Es probable que reciba estas actualizaciones de servicio durante su programación de mantenimiento identificada.

- [Marzo de 2019](./release-notes-10-0-10106-0.md#march-2019)
- [Enero de 2019](./release-notes-10-0-10106-0.md#january-2019)
- [Diciembre de 2018](./release-notes-10-0-10106-0.md#december-2018)
- [Octubre de 2018](./release-notes-10-0-10106-0.md#october-2018)
- [Septiembre de 2018](./release-notes-september-2018.md)
- [Agosto de 2018](./release-notes-august-2018.md)
- [Julio de 2018](./release-notes-july-2018.md)
- [Junio de 2018](./release-notes-june-2018.md)
- [Mayo de 2018](./release-notes-may-2018.md)

## <a name="checking-the-code-version-that-has-been-applied-to-your-data-warehouse"></a>Comprobación de la versión de código que se ha aplicado al almacenamiento de datos

Para confirmar qué versión se ha aplicado a su almacén de datos, conéctese a su almacén de datos mediante SSMS y ejecute la siguiente sintaxis para devolver la versión actual de SQL Data Warehouse.

```sql
SELECT @@VERSION AS 'SQL Data Warehouse';
```

Salida de ejemplo: ![Versión de SQL Data Warehouse](./media/release-notes/sql_data_warehouse_version.png)

Use la fecha identificada para confirmar qué versión se ha aplicado a su instancia de Azure SQL Data Warehouse. 


## <a name="next-steps"></a>Pasos siguientes
- [Obtenga más información](https://docs.microsoft.com/azure/sql-data-warehouse/viewing-maintenance-schedule) sobre cómo ver una programación de mantenimiento. 
- [Obtenga más información](https://docs.microsoft.com/azure/sql-data-warehouse/changing-maintenance-schedule) sobre cómo cambiar una programación de mantenimiento.
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/alert-metric) sobre cómo crear, ver y administrar alertas mediante Azure Monitor.
- [Obtenga más información](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log-webhook) sobre las acciones del webhook para las reglas de alerta de registro.
- [Obtenga más información](https://docs.microsoft.com/azure/service-health/service-health-overview) acerca de Azure Service Health