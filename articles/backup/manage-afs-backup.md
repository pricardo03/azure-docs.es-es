---
title: Administración de copias de seguridad de recursos compartidos de archivos de Azure
description: En este artículo se describen las tareas comunes para administrar y supervisar los recursos compartidos de archivos de Azure de los que el servicio Azure Backup realiza una copia de seguridad.
ms.topic: conceptual
ms.date: 01/07/2020
ms.openlocfilehash: a5477d021b6e3600693e183d8707e11592b7cc38
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294894"
---
# <a name="manage-azure-file-share-backups"></a>Administración de copias de seguridad de recursos compartidos de archivos de Azure

En este artículo se describen las tareas comunes para administrar y supervisar los recursos compartidos de archivos de Azure de los que el servicio [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview) realiza una copia de seguridad. Aprenderá a ejecutar las siguientes tareas de administración en el almacén de Recovery Services:

* [Supervisión de trabajos](#monitor-jobs)
* [Creación de una nueva directiva](#create-a-new-policy)
* [Modificación de directivas](#modify-policy)
* [Detención de la protección en un recurso compartido de archivos](#stop-protection-on-a-file-share)
* [Reanudación de la protección en un recurso compartido de archivos](#resume-protection-on-a-file-share)
* [Eliminación de datos de copia de seguridad](#delete-backup-data)
* [Anulación del registro de la cuenta de almacenamiento](#unregister-storage-account)

## <a name="monitor-jobs"></a>Supervisión de trabajos

Al desencadenar operaciones de copia de seguridad o restauración, el servicio de copia de seguridad crea un trabajo para realizar un seguimiento. Puede supervisar el progreso de todos los trabajos en la página **Backup Jobs** (Trabajos de copia de seguridad).

Para abrir la página **Backup Jobs** (Trabajos de copia de seguridad), siga estos pasos:

1. Abra el almacén de Recovery Services que usó para configurar la copia de seguridad de los recursos compartidos de archivos. En la hoja **Información general**, haga clic en **Trabajos de copia de seguridad** en la sección **Supervisión**.

   ![Trabajos de copia de seguridad en la sección Supervisión](./media/manage-afs-backup/backup-jobs.png)

2. Después de hacer clic en Aceptar, la hoja **Trabajos de copia de seguridad** aparece y en ella se enumera el estado de todos los trabajos. Puede hacer clic en el nombre de la carga de trabajo correspondiente al recurso compartido de archivos que desea supervisar.

   ![Nombre de la carga de trabajo](./media/manage-afs-backup/workload-name.png)

## <a name="create-a-new-policy"></a>Creación de una nueva directiva

Puede crear una nueva directiva para realizar una copia de seguridad de los recursos compartidos de archivos de Azure desde la sección **Directivas de copia de seguridad** del almacén de Recovery Services. Todas las directivas que se crean al configurar la copia de seguridad de los recursos compartidos de archivos se muestran con el tipo de directiva como recurso compartido de archivos de Azure.

Para ver las directivas de copia de seguridad existentes, siga estos pasos:

1. Abra el almacén de Recovery Services que usó para configurar la copia de seguridad del recurso compartido de archivos y, en el menú del almacén de Recovery Services, haga clic en **Directivas de copia de seguridad** en la sección Administrar. Se enumerarán todas las directivas de copia de seguridad configuradas en el almacén.

   ![Todas las directivas de copia de seguridad](./media/manage-afs-backup/all-backup-policies.png)

2. Para ver las directivas específicas del recurso compartido de archivos de Azure, seleccione **Recurso compartido de archivos de Azure** en la lista desplegable de la parte superior derecha.

   ![Elección de Recurso compartido de archivos de Azure](./media/manage-afs-backup/azure-file-share.png)

Para crear una nueva directiva de copia de seguridad, siga estos pasos:

1. Haga clic en **+ Agregar** en la hoja Directivas de copia de seguridad.

   ![Nueva directiva de copia de seguridad](./media/manage-afs-backup/new-backup-policy.png)

2. Seleccione **Recurso compartido de archivos de Azure** como **Tipo de directiva** en la hoja **Agregar**. Se abrirá la hoja Directivas de copia de seguridad del recurso compartido de archivos de Azure. Especifique el nombre de la directiva, la frecuencia de copia de seguridad y el intervalo de retención para los puntos de recuperación. Cuando haya definido la directiva, haga clic en **Aceptar**.

   ![Definición de la directiva de copia de seguridad](./media/manage-afs-backup/define-backup-policy.png)

## <a name="modify-policy"></a>Modificación de directivas

Puede modificar una directiva de copia de seguridad para cambiar la frecuencia de las copias de seguridad o la duración de retención.

Para modificar una directiva:

1. Abra el almacén de Recovery Services que usó para configurar la copia de seguridad del recurso compartido de archivos y, en el menú del almacén de Recovery Services, haga clic en **Directivas de copia de seguridad** en la sección Administrar. Se enumerarán todas las directivas de copia de seguridad configuradas en el almacén.

   ![Todas las directivas de copia de seguridad del almacén](./media/manage-afs-backup/all-backup-policies-modify.png)

2. Para ver las directivas específicas de un recurso compartido de archivos de Azure, seleccione **Recurso compartido de archivos de Azure** en la lista desplegable de la parte superior derecha. Haga clic en la directiva de copia de seguridad que desee modificar.

   ![Recurso compartido de archivos de Azure que se va a modificar](./media/manage-afs-backup/azure-file-share-modify.png)

3. La hoja **Programación** se abrirá. Edite el período de retención/la programación de copia de seguridad según sea necesario y haga clic en **Guardar**. Verá el mensaje "Actualización en curso" en la hoja y, cuando los cambios en la directiva se actualicen correctamente, el mensaje, "La directiva de copia de seguridad se actualizó correctamente".

   ![Directiva modificada y guardada](./media/manage-afs-backup/save-policy.png)

## <a name="stop-protection-on-a-file-share"></a>Detención de la protección en un recurso compartido de archivos

Hay dos maneras de dejar de proteger recursos compartidos de archivos de Azure:

* Detener todos los trabajos futuros de copia de seguridad y *eliminar todos los puntos de recuperación*
* Detener todos los trabajos futuros de copia de seguridad pero *dejar los puntos de recuperación*

Puede que dejar los puntos de recuperación en el almacenamiento conlleve un costo asociado, dado que las instantáneas subyacentes creadas por Azure Backup se conservan. Sin embargo, la ventaja de dejarlos es que puede restaurar el recurso compartido de archivos más adelante, si así lo desea. Para más información sobre el costo de dejar los puntos de recuperación, consulte la [información sobre precios](https://azure.microsoft.com/pricing/details/backup/). Si opta por eliminar todos los puntos de recuperación, no podrá restaurar el recurso compartido de archivos.

Para dejar de proteger recursos compartidos de archivos de Azure, siga estos pasos:

1. Abra el almacén de Recovery Services que contiene los puntos de recuperación del recurso compartido de archivos y haga clic en **Elementos de copia de seguridad** en la sección Elementos protegidos. Aparece la lista de tipos de elementos de copia de seguridad.

   ![Elementos de copia de seguridad](./media/manage-afs-backup/backup-items.png)

2. En la lista **Backup Management Type** (Tipo de administración de copia de seguridad), seleccione **Azure Storage (Azure Files)** . Aparecerá la lista **Backup Items for (Azure Storage (Azure Files))** (Elementos de copia de seguridad para [Azure Storage (Azure Files)]).

   ![Selección de Azure Storage (Azure Files)](./media/manage-afs-backup/azure-storage-azure-files.png)

3. En la lista **Backup Items for (Azure Storage (Azure Files))** (Elementos de copia de seguridad para [Azure Storage (Azure Files)]), seleccione el elemento de copia de seguridad que quiera detener.

4. Seleccione la opción **Detener copia de seguridad** del menú **Elemento de copia de seguridad** de la hoja.

   ![Seleccionar Detener copia de seguridad](./media/manage-afs-backup/stop-backup.png)

5. En la hoja **Detener copia de seguridad**, elija **Retener datos de copia de seguridad** o **Eliminar datos de la copia de seguridad** y haga clic en **Detener copia de seguridad**.

    ![Elección de conservar o eliminar los datos de la copia de seguridad](./media/manage-afs-backup/retain-or-delete-backup-data.png)

## <a name="resume-protection-on-a-file-share"></a>Reanudación de la protección en un recurso compartido de archivos

Si eligió la opción **Retener datos de copia de seguridad** al detener la protección del recurso compartido de archivos, es posible reanudar la protección. Si eligió la opción **Eliminar datos de la copia de seguridad**, no se puede reanudar la protección del recurso compartido de archivos.

Para reanudar la protección del recurso compartido de archivos de Azure:

1. Abra el almacén de Recovery Services que contiene los puntos de recuperación del recurso compartido de archivos y haga clic en **Elementos de copia de seguridad** en la sección Elementos protegidos. Aparece la lista de tipos de elementos de copia de seguridad.

   ![Elementos de copia de seguridad para reanudar](./media/manage-afs-backup/backup-items-resume.png)

2. En la lista **Backup Management Type** (Tipo de administración de copia de seguridad), seleccione **Azure Storage (Azure Files)** . Aparecerá la lista **Backup Items for (Azure Storage (Azure Files))** (Elementos de copia de seguridad para [Azure Storage (Azure Files)]).

   ![Lista de Azure Storage (Azure Files)](./media/manage-afs-backup/azure-storage-azure-files.png)

3. En la lista **Backup Items for (Azure Storage (Azure Files))** (Elementos de copia de seguridad para [Azure Storage (Azure Files)]), seleccione el elemento de copia de seguridad para el cual desee reanudar la protección.

4. Seleccione la opción **Reanudar copia de seguridad** del menú **Elemento de copia de seguridad** de la hoja.

   ![Selección de Reanudar copia de seguridad](./media/manage-afs-backup/resume-backup.png)

5. La hoja **Directiva de Backup** se abrirá y podrá elegir la directiva que prefiera para reanudar la copia de seguridad.

6. Haga clic en **Guardar** después de seleccionar la **Directiva de Backup**. Verá un mensaje "Actualización en curso" en el portal y, una vez que la copia de seguridad se haya reanudado correctamente, el mensaje, "Successfully updated backup Policy for Protected Azure File Share" (La directiva de copia de seguridad se actualizó correctamente para el recurso compartido de archivos de Azure protegido).

   ![Directiva de copia de seguridad actualizada correctamente](./media/manage-afs-backup/successfully-updated.png)

## <a name="delete-backup-data"></a>Eliminación de datos de copia de seguridad

Puede eliminar la copia de seguridad de un recurso compartido de archivos durante el trabajo **Detener copia de seguridad** o en cualquier momento después de haber detenido la protección. Incluso puede resultar útil esperar días o semanas antes de eliminar los puntos de recuperación. Al eliminar datos de copia de seguridad, no se pueden elegir los puntos de recuperación específicos que se van a eliminar. Si elige eliminar los datos de copia de seguridad, se eliminarán todos los puntos de recuperación asociados con el recurso compartido de archivos.

En el procedimiento siguiente se presupone que se ha detenido la protección del recurso compartido de archivos.

Para eliminar los datos de copia de seguridad del recurso compartido de archivos de Azure:

1. Una vez detenido el trabajo de copia de seguridad, se dispone de las opciones **Reanudar copia de seguridad** y **Eliminar datos de copia de seguridad** en el panel **Elemento de copia de seguridad**. Haga clic en la opción **Eliminar datos de la copia de seguridad** en el menú **Elemento de copia de seguridad** de la hoja.

   ![Eliminación de datos de copia de seguridad](./media/manage-afs-backup/delete-backup-data.png)

2. La hoja **Eliminar datos de copia de seguridad** se abrirá. Escriba el nombre del recurso compartido de archivos para confirmar la eliminación. Puede añadir una razón en **Motivo** de la eliminación o notas en **Comment** (Comentario). Haga clic en **Eliminar**cuando esté seguro de querer eliminar los datos de copia de seguridad.

   ![Confirmación de la eliminación de los datos](./media/manage-afs-backup/confirm-delete-data.png)

## <a name="unregister-storage-account"></a>Anulación del registro de la cuenta de almacenamiento

Si desea proteger los recursos compartidos de archivos en una cuenta de almacenamiento determinada mediante un almacén de Recovery Services diferente, primero debe [detener la protección de todos los recursos compartidos de archivos](#stop-protection-on-a-file-share) en esa cuenta de almacenamiento. A continuación, anule el registro de la cuenta del almacén de Recovery Services que se usa actualmente para la protección.

En el procedimiento siguiente se presupone que se ha detenido la protección de todos los recursos compartidos de archivos de la cuenta de almacenamiento cuyo registro desea anular.

Para anular el registro de la cuenta de almacenamiento:

1. Abra el almacén de Recovery Services en el que está registrada la cuenta de almacenamiento.
2. Haga clic en la opción **Infraestructura de Backup** en la sección **Administrar** de la hoja **Información general**.

   ![Clic en Infraestructura de Backup](./media/manage-afs-backup/backup-infrastructure.png)

3. La hoja **Infraestructura de Backup** se abrirá. Haga clic en **Cuentas de almacenamiento** en la sección **Cuentas de Azure Storage** de esta hoja.

   ![Clic en Cuentas de almacenamiento](./media/manage-afs-backup/storage-accounts.png)

4. Cuando haya hecho clic en **Cuentas de almacenamiento**, se mostrará una lista de las cuentas de almacenamiento registradas en el almacén.
5. Haga clic con el botón derecho en la cuenta cuyo registro desee eliminar y elija **Cancelar registro**.

   ![Seleccionar Anular registro](./media/manage-afs-backup/select-unregister.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte [Solución de problemas de copia de seguridad y restauración para recursos compartidos de archivos de Azure](https://docs.microsoft.com/azure/backup/troubleshoot-azure-files).
