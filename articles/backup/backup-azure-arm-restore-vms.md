---
title: 'Azure Backup: restauración de máquinas virtuales mediante Azure Portal'
description: Restauración de una máquina virtual de Azure desde un punto de recuperación con Azure Portal
services: backup
author: geethalakshmig
manager: vijayts
keywords: restaurar copias de seguridad; cómo restaurar; punto de recuperación;
ms.service: backup
ms.topic: conceptual
ms.date: 09/04/2017
ms.author: geg
ms.openlocfilehash: 7de9d1674860a6369c1dc09462a06def672fbdf2
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50420530"
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>Uso de Azure Portal para restaurar máquinas virtuales
Proteja sus datos tomando instantáneas de sus datos a intervalos definidos. Estas instantáneas se denominan puntos de recuperación y se almacenan en almacenes de Recovery Services. Si es necesario reparar o recompilar una máquina virtual, puede restaurarla desde cualquiera de los puntos de recuperación guardados. Cuando restaura un punto de recuperación, puede:

* Crear una nueva máquina virtual que sea una representación de un momento dado de la máquina virtual de copia de seguridad.
* Restaurar discos y usar la plantilla que se incluye con el proceso para personalizar la máquina virtual restaurada o realizar una recuperación de archivos individuales.

En este artículo se explica cómo restaurar una máquina virtual en una nueva máquina virtual o restaurar todos los discos de copia de seguridad. Para la recuperación de archivos individuales, consulte el artículo sobre cómo [recuperar archivos de copias de seguridad de máquinas virtuales de Azure](backup-azure-restore-files-from-vm.md).

![Tres formas de restaurar copias de seguridad de máquinas virtuales](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)

> [!NOTE]
> Azure cuenta con dos modelos de implementación para crear recursos y trabajar con ellos: [Azure Resource Manager y el modelo clásico](../azure-resource-manager/resource-manager-deployment-model.md). En este artículo se proporcionan la información y los procedimientos usados para restaurar las máquinas virtuales implementadas mediante el modelo de Resource Manager.
>
>

Para restaurar una máquina virtual o todos los discos a partir de copias de seguridad de máquinas virtuales, hay que realizar dos pasos:

* Seleccione un punto de restauración.
* Seleccionar el tipo de restauración, cree una nueva máquina virtual o restaure los discos y especifique los parámetros necesarios.

## <a name="select-a-restore-point-for-restore"></a>Seleccionar un punto de restauración
1. Inicie sesión en el [Azure Portal](http://portal.azure.com/).

2. En el menú de Azure, seleccione **Todos los servicios**. En la lista de servicios, escriba **Recovery Services** o vaya a **STORAGE**, donde se enumeran los **almacenes de Recovery Services**, y selecciónelo.

    ![Almacén de Recovery Services](./media/backup-azure-arm-restore-vms/open-recovery-services-vault1.png)

3. Se muestra la lista de almacenes de la suscripción.

    ![Lista de almacenes de Recovery Services](./media/backup-azure-arm-restore-vms/list-of-rs-vaults1.png)

4. En la lista de almacenes de Recovery Services, seleccione el almacén asociado a la máquina virtual que quiere restaurar. Al hacer clic en el almacén, se abre su panel.

    ![Almacén de Recovery Services seleccionado](./media/backup-azure-arm-restore-vms/select-vault-open-vault-blade1.png)

5. En el panel del almacén, en el icono **Elementos de copia de seguridad**, seleccione **Máquina virtual de Azure**.

    ![Panel del almacén](./media/backup-azure-arm-restore-vms/vault-dashboard1.png)

6. Se abre la hoja **Elementos de copia de seguridad** con la lista de máquinas virtuales de Azure.

    ![Lista de máquinas virtuales en el almacén](./media/backup-azure-arm-restore-vms/list-of-vms-in-vault1.png)

7. En la lista, seleccione una máquina virtual para abrir el panel. Se abre el panel de máquinas virtuales en el área Supervisión que contiene el icono **Puntos de recuperación**. Todas las operaciones de nivel de máquina virtual, como **Realizar copia de seguridad ahora**, **Recuperación de archivos** o **Detener copia de seguridad**, se pueden realizar desde esta hoja.
La restauración puede realizarse de muchas maneras desde esta hoja. Esta hoja muestra solo los puntos de recuperación de los últimos 30 días.

    a) Haga clic con el botón derecho en el punto de recuperación de esta hoja (menos de 30 días) e inicie **Restaurar VM**.

    b) Para restaurar puntos de recuperación de más de 30 días, puede usar la opción Haga clic aquí de la hoja.

    c) **Restaurar VM** en el encabezado del menú proporciona una opción para enumerar y filtrar las máquinas virtuales de fechas personalizadas como preferidas.

    Utilice la opción de filtro para modificar el intervalo de tiempo de los puntos de restauración mostrados. De forma predeterminada, se muestran todos los puntos de restauración de todas las coherencias. Modifique el filtro Todos los puntos de restauración para seleccionar una coherencia específica de puntos de restauración. Para más información acerca de cada tipo de punto de restauración, consulte [Coherencia de datos](backup-azure-vms-introduction.md#data-consistency).

    Opciones de coherencia de puntos de restauración:
    - Puntos de restauración coherentes frente a bloqueos
    - Puntos de restauración coherentes con la aplicación
    - Puntos de restauración coherentes con el sistema de archivos
    - Todos los puntos de restauración

  ![Puntos de restauración](./media/backup-azure-arm-restore-vms/vm-blade1.png)

    >  [!NOTE]
    > Tipo de recuperación representa si se encuentra en la cuenta de almacenamiento del cliente, en el almacén o en ambos. Obtenga más información sobre el [punto de recuperación instantánea](https://azure.microsoft.com/blog/large-disk-support/).

8. En la hoja **Restaurar**, seleccione **Punto de restauración**.

    ![Seleccionar punto de restauración](./media/backup-azure-arm-restore-vms/select-recovery-point1.png)

    La hoja **Restaurar** muestra que el punto de restauración está establecido al hacer clic en **Aceptar**.
9. Si aún no está allí, vaya a la hoja **Restaurar**. Asegúrese de que se selecciona un [punto de restauración](#select-a-restore-point-for-restore) y seleccione **Restaurar configuración**. Se abre la hoja **Restaurar configuración**.

## <a name="choose-a-vm-restore-configuration"></a>Elección de la configuración de restauración de una máquina virtual
Después de seleccionar el punto de restauración, elija la configuración de restauración de una máquina virtual. Para configurar la máquina virtual restaurada, puede usar Azure Portal o PowerShell.

1. Si aún no está allí, vaya a la hoja **Restaurar**. Asegúrese de que se selecciona un [punto de restauración](#select-a-restore-point-for-restore) y seleccione **Restaurar configuración**. Se abre la hoja **Restaurar configuración**.
2. Actualmente, esta hoja tiene dos opciones: **Crear nuevo**, que es la opción predeterminada, y **Reemplazar el existente**, que es la restauración en contexto para reemplazar los discos y conservar solo las configuraciones y extensiones existentes.

> [!NOTE]
> Estamos trabajando en el reemplazo de toda la máquina virtual por los discos, la configuración de red, y las configuraciones y extensiones en los próximos meses.
>
>

 En la opción **Crear nuevo**, que restaura los datos a la nueva máquina virtual o a discos nuevos, tiene dos opciones:

 ![Asistente para configuración de restauración](./media/backup-azure-arm-restore-vms/restore-configuration-create-new.png)

 - **Crear la máquina virtual**
 - **Restaura los discos**

![Asistente para configuración de restauración](./media/backup-azure-arm-restore-vms/restore-configuration-create-new1.png)

El portal proporciona una opción **Creación rápida** para la máquina virtual restaurada. Para personalizar la configuración de la máquina virtual o los nombres de los recursos creados como parte de la creación de una nueva opción de máquina virtual, use PowerShell o el portal para restaurar los discos de copia de seguridad. Use comandos de PowerShell para asociarlos a su elección de configuración de máquina virtual. También puede usar la plantilla que se incluye con los discos restaurados para personalizar la máquina virtual restaurada. Para más información sobre cómo restaurar una máquina virtual que tiene varias NIC o que se encuentra bajo un equilibrador de carga, consulte [Restore a VM with special network configurations](#restore-vms-with-special-network-configurations) (Restauración de una máquina virtual con configuraciones de red especiales). Si su máquina virtual Windows usa [licencia de concentrador](../virtual-machines/windows/hybrid-use-benefit-licensing.md), restaure los discos y use PowerShell o una plantilla como se ha especificado en este artículo para crear la máquina virtual. Asegúrese de que especifica el **tipo de licencia** como "Windows_Server" al crear la máquina virtual para aprovechar las ventajas de HUB en la máquina virtual restaurada. Tenga en cuenta que esto puede hacerse más adelante después de la creación de la máquina virtual.

## <a name="create-a-new-vm-from-a-restore-point"></a>Creación de una nueva máquina virtual desde el punto de restauración
1. En la hoja **Restaurar configuración**, mencionada en la sección anterior, escriba o seleccione valores en cada uno de los siguientes campos:

    a. **Tipo de restauración**. Cree una máquina virtual.

    b. **Nombre de la máquina virtual**. Escriba un nombre de VM que no exista en la suscripción.

    c. **Grupo de recursos**. Use un grupo de recursos existente o cree uno. Si va a restaurar una máquina virtual clásica, use este campo para especificar el nombre de un nuevo servicio en la nube. Si va a crear un nuevo grupo de recursos o servicio en la nube, el nombre debe ser globalmente único. Por lo general, el nombre del servicio en la nube está asociado a una dirección URL pública; por ejemplo, [cloudservice].cloudapp.net. Si intenta usar un nombre para el servicio en la nube o el grupo de recursos de nube que ya se ha utilizado, Azure asigna al servicio en la nube o grupo de recursos el mismo nombre que la máquina virtual. Azure muestra servicios en la nube o grupos de recursos y máquinas virtuales no asociados a ningún grupo de afinidad. Para más información, consulte [cómo migrar grupos de afinidad a una red virtual regional](../virtual-network/virtual-networks-migrate-to-regional-vnet.md).

    d. **Red virtual**. Seleccione la red virtual al crear la VM. El campo proporciona todas las redes virtuales asociadas a la suscripción. El grupo de recursos de la máquina virtual se muestra entre paréntesis.

    e. **Subred**. Si la red virtual tiene subredes, la primera subred se selecciona de forma predeterminada. Si hay subredes adicionales, seleccione la que desee.

    f. **Ubicación de almacenamiento** Las cuentas de almacenamiento son la ubicación de ensayo. Este menú muestra la lista de cuentas de almacenamiento de la misma ubicación que el almacén de Recovery Services. No se admiten cuentas de almacenamiento con redundancia de zona. Si no hay ninguna cuenta de almacenamiento con la misma ubicación que el almacén de Recovery Services, debe crear una antes de iniciar la operación de restauración. El tipo de replicación de la cuenta de almacenamiento se muestra entre paréntesis.

    ![Asistente para configuración de restauración](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

    > [!NOTE]
    > * Una red virtual es opcional para una VM clásica y obligatoria para la VM implementada mediante Resource Manager.

    > * El tipo de almacenamiento proporcionado en la cuenta de almacenamiento (Premium o Estándar) en la ubicación de ensayo decide el tipo de almacenamiento de los discos de restauración. En este momento no se admite el modo mixto de los discos durante la restauración.
    >
    >

2. En la hoja **Restore configuration** (Configuración de restauración), haga clic en **Aceptar** para finalizar la configuración de la restauración. En la hoja **Restaurar**, seleccione **Restaurar** para desencadenar la operación de restauración.

## <a name="restore-backed-up-disks"></a>Restauración de discos de copia de seguridad
El valor de tipo de restauración **Restaurar disco** en la hoja **Restaurar configuración** permite crear una VM con configuraciones personalizadas. Al restaurar discos, la cuenta de Storage que se seleccionará debe estar en la misma ubicación que el almacén de Recovery Services. Es obligatorio crear una cuenta de almacenamiento si no hay ninguna con la misma ubicación que el almacén de Recovery Services. Las cuentas de Storage de ZRS no son compatibles. El tipo de replicación de la cuenta de Storage se muestra entre paréntesis.

Para la operación posterior a la restauración, use lo siguiente:
* [Usar la plantilla para personalizar la máquina virtual restaurada](#use-templates-to-customize-restore-vm)
* [Usar los discos restaurados para asociarlos a una máquina virtual existente](../virtual-machines/windows/attach-managed-disk-portal.md)
* [Crear una máquina virtual con PowerShell desde los discos restaurados](./backup-azure-vms-automation.md#restore-an-azure-vm)

En la hoja **Restore configuration** (Configuración de restauración), haga clic en **Aceptar** para finalizar la configuración de la restauración. En la hoja **Restaurar**, seleccione **Restaurar** para desencadenar la operación de restauración.

![Configuración de recuperación completa](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

La **restauración en contexto** se realiza a través de la pestaña **Reemplazar el existente**.

## <a name="replace-existing-disks-from-a-restore-point"></a>Reemplazar discos existentes desde un punto de restauración
La opción **Reemplazar el existente** ayuda a reemplazar los discos existentes en la VM actual por el punto de restauración seleccionado. Esta operación puede realizarse solo si existe la VM actual. Si se ha eliminado por algún motivo, no se puede realizar esta operación; como alternativa, se recomienda usar la opción **Crear nuevo** para crear máquinas virtuales o discos para continuar con las operaciones de restauración. Durante las operaciones de reemplazo de discos existentes, como medida de precaución, realizamos la copia de seguridad de los datos antes de iniciar dichas operaciones. Si el punto de restauración tiene más o menos discos que la máquina virtual actual, el número de discos del punto de restauración solo se reflejará en la VM. La opción **Reemplazar el existente** actualmente no es compatible con discos no administrados y máquinas virtuales cifradas. Tampoco se admite para [máquinas virtuales generalizadas](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource) y para máquinas virtuales creadas mediante [imágenes personalizadas](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).  

 En la hoja **Restaurar configuración**, la única entrada que debe seleccionarse es **Ubicación de ensayo**.

   ![Opción Reemplazar el existente del asistente para configuración de restauración](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)

 a. **Tipo de restauración**. Reemplace los discos que representan que el punto de restauración seleccionado reemplazará los discos en la VM existente.

 b. **Ubicación de ensayo**. Las cuentas de almacenamiento son la ubicación de ensayo. Este menú muestra la lista de cuentas de almacenamiento de la misma ubicación que el almacén de Recovery Services. No se admiten cuentas de almacenamiento con redundancia de zona. Si no hay ninguna cuenta de almacenamiento con la misma ubicación que el almacén de Recovery Services, debe crear una antes de iniciar la operación de restauración. El tipo de replicación de la cuenta de almacenamiento se muestra entre paréntesis.

## <a name="track-the-restore-operation"></a>Seguimiento de la operación de restauración
Una vez que se desencadene la operación de restauración, el servicio de copia de seguridad crea un trabajo para realizar su seguimiento. El servicio de copia de seguridad también crea y muestra temporalmente la notificación en el área **Notificaciones** del portal. Si no ve la notificación, seleccione el símbolo **Notificaciones** para ver las notificaciones.

![Restauración desencadenada](./media/backup-azure-arm-restore-vms/restore-notification1.png)

Haga clic en el hipervínculo de las notificaciones para ir a la lista **BackupJobs**. Todos los detalles de las operaciones de un trabajo determinado están disponibles en la lista **BackupJobs**. Puede ir a la lista **BackupJobs** desde el panel del almacén. Para ello, haga clic en el icono Trabajos de copia de seguridad y seleccione **Máquina virtual de Azure** para mostrar los trabajos asociados al almacén.

Se abre la hoja **Trabajos de copia de seguridad**, que muestra la lista de trabajos.

![Lista de máquinas virtuales en un almacén](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

## <a name="use-templates-to-customize-a-restored-vm"></a>Uso de plantillas para personalizar una máquina virtual restaurada
Una vez que ha [finalizado la operación de los discos de restauración](#Track-the-restore-operation), use la plantilla que se generó como parte de la operación de restauración para crear una nueva máquina virtual con una configuración diferente a la de la configuración de copia de seguridad. También puede usarla para personalizar los nombres de recursos que se crearon durante el proceso de creación de una nueva máquina virtual desde un punto de restauración.

![Exploración en profundidad del trabajo de restauración](./media/backup-azure-arm-restore-vms/restore-job-drill-down1.png)

Para obtener la plantilla que se generó como parte de la opción de discos de restauración, siga estos pasos:

1. Vaya a la opción **Restore Job Details** (Detalles del trabajo de restauración) correspondiente al trabajo.

2. En la pantalla **Restore Job Details** (Detalles del trabajo de restauración), seleccione **Implementar plantilla** para iniciar la implementación de la plantilla.

3. En la hoja **Implementar plantilla** para la implementación personalizada, use la implementación de plantillas para [editar e implementar la plantilla](../azure-resource-manager/resource-group-template-deploy-portal.md#deploy-resources-from-custom-template), o bien agregue más personalizaciones y [cree una plantilla](../azure-resource-manager/resource-group-authoring-templates.md) antes de realizar la implementación.

  ![Carga de la implementación de plantilla](./media/backup-azure-arm-restore-vms/edit-template1.png)

4. Después de escribir los valores necesarios, acepte los **términos y condiciones** y seleccione **Comprar**.

  ![Envío de la implementación de plantilla](./media/backup-azure-arm-restore-vms/submitting-template1.png)

## <a name="post-restore-steps"></a>Pasos posteriores a la restauración
* Si usa una distribución de Linux basada en cloud-init, como Ubuntu, la contraseña se bloquea después de la restauración por motivos de seguridad. Use la extensión VMAccess en la máquina virtual restaurada para [restablecer la contraseña](../virtual-machines/linux/reset-password.md). Se recomienda usar claves SSH en estas distribuciones para evitar que se restablezca la contraseña después de la restauración.
* Las extensiones existentes durante la configuración de copia de seguridad están instaladas, pero no se habilitarán. Si ve un problema, vuelva a instalar las extensiones.
* Si la máquina virtual de copia de seguridad tiene una dirección IP estática posterior a la restauración, la máquina virtual restaurada tendrá una dirección IP dinámica para evitar conflictos cuando se cree una máquina virtual restaurada. Aprenda más sobre cómo puede [agregar una dirección IP estática a una máquina virtual restaurada](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm).
* Una máquina virtual restaurada no tiene un conjunto de valores de disponibilidad. Se recomienda usar la opción de discos de restauración para [agregar un conjunto de disponibilidad](../virtual-machines/windows/tutorial-availability-sets.md) cuando se crea una máquina virtual con PowerShell o plantillas mediante los discos restaurados.


## <a name="backup-for-restored-vms"></a>Copia de seguridad de máquinas virtuales restauradas
Si restauró una máquina virtual en el mismo grupo de recursos con el mismo nombre que la máquina virtual de copia de seguridad original, la copia de seguridad seguirá en la máquina virtual después de la restauración. Si restauró la máquina virtual en otro grupo de recursos o especificó un nombre diferente para la máquina virtual restaurada, la máquina virtual se trata como si fuera una nueva máquina virtual. Deberá configurar la copia de seguridad para la máquina virtual restaurada.

## <a name="restore-a-vm-during-an-azure-datacenter-disaster"></a>Restauración de una máquina durante un desastre del centro de datos de Azure
Azure Backup permite restaurar máquinas virtuales de copia de seguridad en el centro de datos emparejado, en caso de que el centro de datos principal donde se ejecutan las máquinas virtuales experimente una situación de desastre y haya configurado el almacén de copia de seguridad para que tenga redundancia geográfica. Durante estos escenarios, seleccione una cuenta de almacenamiento, que esté presente en un centro de datos emparejado. El resto del proceso de restauración sigue siendo el mismo. Backup usa el servicio de proceso de la zona geográfica emparejada para crear la máquina virtual restaurada. Para más información, consulte [Azure datacenter resiliency](../resiliency/resiliency-technical-guidance-recovery-loss-azure-region.md) (Resistencia del centro de datos de Azure).

## <a name="restore-domain-controller-vms"></a>Restauración de máquinas virtuales de controlador de dominio
La copia de seguridad de las máquinas virtuales de controlador de dominio (DC) es un escenario admitido con Azure Backup. Sin embargo, se debe tener cuidado durante el proceso de restauración. El proceso de restauración correcto depende de la estructura del dominio. El caso más simple es tener un único DC en un solo dominio. El caso más común en las cargas de producción es tener un solo dominio con varios DC y, quizás, algún DC local. Por último, puede tener un bosque con varios dominios.

Desde la perspectiva de Active Directory, una máquina virtual de Azure es como cualquier otra máquina virtual de un hipervisor admitido moderno. La principal diferencia respecto a los hipervisores locales es que no hay ninguna consola de máquina virtual disponible en Azure. Se necesita una consola para determinados escenarios, como para la recuperación mediante una copia de seguridad de reconstrucción completa (BMR). Sin embargo, la restauración de una VM desde el almacén de Backup es una sustitución completa para una BMR. El modo de restauración de servicios de directorio (DSRM) también está disponible, de modo que todos los escenarios de recuperación de Active Directory son viables. Para más información, consulte [Consideraciones relacionadas con la copia de seguridad y la restauración para controladores de dominio virtualizados](https://technet.microsoft.com/library/virtual_active_directory_domain_controller_virtualization_hyperv(v=ws.10).aspx#backup_and_restore_considerations_for_virtualized_domain_controllers) y [Planning for Active Directory Forest Recovery](https://technet.microsoft.com/library/planning-active-directory-forest-recovery(v=ws.10).aspx) (Planeamiento de la recuperación de bosques de Active Directory).

### <a name="single-dc-in-a-single-domain"></a>Controlador de dominio único en un solo dominio
La máquina virtual se puede restaurar (como cualquier otra máquina virtual) desde Azure Portal o mediante PowerShell.

### <a name="multiple-dcs-in-a-single-domain"></a>Varios controladores de dominio en un solo dominio
Cuando se pueda acceder a otros controladores de dominio del mismo dominio a través de la red, el controlador de dominio se podrá restaurar como cualquier VM. Si se trata del último controlador de dominio que queda en el dominio o bien se lleva a cabo una recuperación en una red aislada, debe seguirse el procedimiento de recuperación de bosques.

### <a name="multiple-domains-in-one-forest"></a>Varios dominios en un solo bosque
Cuando se pueda acceder a otros controladores de dominio del mismo dominio a través de la red, el controlador de dominio se podrá restaurar como cualquier VM. En todos los demás casos, se recomienda una recuperación de bosques.

## <a name="restore-vms-with-special-network-configurations"></a>Restauración de máquinas virtuales con configuraciones de red especiales
Es posible hacer una copia de seguridad y restaurar máquinas virtuales con las siguientes configuraciones de red especiales. Sin embargo, estas configuraciones requieren consideraciones especiales al pasar por el proceso de restauración:

* Máquinas virtuales con equilibradores de carga (interno y externo)
* Máquinas virtuales con varias direcciones IP reservadas
* Paso 3: Creación de máquinas virtuales con varias NIC

> [!IMPORTANT]
> Al crear la configuración especial de red para las máquinas virtuales, debe usar PowerShell para crear máquinas virtuales a partir de los discos restaurados.
>
>

Con el fin de volver a crear completamente las máquinas virtuales después de restaurarlas en el disco, siga estos pasos:

1. Restaure los discos desde un almacén de Recovery Services mediante [PowerShell](backup-azure-vms-automation.md#restore-an-azure-vm).

1. Cree la configuración de máquina virtual necesaria para el equilibrador de carga/varias NIC/varias IP reservadas mediante los cmdlets de PowerShell. Úsela para crear la máquina virtual con la configuración deseada:

   a. Cree una máquina virtual en el servicio en la nube con el [equilibrador de carga interno ](https://azure.microsoft.com/documentation/articles/load-balancer-internal-getstarted/).

   b. Cree una máquina virtual para conectarse al [equilibrador de carga accesible desde Internet](https://azure.microsoft.com/documentation/articles/load-balancer-internet-getstarted/).

   c. Cree una máquina virtual con [varias NIC](../virtual-machines/windows/multiple-nics.md).

   d. Cree una máquina virtual con [varias direcciones IP reservadas](../virtual-network/virtual-network-multiple-ip-addresses-powershell.md).

## <a name="next-steps"></a>Pasos siguientes
Ahora que se pueden restaurar las máquinas virtuales, consulte el artículo de solución de problemas para más información sobre los errores comunes con las máquinas virtuales. Asimismo, consulte el artículo sobre la administración de las tareas con las máquinas virtuales.

* [Solución de errores](backup-azure-vms-troubleshoot.md#restore)
* [Administración de máquinas virtuales](backup-azure-manage-vms.md)
