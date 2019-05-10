---
title: Restauración de una base de datos de Azure SQL a partir de una copia de seguridad | Microsoft Docs
description: Obtenga información acerca de la restauración a un momento dado, que le permite revertir una base de datos de Azure SQL a un momento dado anterior (hasta 35 días).
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
manager: craigg
ms.date: 04/30/2019
ms.openlocfilehash: 47bf59adb33f3685b31430c652b31880d383833e
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232647"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Recuperación de una base de datos de Azure SQL mediante copias de seguridad automatizadas

De forma predeterminada, las copias de seguridad de SQL Database se almacenan en almacenamiento de blobs de replicación geográfica (RA-GRS). Las siguientes opciones están disponibles para la recuperación de bases de datos mediante [copias de seguridad de base de datos automatizadas](sql-database-automated-backups.md):

- Cree una nueva base de datos en el mismo servidor de SQL Database recuperada en un punto especificado en el tiempo durante el período de retención.
- Cree una base de datos en el mismo servidor de SQL Database recuperado a la hora de eliminación de una base de datos eliminada.
- Cree una nueva base de datos en cualquier servidor de SQL Database de la misma región recuperada hasta el momento de las copias de seguridad más recientes.
- Cree una nueva base de datos en cualquier servidor de SQL Database de cualquier región recuperada hasta el momento de las copias de seguridad con replicación más recientes.

Si configuró [copia de seguridad de retención a largo plazo](sql-database-long-term-retention.md), también puede crear una nueva base de datos desde cualquier copia de seguridad LTR en cualquier servidor de base de datos SQL.

> [!IMPORTANT]
> Durante la restauración, no se puede sobrescribir la base de datos existente.

Cuando se usa el nivel de servicio Standard o Premium, una base de datos restaurada incurre en un costo de almacenamiento adicional en las siguientes condiciones:

- Restauración de P11 – P15 a S4-S12 o P1–P6 si el tamaño máximo de la base de datos es mayor de 500 GB.
- Restauración de P1-P6 a S4-S12 si el tamaño máximo de la base de datos es mayor de 250 GB.

El archivo extra costo es icurred cuando el tamaño máximo de la base de datos restaurada es mayor que la cantidad de almacenamiento incluido con el nivel de rendimiento y de nivel de servicio de la base de datos del destino. El almacenamiento adicional aprovisionado por encima del importe incluido se aplicarán cargos adicionales. Para más información sobre los precios del almacenamiento adicional, consulte la [página de precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Si la cantidad de espacio usado real es menor que la cantidad de almacenamiento incluido, puede evitar este costo adicional estableciendo el tamaño máximo de la base de datos a la cantidad incluida.

> [!NOTE]
> Las [copias de seguridad de base de datos automatizadas](sql-database-automated-backups.md) se usan cuando se crea una [copia de la base de datos](sql-database-copy.md).

## <a name="recovery-time"></a>Tiempo de recuperación

El tiempo de recuperación para restaurar una base de datos mediante copias de seguridad de base de datos automatizadas se ve afectado por una serie de factores:

- El tamaño de la base de datos
- El tamaño de proceso de la base de datos
- El número de registros de transacciones implicados
- La cantidad de actividad que debe reproducirse para la recuperación hasta el punto de restauración
- El ancho de banda de red si la restauración es a una región diferente
- El número de solicitudes de restauración simultáneas que se están procesando en la región de destino

Bases de datos grandes o muy activo, la restauración puede tardar varias horas. Si hay una interrupción prolongada en una región, es posible que haya un gran número de solicitudes de restauración geográfica que se procesan en otras regiones. Si hay muchas solicitudes, puede aumentar el tiempo de recuperación de las bases de datos en esa región. La mayoría de las restauraciones de bases de datos dura menos de 12 horas.

Para una única suscripción, existen limitaciones en el número de solicitudes simultáneas de restauración.  Estas limitaciones se aplican a cualquier combinación de punto en restauraciones a un momento, la restauración geográfica y restauraciones de copia de seguridad de retención a largo plazo):

| | **Número máximo de solicitudes simultáneas que se van a procesar** | **Número máximo de solicitudes simultáneas que se van a enviar** |
| :--- | --: | --: |
|Base de datos única (por suscripción)|10|60|
|Grupo elástico (por grupo)|4|200|
||||

Actualmente no hay ningún método integrado que restaurar todo el servidor. El script [Azure SQL Database: Completa la recuperación del servidor](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) script es un ejemplo de cómo puede realizar esta tarea.

> [!IMPORTANT]
> Para poder efectuar una recuperación con copias de seguridad automatizadas, debe ser miembro del rol de colaborador de SQL Server en la suscripción o ser el propietario de la suscripción: consulte [RBAC: roles integrados](../role-based-access-control/built-in-roles.md). Las recuperaciones se pueden realizar a través del Portal de Azure, PowerShell o la API de REST. No puede utilizar Transact-SQL.

## <a name="point-in-time-restore"></a>Restauración a un momento dado

Puede restaurar una independiente, agrupada, o la instancia de base de datos a un momento anterior en el tiempo mediante el portal de Azure, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase), o el [API de REST](https://docs.microsoft.com/rest/api/sql/databases). La solicitud puede especificar cualquier nivel de servicio o calcular el tamaño de la base de datos restaurada. Asegúrese de que haya suficientes recursos en el servidor en el que se va a restaurar la base de datos. Una vez que haya finalizado, se creará una nueva base de datos en el mismo servidor que la base de datos original. La base de datos restaurada se cobrará tarifas normales basándose en su nivel de servicio y el tamaño de proceso. No se incurre en gastos hasta que finaliza la restauración de la base de datos.

Por lo general, una base de datos se restaura a un punto anterior para fines de recuperación. Puede tratar la base de datos restaurada como sustituto de la base de datos original o utilizarla como un origen de datos para actualizar la base de datos original.

- **Sustituto de base de datos**

  Si la base de datos restaurada está pensada como sustituto de la base de datos original, debe especificar el tamaño de la base de datos del orinal de proceso y el nivel de servicio. A continuación, puede cambiar el nombre de la base de datos original y asigne a la base de datos restaurada el nombre original mediante el [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) comando T-SQL.

- **Recuperación de datos**

  Si va a recuperar datos de la base de datos restaurada para recuperarse de un error del usuario o aplicación, necesita escribir y ejecutar un script de recuperación de datos que extrae datos de la base de datos restaurada y se aplica a la base de datos original. Aunque la operación de restauración puede tardar mucho tiempo en finalizar, la base de datos restaurada será visible en la lista de bases de datos en todo el proceso de restauración. Si elimina la base de datos durante la restauración, se cancelará la operación de restauración y no se le cobrará de la base de datos que no se completó la restauración.

Para recuperar una base de datos única, agrupada o de instancia a un momento dado mediante Azure Portal, abra la página de la base de datos y haga clic en **Restaurar** en la barra de herramientas.

![restauración a un momento dado](./media/sql-database-recovery-using-backups/point-in-time-recovery.png)

> [!IMPORTANT]
> Para restaurar una base de datos mediante programación desde una copia de seguridad, consulte [Recuperación mediante programación con copias de seguridad automatizadas](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="deleted-database-restore"></a>Restauración de la base de datos eliminada

Puede restaurar una base de datos eliminada a la hora de eliminación o en un momento anterior en el tiempo en el mismo servidor de base de datos SQL mediante Azure portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase), o el [REST (createMode = Restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate). También puede [restaurar una base de datos eliminada en Instancia administrada mediante PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance). 

> [!TIP]
> Para obtener un script de PowerShell de ejemplo que muestre cómo realizar una restauración en una base de datos eliminada, vea [Restauración de una instancia de SQL Database con PowerShell](scripts/sql-database-restore-database-powershell.md).
> [!IMPORTANT]
> Si elimina una instancia de servidor de Azure SQL Database, todas sus bases de datos también se eliminan y no se pueden recuperar. En estos momentos no es posible restaurar un servidor eliminado.

### <a name="deleted-database-restore-using-the-azure-portal"></a>Restauración de bases de datos eliminadas mediante Azure Portal

Para recuperar una base de datos eliminada mediante Azure portal, abra la página para el servidor y en el área de operaciones, haga clic en **bases de datos eliminadas**.

![restauración-base de datos-eliminada-1](./media/sql-database-recovery-using-backups/deleted-database-restore-1.png)

![restauración-base de datos-eliminada-2](./media/sql-database-recovery-using-backups/deleted-database-restore-2.png)

> [!IMPORTANT]
> Para restaurar una base de datos eliminada mediante programación, consulte [Recuperación mediante programación con copias de seguridad automatizadas](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="geo-restore"></a>Restauración geográfica

Una base de datos SQL situada en cualquier servidor de cualquier región de Azure se puede restaurar desde las copias de seguridad con replicación geográfica más recientes. Restauración geográfica usa una copia de seguridad con replicación geográfica como su origen. Puede solicitar aunque la base de datos o el centro de datos sea inaccesible debido a una interrupción.

Restauración geográfica es la opción de recuperación predeterminada cuando la base de datos no está disponible debido a una incidencia en la región de hospedaje. Puede restaurar la base de datos en un servidor en cualquier otra región. Hay un retraso entre el momento en que se realiza una copia de seguridad y el momento en que se replica geográficamente en un blob de Azure de una región diferente. Como resultado, la base de datos restaurada puede ser hasta una hora detrás de la base de datos original. En la siguiente ilustración se muestra la restauración de la base de datos a partir de la última copia de seguridad disponible en otra región.

![Restauración geográfica](./media/sql-database-geo-restore/geo-restore-2.png)

> [!TIP]
> Para obtener un script de PowerShell de ejemplo que muestre cómo realizar una georrestauración, vea [Restauración de una instancia de SQL Database con PowerShell](scripts/sql-database-restore-database-powershell.md).

Actualmente no se admite la restauración a un momento dad en una base de datos geográfica secundaria. La restauración a un momento dado solo puede realizarse en una base de datos principal. Para obtener información detallada sobre cómo usar la restauración geográfica a fin de recuperarse de una interrupción, consulte [Restauración de una base de datos SQL de Azure o una conmutación por error en una secundaria](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> La restauración geográfica es la solución de recuperación ante desastres más básica disponible en la base de datos SQL. Se basa en crea automáticamente copias de seguridad con replicación geográfica con RPO = 1 hora y el tiempo de recuperación estimado de hasta 12 horas. No garantiza que la región de destino tendrán la capacidad de restaurar las bases de datos después de un ourage regional porque es probable que un considerable aumento de la demanda. Para aplicaciones críticas que no son empresariales que usan bases de datos relativamente pequeñas, la restauración geográfica es una solución de recuperación ante desastres adecuada. Para aplicaciones críticas de otra de empresa que usan bases de datos grandes y deben garantizar la continuidad empresarial, debe usar [grupos de conmutación por error automática](sql-database-auto-failover-group.md). Ofrece RPO y RTO mucho menores, y siempre se garantiza la capacidad. Para más información sobre las opciones de continuidad empresarial, vea [Introducción a la continuidad empresarial](sql-database-business-continuity.md).

### <a name="geo-restore-using-the-azure-portal"></a>Restauración geográfica mediante Azure Portal

Para realizar una restauración geográfica de una base de datos durante su [período de retención en el modelo basado en DTU](sql-database-service-tiers-dtu.md) o su [período de retención en el modelo basado en núcleos virtuales](sql-database-service-tiers-vcore.md) mediante Azure Portal, abra la página Bases de datos SQL y haga clic en **Agregar**. En el cuadro de texto **Seleccionar origen** cuadro de texto, seleccione **Copia de seguridad**. Especifique la copia de seguridad desde la que se va a recuperar en la región y en el servidor de su elección.

> [!Note]
> Restauración geográfica mediante Azure portal no está disponible en la instancia administrada. Use PowerShell en su lugar.

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Recuperación mediante programación con copias de seguridad automatizadas

Como se dijo anteriormente, además de Azure Portal, la recuperación de una base de datos se puede realizar mediante programación con Azure PowerShell o la API de REST. En las tablas siguientes se describe el conjunto de comandos disponibles.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de PowerShell de Azure Resource Manager es compatible aún con Azure SQL Database, pero todo el desarrollo futuro es para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo de Az y en los módulos AzureRm son esencialmente idénticos.

- Para restaurar una base de datos agrupada o independiente, consulte [restauración AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase).

  | Cmdlet | DESCRIPCIÓN |
  | --- | --- |
  | [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) |Obtiene una o más bases de datos. |
  | [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Obtiene una base de datos eliminada que se puede restaurar. |
  | [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) |Obtiene una copia de seguridad con redundancia geográfica de una base de datos. |
  | [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) |Restaura una Base de datos SQL. |

  > [!TIP]
  > Para obtener un script de PowerShell de ejemplo que muestre cómo realizar una restauración a un momento dado, vea [Restauración de una instancia de SQL Database con PowerShell](scripts/sql-database-restore-database-powershell.md).

- Para restaurar una base de datos de instancia administrada, consulte [restauración AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase).

  | Cmdlet | DESCRIPCIÓN |
  | --- | --- |
  | [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance) |Obtiene uno o más instancias administradas. |
  | [Get-AzSqlInstanceDatabase](/powershell/module/az.sql/get-azsqlinstancedatabase) | Obtiene una instancia de las bases de datos. |
  | [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase) |Restaura una base de datos de instancia. |

### <a name="rest-api"></a>API DE REST

Para restaurar una base de datos única o agrupada mediante la API REST, consulte la siguiente tabla:

| API | DESCRIPCIÓN |
| --- | --- |
| [REST (createMode=Recovery)](https://docs.microsoft.com/rest/api/sql/databases) |Restaura una base de datos. |
| [Obtener el estado de creación o actualización de la base de datos](https://docs.microsoft.com/rest/api/sql/operations) |Devuelve el estado durante una operación de restauración. |

### <a name="azure-cli"></a>Azure CLI

- Para restaurar una base de datos única o agrupada mediante la CLI de Azure, consulte la referencia sobre el comando [az sql db restore](/cli/azure/sql/db#az-sql-db-restore).
- Para restaurar una instancia administrada mediante la CLI de Azure, consulte [restauración midb de az sql](/cli/azure/sql/midb#az-sql-midb-restore)

## <a name="summary"></a>Resumen

Las copias de seguridad automáticas protegen las bases de datos de los errores de usuario y de aplicación, la eliminación accidental de la base de datos y las interrupciones prolongadas. Esta funcionalidad integrada está disponible para todos los niveles de servicio y tamaños de proceso.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener una descripción general y los escenarios de la continuidad empresarial, consulte [Información general sobre la continuidad empresarial](sql-database-business-continuity.md).
- Para saber en qué consisten las copias de seguridad automatizadas de Azure SQL Database, consulte [Información general: copias de seguridad automatizadas de SQL Database](sql-database-automated-backups.md).
- Para obtener más información sobre la retención de copia de seguridad a largo plazo, lea sobre la [retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md).
- Para conocer las opciones de recuperación más rápidas, consulte el artículo sobre la [replicación geográfica activa](sql-database-active-geo-replication.md) y los [grupos de conmutación por error automáticos](sql-database-auto-failover-group.md).
