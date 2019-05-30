---
title: Use Azure Backup para restaurar las bases de datos de SQL Server de copia de seguridad en una máquina virtual de Azure | Microsoft Docs
description: En este artículo se describe cómo restaurar las bases de datos de SQL Server que se ejecutan en una máquina virtual de Azure y que son una copia de seguridad con Azure Backup.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: raynew
ms.openlocfilehash: d8ade598e4f1b6331367e8bd04ad59951ef5de8f
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66242365"
---
# <a name="restore-sql-server-databases-on-azure-vms"></a>Restauración de bases de datos SQL Server en máquinas virtuales de Azure

En este artículo se describe cómo restaurar una base de datos de SQL Server que se ejecuta en una máquina virtual (VM) de Azure que el [Azure Backup](backup-overview.md) con una copia de seguridad de servicio en un almacén de copia de seguridad de Azure Recovery Services.

En este artículo se describe cómo restaurar las bases de datos SQL Server. Para obtener más información, consulte [copia de seguridad de bases de datos de SQL Server en máquinas virtuales de Azure](backup-azure-sql-database.md).

## <a name="restore-to-a-time-or-a-recovery-point"></a>Restaurar a una hora o un punto de recuperación

Copia de seguridad de Azure puede restaurar las bases de datos de SQL Server que se ejecutan en máquinas virtuales de Azure como sigue:

- Restaurar en una fecha u hora específicas (para la segunda) mediante el uso de copias de seguridad de registro de transacciones. Azure Backup determina automáticamente la copia de seguridad diferencial completas adecuado y la cadena de copias de seguridad del registro que son necesarias para restaurar según la hora seleccionada.
- Restaurar una copia de seguridad completa o diferencial específica para restaurar a un punto de recuperación específico.


## <a name="prerequisites"></a>Requisitos previos

Antes de restaurar una base de datos, tenga en cuenta lo siguiente:

- Puede restaurar la base de datos en una instancia de SQL Server en la misma región de Azure.
- El servidor de destino debe estar registrado como origen en el mismo almacén.
- Para restaurar una base de datos tenga cifrado TDE a otra de SQL Server, primero debe [restaurar el certificado en el servidor de destino](https://docs.microsoft.com/sql/relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server?view=sql-server-2017).
- Antes de restaurar la base de datos "master", inicie la instancia de SQL Server en modo de usuario único mediante el uso de la opción de inicio **-m AzureWorkloadBackup**.
    - El valor de **-m** es el nombre del cliente.
    - Solo el nombre del cliente especificado puede abrir la conexión.
- Para todas las bases de datos del sistema (modelo, master, msdb), detenga el servicio Agente SQL Server antes de desencadenar la restauración.
- Cierre las aplicaciones que podrían intentar realizar una conexión a cualquiera de estas bases de datos.
- Si tiene varias instancias que se ejecutan en un servidor, todas las instancias deben estar en y ejecuta el servidor de lo contrario no aparecerán en la lista de servidores de destino para restaurar la base de datos.

## <a name="restore-a-database"></a>Restauración de una base de datos

Para restaurar, necesita los siguientes permisos:

* **Operador de copia de seguridad** permisos en el almacén donde se está realizando la restauración.
* Acceso de **colaborador (escritura)** a la máquina virtual de origen de la que se realiza la copia de seguridad.
* Acceso de **colaborador (escritura)** a la máquina virtual de destino:
    - Si va a restaurar en la misma máquina virtual, se trata la máquina virtual de origen.
    - Si va a restaurar en una ubicación alternativa, se trata la nueva máquina virtual de destino.

Realice la restauración como sigue:
1. Abra el almacén en el que está registrada la VM con SQL Server.
2. En el panel del almacén, en **Uso**, seleccione **Elementos de copia de seguridad**.
3. En **Elementos de copia de seguridad**, en **Tipo de administración de copia de seguridad**, seleccione **SQL en máquina virtual de Azure**.

    ![Seleccionar SQL en la máquina virtual de Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)

4. Seleccione la base de datos que se va a restaurar.

    ![Selección de la base de datos que se va a restaurar](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

5. Revise el menú de la base de datos. Proporciona información sobre la copia de seguridad de la base de datos, como:

    * Los puntos de restauración más antiguos y más recientes.
    * El estado de copia de seguridad de registro de las últimas 24 horas para las bases de datos que están en modo de recuperación full y bulk-logged y que están configurados para las copias de seguridad del registro transaccional.

6. Seleccione **Restore DB**.

    ![Seleccionar Restaurar BD](./media/backup-azure-sql-database/restore-db-button.png)

7. En **Restore Configuration**, especifique dónde desea restaurar los datos:
   - **Ubicación alternativa**: Restaurar la base de datos en una ubicación alternativa y mantener la base de datos de origen original.
   - **Sobrescribir la base de datos**: restaura los datos en la misma instancia de SQL Server que el origen. Esta opción sobrescribe la base de datos original.

     > [!Important]
     > Si la base de datos seleccionada pertenece a un grupo de disponibilidad AlwaysOn, SQL no permite sobrescribir la base de datos. Solo está disponible **Ubicación alternativa**.
     >

     ![Menú Restaurar configuración](./media/backup-azure-sql-database/restore-restore-configuration-menu.png)

### <a name="restore-to-an-alternate-location"></a>Restauración a una ubicación alternativa

1. En el **Restore Configuration** menú, en **dónde quiere restaurarlo**, seleccione **ubicación alternativa**.
2. Seleccione el nombre de SQL Server y la instancia en la que desea restaurar la base de datos.
3. En el cuadro de diálogo **Nombre de la base de datos restaurada**, escriba el nombre de la base de datos de destino.
4. Si procede, seleccione **Overwrite if the DB with the same name already exists on selected SQL instance** (Sobrescribir si ya existe una base de datos con el mismo nombre en la instancia de SQL seleccionada).
5. Seleccione **Aceptar**.

    ![Proporcionar valores para el menú Restaurar configuración](./media/backup-azure-sql-database/restore-configuration-menu.png)

2. En **Seleccionar punto de restauración**, seleccione si desea [restaurar a un momento específico en el tiempo](#restore-to-a-specific-point-in-time) o a [restaurar a un punto de recuperación específico](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > La restauración en un momento solo está disponible para las copias de seguridad del registro para las bases de datos que se encuentran en modo de recuperación full y bulk-logged.

### <a name="restore-and-overwrite"></a>Restauración y sobrescritura

1. En el **Restore Configuration** menú, en **dónde quiere restaurarlo**, seleccione **sobrescribir DB** > **Aceptar**.

    ![Seleccionar Sobrescribir la base de datos](./media/backup-azure-sql-database/restore-configuration-overwrite-db.png)

2. En **Seleccionar punto de restauración**, seleccione **registros (momento dado)** a [restaurar a un momento específico en el tiempo](#restore-to-a-specific-point-in-time). O bien seleccione **completa y diferencial** para restaurar a un [punto de recuperación específico](#restore-to-a-specific-restore-point).

    > [!NOTE]
    > La restauración en un momento solo está disponible para las copias de seguridad del registro para las bases de datos que se encuentran en modo de recuperación full y bulk-logged.

### <a name="restore-to-a-specific-point-in-time"></a>Restauración a un momento dado

Si ha seleccionado **Registros (punto en el tiempo)** como el tipo de restauración, haga lo siguiente:

1.  En **restaurar la fecha y hora**, abrir el calendario. En el calendario, las fechas que tienen puntos de recuperación se muestran en negrita y resaltar la fecha actual.
1. Seleccione una fecha que tiene puntos de recuperación. No puede seleccionar las fechas con ningún punto de recuperación.

    ![Abrir el calendario](./media/backup-azure-sql-database/recovery-point-logs-calendar.png)

1. Una vez seleccionada una fecha, el gráfico de escala de tiempo muestra los puntos de recuperación disponibles en un rango continuo.
1. Especifique una hora para la recuperación en el gráfico de escala de tiempo, o seleccione una hora. Después seleccione **Aceptar**.

    ![Seleccione una hora de restauración](./media/backup-azure-sql-database/recovery-point-logs-graph.png)


1. En el **Advanced Configuration** menú, si desea mantener la base de datos no operativa después de la restauración, habilitar **restaurar con NORECOVERY**.
1. Si desea cambiar la ubicación de restauración en el servidor de destino, escriba una nueva ruta de acceso de destino.
1. Seleccione **Aceptar**.

    ![Menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

1. En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración.
1. Realizar un seguimiento del progreso de la restauración en el **notificaciones** área, o realizar su seguimiento seleccionando **trabajos de restauración** en el menú de la base de datos.

    ![Progreso del trabajo de restauración](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-to-a-specific-restore-point"></a>Restauración a un punto de restauración específico

Si ha seleccionado **Completo y diferencial** como el tipo de restauración, haga lo siguiente:

1. Seleccione un punto de recuperación de la lista y seleccione **Aceptar** para completar el procedimiento de punto de restauración.

    ![Elegir un punto de recuperación completo](./media/backup-azure-sql-database/choose-fd-recovery-point.png)

1. En el **Advanced Configuration** menú, si desea mantener la base de datos no operativa después de la restauración, habilitar **restaurar con NORECOVERY**.
1. Si desea cambiar la ubicación de restauración en el servidor de destino, escriba una nueva ruta de acceso de destino.
1. Seleccione **Aceptar**.

    ![Menú de configuración avanzada](./media/backup-azure-sql-database/restore-point-advanced-configuration.png)

1. En el menú **Restaurar**, seleccione **Restaurar** para iniciar el trabajo de restauración.
1. Realizar un seguimiento del progreso de la restauración en el **notificaciones** área, o realizar su seguimiento seleccionando **trabajos de restauración** en el menú de la base de datos.

    ![Progreso del trabajo de restauración](./media/backup-azure-sql-database/restore-job-notification.png)

### <a name="restore-databases-with-large-number-of-files"></a>Restaurar bases de datos con un gran número de archivos

Si el tamaño de la cadena total de archivos en una base de datos es mayor que un [determinado límite](backup-sql-server-azure-troubleshoot.md#files-size-limit-beyond-which-restore-happens-to-default-path), Azure Backup almacena la lista de archivos de base de datos en un componente pit diferentes que no podrá establecer la ruta de acceso de la restauración de destino durante la restauración operación. Los archivos se restaurarán en la ruta predeterminada SQL en su lugar.

  ![Restaurar la base de datos con archivos de gran tamaño](./media/backup-azure-sql-database/restore-large-files.jpg)


## <a name="next-steps"></a>Pasos siguientes

[Administrar y supervisar](manage-monitor-sql-database-backup.md) bases de datos de SQL Server que estén respaldados por Azure Backup.
