---
title: Administración y supervisión de las copias de seguridad de máquina virtual de Azure con el servicio Azure Backup
description: Aprenda a administrar y supervisar la copia de seguridad de máquina virtual de Azure con el servicio Azure Backup.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 02/17/2019
ms.author: sogup
ms.openlocfilehash: f5c0373e2ef094a7fc5be64f4aeb8c0bb132e683
ms.sourcegitcommit: 9aa9552c4ae8635e97bdec78fccbb989b1587548
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/20/2019
ms.locfileid: "56430070"
---
# <a name="manage-azure-vm-backups"></a>Administración de copias de seguridad de máquinas virtuales de Azure

En este artículo se describe cómo administrar las máquinas virtuales de Azure copiadas con las copias de seguridad del [servicio Azure Backup](backup-overview.md) y se resume la información de alertas de copia de seguridad disponible en el panel del portal.


En Azure Portal, el panel Almacén de Recovery Services proporciona acceso a información sobre el almacén, como:

* La última copia de seguridad, que también es el punto de restauración más reciente
* La directiva de copia de seguridad
* El tamaño total de todas las instantáneas de copia de seguridad
* El número de máquinas virtuales habilitadas para la copia de seguridad

Puede administrar copias de seguridad mediante el panel y por medio de la exploración en profundidad hasta las máquinas virtuales individuales. La copia de seguridad de la máquina comienza con la apertura del almacén en el panel. 

![Vista completa con control deslizante](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-in-the-dashboard"></a>Visualización de máquinas virtuales en el panel

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. En el menú del centro, haga clic en **Examinar** y, en la lista de recursos, escriba **Recovery Services**. Cuando comience a escribir, la lista se filtrará en función de la entrada. Haga clic en **Almacén de Recovery Services**. 
    ![Creación del almacén de Recovery Services, paso 1](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Para facilitar su uso, haga clic con el botón derecho en el almacén en la lista de almacenes > **Anclar al panel**.
3. Abra el panel del almacén. 
    ![Abrir el panel del almacén y la hoja Configuración](./media/backup-azure-manage-vms/full-view-rs-vault.png)

4. En el icono **Elementos de copia de seguridad**, haga clic en **Azure Virtual Machines**.

    ![Abrir el icono Elementos de copia de seguridad](./media/backup-azure-manage-vms/contoso-vault-1606.png)

5. En la hoja **Elementos de copia de seguridad**, puede ver el último trabajo de copia de seguridad para cada elemento. En este ejemplo, hay una máquina virtual, demovm-markgal, protegida por este almacén.  

    ![Icono Elementos de copia de seguridad](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

 
6. Desde el panel de elementos del almacén, puede crear o modificar las directivas de copia de seguridad, ver los puntos de restauración, ejecutar una copia de seguridad a petición, detener y reanudar la protección de las máquinas virtuales, eliminar puntos de recuperación y ejecutar una restauración.

    ![Panel Elementos de copia de seguridad con hoja Configuración](./media/backup-azure-manage-vms/item-dashboard-settings.png)



## <a name="manage-backup-policies"></a>Administrar directivas de copia de seguridad
1. En el [panel de elementos del almacén](backup-azure-manage-vms.md#open-a-vault-item-dashboard), haga clic en **Toda la configuración**.

    ![Hoja Directiva de copia de seguridad](./media/backup-azure-manage-vms/all-settings-button.png)
2. En **Configuración**, haga clic en**Directiva de copia de seguridad**e.
3. En el menú **Elegir directiva de copia de seguridad** :

   * Para cambiar de directiva, seleccione una directiva diferente y haga clic en **Guardar**. La nueva directiva se aplica inmediatamente al almacén.
   * Para crear una directiva, seleccione **Crear nuevo**. [Más información](backup-azure-arm-vms-prepare.md#configure-a-backup-policy)

     ![Copia de seguridad de máquina virtual](./media/backup-azure-manage-vms/backup-policy-create-new.png)


## <a name="run-an-on-demand-backup"></a>Ejecución de una copia de seguridad a petición
Puede realizar una copia de seguridad a petición de una máquina virtual una vez que esté configurada para la protección.
- Si está pendiente la copia de seguridad inicial, la copia de seguridad a petición creará una copia completa de la máquina virtual en el almacén de Recovery Services.
- Si se ha completado la copia de seguridad inicial, una copia de seguridad a petición solo enviará los cambios respecto a la instantánea anterior al almacén de Recovery Services. Es decir, las copias de seguridad posteriores siempre son incrementales.
- La duración de retención para una copia de seguridad a petición es el valor de retención especificado para el punto de copia de seguridad diaria en la directiva. Si no se selecciona ningún punto de copia de seguridad diaria, se usa el semanal.


Para desencadenar una copia de seguridad a petición:

1. En el [panel del elemento del almacén](backup-azure-manage-vms.md#open-a-vault-item-dashboard), haga clic en **Realizar copia de seguridad ahora**.

    ![Botón Realizar copia de seguridad ahora](./media/backup-azure-manage-vms/backup-now-button.png)

 2. Haga clic en **Sí** para iniciar el trabajo.

    ![Botón Realizar copia de seguridad ahora](./media/backup-azure-manage-vms/backup-now-check.png)

 
 El trabajo de copia de seguridad crea un punto de recuperación. La duración de retención del punto de recuperación será la misma que la especificada en la directiva asociada a la máquina virtual. Para realizar un seguimiento del progreso del trabajo, en el panel del almacén, haga clic en el icono **Trabajos de copia de seguridad** .  

## <a name="stop-protecting-a-vm"></a>Eliminar la protección de una máquina virtual

Hay dos formas de eliminar la protección de máquinas virtuales:

- Detener todos los trabajos futuros de copia de seguridad y eliminar todos los puntos de recuperación. En este caso, no podrá restaurar la máquina virtual.
- Detener todos los trabajos futuros de copia de seguridad pero dejar los puntos de recuperación. Dejar los puntos de recuperación almacenados conlleva un costo. Sin embargo, la ventaja de dejarlos es que puede restaurar la máquina virtual cuando sea necesario. [Más información](https://azure.microsoft.com/pricing/details/backup/) sobre los precios.

Para detener la protección de una máquina virtual:

1. En el [panel del elemento del almacén](backup-azure-manage-vms.md#open-a-vault-item-dashboard), haga clic en **Detener copia de seguridad**.

    ![Stop backup button](./media/backup-azure-manage-vms/stop-backup-button.png
2. Elija si quiere conservar o eliminar los datos de copia de seguridad y confirme cuando sea necesario. Confirme cuando sea necesario y, opcionalmente, proporcione un comentario. Si no está seguro del nombre, mantenga el mouse sobre el signo de exclamación para verlo.

    ![Detener protección](./media/backup-azure-manage-vms/retain-or-delete-option.png)

 Un mensaje de notificación le confirma que se han detenido los trabajos de copia de seguridad.


## <a name="resume-protection-of-a-vm"></a>Reanudación de la protección de una máquina virtual

Si conservó datos de copia de seguridad cuando se detuvo la máquina virtual, puede reanudar la protección. Si los datos de copia de seguridad se eliminaron, no puede reanudarla.

Te

1. En el [panel del elemento del almacén](backup-azure-manage-vms.md#open-a-vault-item-dashboard), haga clic en **Reanudar copia de seguridad**.

2. Siga los pasos de [Administrar directivas de copia de seguridad](backup-azure-manage-vms.md#manage-backup-policies) para asignar la directiva para la máquina virtual. Puede elegir una directiva diferente a la directiva con la que estaba protegida inicialmente la máquina virtual.
3. Después de aplicar la directiva de copia de seguridad a la máquina virtual, verá el mensaje siguiente.

    ![Máquina virtual protegida correctamente](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Eliminar datos de copia de seguridad

Puede eliminar los datos de copia de seguridad asociados a una máquina virtual durante el trabajo **Detener copia de seguridad** o en cualquier momento una vez finalizado el trabajo de copia de seguridad.

- Incluso puede resultar útil esperar días o semanas antes de eliminar los puntos de recuperación.
- A diferencia de la restauración de puntos de recuperación, cuando se eliminan datos de copia de seguridad, no se pueden elegir los puntos de recuperación específicos que se van a eliminar. Si elige eliminar los datos de copia de seguridad, se eliminarán todos los puntos de recuperación asociados con el elemento.

Este procedimiento da por supuesto que el trabajo de copia de seguridad de la máquina virtual se ha detenido o deshabilitado.


1. En el [panel del elemento del almacén](backup-azure-manage-vms.md#open-a-vault-item-dashboard), haga clic en **Delete backup**(Eliminar copia de seguridad).

    ![Tipo de máquina virtual](./media/backup-azure-manage-vms/delete-backup-buttom.png)

2. Escriba el nombre del elemento para confirmar que desea eliminar los puntos de recuperación.

    ![Comprobación de la detención](./media/backup-azure-manage-vms/item-verification-box.png)

4. Para eliminar los datos de copia de seguridad del elemento actual, haga clic en el botón ![Detener copia de seguridad](./media/backup-azure-manage-vms/delete-button.png).

    Un mensaje de notificación le confirma que se han eliminado los datos de copia de seguridad.

## <a name="next-steps"></a>Pasos siguientes
- [Más información sobre](backup-azure-vms-first-look-arm.md) la copia de seguridad de máquinas virtuales de Azure desde la configuración de máquina virtual.
- [Más información sobre](backup-azure-arm-restore-vms.md) la restauración de máquinas virtuales. 
- [Más información sobre](backup-azure-monitor-vms.md) la supervisión de copias de seguridad de máquina virtual de Azure.
 
