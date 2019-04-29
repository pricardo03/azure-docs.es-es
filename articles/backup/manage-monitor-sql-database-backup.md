---
title: Administrar y supervisar las bases de datos de SQL Server en una máquina virtual de Azure que está respaldada por Azure Backup | Microsoft Docs
description: En este artículo se describe cómo administrar y supervisar las bases de datos de SQL Server que se ejecutan en una máquina virtual de Azure.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: raynew
ms.openlocfilehash: ea5495867d5f453db014e000e01d533d049dc628
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60849412"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Administración y supervisión de bases de datos SQL Server con copia de seguridad

En este artículo se describe tareas comunes para administrar y supervisar las bases de datos de SQL Server que se ejecutan en una máquina virtual (VM) de Azure y que se copian en una copia de seguridad de Azure Recovery Services del almacén por el [Azure Backup](backup-overview.md) service. Obtendrá información sobre cómo supervisar los trabajos y alertas, detener y reanudar la protección de la base de datos, ejecutar trabajos de copia de seguridad y anular el registro de una máquina virtual desde las copias de seguridad.

Si todavía no ha configurado las copias de seguridad para las bases de datos de SQL Server, consulte [copia de seguridad de bases de datos de SQL Server en máquinas virtuales de Azure](backup-azure-sql-database.md)

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>Supervisión de trabajos de copia de seguridad manuales en el portal

Copia de seguridad de Azure muestra todos los trabajos desencadenados manualmente en el **trabajos de copia de seguridad** portal. Los trabajos, consulte en este portal include detección de base de datos y registro y copia de seguridad y las operaciones de restauración.

![El portal de trabajos de copia de seguridad](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> El **trabajos de copia de seguridad** portal no muestra los trabajos de copia de seguridad programados. Use SQL Server Management Studio para supervisar los trabajos de copia de seguridad programados, como se describe en la sección siguiente.
>

Para obtener más información sobre los escenarios de supervisión, vaya a [supervisión en Azure Portal](backup-azure-monitoring-built-in-monitor.md) y [supervisión con Azure Monitor](backup-azure-monitoring-use-azuremonitor.md).  


## <a name="view-backup-alerts"></a>Visualización de alertas de copia de seguridad

Dado que las copias de seguridad de registros se producen con una frecuencia de 15 minutos, la supervisión de los trabajos de copia de seguridad puede resultar tediosa. Copia de seguridad de Azure facilita la supervisión mediante el envío de alertas por correo electrónico. Alertas por correo electrónico son:

- Se desencadena para todos los errores de copia de seguridad.
- Consolidado en el nivel de base de datos por código de error.
- Envía solo para el primer error de copia de seguridad de una base de datos.

Para supervisar las alertas de copia de seguridad de base de datos:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. En el panel del almacén, seleccione **Alertas y eventos**.

   ![Seleccionar Alertas y eventos](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

3. En **Alertas y eventos**, seleccione **Alertas de copia de seguridad**.

   ![Seleccionar alertas de copia de seguridad](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>Detención de la protección de una base de datos de SQL Server

Puede detener la copia de seguridad de una base de datos SQL Server de las dos formas siguientes:

* Detener todos los trabajos futuros de copia de seguridad y eliminar todos los puntos de recuperación.
* Detener todos los trabajos futuros de copia de seguridad y deja intactos los puntos de recuperación.

Si decide dejar los puntos de recuperación, tenga estos detalles en mente:

* Todos los puntos de recuperación permanecerá intactos para siempre, eliminación de todos los deberá detener al detener la protección con conservar datos.
* Se le cobrará de la instancia protegida y el almacenamiento consumido. Para obtener más información, consulte [precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
* Si elimina un origen de datos sin detener las copias de seguridad, se producirá un error en nuevas copias de seguridad.

Para detener la protección de una base de datos:

1. En el panel del almacén, seleccione **elementos de copia de seguridad**.

2. En **tipo de administración de copia de seguridad**, seleccione **SQL en la máquina virtual de Azure**.

    ![Seleccionar SQL en la máquina virtual de Azure](./media/backup-azure-sql-database/sql-restore-backup-items.png)

3. Seleccione la base de datos para la que desea detener la protección.

    ![Seleccionar la base de datos en la que detener la protección](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

4. En el menú de la base de datos, seleccione **Detener copia de seguridad**.

    ![Seleccionar Detener copia de seguridad](./media/backup-azure-sql-database/stop-db-button.png)


5. En el **Detener copia de seguridad** menú, seleccione si desea conservar o eliminar datos. Si lo desea, proporcione un motivo y comentario.

    ![Conservar o eliminar datos en el menú Detener copia de seguridad](./media/backup-azure-sql-database/stop-backup-button.png)

6. Seleccione **Detener copia de seguridad**.


## <a name="resume-protection-for-a-sql-database"></a>Reanudación de la protección en una base de datos SQL

Al detener la protección de la base de datos SQL, si selecciona el **conservar datos de copia de seguridad** opción, más tarde puede reanudar la protección. Si no mantiene los datos de copia de seguridad, no se puede reanudar la protección.

Para reanudar la protección de una base de datos SQL:

1. Abra el elemento de copia de seguridad y seleccione **Reanudar copia de seguridad**.

    ![Seleccionar Reanudar copia de seguridad para reanudar la protección de la base de datos](./media/backup-azure-sql-database/resume-backup-button.png)

2. En el menú **Directiva de copia de seguridad**, seleccione una directiva y, después, seleccione **Guardar**.

## <a name="run-an-on-demand-backup"></a>Ejecución de una copia de seguridad a petición

Puede ejecutar diferentes tipos de copias de seguridad a petición:

* Copia de seguridad completa
* Copiar solo copia de seguridad completa
* Copia de seguridad diferencial
* Copia de seguridad de registro

Aunque puede que necesite especificar la duración de retención de copia de seguridad completa de solo copia, la duración de retención para otros tipos de copia de seguridad se establece automáticamente en 30 días desde el momento actual. <br/>
Para obtener más información, consulte [tipos de copia de seguridad de SQL Server](backup-architecture.md#sql-server-backup-types).

## <a name="unregister-a-sql-server-instance"></a>Anulación del registro de una instancia de SQL Server

Anular el registro de una instancia de SQL Server después de deshabilitar protección pero antes de eliminar el almacén:

1. En el panel del almacén, en **administrar**, seleccione **infraestructura de copia de seguridad**.  

   ![Seleccionar Infraestructura de Backup](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. En **Servidores de administración**, seleccione **Servidores protegidos**.

   ![Seleccionar servidores protegidos](./media/backup-azure-sql-database/protected-servers.png)

3. En **Servidores protegidos**, seleccione el servidor del que desea anular el registro. Para eliminar el almacén, debe anular el registro de todos los servidores.

4. Haga clic en el servidor protegido y seleccione **eliminar**.

   ![Seleccionar Eliminar](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="re-register-extension-on-the-sql-server-vm"></a>Volver a registrar la extensión en la máquina virtual de SQL Server

A veces, la extensión de la carga de trabajo en la máquina virtual puede verse afectada por una razón u otra. En tales casos, todas las operaciones que se desencadena la en la máquina virtual comenzará a producir un error. Es posible que, a continuación, deberá volver a registrar la extensión en la máquina virtual. **Volver a registrar** operación vuelve a instalar la extensión de copia de seguridad de cargas de trabajo en la máquina virtual para las operaciones continuar.  <br>

Se recomienda usar esta opción con precaución; Cuando se desencadena en una máquina virtual con la extensión ya correcto, esta operación hará que la extensión empezar a reiniciar. Esto puede dar lugar a que todos los trabajos en curso para producir un error. . Compruebe para uno o varios de los [síntomas](backup-sql-server-azure-troubleshoot.md#symptoms) antes de desencadenar la operación de volver a registrar.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información, consulte [solucionar problemas de copias de seguridad en una base de datos de SQL Server](backup-sql-server-azure-troubleshoot.md).
