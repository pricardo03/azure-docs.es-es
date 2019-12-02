---
title: Habilitar copia de seguridad al crear una máquina virtual de Azure
description: Describe cómo habilitar la copia de seguridad al crear una VM de Azure con Azure Backup.
ms.topic: conceptual
ms.date: 06/13/2019
ms.openlocfilehash: f34c5dd8cfdc94775b9bd9a896b4cfbe4154ecf8
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2019
ms.locfileid: "74172356"
---
# <a name="enable-backup-when-you-create-an-azure-vm"></a>Habilitar copia de seguridad al crear una máquina virtual de Azure

Use el servicio Azure Backup para hacer copias de seguridad de máquinas virtuales (VM) de Azure. Las copias de seguridad de las VM se crean según una programación especificada en una directiva de copia de seguridad, y los puntos de recuperación se crean a partir de las copias de seguridad. Los datos puntos de recuperación se guardan en almacenes de Recovery Services.

En este artículo se detalla cómo habilitar la copia de seguridad al crear una máquina virtual (VM) en Azure Portal.  

## <a name="before-you-start"></a>Antes de comenzar

- [Compruebe](backup-support-matrix-iaas.md#supported-backup-actions) qué sistemas operativos se admiten si habilita la copia de seguridad al crear una VM.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Si aún no ha iniciado sesión en su cuenta, hágalo en [Azure Portal](https://portal.azure.com).

## <a name="create-a-vm-with-backup-configured"></a>Crear una VM con la copia de seguridad configurada

1. En Azure Portal, haga clic en **Crear un recurso**.

2. En Azure Marketplace, haga clic en **Proceso** y, luego, seleccione una imagen de VM.

3. Configure la VM de acuerdo con las instrucciones para [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) o [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal).

4. En la pestaña **Administración**, en **Habilitar copia de seguridad**, haga clic en **Activado**.
5. Las copias de seguridad de Azure Backup se hacen en un almacén de Recovery Services. Haga clic en **Crear nuevo** si no tiene un almacén existente.
6. Acepte el nombre del almacén sugerido o especifique el suyo.
7. Especifique o cree un grupo de recursos en el que se ubicará el almacén. El almacén del grupo de recursos puede ser diferente del grupo de recursos de la VM.

    ![Habilitar la copia de seguridad para una VM](./media/backup-during-vm-creation/enable-backup.png)

8. Acepte la directiva de copia de seguridad predeterminada o modifique la configuración.
    - Una directiva de copia de seguridad especifica la frecuencia con que se toman instantáneas de copia de seguridad de la VM, y el tiempo que quiere conservar esas copias de seguridad.
    - La directiva predeterminada hace una copia de seguridad de la VM una vez al día.
    - Puede personalizar su propia directiva de copia de seguridad para que una VM de Azure haga copias de seguridad diarias o semanales.
    - [Obtenga más información](backup-azure-vms-introduction.md#backup-and-restore-considerations) sobre las consideraciones de copia de seguridad de VM de Azure.
    - [Obtenga más información](backup-instant-restore-capability.md) sobre la funcionalidad de restauración instantánea.

      ![Directiva de copia de seguridad predeterminada](./media/backup-during-vm-creation/daily-policy.png)

> [!NOTE]
> El servicio Azure Backup crea un grupo de recursos independiente (distinto del grupo de recursos de la VM) para almacenar la instantánea con el formato de nomenclatura **AzureBackupRG_geografía_número** (ejemplo: AzureBackupRG_northeurope_1). Los datos de este grupo de recursos se conservarán durante el intervalo de días especificado en la sección *Conservar las instantáneas de recuperación instantánea* de la directiva de copia de seguridad de la máquina virtual de Azure.  Si se aplica un bloqueo a este grupo de recursos, pueden producirse errores de copia de seguridad. <br> Este grupo de recursos también debe excluirse de todas las restricciones de nombre o etiqueta, ya que una directiva de restricción podría bloquear la creación de colecciones de puntos de recursos en el grupo, lo que provocaría errores de copia de seguridad.

## <a name="start-a-backup-after-creating-the-vm"></a>Iniciar una copia de seguridad después de crear la VM

La copia de seguridad de la VM se ejecuta según la directiva de copia de seguridad. Sin embargo, se recomienda ejecutar una copia de seguridad inicial.

Una vez creada la VM, haga lo siguiente:

1. En las propiedades de la VM, haga clic en **Copia de seguridad**. El estado de la VM es Copia de seguridad inicial pendiente hasta que se ejecuta la copia de seguridad inicial.
2. Haga clic en **Hacer copia ahora** para ejecutar una copia de seguridad a petición.

    ![Ejecución de una copia de seguridad a petición](./media/backup-during-vm-creation/run-backup.png)

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Uso de una plantilla de Resource Manager para implementar una VM protegida

En los pasos anteriores se explica cómo usar Azure Portal para crear una máquina virtual y protegerla en un almacén de Recovery Services. Para implementar una o varias VM rápidamente y protegerlas en un almacén de Azure Recovery Services, consulte la plantilla [Deploy a Windows VM and enable backup](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/) (Implementar una VM Windows y habilitar la copia de seguridad).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha protegido su VM, obtenga información sobre cómo administrarla y restaurarla.

- [Administración y supervisión de máquinas virtuales](backup-azure-manage-vms.md)
- [Restauración de máquinas virtuales](backup-azure-arm-restore-vms.md)

Si tiene algún problema, [revise](backup-azure-vms-troubleshoot.md) la guía de solución de problemas.
