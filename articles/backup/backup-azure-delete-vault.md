---
title: Eliminar un almacén de Recovery Services en Azure
description: Describe cómo eliminar un almacén de Recovery Services.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/28/2019
ms.author: raynew
ms.openlocfilehash: 94d66e28f8edbda6c41dcceaf427d7d7d869c90f
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620124"
---
# <a name="delete-a-recovery-services-vault"></a>Eliminación de un almacén de Recovery Services

En este artículo se describe cómo eliminar un [Azure Backup](backup-overview.md) almacén de Recovery Services. Contiene instrucciones para quitar las dependencias y, a continuación, eliminar un almacén y eliminar un almacén a la fuerza.


## <a name="before-you-start"></a>Antes de comenzar

Antes de empezar, es importante entender que no se puede eliminar un almacén de Recovery Services que tiene servidores registrados en él, o que contiene datos de copia de seguridad.

- Para eliminar un almacén correctamente, anular el registro de servidores en él, quitar los datos del almacén y, a continuación, elimine el almacén.
- Si se intenta eliminar un almacén que todavía tiene dependencias, se emite un mensaje de error. y deberá quitar manualmente las dependencias del almacén, incluyendo:
    - Copia los elementos de seguridad
    - Servidores protegidos
    - Copia de seguridad de servidores de administración (servidor de copia de seguridad de Azure, DPM) ![seleccione el almacén para abrir su panel](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)
- Si no desea conservar los datos en el almacén de Recovery Services y quiere eliminar el almacén, puede eliminar el almacén a la fuerza.
- Si intenta eliminar un almacén, pero no puede, el almacén todavía está configurado para recibir datos de copia de seguridad.


## <a name="delete-a-vault-from-the-azure-portal"></a>Eliminar un almacén de Azure portal

1. Abra el panel del almacén.  
2. En el panel, haga clic en **eliminar**. Compruebe que desea eliminar.

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

Si recibe un error, quite [elementos de copia de seguridad](#remove-backup-items), [servidores de infraestructura](#remove-backup-infrastructure-servers), y [puntos de recuperación](#remove-azure-backup-agent-recovery-points)y, a continuación, elimine el almacén.

![Eliminar error de almacén](./media/backup-azure-delete-vault/error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>Eliminación del almacén de Recovery Services a la fuerza

Puede eliminar un almacén a la fuerza con PowerShell. Forzar la eliminación significa que se elimina permanentemente del almacén y todos sus datos de copia de seguridad.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]


Para eliminar un almacén a la fuerza:

1. Inicie sesión en su suscripción de Azure con el `Connect-AzAccount` comando y siga el en pantalla direcciones.

   ```powershell
    Connect-AzAccount
   ```
2. La primera vez que use Azure Backup, debe registrar el proveedor de Azure Recovery Services en su suscripción con [Register-AzResourceProvider](/powershell/module/az.Resources/Register-azResourceProvider).

   ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

3. Abra la ventana de PowerShell con privilegios de administrador.
4. Utilice `Set-ExecutionPolicy Unrestricted` para quitar las restricciones.
5. Ejecute el comando siguiente para descargar el paquete de cliente de Azure Resource Manager desde chocolately.org.

    `iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

6. Use el siguiente comando para instalar el cliente de API de Azure Resource Manager.

   `choco.exe install armclient`

7. En el portal de Azure, recopile el nombre del grupo suscripción ID y recursos para el almacén que desea eliminar.
8. En PowerShell, ejecute el comando siguiente con el Id. de suscripción, el nombre del grupo de recursos y el nombre del almacén. Al ejecutar el comando, se elimina el almacén y todas las dependencias.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
9. Si el almacén Value no de vacío, recibe el error "No se puede eliminar porque hay recursos existentes dentro de este almacén". Para quitar un contenida dentro de un almacén, haga lo siguiente:

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

10. En el portal de Azure, compruebe que se ha eliminado el almacén.


## <a name="remove-vault-items-and-delete-the-vault"></a>Quitar elementos del almacén y la eliminación del almacén

Estos procedimientos se proporcionan algunos ejemplos para quitar datos de copia de seguridad y los servidores de infraestructura. Después de quita todo el contenido de un almacén, puede eliminarlo.

### <a name="remove-backup-items"></a>Quitar elementos de copia de seguridad

Este procedimiento proporciona un ejemplo que muestra cómo quitar datos de copia de seguridad de Azure Files.

1. Haga clic en **elementos de copia de seguridad** > **Azure Storage (Azure Files)**

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

2. Haga clic en cada elemento de archivos de Azure para quitar y haga clic en **Detener copia de seguridad**.

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/stop-backup-item.png)


3. En **Detener copia de seguridad** > **elija una opción**, seleccione **eliminar datos de copia de seguridad**.
4. Escriba el nombre del elemento y haga clic en **Detener copia de seguridad**. 
   - Esto confirma que desea eliminar el elemento.
   - El **Detener copia de seguridad** botón activa después de comprobar.
   - Si se conservan y no eliminan los datos, no podrá eliminar el almacén.

     ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Opcionalmente, proporcionar un motivo por qué está eliminando los datos y agregar comentarios.
6. Para comprobar que el trabajo de eliminación completado, compruebe los mensajes de Azure ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/messages.png).
7. Una vez completado el trabajo, el servicio envía un mensaje: **se detuvo el proceso de copia de seguridad y se han eliminado los datos de copia de seguridad**.
8. Después de eliminar un elemento de la lista, en el menú **Elementos de copia de seguridad**, haga clic en **Actualizar** para ver los elementos del almacén.

      ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/empty-items-list.png)


### <a name="remove-backup-infrastructure-servers"></a>Quitar servidores de infraestructura de copia de seguridad

1. En el menú del panel del almacén, haga clic en **infraestructura de copia de seguridad**.
2. Haga clic en **servidores de administración de copia de seguridad** para ver los servidores. 

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

2. Haga clic en el elemento > **eliminar**.

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

3. . En **Detener copia de seguridad** > **elija una opción**, seleccione **eliminar datos de copia de seguridad**.
4. Escriba el nombre del elemento y haga clic en **Detener copia de seguridad**. 
   - Esto confirma que desea eliminar el elemento.
   - El **Detener copia de seguridad** botón activa después de comprobar.
   - Si se conservan y no eliminan los datos, no podrá eliminar el almacén.

     ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

5. Opcionalmente, proporcionar un motivo por qué está eliminando los datos y agregar comentarios.
6. Para comprobar que el trabajo de eliminación completado, compruebe los mensajes de Azure ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/messages.png).
7. Una vez completado el trabajo, el servicio envía un mensaje: **se detuvo el proceso de copia de seguridad y se han eliminado los datos de copia de seguridad**.
8. Después de eliminar un elemento de la lista, en el menú **Elementos de copia de seguridad**, haga clic en **Actualizar** para ver los elementos del almacén.


### <a name="remove-azure-backup-agent-recovery-points"></a>Quitar puntos de recuperación de agente de copia de seguridad de Azure

1. En el menú del panel del almacén, haga clic en **infraestructura de copia de seguridad**.
2. Haga clic en **servidores de administración de copia de seguridad** para ver los servidores de infraestructura.

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/identify-protected-servers.png)

3. En la lista **Servidores protegidos**, haga clic en el agente de Azure Backup.

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

4. Haga clic en el servidor en la lista de servidores protegidos con Azure Backup agent.

    ![Seleccionar el servidor protegido específico](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

5. En el panel del servidor seleccionado, haga clic en **eliminar**.

    ![Eliminar el servidor seleccionado](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

6. En el menú **Eliminar**, escriba el nombre del elemento y haga clic en **Eliminar**.
   - Esto confirma que desea eliminar el elemento.
   - El **Detener copia de seguridad** botón activa después de comprobar.
   - Si se conservan y no eliminan los datos, no podrá eliminar el almacén.

     ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. Opcionalmente, proporcionar un motivo por qué está eliminando los datos y agregar comentarios.
8. Para comprobar que el trabajo de eliminación completado, compruebe los mensajes de Azure ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/messages.png).
7. Una vez completado el trabajo, el servicio envía un mensaje: **se detuvo el proceso de copia de seguridad y se han eliminado los datos de copia de seguridad**.
8. Después de eliminar un elemento de la lista, en el menú **Elementos de copia de seguridad**, haga clic en **Actualizar** para ver los elementos del almacén.






### <a name="delete-the-vault-after-removing-dependencies"></a>Eliminar el almacén después de quitar las dependencias

1. Cuando se han quitado todas las dependencias, desplácese a la **Essentials** panel en el menú del almacén.
2. Compruebe que no existe ninguna **elementos de copia de seguridad**, **servidores de administración de copia de seguridad**, o **elementos replicados** enumerados. Si todavía aparecen elementos en el almacén, quitarlos.

2. Cuando no haya ningún otro elemento en el almacén, haga clic en **Eliminar** en el panel del almacén.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Para verificar que desea eliminar el almacén, haga clic en **Sí**. Se elimina el almacén y el portal vuelve al menú **Nuevo servicio** .

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>¿Qué ocurre si detengo el proceso de copia de seguridad pero retengo los datos?

Si detener el proceso de copia de seguridad pero mantener accidentalmente los datos, debe eliminar los datos de copia de seguridad, como se describe en las secciones anteriores.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información sobre](backup-azure-recovery-services-vault-overview.md) almacenes de Recovery Services.
