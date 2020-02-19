---
title: Copias de seguridad automáticas con redundancia geográfica
description: SQL Database crea automáticamente una copia de seguridad local de la base de datos cada pocos minutos y usa almacenamiento con redundancia geográfica con acceso de lectura de Azure para proporcionar redundancia geográfica.
services: sql-database
ms.service: sql-database
ms.subservice: backup-restore
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab, danil
manager: craigg
ms.date: 12/13/2019
ms.openlocfilehash: f460bc3e4809b8a1cbabe1161c888255a7a484db
ms.sourcegitcommit: 76bc196464334a99510e33d836669d95d7f57643
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77157523"
---
# <a name="automated-backups"></a>Copias de seguridad automatizadas

SQL Database crea automáticamente las copias de seguridad de base de datos que se conservan durante el período de retención configurado y usa el [almacenamiento con redundancia geográfica de acceso de lectura (RA-GRS)](../storage/common/storage-redundancy.md) de Azure para asegurarse de que se conservan incluso si el centro de datos no está disponible. Estas copias de seguridad se crean automáticamente. Las copias de seguridad de base de datos son una parte esencial de cualquier estrategia de recuperación ante desastres y continuidad del negocio, ya que protegen los datos de daños o eliminaciones accidentales. Si las reglas de seguridad exigen que las copias de seguridad estén disponibles durante un largo período de tiempo (hasta 10 años), puede configurar una [retención a largo plazo](sql-database-long-term-retention.md) en bases de datos singleton y grupos elásticos.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>Qué es una copia de seguridad de SQL Database

SQL Database emplea tecnología de SQL Server para crear [copias de seguridad completas](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server) cada semana, [copias de seguridad diferenciales](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) cada 12 horas y [copias de seguridad del registro de transacciones](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) cada 5 o 10 minutos. Las copias de seguridad se almacenan en [blobs de almacenamiento RA-GRS](../storage/common/storage-redundancy.md) que se replican en un [centro de datos emparejado](../best-practices-availability-paired-regions.md) con el fin de brindar protección frente a interrupciones en el centro de datos. Cuando se restaura una base de datos, el servicio calcula qué copia de seguridad completa, diferencial o del registro de transacciones es necesario restaurar.

Puede utilizar estas copias de seguridad para realizar lo siguiente:

- **Restaurar una base de datos existente a un momento en el tiempo pasado** dentro del período de retención mediante Azure Portal, Azure PowerShell, la CLI de Azure o una API REST. En bases de datos singleton y grupos elásticos, esta operación creará una base de datos en el mismo servidor que la base de datos original. En instancias administradas, esta operación puede crear una copia de la base de datos o una instancia administrada igual o diferente en la misma suscripción.
- **Restaurar una base de datos eliminada al momento en que se eliminó** o a cualquier otro punto dentro del periodo de retención. La base de datos eliminada solo se puede restaurar en el mismo servidor local o instancia administrada donde se creó la base de datos original.
- **Restaurar una base de datos en otra región geográfica**. La restauración geográfica le permite recuperarse de un desastre en una región geográfica cuando no puede acceder a su servidor y base de datos. Crea una nueva base de datos en cualquier servidor existente del mundo.
- **Restaurar una base de datos desde una copia de seguridad a largo plazo específica** en bases de datos singleton y grupos elásticos si la base de datos se ha configurado con una directiva de retención a largo plazo (LTR). LTR permite restaurar una versión antigua de la base de datos con [Azure Portal](sql-database-long-term-backup-retention-configure.md#using-azure-portal) o [Azure PowerShell](sql-database-long-term-backup-retention-configure.md#using-powershell) para respetar una solicitud de cumplimiento o ejecutar una versión antigua de la aplicación. Para más información, consulte [retención a largo plazo](sql-database-long-term-retention.md).
- Para llevar a cabo una restauración, consulte el artículo sobre la [restauración de bases de datos a partir de copias de seguridad](sql-database-recovery-using-backups.md).

> [!NOTE]
> En Azure Storage, el término *replicación* hace referencia a la copia de archivos desde una ubicación a otra. La *replicación de base de datos* de SQL hace referencia al mantenimiento de varias bases de datos secundarias sincronizadas con una base de datos principal.

Puede probar algunas de estas operaciones con los ejemplos siguientes:

| | El Portal de Azure | Azure PowerShell |
|---|---|---|
| Cambio del período de retención de copia de seguridad | [Base de datos única](sql-database-automated-backups.md?tabs=managed-instance#change-pitr-backup-retention-period-using-azure-portal) <br/> [Instancia administrada](sql-database-automated-backups.md?tabs=managed-instance#change-pitr-backup-retention-period-using-azure-portal) | [Base de datos única](sql-database-automated-backups.md#change-pitr-backup-retention-period-using-powershell) <br/>[Instancia administrada](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlinstancedatabasebackupshorttermretentionpolicy) |
| Cambio del período de retención de copia de seguridad a largo plazo | [Base de datos única](sql-database-long-term-backup-retention-configure.md#configure-long-term-retention-policies)<br/>Instancia administrada: N/D  | [Base de datos única](sql-database-long-term-backup-retention-configure.md)<br/>Instancia administrada: N/D  |
| Restauración de la base de datos a partir de un momento en el tiempo | [Base de datos única](sql-database-recovery-using-backups.md#point-in-time-restore) | [Base de datos única](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqldatabase) <br/> [Instancia administrada](https://docs.microsoft.com/powershell/module/az.sql/restore-azsqlinstancedatabase) |
| Restaurar la base de datos eliminada | [Base de datos única](sql-database-recovery-using-backups.md) | [Base de datos única](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeleteddatabasebackup) <br/> [Instancia administrada](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldeletedinstancedatabasebackup)|
| Restauración de la base de datos desde Azure Blob Storage | Base de datos única: N/D <br/>Instancia administrada: N/D  | Base de datos única: N/D <br/>[Instancia administrada](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-get-started-restore) |


## <a name="backup-frequency"></a>Frecuencia de copia de seguridad

### <a name="point-in-time-restore"></a>Restauración a un momento dado

SQL Database admite el autoservicio de restauración a un momento dado (PITR) mediante la creación automática de copias de seguridad completas, copias de seguridad diferenciales y copias de seguridad de registro de transacciones. Las copias de seguridad de la base de datos completas se crean todas las semanas, las copias de seguridad de la base de datos diferenciales se suelen crear cada 12 horas y las copias de seguridad del registro de transacciones, cada 5-10 minutos; la frecuencia se basa en el tamaño de proceso y la cantidad de actividad de la base de datos. La primera copia de seguridad completa se programa inmediatamente después de la creación de la base de datos. Normalmente, se completa en 30 minutos pero puede tardar más si la base de datos tiene un tamaño considerable. Por ejemplo, la copia de seguridad inicial puede tardar más en una base de datos restaurada o una copia de la base de datos. Después de la primera copia de seguridad completa, todas las copias de seguridad adicionales se programan automáticamente y se administran silenciosamente en segundo plano. El servicio SQL Database determina el momento exacto en el que se producen todas las copias de seguridad de la base de datos a medida que equilibra la carga de trabajo global del sistema. No se pueden cambiar o deshabilitar los trabajos de copia de seguridad.

Las copias de seguridad de PITR están protegidas con almacenamiento con redundancia geográfica. Para obtener más información, consulte [Redundancia de Azure Storage](../storage/common/storage-redundancy.md).

Para más información, consulte [Restauración a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore).

### <a name="long-term-retention"></a>Retención a largo plazo

Las bases de datos únicas y agrupadas le ofrecen la opción de configurar la retención a largo plazo (LTR) de las copias de seguridad completas durante un período máximo de 10 años en Azure Blob Storage. Si la directiva LTR está habilitada, las copias de seguridad completas semanales se copian de forma automática en otro contenedor de almacenamiento de RA-GRS. Para satisfacer los distintos requisitos de cumplimiento, puede seleccionar distintos períodos de retención para copias de seguridad semanales, mensuales o anuales. El consumo de almacenamiento depende de la frecuencia seleccionada de las copias de seguridad y de los períodos de retención. Para estimar el costo del almacenamiento de LTR, se puede usar la [calculadora de precios de LTR](https://azure.microsoft.com/pricing/calculator/?service=sql-database).

Al igual que PITR, las copias de seguridad LTR están protegidas con almacenamiento con redundancia geográfica. Para obtener más información, consulte [Redundancia de Azure Storage](../storage/common/storage-redundancy.md).

Para obtener más información, vea [Retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md).

## <a name="backup-storage-consumption"></a>Consumo del almacenamiento de copia de seguridad 

En el caso de las bases de datos únicas, el uso del almacenamiento de copia de seguridad total se calcula de la siguiente manera:   
`Total backup storage size = (size of full backups + size of differential backups + size of log backups) – database size`.

En el caso de los grupos elásticos, el tamaño total del almacenamiento de copia de seguridad se agrega en el nivel de grupo y se calcula de la siguiente manera:   
`Total backup storage size = (total size of all full backups + total size of all differential backups + total size of all log backups) - allocated pool data storage`. 

Las copias de seguridad anteriores al período de retención se depuran automáticamente en función de su marca de tiempo. Dado que las copias de seguridad diferenciales y las copias de seguridad de registros necesitan una copia de seguridad completa anterior para ser útiles, se depuran en fragmentos semanales. 

Azure SQL Database calculará el almacenamiento de copia de seguridad total en retención como un valor acumulativo. Cada hora, este valor se envía a la canalización de facturación de Azure, que se encarga de agregar este uso por hora para calcular el consumo al final de cada mes. Después de quitar la base de datos, el consumo disminuye a medida que aumenta la antigüedad de las copias de seguridad. En el momento en que estas tienen una antigüedad superior al período de retención, la facturación se detiene. 


### <a name="monitoring-consumption"></a>Supervisar el consumo

Cada tipo de copia de seguridad (completa, diferencial y de registro) se notifica en la hoja de supervisión de la base de datos como una métrica independiente. En este diagrama se muestra cómo supervisar el consumo de almacenamiento de las copias de seguridad.  

![Supervisión del consumo de copia de seguridad en la hoja de supervisión de la base de datos de Azure Portal](media/sql-database-automated-backup/backup-metrics.png)

### <a name="fine-tune-the-backup-storage-consumption"></a>Ajustar el consumo del almacenamiento de copia de seguridad

El exceso de consumo del almacenamiento de copia de seguridad dependerá de la carga de trabajo y el tamaño de las bases de datos individuales. Puede considerar la posibilidad de implementar algunas de las siguientes técnicas de optimización para reducir aún más el consumo de almacenamiento de copia de seguridad:

* Reduzca el [período de retención de copias de seguridad](#change-pitr-backup-retention-period-using-azure-portal) al mínimo posible para sus necesidades.
* Evite realizar operaciones de escritura de gran tamaño con más frecuencia de la necesaria, como recompilaciones de índice.
* En el caso de operaciones de carga de datos de gran tamaño, considere la posibilidad de usar [índices de almacén de columnas en clúster](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes), reducir el número de índices no agrupados y también tener en cuenta las operaciones de carga masiva con un recuento de filas de alrededor de un millón.
* En el nivel de servicio de uso general, el almacenamiento de datos aprovisionado es más económico que el precio del almacenamiento de copia de seguridad en exceso, debido a que los clientes con costos de almacenamiento de copia de seguridad de gran exceso pueden considerar aumentar el almacenamiento de datos para ahorrar en el almacenamiento de copia de seguridad.
* Use TempDB en la lógica de ETL para almacenar resultados temporales, en lugar de tablas permanentes (aplicable solo a instancias administradas).
* Considere la posibilidad de desactivar el cifrado de TDE para las bases de datos que no contengan información confidencial (por ejemplo, bases de datos de desarrollo o de prueba). Las copias de seguridad de las bases de datos no cifradas suelen comprimirse con una relación de compresión mayor.

> [!IMPORTANT]
> En el caso de las cargas de trabajo analíticas de almacenamiento de datos o data mart, se recomienda encarecidamente usar [índices de almacén de columnas agrupados](https://docs.microsoft.com/sql/database-engine/using-clustered-columnstore-indexes), reducir el número de índices no agrupados y también tener en cuenta las operaciones de carga masiva con el recuento de filas en torno a un millón, para reducir el consumo en exceso de almacenamiento de copia de seguridad.


## <a name="storage-costs"></a>Costos de almacenamiento


### <a name="dtu-model"></a>Modelo de DTU

No hay ningún cargo adicional por el almacenamiento de copia de seguridad para las bases de datos y los grupos elásticos mediante el modelo de DTU. 

### <a name="vcore-model"></a>Modelo de núcleos virtuales

Para bases de datos únicas, se ofrece una cantidad de almacenamiento de copia de seguridad mínimo igual al 100 % del tamaño de la base de datos sin costo adicional. En el caso de los grupos elásticos y las instancias administradas, se proporciona una cantidad mínima de almacenamiento de copia de seguridad igual al 100 % del almacenamiento de datos asignado para el tamaño de grupo o instancia, respectivamente, sin costo adicional. El consumo adicional de almacenamiento de copia de seguridad se cobrará en GB/mes. Este consumo adicional dependerá de la carga de trabajo y el tamaño de las bases de datos individuales.

Azure SQL Database calculará el almacenamiento de copia de seguridad total en retención como un valor acumulativo. Cada hora, este valor se envía a la canalización de facturación de Azure, que se encarga de agregar este uso por hora para obtener el consumo al final de cada mes. Después de quitar la base de datos, se reduce el consumo a medida que aumenta la antigüedad de las copias de seguridad. En el momento en que estas tienen una antigüedad superior al período de retención, la facturación se detiene. Dado que todas las copias de seguridad de registros y las copias de seguridad diferenciales se conservan durante el período de retención completo, las bases de datos que se modifican considerablemente tendrán mayores cargos de copia de seguridad. 

Supongamos que la base de datos ha acumulado 744 GB de almacenamiento de copia de seguridad y esta cantidad permanece constante durante todo un mes. Para convertir este consumo de almacenamiento acumulativo en un uso por hora, lo dividimos por 744,0 (31 días al mes * 24 horas al día). Por lo tanto, SQL DataBase informará de que la base de datos usó 1 GB de copia de seguridad de PITR cada hora. La facturación de Azure agregará esto y mostrará un uso de 744 GB para todo el mes y el costo basado en la tarifa de $/GB/mes en su región. 

Veamos ahora un ejemplo más complejo. Supongamos que la base de datos ha aumentado su retención hasta 14 días a mediados de mes y esto (hipotéticamente) da lugar a que el almacenamiento de copia de seguridad total se duplique a 1488 GB. SQL Database notificaría 1 GB de uso para las horas 1-372 y, después, notificaría el uso como 2 GB para las horas 373-744. Esto se sumaría en una factura final de 1116 GB/mes. 

Se puede usar el análisis de costos de suscripción de Azure para determinar el gasto actual en el almacenamiento de copia de seguridad.

![Análisis de costos del almacenamiento de copia de seguridad](./media/sql-database-automated-backup/check-backup-storage-cost-sql-mi.png)

Por ejemplo, para entender los costos de almacenamiento de copia de seguridad de la instancia administrada, vaya a su suscripción en Azure Portal y abra la hoja Análisis de costos. Seleccione la subcategoría de medidor **almacenamiento de copia de seguridad de PITR** para ver el costo actual de copia de seguridad y la previsión de cargos. También se pueden incluir otras subcategorías de medidor, como **instancia administrada de uso general: almacenamiento** o **instancia administrada de uso general: proceso gen5** para comparar el costo del almacenamiento de copia de seguridad con otras categorías de costos.

## <a name="backup-retention"></a>Retención de copias de seguridad

Todas las bases de datos de Azure SQL (únicas, agrupadas y de instancia administrada) tienen un período de retención de copias de seguridad predeterminado de **siete** días. Puede [cambiar el período de retención de copia de seguridad a 35 días máximo](#change-pitr-backup-retention-period).

Si elimina una base de datos, SQL Database mantendrá las copias de seguridad de la misma manera que para una base de datos en línea. Por ejemplo, si elimina una base de datos básica que tiene un período de retención de siete días, una copia de seguridad con cuatro días de antigüedad se guarda durante tres días más.

Si tiene que conservar las copias de seguridad durante más tiempo que el período de retención máximo, puede modificar las propiedades de copia de seguridad para agregar uno o varios períodos de retención a largo plazo a la base de datos. Para más información, consulte [retención a largo plazo](sql-database-long-term-retention.md).

> [!IMPORTANT]
> Si elimina el servidor de Azure SQL que hospeda las bases de datos SQL, todos los grupos elásticos y las bases de datos que pertenecen al servidor también se eliminan y no se pueden recuperar. No puede restaurar un servidor eliminado. Pero si ha configurado la retención a largo plazo, no se eliminarán las copias de seguridad de las bases de datos con LTR y estas bases de datos se pueden restaurar.

## <a name="encrypted-backups"></a>Copias de seguridad cifradas

Si la base de datos se cifra con TDE, las copias de seguridad se cifran de forma automática en reposo, incluidas las copias de seguridad LTR. Cuando TDE está habilitado para una base de datos de Azure SQL, también se cifran las copias de seguridad. Todas las nuevas bases de datos de Azure SQL están configuradas con TDE habilitado de forma predeterminada. Para obtener más información sobre TDE, vea [Cifrado de datos transparente con Azure SQL Database](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="backup-integrity"></a>Integridad de copia de seguridad

De forma continuada, el equipo de ingeniería de Azure SQL Database prueba automáticamente la restauración de las copias de seguridad automatizadas de las bases de datos que se encuentran en los servidores lógicos y los grupos elásticos (esta función no está disponible en Instancia administrada). Tras la restauración a un momento dado, las bases de datos también reciben comprobaciones de integridad mediante DBCC CHECKDB.

Instancia administrada realiza una copia de seguridad inicial automática con `CHECKSUM` de las bases de datos restauradas mediante el comando `RESTORE` nativo o el servicio de migración de datos una vez finalizada la migración.

Los problemas encontrados durante la comprobación de integridad producen una alerta para el equipo de ingeniería. Para más información acerca de la integridad de los datos en Azure SQL Database, consulte [Data Integrity in Azure SQL Database](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/) (Integridad de datos en Azure SQL Database).

## <a name="compliance"></a>Cumplimiento normativo

Al migrar la base de datos de un nivel de servicio basado en DTU a un nivel de servicio basado en núcleos virtuales, se conserva la retención PITR para garantizar que la directiva de recuperación de datos de la aplicación no se ponga en peligro. Si el período de retención predeterminado no satisface los requisitos de cumplimiento, puede cambiar el período de retención PITR con PowerShell o la API REST. Para más información, consulte [Cambio del período de retención de Azure Backup](#change-pitr-backup-retention-period).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="change-pitr-backup-retention-period"></a>Cambiar el período de retención de copia de seguridad de PITR

Puede cambiar el período de retención predeterminado de copia de seguridad de PITR mediante Azure Portal, PowerShell o la API REST. En los ejemplos siguientes se muestra cómo cambiar la retención PITR a 28 días.

> [!WARNING]
> Si reduce el período de retención actual, todas las copias de seguridad existentes anteriores al período de retención nuevo dejan de estar disponibles. Si aumenta el período de retención actual, SQL Database mantendrá las copias de seguridad existentes hasta que se alcance el nuevo período de retención.

> [!NOTE]
> Estas API solo afectarán al período de retención PITR. Si ha configurado LTR para la base de datos, no se verá afectada. Para más información sobre cómo cambiar los períodos de retención de LTR, consulte [Retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md).

### <a name="change-pitr-backup-retention-period-using-azure-portal"></a>Cambio del período de retención de copia de seguridad de PITR mediante Azure Portal

Para cambiar el periodo de retención de copia de seguridad de PITR mediante Azure Portal, vaya al objeto de servidor cuyo período de retención desea cambiar dentro del portal y, luego, seleccione la opción apropiada según el objeto de servidor que va a modificar.

#### <a name="single-database--elastic-pools"></a>[Grupos elásticos y base de datos única](#tab/single-database)

El cambio de la retención de copia de seguridad de PITR para las bases de datos únicas de Azure SQL Database solo se realiza en el nivel de servidor. Los cambios realizados en el nivel de servidor se aplican a las bases de datos de ese servidor. Para cambiar la recuperación a un momento dado para el servidor de Azure SQL Database en Azure Portal, vaya a la hoja de información general del servidor, haga clic en Administrar copias de seguridad en el menú de navegación y, a continuación, haga clic en Configurar retención en la barra de navegación.

![Cambio de PITR en Azure Portal](./media/sql-database-automated-backup/configure-backup-retention-sqldb.png)

#### <a name="managed-instance"></a>[Instancia administrada](#tab/managed-instance)

El cambio de la retención de copia de seguridad de recuperación a un momento dado para Instancia administrada de SQL Database se realiza en un nivel de base de datos individual. Para cambiar la retención de copia de seguridad de recuperación a un momento dado de una base de datos de instancia en Azure Portal, vaya a la hoja de información general de la base de datos individual y haga clic en Configurar retención de copias de seguridad en la barra de navegación.

![Cambio de PITR en Azure Portal](./media/sql-database-automated-backup/configure-backup-retention-sqlmi.png)

---

### <a name="change-pitr-backup-retention-period-using-powershell"></a>Cambiar el período de retención de copia de seguridad PITR mediante PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> El módulo de Azure Resource Manager para PowerShell todavía es compatible con Azure SQL Database, pero todo el desarrollo futuro se realizará para el módulo Az.Sql. Para estos cmdlets, consulte [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). Los argumentos para los comandos del módulo Az y los módulos AzureRm son esencialmente idénticos.

```powershell
Set-AzSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

### <a name="change-pitr-retention-period-using-rest-api"></a>Cambio del período de retención PITR mediante la API REST

#### <a name="sample-request"></a>Solicitud de ejemplo

```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```

#### <a name="request-body"></a>Cuerpo de la solicitud

```json
{
  "properties":{
    "retentionDays":28
  }
}
```

#### <a name="sample-response"></a>Respuesta de ejemplo

Código de estado: 200

```json
{
  "id": "/subscriptions/00000000-1111-2222-3333-444444444444/providers/Microsoft.Sql/resourceGroups/resourceGroup/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default",
  "name": "default",
  "type": "Microsoft.Sql/resourceGroups/servers/databases/backupShortTermRetentionPolicies",
  "properties": {
    "retentionDays": 28
  }
}
```

Para más información, consulte [API REST de retención de Backup](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies).

## <a name="next-steps"></a>Pasos siguientes

- Las copias de seguridad de base de datos son una parte esencial de cualquier estrategia de recuperación ante desastres y continuidad del negocio, ya que protegen los datos de daños o eliminaciones accidentales. Para descubrir otras soluciones de continuidad empresarial de Azure SQL Database, consulte el artículo de [información general sobre la continuidad empresarial](sql-database-business-continuity.md).
- Para restaurar a un momento dado mediante Azure Portal, consulte [Restauración de una base de datos SQL de Azure a un momento dado anterior con Azure Portal](sql-database-recovery-using-backups.md).
- Para restaurar a un momento dado mediante PowerShell, consulte [Restauración de una base de datos SQL de Azure a un momento dado anterior con PowerShell](scripts/sql-database-restore-database-powershell.md).
- Para configurar, administrar y restaurar desde la retención a largo plazo de copias de seguridad automatizadas en Azure Blob Storage mediante Azure Portal, consulte [Administración de la retención de copia de seguridad a largo plazo mediante Azure Portal](sql-database-long-term-backup-retention-configure.md).
- Para configurar, administrar y restaurar desde la retención a largo plazo de copias de seguridad automatizadas en Azure Blob Storage mediante PowerShell, consulte [Administración de la retención de copia de seguridad a largo plazo mediante PowerShell](sql-database-long-term-backup-retention-configure.md).
