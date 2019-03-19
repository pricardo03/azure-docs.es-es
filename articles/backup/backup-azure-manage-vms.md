---
title: Administrar y supervisar las copias de seguridad de máquina virtual de Azure mediante el servicio Azure Backup
description: Aprenda a administrar y supervisar las copias de seguridad de máquina virtual de Azure mediante el servicio de copia de seguridad de Azure.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: sogup
ms.openlocfilehash: 01c3e8072db81620764ccdd3ea99258de4649807
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57858947"
---
# <a name="manage-azure-vm-backups"></a>Administración de copias de seguridad de máquinas virtuales de Azure

En este artículo se describe cómo administrar máquinas virtuales (VM) que se copia de seguridad mediante el uso de la [servicio Azure Backup](backup-overview.md). El artículo también resume la información de copia de seguridad que puede encontrar en el panel del almacén.


En el portal de Azure, el panel del almacén de Recovery Services proporciona acceso al almacén de información, incluidos:

* La última copia de seguridad, que también es el punto de restauración más reciente
* La directiva de copia de seguridad.
* El tamaño total de todas las instantáneas de copia de seguridad.
* El número de máquinas virtuales que están habilitadas para las copias de seguridad.

Puede administrar las copias de seguridad mediante el panel y cómo llegar a las máquinas virtuales individuales. Para empezar a las copias de seguridad de la máquina, abra el almacén en el panel.

![Vista de panel completo con el control deslizante](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-on-the-dashboard"></a>Visualización de máquinas virtuales en el panel

Para ver las máquinas virtuales en el panel del almacén:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. En el menú del concentrador, seleccione **examinar**. En la lista de recursos, escriba **Recovery Services**. A medida que escribe, la lista se filtra en función de la entrada. Seleccione **Almacenes de Recovery Services**.

    ![Creación de un almacén de Recovery Services](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Para facilitar su uso, haga clic en el almacén y seleccione **Anclar al panel**.
4. Abra el panel del almacén.

    ![Abra el panel del almacén y la hoja de configuración](./media/backup-azure-manage-vms/full-view-rs-vault.png)

5. En el **elementos de copia de seguridad** icono, seleccione **Azure Virtual Machines**.

    ![Abra el icono de elementos de copia de seguridad](./media/backup-azure-manage-vms/contoso-vault-1606.png)

6. En el **elementos de copia de seguridad** hoja, puede ver la lista de máquinas virtuales protegidas. En este ejemplo, el almacén protege una máquina virtual: demobackup.  

    ![Ver la hoja elementos de copia de seguridad](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

7. Desde el panel del elemento del almacén, modificar las directivas de copia de seguridad, ejecute una detención de copia de seguridad, y a petición o reanudar la protección de máquinas virtuales, eliminar datos de copia de seguridad, ver los puntos de restauración y ejecutar una restauración.

    ![El panel de elementos de copia de seguridad y la hoja de configuración](./media/backup-azure-manage-vms/item-dashboard-settings.png)

## <a name="manage-backup-policy-for-a-vm"></a>Administrar la directiva de copia de seguridad para una máquina virtual

Para administrar una directiva de copia de seguridad:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/). Abra el panel del almacén.
2. En el **elementos de copia de seguridad** icono, seleccione **Azure Virtual Machines**.

    ![Abra el icono de elementos de copia de seguridad](./media/backup-azure-manage-vms/contoso-vault-1606.png)

3. En el **elementos de copia de seguridad** hoja, puede ver la lista de máquinas virtuales protegidas y el estado de la última copia de seguridad con la hora de puntos de restauración más reciente.

    ![Ver la hoja elementos de copia de seguridad](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

4. Desde el panel del elemento del almacén, puede seleccionar una directiva de copia de seguridad.

   * Para cambiar las directivas, seleccione una directiva diferente y, a continuación, seleccione **guardar**. La nueva directiva se aplica inmediatamente en el almacén.

     ![Elegir una directiva de copia de seguridad](./media/backup-azure-manage-vms/backup-policy-create-new.png)

## <a name="run-an-on-demand-backup"></a>Ejecución de una copia de seguridad a petición
Puede ejecutar una copia de seguridad y a petición de una máquina virtual después de configurar su protección. Estos detalles se tenga en cuenta:

- Si la copia de seguridad inicial está pendiente, copia de seguridad y a petición crea una copia completa de la máquina virtual en el almacén de Recovery Services.
- Si se ha completado la copia de seguridad inicial, una copia de seguridad y a petición solo enviará los cambios desde la instantánea anterior en el almacén de Recovery Services. Es decir, las copias de seguridad posteriores siempre son incrementales.
- La duración de retención para una copia de seguridad y a petición es el valor de retención que especifique cuando se desencadena la copia de seguridad.

Para desencadenar una copia de seguridad a petición:

1. En el [panel del elemento del almacén](#view-vms-in-the-dashboard), en **elemento protegido**, seleccione **elemento de copia de seguridad**.

    ![La opción de copia de seguridad ahora](./media/backup-azure-manage-vms/backup-now-button.png)

2. Desde **tipo de administración de copia de seguridad**, seleccione **Máquina Virtual de Azure**. El **elemento de copia de seguridad (Máquina Virtual de Azure)** aparece la hoja.
3. Seleccione una máquina virtual y seleccione **copia de seguridad ahora** para crear una copia de seguridad y a petición. El **copia de seguridad ahora** aparece la hoja.
4. En el **conservar copia de seguridad hasta** , especifique una fecha para la copia de seguridad que se conservarán.

    ![El calendario de copia de seguridad ahora](./media/backup-azure-manage-vms/backup-now-check.png)

5. Seleccione **Aceptar** para ejecutar el trabajo de copia de seguridad.

Para seguir el progreso del trabajo, en el panel del almacén, seleccione el **trabajos de copia de seguridad** icono.

## <a name="stop-protecting-a-vm"></a>Eliminar la protección de una máquina virtual

Hay dos formas de detener la protección de una máquina virtual:

- Detener todos los trabajos futuros de copia de seguridad y eliminar todos los puntos de recuperación. En este caso, no podrá restaurar la máquina virtual.
- Detener todos los trabajos futuros de copia de seguridad y mantenga los puntos de recuperación. Aunque deberá pagar para mantener los puntos de recuperación en el almacén, podrá restaurar la máquina virtual si es necesario. Para obtener más información, consulte [precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

>[!NOTE]
>Si elimina un origen de datos sin detener las copias de seguridad, se producirá un error en nuevas copias de seguridad. Puntos de recuperación antiguos expirará según la directiva, pero un último punto de recuperación siempre se mantienen hasta que detenga las copias de seguridad y elimina los datos.
>

Para detener la protección de una máquina virtual:

1. En el [panel del elemento del almacén de](#view-vms-in-the-dashboard), seleccione **Detener copia de seguridad**.
2. Elija si desea conservar o eliminar los datos de copia de seguridad y confirme la selección según sea necesario. Si quiere agregar un comentario. Si no está seguro de que el nombre del elemento, mantenga el mouse sobre el signo de exclamación para ver el nombre.

    ![Detener protección](./media/backup-azure-manage-vms/retain-or-delete-option.png)

     Una notificación le confirma que se han detenido los trabajos de copia de seguridad.

## <a name="resume-protection-of-a-vm"></a>Reanudación de la protección de una máquina virtual

Si mantiene los datos de copia de seguridad cuando se detiene la máquina virtual, más tarde puede reanudar la protección. Si elimina los datos de copia de seguridad, no se puede reanudar la protección.

Para reanudar la protección de una máquina virtual:

1. En el [panel del elemento del almacén de](#view-vms-in-the-dashboard), seleccione **Reanudar copia de seguridad**.

2. Siga los pasos de [administrar directivas de copia de seguridad](#manage-backup-policies) para asignar la directiva para la máquina virtual. No es necesario elegir directiva de protección inicial de la máquina virtual.
3. Después de aplicar la directiva de copia de seguridad a la máquina virtual, consulte el siguiente mensaje:

    ![Mensaje que indica que una máquina virtual protegida correctamente](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Eliminación de datos de copia de seguridad

Puede eliminar datos de copia de seguridad de la máquina virtual durante la **Detener copia de seguridad** trabajo o una vez finalizado el trabajo de copia de seguridad. Antes de eliminar datos de copia de seguridad, tenga estos detalles en mente:

- Podría ser una buena idea esperar días o semanas antes de eliminar los puntos de recuperación.
- A diferencia del proceso de restauración de puntos de recuperación, cuando se eliminan datos de copia de seguridad, no puede elegir puntos de recuperación específicos para eliminar. Si elimina los datos de copia de seguridad, elimine todos los puntos de recuperación asociados.

Después de detener o deshabilitar el trabajo de copia de seguridad de la máquina virtual, puede eliminar los datos de copia de seguridad:


1. En el [panel del elemento del almacén](#view-vms-in-the-dashboard), seleccione **eliminar datos de copia de seguridad**.

    ![Seleccione Eliminar copia de seguridad](./media/backup-azure-manage-vms/delete-backup-buttom.png)

1. Escriba el nombre del elemento de copia de seguridad para confirmar que desea eliminar los puntos de recuperación.

    ![Confirmar que desea eliminar los puntos de recuperación](./media/backup-azure-manage-vms/item-verification-box.png)

1. Para eliminar los datos de copia de seguridad para el elemento, seleccione **eliminar**. Un mensaje de notificación permite saber que se ha eliminado los datos de copia de seguridad.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre cómo [copia de seguridad de máquinas virtuales de Azure desde la configuración de la máquina virtual](backup-azure-vms-first-look-arm.md).
- Obtenga información sobre cómo [restauración de máquinas virtuales](backup-azure-arm-restore-vms.md).
- Obtenga información sobre cómo [supervisar las copias de seguridad de máquina virtual de Azure](backup-azure-monitor-vms.md).
