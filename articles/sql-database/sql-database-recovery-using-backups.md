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
ms.reviewer: mathoma, carlrab, danil
ms.date: 09/26/2019
ms.openlocfilehash: 890a9701615a05186b34883f4e953bbc045e906f
ms.sourcegitcommit: 7f6d986a60eff2c170172bd8bcb834302bb41f71
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71350073"
---
# <a name="recover-an-azure-sql-database-using-automated-database-backups"></a>Recuperación de una base de datos de Azure SQL mediante copias de seguridad automatizadas

De forma predeterminada, las copias de seguridad de SQL Database se almacenan en almacenamiento de blobs de replicación geográfica (RA-GRS). Las siguientes opciones están disponibles para la recuperación de bases de datos mediante [copias de seguridad de base de datos automatizadas](sql-database-automated-backups.md):

- Cree una nueva base de datos en el mismo servidor de SQL Database recuperada en un punto especificado en el tiempo durante el período de retención.
- Cree una base de datos en el mismo servidor de SQL Database recuperado a la hora de eliminación de una base de datos eliminada.
- Cree una nueva base de datos en cualquier servidor de SQL Database de la misma región recuperada hasta el momento de las copias de seguridad más recientes.
- Cree una nueva base de datos en cualquier servidor de SQL Database de cualquier región recuperada hasta el momento de las copias de seguridad con replicación más recientes.

Si ha configurado la [retención a largo plazo de copia de seguridad](sql-database-long-term-retention.md) también puede crear una nueva base de datos desde cualquier copia de seguridad de retención a largo plazo en cualquier servidor de SQL Database.

> [!IMPORTANT]
> Durante la restauración, no se puede sobrescribir la base de datos existente.

Cuando se usa el nivel de servicio Standard o Premium, una base de datos restaurada incurre en un costo de almacenamiento adicional en las siguientes condiciones:

- Restauración de P11 – P15 a S4-S12 o P1–P6 si el tamaño máximo de la base de datos es mayor de 500 GB.
- Restauración de P1-P6 a S4-S12 si el tamaño máximo de la base de datos es mayor de 250 GB.

Se generan gastos adicionales cuando el tamaño máximo de la base de datos restaurada es mayor que la cantidad de almacenamiento incluida en el nivel de rendimiento y el nivel de servicio de la base de datos de destino. El almacenamiento adicional aprovisionado por encima del importe incluido se cobra como gastos adicionales. Para más información sobre los precios del almacenamiento adicional, consulte la [página de precios de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/). Cuando la cantidad de espacio real usado es menor que la cantidad de almacenamiento incluido, se puede evitar este costo adicional si establece el tamaño máximo de la base de datos en la cantidad incluida.

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

En bases de datos muy grandes o activas, la restauración puede tardar varias horas. Si hay una interrupción prolongada en una región, es posible que haya un gran número de solicitudes de restauración geográfica que se procesan en otras regiones. Si hay muchas solicitudes, puede aumentar el tiempo de recuperación de las bases de datos en esa región. La mayoría de las restauraciones de bases de datos dura menos de 12 horas.

Para una única suscripción, existen limitaciones en el número de solicitudes simultáneas de restauración.  Estas limitaciones se aplican a cualquier combinación de restauraciones a un momento dado, restauraciones geográficas y restauraciones a partir de copias de seguridad de retención a largo plazo):

| | **Número máximo de solicitudes simultáneas que se van a procesar** | **Número máximo de solicitudes simultáneas que se van a enviar** |
| :--- | --: | --: |
|Base de datos única (por suscripción)|10|60|
|Grupo elástico (por grupo)|4|200|
||||

Actualmente no hay ningún método integrado para restaurar todo el servidor. El script [Azure SQL Database: Full Server Recovery](https://gallery.technet.microsoft.com/Azure-SQL-Database-Full-82941666) es un ejemplo de cómo realizar esta tarea.

> [!IMPORTANT]
> Para poder efectuar una recuperación con copias de seguridad automatizadas, debe ser miembro del rol de colaborador de SQL Server en la suscripción o ser el propietario de la suscripción: consulte [RBAC: roles integrados](../role-based-access-control/built-in-roles.md). Las recuperaciones se pueden realizar a través de Azure Portal, PowerShell o la API REST. No puede utilizar Transact-SQL.

## <a name="point-in-time-restore"></a>Restauración a un momento dado

Puede restaurar una base de datos independiente, agrupada o de instancia a un momento dado mediante Azure Portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) o la [API REST](https://docs.microsoft.com/rest/api/sql/databases). La solicitud puede especificar cualquier nivel de servicio o calcular el tamaño de la base de datos restaurada. Asegúrese de que haya suficientes recursos en el servidor en el que se va a restaurar la base de datos. Una vez completada, se creará una nueva base de datos en el mismo servidor que la base de datos original. La base de datos restaurada se cobrará según la tarifa normal en función de su nivel de servicio y tamaño de proceso. No se incurre en gastos hasta que finaliza la restauración de la base de datos.

Por lo general, una base de datos se restaura a un punto anterior para fines de recuperación. Puede tratar la base de datos restaurada como sustituta de la base de datos original o usarla como datos de origen para actualizar después la base de datos original.

- **Sustituto de base de datos**

  Si la base de datos restaurada está pensada como sustituto de la base de datos original, debe especificar el tamaño de proceso y el nivel de servicio de la base de datos original. Luego puede cambiar el nombre de la base de datos original y asignar a la base de datos restaurada el nombre original mediante el comando [ALTER DATABASE](/sql/t-sql/statements/alter-database-azure-sql-database) en T-SQL.

- **Recuperación de datos**

  Si va a recuperar datos de la base de datos restaurada para recuperarse de un error de usuario o de aplicación, debe escribir y ejecutar un script de recuperación de datos que extraiga datos de la base de datos restaurada y se aplique a la base de datos original. Aunque la operación de restauración puede tardar mucho tiempo en finalizar, la base de datos restaurada será visible en la lista de bases de datos en todo el proceso de restauración. Si elimina la base de datos durante la restauración, la operación de restauración se cancelará y no se le cobrará por la base de datos que no finalizó la restauración.
  
### <a name="point-in-time-restore-using-azure-portal"></a>Restauración a un momento dado con Azure Portal

La recuperación de una única base de datos SQL o una base de datos de instancia a un momento dado se realiza desde la hoja de información general de la base de datos que desea restaurar en Azure Portal.

#### <a name="single-azure-sql-database"></a>Base de datos única de Azure SQL Database

Para recuperar una base de datos única o agrupada a un momento dado mediante Azure Portal, abra la página de información general y haga clic en **Restaurar** en la barra de herramientas. Elija el origen de la copia de seguridad y seleccione el punto de copia de seguridad el momento a partir del cual se creará una nueva base de datos. 

  ![point-in-time-restore-single-sql-database](./media/sql-database-recovery-using-backups/pitr-backup-sql-database-annotated.png)

#### <a name="managed-instance-database"></a>Base de datos de Instancia administrada

Para recuperar una base de datos de instancia administrada a un momento dado mediante Azure Portal, abra la página de información general y haga clic en **Restaurar** en la barra de herramientas. Elija el punto de copia de seguridad a un momento dado del cual se creará una nueva base de datos. 

  ![point-in-time-restore-managed-instance-database](./media/sql-database-recovery-using-backups/pitr-backup-managed-instance-annotated.png)

> [!TIP]
> Para restaurar una base de datos mediante programación desde una copia de seguridad, consulte [Recuperación mediante programación con copias de seguridad automatizadas](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="deleted-database-restore"></a>Restauración de la base de datos eliminada

Puede restaurar una base de datos eliminada al momento en que se eliminó o un momento anterior en el mismo servidor de SQL Database o la misma instancia administrada a través de Azure Portal, [PowerShell](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) o [REST (createMode=Restore)](https://docs.microsoft.com/rest/api/sql/databases/createorupdate). La restauración de una base de datos eliminada se realiza mediante la creación de una nueva base de datos a partir de la copia de seguridad.

> [!IMPORTANT]
> Si elimina un servidor Azure SQL Database o una instancia administrada, todas sus bases de datos también se eliminan y no se pueden recuperar. Actualmente no hay soporte para restaurar un servidor eliminado o para restaurar una instancia administrada eliminada.

### <a name="deleted-database-restore-using-azure-portal"></a>Restauración de base de datos eliminada con Azure Portal

La restauración de bases de datos eliminadas de Azure Portal se realiza desde el servidor y el recurso de instancia.

#### <a name="single-azure-sql-database"></a>Base de datos única de Azure SQL Database

Para recuperar una base de datos eliminada única o agrupada con Azure Portal, abra la página de información general del servidor y haga clic en **Bases de datos eliminadas** en el menú de navegación. Seleccione la base de datos eliminada que desea restaurar y escriba el nombre de la nueva base de datos que se creará con los datos restaurados desde la copia de seguridad.

  ![deleted-database-restore](./media/sql-database-recovery-using-backups/restore-deleted-sql-database-annotated.png)

#### <a name="managed-instance-database"></a>Base de datos de Instancia administrada

En este momento, la opción para restaurar la base de datos eliminada para la instancia administrada no está disponible en Azure Portal. Puede usar PowerShell para restaurar la base de datos eliminada en una instancia administrada, consulte [Restaurar la base de datos eliminada en la instancia administrada mediante PowerShell ](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance).

### <a name="deleted-database-restore-using-powershell"></a>Restauración de base de datos eliminada mediante PowerShell

Use los scripts de muestra que se proporcionan a continuación para restaurar la base de datos eliminada para Azure SQL Database y la instancia administrada mediante PowerShell.

#### <a name="single-azure-sql-database"></a>Base de datos única de Azure SQL Database

Para obtener un script de PowerShell de ejemplo que muestre cómo realizar una restauración en una base de datos de Azure SQL eliminada, consulte [Restauración de una instancia de SQL Database con PowerShell](scripts/sql-database-restore-database-powershell.md).

#### <a name="managed-instance-database"></a>Base de datos de Instancia administrada

Para obtener un script de PowerShell de ejemplo que muestre cómo realizar una restauración en una base de datos de instancias eliminadas, consulte [Restaurar la base de datos eliminada en una instancia administrada mediante PowerShell](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../recreate-dropped-database-on-azure-sql-managed-instance). 

> [!TIP]
> Para restaurar una base de datos eliminada mediante programación, consulte [Recuperación mediante programación con copias de seguridad automatizadas](sql-database-recovery-using-backups.md#programmatically-performing-recovery-using-automated-backups)

## <a name="geo-restore"></a>Restauración geográfica

Una base de datos SQL situada en cualquier servidor de cualquier región de Azure se puede restaurar desde las copias de seguridad con replicación geográfica más recientes. La restauración geográfica usa una copia de seguridad con replicación geográfica como su origen. Puede solicitarla aunque la base de datos o el centro de datos sea inaccesible debido a una interrupción.

La función de restauración geográfica proporciona la opción de recuperación predeterminada cuando la base de datos no está disponible debido a una incidencia en la región de hospedaje. Puede restaurar la base de datos a un servidor de cualquier otra región. Hay un retraso entre el momento en que se realiza una copia de seguridad y el momento en que se replica geográficamente en un blob de Azure de una región diferente. Como resultado, la base de datos restaurada puede encontrarse hasta una hora por detrás de la base de datos original. En la siguiente ilustración se muestra la restauración de la base de datos a partir de la última copia de seguridad disponible en otra región.

![Restauración geográfica](./media/sql-database-geo-restore/geo-restore-2.png)

### <a name="geo-restore-using-azure-portal"></a>Restauración geográfica mediante Azure Portal

El concepto general de restauración geográfica de una base de datos de Azure Portal se realiza mediante la creación de una base de datos única o de instancia administrada y mediante la selección de una copia de seguridad de restauración geográfica disponible en la pantalla de creación de base de datos. La base de datos recién creada contendrá los datos de la copia de seguridad de la restauración geográfica.

#### <a name="single-azure-sql-database"></a>Base de datos única de Azure SQL Database

Para realizar una restauración geográfica de una base de datos única de Azure SQL Database de Azure Portal en la región y el servidor de su elección, siga estos pasos:

1. Haga clic en **+Agregar** en Marketplace y seleccione **Crear base de datos SQL** y rellene la información necesaria en la **pestaña Datos básicos**
2. Seleccione la pestaña **Configuración adicional**.
3. En Usar datos existentes, haga clic en **Copia de seguridad**.
4. Seleccione una copia de seguridad en la lista desplegable de copias de seguridad disponibles de restauración geográfica

    ![Restauración geográfica de una base de datos única de Azure SQL Database](./media/sql-database-recovery-using-backups/geo-restore-azure-sql-database-list-annotated.png)

Complete el proceso de creación de una nueva base de datos. Una vez creada, contendrá la copia de seguridad restaurada geográficamente.

#### <a name="managed-instance-database"></a>Base de datos de Instancia administrada

Para realizar la restauración geográfica de una base de datos de instancia administrada de Azure Portal en una instancia administrada ya existente de una región de su elección, seleccione la instancia administrada en la que desea que se restaure la base de datos y siga estos pasos:

1. Haga clic en **+Nueva base de datos**.
2. Escriba el nombre que desee para la base de datos.
3. En Usar datos existentes, seleccione la opción **Copia de seguridad**.
4. Seleccione una copia de seguridad en la lista desplegable de copias de seguridad disponibles de restauración geográfica

    ![Base de datos de instancia administrada con restauración geográfica](./media/sql-database-recovery-using-backups/geo-restore-sql-managed-instance-list-annotated.png)

Complete el proceso de creación de una nueva base de datos. Una vez creada la base de datos de instancia, contendrá la copia de seguridad restaurada geográficamente.

### <a name="geo-restore-using-powershell"></a>Restauración geográfica mediante PowerShell

#### <a name="single-azure-sql-database"></a>Base de datos única de Azure SQL Database

Para ver un script de PowerShell que indica cómo realizar la restauración geográfica de una base de datos única de Azure SQL Database, consulte [Uso de PowerShell para restaurar una base de datos única de Azure SQL a un momento anterior](scripts/sql-database-restore-database-powershell.md).

#### <a name="managed-instance-database"></a>Base de datos de Instancia administrada

Para ver un script de PowerShell que indica cómo realizar la restauración geográfica de una base de datos de instancia administrada, consulte [Uso de PowerShell para restaurar una base de datos de Instancia administrada en otra región con replicación geográfica](scripts/sql-managed-instance-restore-geo-backup.md).

### <a name="geo-restore-considerations"></a>Consideraciones sobre la restauración geográfica

Actualmente no se admite la restauración a un momento dad en una base de datos geográfica secundaria. La restauración a un momento dado solo puede realizarse en una base de datos principal. Para obtener información detallada sobre cómo usar la restauración geográfica a fin de recuperarse de una interrupción, consulte [Restauración de una base de datos SQL de Azure o una conmutación por error en una secundaria](sql-database-disaster-recovery.md).

> [!IMPORTANT]
> La restauración geográfica es la solución de recuperación ante desastres más básica que hay disponible en SQL Database. Depende de copias de seguridad con replicación geográfica creadas automáticamente con RPO = 1 hora y el tiempo de recuperación estimado de hasta 12 horas. No garantiza que la región de destino tenga la capacidad de restaurar sus bases de datos después de una interrupción regional porque es probable que se produzca un fuerte aumento de la demanda. Para aplicaciones críticas (no necesariamente para empresas) que usan bases de datos relativamente pequeñas, la restauración geográfica es una solución de recuperación ante desastres adecuada. En el caso de las aplicaciones críticas para la empresa que usan bases de datos grandes y deben garantizar la continuidad empresarial, debe usar [grupos de conmutación por error automática](sql-database-auto-failover-group.md). Ofrecen un RPO y RTO mucho menores y la capacidad siempre está garantizada. Para más información sobre las opciones de continuidad empresarial, vea [Introducción a la continuidad empresarial](sql-database-business-continuity.md).

## <a name="programmatically-performing-recovery-using-automated-backups"></a>Recuperación mediante programación con copias de seguridad automatizadas

Como se dijo anteriormente, además de Azure Portal, la recuperación de una base de datos se puede realizar mediante programación con Azure PowerShell o la API REST. En las tablas siguientes se describe el conjunto de comandos disponibles.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager de PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos en el módulo Az y en los módulos AzureRm son en gran medida idénticos.

#### <a name="single-azure-sql-database"></a>Base de datos única de Azure SQL Database

- Para restaurar una base de datos independiente o agrupada, vea [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase).

  | Cmdlet | DESCRIPCIÓN |
  | --- | --- |
  | [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) |Obtiene una o más bases de datos. |
  | [Get-AzSqlDeletedDatabaseBackup](/powershell/module/az.sql/get-azsqldeleteddatabasebackup) | Obtiene una base de datos eliminada que se puede restaurar. |
  | [Get-AzSqlDatabaseGeoBackup](/powershell/module/az.sql/get-azsqldatabasegeobackup) |Obtiene una copia de seguridad con redundancia geográfica de una base de datos. |
  | [Restore-AzSqlDatabase](/powershell/module/az.sql/restore-azsqldatabase) |Restaura una Base de datos SQL. |

  > [!TIP]
  > Para obtener un script de PowerShell de ejemplo que muestre cómo realizar una restauración a un momento dado, vea [Restauración de una instancia de SQL Database con PowerShell](scripts/sql-database-restore-database-powershell.md).

#### <a name="managed-instance-database"></a>Base de datos de Instancia administrada

- Para restaurar una base de datos de instancia administrada, vea [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase).

  | Cmdlet | DESCRIPCIÓN |
  | --- | --- |
  | [Get-AzSqlInstance](/powershell/module/az.sql/get-azsqlinstance) |Obtiene una o más instancias administradas. |
  | [Get-AzSqlInstanceDatabase](/powershell/module/az.sql/get-azsqlinstancedatabase) | Obtiene una base de datos de instancias. |
  | [Restore-AzSqlInstanceDatabase](/powershell/module/az.sql/restore-azsqlinstancedatabase) |Restaura una base de datos de una instancia. |

### <a name="rest-api"></a>API DE REST

Para restaurar una base de datos única o agrupada mediante la API REST, consulte la siguiente tabla:

| API | DESCRIPCIÓN |
| --- | --- |
| [REST (createMode=Recovery)](https://docs.microsoft.com/rest/api/sql/databases) |Restaura una base de datos. |
| [Obtener el estado de creación o actualización de la base de datos](https://docs.microsoft.com/rest/api/sql/operations) |Devuelve el estado durante una operación de restauración. |

### <a name="azure-cli"></a>CLI de Azure

#### <a name="single-azure-sql-database"></a>Base de datos única de Azure SQL Database

Para restaurar una base de datos única o agrupada mediante la CLI de Azure, consulte la referencia sobre el comando [az sql db restore](/cli/azure/sql/db#az-sql-db-restore).

#### <a name="managed-instance-database"></a>Base de datos de Instancia administrada

Para restaurar una base de datos de instancia administrada mediante la CLI de Azure, consulte [az sql midb restore](/cli/azure/sql/midb#az-sql-midb-restore)

## <a name="summary"></a>Resumen

Las copias de seguridad automáticas protegen las bases de datos de los errores de usuario y de aplicación, la eliminación accidental de la base de datos y las interrupciones prolongadas. Esta funcionalidad integrada está disponible para todos los niveles de servicio y tamaños de proceso.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener una descripción general y los escenarios de la continuidad empresarial, consulte [Información general sobre la continuidad empresarial](sql-database-business-continuity.md).
- Para saber en qué consisten las copias de seguridad automatizadas de Azure SQL Database, consulte [Información general: copias de seguridad automatizadas de SQL Database](sql-database-automated-backups.md).
- Para obtener más información sobre la retención de copia de seguridad a largo plazo, lea sobre la [retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md).
- Para conocer las opciones de recuperación más rápidas, consulte el artículo sobre la [replicación geográfica activa](sql-database-active-geo-replication.md) y los [grupos de conmutación por error automáticos](sql-database-auto-failover-group.md).
