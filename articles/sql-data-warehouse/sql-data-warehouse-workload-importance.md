---
title: Importancia de la carga de trabajo | Microsoft Docs
description: Instrucciones de configuración de importancia para las consultas en Azure SQL Data Warehouse.
services: sql-data-warehouse
author: ronortloff
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: workload management
ms.date: 03/13/2019
ms.author: rortloff
ms.reviewer: jrasnick
ms.openlocfilehash: 12e7d9bc22eff14bbf302aed50080412d04a40d3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61474709"
---
# <a name="sql-data-warehouse-workload-importance-preview"></a>Importancia de carga de trabajo de SQL Data Warehouse (versión preliminar)

En este artículo se explica cómo importancia de la carga de trabajo puede influir en el orden de ejecución para las solicitudes de SQL Data Warehouse.

> [!Note]
> La versión preliminar de la clasificación de la carga de trabajo está disponible en SQL Data Warehouse Gen2. La versión preliminar de la importancia y la clasificación de la carga de trabajo es para las compilaciones publicadas a partir del 9 de abril de 2019.  Los usuarios deben evitar usar compilaciones anteriores a esta fecha para realizar pruebas de administración de cargas de trabajo.  Para determinar si la compilación es capaz de administrar cargas de trabajo, ejecute la selección @@version cuando esté conectado a la instancia de SQL Data Warehouse.

## <a name="importance"></a>importancia

> [!Video https://www.youtube.com/embed/_2rLMljOjw8]

Las necesidades empresariales pueden requerir que las cargas de trabajo para que sea más importante que otros usuarios de almacenamiento de datos.  Considere un escenario donde esté cargados los datos de ventas de misión crítica antes de que el fiscal período cerrar.  Cargas de datos para otros orígenes, como los datos meteorológicos no tienen SLA estricta.   Establecer importancia alta para una solicitud para cargar los datos de ventas y poca importancia en una solicitud para cargar si los datos garantiza que la carga de datos de ventas Obtiene el primer acceso a los recursos y se completa con mayor rapidez.

## <a name="importance-levels"></a>Niveles de importancia

Hay cinco niveles de importancia: baja, normal, below_normal, above_normal y alto.  Las solicitudes que no establecen la importancia se asignan en el nivel predeterminado de normal.  Las solicitudes que tienen el mismo nivel de importancia tienen el mismo comportamiento de programación que existe hoy en día.

## <a name="importance-scenarios"></a>Escenarios de importancia

Más allá del escenario básico de importancia que se ha descrito anteriormente con las ventas y datos meteorológicos, hay otros escenarios donde la importancia de la carga de trabajo ayuda a satisfacer las necesidades de consulta y procesamiento de datos.

### <a name="locking"></a>Bloqueo

El acceso a los bloqueos de lectura y escritura de actividad es un área de contención natural.  Algunas actividades, como [conmutación de particiones](/azure/sql-data-warehouse/sql-data-warehouse-tables-partition) o [RENAME OBJECT](/sql/t-sql/statements/rename-transact-sql) requieren bloqueos con privilegios elevados.  Sin la importancia de la carga de trabajo, SQL Data Warehouse se optimiza para el rendimiento.  Optimización de rendimiento significa que cuando se ejecuta y solicitudes en cola tienen las mismas necesidades de bloqueos y los recursos están disponibles, las solicitudes en cola pueden omitir las solicitudes con mayores necesidades de bloqueo que ha llegado en la cola de solicitudes anteriormente.  Una vez importancia de la carga de trabajo se aplica a las solicitudes con mayor bloqueo necesario. Solicitud con mayor importancia se ejecutará antes de la solicitud con una importancia menor.

Considere el siguiente ejemplo:

Q1 ejecuta activamente y seleccionar datos de SalesFact.
P2 se pone en cola esperando Q1 en completarse.  Se envió a las 9 a.m. y está intentando datos nuevos del conmutador de partición en SalesFact.
P3 se envía a las 9:01 a.m. y desea seleccionar datos de SalesFact.

Si T2 y Q3 tienen la misma importancia y Q1 todavía se está ejecutando, Q3 inicia la ejecución. Q2 continuará esperando un bloqueo exclusivo en SalesFact.  Si T2 tiene una importancia mayor que Q3, Q3 esperará hasta que Q2 finalice antes de comenzar la ejecución.

### <a name="non-uniform-requests"></a>Solicitudes no uniforme

Otro escenario donde puede ayudar a satisfacer las demandas de consultas importancia es cuando se envían las solicitudes con diferentes clases de recursos.  Como se mencionó anteriormente, en la misma importancia, SQL Data Warehouse se optimiza para el rendimiento.  Cuando se ponen en cola las solicitudes de tamaño mixto (por ejemplo, "smallrc" o mediumrc), SQL Data Warehouse elegirá la solicitud que llegan más antiguo que se adapta a los recursos disponibles.  Si se aplica la importancia de la carga de trabajo, programar la solicitud de importancia más alta a continuación.
  
Considere el siguiente ejemplo en DW500c:

P1, P2, P3 y P4 ejecutan consultas smallrc.
P5 se envía con la clase de recurso mediumrc a las 9 a.m.
P6 se envía con la clase de recursos smallrc a las 9:01 am.

Dado que P5 es mediumrc, requiere dos espacios de simultaneidad.  P5 debe esperar para dos de las consultas que completar.  Sin embargo, cuando se completa una de las consultas de ejecución (P1 a P4), P6 está programada en inmediatamente porque los recursos existen para ejecutar la consulta.  Si P5 tiene mayor importancia que P6, P6 espera hasta que P5 se está ejecutando antes de empezar a ejecutar.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información acerca de la clasificación de la carga de trabajo de SQL Data Warehouse, consulte [clasificación de carga de trabajo de almacenamiento de datos de SQL](sql-data-warehouse-workload-classification.md) y [crear un clasificador de carga de trabajo](quickstart-create-a-workload-classifier-tsql.md). Consulte [sys.dm_pdw_exec_requests](/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql) para ver las consultas y la importancia asignada.
