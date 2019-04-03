---
title: Preguntas más frecuentes sobre la copia de seguridad de máquinas virtuales de Azure con Azure Backup
description: Respuestas a preguntas habituales sobre cómo realizar copias de seguridad de máquinas virtuales de Azure con Azure Backup.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: sogup
ms.openlocfilehash: 9f233af316bd6022b93a7208bf3fae37e913e6af
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885271"
---
# <a name="frequently-asked-questions-back-up-azure-vms"></a>Preguntas más frecuentes-Back de máquinas virtuales de Azure

En este artículo se responde preguntas comunes sobre la copia de seguridad de máquinas virtuales de Azure con el [Azure Backup](backup-introduction-to-azure-backup.md) service.


## <a name="backup"></a>Copia de seguridad

### <a name="which-vm-images-can-be-enabled-for-backup-when-i-create-them"></a>¿Cuando crean, qué imágenes de máquina virtual pueden habilitarse para copia de seguridad?
Cuando se crea una máquina virtual, puede habilitar la copia de seguridad para máquinas virtuales que ejecutan [sistemas operativos compatibles](backup-support-matrix-iaas.md#supported-backup-actions)
 
### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>¿Es el costo de copia de seguridad incluido en el costo de máquina virtual? 

 No. Los costos de copia de seguridad son independientes de los costos de la máquina virtual. Obtenga más información sobre [precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-for-a-vm"></a>¿Qué permisos son necesarios para habilitar la copia de seguridad para una máquina virtual? 

Si es un colaborador de máquina virtual, puede habilitar la copia de seguridad en la máquina virtual. Si usa un rol personalizado, necesita los siguientes permisos para habilitar la copia de seguridad en la máquina virtual: 

- Microsoft.RecoveryServices/Vaults/write 
- Microsoft.RecoveryServices/Vaults/read 
- Microsoft.RecoveryServices/locations/* 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/*/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/read 
- Microsoft.RecoveryServices/Vaults/backupFabrics/protectionContainers/protectedItems/write 
- Microsoft.RecoveryServices/Vaults/backupFabrics/backupProtectionIntent/write 
- Microsoft.RecoveryServices/Vaults/backupPolicies/read 
- Microsoft.RecoveryServices/Vaults/backupPolicies/write 
 
Si el almacén de Recovery Services y la máquina virtual tienen distintos grupos de recursos, asegúrese de que tiene permisos de escritura en el grupo de recursos para el almacén de Recovery Services.  


### <a name="what-azure-vms-can-you-back-up-using-azure-backup"></a>¿De qué máquinas virtuales de Azure puede realizar copias de seguridad mediante Azure Backup?

Revise el [matriz de compatibilidad](backup-support-matrix-iaas.md) para detalles sobre la compatibilidad y las limitaciones.

### <a name="does-an-on-demand-backup-job-use-the-same-retention-schedule-as-scheduled-backups"></a>¿Usa un trabajo de copia de seguridad a petición la misma programación de retención que las copias de seguridad programadas?
 No. Especifique la duración de retención para un trabajo de copia de seguridad y a petición. De forma predeterminada, se conservará durante 30 días si se desencadena desde el portal.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Recientemente habilité Azure Disk Encryption en algunas máquinas virtuales. ¿Seguirán funcionando mis copias de seguridad?
Proporcionar permisos para Azure Backup para acceder a Key Vault. Especifique los permisos en PowerShell como se indica en la sección **Habilitar copia de seguridad** de la documentación sobre [PowerShell de Azure Backup](backup-azure-vms-automation.md).

### <a name="i-migrated-vm-disks-to-managed-disks-will-my-backups-continue-to-work"></a>Migré los discos de una máquina virtual a discos administrados. ¿Seguirán funcionando mis copias de seguridad?
Sí, las copias de seguridad funcionan sin problemas. No hay necesidad de volver a configurar nada.

### <a name="why-cant-i-see-my-vm-in-the-configure-backup-wizard"></a>¿Por qué no aparece mi máquina virtual en el asistente para configuración de la copia de seguridad?
El asistente solo muestra las máquinas virtuales que están en la misma región que el almacén y de las que no se está realizando copia de seguridad.

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Mi máquina virtual está apagada. ¿Funcionará un trabajo de copia de seguridad a petición o programado?
Sí. Las copias de seguridad se ejecutan cuando una máquina está apagada. El punto de recuperación se marca como coherente frente a bloqueos.

### <a name="can-i-cancel-an-in-progress-backup-job"></a>¿Puedo cancelar un trabajo de copia de seguridad en curso?
Sí. Puede cancelar el trabajo de copia de seguridad si se encuentra en el estado **Tomando instantánea**. No puede cancelar un trabajo si la transferencia de datos desde la instantánea está en curso.

### <a name="i-enabled-lock-on-resource-group-created-by-azure-backup-service-ie-azurebackuprggeonumber-will-my-backups-continue-to-work"></a>He habilitado un bloqueo en el grupo de recursos creado por el servicio de copia de seguridad de Azure (como) `AzureBackupRG_<geo>_<number>`¿), se seguirán funcionando Mis copias de seguridad?
Si bloquea el grupo de recursos creado por el servicio de copia de seguridad de Azure, las copias de seguridad se iniciará un error porque no hay un límite máximo de 18 puntos de restauración.

Usuario debe quitar el bloqueo y borrar la colección de puntos de restauración de ese grupo de recursos para hacer que las futuras copias de seguridad correcta, [siga estos pasos](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-azure-portal) para quitar la colección de puntos de restauración.

### <a name="does-the-backup-policy-consider-daylight-saving-time-dst"></a>¿Tiene la directiva de copia de seguridad en cuenta el horario de verano (DST)?
 No. La fecha y hora del equipo local tiene el valor local actualizado con el horario de verano actual aplicado. Puede que el establecimiento de la hora de las copias de seguridad programadas difiera de la hora local debido al horario de verano.

### <a name="how-many-data-disks-can-i-attach-to-a-vm-backed-up-by-azure-backup"></a>¿Cuántos discos de datos se pueden conectar a una máquina virtual de la cual Azure Backup ha realizado una copia de seguridad?
Azure Backup puede realizar una copia de seguridad de máquinas virtuales con hasta 16 discos. Se proporciona compatibilidad con 16 discos en la [restauración instantánea](backup-instant-restore-capability.md).

### <a name="does-azure-backup-support-standard-ssd-managed-disk"></a>¿Admite Azure Backup los discos administrados SSD estándar?
Azure Backup admite los [discos administrados SSD estándar](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/). Discos administrados de SSD proporcionan un nuevo tipo de almacenamiento duradero para máquinas virtuales de Azure. Se proporciona compatibilidad con discos SSD administrados en la [restauración instantánea](backup-instant-restore-capability.md).

### <a name="can-we-back-up-a-vm-with-a-write-accelerator-wa-enabled-disk"></a>¿Podemos realizar copias de seguridad de una máquina virtual con un disco habilitado para el Acelerador de escritura?
No se pueden tomar instantáneas en un disco habilitado para el Acelerador de escritura. No obstante, el servicio Azure Backup puede excluir este tipo de disco de la copia de seguridad. La exclusión de discos habilitados para el Acelerador de escritura de las máquinas virtuales solo se admite en las suscripciones actualizadas a Restauración instantánea.

### <a name="i-have-a-vm-with-write-accelerator-wa-disks-and-sap-hana-installed-how-do-i-back-up"></a>Tengo una máquina virtual con discos habilitados para el Acelerador de escritura (WA) y SAP HANA instalada. ¿Cómo puedo realizar la copia de seguridad?
Azure Backup no puede realizar la copia de seguridad de un disco habilitado para el Acelerador de escritura pero puede excluirlo de la copia. Sin embargo, la copia de seguridad no proporcionará coherencia de base de datos ya que no se ha realizado la copia de seguridad de la información contenida en este disco. Puede realizar copias de seguridad de discos con esta configuración si desea realizar la copia de seguridad de un disco de sistema operativo y la copia de seguridad de aquellos discos que no estén habilitados para el Acelerador de escritura.

Estamos ejecutando versión preliminar privada para una copia de seguridad de SAP HANA con un RPO de 15 minutos. Se ha creado de modo similar a la copia de seguridad de bases de datos SQL y usa la interfaz backInt para soluciones de terceros certificadas por SAP HANA. Si le interesa, envíenos un correo electrónico en `AskAzureBackupTeam@microsoft.com` con el asunto **suscribirse a una versión preliminar privada para la copia de seguridad de SAP HANA en Azure Virtual Machines**.


## <a name="restore"></a>Restauración

### <a name="how-do-i-decide-whether-to-restore-disks-only-or-a-full-vm"></a>¿Cómo decidir si es necesario restaurar solo los discos o una máquina virtual completa?
Considere la restauración de máquinas virtuales como una opción de creación rápida para una máquina virtual de Azure. Esta opción cambia los nombres de disco, contenedores usados por los discos, las direcciones IP públicas y los nombres de interfaz de red. El cambio conserva recursos exclusivos cuando se crea una máquina virtual. La máquina virtual no se agrega a un conjunto de disponibilidad.

Puede usar la opción de restauración de disco si quiere:
  * Personalizar la máquina virtual que se crea. Por ejemplo, cambiar el tamaño.
  * Agregar opciones de configuración que no existe en el momento de la copia de seguridad.
  * Controlar la convención de nomenclatura para los recursos que se crean.
  * Agregar la máquina virtual a un conjunto de disponibilidad.
  * Agregar cualquier otro valor que se deba configurar mediante PowerShell o una plantilla.

### <a name="can-i-restore-backups-of-unmanaged-vm-disks-after-i-upgrade-to-managed-disks"></a>¿Puedo restaurar las copias de seguridad de discos de máquinas virtuales no administrados después de actualizar a discos administrados?
Sí, puede usar las copias de seguridad tomadas antes de migrar los discos de no administrados a administrados.
- De forma predeterminada, un trabajo de restauración de máquina virtual crea una máquina virtual no administrada.
- Sin embargo, puede restaurar discos y usarlos para crear una máquina virtual administrada.

### <a name="how-do-i-restore-a-vm-to-a-restore-point-before-the-vm-was-migrated-to-managed-disks"></a>¿Cómo restauro una máquina virtual a un punto de restauración anterior a la migración de la máquina virtual a discos administrados?
De forma predeterminada, un trabajo de restauración de máquina virtual se crea una máquina virtual con discos no administrados. Para crear una máquina virtual con discos administrados:
1. [Restaure a discos no administrados](tutorial-restore-disk.md#restore-a-vm-disk).
2. [Convierta los discos restaurados en discos administrados](tutorial-restore-disk.md#convert-the-restored-disk-to-a-managed-disk).
3. [Cree una máquina virtual con discos administrados](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk).

[Más información](backup-azure-vms-automation.md#restore-an-azure-vm) sobre cómo hacer esto en PowerShell.

### <a name="can-i-restore-the-vm-thats-been-deleted"></a>¿Puedo restaurar la máquina virtual que se ha eliminado?
Sí. Incluso si elimina la máquina virtual, puede ir al elemento de la copia de seguridad correspondiente en el almacén y realizar la restauración desde un punto de recuperación.

### <a name="how-to-restore-a-vm-to-the-same-availability-sets"></a>¿Cómo restaurar una máquina virtual en los mismos conjuntos de disponibilidad?
Para una máquina virtual de Azure con discos administrados, se puede habilitar la restauración en los conjuntos de disponibilidad proporcionando una opción en una plantilla mientras se restaura como discos administrados. Esta plantilla tiene el parámetro de entrada denominado **Conjuntos de disponibilidad**.

### <a name="how-do-we-get-faster-restore-performances"></a>¿Cómo se consiguen rendimientos de restauración más rápidos?
Para mejorar el rendimiento de restauración, nos estamos trasladando a la funcionalidad [restauración instantánea](backup-instant-restore-capability.md).

## <a name="manage-vm-backups"></a>Administrar copias de seguridad de máquina virtual

### <a name="what-happens-if-i-modify-a-backup-policy"></a>¿Qué ocurre si se modifica una directiva de copia de seguridad?
Se realiza la configuración de la máquina virtual con los valores de programación y retención de la directiva nueva o modificada.

- Si se amplía la retención, los puntos de recuperación existentes se marcarán para mantenerlos de acuerdo con la nueva directiva.
- Si se reduce la retención, se marcarán para eliminarse y, posteriormente, se eliminarán en el siguiente trabajo de limpieza.

### <a name="how-do-i-move-a-vm-backed-up-by-azure-backup-to-a-different-resource-group"></a>¿Cómo se puede mover una máquina virtual de la que Azure Backup ha realizado una copia de seguridad a un grupo de recursos diferente?

1. Detenga temporalmente la copia de seguridad y conserve los datos de esta.
2. Traslade la máquina virtual al grupo de recursos de destino.
3. Volver a habilitar la copia de seguridad en el mismo o nuevo almacén.

Puede restaurar la máquina virtual desde los puntos de restauración disponibles creados antes de la operación de traslado.
