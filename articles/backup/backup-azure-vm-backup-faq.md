---
title: Preguntas más frecuentes sobre la copia de seguridad de máquinas virtuales de Azure
description: Respuestas a preguntas habituales sobre cómo funciona la copia de seguridad de máquinas virtuales de Azure, las limitaciones y lo que sucede cuando se producen cambios en las directivas
services: backup
author: trinadhk
manager: shreeshd
keywords: copia de seguridad de máquinas virtuales de azure, restauración de máquinas virtuales de azure, directiva de copia de seguridad
ms.service: backup
ms.topic: conceptual
ms.date: 8/16/2018
ms.author: trinadhk
ms.openlocfilehash: ba77ec34e7887f676ea3df101e87c1ea80fceec5
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50414801"
---
# <a name="questions-about-the-azure-vm-backup-service"></a>Preguntas sobre el servicio de copia de seguridad de Azure Virtual Machines
En este artículo se incluyen respuestas a preguntas habituales para ayudarle a comprender rápidamente los componentes del servicio Backup de Azure Virtual Machines. En algunas de las respuestas, hay vínculos a artículos que tienen información completa. También se pueden publicar preguntas sobre el servicio Azure Backup en el [foro de debate](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="configure-backup"></a>Configuración de la copia de seguridad
### <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>¿Admiten los almacenes de Recovery Services máquinas virtuales implementadas con el modelo clásico o máquinas virtuales implementadas con Resource Manager? <br/>
Los almacenes de Recovery Services admiten ambos modelos.  Puede realizar copias de seguridad de una VM clásica o una VM de Resource Manager en un almacén de Recovery Services.

### <a name="what-configurations-are-not-supported-by-azure-vm-backup"></a>¿Qué configuraciones no se admiten en la copia de seguridad de máquinas virtuales de Azure?
Consulte los [sistemas operativos admitidos](backup-azure-arm-vms-prepare.md#supported-operating-systems-for-backup) y las [limitaciones de la copia de seguridad de máquinas virtuales](backup-azure-arm-vms-prepare.md#limitations-when-backing-up-and-restoring-a-vm).

### <a name="why-cant-i-see-my-vm-in-configure-backup-wizard"></a>¿Por qué no aparece mi máquina virtual en el Asistente para configuración de la copia de seguridad?
En el Asistente para configuración de la copia de seguridad, Azure Backup solo muestra máquinas virtuales con los siguientes criterios:
  * No están aún protegidas: para comprobar el estado de copia de seguridad de una máquina virtual, vaya a la hoja de la máquina virtual y compruebe el estado en el menú Configuración. Aprenda más sobre cómo [comprobar el estado de copia de seguridad de una máquina virtual](backup-azure-vms-first-look-arm.md#configure-the-backup-job-from-the-vm-operations-menu).
  * Pertenece a la misma región que la máquina virtual

## <a name="backup"></a>Copia de seguridad
### <a name="will-on-demand-backup-job-follow-same-retention-schedule-as-scheduled-backups"></a>¿Seguirá el trabajo de copia de seguridad a petición la misma programación de retención que las copias de seguridad programadas?
No. Para un trabajo de copia de seguridad a petición debe especificar el intervalo de retención. De forma predeterminada, se conservará durante 30 días cuando se desencadene desde el portal.

### <a name="i-recently-enabled-azure-disk-encryption-on-some-vms-will-my-backups-continue-to-work"></a>Recientemente habilité Azure Disk Encryption en algunas máquinas virtuales. ¿Seguirán funcionando mis copias de seguridad?
Para acceder a Key Vault, debe conceder permisos para el servicio Azure Backup. Puede proporcionar estos permisos en PowerShell mediante los pasos que se indican en la sección sobre *cómo habilitar Backup* de la documentación de [PowerShell](backup-azure-vms-automation.md).

### <a name="i-migrated-disks-of-a-vm-to-managed-disks-will-my-backups-continue-to-work"></a>Migré los discos de una máquina virtual a discos administrados. ¿Seguirán funcionando mis copias de seguridad?
Sí, las copias de seguridad funcionan a la perfección y sin necesidad de volver a configurar la copia de seguridad.

### <a name="my-vm-is-shut-down-will-an-on-demand-or-a-scheduled-backup-work"></a>Mi máquina virtual está apagada. ¿Funcionará un trabajo de copia de seguridad a petición o programado?
Sí. Incluso cuando una máquina virtual está apagada, las copias de seguridad funcionan y se marca el punto de recuperación como coherente frente a bloqueos. Para más información, consulte la sección de coherencia de datos en [este artículo](backup-azure-vms-introduction.md#how-does-azure-back-up-virtual-machines)

### <a name="can-i-cancel-an-in-progress-backup-job"></a>¿Puedo cancelar un trabajo de copia de seguridad en curso?
Sí. Puede cancelar el trabajo de copia de seguridad si se encuentra en la fase denominada "Tomando instantánea". **No puede cancelar un trabajo si la transferencia de datos de la instantánea está en curso**.

### <a name="i-enabled-resource-group-lock-on-my-backed-up-managed-disk-vms-will-my-backups-continue-to-work"></a>Habilité el bloqueo de grupo de recursos en las máquinas virtuales de mi disco administrado con copia de seguridad. ¿Seguirán funcionando mis copias de seguridad?
Si el usuario bloquea el grupo de recursos, el servicio de Backup no puede eliminar los puntos de restauración anteriores. Debido a esto, las copias de seguridad nuevas comienzan a presentar errores debido a que hay un límite de 18 puntos de restauración que impone el back-end. Si las copias de seguridad presentan un error interno después del bloqueo de grupo de recursos, siga estos [pasos para quitar la colección de puntos de restauración](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md#clean-up-restore-point-collection-from-portal-created-by-backup-service).

### <a name="does-backup-policy-take-daylight-saving-timedst-into-account"></a>¿Tiene en cuenta la directiva de copia de seguridad el horario de verano?
No. Tenga en cuenta que la fecha y la hora de su equipo se muestran en la hora local y con la diferencia del horario de verano actual. Por lo tanto, la hora de las copias de seguridad programadas puede diferir de la hora local debido al horario de verano.

### <a name="maximum-of-how-many-data-disks-can-i-attach-to-a-vm-to-be-backed-up-by-azure-backup"></a>¿Cuántos discos de datos se pueden conectar como máximo a una máquina virtual para realizar copias de seguridad con Azure Backup?
Azure Backup admite ahora la copia de seguridad de máquinas virtuales con hasta 32 discos. Para obtener compatibilidad para 32 discos, [actualice a la versión 2 de la pila de copia de seguridad de máquinas virtuales de Azure](backup-upgrade-to-vm-backup-stack-v2.md). A partir del 24 de septiembre de 2018 se admitirán todas las máquinas virtuales que habiliten la protección.

### <a name="does-azure-backup-support-standard-ssd-managed-disk"></a>¿Azure Backup admite las instancias SSD estándar de Managed Disks?
Azure Backup admite las [instancias SSD estándar de Managed Disks](https://azure.microsoft.com/blog/announcing-general-availability-of-standard-ssd-disks-for-azure-virtual-machine-workloads/), un nuevo tipo de almacenamiento duradero para Microsoft Azure Virtual Machines. Es compatible con discos administrados en la [versión 2 de la pila de copia de seguridad de máquinas virtuales de Azure](backup-upgrade-to-vm-backup-stack-v2.md).

## <a name="restore"></a>Restauración
### <a name="how-do-i-decide-between-restoring-disks-versus-full-vm-restore"></a>¿Cómo decido entre la restauración de discos frente a restauración completa de máquinas virtuales?
Considere la restauración completa de máquinas virtuales de Azure como una opción de creación rápida. La opción de restauración de máquinas virtuales cambia los nombres de los discos, los contenedores usados por esos discos, las direcciones IP públicas y los nombres de las interfaces de red. El cambio es obligatorio para mantener la exclusividad de los recursos creados durante la creación de máquinas virtuales. Pero no permitirá agregar la máquina virtual al conjunto de disponibilidad.

Use discos de restauración para:
  * Personalizar la máquina virtual que se crea a partir de la configuración de restauración a un momento dado, como cambiar el tamaño
  * Agregar configuraciones que no están presentes en el momento de la copia de seguridad
  * Controlar la convención de nomenclatura para los recursos que se crean
  * Agregar una máquina virtual al conjunto de disponibilidad
  * Para cualquier configuración que solo se puede lograr mediante PowerShell o una definición de plantilla declarativa

### <a name="can-i-use-backups-of-unmanaged-disk-vm-to-restore-after-i-upgrade-my-disks-to-managed-disks"></a>¿Puedo usar copias de seguridad de la VM de disco no administrada que se va a restaurar una vez que actualice mis discos a discos administrados?
Sí, puede usar las copias de seguridad tomadas antes de migrar los discos de no administrados a administrados. De forma predeterminada, al restaurarse un trabajo de máquina virtual se creará una máquina virtual con discos no administrados. Puede usar la funcionalidad de restauración de discos para restaurar discos y usarlos para crear una máquina virtual en los discos administrados.

### <a name="what-is-the-procedure-to-restore-a-vm-to-a-restore-point-taken-before-the-conversion-from-unmanaged-to-managed-disks-was-done-for-a-vm"></a>¿Cuál es el procedimiento para restaurar una máquina virtual a un punto de restauración creado antes de que se realizara la conversión de discos no administrados a administrados para una máquina virtual?
En este escenario, de forma predeterminada, al restaurar un trabajo de máquina virtual se creará una máquina virtual con discos no administrados. Para crear una máquina virtual con discos administrados:
1. [Restaure a discos no administrados](tutorial-restore-disk.md#restore-a-vm-disk)
2. [Convierta los discos restaurados en discos administrados](tutorial-restore-disk.md#convert-the-restored-disk-to-a-managed-disk)
3. [Cree una máquina virtual con discos administrados](tutorial-restore-disk.md#create-a-vm-from-the-restored-disk) <br>
Para cmdlets de Powershell, consulte [aquí](backup-azure-vms-automation.md#restore-an-azure-vm).

### <a name="can-i-restore-the-vm-if-my-vm-is-deleted"></a>¿Puedo restaurar la VM si se ha eliminado la VM?
Sí. El ciclo de vida de la VM y su correspondiente elemento de copia de seguridad son diferentes. Por lo tanto, incluso si elimina la VM, puede ir al elemento de copia de seguridad correspondiente en el almacén de Recovery Services y desencadenar una restauración con uno de los puntos de recuperación.

## <a name="manage-vm-backups"></a>Administrar copias de seguridad de máquina virtual
### <a name="what-happens-when-i-change-a-backup-policy-on-vms"></a>¿Qué ocurre cuando se cambia una directiva de copia de seguridad en las máquinas virtuales?
Cuando se aplica una nueva directiva en las máquinas virtuales, se sigue la programación y retención de la nueva directiva. Si se amplía la retención, los puntos de recuperación existentes se marcarán para mantenerlos de acuerdo con la nueva directiva. Si se reduce la retención, se marcan para eliminarse y, posteriormente, se eliminan en el siguiente trabajo de limpieza.

### <a name="how-can-i-move-a-vm-enrolled-in-azure-backup-between-resource-groups"></a>¿Cómo puedo mover una máquina virtual inscrita en Azure Backup entre grupos de recursos?
Siga los pasos que aparecen a continuación para mover correctamente la máquina virtual de la que se ha realizado la copia de seguridad al grupo de recursos de destino.
1. Detenga temporalmente la copia de seguridad y conserve los datos de esta
2. Traslade la máquina virtual al grupo de recursos de destino
3. Vuelva a protegerla con el mismo almacén u otro nuevo

Los usuarios pueden restaurar desde los puntos de restauración disponibles creados antes de la operación de traslado.
