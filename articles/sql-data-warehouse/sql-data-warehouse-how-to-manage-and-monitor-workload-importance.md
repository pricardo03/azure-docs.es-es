---
title: Administrar y supervisar la importancia de la carga de trabajo en Azure SQL Data Warehouse | Microsoft Docs
description: Aprenda a administrar y supervisar la importancia de nivel de solicitud.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: workload management
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: a39d71e0f8b8072cab6a3af9a2f0913973f303ee
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235922"
---
# <a name="manage-and-monitor-workload-importance-in-azure-sql-data-warehouse"></a>Administrar y supervisar la importancia de la carga de trabajo en Azure SQL Data Warehouse

Administrar y supervisar la importancia de nivel de solicitud en Azure SQL Data Warehouse mediante vistas de catálogo y DMV.

## <a name="monitor-importance"></a>Importancia del Monitor

Supervisar la importancia con la nueva columna de importancia en el [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql?view=azure-sqldw-latest) vista de administración dinámica.
La siguiente consulta se muestra en tiempo de envío y la hora de inicio para las consultas de supervisión. Revise la hora de envío y hora junto con importancia para ver cómo influye importancia la programación de inicio.

```sql
SELECT s.login_name, r.status, r.importance, r.submit_time, r.start_time
  FROM sys.dm_pdw_exec_sessions s
  JOIN sys.dm_pdw_exec_requests r ON s.session_id = r.session_id
  WHERE r.resource_class is not null
ORDER BY r.start_time
```

Para buscar más en cómo las consultas se están programación, utilice las vistas de catálogo.

## <a name="manage-importance-with-catalog-views"></a>Administrar la importancia con vistas de catálogo

La vista de catálogo sys.workload_management_workload_classifiers contiene información sobre los clasificadores en la instancia de Azure SQL Data Warehouse. Para excluir los clasificadores definidas por el sistema que se asignan a las clases de recursos ejecutan el siguiente código:

```sql
SELECT *
  FROM sys.workload_management_workload_classifiers
  WHERE classifier_id > 12
```

La vista de catálogo [sys.workload_management_workload_classifier_details](/sql/relational-databases/system-catalog-views/sys-workload-management-workload-classifier-details-transact-sql?view=azure-sqldw-latest), contiene información sobre los parámetros utilizados en la creación del clasificador.  La siguiente consulta muestra que se creó ExecReportsClassifier el ```membername``` parámetro para los valores con ExecutiveReports:

```sql
SELECT c.name,cd.classifier_type, classifier_value
  FROM sys.workload_management_workload_classifiers c
  JOIN sys.workload_management_workload_classifier_details cd
    ON cd.classifier_id = c.classifier_id
  WHERE c.name = 'ExecReportsClassifier'
```

![Resultados de la consulta](./media/sql-data-warehouse-how-to-manage-and-monitor-workload-importance/wlm-query-results.png)

Para simplificar la solución de problemas de clasificaciones incorrectas, se recomienda que quitar las asignaciones de rol de clase de recursos a medida que cree clasificadores de carga de trabajo. El código siguiente devuelve las pertenencias a roles de clase de recurso existente. Ejecute sp_droprolemember para cada ```membername``` devuelto desde la clase de recurso correspondiente.
A continuación es un ejemplo de la comprobación de existencia antes de quitar un clasificador de carga de trabajo:

```sql
IF EXISTS (SELECT 1 FROM sys.workload_management_workload_classifiers WHERE name = 'ExecReportsClassifier')
  DROP WORKLOAD CLASSIFIER ExecReportsClassifier;
GO
```

## <a name="next-steps"></a>Pasos siguientes
- Para obtener más información sobre la clasificación, vea [clasificación de la carga de trabajo](sql-data-warehouse-workload-classification.md).
- Para obtener más información sobre la importancia, consulte [importancia de la carga de trabajo](sql-data-warehouse-workload-importance.md)

> [!div class="nextstepaction"]
> [Ir a la configuración de la importancia de la carga de trabajo ](sql-data-warehouse-how-to-configure-workload-importance.md)
