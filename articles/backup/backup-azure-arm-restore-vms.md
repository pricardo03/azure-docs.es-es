---
title: 'Azure Backup: Restauración de máquinas virtuales mediante Azure Portal'
description: Restauración de una máquina virtual de Azure desde un punto de recuperación con Azure Portal
services: backup
author: geethalakshmig
manager: vijayts
keywords: restaurar copias de seguridad; cómo restaurar; punto de recuperación;
ms.service: backup
ms.topic: conceptual
ms.date: 09/04/2017
ms.author: geg
ms.openlocfilehash: ac0c727e41ad9b361ea9f558a97f16446c12ef0e
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/27/2018
ms.locfileid: "53793383"
---
# <a name="use-the-azure-portal-to-restore-virtual-machines"></a>Uso de Azure Portal para restaurar máquinas virtuales
Proteja sus datos tomando instantáneas de sus datos a intervalos definidos. Estas instantáneas se denominan puntos de recuperación y se almacenan en almacenes de Recovery Services. Si es necesario reparar o recompilar una máquina virtual, puede restaurarla desde cualquiera de los puntos de recuperación guardados. Cuando restaura un punto de recuperación, puede:

* Crear una máquina virtual: desde un punto de recuperación a un momento dado de la máquina virtual de copia de seguridad.
* Restaurar discos: use la plantilla que se incluye con el proceso para personalizar la máquina virtual restaurada o realizar una recuperación de archivos individuales.

En este artículo se explica cómo restaurar una máquina virtual en una nueva máquina virtual o restaurar todos los discos de copia de seguridad. Para la recuperación de archivos individuales, consulte el artículo sobre cómo [recuperar archivos de copias de seguridad de máquinas virtuales de Azure](backup-azure-restore-files-from-vm.md).

![Tres formas de restaurar copias de seguridad de máquinas virtuales](./media/backup-azure-arm-restore-vms/azure-vm-backup-restore.png)


Para restaurar una máquina virtual o todos los discos a partir de copias de seguridad de máquinas virtuales, hay que realizar dos pasos:

* Seleccione un punto de restauración.
* Selección del tipo de restauración
    - Opción 1: Creación de una máquina virtual nueva
    - Opción 2: Restauración de los discos

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

    - Para restaurar mediante un punto de restauración de los últimos 30 días, haga clic con el botón derecho en la máquina virtual > **Restore VM** (Restaurar máquina virtual).
    - Para restaurar mediante un punto de restauración de más de 30 días, haga clic en el vínculo situado bajo **Puntos de restauración**.
    - Para enumerar y filtrar las máquinas virtuales con fechas personalizadas, haga clic en **Restore VM** (Restaurar máquina virtual) en el menú. Use el filtro para modificar el intervalo de tiempo para los puntos de restauración mostrados. También puede filtrar por diferentes tipos de coherencia de los datos.
8. Revise la configuración de punto de restauración:
    - La coherencia de datos muestra el [tipo de coherencia](backup-azure-vms-introduction.md#consistency-types) en el punto de recuperación.
    - El **tipo de recuperación** muestra dónde se almacena el punto (en una cuenta de almacenamiento, en el almacén, o ambos). [Más información](https://azure.microsoft.com/blog/large-disk-support/) sobre los puntos de recuperación instantáneos.

  ![Puntos de restauración](./media/backup-azure-arm-restore-vms/vm-blade1.png)
9. Seleccione un punto de restauración.

10. Seleccione **Restaurar configuración**. Se abre la hoja **Restaurar configuración**.

## <a name="choose-a-vm-restore-configuration"></a>Elección de la configuración de restauración de una máquina virtual
Después de seleccionar el punto de restauración, elija la configuración de restauración de una máquina virtual. Para configurar la máquina virtual restaurada, puede usar Azure Portal o PowerShell.

### <a name="restore-options"></a>Opciones de restauración

**Opción** | **Detalles**
--- | ---
**Crear: crear máquina virtual** | Es equivalente a la creación rápida de una máquina virtual. Pone a funcionar una máquina virtual básica desde un punto de restauración.<br/><br/> La configuración de la máquina virtual se puede modificar como parte del proceso de restauración.
**Crear: restaurar disco** | Crea una máquina virtual que se puede personalizar como parte del proceso de restauración.<br/><br/> Esta opción copia los discos duros virtuales en la cuenta de almacenamiento que especifique.<br/><br/> - La cuenta de almacenamiento debe estar en la misma ubicación que el almacén.<br/><br/> Si no tiene una cuenta de almacenamiento adecuada, debe crear una.<br/><br/> El tipo de replicación de la cuenta de almacenamiento se muestra entre paréntesis. No se admite almacenamiento con redundancia de zona (ZRS).<br/><br/> Desde la cuenta de almacenamiento, puede asociar los discos restaurados a una máquina virtual existente o crear una máquina virtual a partir de los discos restaurados mediante PowerShell.
**Reemplazar el existente** | Con esta opción no es necesario crear una máquina virtual.<br/><br/> La máquina virtual actual debe existir. Si se ha eliminado, no se puede usar esta opción.<br/><br/> Azure Backup toma una instantánea de la máquina virtual existente y la almacena en la ubicación de almacenamiento provisional especificada. Los discos existentes conectados a la máquina virtual se reemplazan entonces por el punto de restauración seleccionado. La instantánea que creó anteriormente se copia en el almacén y se almacena como un punto de recuperación según la directiva de retención de copia de seguridad.<br/><br/> Esta opción es compatible con máquinas virtuales administradas no cifradas. No se admite para discos no administrados, [máquinas virtuales generalizadas](https://docs.microsoft.com/azure/virtual-machines/windows/capture-image-resource) o para máquinas virtuales [creadas con imágenes personalizadas](https://azure.microsoft.com/resources/videos/create-a-custom-virtual-machine-image-in-azure-resource-manager-with-powershell/).<br/><br/> Si el punto de restauración tiene más o menos discos que la máquina virtual actual, el número de discos del punto de restauración solo reflejará la máquina virtual.

> [!NOTE]
> Si quiere restaurar una máquina virtual con configuración de red avanzada, por ejemplo, si se administra con un equilibrador de carga interno o externo, o tiene varias NIC o varias direcciones IP reservadas, realice la restauración con PowerShell. [Más información](#restore-vms-with-special-network-configurations).
> Si una máquina virtual Windows usa [licencias de HUB](../virtual-machines/windows/hybrid-use-benefit-licensing.md), utilice la opción **Crear: restaurar disco** y, luego, use PowerShell o la plantilla de restauración para crear la máquina virtual con **Tipo de licencia** establecido en **Windows_Server**. Esta configuración también se puede aplicar después de crear la máquina virtual.


Especifique la configuración de restauración de la manera siguiente:

1. En **Restaurar**, seleccione un [punto de restauración](#select-a-restore-point-for-restore) > **Restaurar configuración**.
2. En **Restaurar configuración**, seleccione cómo desea restaurar la máquina virtual según la configuración resumida en la tabla anterior.

    ![Asistente para configuración de restauración](./media/backup-azure-arm-restore-vms/restore-configuration-create-new1.png)


## <a name="create-new-create-a-vm"></a>Crear nuevo: crear una máquina virtual

1. En **Restaurar configuración** > **Crear nuevo** > **Tipo de restauración**, seleccione **Crear una máquina virtual**.
2. En **Nombre de la máquina virtual**, especifique una máquina virtual que no exista en la suscripción.
3. En **Grupo de recursos**, seleccione un grupo de recursos existente para la nueva máquina virtual, o cree uno con un nombre único global. Si asigna un nombre que ya existe, Azure asigna al grupo el mismo nombre que la máquina virtual.
4. En **Red virtual**, seleccione la red virtual en el que se colocará la máquina virtual. Se muestran todas las redes virtuales asociadas con la suscripción. Seleccione la subred. De forma predeterminada se selecciona la primera subred.
5. En **Ubicación de almacenamiento**, especifique el tipo de almacenamiento usado para la máquina virtual.

    ![Asistente para configuración de restauración](./media/backup-azure-arm-restore-vms/recovery-configuration-wizard1.png)

6. En **Restaurar configuración**, seleccione **Aceptar**. En **Restaurar**, haga clic en **Restaurar** para desencadenar la operación de restauración.



## <a name="create-new-restore-disks"></a>Crear nuevo: restaurar discos

1. En **Restaurar configuración** > **Crear nuevo** > **Tipo de restauración**, seleccione **Restaurar discos**.
2. En **Grupo de recursos**, seleccione un grupo de recursos existente para los discos restaurados o cree uno con un nombre único global.
3. En **Cuenta de almacenamiento**, especifique la cuenta en la que copiar los discos duros virtuales. Asegúrese de que la cuenta esté en la misma región que el almacén.

    ![Configuración de recuperación completa](./media/backup-azure-arm-restore-vms/trigger-restore-operation1.png)

4. En **Restaurar configuración**, seleccione **Aceptar**. En **Restaurar**, haga clic en **Restaurar** para desencadenar la operación de restauración.
5. Después de restaurar el disco, puede realizar cualquiera de las siguientes acciones para completar la operación de restauración de la máquina virtual.

    - Use la plantilla que se generó como parte de la operación de restauración para personalizar la configuración y desencadene la implementación de la máquina virtual. Editará la configuración de plantilla predeterminada y enviará la plantilla para la implementación de la máquina virtual.
    - Puede [asociar los discos restaurados](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-ps) a una máquina virtual existente.
    - Puede [crear una máquina virtual](backup-azure-vms-automation.md#restore-an-azure-vm) a partir de los discos restaurados mediante PowerShell.


## <a name="replace-existing-disks"></a>Reemplazar los discos existentes

Use esta opción para reemplazar los discos existentes de la máquina virtual actual por el punto de restauración seleccionado.

1. En **Restaurar configuración**, haga clic en **Reemplazar el existente**.
2. En **Tipo de restauración**, seleccione **Remplazar discos** (el punto de restauración que reemplazará los discos de la máquina virtual existentes).
3. En **Ubicación de ensayo**, especifique dónde se deben guardar las instantáneas de los discos administrados actuales.

   ![Opción Reemplazar el existente del asistente para configuración de restauración](./media/backup-azure-arm-restore-vms/restore-configuration-replace-existing.png)


## <a name="track-the-restore-operation"></a>Seguimiento de la operación de restauración
Una vez que se desencadene la operación de restauración, el servicio de copia de seguridad crea un trabajo para realizar su seguimiento. El servicio de copia de seguridad también crea y muestra temporalmente la notificación en el área **Notificaciones** del portal. Si no ve la notificación, seleccione el símbolo **Notificaciones** para ver las notificaciones.

![Restauración desencadenada](./media/backup-azure-arm-restore-vms/restore-notification1.png)

Haga clic en el hipervínculo de las notificaciones para ir a la lista **BackupJobs**. Todos los detalles de las operaciones de un trabajo determinado están disponibles en la lista **BackupJobs**. Puede ir a la lista **BackupJobs** desde el panel del almacén. Para ello, haga clic en el icono Trabajos de copia de seguridad y seleccione **Máquina virtual de Azure** para mostrar los trabajos asociados al almacén.

Se abre la hoja **Trabajos de copia de seguridad**, que muestra la lista de trabajos.

![Lista de máquinas virtuales en un almacén](./media/backup-azure-arm-restore-vms/restore-job-in-progress1.png)

La **barra de progreso** ahora está disponible en la hoja **Detalles de la restauración**. Para abrir la hoja **Detalles de la restauración** se puede hacer clic en cualquier trabajo de restauración que tenga el estado **en curso**. La **barra de progreso** está disponible en todas las variantes de restauraciones como **Crear nuevo**, **Restaurar disco** y **Reemplazar el existente**. Los detalles pertenecientes a la barra de progreso de restauración son **hora estimada de restauración**, **porcentaje de restauración** y **número de bytes transferidos**.

A continuación se proporcionan los detalles de la barra de progreso de restauración:

- La **hora estimada de restauración** proporciona inicialmente el tiempo necesario para completar la operación de restauración. A medida que la operación avanza, el tiempo necesario se reduce y llega a 0 una vez que la operación de restauración finaliza.
- El **porcentaje de restauración** proporciona los datos del porcentaje completado de la operación de restauración.
- El **número de bytes transferidos** está disponible en la subtarea cuando la restauración se realiza mediante la opción para crear una máquina virtual. Se proporcionan los detalles de cuántos números de bytes se transfirieron frente al número total de bytes que se van a transferir.


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
