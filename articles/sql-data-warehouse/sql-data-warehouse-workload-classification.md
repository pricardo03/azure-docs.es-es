---
title: Clasificación de almacenamiento de datos SQL | Microsoft Docs
description: Instrucciones para usar la clasificación para administrar la simultaneidad, importancia y recursos de proceso para las consultas en Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 888a64de29178834fc47199a033eb6bc62858e57
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617757"
---
# <a name="sql-data-warehouse-workload-classification-preview"></a>Clasificación de carga de trabajo de SQL Data Warehouse (versión preliminar)

En este artículo se explica el proceso de clasificación de la carga de trabajo de SQL Data Warehouse de asignación de una clase de recursos y la importancia a las solicitudes entrantes.

> [!Note]
> Clasificación de la carga de trabajo está disponible para vista previa en SQL Data Warehouse Gen2. Clasificación de la administración de cargas de trabajo y la importancia de vista previa es para las compilaciones con una fecha de lanzamiento de 9 de abril de 2019 o posterior.  Los usuarios deben evitar usar compilaciones anteriores a esta fecha para pruebas de carga de trabajo administración.  Para determinar si la compilación es capaz de administración de cargas de trabajo, ejecutar select @@version cuando se conecta a la instancia de SQL Data Warehouse.

## <a name="classification"></a>clasificación

> [!Video https://www.youtube.com/embed/QcCRBAhoXpM]

Clasificación de la administración de cargas de trabajo permite que las directivas de carga de trabajo que se aplicará a las solicitudes mediante la asignación de [las clases de recursos](resource-classes-for-workload-management.md#what-are-resource-classes) y [importancia](sql-data-warehouse-workload-importance.md).

Aunque hay muchas maneras de clasificar las cargas de trabajo de almacenamiento de datos, la clasificación más sencilla y común es la carga y la consulta. Cargar datos con insert, update y las instrucciones delete.  Consultar los datos mediante instrucciones SELECT. Una solución de almacenamiento de datos a menudo tendrá una directiva de carga de trabajo para la actividad de carga, como la asignación de una clase de recurso superior con más recursos. Podría aplicar una directiva de carga de trabajo diferentes a las consultas, como la importancia menor en comparación con las actividades de carga.

También puede subclassify las cargas de trabajo de carga y la consulta. Subclasificación ofrece un mayor control de las cargas de trabajo. Por ejemplo, las cargas de trabajo de consulta pueden constar de las actualizaciones de cubos, las consultas del panel o consultas ad hoc. Puede clasificar cada una de estas cargas de trabajo de consulta con diferentes clases de recursos o la configuración de importancia. Carga también puede beneficiarse de subclasificación. Transformaciones de gran tamaño pueden asignarse a las clases de recursos más grandes. Mayor importancia se puede usar para asegurarse de que los datos de ventas claves están una fuente de datos sociales o de cargador antes de los datos meteorológicos.

No todas las instrucciones se clasifican como no se requieren recursos ni necesita importancia para influir en ejecución.  Comandos DBCC, las instrucciones BEGIN, COMMIT y ROLLBACK TRANSACTION no se clasifican.

## <a name="classification-process"></a>Proceso de clasificación

La clasificación en SQL Data Warehouse se logra hoy mismo mediante la asignación de usuarios a un rol que tiene una clase de recurso correspondiente asignada a ella mediante [sp_addrolemember](/sql/relational-databases/system-stored-procedures/sp-addrolemember-transact-sql). La capacidad para caracterizar las convocatorias que superen un inicio de sesión a una clase de recurso está limitada con esta funcionalidad. Ahora está disponible con un método más completo para la clasificación del [crear CLASIFICADOR de carga de trabajo](/sql/t-sql/statements/create-workload-classifier-transact-sql) sintaxis.  Con esta sintaxis, los usuarios de SQL Data Warehouse pueden asignar una clase de recursos y de importancia a las solicitudes.  

> [!NOTE]
> Clasificación se evalúa en función de la solicitud. Varias solicitudes en una única sesión se pueden clasificar de forma diferente.

## <a name="classification-precedence"></a>Precedencia de clasificación

Como parte del proceso de clasificación, la prioridad es en su lugar para determinar qué clase de recurso se asigna. La clasificación basada en un usuario de base de datos tiene prioridad sobre pertenencia a roles. Si crea un clasificador que asigna al usuario de base de datos de UserA a la clase de recurso mediumrc. A continuación, asigne el rol de base de datos RoleA (de los cuales el usuario es miembro) a la clase de recurso largerc. El clasificador que el usuario de base de datos se asigna a la clase de recurso mediumrc tendrá prioridad sobre el clasificador que se asigna el rol de base de datos RoleA a la clase de recurso largerc.

Si un usuario es miembro de varios roles con diferentes clases de recursos asignado o en varios clasificadores no coincidía, el usuario tiene la asignación de clase de recursos más alta.  Este comportamiento es coherente con el comportamiento de asignación de clase de recursos existente.

## <a name="system-classifiers"></a>Clasificadores de sistema

Clasificación de la carga de trabajo tiene clasificadores de carga de trabajo del sistema. Los clasificadores de sistema asignan miembros de la función de clase de recursos existentes a las asignaciones de recursos de clase de recursos con importancia normal. No se puede quitar los clasificadores de sistema. Para ver los clasificadores de sistema, puede ejecutar la siguiente consulta:

```sql
SELECT * FROM sys.workload_management_workload_classifiers where classifier_id <= 12
```

## <a name="mixing-resource-class-assignments-with-classifiers"></a>Mezclar las asignaciones de clase con clasificadores de recursos

Clasificadores sistema creados en su nombre proporcionan una ruta de acceso sencilla para migrar a la clasificación de la carga de trabajo. Con asignaciones de rol de clase de recursos con la precedencia de clasificación, puede provocar clasificaciones incorrectas cuando empiece a crear nuevas clasificadores con importancia.

Considere el siguiente escenario:

•Se existente data warehouse tiene un usuario de base de datos que dbauser asignado a la función de clase de recurso largerc. La asignación de la clase de recurso se realizaba con sp_addrolemember.
•La almacenamiento de datos se ha actualizado con la administración de cargas de trabajo.
•Para probar la nueva sintaxis de clasificación, el rol de base de datos DBARole (que DBAUser es un miembro), tiene un clasificador creado para ellos su asignación a mediumrc e importancia alta.
•When DBAUser inicia sesión y se ejecuta una consulta, la consulta se asignará a largerc. Dado que un usuario tiene prioridad sobre una pertenencia a roles.

Para simplificar la solución de problemas de clasificaciones incorrectas, se recomienda que quitar las asignaciones de rol de clase de recursos a medida que cree clasificadores de carga de trabajo.  El código siguiente devuelve las pertenencias a roles de clase de recurso existente.  Ejecute [sp_droprolemember](/sql/relational-databases/system-stored-procedures/sp-droprolemember-transact-sql) para cada nombre de miembro devuelto desde la clase de recurso correspondiente.

```sql
SELECT  r.name AS [Resource Class]
,       m.name AS membername
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r ON rm.role_principal_id = r.principal_id
JOIN    sys.database_principals AS m ON rm.member_principal_id = m.principal_id
WHERE   r.name IN ('mediumrc','largerc','xlargerc','staticrc10','staticrc20','staticrc30','staticrc40','staticrc50','staticrc60','staticrc70','staticrc80');

--for each row returned run
sp_droprolemember ‘[Resource Class]’, membername
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la clasificación de la carga de trabajo de SQL Data Warehouse y la importancia, consulte [crear un clasificador de carga de trabajo](quickstart-create-a-workload-classifier-tsql.md) y [SQL Data Warehouse importancia](sql-data-warehouse-workload-importance.md). Consulte [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) para ver las consultas y la importancia asignada.
