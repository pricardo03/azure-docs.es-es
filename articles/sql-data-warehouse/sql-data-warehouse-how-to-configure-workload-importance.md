---
title: Configure la importancia de carga de trabajo en Azure SQL Data Warehouse | Microsoft Docs
description: Obtenga información sobre cómo establecer la importancia de nivel de solicitud.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.subservice: workload management
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: rortloff
ms.reviewer: igorstan
ms.openlocfilehash: 7121d2f022f9c9a5bbc02f04955d2857f3ec986f
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66241241"
---
# <a name="configure-workload-importance-in-azure-sql-data-warehouse"></a>Configure la importancia de carga de trabajo en Azure SQL Data Warehouse

Establecimiento de importancia en el almacén de datos de SQL permite influyen en la programación de las consultas. Las consultas con mayor importancia se programará para ejecutarse antes de las consultas con una importancia menor. Para asignar importancia a las consultas, deberá crear un clasificador de carga de trabajo.

## <a name="create-a-workload-classifier-with-importance"></a>Crear un clasificador de carga de trabajo con importancia

A menudo en un escenario de almacenamiento de datos tienen los usuarios que necesitan sus consultas ejecutarse rápidamente.  El usuario podría ser ejecutivos de la compañía que necesitan para ejecutar informes o el usuario podrían ser un analista que ejecuta una consulta ad hoc. Crear un clasificador de carga de trabajo para asignar prioridad a una consulta.  Los ejemplos siguientes usan el nuevo [crear clasificador de carga de trabajo](/sql/t-sql/statements/create-workload-classifier-transact-sql?view=azure-sqldw-latest) sintaxis para crear dos clasificadores.  MemberName puede ser un único usuario o un grupo. Las clasificaciones de usuario individuales tienen prioridad sobre las clasificaciones de rol. Para buscar los usuarios del almacén de datos existente, ejecute:

```sql
Select name from sys.sysusers
```

Para crear un clasificador de carga de trabajo, para un usuario con mayor importancia ejecute:

```sql
CREATE WORKLOAD CLASSIFIER ExecReportsClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
                   ,MEMBERNAME        = 'name'  
                   ,IMPORTANCE        =  above_normal);  

```

Para crear un clasificador de carga de trabajo para un usuario que ejecuta consultas ad hoc con menor importancia ejecutar:  

```sql
CREATE WORKLOAD CLASSIFIER AdhocClassifier  
    WITH (WORKLOAD_GROUP = 'xlargerc'
                   ,MEMBERNAME        = 'name'  
                   ,IMPORTANCE        =  below_normal);  
```

## <a name="next-steps"></a>Pasos siguientes
- Para obtener más información acerca de la administración de cargas de trabajo, consulte [clasificación de la carga de trabajo](sql-data-warehouse-workload-classification.md)
- Para obtener más información sobre la importancia, consulte [importancia de la carga de trabajo](sql-data-warehouse-workload-importance.md)

> [!div class="nextstepaction"]
> [Vaya a administrar y supervisar la importancia de la carga de trabajo ](sql-data-warehouse-how-to-manage-and-monitor-workload-importance.md)
