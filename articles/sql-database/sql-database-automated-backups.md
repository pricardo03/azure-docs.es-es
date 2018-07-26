---
title: Copias de seguridad automáticas con redundancia geográfica de Azure SQL Database | Microsoft Docs
description: SQL Database crea automáticamente una copia de seguridad local de la base de datos cada pocos minutos y usa almacenamiento con redundancia geográfica con acceso de lectura de Azure para proporcionar redundancia geográfica.
services: sql-database
author: anosov1960
manager: craigg
ms.service: sql-database
ms.custom: business continuity
ms.topic: conceptual
ms.workload: Active
ms.date: 07/18/2018
ms.author: sashan
ms.reviewer: carlrab
ms.openlocfilehash: cedad5f48769ed864fef10cfd7059111a4502fd3
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136611"
---
# <a name="learn-about-automatic-sql-database-backups"></a>Más información sobre copias de seguridad automáticas de SQL Database

SQL Database crea automáticamente copias de seguridad de la base de datos y usa almacenamiento con redundancia geográfica con acceso de lectura de Azure (RA-GRS) para ofrecer redundancia geográfica. Estas copias de seguridad se crean automáticamente y sin cargos adicionales. No es necesario hacer nada para que se produzcan. Las copias de seguridad de base de datos son una parte esencial de cualquier estrategia de recuperación ante desastres y continuidad del negocio, ya que protegen los datos de daños o eliminaciones accidentales. Si las reglas de seguridad requieren que las copias de seguridad estén disponibles durante un largo período de tiempo, puede configurar una directiva de retención de copia de seguridad a largo plazo. Para más información, consulte [retención a largo plazo](sql-database-long-term-retention.md).

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="what-is-a-sql-database-backup"></a>¿Qué es una copia de seguridad de SQL Database?

SQL Database emplea tecnología de SQL Server para crear copias de seguridad [completas](https://msdn.microsoft.com/library/ms186289.aspx), [diferenciales](http://msdn.microsoft.com/library/ms175526.aspx) y de [registro de transacciones](https://msdn.microsoft.com/library/ms191429.aspx) para la restauración a un momento dado. Por lo general, las copias de seguridad del registro de transacciones se producen cada 5-10 minutos y las copias de seguridad diferenciales cada 12 horas, la frecuencia depende del nivel de rendimiento y de la cantidad de actividad de la base de datos. Las copias del registro de transacciones, junto con las completas y diferenciales, permiten restaurar una base de datos a un momento específico en el mismo servidor que hospede la base de datos. Cuando se restaura una base de datos, el servicio calcula qué copia de seguridad completa, diferencial o del registro de transacciones es necesario restaurar.


Puede utilizar estas copias de seguridad para realizar lo siguiente:

* Restaurar una base de datos a un momento dado dentro del período de retención. Esta operación creará una base de datos nueva en el mismo servidor de la base de datos original.
* Restaurar una base de datos eliminada al momento en que se eliminó o a cualquier otro punto comprendido en el periodo de retención. La base de datos eliminada solo se puede restaurar en el mismo servidor donde se creara la original.
* Restaurar una base de datos en otra región geográfica. Esta opción permite recuperarse de un desastre en una región geográfica cuando no puede acceder a su servidor y base de datos. Crea una nueva base de datos en cualquier servidor existente del mundo. 
* Restaure una base de datos desde una copia de seguridad a largo plazo específica si la base de datos se ha configurado con una directiva de retención a largo plazo (LTR). Esta opción permite restaurar una versión antigua de la base de datos para respetar una solicitud de cumplimiento o ejecutar una versión antigua de la aplicación. Consulte el tema sobre la [retención a largo plazo](sql-database-long-term-retention.md).
* Para llevar a cabo una restauración, consulte el artículo sobre la [restauración de bases de datos a partir de copias de seguridad](sql-database-recovery-using-backups.md).

> [!NOTE]
> En Azure Storage, el término *replicación* hace referencia a la copia de archivos desde una ubicación a otra. La *replicación de base de datos* de SQL hace referencia al mantenimiento de varias bases de datos secundarias sincronizadas con una base de datos principal. 
> 

## <a name="how-long-are-backups-kept"></a>¿Cuánto tiempo se conservan las copias de seguridad?
Cada copia de seguridad de SQL Database tiene un período de retención predeterminado que se basa en el nivel de servicio de la base de datos y es diferente al [modelo de compra basado en DTU](sql-database-service-tiers-dtu.md) y al [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md). Puede actualizar el período de retención de copia de seguridad de una base de datos. Vea [Cambiar el período de retención de copia de seguridad](#how-to-change-backup-retention-period) para obtener más detalles.

Si elimina una base de datos, SQL Database mantendrá las copias de seguridad de la misma manera que para una base de datos en línea. Por ejemplo, si elimina una base de datos básica que tiene un período de retención de siete días, una copia de seguridad con cuatro días de antigüedad se guarda durante tres días más.

Si tiene que conservar las copias de seguridad durante más tiempo que el período de retención PITR máximo, puede modificar las propiedades de copia de seguridad para agregar uno o varios períodos de retención a largo plazo a la base de datos. Vea [Retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md) para obtener más detalles.

> [!IMPORTANT]
> Si elimina el servidor de Azure SQL que hospeda las bases de datos SQL, todos los grupos elásticos y las bases de datos que pertenecen al servidor también se eliminan y no se pueden recuperar. No puede restaurar un servidor eliminado. Pero si ha configurado la retención a largo plazo, no se eliminarán las copias de seguridad de las bases de datos con LTR y estas bases de datos se pueden restaurar.

### <a name="pitr-retention-for-dtu-based-service-tiers"></a>Retención PITR para niveles de servicio basados en DTU
El período de retención predeterminado de una base de datos creada mediante el modelo de compra basado en DTU depende del nivel de servicio:

* El nivel de servicio Básico es de 1 semana.
* El nivel de servicio Estándar es de 5 semanas.
* El nivel de servicio Premium es de 5 semanas.

Si reduce el período de retención PITR actual, todas las copias de seguridad existentes anteriores al período de retención nuevo ya no estarán disponibles. 

Si aumenta el período de retención PITR actual, SQL Database mantendrá las copias de seguridad existentes hasta que se alcance el período de retención más largo.

## <a name="how-often-do-backups-happen"></a>¿Con qué frecuencia se producen las copias de seguridad?
### <a name="backups-for-point-in-time-restore"></a>Copias de seguridad para la restauración a un momento dado
SQL Database admite el autoservicio de restauración a un momento dado (PITR) mediante la creación automática de copias de seguridad completas, copias de seguridad diferenciales y copias de seguridad de registro de transacciones. Las copias de seguridad de la base de datos completas se crean semanalmente, las copias de seguridad de la base de datos diferenciales generalmente se crean cada 12 horas y las copias de seguridad del registro de transacciones generalmente se crean cada 5-10 minutos, la frecuencia se basa en el nivel de rendimiento y la cantidad de actividad de la base de datos. La primera copia de seguridad completa se programa inmediatamente después de la creación de la base de datos. Normalmente, se completa en 30 minutos pero puede tardar más si la base de datos tiene un tamaño considerable. Por ejemplo, la copia de seguridad inicial puede tardar más en una base de datos restaurada o una copia de la base de datos. Después de la primera copia de seguridad completa, todas las copias de seguridad adicionales se programan automáticamente y se administran silenciosamente en segundo plano. El servicio SQL Database determina el momento exacto en el que se producen todas las copias de seguridad de la base de datos a medida que equilibra la carga de trabajo global del sistema.

Las copias de seguridad PITR tienen redundancia geográfica y se protegen mediante la [replicación entre regiones de Azure Storage](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage)

Para más información, consulte [Restauración a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore).

### <a name="backups-for-long-term-retention"></a>Copias de seguridad para retención a largo plazo
SQL Database ofrece la opción de configurar la retención a largo plazo (LTR) de las copias de seguridad completas durante un período máximo de 10 años. Si la directiva LTR está habilitada, las copias de seguridad completas semanales se copian de forma automática en otro contenedor de almacenamiento de RA-GRS. Para satisfacer los distintos requisitos de cumplimiento, puede seleccionar distintos períodos de retención para copias de seguridad semanales, mensuales o anuales. El consumo de almacenamiento depende de la frecuencia seleccionada de las copias de seguridad y de los períodos de retención. Para estimar el costo del almacenamiento de LTR, se puede usar la [calculadora de precios de LTR](https://azure.microsoft.com/pricing/calculator/?service=sql-database). 

Como sucede con PITR, las copias de seguridad LTR tienen redundancia geográfica y se protegen mediante la [replicación entre regiones de Azure Storage](../storage/common/storage-redundancy-grs.md#read-access-geo-redundant-storage).

Para obtener más información, vea [Retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md).

## <a name="are-backups-encrypted"></a>¿Se cifran las copias de seguridad?

Si la base de datos se cifra con TDE, las copias de seguridad se cifran de forma automática en reposo, incluidas las copias de seguridad LTR. Cuando TDE está habilitado para una instancia de Azure SQL Database, también se cifran las copias de seguridad. Todas las instancias nuevas de Azure SQL Database están configuradas con TDE habilitado de forma predeterminada. Para obtener más información sobre TDE, vea [Cifrado de datos transparente con Azure SQL Database](/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql).

## <a name="how-do-automated-backups-impact-my-compliance"></a>¿Cómo afectan las copias de seguridad automatizadas al cumplimiento?

Al migrar la base de datos de un nivel de servicio basado en DTU con la retención PITR predeterminada de 35 días a un nivel de servicio basado en núcleos virtuales, se conserva la retención PITR para garantizar que la directiva de recuperación de datos de la aplicación no se pone en peligro. Si el período de retención predeterminado no satisface los requisitos de cumplimiento, puede cambiar el período de retención PITR con PowerShell o la API REST. Vea [Cambiar el período de retención de copia de seguridad](#how-to-change-backup-retention-period) para obtener más detalles.

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="how-to-change-backup-retention-period"></a>Cómo cambiar el período de retención de las copias de seguridad
La retención predeterminada se puede cambiar mediante la API REST o PowerShell. Los valores admitidos son: 7, 14, 21, 28 o 35 días. En los ejemplos siguientes se muestra cómo cambiar la retención PITR a 28 días. 

> [!NOTE]
> Las API solo afectarán al período de retención PITR. Si ha configurado LTR para la base de datos, no se verá afectada. Vea [Retención de copias de seguridad a largo plazo](sql-database-long-term-retention.md) para obtener más información sobre cómo cambiar los períodos de retención LTR.

### <a name="change-pitr-backup-retention-period-using-powershell"></a>Cambiar el período de retención de copia de seguridad PITR mediante PowerShell
```powershell
Set-AzureRmSqlDatabaseBackupShortTermRetentionPolicy -ResourceGroupName resourceGroup -ServerName testserver -DatabaseName testDatabase -RetentionDays 28
```
> [!IMPORTANT]
> Esta API se incluye en el módulo AzureRM.Sql de PowerShell a partir de la versión [4.7.0-preview](https://www.powershellgallery.com/packages/AzureRM.Sql/4.7.0-preview). 

### <a name="change-pitr-retention-period-using-rest-api"></a>Cambio del período de retención PITR mediante la API REST
**Solicitud de ejemplo**
```http
PUT https://management.azure.com/subscriptions/00000000-1111-2222-3333-444444444444/resourceGroups/resourceGroup/providers/Microsoft.Sql/servers/testserver/databases/testDatabase/backupShortTermRetentionPolicies/default?api-version=2017-10-01-preview
```
**Cuerpo de la solicitud**
```json
{
  "properties":{  
      "retentionDays":28
   }
}
```
**Respuesta de ejemplo**

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
Vea [API REST de retención de copias de seguridad](https://docs.microsoft.com/rest/api/sql/backupshorttermretentionpolicies) para obtener más detalles.

## <a name="next-steps"></a>Pasos siguientes

- Las copias de seguridad de base de datos son una parte esencial de cualquier estrategia de recuperación ante desastres y continuidad del negocio, ya que protegen los datos de daños o eliminaciones accidentales. Para descubrir otras soluciones de continuidad empresarial de Azure SQL Database, consulte el artículo de [información general sobre la continuidad empresarial](sql-database-business-continuity.md).
- Para restaurar a un momento dado mediante Azure Portal, consulte [Restauración de una base de datos SQL de Azure a un momento dado anterior con Azure Portal](sql-database-recovery-using-backups.md).
- Para restaurar a un momento dado mediante PowerShell, consulte [Restauración de una base de datos SQL de Azure a un momento dado anterior con PowerShell](scripts/sql-database-restore-database-powershell.md).
- Para configurar, administrar y restaurar desde la retención a largo plazo de copias de seguridad automatizadas en un almacén de Azure Recovery Services mediante Azure Portal, consulte [Administración de la retención de copia de seguridad a largo plazo mediante Azure Portal](sql-database-long-term-backup-retention-configure.md).
- Para configurar, administrar y restaurar desde la retención a largo plazo de copias de seguridad automatizadas en un almacén de Azure Recovery Services mediante PowerShell, consulte [Administración de la retención de copia de seguridad a largo plazo mediante PowerShell](sql-database-long-term-backup-retention-configure.md).
