---
title: Eliminación de un almacén de Recovery Services en Azure
description: En este artículo se explica cómo eliminar un almacén de Recovery Services. El artículo incluye pasos para solucionar los problemas que se producen cuando se intenta eliminar un almacén, pero no puede.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: raynew
ms.openlocfilehash: d15e3773a9b6e3dceb0799d206070730675c211d
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/15/2019
ms.locfileid: "56310973"
---
# <a name="delete-a-recovery-services-vault"></a>Eliminación de un almacén de Recovery Services

En este artículo, se explica cómo se quitan todos los elementos de un almacén de Recovery Services para después eliminarlo. Puede eliminar un almacén de Recovery Services si está registrado en un servidor y retiene datos de copia de seguridad. Si intenta eliminar un almacén, pero no puede, el almacén todavía está configurado para recibir datos de copia de seguridad.

Para obtener información sobre cómo eliminar un almacén, consulte la sección [Delete a vault from Azure Portal](backup-azure-delete-vault.md#delete-a-vault-from-azure-portal) (Eliminación de un almacén de Azure Portal). Si no quiere conservar datos en el almacén de Recovery Services y quiere eliminar el almacén, consulte la sección [Delete the vault by force](backup-azure-delete-vault.md#delete-the-recovery-services-vault-by-force) (Eliminación de un almacén a la fuerza). Si no está seguro de lo que hay en el almacén y tiene que asegurarse de que puede eliminarlo, consulte la sección [Remove vault dependencies and delete vault](backup-azure-delete-vault.md#remove-vault-dependencies-and-delete-vault) (Quitar las dependencias del almacén y eliminarlo).

## <a name="delete-a-vault-from-azure-portal"></a>Eliminación de un almacén de Azure Portal

Si ya tiene abierto el almacén de Recovery Services, vaya al segundo paso.

1. Abra Azure Portal y, desde el panel, abra el almacén que desea eliminar.

   Si el almacén de Recovery Services no está anclado al panel, en el menú Centro, haga clic en **Todos los servicios** y, en la lista de recursos, escriba **Recovery Services**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Para ver la lista de almacenes de la suscripción, haga clic en **Almacenes de Recovery Services**.

   ![Creación del almacén de Recovery Services, paso 1](./media/backup-azure-delete-vault/open-recovery-services-vault.png) <br/>

   Se muestra la lista de almacenes de Recovery Services.

   ![elegir el almacén en la lista](./media/backup-azure-delete-vault/choose-vault-to-delete-.png)

1. En la lista, seleccione el almacén que desea eliminar. Al hacer clic en el almacén, se abre su panel.

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/contoso-bkpvault-settings.png)

1. Para eliminar un almacén, en el panel de almacén, haga clic en **Eliminar**. Se le pedirá que confirme si quiere eliminar el almacén.

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/click-delete-button-to-delete-vault.png)

    Si aparece el **error de eliminación del almacén**, puede quitar las dependencias del almacén o usar PowerShell para eliminar el almacén a la fuerza. En las secciones siguientes se explica cómo llevar a cabo estas tareas.

    ![Error de eliminación del almacén](./media/backup-azure-delete-vault/vault-delete-error.png)


## <a name="delete-the-recovery-services-vault-by-force"></a>Eliminación del almacén de Recovery Services a la fuerza

Puede usar PowerShell para eliminar un almacén de Recovery Services a la fuerza. Esto significa que se eliminarán permanentemente el almacén de Recovery Services y todos los datos de copia de seguridad asociados.

> [!Warning]
> Al usar PowerShell para eliminar un almacén de Recovery Services, asegúrese de que quiere eliminar permanentemente todos los datos de copia de seguridad del almacén.
>

Para eliminar un almacén de Recovery Services:

1. Inicie sesión en la cuenta de Azure.

   Inicie sesión en la suscripción a Azure con el comando `Connect-AzureRmAccount` y siga las instrucciones de la pantalla.

   ```powershell
    Connect-AzureRmAccount
   ```
   La primera vez que use Azure Backup, debe registrar el proveedor de Azure Recovery Services en su suscripción con [Register-AzureRmResourceProvider](/powershell/module/AzureRM.Resources/Register-AzureRmResourceProvider).

   ```powershell
    Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
   ```

1. Abra la ventana de PowerShell con privilegios de administrador.

1. Utilice `Set-ExecutionPolicy Unrestricted` para quitar las restricciones.

1. Ejecute el comando siguiente para descargar el paquete de cliente de Azure Resource Manager desde chocolately.org.

    `iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

1. Use el siguiente comando para instalar el cliente de API de Azure Resource Manager.

   `choco.exe install armclient`

1. En Azure Portal, recopile el id. de suscripción y el nombre del grupo de recursos asociado del almacén de Recovery Services que quiere eliminar.

1. En PowerShell, ejecute el comando siguiente con el id. de suscripción, el nombre del grupo de recursos y el nombre de almacén de Recovery Services. Al ejecutar el comando, se elimina el almacén y todas las dependencias.

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>?api-version=2015-03-15
   ```
   El almacén debe estar vacío para poder eliminarlo. En caso contrario, se produce un error que indica que no se puede eliminar el almacén porque contiene recursos. El comando siguiente muestra cómo quitar un contenedor de un almacén:

   ```powershell
   ARMClient.exe delete /subscriptions/<subscriptionID>/resourceGroups/<resourcegroupname>/providers/Microsoft.RecoveryServices/vaults/<recovery services vault name>/registeredIdentities/<container name>?api-version=2016-06-01
   ```

1. Inicie sesión en su suscripción en Azure Portal y compruebe si se eliminó el almacén.


## <a name="remove-vault-dependencies-and-delete-vault"></a>Quitar las dependencias del almacén y eliminar el almacén

Para quitar manualmente las dependencias del almacén, elimine la configuración entre cada elemento o servidor, y el almacén de Recovery Services. A medida que avance por el siguiente procedimiento, use el menú **Elementos de copia de seguridad** (consulte la imagen) para:

* Copias de seguridad de Azure Storage (Azure Files)
* Copias de seguridad de SQL Server en VM de Azure
* Copias de seguridad de máquinas virtuales de Azure

Use el menú **Infraestructura de copia de seguridad** (consulte la imagen) para:

* Copias de seguridad de Azure Backup Server
* Copias de seguridad de System Center DPM

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/backup-items-backup-infrastructure.png)

1. En el menú del panel del almacén, desplácese hacia abajo hasta la sección Elementos protegidos y haga clic en **Elementos de copia de seguridad**. En este menú, puede detener y eliminar máquinas virtuales de Azure, servidores SQL Server de VM de Azure y servidores Azure Files. En este ejemplo, quitaremos los datos de copia de seguridad de un servidor de Azure Files.

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/selected-backup-items.png)

1. Seleccione un tipo de copia de seguridad para ver todos los elementos de ese tipo.

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

1. Para todos los elementos de la lista, haga clic con el botón derecho en el elemento y, en el menú contextual, seleccione **Detener copia de seguridad**.

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/stop-backup-item.png)

    Se abre el menú Detener copia de seguridad.

1. En el menú **Detener copia de seguridad**, en el menú **Elija una opción**, seleccione **Eliminar datos de copia de seguridad**, escriba el nombre del elemento y haga clic en **Detener copia de seguridad**.

    Escriba el nombre del elemento para verificar que desea eliminarlo. El botón **Detener copia de seguridad** se activa después de verificar el elemento. Si se conservan los datos, no podrá eliminar el almacén.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Si quiere, puede proporcionar el motivo por el que va a eliminar los datos y agregar comentarios. Para comprobar que el trabajo se ha completado, compruebe los mensajes de Azure ![delete backup data](./media/backup-azure-delete-vault/messages.png). <br/>
    Una vez finalizado el trabajo, el servicio envía un mensaje que indica que *el proceso de copia de seguridad se ha detenido y se han eliminado los datos de copia de seguridad*.

1. Después de eliminar un elemento de la lista, en el menú **Elementos de copia de seguridad**, haga clic en **Actualizar** para ver los elementos del almacén.

      ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/empty-items-list.png)

      Cuando no haya ningún elemento en la lista, desplácese al panel **Essentials** en el menú del almacén de Recovery Services. No debería haber **elementos de copia de seguridad**, **servidores de administración de copias de seguridad** ni **elementos replicados** en la lista. Si todavía aparecen elementos en el almacén, vuelva al paso 3 y elija otra lista de tipos de elemento.  

1. Cuando no hay ningún elemento más en la barra de herramientas de almacén, haga clic en **Eliminar**.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Para verificar que desea eliminar el almacén, haga clic en **Sí**.

    Se elimina el almacén y el portal vuelve al menú **Nuevo servicio** .

## <a name="removing-azure-backup-server-or-dpm"></a>Quitar Azure Backup Server o DPM

1. En el menú del panel del almacén, desplácese hacia abajo hasta la sección Administrar y haga clic en **Infraestructura de Backup**.

1. En el submenú, haga clic en **Servidores de administración de Backup** para ver los servidores de Azure Backup Server y System Center DPM. Puede detener y eliminar máquinas virtuales de Azure, servidores SQL Server de VM de Azure y servidores Azure Files.

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/delete-backup-management-servers.png)

1. Haga clic en el elemento que quiere eliminar y, en el submenú, seleccione **Eliminar**.

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/azure-storage-selected-list.png)

    Se abre el menú Detener copia de seguridad.

1. En el menú **Detener copia de seguridad**, en el menú **Elija una opción**, seleccione **Eliminar datos de copia de seguridad**, escriba el nombre del elemento y haga clic en **Detener copia de seguridad**.

    Escriba el nombre del elemento para verificar que quiere eliminarlo. El botón **Detener copia de seguridad** se activa después de verificar el elemento. Si se conservan los datos, no podrá eliminar el almacén.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/stop-backup-blade-delete-backup-data.png)

    Opcionalmente, puede proporcionar el motivo por el qué va a eliminar los datos y agregar comentarios. Para comprobar que el trabajo se ha completado, compruebe los mensajes de Azure ![delete backup data](./media/backup-azure-delete-vault/messages.png). <br/>
    Una vez finalizado el trabajo, el servicio envía un mensaje: el proceso de copia de seguridad se ha detenido y se han eliminado los datos de copia de seguridad.

1. Después de eliminar un elemento de la lista, en el menú **Elementos de copia de seguridad**, haga clic en **Actualizar** para ver los restantes elementos del almacén.

      ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/empty-items-list.png)

      Cuando no haya ningún elemento en la lista, desplácese al panel **Essentials** en el menú del almacén de Recovery Services. No debería haber **elementos de copia de seguridad**, **servidores de administración de copias de seguridad** ni **elementos replicados** en la lista. Si todavía aparecen elementos en el almacén, vuelva al paso 3 y elija otra lista de tipos de elemento.  
1. Cuando no haya ningún otro elemento en el almacén, haga clic en **Eliminar** en el panel del almacén.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Para verificar que desea eliminar el almacén, haga clic en **Sí**.

    Se elimina el almacén y el portal vuelve al menú **Nuevo servicio** .


## <a name="removing-azure-backup-agent-recovery-points"></a>Quitar puntos de recuperación del agente de Azure Backup

1. En el menú del panel del almacén, desplácese hacia abajo hasta la sección Administrar y haga clic en **Infraestructura de Backup**.

1. En el submenú, haga clic en **Servidores protegidos** ver la lista de tipos de servidores protegidos, incluido el agente de Azure Backup.

    ![Seleccionar el almacén para abrir su panel](./media/backup-azure-delete-vault/identify-protected-servers.png)

1. En la lista **Servidores protegidos**, haga clic en el agente de Azure Backup.

    ![Seleccionar el tipo de copia de seguridad](./media/backup-azure-delete-vault/list-of-protected-server-types.png)

    Se abre la lista de servidores protegidos con el agente de Azure Backup.

    ![Seleccionar el servidor protegido específico](./media/backup-azure-delete-vault/azure-backup-agent-protected-servers.png)

1. En la lista de servidores, haga clic en uno de ellos para abrir el menú.

    ![Ver el panel del servidor seleccionado](./media/backup-azure-delete-vault/selected-protected-server.png)

1. En el menú del panel del servidor seleccionado, haga clic en **Eliminar**.

    ![Eliminar el servidor seleccionado](./media/backup-azure-delete-vault/selected-protected-server-click-delete.png)

1. En el menú **Eliminar**, escriba el nombre del elemento y haga clic en **Eliminar**.

    Escriba el nombre del elemento para verificar que desea eliminarlo. El botón **Eliminar** se activa después de verificar el elemento.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/delete-protected-server-dialog.png)

    Opcionalmente, puede proporcionar el motivo por el qué va a eliminar los datos y agregar comentarios. Para comprobar que el trabajo se ha completado, compruebe los mensajes de Azure ![delete backup data](./media/backup-azure-delete-vault/messages.png). <br/>
    Una vez finalizado el trabajo, el servicio envía un mensaje: el proceso de copia de seguridad se ha detenido y se han eliminado los datos de copia de seguridad.

1. Después de eliminar un elemento de la lista, en el menú **Elementos de copia de seguridad**, haga clic en **Actualizar** para ver los restantes elementos del almacén.

      ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/empty-items-list.png)

      Cuando no haya ningún elemento en la lista, desplácese al panel **Essentials** en el menú del almacén de Recovery Services. No debería haber **elementos de copia de seguridad**, **servidores de administración de copias de seguridad** ni **elementos replicados** en la lista. Si todavía aparecen elementos en el almacén, vuelva al paso 3 y elija otra lista de tipos de elemento.  
1. Cuando no haya ningún otro elemento en el almacén, haga clic en **Eliminar** en el panel del almacén.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/vault-ready-to-delete.png)

1. Para verificar que desea eliminar el almacén, haga clic en **Sí**.

    Se elimina el almacén y el portal vuelve al menú **Nuevo servicio** .

## <a name="what-if-i-stop-the-backup-process-but-retain-the-data"></a>¿Qué ocurre si detengo el proceso de copia de seguridad pero retengo los datos?

Si detiene el proceso de copia de seguridad, pero accidentalmente *retiene* los datos, debe eliminar los datos de copia de seguridad para poder eliminar el almacén. Para eliminar los datos de copia de seguridad:

1. En el menú **Elementos de copia de seguridad**, haga clic con el botón derecho en el elemento y, en el menú contextual, haga clic en **Eliminar datos de copia de seguridad**.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/delete-backup-data-menu.png)

    Se abre el menú **Eliminar datos de copia de seguridad**.
1. En el menú **Eliminar datos de copia de seguridad**, escriba el nombre del elemento y haga clic en **Eliminar**.

    ![Eliminar datos de copia de seguridad](./media/backup-azure-delete-vault/delete-retained-vault.png)

    Una vez eliminados los datos, vaya al paso 4c y continúe con el proceso.
