---
title: Uso de Azure Backup para restaurar bases de datos SQL Server de copia de seguridad en una VM de Azure | Microsoft Docs
description: En este artículo se describe cómo restaurar bases de datos SQL Server que se ejecutan en una máquina virtual de Azure y cuyas copias de seguridad se realizan con Azure Backup.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: dacurwin
ms.openlocfilehash: 71867e520d9c98b4af4d4f18f3d08c9e8cc4a8c4
ms.sourcegitcommit: 3877b77e7daae26a5b367a5097b19934eb136350
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/30/2019
ms.locfileid: "68639550"
---
# <a name="restore-sql-server-databases-on-azure-vms"></a>Restauración de bases de datos SQL Server en máquinas virtuales de Azure

En este artículo se describe cómo restaurar una base de datos SQL Server que se ejecuta en una máquina virtual (VM) de Azure cuya copia de seguridad se ha realizado en un almacén de Azure Backup Recovery Services con el servicio [Azure Backup](backup-overview.md).

En este artículo se describe cómo restaurar las bases de datos SQL Server. Para obtener más información, consulte [Copia de seguridad de bases de datos de SQL Server en máquinas virtuales de Azure](backup-azure-sql-database.md).

## <a name="restore-to-a-time-or-a-recovery-point"></a>Restauración a un momento dado o a un punto de recuperación

Azure Backup puede restaurar las bases de datos SQL Server que se ejecutan en las máquinas virtuales de Azure como se indica a continuación:

- Restaure a una fecha u hora específicas (hasta los segundos) mediante copias de seguridad del registro de transacciones. Azure Backup determina automáticamente la copia de seguridad diferencial completa apropiada y la cadena de copias de seguridad de registros necesarias para restaurar los datos en función del tiempo seleccionado.
- Restaure una copia de seguridad completa o diferencial específica para restaurar a un punto de recuperación específico.


## <a name="prerequisites"></a>Requisitos previos

Antes de restaurar una base de datos, tenga en cuenta lo siguiente:

- Puede restaurar la base de datos en una instancia de SQL Server en la misma región de Azure.
- El servidor de destino debe estar registrado como origen en el mismo almacén.
- Para restaurar una base de datos cifrada TDE en otra de SQL Server, primero debe [restaurar el certificado en el servidor de destino](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
- Antes de realizar una restauración de la base de datos "maestra", inicie la instancia de SQL Server en modo de usuario único con la opción de inicio **-m AzureWorkloadBackup**.
    - El valor de **-m** es el nombre del cliente.
    - Solo el nombre de cliente especificado puede abrir la conexión.
- Para todas las bases de datos del sistema (modelo, master, msdb), detenga el servicio Agente SQL Server antes de desencadenar la restauración.
- Cierre las aplicaciones que pueden intentar quitar una conexión a cualquiera de estas bases de datos.
- Si tiene varias instancias ejecutándose en un servidor, todas ellas deberían estar funcionando y ejecutándose; de lo contrario, el servidor no aparecerá en la lista de servidores de destino en los que restaurar la base de datos.

## <a name="restore-a-database"></a>Restauración de una base de datos

Para realizar la restauración, necesita los siguientes permisos:

* Permisos de **operador de copia de seguridad** en el almacén en el que se realiza la restauración.
* Acceso de **colaborador (escritura)** a la máquina virtual de origen de la que se realiza la copia de seguridad.
* Acceso de **colaborador (escritura)** a la máquina virtual de destino:
    - Si va a restaurar en la misma VM, esta será la VM de origen.
    - Si va a restaurar en una ubicación alternativa, esta será la nueva VM de destino.

Realice la restauración como sigue:
1. Abra el almacén en el que está registrada la VM con SQL Server.
2. En el panel del almacén, en **Uso**, seleccione **Elementos de copia de seguridad**.
3. En **Elementos de copia de seguridad**, en **Tipo de administración de copia de seguridad**, seleccione **SQL en máquina virtual de Azure**.

    ![Seleccionar SQL en la máquina virtual de Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)

4. Seleccione la base de datos que se va a restaurar.

    ![Selección de la base de datos que se va a restaurar](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

5. Revise el menú de la base de datos. Proporciona información sobre la copia de seguridad de la base de datos, como:

    * Los puntos de restauración más antiguos y más recientes.
    * El estado de copia de seguridad de registros de las 24 últimas horas, para bases de datos con el modelo de recuperación optimizado para cargas masivas de registros y el modelo de recuperación optimizado para cargas completas de registros, y configuradas para copias de seguridad del registro de transacciones.

6. Seleccione **Restaurar BD**.

    ![Seleccionar Restaurar BD](./media/backup-azure-sql-database/restore-db-button.png)

7. En **Restaurar configuración**, especifique dónde quiere restaurar los datos:
   - **Ubicación alternativa**: restaura la base de datos en una ubicación alternativa y mantiene la base de datos de origen original.
   - **Sobrescribir la base de datos**: restaura los datos en la misma instancia de SQL Server que el origen. Esta opción sobrescribe la base de datos original.

     > [!Important]
     > Si la base de datos seleccionada pertenece a un grupo de disponibilidad AlwaysOn, SQL no permite sobrescribir la base de datos. Solo está disponible **Ubicación alternativa**.
     >

     ![Menú Restaurar configuración](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Restauración a una ubicación alternativa

1. En el menú **Restaurar configuración**, en **Where to Restore** (Ubicación de restauración), seleccione **Ubicación alternativa**.
2. Seleccione el nombre de SQL Server y la instancia en la que desea restaurar la base de datos.
3. En el cuadro de diálogo **Nombre de la base de datos restaurada**, escriba el nombre de la base de datos de destino.
4. Si procede, seleccione **Overwrite if the DB with the same name already exists on selected SQL instance** (Sobrescribir si ya existe una base de datos con el mismo nombre en la instancia de SQL seleccionada).
5. Seleccione **Aceptar**.

    ![Proporcionar valores para el menú Restaurar configuración](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. En **Seleccionar punto de restauración**, seleccione si desea [restaurar a un momento dado](#restore-to-a-specific-point-in-time) o restaurar a un [punto de recuperación específico](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > La restauración a un momento dado solo está disponible para copias de seguridad de registros de bases de datos con un modelo de recuperación optimizado para cargas masivas y completas de registros.

### <a name="restore-and-overwrite"></a>Restauración y sobrescritura

1. En el menú **Restaurar configuración**, en **Where to Restore** (Ubicación de restauración), seleccione **Sobrescribir la base de datos** > **Aceptar**.

    ![Seleccionar Sobrescribir la base de datos](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

2. En **Seleccionar punto de restauración**, seleccione **Registros (a un momento dado)** a [Restauración a un momento dado](#restore-to-a-specific-point-in-time). O bien, seleccione **Completa y diferencial** para restaurar a un [punto de recuperación específico](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > La restauración a un momento dado solo está disponible para copias de seguridad de registros de bases de datos con un modelo de recuperación optimizado para cargas masivas y completas de registros.

### <a name="restore-to-a-specific-point-in-time"></a>Restauración a un momento dado

Si ha seleccionado **Registros (punto en el tiempo)** como el tipo de restauración, haga lo siguiente:

1.  En **Fecha y hora de restauración**, abra el calendario. En el calendario, las fechas que contienen puntos de recuperación se muestran en negrita, y la fecha actual está resaltada.
1. Seleccione una fecha con puntos de recuperación. No puede seleccionar fechas que no tengan puntos de recuperación.

    ![Abrir el calendario](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

1. Una vez seleccionada una fecha, el gráfico de escala de tiempo muestra los puntos de recuperación disponibles en un rango continuo.
1. Especifique una hora para la recuperación en el gráfico de escala de tiempo o seleccione una hora. Después seleccione **Aceptar**.

    ![Seleccione una hora de restauración.](./media/backup-azure-sql-database/recovery-point-logs-graph.png)


1. En el menú **Configuración avanzada**, si desea mantener la base de datos no operativa después de la restauración, habilite **Restaurar con NORECOVERY**.
1. Si desea cambiar la ubicación de restauración en el servidor de destino, escriba una nueva ruta de acceso de destino.
1. Seleccione **Aceptar**.

    ![Menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

1. En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración.
1. Realice un seguimiento del progreso de la restauración en el área de **notificaciones** o seleccione **Trabajos de restauración** en el menú de la base de datos.

    ![Progreso del trabajo de restauración](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-to-a-specific-restore-point"></a>Restauración a un punto de restauración específico

Si ha seleccionado **Completo y diferencial** como el tipo de restauración, haga lo siguiente:

1. Seleccione un punto de recuperación de la lista y seleccione **Aceptar** para completar el procedimiento de punto de restauración.

    ![Elegir un punto de recuperación completo](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

1. En el menú **Configuración avanzada**, si desea mantener la base de datos no operativa después de la restauración, habilite **Restaurar con NORECOVERY**.
1. Si desea cambiar la ubicación de restauración en el servidor de destino, escriba una nueva ruta de acceso de destino.
1. Seleccione **Aceptar**.

    ![Menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

1. En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración.
1. Realice un seguimiento del progreso de la restauración en el área de **notificaciones** o seleccione **Trabajos de restauración** en el menú de la base de datos.

    ![Progreso del trabajo de restauración](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-databases-with-large-number-of-files"></a>Restauración de bases de datos con un gran número de archivos

Si el tamaño de la cadena total de archivos en una base de datos es mayor que un [determinado límite](backup-sql-server-azure-troubleshoot.md#size-limit-for-files), Azure Backup almacena la lista de archivos de la base de datos en un componente pit diferente y no podrá definir la ruta de la restauración de destino durante la operación de restauración. En su lugar, los archivos se restaurarán en la ruta predeterminada SQL.

  ![Restauración de bases de datos con archivos de gran tamaño](./media/backup-azure-sql-database/restore-large-files.jpg)


## <a name="next-steps"></a>Pasos siguientes

[Administrar y supervisar](manage-monitor-sql-database-backup.md) bases de datos SQL Server cuya copia de seguridad se ha realizado con Azure Backup.
