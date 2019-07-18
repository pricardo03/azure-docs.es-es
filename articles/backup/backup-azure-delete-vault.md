---
title: Eliminación de un almacén de Recovery Services en Azure
description: Se describe cómo eliminar un almacén de Recovery Services.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 07/02/2019
ms.author: raynew
ms.openlocfilehash: e195d9a4b9d2bbe21848e083dbccf864188e0790
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67508456"
---
# <a name="delete-a-recovery-services-vault"></a>Eliminación de un almacén de Recovery Services

En este artículo se describe cómo eliminar un almacén de Recovery Services de [Azure Backup](backup-overview.md). Contiene instrucciones para quitar las dependencias y, a continuación, eliminar un almacén, así como eliminar un almacén a la fuerza.


## <a name="before-you-start"></a>Antes de comenzar

Antes de comenzar, es importante entender que no se puede eliminar un almacén de Recovery Services que tiene servidores registrados en él o que contiene datos de copia de seguridad.

- Para eliminar correctamente un almacén, anule el registro de los servidores que contiene, quite los datos del almacén y, a continuación, elimine el almacén.
- Si se intenta eliminar un almacén que todavía tiene dependencias, se emite un mensaje de error y deberá quitar manualmente las dependencias del almacén, incluidos:
    - Los elementos de los que se ha hecho una copia de seguridad.
    - Los servidores protegidos.
    - Realice la copia de seguridad de los servidores de administración (Azure Backup Server y DPM) y ![seleccione el almacén para abrir su panel](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png).
- Si no quiere conservar datos en el almacén de Recovery Services y quiere eliminar el almacén, puede eliminar un almacén a la fuerza.
- Si intenta eliminar un almacén, pero no puede, el almacén todavía está configurado para recibir datos de copia de seguridad.


## <a name="delete-a-vault-from-the-azure-portal"></a>Eliminación de un almacén desde Azure Portal

1. Abra el panel del almacén.  
2. En el panel, haga clic en **Eliminar**. Compruebe si quiere eliminar.

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

Si recibe un error, quite los [elementos de copia de seguridad](#remove-backup-items), los [servidores de infraestructura](#remove-azure-backup-management-servers) y los [puntos de recuperación](#remove-azure-backup-agent-recovery-points) y, a continuación, elimine el almacén.

![error de eliminación de almacén](./media/backup-azure-delete-vault/error.png)


## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Eliminación del almacén de Recovery Services mediante el cliente de Azure Resource Manager

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Instale Chocolatey desde [aquí](https://chocolatey.org/) y ejecute el comando siguiente para instalar ARMClient:

   `choco install armclient --source=https://chocolatey.org/api/v2/`
2. Inicie sesión en su cuenta de Azure y ejecute este comando:

    `ARMClient.exe login [environment name]`

3. En Azure Portal, recopile el id. de suscripción y el nombre del grupo de recursos del almacén que quiere eliminar.

Para obtener más información sobre el comando de ARMClient, consulte este [documento](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Uso del cliente de Azure Resource Manager para eliminar el almacén de Recovery Services

1. Ejecute el comando siguiente con el id. de suscripción, el nombre del grupo de recursos y el nombre de almacén. Al ejecutar el comando, se elimina el almacén y ya no tendrá ninguna dependencia.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Si el almacén no está vacío, se recibe un error que indica que no se puede eliminar el almacén porque contiene recursos. Para quitar un contenedor o elementos protegidos de un almacén, haga lo siguiente:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. En Azure Portal, compruebe que se ha eliminado el almacén.


## <a name="remove-vault-items-and-delete-the-vault"></a>Quite los elementos del almacén y elimine el almacén.

Quite todas las dependencias antes de eliminar el almacén de Recovery Services.

### <a name="remove-backup-items"></a>Eliminación de elementos de copia de seguridad

Este procedimiento proporciona un ejemplo que muestra cómo quitar datos de copia de seguridad de Azure Files.

1. Haga clic en **Elementos de copia de seguridad** > **Azure Storage (Azure Files)** .

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

2. Haga clic con el botón derecho en los elementos de Azure Files que se deben quitar y haga clic en **Detener copia de seguridad**.

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/stop-backup-item.png)


3. En **Detener copia de seguridad** > **Elegir una opción**, seleccione **Eliminar datos de copia de seguridad**.
4. Escriba el nombre del elemento y haga clic en **Detener copia de seguridad**.
   - Esta acción verifica que quiere eliminar el elemento.
   - El botón **Detener copia de seguridad** se activa después de la verificación.
   - Si se conservan y no se eliminan los datos, no podrá eliminar el almacén.

     ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. También puede proporcionar el motivo por el que va a eliminar los datos y agregar comentarios.
6. Para comprobar que el trabajo de eliminación se ha completado, compruebe los mensajes de Azure ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/messages.png).
7. Una vez finalizado el trabajo, el servicio envía un mensaje que indica que **el proceso de copia de seguridad se ha detenido y se han eliminado los datos de copia de seguridad**.
8. Después de eliminar un elemento de la lista, en el menú **Elementos de copia de seguridad**, haga clic en **Actualizar** para ver los elementos del almacén.

      ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/empty-items-list.png)

## <a name="deleting-backup-items-from-management-console"></a>Eliminación de elementos de copia de seguridad de la consola de administración

Para eliminar los elementos de copia de seguridad de la infraestructura de copia de seguridad, vaya a la consola de administración (MARS, Azure Backup Server o SC DPM en función de dónde se protegen los elementos de copia de seguridad) del servidor local.

### <a name="for-mars-agent"></a>Para el agente de MARS

- Inicie la consola de administración de MARS, vaya al panel **Acciones** y elija **Programar copia de seguridad**.
- En el asistente **Modificar o detener una copia de seguridad programada**, elija la opción **Dejar de utilizar esta programación de copias de seguridad y eliminar todas las copias de seguridad almacenadas** y haga clic en **Siguiente**.

    ![Modificación o detención de una copia de seguridad programada](./media/backup-azure-delete-vault/modify-schedule-backup.png)

- En el asistente **Detener copia de seguridad programada**, haga clic en **Finalizar**.

    ![Detención de una copia de seguridad programada](./media/backup-azure-delete-vault/stop-schedule-backup.png)
- Se le solicita que escriba un PIN de seguridad. Para generar el PIN, realice los pasos siguientes:
  - Inicie sesión en el Portal de Azure.
  - Vaya al **almacén de Recovery Services** > **Configuración** > **Propiedades**.
  - En **PIN de seguridad**, haga clic en **Generar**. Copie este código PIN. (Este PIN solo es válido durante cinco minutos).
- En la consola de administración (aplicación cliente), pegue el código PIN y haga clic en **Aceptar**.

  ![PIN de seguridad](./media/backup-azure-delete-vault/security-pin.png)

- En el asistente **Modificar progreso de la copia de seguridad** verá que los *datos de copia de seguridad eliminados se conservarán durante 14 días. Tras ese período, se eliminarán permanentemente los datos de copia de seguridad.*  

    ![Eliminación de la infraestructura de copia de seguridad](./media/backup-azure-delete-vault/deleted-backup-data.png)

Ahora que ha eliminado los elementos de copia de seguridad del entorno local, complete los pasos siguientes desde el portal:
- Para MARS, siga los pasos descritos en [Eliminación de puntos de recuperación del agente de Azure Backup](#remove-azure-backup-agent-recovery-points).

### <a name="for-mabs-agent"></a>Para el agente de MABS

Existen diferentes métodos para detener o eliminar la protección en línea; lleve a cabo cualquiera de los siguientes métodos:

**Método 1**

Inicie la consola de **administración de MABS**. En la sección **Seleccionar método de protección de datos**, anule la selección de la opción **Deseo protección en línea**.

  ![seleccionar método de protección de datos](./media/backup-azure-delete-vault/data-protection-method.png)

**Método 2**

Para eliminar un grupo de protección, primero debe detener la protección del grupo. Use el procedimiento siguiente para detener la protección y permitir la eliminación de un grupo de protección.

1.  En la Consola de administrador DPM, haga clic en **Protección** en la barra de navegación.
2.  En el panel de información, seleccione el miembro del grupo de protección que quiere quitar. Haga clic con el botón derecho para elegir la opción **Stop Protection of Group Members** (Detener la protección de miembros del grupo).
3.  Desde el cuadro de diálogo **Detener protección**, seleccione la casilla **Eliminar datos protegidos** > **Eliminar almacenamiento en línea** y, a continuación, haga clic en **Detener protección**.

    ![Eliminación de almacenamiento en línea](./media/backup-azure-delete-vault/delete-storage-online.png)

Ahora se cambia el estado de miembro protegido a **Réplica inactiva disponible**.

5. Haga clic en el grupo de protección inactiva y seleccione **Quitar protección inactiva**.

    ![Quitar protección inactiva](./media/backup-azure-delete-vault/remove-inactive-protection.png)

6. Desde la ventana **Eliminar protección inactiva**, seleccione **Eliminar almacenamiento en línea** y haga clic en **Aceptar**.

    ![Quitar las réplicas en el disco y en línea](./media/backup-azure-delete-vault/remove-replica-on-disk-and-online.png)

Ahora que ha eliminado los elementos de copia de seguridad del entorno local, complete los pasos siguientes desde el portal:
- Para MABS y DPM, siga los pasos descritos en [Eliminación de servidores de administración de Azure Backup](#remove-azure-backup-management-servers).


### <a name="remove-azure-backup-management-servers"></a>Eliminación de servidores de administración de Azure Backup

Antes de quitar el servidor de administración de Azure Backup, asegúrese de realizar los pasos descritos en [Eliminación de elementos de copia de seguridad de la consola de administración](#deleting-backup-items-from-management-console).

1. En el menú del panel del almacén, haga clic en **Infraestructura de copia de seguridad**.
2. Haga clic en **Servidores de administración de copias de seguridad** para ver los servidores.

    ![Seleccionar el almacén para abrir su panel.](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

3. Haga clic con el botón derecho en el elemento > **Eliminar**.
4. En el menú **Eliminar**, escriba el nombre del servidor y haga clic en **Eliminar**.

     ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)
5.  También puede proporcionar el motivo por el que va a eliminar los datos y agregar comentarios.

> [!NOTE]
> Si ve el siguiente error, primero siga los pasos indicados en la sección [Eliminación de elementos de copia de seguridad de la consola de administración](#deleting-backup-items-from-management-console).
>
>![error en la eliminación](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Si no puede realizar los pasos para eliminar las copias de seguridad de la consola de administración, por ejemplo, debido a la indisponibilidad del servidor con la consola de administración, póngase en contacto con soporte técnico de Microsoft.

6. Para comprobar que el trabajo de eliminación se ha completado, compruebe los mensajes de Azure ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/messages.png).
7. Una vez finalizado el trabajo, el servicio envía un mensaje que indica que **el proceso de copia de seguridad se ha detenido y se han eliminado los datos de copia de seguridad**.
8. Después de eliminar un elemento de la lista, en el menú **Infraestructura de copia de seguridad**, haga clic en **Actualizar** para ver los elementos del almacén.


### <a name="remove-azure-backup-agent-recovery-points"></a>Quitar puntos de recuperación del agente de Azure Backup

Antes de quitar puntos de recuperación de Azure Backup, asegúrese de realizar los pasos descritos en [Eliminación de elementos de copia de seguridad de la consola de administración](#deleting-backup-items-from-management-console).

1. En el menú del panel del almacén, haga clic en **Infraestructura de copia de seguridad**.
2. Haga clic en **Servidores protegidos** para ver los servidores de infraestructura.

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. En la lista **Servidores protegidos**, haga clic en el agente de Azure Backup.

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Haga clic en el servidor en la lista de servidores protegidos con el agente de Azure Backup.

    ![Seleccionar el servidor protegido específico](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. En el panel del servidor seleccionado, haga clic en **Eliminar**.

    ![Eliminar el servidor seleccionado](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. En el menú **Eliminar**, escriba el nombre del servidor y haga clic en **Eliminar**.

     ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. También puede proporcionar el motivo por el que va a eliminar los datos y agregar comentarios.

> [!NOTE]
> Si ve el siguiente error, primero siga los pasos indicados en la sección [Eliminación de elementos de copia de seguridad de la consola de administración](#deleting-backup-items-from-management-console).
>
>![error en la eliminación](./media/backup-azure-delete-vault/deletion-failed.png)
>
> Si no puede realizar los pasos para eliminar las copias de seguridad de la consola de administración, por ejemplo, debido a la indisponibilidad del servidor con la consola de administración, póngase en contacto con soporte técnico de Microsoft. 

8. Para comprobar que el trabajo de eliminación se ha completado, compruebe los mensajes de Azure ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/messages.png).
9. Después de eliminar un elemento de la lista, en el menú **Infraestructura de copia de seguridad**, haga clic en **Actualizar** para ver los elementos del almacén.


### <a name="delete-the-vault-after-removing-dependencies"></a>Eliminación del almacén después de quitar las dependencias

1. Cuando se hayan quitado todas las dependencias, desplácese al panel **Essentials** en el menú del almacén.
2. Compruebe que no haya **elementos de copia de seguridad**, **servidores de administración de copias de seguridad** ni **elementos replicados** en la lista. Si todavía aparecen elementos en el almacén, quítelos.

3. Cuando no haya ningún otro elemento en el almacén, haga clic en **Eliminar** en el panel del almacén.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Para verificar que desea eliminar el almacén, haga clic en **Sí**. Se elimina el almacén y el portal vuelve al menú **Nuevo servicio** .

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>¿Qué ocurre si detengo el proceso de copia de seguridad pero retengo los datos?

Si detiene el proceso de copia de seguridad, pero accidentalmente retiene los datos, debe eliminar los datos de copia de seguridad conforme se describe en las secciones anteriores.

## <a name="next-steps"></a>Pasos siguientes

[Más información](backup-azure-recovery-services-vault-overview.md) sobre los almacenes de Recovery Services.
