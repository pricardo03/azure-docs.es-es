---
title: 'Copia de seguridad y restauración de Azure SQL Data Warehouse: instantáneas y redundancia geográfica | Microsoft Docs'
description: Obtenga información acerca de cómo funcionan la copia de seguridad y la restauración en Azure SQL Data Warehouse. Use copias de seguridad de almacenamiento de datos para restaurar el almacenamiento de datos a un punto de restauración en la región primaria. Use copias de seguridad con redundancia geográfica para restaurar en una región geográfica distinta.
services: sql-data-warehouse
author: kevinvngo
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 03/01/2019
ms.author: kevin
ms.reviewer: igorstan
ms.openlocfilehash: 55874d261ac453d559975f25b2272319cdc6a7db
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448005"
---
# <a name="backup-and-restore-in-azure-sql-data-warehouse"></a>Copia de seguridad y restauración en Azure SQL Data Warehouse

Aprenda a usar la copia de seguridad y restauración en Azure SQL Data Warehouse. Use puntos de restauración de almacenamiento de datos para devolver el almacenamiento a un estado anterior o copiarlo en la región primaria. Utilice copias de seguridad con redundancia geográfica del almacenamiento de datos para restaurarlo en otra región geográfica.

## <a name="what-is-a-data-warehouse-snapshot"></a>¿Qué es una instantánea de almacenamiento de datos

Una *instantánea de almacenamiento de datos* crea un punto de restauración que se puede aprovechar para recuperar o copiar el almacenamiento de datos en un estado anterior.  Dado que SQL Data Warehouse es un sistema distribuido, una instantánea de almacenamiento de datos consta de muchos archivos que se almacenan en Azure Storage. Las instantáneas capturan los cambios incrementales de los datos almacenados en el almacenamiento de datos.

Una *restauración de almacenamiento de datos* es un nuevo almacenamiento de datos que se crea a partir de un punto de restauración de un almacenamiento de datos existente o eliminado. La restauración del almacenamiento de datos es una parte esencial de cualquier estrategia de recuperación ante desastres y continuidad empresarial, ya que vuelve a crear los datos tras daños o eliminaciones accidentales. El almacenamiento de datos es también un mecanismo eficaz para crear copias del almacenamiento de datos con fines de prueba o desarrollo.  SQL Data Warehouse utiliza mecanismos de restauración rápida en la misma región, lo que se ha medido para que tarde menos de 20 minutos con cualquier tamaño de datos.

## <a name="automatic-restore-points"></a>Puntos de restauración automáticos

Las instantáneas son una característica integrada del servicio que crea los puntos de restauración. No es necesario habilitar esta funcionalidad. Actualmente, los usuarios no pueden eliminar los puntos de restauración automática cuando el servicio utiliza estos puntos de restauración para mantener los contratos de nivel de servicio para la recuperación.

SQL Data Warehouse toma instantáneas del almacenamiento de datos a lo largo del día que crean puntos de restauración que están disponibles durante siete días. No se puede cambiar este período de retención. SQL Data Warehouse admite un objetivo de punto de recuperación (RPO) de ocho horas. Puede restaurar el almacenamiento de datos de la región primaria a partir de cualquiera de las instantáneas capturadas en los últimos siete días.

Para ver cuándo se inició la última instantánea, ejecute esta consulta en SQL Data Warehouse en línea.

```sql
select   top 1 *
from     sys.pdw_loader_backup_runs
order by run_id desc
;
```

## <a name="user-defined-restore-points"></a>Puntos de restauración definidos por el usuario

Esta característica permite a manualmente las instantáneas de desencadenador para crear puntos de restauración del almacenamiento de datos antes y después de realizar grandes modificaciones. Esta funcionalidad garantiza que los puntos de restauración son lógicamente coherentes, que proporciona protección de datos adicionales en el caso de las interrupciones de la carga de trabajo o errores de usuario para el tiempo de recuperación rápida. Los puntos de restauración definidos por el usuario están disponibles durante siete días y se eliminan automáticamente. No se puede cambiar el período de retención de los puntos de restauración definidos por el usuario. Se garantizan **42 puntos de restauración definidos por el usuario** en un momento dado, por lo que deben [eliminarse](https://go.microsoft.com/fwlink/?linkid=875299) antes de crear otro punto de restauración. Puede desencadenar instantáneas para crear puntos de restauración definidos por el usuario mediante [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.sql/new-azurermsqldatabaserestorepoint#examples) o Azure Portal.

> [!NOTE]
> Si necesita más de siete días de puntos de restauración, vote por esta funcionalidad [aquí](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/35114410-user-defined-retention-periods-for-restore-points). También puede crear un punto de restauración definido por el usuario y restaurar desde el punto de restauración recién creado en un nuevo almacenamiento de datos. Cuando haya realizado la restauración, tendrá el almacenamiento de datos en línea y podrá pausarlo indefinidamente para ahorrar costos de proceso. La base de datos en pausa genera gastos de almacenamiento según la tarifa de Azure Premium Storage. Si necesita una copia activa del almacenamiento de datos restaurado, puede reanudarlo, lo que sólo le llevará unos minutos.

### <a name="restore-point-retention"></a>Retención de punto de recuperación

Los detalles de las listas siguientes para períodos de retención de punto de restauración:

1. SQL Data Warehouse elimina un punto de restauración cuando alcanza el período de retención de 7 días **y** cuando hay al menos 42 puntos de restauración totales (incluidos los definidos por el usuario y los automáticos).
2. Las instantáneas no se toman cuando un almacenamiento de datos está en pausa.
3. La antigüedad de un punto de restauración se mide por los días naturales absolutos a partir del momento en que se toma el punto de restauración, incluso cuando el almacenamiento de datos está en pausa.
4. En cualquier momento, se garantiza que un almacenamiento de datos podrá almacenar hasta 42 puntos de restauración definidos por el usuario y 42 puntos de restauración automáticos siempre y cuando estos puntos de restauración no hayan alcanzado el período de retención de 7 días.
5. Si se toma una instantánea, el almacenamiento de datos se detiene durante más de 7 días y luego se reanuda, es posible que el punto de restauración persista hasta que haya un total de 42 puntos de restauración (incluidos los definidos por el usuario y los automáticos).

### <a name="snapshot-retention-when-a-data-warehouse-is-dropped"></a>Retención de instantáneas cuando se quita un almacenamiento de datos

Cuando se quita un almacenamiento de datos SQL Data Warehouse crea una instantánea final y la guarda durante siete días. Puede restaurar el almacenamiento de datos en el punto de restauración final durante la eliminación.

> [!IMPORTANT]
> Si elimina una instancia de servidor de SQL lógica, todas las bases de datos que pertenecen a la instancia también se eliminan y no se pueden recuperar. No puede restaurar un servidor eliminado.

## <a name="geo-backups-and-disaster-recovery"></a>Copias de seguridad geográficas y recuperación ante desastres

SQL Data Warehouse realiza una copia de seguridad geográfica una vez al día en un [centro de datos emparejado](../best-practices-availability-paired-regions.md). El RPO para una restauración geográfica es de 24 horas. Puede restaurar la copia de seguridad de replicación geográfica en un servidor de cualquier otra región donde se admita SQL Data Warehouse. Una copia de seguridad geográfica garantiza que pueda restaurar el almacenamiento de datos en caso de que no tenga acceso a los puntos de restauración de su región primaria.

Las copias de seguridad geográficas están activadas de manera predeterminada. Si el almacenamiento de datos es Gen1, puede [optar por no participar](/powershell/module/az.sql/set-azsqldatabasegeobackuppolicy), si lo desea. No se puede optar por no realizar las copias de seguridad de replicación geográfica para Gen2, ya que la protección de datos es una garantía integrada.

> [!NOTE]
> Si necesita un objetivo de punto de recuperación más reducido para copias de seguridad de replicación geográfica, vote por esta funcionalidad [aquí](https://feedback.azure.com/forums/307516-sql-data-warehouse). También puede crear un punto de restauración definido por el usuario y restaurar a partir del punto de restauración recién creado en un nuevo almacenamiento de datos. Cuando haya realizado la restauración, tendrá el almacenamiento de datos en línea y podrá pausarlo indefinidamente para ahorrar costos de proceso. La base de datos en pausa genera gastos de almacenamiento según la tarifa de Azure Premium Storage. Si necesita una copia activa del almacenamiento de datos, puede reanudarlo, lo que solo le llevará unos minutos.

## <a name="backup-and-restore-costs"></a>Costos de copia de seguridad y restauración

Observará que la factura de Azure tiene un elemento de línea para Storage y un elemento de línea para Storage de recuperación ante desastres. El cargo de Storage corresponde al costo total del almacenamiento de sus datos en la región primaria con los cambios incrementales que capturan las instantáneas. Para obtener una explicación más detallada de cómo se cobran las instantáneas, consulte [Understanding how Snapshots Accrue Charges](https://docs.microsoft.com/rest/api/storageservices/Understanding-How-Snapshots-Accrue-Charges?redirectedfrom=MSDN#snapshot-billing-scenarios) (Cómo se acumulan los cargos de instantáneas). El cargo con redundancia geográfica abarca el costo de almacenamiento de las copias de seguridad geográficas.  

El costo total del almacenamiento de datos principal y de los siete días de cambios de instantánea se redondea al TB más cercano. Por ejemplo, si el almacenamiento de datos es de 1,5 TB y las capturas de instantáneas 100 GB, se le facturan 2 TB de datos según las tarifas de Azure Premium Storage.

Si usa almacenamiento con redundancia geográfica, recibirá un cargo de almacenamiento por separado. El almacenamiento con redundancia geográfica se factura según la tarifa estándar de almacenamiento geográficamente redundante con acceso de lectura (RA-GRS).

Para más información sobre los precios de SQL Data Warehouse, vea [Precios de SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/) y [cargos de salida](https://azure.microsoft.com/pricing/details/bandwidth/) al restaurar entre regiones.

## <a name="restoring-from-restore-points"></a>Restauración a partir de puntos de restauración

Cada instantánea crea un punto de restauración que representa la hora de inicio de la instantánea. Para restaurar un almacenamiento de datos, elija un punto de restauración y emita un comando de restauración.  

Puede mantener el almacenamiento de datos restaurado y el actual, o eliminar uno de ellos. Si quiere reemplazar el almacenamiento de datos actual por el restaurado, puede cambiarle el nombre mediante [ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse) con la opción MODIFY NAME.

Para restaurar un almacenamiento de datos, consulte [Restauración de un almacenamiento de datos mediante Azure Portal](sql-data-warehouse-restore-database-portal.md), [Restauración de un almacenamiento de datos mediante PowerShell](sql-data-warehouse-restore-database-powershell.md) o [Restauración de un almacenamiento de datos mediante API REST](sql-data-warehouse-restore-database-rest-api.md).

Para restaurar un almacenamiento de datos eliminado o en pausa, puede [crear una incidencia de soporte técnico](sql-data-warehouse-get-started-create-support-ticket.md).

## <a name="cross-subscription-restore"></a>Entre la restauración de la suscripción

Si necesita restaurar directamente a través de la suscripción, vote por esta funcionalidad [aquí](https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/36256231-enable-support-for-cross-subscription-restore). Restaurar a un servidor lógico diferente y ['Mover'](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources) el servidor a través de suscripciones para realizar una restauración de suscripción cruzado. 

## <a name="geo-redundant-restore"></a>Restauración con redundancia geográfica

Puede [restaurar el almacenamiento de datos](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-restore-database-powershell#restore-from-an-azure-geographical-region) en cualquier región que admita SQL Data Warehouse en el nivel de rendimiento elegido.

> [!NOTE]
> Para llevar a cabo una restauración con redundancia geográfica no puede haber anulado esta característica.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de la planificación ante desastres, consulte [Información general sobre la continuidad empresarial](../sql-database/sql-database-business-continuity.md).
