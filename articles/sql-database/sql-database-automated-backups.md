---
title: Copias de seguridad automáticas con redundancia geográfica de Azure SQL Database | Microsoft Docs
description: SQL Database crea automáticamente una copia de seguridad local de la base de datos cada pocos minutos y usa almacenamiento con redundancia geográfica con acceso de lectura de Azure para proporcionar redundancia geográfica.
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
ms.date: 12/10/2018
ms.openlocfilehash: 0be1ddea4d5eaa253850ae640152b2538b39d0ca
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54035430"
---
# <a name="automated-backups"></a>Copias de seguridad automatizadas

SQL Database crea automáticamente copias de seguridad de base de datos que se conservan entre 7 y 35 días, y usa el almacenamiento con redundancia geográfica de acceso de lectura (RA-GRS) para asegurarse de que se conservan incluso si el centro de datos no está disponible. Estas copias de seguridad se crean automáticamente y sin cargos adicionales. No es necesario hacer nada para que tengan lugar y puede [cambiar el período de retención de Azure Backup](#how-to-change-the-pitr-backup-retention-period). Las copias de seguridad de base de datos son una parte esencial de cualquier estrategia de recuperación ante desastres y continuidad del negocio, ya que protegen los datos de daños o eliminaciones accidentales. Si las reglas de seguridad requieren que las copias de seguridad estén disponibles durante un largo período de tiempo (hasta diez años), puede configurar una directiva [de retención a largo plazo](sql-database-long-term-retention.md).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>Qué es una copia de seguridad de SQL Database

SQL Database emplea tecnología de SQL Server para crear copias de seguridad [completas](https://docs.microsoft.com/sql/relational-databases/backup-restore/full-database-backups-sql-server), [diferenciales](https://docs.microsoft.com/sql/relational-databases/backup-restore/differential-backups-sql-server) y de [registro de transacciones](https://docs.microsoft.com/sql/relational-databases/backup-restore/transaction-log-backups-sql-server) para la restauración a un momento dado. Por lo general, las copias de seguridad del registro de transacciones se producen cada 5-10 minutos y las copias de seguridad diferenciales cada 12 horas, la frecuencia depende del tamaño del proceso y de la cantidad de actividad de la base de datos. Las copias de seguridad completas, diferenciales y de registro de transacciones permiten restaurar una base de datos a un momento específico en el mismo servidor que hospeda la base de datos. Las copias de seguridad se almacenan en blobs de almacenamiento RA-GRS que se replican en un [centro de datos emparejado](../best-practices-availability-paired-regions.md) con el fin de brindar protección frente a interrupciones en el centro de datos. Cuando se restaura una base de datos, el servicio calcula qué copia de seguridad completa, diferencial o del registro de transacciones es necesario restaurar.

Puede utilizar estas copias de seguridad para realizar lo siguiente:

- Restaurar una base de datos a un momento dado dentro del período de retención. Esta operación creará una base de datos nueva en el mismo servidor de la base de datos original.
- Restaurar una base de datos eliminada al momento en que se eliminó o a cualquier otro punto comprendido en el periodo de retención. La base de datos eliminada solo se puede restaurar en el mismo servidor donde se creara la original.
- Restaurar una base de datos en otra región geográfica. La restauración geográfica le permite recuperarse de un desastre en una región geográfica cuando no puede acceder a su servidor y base de datos. Crea una nueva base de datos en cualquier servidor existente del mundo.
- Restaure una base de datos desde una copia de seguridad a largo plazo específica si la base de datos se ha configurado con una directiva de retención a largo plazo (LTR). LTR permite restaurar una versión antigua de la base de datos para respetar una solicitud de cumplimiento o ejecutar una versión antigua de la aplicación. Para más información, consulte [retención a largo plazo](sql-database-long-term-retention.md).
- Para llevar a cabo una restauración, consulte el artículo sobre la [restauración de bases de datos a partir de copias de seguridad](sql-database-recovery-using-backups.md).

> [!NOTE]
> En Azure Storage, el término *replicación* hace referencia a la copia de archivos desde una ubicación a otra. La *replicación de base de datos* de SQL hace referencia al mantenimiento de varias bases de datos secundarias sincronizadas con una base de datos principal.

## <a name="how-long-are-backups-kept"></a>Cuánto tiempo se conservan las copias de seguridad

Cada instancia de SQL Database tiene un período de retención de copia de seguridad predeterminado de entre siete y 35 días que depende del modelo de compra y el nivel de servicio. Puede actualizar el período de retención de copia de seguridad de una base de datos en un servidor lógico de Azure. Para más información, consulte [Cambio del período de retención de Azure Backup](#how-to-change-the-pitr-backup-retention-period).

Si elimina una base de datos, SQL Database mantendrá las copias de seguridad de la misma manera que para una base de datos en línea. Por ejemplo, si elimina una base de datos básica que tiene un período de retención de siete días, una copia de seguridad con cuatro días de antigüedad se guarda durante tres días más.

Si tiene que conservar las copias de seguridad durante más tiempo que el período de retención máximo, puede modificar las propiedades de copia de seguridad para agregar uno o varios períodos de retención a largo plazo a la base de datos. Para más información, consulte [retención a largo plazo](sql-database-long-term-retention.md).

> [!IMPORTANT]
> Si elimina el servidor de Azure SQL que hospeda las bases de datos SQL, todos los grupos elásticos y las bases de datos que pertenecen al servidor también se eliminan y no se pueden recuperar. No puede restaurar un servidor eliminado. Pero si ha configurado la retención a largo plazo, no se eliminarán las copias de seguridad de las bases de datos con LTR y estas bases de datos se pueden restaurar.

### <a name="default-backup-retention-period"></a>Período de retención predeterminado de la copia de seguridad

#### <a name="dtu-based-purchasing-model"></a>Modelo de compra basado en DTU

El período de retención predeterminado de una base de datos creada mediante el modelo de compra basado en DTU depende del nivel de servicio:

- El nivel de servicio Básico es de 1 semana.
- El nivel de servicio Estándar es de 5 semanas.
- El nivel de servicio Premium es de 5 semanas.

#### <a name="vcore-based-purchasing-model"></a>Modelo de compra basado en núcleos virtuales

Si va a usar el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md), el período de retención de copia de seguridad predeterminado es de 7 días (para bases de datos únicos, agrupadas y de Instancia administrada). Para todas las bases de datos Azure SQL (únicas, agrupadas y de Instancia administrada) puede [cambiar el período de retención de copia de seguridad hasta 35 días](#how-to-change-the-pitr-backup-retention-period).

> [!WARNING]
> Si reduce el período de retención actual, todas las copias de seguridad existentes anteriores al período de retención nuevo dejan de estar disponibles. Si aumenta el período de retención actual, SQL Database mantendrá las copias de seguridad existentes hasta que se alcance el nuevo período de retención.

## <a name="how-often-do-backups-happen"></a>Con qué frecuencia se producen las copias de seguridad

### <a name="backups-for-point-in-time-restore"></a>Copias de seguridad para la restauración a un momento dado

SQL Database admite el autoservicio de restauración a un momento dado (PITR) mediante la creación automática de copias de seguridad completas, copias de seguridad diferenciales y copias de seguridad de registro de transacciones. Las copias de seguridad de la base de datos completas se crean todas las semanas, las copias de seguridad de la base de datos diferenciales se suelen crear cada 12 horas y las copias de seguridad del registro de transacciones, cada 5-10 minutos; la frecuencia se basa en el tamaño del proceso y la cantidad de actividad de la base de datos. La primera copia de seguridad completa se programa inmediatamente después de la creación de la base de datos. Normalmente, se completa en 30 minutos pero puede tardar más si la base de datos tiene un tamaño considerable. Por ejemplo, la copia de seguridad inicial puede tardar más en una base de datos restaurada o una copia de la base de datos. Después de la primera copia de seguridad completa, todas las copias de seguridad adicionales se programan automáticamente y se administran silenciosamente en segundo plano. El servicio SQL Database determina el momento exacto en el que se producen todas las copias de seguridad de la base de datos a medida que equilibra la carga de trabajo global del sistema.

Las copias de seguridad PITR tienen redundancia geográfica y se protegen mediante la [replicación entre regiones de Azure Storage](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)

Para más información, consulte [Restauración a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore).

### <a name="backups-for-long-term-retention"></a>Copias de seguridad para retención a largo plazo

SQL Database hospedado en servidor lógico ofrece la opción de configurar la retención a largo plazo (LTR) de las copias de seguridad completas durante un período máximo de 10 años en Azure Blob Storage. Si la directiva LTR está habilitada, las copias de seguridad completas semanales se copian de forma automática en otro contenedor de almacenamiento de RA-GRS. Para satisfacer los distintos requisitos de cumplimiento, puede seleccionar distintos períodos de retención para copias de seguridad semanales, mensuales o anuales. El consumo de almacenamiento depende de la frecuencia seleccionada de las copias de seguridad y de los períodos de retención. Para estimar el costo del almacenamiento de LTR, se puede usar la [calculadora de precios de LTR](https://azure.microsoft.com/pricing/calculator/?service=sql-database).

Como sucede con PITR, las copias de seguridad LTR tienen redundancia geográfica y se protegen mediante la [replicación entre regiones de Azure Storage](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage).

Para obtener más información, vea [Retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md).

## <a name="are-backups-encrypted"></a>Cifrado de las copias de seguridad

Si la base de datos se cifra con TDE, las copias de seguridad se cifran de forma automática en reposo, incluidas las copias de seguridad LTR. Cuando TDE está habilitado para una instancia de Azure SQL Database, también se cifran las copias de seguridad. Todas las instancias nuevas de Azure SQL Database están configuradas con TDE habilitado de forma predeterminada. Para obtener más información sobre TDE, vea [Cifrado de datos transparente con Azure SQL Database](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="how-does-microsoft-ensure-backup-integrity"></a>Garantía de integridad de la copia de seguridad de Microsoft

El equipo de ingeniería de Azure SQL Database prueba permanentemente y de manera automática la restauración de las copias de seguridad automatizadas de las bases de datos del servicio. Tras la restauración, las bases de datos también reciben comprobaciones de integridad mediante DBCC CHECKDB. Los problemas encontrados durante la comprobación de integridad producen una alerta para el equipo de ingeniería. Para más información acerca de la integridad de los datos en Azure SQL Database, consulte [Data Integrity in Azure SQL Database](https://azure.microsoft.com/blog/data-integrity-in-azure-sql-database/) (Integridad de datos en Azure SQL Database).

## <a name="how-do-automated-backups-impact-compliance"></a>Cómo afectan las copias de seguridad automatizadas al cumplimiento

Al migrar la base de datos de un nivel de servicio basado en DTU con la retención PITR predeterminada de 35 días a un nivel de servicio basado en núcleos virtuales, se conserva la retención PITR para garantizar que la directiva de recuperación de datos de la aplicación no se pone en peligro. Si el período de retención predeterminado no satisface los requisitos de cumplimiento, puede cambiar el período de retención PITR con PowerShell o la API REST. Vea [Cambiar el período de retención de copia de seguridad](#how-to-change-the-pitr-backup-retention-period) para obtener más detalles.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="how-to-change-the-pitr-backup-retention-period"></a>Cómo cambiar el período de retención de las copias de seguridad de PITR

Puede cambiar el período de retención predeterminado de copia de seguridad de PITR mediante Azure Portal, PowerShell o la API REST. Los valores admitidos son: 7, 14, 21, 28 o 35 días. En los ejemplos siguientes se muestra cómo cambiar la retención PITR a 28 días.

> [!NOTE]
> Estas API solo afectarán al período de retención PITR. Si ha configurado LTR para la base de datos, no se verá afectada. Para más información sobre cómo cambiar los períodos de retención de LTR, consulte [Retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md).

### <a name="change-pitr-backup-retention-period-using-the-azure-portal"></a>Cambio del período de retención de copia de seguridad de PITR mediante Azure Portal

Para cambiar el periodo de retención de copia de seguridad de PITR mediante Azure Portal, vaya al objeto de servidor cuyo período de retención desea cambiar dentro del Portal y luego seleccione la opción apropiada basada en el objeto de servidor que se modifica.

#### <a name="change-pitr-for-a-logical-server"></a>Cambio de PITR para un servidor lógico

![Cambio de PITR en Azure Portal](./media/sql-database-automated-backup/configure-backup-retention-sqldb.png)

#### <a name="change-pitr-for-a-managed-instance"></a>Cambio de PITR de una Instancia administrada

![Cambio de PITR en Azure Portal](./media/sql-database-automated-backup/configure-backup-retention-sqlmi.png)

### <a name="change-pitr-backup-retention-period-using-powershell"></a>Cambiar el período de retención de copia de seguridad PITR mediante PowerShell

```powershell
Set-AzureRmSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```

> [!IMPORTANT]
> Esta API se incluye en el módulo AzureRM.Sql de PowerShell a partir de la versión [4.7.0-preview](https://www.powershellgallery.com/packages/AzureRM.Sql/4.7.0-preview).

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
