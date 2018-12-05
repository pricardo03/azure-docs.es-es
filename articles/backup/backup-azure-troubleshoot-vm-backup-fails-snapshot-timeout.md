---
title: 'Solución del error de Azure Backup: no está disponible el estado del agente invitado'
description: Síntomas, causas y soluciones de errores de Azure Backup relacionados con el agente, la extensión y los discos.
services: backup
author: genlin
manager: cshepard
keywords: Azure Backup; Agente de máquina virtual; Conectividad de red;
ms.service: backup
ms.topic: troubleshooting
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: d8b78551a762b4388344aaf3b44e7472127737ae
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "51977121"
---
# <a name="troubleshoot-azure-backup-failure-issues-with-the-agent-or-extension"></a>Solución de errores de Azure Backup: problemas con el agente o la extensión

En este artículo se proporcionan los pasos de solución de problemas que pueden ayudar a resolver los errores de Azure Backup relacionados con la comunicación con el agente y la extensión de máquina virtual.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="UserErrorGuestAgentStatusUnavailable-vm-agent-unable-to-communicate-with-azure-backup"></a>UserErrorGuestAgentStatusUnavailable: El agente de máquina virtual no se puede comunicar con Azure Backup

**Código de error**: UserErrorGuestAgentStatusUnavailable <br>
**Mensaje de error**: El agente de máquina virtual no se puede comunicar con Azure Backup<br>

Después de registrar y programar una máquina virtual para el servicio Backup, dicho servicio inicia el trabajo comunicándose con el agente de la máquina virtual para sacar una instantánea de un momento dado. Cualquiera de las condiciones siguientes puede impedir que la instantánea se desencadene. Cuando una instantánea no se desencadena, se puede producir un error en la copia de seguridad. Realice los pasos de solución de problemas siguientes en el orden indicado y, a continuación, vuelva a intentar la operación:<br>
**Causa 1: [El agente está instalado en la máquina virtual, pero no responde (en máquinas virtuales Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**    
**Causa 2: [El agente instalado en la máquina virtual está obsoleto (en máquinas virtuales Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 3: [No se puede recuperar el estado de las instantáneas o no se pueden tomar instantáneas](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**    
**Causa 4: [La extensión de la copia de seguridad no se puede actualizar ni cargar](#the-backup-extension-fails-to-update-or-load)**  
**Causa 5: [La VM no tiene acceso a Internet](#the-vm-has-no-internet-access)**

## <a name="guestagentsnapshottaskstatuserror---could-not-communicate-with-the-vm-agent-for-snapshot-status"></a>GuestAgentSnapshotTaskStatusError: No se pudo comunicar con el agente de máquina virtual para el estado de la instantánea

**Código de error**: GuestAgentSnapshotTaskStatusError<br>
**Mensaje de error**: No se pudo comunicar con el agente de máquina virtual para ver el estado de la instantánea <br>

Después de registrar y programar una máquina virtual para el servicio de Azure Backup, Backup inicia el trabajo al comunicarse con la extensión de copia de seguridad de la máquina virtual para sacar una instantánea de un momento dado. Cualquiera de las condiciones siguientes puede impedir que la instantánea se desencadene. Si la instantánea no se desencadena, se podría producir un error en la copia de seguridad. Realice los pasos de solución de problemas siguientes en el orden indicado y, a continuación, vuelva a intentar la operación:  
**Causa 1: [El agente está instalado en la máquina virtual, pero no responde (en máquinas virtuales Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 2: [El agente instalado en la máquina virtual está obsoleto (en máquinas virtuales Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 3: [La máquina virtual no tiene acceso a Internet](#the-vm-has-no-internet-access)**

## <a name="usererrorrpcollectionlimitreached---the-restore-point-collection-max-limit-has-reached"></a>UserErrorRpCollectionLimitReached: Se ha alcanzado el límite máximo de colecciones del punto de restauración

**Código de error**: UserErrorRpCollectionLimitReached <br>
**Mensaje de error**: Se ha alcanzado el límite máximo de colecciones del punto de restauración. <br>
* Este problema puede ocurrir si hay un bloqueo en el grupo de recursos del punto de recuperación que impida la limpieza automática del punto de recuperación.
* Este problema también puede ocurrir si se desencadenan varias copias de seguridad al día. Actualmente recomendamos solo una copia de seguridad por día, ya que los puntos de restauración instantáneos se retienen durante 7 días y solo 18 puntos de restauración instantáneos se pueden asociar a una máquina virtual en cualquier momento dado. <br>

Acción recomendada:<br>
Para resolver este problema, elimine el bloqueo del grupo de recursos y vuelva a intentar la operación para desencadenar la limpieza.

> [!NOTE]
    > El servicio Backup crea un grupo de recursos diferente al de la máquina virtual para guardar la colección de puntos de restauración. Es conveniente que los clientes no bloqueen el grupo de recursos que se ha creado para que lo utilice el servicio Backup. El formato de nomenclatura del grupo de recursos creado por el servicio Backup es: AzureBackupRG_`<Geo>`_`<number>` Eg: AzureBackupRG_northeurope_1

**Paso 1: [Eliminación del bloqueo del grupo de recursos de punto de restauración](#remove_lock_from_the_recovery_point_resource_group)** <br>
**Paso 2:[ Limpieza de la colección de puntos de restauración](#clean_up_restore_point_collection)**<br>

## <a name="usererrorkeyvaultpermissionsnotconfigured---backup-doesnt-have-sufficient-permissions-to-the-key-vault-for-backup-of-encrypted-vms"></a>UserErrorKeyvaultPermissionsNotConfigured: Backup no tiene suficientes permisos para el almacén de claves y no se puede realizar la copia de seguridad de las VM cifradas.

**Código de error**: UserErrorKeyvaultPermissionsNotConfigured <br>
**Mensaje de error**: Backup no tiene suficientes permisos para el almacén de claves y no se puede realizar la copia de seguridad de las VM cifradas. <br>

Para que la operación de copia de seguridad se complete correctamente en las VM cifradas, debe tener permisos para acceder al almacén de claves. Esto puede hacerse mediante [Azure Portal](https://docs.microsoft.com/azure/backup/backup-azure-vms-encryption#provide-permissions-to-backup) o [PowerShell](https://docs.microsoft.com/azure/backup/backup-azure-vms-automation#enable-protection).

## <a name="ExtensionSnapshotFailedNoNetwork-snapshot-operation-failed-due-to-no-network-connectivity-on-the-virtual-machine"></a>ExtensionSnapshotFailedNoNetwork: Error de la operación de instantánea debido a que no hay conectividad de red en la máquina virtual

**Código de error**: ExtensionSnapshotFailedNoNetwork<br>
**Mensaje de error**: Error de la operación de instantánea debido a que no hay conectividad de red en la máquina virtual<br>

Después de registrar y programar una máquina virtual para el servicio de Azure Backup, Backup inicia el trabajo al comunicarse con la extensión de copia de seguridad de la máquina virtual para sacar una instantánea de un momento dado. Cualquiera de las condiciones siguientes puede impedir que la instantánea se desencadene. Si la instantánea no se desencadena, se podría producir un error en la copia de seguridad. Realice los pasos de solución de problemas siguientes en el orden indicado y, a continuación, vuelva a intentar la operación:    
**Causa 1: [No se puede recuperar el estado de las instantáneas o no se pueden tomar instantáneas](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 2: [La extensión de la copia de seguridad no se puede actualizar ni cargar](#the-backup-extension-fails-to-update-or-load)**  
**Causa 3: [La máquina virtual no tiene acceso a Internet](#the-vm-has-no-internet-access)**

## <a name="ExtentionOperationFailed-vmsnapshot-extension-operation-failed"></a>ExtentionOperationFailedForManagedDisks: error en la operación de extensión VMSnapshot

**Código de error**: ExtentionOperationFailedForManagedDisks <br>
**Mensaje de error**: Error en la operación de extensión VMSnapshot<br>

Después de registrar y programar una máquina virtual para el servicio de Azure Backup, Backup inicia el trabajo al comunicarse con la extensión de copia de seguridad de la máquina virtual para sacar una instantánea de un momento dado. Cualquiera de las condiciones siguientes puede impedir que la instantánea se desencadene. Si la instantánea no se desencadena, se podría producir un error en la copia de seguridad. Realice los pasos de solución de problemas siguientes en el orden indicado y, a continuación, vuelva a intentar la operación:  
**Causa 1: [No se puede recuperar el estado de las instantáneas o no se pueden tomar instantáneas](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 2: [La extensión de la copia de seguridad no se puede actualizar ni cargar](#the-backup-extension-fails-to-update-or-load)**  
**Causa 3: [El agente está instalado en la máquina virtual, pero no responde (en máquinas virtuales Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 4: [El agente instalado en la máquina virtual está obsoleto (en máquinas virtuales Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**

## <a name="backupoperationfailed--backupoperationfailedv2---backup-fails-with-an-internal-error"></a>BackUpOperationFailed/BackUpOperationFailedV2: Se ha producido un error interno en la copia de seguridad

**Código de error**: BackUpOperationFailed/BackUpOperationFailedV2 <br>
**Mensaje de error**: Error interno al realizar la copia de seguridad. Intente volver a realizar la operación en unos minutos <br>

Después de registrar y programar una máquina virtual para el servicio de Azure Backup, Backup inicia el trabajo al comunicarse con la extensión de copia de seguridad de la máquina virtual para sacar una instantánea de un momento dado. Cualquiera de las condiciones siguientes puede impedir que la instantánea se desencadene. Si la instantánea no se desencadena, se podría producir un error en la copia de seguridad. Realice los pasos de solución de problemas siguientes en el orden indicado y, a continuación, vuelva a intentar la operación:  
**Causa 1: [El agente está instalado en la VM, pero no responde (en VM Windows)](#the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms)**  
**Causa 2: [El agente instalado en la máquina virtual está obsoleto (en máquinas virtuales Linux)](#the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms)**  
**Causa 3: [No se puede recuperar el estado de las instantáneas o no se pueden tomar instantáneas](#the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken)**  
**Causa 4: [La extensión de la copia de seguridad no se puede actualizar ni cargar](#the-backup-extension-fails-to-update-or-load)**  
**Causa 5: [El servicio de Backup no tiene permiso para eliminar los puntos de restauración antiguos debido a un bloqueo del grupo de recursos](#backup-service-does-not-have-permission-to-delete-the-old-restore-points-due-to-resource-group-lock)** <br>
**Causa 6: [La VM no tiene acceso a Internet](#the-vm-has-no-internet-access)**

## <a name="usererrorunsupporteddisksize---currently-azure-backup-does-not-support-disk-sizes-greater-than-1023gb"></a>UserErrorUnsupportedDiskSize: Azure Backup no admite actualmente tamaños de disco mayores que 1023 GB.

**Código de error**: UserErrorUnsupportedDiskSize <br>
**Mensaje de error**: Azure Backup no admite actualmente tamaños de disco mayores que 1023 GB. <br>

La operación de copia de seguridad podría generar un error al realizar una copia de seguridad de una máquina virtual con un tamaño de disco mayor que 1023 GB, ya que el almacén no se actualiza a la pila de copia de seguridad de máquina virtual de Azure V2. Al actualizar a la pila de copia de seguridad de máquina virtual de Azure V2, se admitirán hasta 4 TB. Revise estas [ventajas](backup-upgrade-to-vm-backup-stack-v2.md) y [consideraciones](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade) y después continúe con la actualización según estas [instrucciones](backup-upgrade-to-vm-backup-stack-v2.md#upgrade).  

## <a name="usererrorstandardssdnotsupported---currently-azure-backup-does-not-support-standard-ssd-disks"></a>UserErrorStandardSSDNotSupported: actualmente Azure Backup no admite discos SSD estándar.

**Código de error**: UserErrorStandardSSDNotSupported <br>
**Mensaje de error**: actualmente Azure Backup no admite discos SSD estándar. <br>

Actualmente Azure Backup admite discos SSD estándar solo para los almacenes que se actualizan a la pila de copia de seguridad de máquina virtual de Azure V2. Revise estas [ventajas](backup-upgrade-to-vm-backup-stack-v2.md) y [consideraciones](backup-upgrade-to-vm-backup-stack-v2.md#considerations-before-upgrade) y después continúe con la actualización según estas [instrucciones](backup-upgrade-to-vm-backup-stack-v2.md#upgrade).


## <a name="causes-and-solutions"></a>Causas y soluciones

### <a name="the-vm-has-no-internet-access"></a>La máquina virtual no tiene acceso a Internet.
Según el requisito de implementación, la máquina virtual no tiene acceso a Internet. O bien, podría tener restricciones que impiden el acceso a la infraestructura de Azure.

Para poder funcionar correctamente, la extensión de copia de seguridad requiere conectividad a las direcciones IP públicas de Azure. La extensión envía comandos a un punto de conexión de Azure Storage (dirección URL de HTTPS) para administrar las instantáneas de la máquina virtual. Si la extensión no tiene acceso a la red Internet pública, se produce un error en la copia de seguridad.

Es posible implementar un servidor proxy para enrutar el tráfico de la máquina virtual.
##### <a name="create-a-path-for-https-traffic"></a>Crear una ruta de acceso para el tráfico HTTPS

1. Si tiene alguna restricción de red implementada (por ejemplo, un grupo de seguridad de red), implemente un servidor proxy HTTPS para enrutar el tráfico.
2. Para permitir el acceso a Internet desde el servidor proxy HTTPS, agregue las reglas al grupo de seguridad de red, si dispone de uno.

Para aprender a configurar un proxy HTTPS para las copias de seguridad de la máquina virtual, consulte [Preparación del entorno para la copia de seguridad de Azure Virtual Machines](backup-azure-arm-vms-prepare.md#establish-network-connectivity).

La máquina virtual de la que se ha realizado la copia de seguridad o el servidor proxy mediante el cual se ha enrutado el tráfico requieren acceso a las direcciones IP públicas de Azure

####  <a name="solution"></a>Solución
Para solucionar este problema, pruebe uno de los métodos siguientes:

##### <a name="allow-access-to-azure-storage-that-corresponds-to-the-region"></a>Permitir el acceso al almacenamiento de Azure que corresponda a la región.

Puede usar [etiquetas de servicio](../virtual-network/security-overview.md#service-tags) para permitir conexiones al almacenamiento en la región específica. Asegúrese de que la regla que permite el acceso a la cuenta de almacenamiento tenga mayor prioridad que la que bloquea el acceso a Internet.

![Grupo de seguridad de red con etiquetas de almacenamiento para una región](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Para entender el procedimiento paso a paso para configurar etiquetas de servicio, vea [este vídeo](https://youtu.be/1EjLQtbKm1M).

> [!WARNING]
> Las etiquetas de servicio de almacenamiento se encuentran en versión preliminar. Solo están disponibles en determinadas regiones. Para ver la lista de regiones, consulte el apartado [Etiquetas de servicio para almacenamiento](../virtual-network/security-overview.md#service-tags).

Si usa Azure Managed Disks, necesitará abrir otro puerto (8443) en los firewalls.

Además, si la subred no tiene una ruta para el tráfico saliente de Internet, deberá agregar un punto de conexión de servicio con la etiqueta de servicio "Microsoft.Storage" a la subred.

### <a name="the-agent-installed-in-the-vm-but-unresponsive-for-windows-vms"></a>El agente está instalado en la máquina virtual, pero no responde (en máquinas virtuales Windows)

#### <a name="solution"></a>Solución
Es posible que el agente de máquina virtual se haya dañado o que el servicio se haya detenido. Puede intentar volver a instalar al agente de máquina virtual para obtener la versión más reciente. O bien, intente restablecer la comunicación con el servicio.

1. Determine si el servicio Windows Guest Agent se ejecuta en los servicios de máquina virtual (services.msc). Intente reiniciar el servicio Windows Guest Agent e inicie la copia de seguridad.    
2. Si el servicio Windows Guest Agent no se muestra en los servicios, vaya al Panel de control y seleccione **Programas y características** para determinar si dicho servicio está instalado.
4. Si aparece en **Programas y características**, desinstálelo.
5. Descargue e instale la [versión más reciente del MSI del agente](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Debe tener derechos de administrador para completar la instalación.
6. Compruebe que los servicios de agente invitado de Windows aparecen en los servicios.
7. Ejecute un trabajo de copia de seguridad a petición:
    * En el portal, seleccione **Crear copia de seguridad ahora**.

Además, compruebe que [Microsoft .NET 4.5](https://docs.microsoft.com/dotnet/framework/migration-guide/how-to-determine-which-versions-are-installed) está instalado en la máquina virtual, ya que se requiere para que el agente de máquina virtual se comunique con el servicio.

### <a name="the-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>El agente instalado en la máquina virtual está obsoleto (en el caso de máquinas virtuales Linux)

#### <a name="solution"></a>Solución
La mayoría de los errores relacionados con el agente o la extensión de máquinas virtuales de Linux están provocados por problemas que afectan a un agente VM obsoleto. Para solucionar este problema, siga estas directrices generales:

1. Siga las instrucciones para [actualizar el agente de máquina virtual Linux ](../virtual-machines/linux/update-agent.md).

 > [!NOTE]
 > Se *recomienda encarecidamente* actualizar el agente solo a través de un repositorio de distribución. No recomendamos descargar el código de agente desde GitHub directamente y actualizarlo. Si el último agente no está disponible para su distribución, póngase en contacto con el soporte técnico de distribución para obtener instrucciones sobre cómo instalarlo. Para buscar el agente más reciente, vaya a la página del [agente Linux de Microsoft Azure](https://github.com/Azure/WALinuxAgent/releases) en el repositorio de GitHub.

2. Asegúrese de que el agente de Azure se ejecuta en la máquina virtual mediante la ejecución del comando siguiente: `ps -e`.

 Si el proceso no se está ejecutando, reinícielo mediante los siguientes comandos:

 * Para Ubuntu: `service walinuxagent start`
 * Para otras distribuciones: `service waagent start`

3. [Configure el agente de reinicio automático](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).
4. Ejecute una nueva copia de seguridad de prueba. Si el error persiste, recopile los registros siguientes de la máquina virtual:

   * /var/lib/waagent/*.xml
   * /var/log/waagent.log
   * /var/log/azure/*

Si se requiere el registro detallado para waagent, siga estos pasos:

1. En el archivo /etc/waagent.conf, localice la línea siguiente: **Enable verbose logging (y|n)** (Habilitar registro detallado [s/n]).
2. Cambie el valor de **Logs.Verbose** de *n* a *y*.
3. Guarde el cambio y, a continuación, reinicie waagent siguiendo los pasos descritos anteriormente en esta sección.

###  <a name="the-snapshot-status-cannot-be-retrieved-or-a-snapshot-cannot-be-taken"></a>No se puede recuperar el estado de las instantáneas o no se pueden tomar instantáneas
La copia de seguridad de máquina virtual se basa en la emisión de comandos de instantánea para la cuenta del almacenamiento subyacente. La copia de seguridad puede producir un error porque no tiene ningún acceso a la cuenta de almacenamiento o porque se retrasa la ejecución de la tarea de instantáneas.

#### <a name="solution"></a>Solución
Las siguientes condiciones podrían hacer que la tarea de instantáneas no se realizara:

| Causa | Solución |
| --- | --- |
| El estado de la máquina virtual no se notifica correctamente porque la máquina virtual está apagada en el Protocolo de escritorio remoto (RDP). | Si ha apagado la máquina virtual en RDP, compruebe el portal para determinar si ese estado de la máquina virtual es correcto. Si no es así, apague la máquina virtual en el portal mediante la opción **Apagar** en el panel de la máquina virtual. |
| La máquina virtual no puede obtener la dirección de host o de tejido de DHCP. | DHCP debe estar habilitado dentro del invitado para que la copia de seguridad de la máquina virtual de IaaS funcione. Si la máquina virtual no puede obtener la dirección de host o de tejido de la respuesta 245 de DHCP, no podrá descargar ni ejecutar ninguna extensión. Si necesita una dirección IP privada estática, debe configurarla a través de **Azure Portal** o **PowerShell** y asegurarse de que está habilitada la opción DHCP dentro de la máquina virtual. Para obtener más información sobre cómo configurar una dirección IP estática a través de PowerShell, consulte [máquina virtual de modelo clásico](../virtual-network/virtual-networks-reserved-private-ip.md#how-to-add-a-static-internal-ip-to-an-existing-vm) y [máquina virtual de Resource Manager](../virtual-network/virtual-networks-static-private-ip-arm-ps.md#change-the-allocation-method-for-a-private-ip-address-assigned-to-a-network-interface).

### <a name="the-backup-extension-fails-to-update-or-load"></a>No se puede actualizar ni cargar la extensión de copia de seguridad
Si no se pueden cargar las extensiones, no se puede realizar la copia de seguridad porque no se puede realizar una instantánea.

#### <a name="solution"></a>Solución

Desinstale la extensión para obligar a la extensión VMSnapshot a cargarse de nuevo. El siguiente intento de copia de seguridad vuelve a cargar la extensión.

Para desinstalar la extensión, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), vaya a la máquina virtual que experimenta los errores de copia de seguridad.
2. Seleccione **Configuración**.
3. Seleccione **Extensiones**.
4. Seleccione **Extensión Vmsnapshot**.
5. Seleccione **Desinstalar**.

Para las VM de Linux, si la extensión VMSnapshot no aparece en Azure Portal, [actualice el agente Linux de Azure](../virtual-machines/linux/update-agent.md) y, a continuación, ejecute la copia de seguridad.

La realización de estos pasos hace que se vuelva a instalar la extensión durante la siguiente copia de seguridad.

### <a name="remove_lock_from_the_recovery_point_resource_group"></a>Eliminación del bloqueo del grupo de recursos de punto de restauración
1. Inicie sesión en el [Azure Portal](http://portal.azure.com/).
2. Vaya a la opción **Todos los recursos**, seleccione el grupo de recursos de la colección de puntos de restauración en el siguiente formato AzureBackupRG_`<Geo>`_`<number>`.
3. En la sección **Configuración**, seleccione **Bloqueos** para mostrar los bloqueos.
4. Para quitar el bloqueo, seleccione los puntos suspensivos y haga clic en **Eliminar**.

    ![Eliminación del bloqueo ](./media/backup-azure-arm-vms-prepare/delete-lock.png)

### <a name="clean_up_restore_point_collection"></a> Eliminación de la colección de puntos de restauración
Después de quitar el bloqueo, los puntos de restauración deben limpiarse. Para limpiar los puntos de restauración, siga cualquiera de los métodos siguientes:<br>
* [Limpieza de la colección de puntos de restauración mediante la ejecución de la copia de seguridad ad hoc](#clean-up-restore-point-collection-by-running-ad-hoc-backup)<br>
* [Eliminación de la colección de puntos de restauración desde Azure Portal](#clean-up-restore-point-collection-from-azure-portal)<br>

#### <a name="clean-up-restore-point-collection-by-running-ad-hoc-backup">Limpieza de la colección de puntos de restauración mediante la ejecución de la copia de seguridad ad hoc</a>
Después de quitar el bloqueo, desencadene una copia de seguridad ad-hoc o manual. Esto garantizará que los puntos de restauración se limpian automáticamente. Espere que esta operación ad-hoc o manual produzca un error la primera vez; sin embargo, asegurará la limpieza automática en lugar de la eliminación manual de los puntos de restauración. Después de la limpieza, debería realizarse correctamente la siguiente copia de seguridad programada.

> [!NOTE]
    > Se realizará la limpieza automática después de unas horas de desencadenar la copia de seguridad ad-hoc o manuales. Si la copia de seguridad programada sigue produciendo un error, pruebe a eliminar manualmente la colección de puntos de restauración mediante los pasos indicados [aquí](#clean-up-restore-point-collection-from-azure-portal).

#### <a name="clean-up-restore-point-collection-from-azure-portal"></a>Eliminación de la colección de puntos de restauración desde Azure Portal <br>

Para borrar manualmente la colección de puntos de restauración que no se han borrado debido al bloqueo del grupo de recursos, pruebe con los siguientes pasos:
1. Inicie sesión en el [Azure Portal](http://portal.azure.com/).
2. En el menú **central**, haga clic en **Todos los recursos**, seleccione el grupo de recursos con el siguiente formato AzureBackupRG_`<Geo>`_`<number>` donde se encuentra la máquina virtual.

    ![Eliminación del bloqueo ](./media/backup-azure-arm-vms-prepare/resource-group.png)

3. Haga clic en el grupo de recursos; se muestra la hoja **Información general**.
4. Seleccione la opción **Mostrar tipos ocultos** para mostrar todos los recursos ocultos. Seleccione las colecciones de puntos de restauración con el siguiente formato AzureBackupRG_`<VMName>`_`<number>`.

    ![Eliminación del bloqueo ](./media/backup-azure-arm-vms-prepare/restore-point-collection.png)

5. Haga clic en **Eliminar** para borrar la colección de puntos de restauración.
6. Vuelva a intentar la operación de copia de seguridad.
