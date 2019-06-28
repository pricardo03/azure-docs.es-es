---
title: Administración y supervisión de las copias de seguridad de Azure VM con el servicio Azure Backup
description: Aprenda a administrar y supervisar las copias de seguridad de Azure VM con el servicio Azure Backup.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: sogup
ms.openlocfilehash: aa953440f03137f3359276bc9e06cb0c73f0ab4a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "61219290"
---
# <a name="manage-azure-vm-backups"></a>Administración de copias de seguridad de máquinas virtuales de Azure

En este artículo se describe cómo administrar máquinas virtuales (VM) de Azure cuya copia de seguridad se realiza con el [servicio Azure Backup](backup-overview.md). El artículo también resume la información de copia de seguridad que puede encontrar en el panel del almacén.


En Azure Portal, el panel Almacén de Recovery Services proporciona acceso a información sobre el almacén, como:

* La última copia de seguridad, que también es el punto de restauración más reciente
* La directiva de copia de seguridad.
* El tamaño total de todas las instantáneas de copia de seguridad.
* El número de máquinas virtuales habilitadas para la copia de seguridad.

Puede administrar copias de seguridad mediante el panel y por medio de la exploración en profundidad hasta las máquinas virtuales individuales. Para empezar las copias de seguridad de la máquina, abra el almacén en el panel.

![Vista de panel completo con el control deslizante](./media/backup-azure-manage-vms/bottom-slider.png)

## <a name="view-vms-on-the-dashboard"></a>Visualización de máquinas virtuales en el panel

Para ver las máquinas virtuales en el panel:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
2. En el menú Hub, seleccione **Examinar**. En la lista de recursos, escriba **Recovery Services**. A media que escribe, la lista se filtra en función de lo que escriba. Seleccione **Almacenes de Recovery Services**.

    ![Creación de un almacén de Recovery Services](./media/backup-azure-manage-vms/browse-to-rs-vaults.png)

3. Para facilitar su uso, haga clic con el botón derecho en el almacén y seleccione **Anclar al panel**.
4. Abra el panel del almacén.

    ![Apertura del panel del almacén y la hoja Configuración](./media/backup-azure-manage-vms/full-view-rs-vault.png)

5. En el icono **Elementos de copia de seguridad**, seleccione **Azure Virtual Machines**.

    ![Apertura del menú Elementos de copia de seguridad](./media/backup-azure-manage-vms/contoso-vault-1606.png)

6. En la hoja  **Elementos de copia de seguridad** , puede ver la lista de máquinas virtuales protegidas. En este ejemplo, el almacén protege una máquina virtual: demobackup.  

    ![Visualización de la hoja Elementos de copia de seguridad](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

7. Desde el panel de elementos del almacén, puede modificar las directivas de copia de seguridad, ejecutar una copia de seguridad a petición, detener o reanudar la protección de las máquinas virtuales, eliminar los puntos de recuperación y ejecutar una restauración.

    ![Panel Elementos de copia de seguridad en la hoja Configuración](./media/backup-azure-manage-vms/item-dashboard-settings.png)

## <a name="manage-backup-policy-for-a-vm"></a>Administración de la directiva de copia de seguridad de una máquina virtual

Para administrar una directiva de copia de seguridad:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/). Abra el panel del almacén.
2. En el icono **Elementos de copia de seguridad**, seleccione **Azure Virtual Machines**.

    ![Apertura del menú Elementos de copia de seguridad](./media/backup-azure-manage-vms/contoso-vault-1606.png)

3. En la hoja  **Elementos de copia de seguridad** , puede ver la lista de máquinas virtuales protegidas y el estado de la última copia de seguridad con la hora de los puntos de restauración más reciente.

    ![Visualización de la hoja Elementos de copia de seguridad](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

4. Desde el panel del elemento del almacén, puede seleccionar una directiva de copia de seguridad.

   * Para cambiar las directivas, seleccione una diferente y, después, seleccione **Guardar**. La nueva directiva se aplica inmediatamente en el almacén.

     ![Elección de una directiva de copia de seguridad](./media/backup-azure-manage-vms/backup-policy-create-new.png)

## <a name="run-an-on-demand-backup"></a>Ejecución de una copia de seguridad a petición
Puede ejecutar una copia de seguridad a petición de una máquina virtual después de configurar su protección. Tenga en cuenta estos detalles:

- Si está pendiente la copia de seguridad inicial, la copia de seguridad a petición creará una copia completa de la máquina virtual en el almacén de Recovery Services.
- Si se ha completado la copia de seguridad inicial, una copia de seguridad a petición solo enviará los cambios respecto a la instantánea anterior al almacén de Recovery Services. Es decir, las copias de seguridad posteriores siempre son incrementales.
- La duración de retención para una copia de seguridad a petición es el valor de retención que especificó al desencadenar la copia de seguridad diaria.

Para desencadenar una copia de seguridad a petición:

1. En el [panel del elemento del almacén](#view-vms-on-the-dashboard), en **Elemento protegido**, seleccione **Elemento de copia de seguridad**.

    ![La opción Crear copia de seguridad ahora](./media/backup-azure-manage-vms/backup-now-button.png)

2. En **Tipo de administración de copias de seguridad**, seleccione **Azure Virtual Machine**. Aparece la hoja **Elemento de copia de seguridad (Azure Virtual Machine)** .
3. Seleccione una máquina virtual y, después, **Crear copia de seguridad ahora** para crear una copia de seguridad a petición. Aparece la hoja **Crear copia de seguridad ahora**.
4. En el campo **Conservar copia de seguridad hasta**, especifique una fecha para la copia de seguridad que se conservará.

    ![Calendario de Crear copia de seguridad ahora](./media/backup-azure-manage-vms/backup-now-check.png)

5. Seleccione **Aceptar** para ejecutar el trabajo de copia de seguridad.

Para realizar un seguimiento del progreso del trabajo, en el panel del almacén, seleccione el icono **Trabajos de copia de seguridad**.

## <a name="stop-protecting-a-vm"></a>Eliminar la protección de una máquina virtual

Hay dos formas de detener la protección de una máquina virtual:

- Detener todos los trabajos futuros de copia de seguridad y eliminar todos los puntos de recuperación. En este caso, no podrá restaurar la máquina virtual.
- Detenga todos los trabajos futuros de copia de seguridad y mantenga los puntos de recuperación. Aunque deberá pagar para mantener los puntos de recuperación en el almacén, podrá restaurar la máquina virtual si es necesario. Para más información, consulte [Precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).

>[!NOTE]
>Si elimina un origen de datos sin detener las copias de seguridad, las nuevas copias de seguridad producirán errores. Los puntos de recuperación anteriores expirarán según la directiva, pero siempre se mantendrá el último punto de recuperación hasta que detenga la copia de seguridad y elimine los datos.
>

Para detener la protección de una máquina virtual:

1. En el [panel del elemento del almacén](#view-vms-on-the-dashboard), seleccione **Detener copia de seguridad**.
2. Elija si quiere conservar o eliminar los datos de copia de seguridad y confirme la selección cuando sea necesario. Agregue un comentario si lo desea. Si no está seguro del nombre del elemento, mantenga el mouse sobre el signo de exclamación para verlo.

    ![Detener protección](./media/backup-azure-manage-vms/retain-or-delete-option.png)

     Una notificación le confirma que se han detenido los trabajos de copia de seguridad.

## <a name="resume-protection-of-a-vm"></a>Reanudación de la protección de una máquina virtual

Si mantiene los datos de copia de seguridad cuando se detiene la máquina virtual, puede reanudar la protección más tarde. Si elimina los datos de copia de seguridad, no se puede reanudar la protección.

Para reanudar la protección para una máquina virtual:

1. En el [panel del elemento del almacén](#view-vms-on-the-dashboard), seleccione **Reanudar copia de seguridad**.

2. Siga los pasos de [Administrar directivas de copia de seguridad](#manage-backup-policy-for-a-vm) para asignar la directiva para la máquina virtual. No es necesario elegir la directiva de protección inicial de la máquina virtual.
3. Después de aplicar la directiva de copia de seguridad a la máquina virtual, consulte el siguiente mensaje:

    ![Mensaje que indica que una máquina virtual está protegida correctamente](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a>Eliminación de datos de copia de seguridad

Puede eliminar los datos de copia de seguridad de la máquina virtual durante el trabajo **Detener copia de seguridad** o una vez finalizado el trabajo de copia de seguridad. Antes de eliminar los datos de copia de seguridad, tenga en cuenta estos detalles:

- Podría ser conveniente esperar días o semanas antes de eliminar los puntos de recuperación.
- A diferencia del proceso de restauración de puntos de recuperación, cuando se eliminan datos de copia de seguridad, no se pueden elegir los puntos de recuperación específicos que se van a eliminar. Si elimina los datos de copia de seguridad, se eliminarán todos los puntos de recuperación asociados.

Después de detener o deshabilitar el trabajo de copia de seguridad de la máquina virtual, puede eliminar los datos de copia de seguridad:


1. En el [panel del elemento del almacén](#view-vms-on-the-dashboard), seleccione **Eliminar copia de seguridad**.

    ![Selección de Eliminar copia de seguridad](./media/backup-azure-manage-vms/delete-backup-buttom.png)

1. Escriba el nombre del elemento de copia de seguridad para confirmar que desea eliminar los puntos de recuperación.

    ![Confirme que desea eliminar los puntos de recuperación.](./media/backup-azure-manage-vms/item-verification-box.png)

1. Para eliminar los datos de copia de seguridad para el elemento, seleccione **Eliminar**. Un mensaje de notificación le confirma que se han eliminado los datos de copia de seguridad.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga información sobre cómo [crear la copia de seguridad de máquinas virtuales de Azure desde la configuración de la máquina virtual](backup-azure-vms-first-look-arm.md).
- Aprenda a [restaurar máquinas virtuales](backup-azure-arm-restore-vms.md).
- Aprenda a [supervisar las copias de seguridad de las máquinas virtuales de Azure](backup-azure-monitor-vms.md).
