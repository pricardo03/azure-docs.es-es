---
title: Eliminar un almacén de Recovery Services en Azure
description: Describe cómo eliminar un almacén de Recovery Services.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: raynew
ms.openlocfilehash: a7dd5530c3941fe55e8a649f8adb217159823f5d
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66492791"
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


## <a name="delete-the-recovery-services-vault-using-azure-resource-manager-client"></a>Eliminación del almacén de Recovery Services mediante el cliente de Azure Resource Manager

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Instalar chocolatey desde [aquí](https://chocolatey.org/) e instalar ARMClient que se ejecute el comando siguiente:

   ` choco install armclient --source=https://chocolatey.org/api/v2/ `
2. Inicie sesión en la cuenta de Azure, ejecute el comando siguiente

    ` ARMClient.exe login [environment name] `

3. En el portal de Azure, recopile el nombre del grupo suscripción ID y recursos para el almacén que desea eliminar.

Para obtener más información sobre el comando de ARMClient, hacer referencia a esta [documento](https://github.com/projectkudu/ARMClient/blob/master/README.md).

### <a name="use-azure-resource-manager-client-to-delete-recovery-services-vault"></a>Use el cliente de Azure Resource Manager para eliminar el almacén de Recovery Services

1. Ejecute el comando siguiente con el Id. de suscripción, el nombre del grupo de recursos y el nombre del almacén. Al ejecutar el comando elimina el almacén si no tienen ninguna dependencia.

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
2. Si el almacén Value no de vacío, recibe el error "No se puede eliminar porque hay recursos existentes dentro de este almacén". Para quitar un elementos protegidos / contenedor dentro de un almacén, haga lo siguiente:

   ```
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

3. En el portal de Azure, compruebe que se ha eliminado el almacén.


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

3. Haga clic en el elemento > **eliminar**.
4. Para comprobar que el trabajo de eliminación completado, compruebe los mensajes de Azure ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/messages.png).
5. Una vez completado el trabajo, el servicio envía un mensaje: **se detuvo el proceso de copia de seguridad y se han eliminado los datos de copia de seguridad**.
6. Después de eliminar un elemento en la lista, en el **infraestructura de copia de seguridad** menú, haga clic en **actualizar** para ver los elementos en el almacén.


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

     ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

7. Opcionalmente, proporcionar un motivo por qué está eliminando los datos y agregar comentarios.
8. Para comprobar que el trabajo de eliminación completado, compruebe los mensajes de Azure ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/messages.png).
9. Después de eliminar un elemento en la lista, en el **infraestructura de copia de seguridad** menú, haga clic en **actualizar** para ver los elementos en el almacén.

### <a name="delete-the-vault-after-removing-dependencies"></a>Eliminar el almacén después de quitar las dependencias

1. Cuando se han quitado todas las dependencias, desplácese a la **Essentials** panel en el menú del almacén.
2. Compruebe que no existe ninguna **elementos de copia de seguridad**, **servidores de administración de copia de seguridad**, o **elementos replicados** enumerados. Si todavía aparecen elementos en el almacén, quitarlos.

3. Cuando no haya ningún otro elemento en el almacén, haga clic en **Eliminar** en el panel del almacén.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

4. Para verificar que desea eliminar el almacén, haga clic en **Sí**. Se elimina el almacén y el portal vuelve al menú **Nuevo servicio** .

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>¿Qué ocurre si detengo el proceso de copia de seguridad pero retengo los datos?

Si detener el proceso de copia de seguridad pero mantener accidentalmente los datos, debe eliminar los datos de copia de seguridad, como se describe en las secciones anteriores.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información sobre](backup-azure-recovery-services-vault-overview.md) almacenes de Recovery Services.
