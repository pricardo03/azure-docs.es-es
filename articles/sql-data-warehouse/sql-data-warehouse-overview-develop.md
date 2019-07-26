---
title: Recursos para desarrollar un almacén de datos en Azure | Microsoft Docs
description: Conceptos de desarrollo, decisiones de diseño, recomendaciones y técnicas de codificación para SQL Data Warehouse.
services: sql-data-warehouse
author: XiaoyuMSFT
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: development
ms.date: 08/29/2018
ms.author: xiaoyul
ms.reviewer: igorstan
ms.openlocfilehash: a78d78618a4cd9bf8d2aaebbd0c0da13697549bc
ms.sourcegitcommit: 75a56915dce1c538dc7a921beb4a5305e79d3c7a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/24/2019
ms.locfileid: "68479473"
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Decisiones de diseño y técnicas de codificación para SQL Data Warehouse
Eche un vistazo a estos artículos de desarrollo para comprender mejor las decisiones de diseño clave, las recomendaciones y las técnicas de codificación para SQL Data Warehouse.

## <a name="key-design-decisions"></a>Decisiones de diseño clave
En los siguientes artículos se resaltan los conceptos y las decisiones de diseño para desarrollar un almacén de datos distribuido mediante SQL Data Warehouse:

* [connections][connections]
* [simultaneidad][concurrency]
* [transacciones][transactions]
* [esquemas definidos por el usuario][user-defined schemas]
* [Distribución de tablas][table distribution]
* [índices de tablas][table indexes]
* [particiones de tablas][table partitions]
* [CTAS][CTAS]
* [estadísticas][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Recomendaciones de desarrollo y técnicas de codificación
En estos artículos se abordan técnicas de codificación, sugerencias y recomendaciones específicas para el desarrollo de SQL Data Warehouse:

* [procedimientos almacenados][stored procedures]
* [etiquetas][labels]
* [vistas][views]
* [tablas temporales][temporary tables]
* [SQL dinámico][dynamic SQL]
* [bucle][looping]
* [opciones de agrupar por][group by options]
* [asignación de variables][variable assignment]

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información de referencia, consulte las [instrucciones de T-SQL de SQL Data Warehouse](sql-data-warehouse-reference-tsql-statements.md).

<!--Image references-->

<!--Article references-->
[concurrency]: ./resource-classes-for-workload-management.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md


<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
