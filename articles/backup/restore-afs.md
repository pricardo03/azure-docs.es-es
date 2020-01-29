---
title: Restauración de recursos compartidos de archivos de Azure
description: Aprenda a usar Azure Portal para restaurar un recurso compartido de archivos completo o archivos específicos desde un punto de restauración creado por el servicio Azure Backup.
ms.topic: conceptual
ms.date: 01/12/2020
ms.openlocfilehash: b16eb4120ff2d269135ae8ae6555ef4fdbdbda5d
ms.sourcegitcommit: 7221918fbe5385ceccf39dff9dd5a3817a0bd807
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/21/2020
ms.locfileid: "76294846"
---
# <a name="restore-azure-file-shares"></a>Restauración de recursos compartidos de archivos de Azure

En este artículo se explica cómo usar Azure Portal para restaurar un recurso compartido de archivos completo o archivos específicos desde un punto de restauración creado por el servicio [Azure Backup](https://docs.microsoft.com/azure/backup/backup-overview).

En esta guía, obtendrá información sobre:

* Restauración de un recurso compartido de archivos de Azure completo
* Restauración de archivos o carpetas individuales
* Seguimiento del estado de la operación de restauración

## <a name="steps-to-perform-restore"></a>Pasos para hacer la restauración

### <a name="select-the-file-share-to-restore"></a>Selección del recurso compartido de archivos que se va a restaurar

1. En [Azure Portal](https://portal.azure.com/), abra el almacén de Recovery Services que usó para configurar la copia de seguridad del recurso compartido de archivos.

2. Haga clic en **Elementos de copia de seguridad** en la sección **Elementos protegidos** de la hoja **Información general**.

    ![Clic en Elementos de copia de seguridad](./media/restore-afs/backup-items.png)

3. Después de hacer clic en **Elementos de copia de seguridad**, se muestra una nueva hoja con todos los tipos de administración de copia de seguridad junto a la hoja **Información general**, de la siguiente manera:

    ![Tipos de administración de copias de seguridad](./media/restore-afs/backup-management.png)

4. En **Elementos de copia de seguridad**, en **Tipo de administración de copias de seguridad**, seleccione **Azure Storage (Azure Files)** . Se ve una lista de todos los recursos compartidos de archivos y la cuenta de almacenamiento correspondiente cuya copia de seguridad se ha realizado mediante este almacén.

    ![Lista de todos los recursos compartidos de archivos](./media/restore-afs/file-shares.png)

5. En la lista de recursos compartidos de archivos de Azure, seleccione el recurso compartido de archivos para el que desea realizar la operación de restauración.

### <a name="full-share-recovery"></a>Recuperación del recurso compartido completo

Puede usar esta opción para restaurar el recurso compartido de archivos completo en la ubicación original o en otra alternativa.

1. Seleccione la opción **Restaurar recurso compartido** de la hoja **Elemento de copia de seguridad** que aparece después de seleccionar el recurso compartido de archivos que desee restaurar en el paso 5 de la sección [Selección del recurso compartido de archivos que se va a restaurar](#select-the-file-share-to-restore).

   ![Selección de Restaurar recurso compartido](./media/restore-afs/restore-share.png)

2. Una vez que haga clic en **Restaurar recurso compartido**, se abre la hoja **Restaurar** con el menú **Punto de restauración**, que muestra la lista de puntos de restauración disponibles para el recurso compartido de archivos seleccionado.

3. Seleccione el punto de restauración que desee usar para la operación de restauración y haga clic en **Aceptar**.

    ![Selección de Punto de restauración](./media/restore-afs/restore-point.png)

4. Una vez que haga clic en Aceptar, el menú de la hoja Restaurar cambiará a **Ubicación de restauración**. En **Ubicación de restauración**, especifique dónde o cómo quiere restaurar los datos. Puede elegir una de las dos siguientes opciones:

    * **Ubicación original**: restaure el recurso compartido de archivos completo en la misma ubicación que el origen inicial.
    * **Ubicación alternativa**: restaure el recurso compartido de archivos completo en una ubicación alternativa y mantenga el recurso compartido de archivos original **como está**.

#### <a name="restore-to-original-location"></a>Restauración en la ubicación original

1. Elija **Ubicación original** como **Destino de recuperación** y seleccione si desea omitir o sobrescribir si hay conflictos. Haga clic en **Aceptar** después de hacer la selección adecuada.

    ![Elección de la ubicación original](./media/restore-afs/original-location.png)

2. Haga clic en **Restaurar** para iniciar la restauración.

    ![Clic en Restaurar para iniciar](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Restauración en una ubicación alternativa

1. Elija **Ubicación alternativa** como destino de recuperación.
2. Seleccione la cuenta de almacenamiento de destino en la que desea restaurar el contenido del que se ha realizado una copia de seguridad, en el menú desplegable del campo **Cuenta de almacenamiento**.
3. En función de la cuenta de almacenamiento que seleccionó en el paso 2, el menú desplegable **Seleccionar un recurso compartido de archivos** mostrará la lista de recursos compartidos de archivos presentes en la cuenta de almacenamiento seleccionada. Seleccione el recurso compartido de archivos en el que desea restaurar el contenido de la copia de seguridad.
4. En el campo **Nombre de la carpeta**, especifique el nombre de la carpeta que desea crear en el recurso compartido de archivos de destino con el contenido restaurado.
5. Seleccione si desea omitir o sobrescribir si hay conflictos.
6. Haga clic en **Aceptar** después de escribir los valores adecuados en todos los campos.

    ![Selección de Ubicación alternativa](./media/restore-afs/alternate-location.png)

7. Haga clic en Restaurar para iniciar la operación de restauración.

    ![Clic en Restaurar para iniciar](./media/restore-afs/click-restore.png)

### <a name="item-level-recovery"></a>Recuperación en el nivel de elementos

Puede usar esta opción para restaurar archivos o carpetas en la ubicación original o en otra alternativa.

1. Seleccione la opción **Recuperación de archivos** de la hoja **Elemento de copia de seguridad** que aparece después de seleccionar el recurso compartido de archivos que desee restaurar en el paso 5 de la sección [Selección del recurso compartido de archivos que se va a restaurar](#select-the-file-share-to-restore).

    ![Selección de Recuperación de archivos](./media/restore-afs/file-recovery.png)

2. Una vez que haga clic en **Recuperación de archivos**, se abre la hoja **Restaurar** con el menú **Punto de restauración**, que muestra la lista de puntos de restauración disponibles para el recurso compartido de archivos seleccionado.

3. Seleccione el punto de restauración que desee usar para la operación de restauración y haga clic en **Aceptar**.

    ![Selección de Punto de restauración](./media/restore-afs/restore-point.png)

4. Una vez que haga clic en Aceptar, el menú de la hoja Restaurar cambiará a **Ubicación de restauración**. En **Ubicación de restauración**, especifique dónde o cómo quiere restaurar los datos. Puede elegir una de las dos siguientes opciones:

    * **Ubicación original**: restaure los archivos o carpetas seleccionados en el mismo recurso compartido de archivos que el origen inicial.
    * **Ubicación alternativa**: restaure los archivos o carpetas seleccionados en una ubicación alternativa y mantenga el contenido del recurso compartido de archivos original **como está**.

#### <a name="restore-to-original-location"></a>Restauración en una ubicación original

1. Elija **Ubicación original** como **Destino de recuperación** y seleccione si desea omitir o sobrescribir si hay conflictos.

    ![Ubicación original de la recuperación en el nivel de elementos](./media/restore-afs/original-location-item-level.png)

2. Haga clic en **Seleccionar archivo** para elegir los archivos o carpetas que desea restaurar.

    ![Clic en Seleccionar archivo](./media/restore-afs/select-file.png)

3. Al hacer clic en **Seleccionar archivo**, se muestra una hoja del recurso compartido de archivos que muestra el contenido del punto de recuperación del recurso compartido de archivos seleccionado para restaurar.

4. Active la casilla correspondiente al archivo o carpeta que desea restaurar y haga clic en **Seleccionar**.

    ![Selección de un archivo o una carpeta](./media/restore-afs/select-file-folder.png)

5. Repita los pasos del 2 al 4 para seleccionar varios archivos o carpetas para la restauración.
6. Después de seleccionar todos los elementos que desea restaurar, haga clic en **Aceptar**.

    ![Después de seleccionar todos los elementos que desea restaurar, haga clic en Aceptar](./media/restore-afs/after-selecting-items.png)

7. Haga clic en Restaurar para iniciar la restauración.

    ![Clic en Restaurar para iniciar](./media/restore-afs/click-restore.png)

#### <a name="restore-to-an-alternate-location"></a>Restauración a una ubicación alternativa

1. Elija **Ubicación alternativa** como destino de recuperación.
2. Seleccione la cuenta de almacenamiento de destino en la que desea restaurar el contenido del que se ha realizado una copia de seguridad, en el menú desplegable del campo **Cuenta de almacenamiento**.
3. En función de la cuenta de almacenamiento que seleccionó en el paso 2, el menú desplegable **Seleccionar un recurso compartido de archivos** mostrará la lista de recursos compartidos de archivos presentes en la cuenta de almacenamiento seleccionada. Seleccione el recurso compartido de archivos en el que desea restaurar el contenido de la copia de seguridad.
4. En el campo **Nombre de la carpeta**, especifique el nombre de la carpeta que desea crear en el recurso compartido de archivos de destino con el contenido restaurado.
5. Seleccione si desea omitir o sobrescribir si hay conflictos.
6. Haga clic en **Seleccionar archivo** para elegir los archivos o carpetas que desea restaurar.

    ![Selección de elementos para restaurar en una ubicación alternativa](./media/restore-afs/restore-to-alternate-location.png)

7. Al hacer clic en **Seleccionar archivo**, se muestra una hoja del recurso compartido de archivos que muestra el contenido del punto de recuperación del recurso compartido de archivos seleccionado para restaurar.
8. Marque la casilla correspondiente al archivo o carpeta que desea restaurar y haga clic en **Seleccionar**.

    ![Selección del destino de recuperación](./media/restore-afs/recovery-destination.png)

9. Repita los pasos del 6 al 8 para seleccionar varios archivos o carpetas para la restauración.
10. Después de seleccionar todos los elementos que desea restaurar, haga clic en **Aceptar**.

    [Haga clic en Aceptar después de seleccionar todos los archivos](./media/restore-afs/after-selecting-all-items.png)

11. Haga clic en **Restaurar** para iniciar la restauración.

## <a name="track-restore-operation"></a>Seguimiento de la operación de restauración

Una vez que se desencadene la operación de restauración, el servicio de copia de seguridad crea un trabajo para realizar su seguimiento. Azure Backup muestra las notificaciones sobre el trabajo en el portal. Para ver las operaciones del trabajo, haga clic en el hipervínculo de notificaciones.

![Clic en hipervínculo de notificaciones](./media/restore-afs/notifications-link.png)

También puede supervisar el progreso de la restauración desde el almacén de Recovery Services. Estos son los pasos para comprobar el estado de la restauración:

1. Abra el almacén de Recovery Services desde donde se generó la operación de restauración.
2. Haga clic en **Trabajos de copia de seguridad** en la **sección Supervisión** de la hoja **Información general** para ver el estado de las operaciones que se ejecutan en cargas de trabajo diferentes.

    ![Clic en Trabajos de copias de seguridad](./media/restore-afs/backup-jobs.png)

3. Haga clic en el nombre de la carga de trabajo correspondiente a su recurso compartido de archivos para ver más detalles sobre la restauración, como los datos transferidos, el número de archivos restaurados, etc.

    ![Consulte los detalles restaurados](./media/restore-afs/restore-details.png)

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [administrar copias de seguridad de recursos compartidos de archivos de Azure](manage-afs-backup.md)
