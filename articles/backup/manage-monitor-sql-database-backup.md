---
title: Administración y supervisión de bases de datos SQL Server en una máquina virtual de Azure con copia de seguridad mediante Azure Backup | Microsoft Docs
description: En este artículo se describe cómo restaurar bases de datos SQL Server que se ejecutan en una máquina virtual de Azure cuya copia de seguridad se realiza con Azure Backup.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 02/19/2018
ms.author: raynew
ms.openlocfilehash: 1c2ce0ba42f0bc3efd1dcc951113b05ab6941b98
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430959"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Administración y supervisión de bases de datos SQL Server con copia de seguridad 


En este artículo se describen las tareas comunes para administrar y supervisar bases de datos SQL Server que se ejecutan en una máquina virtual de Azure cuya copia de seguridad se ha realizado en un almacén de Azure Backup Recovery Services con el servicio [Azure Backup](backup-overview.md). Las tareas incluyen la supervisión de trabajos y alertas, la detención y reanudación de protección de bases de datos, la ejecución de trabajos de copia de seguridad y la anulación del registro de una máquina virtual de una copia de seguridad.


> [!NOTE]
> La copia de seguridad de bases de datos SQL Server que se ejecutan en una máquina virtual de Azure con Azure Backup está actualmente en versión preliminar pública.


Si aún no ha configurado la copia de seguridad de las bases de datos SQL Server, siga las instrucciones de [este artículo](backup-azure-sql-database.md).

## <a name="monitor-backup-jobs"></a>Supervisión de trabajos de copia de seguridad

###  <a name="monitor-ad-hoc-jobs-in-the-portal"></a>Supervisión de trabajos ad hoc en el portal

Azure Backup muestra todos los trabajos desencadenados manualmente en el portal de **trabajos de Backup**, como la detección y el registro de bases de datos y las operaciones de copia de seguridad y restauración.

![Portal de trabajos de copia de seguridad](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> Los trabajos de copia de seguridad programados no se muestran en el portal de **trabajos de Backup**. Use SQL Server Management Studio para supervisar los trabajos de copia de seguridad programados, como se describe en la sección siguiente.
>

### <a name="monitor-backup-jobs-with-sql-server-management-studio"></a>Supervisión de trabajos de copia de seguridad con SQL Server Management Studio 

Azure Backup utiliza API nativas de SQL para todas las operaciones de copia de seguridad.

Use las API nativas para capturar toda la información sobre trabajos de la [tabla del conjunto de copias de seguridad de SQL](https://docs.microsoft.com/sql/relational-databases/system-tables/backupset-transact-sql?view=sql-server-2017) en la base de datos msdb.

El ejemplo siguiente es una consulta para capturar todos los trabajos de copia de seguridad de una base de datos denominada **DB1**. Personalice la consulta para realizar una supervisión avanzada.

```
select CAST (
Case type
                when 'D' 
                                 then 'Full'
                when  'I'
                               then 'Differential' 
                ELSE 'Log'
                END         
                AS varchar ) AS 'BackupType',
database_name, 
server_name,
machine_name,
backup_start_date,
backup_finish_date,
DATEDIFF(SECOND, backup_start_date, backup_finish_date) AS TimeTakenByBackupInSeconds,
backup_size AS BackupSizeInBytes
  from msdb.dbo.backupset where user_name = 'NT SERVICE\AzureWLBackupPluginSvc' AND database_name =  <DB1>  

```

## <a name="view-backup-alerts"></a>Visualización de alertas de copia de seguridad

Dado que las copias de seguridad de registros se producen con una frecuencia de 15 minutos, la supervisión de los trabajos de copia de seguridad puede resultar tediosa. Azure Backup facilita la supervisión con alertas por correo electrónico.

- Las alertas se desencadenan por todos los errores de copia de seguridad.
- Las alertas se consolidan en el nivel de base de datos por código de error.
- Se envía una alerta de correo electrónico solo para el primer error de copia de seguridad de una base de datos. 

Para supervisar las alertas de copia de seguridad:

1. Inicie sesión en la suscripción de Azure en [Azure Portal](https://portal.azure.com) para supervisar las alertas de base de datos.

2. En el panel del almacén, seleccione **Alertas y eventos**.

   ![Seleccionar Alertas y eventos](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

4. En **Alertas y eventos**, seleccione **Alertas de copia de seguridad**.

   ![Seleccionar alertas de copia de seguridad](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>Detención de la protección de una base de datos de SQL Server

Puede detener la copia de seguridad de una base de datos SQL Server de las dos formas siguientes:

* Detener todos los trabajos futuros de copia de seguridad y eliminar todos los puntos de recuperación.
* Detener todos los trabajos futuros de copia de seguridad pero dejar intactos los puntos de recuperación.

Observe lo siguiente:

Si deja los puntos de recuperación, estos se limpiarán según la directiva de copia de seguridad. Incurrirá en cargos pro la instancia protegida y el almacenamiento consumido, hasta que se limpien todos los puntos de recuperación. [Más información](https://azure.microsoft.com/pricing/details/backup/) sobre precios.
- Al dejar intactos los puntos de recuperación, aunque expiren según la directiva de retención, Azure Backup siempre mantiene el último punto de recuperación hasta que se eliminen explícitamente los datos de copia de seguridad.
- Si elimina un origen de datos sin detener la copia de seguridad, las nuevas copias de seguridad empezarán a producir errores. Una vez más, los puntos de recuperación anteriores expirarán según la directiva, pero siempre se mantendrá el último punto de recuperación hasta que detenga la copia de seguridad y elimine los datos.
- No puede detener la copia de seguridad de una base de datos habilitada para protección automática hasta que esta se deshabilite.

Para detener la protección de una base de datos:

1. En el panel del almacén, en **Uso**, seleccione **Elementos de copia de seguridad**.

    ![Abrir el menú Elementos de copia de seguridad](./media/backup-azure-sql-database/restore-sql-vault-dashboard.png).

2. En **Tipo de administración de copias de seguridad**, seleccione **SQL en Azure VM**.

    ![Seleccionar SQL en la máquina virtual de Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)


3. Seleccione la base de datos para la que desea detener la protección.

    ![Seleccionar la base de datos en la que detener la protección](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)


5. En el menú de la base de datos, seleccione **Detener copia de seguridad**.

    ![Seleccionar Detener copia de seguridad](./media/backup-azure-sql-database/stop-db-button.png)


6. En el menú **Detener copia de seguridad**, seleccione si desea conservar o eliminar datos. Opcionalmente, proporcione una razón y un comentario.

    ![Menú Detener copia de seguridad](./media/backup-azure-sql-database/stop-backup-button.png)

7. Haga clic en **Detener copia de seguridad**.

  

### <a name="resume-protection-for-a-sql-database"></a>Reanudación de la protección en una base de datos SQL

Si seleccionó la opción **Retener datos de copia de seguridad** al detener la protección de la base de datos SQL, se puede reanudar la protección. Si no se retuvieron los datos de la copia de seguridad, la protección no se puede reanudar.

1. Para reanudar la protección de la base de datos SQL, abra el elemento de copia de seguridad y seleccione **Reanudar copia de seguridad**.

    ![Seleccionar Reanudar copia de seguridad para reanudar la protección de la base de datos](./media/backup-azure-sql-database/resume-backup-button.png)

2. En el menú **Directiva de copia de seguridad**, seleccione una directiva y, después, seleccione **Guardar**.

## <a name="run-an-on-demand-backup"></a>Ejecución de una copia de seguridad a petición

Puede ejecutar diferentes tipos de copias de seguridad a petición:

* Copia de seguridad completa
* Copiar solo copia de seguridad completa
* Copia de seguridad diferencial
* Copia de seguridad de registro

[Más información](backup-architecture.md#sql-server-backup-types) sobre los tipos de copia de seguridad de SQL Server

## <a name="unregister-a-sql-server-instance"></a>Anulación del registro de una instancia de SQL Server

Anule el registro de una instancia de SQL Server después de deshabilitar la protección, pero antes elimine el almacén:

1. En el panel del almacén, en **Administrar**, seleccione **Infraestructura de Backup**.  

   ![Seleccionar Infraestructura de Backup](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. En **Servidores de administración**, seleccione **Servidores protegidos**.

   ![Seleccionar servidores protegidos](./media/backup-azure-sql-database/protected-servers.png)


3. En **Servidores protegidos**, seleccione el servidor del que desea anular el registro. Para eliminar el almacén, debe anular el registro de todos los servidores.

4. Haga clic con el botón derecho en el servidor protegido > **Eliminar**.

   ![Seleccionar Eliminar](./media/backup-azure-sql-database/delete-protected-server.png)


## <a name="next-steps"></a>Pasos siguientes

[Revise](backup-sql-server-azure-troubleshoot.md) la información sobre solución de problemas para la copia de seguridad de bases de datos SQL Server.
