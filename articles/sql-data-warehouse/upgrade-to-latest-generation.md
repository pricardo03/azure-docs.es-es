---
title: Actualización a la última generación de Azure SQL Data Warehouse | Microsoft Docs
description: Actualización de Azure SQL Data Warehouse a la última generación de arquitectura de almacenamiento y hardware de Azure.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: manage
ms.date: 08/22/2018
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: fe1f2e026aaa4260d34b9b1cb96064053af1c3c7
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51568019"
---
# <a name="optimize-performance-by-upgrading-sql-data-warehouse"></a>Actualización de SQL Data Warehouse para optimizar el rendimiento
Actualización de Azure SQL Data Warehouse a la última generación de arquitectura de almacenamiento y hardware de Azure.

## <a name="why-upgrade"></a>¿Por qué actualizar?
Ahora puede actualizar sin problemas al nivel Gen2 optimizado para proceso de SQL Data Warehouse en Azure Portal. Si tiene un almacenamiento de datos del nivel Gen1 optimizado para proceso, se recomienda actualizar. Mediante la actualización, puede usar la última generación de hardware de Azure y arquitectura de almacenamiento mejorada. Puede aprovechar las ventajas del rendimiento más rápido, mayor escalabilidad y almacenamiento ilimitado en columnas. 

> [!VIDEO https://www.youtube.com/embed/9B2F0gLoyss]

## <a name="applies-to"></a>Se aplica a
Esta actualización se aplica a los almacenamientos de datos del nivel Gen1 optimizado para proceso.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Antes de empezar
> [!NOTE]
> Si el almacenamiento de datos del nivel Gen1 optimizado para proceso existente no está en una región donde está disponible el nivel Gen2 optimizado para proceso, puede realizar la [restauración geográfica](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) mediante PowerShell en una región admitida.
> 
>

1. Si la actualización del almacenamiento de datos del nivel Gen1 optimizado para proceso se pausa, [reanude el almacenamiento de datos](pause-and-resume-compute-portal.md).
2. Prepárese para unos minutos de inactividad. 



## <a name="start-the-upgrade"></a>Iniciar la actualización

1. Vaya al almacenamiento de datos del nivel Gen1 optimizado para proceso en Azure Portal y haga clic en la tarjeta **Actualizar a Gen2** de la pestaña Tareas: ![Upgrade_1](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_1.png)
    
> [!NOTE]
> Si no aparece la tarjeta **Actualizar a Gen2** en la pestaña Tareas, su suscripción es de un tipo que tiene limitaciones en la región actual. [Envíe una incidencia de soporte técnico](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-get-started-create-support-ticket) para obtener la lista de elementos permitidos en la suscripción.

2. De forma predeterminada, **seleccione el nivel de rendimiento sugerido** para el almacenamiento de datos en función del nivel de rendimiento actual en el nivel Gen1 optimizado para proceso mediante la asignación siguiente:
    
   | Nivel Gen1 optimizado para proceso | Nivel Gen2 optimizado para proceso |
   | :----------------------: | :-------------------: |
   |      DW100 – DW600       |        DW500c         |
   |          DW1000          |        DW1000c        |
   |          DW1200          |        DW1500c        |
   |          DW1500          |        DW1500c        |
   |          DW2000          |        DW2000c        |
   |          DW3000          |        DW3000c        |
   |          DW6000          |        DW6000c        |

3. Asegúrese de que la carga de trabajo ha terminado de ejecutarse y está en modo inactivo antes de actualizar. Experimentará tiempo de inactividad unos minutos antes de que el almacenamiento de datos vuelva a estar en línea como un almacenamiento de datos de nivel Gen2 optimizado para proceso. Haga clic en **Actualizar**. Actualmente, el precio del nivel de rendimiento de Gen2 optimizado para proceso está medio desactivado durante el período de versión preliminar:
    
   ![Upgrade_2](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_2.png)

4. Para **supervisar la actualización**, compruebe el estado en Azure Portal:

   ![Upgrade3](./media/sql-data-warehouse-upgrade-to-latest-generation/Upgrade_to_Gen2_3.png)
   
   El primer paso del proceso de actualización recorre la operación de escalado ("Actualización: sin conexión") donde todas las sesiones se terminan y las conexiones se descartan. 
   
   El segundo paso del proceso de actualización es la migración de datos ("Actualización: en línea"). La migración de datos es un proceso en segundo plano lento y en línea, durante el cual se mueven lentamente los datos en columnas desde la arquitectura de almacenamiento antigua hasta la nueva arquitectura de almacenamiento mediante una caché de SSD local. Durante este tiempo, el almacenamiento de datos estará en línea para consulta y carga. Todos los datos estarán disponibles para consulta tanto si se han migrado como si no. La migración de los datos se produce a una velocidad variable, según el tamaño de los datos, el nivel de rendimiento y el número de segmentos del almacén de columnas. 

5. **Recomendación opcional:** para acelerar el proceso en segundo plano de migración de los datos, puede forzar inmediatamente el movimiento de datos mediante la ejecución de [Alter Index rebuild](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-index) en todas las tablas de almacén de columnas principales que consulte con un SLO y una clase de recurso más grandes. Esta operación se realiza **sin conexión** en comparación con el proceso en segundo plano lento que puede tardar horas en completarse en f unción del número y el tamaño de las tablas; sin embargo, la migración de los datos será mucho más rápida donde pueda aprovechar la nueva arquitectura de almacenamiento mejorada una vez completada con grupos de filas de alta calidad. 

La consulta siguiente genera los comandos Alter Index Rebuild necesarios para acelerar el proceso de migración de datos:

```sql
SELECT 'ALTER INDEX [' + idx.NAME + '] ON [' 
       + Schema_name(tbl.schema_id) + '].[' 
       + Object_name(idx.object_id) + '] REBUILD ' + ( CASE 
                                                         WHEN ( 
                                                     (SELECT Count(*) 
                                                      FROM   sys.partitions 
                                                             part2 
                                                      WHERE  part2.index_id 
                                                             = idx.index_id 
                                                             AND 
                                                     idx.object_id = 
                                                     part2.object_id) 
                                                     > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              ELSE '' 
                                                       END ) + '; SELECT ''[' + 
              idx.NAME + '] ON [' + Schema_name(tbl.schema_id) + '].[' + 
              Object_name(idx.object_id) + '] ' + ( 
              CASE 
                WHEN ( (SELECT Count(*) 
                        FROM   sys.partitions 
                               part2 
                        WHERE 
                     part2.index_id = 
                     idx.index_id 
                     AND idx.object_id 
                         = part2.object_id) > 1 ) THEN 
              ' PARTITION = ' 
              + Cast(part.partition_number AS NVARCHAR(256)) 
              + ' completed'';' 
              ELSE ' completed'';' 
                                                    END ) 
FROM   sys.indexes idx 
       INNER JOIN sys.tables tbl 
               ON idx.object_id = tbl.object_id 
       LEFT OUTER JOIN sys.partitions part 
                    ON idx.index_id = part.index_id 
                       AND idx.object_id = part.object_id 
WHERE  idx.type_desc = 'CLUSTERED COLUMNSTORE'; 
```



## <a name="next-steps"></a>Pasos siguientes
El almacenamiento de datos actualizado está en línea. Para aprovechar las ventajas de la arquitectura mejorada, consulte [Clases de recursos para la administración de cargas de trabajo](resource-classes-for-workload-management.md).
 
