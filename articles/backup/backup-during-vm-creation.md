---
title: Habilitar copia de seguridad cuando se crea una máquina virtual de Azure con Azure Backup
description: Describe cómo habilitar la copia de seguridad cuando se crea una máquina virtual de Azure con Azure Backup.
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: raynew
ms.openlocfilehash: d96b898c8f72abd7e4eb3522ae046e9fc926f387
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403585"
---
# <a name="enable-backup-when-you-create-an-azure-vm"></a>Habilitar copia de seguridad al crear una máquina virtual de Azure

Use el servicio Azure Backup para realizar copias de seguridad de máquinas virtuales (VM). Las máquinas virtuales son una copia de seguridad según la programación especificada en una directiva de copia de seguridad y puntos de recuperación se crean a partir de copias de seguridad. Puntos de recuperación se almacenan en almacenes de Recovery Services.

Este artículo describe cómo habilitar la copia de seguridad al crear una máquina virtual (VM) en el portal de Azure.  

## <a name="before-you-start"></a>Antes de empezar

- [Comprobar](backup-support-matrix-iaas.md#supported-backup-actions) qué sistemas operativos se admiten si habilita la copia de seguridad cuando se crea una máquina virtual.

## <a name="sign-in-to-azure"></a>Iniciar sesión en Azure

Si aún no se ha registrado en su cuenta, inicie sesión en el [portal Azure](https://portal.azure.com).
 
## <a name="create-a-vm-with-backup-configured"></a>Crear una máquina virtual con la copia de seguridad configurada

1. En Azure portal, haga clic en **crear un recurso**.

2. En Azure Marketplace, haga clic en **proceso**y, a continuación, seleccione una imagen de máquina virtual.

3. Configurar la máquina virtual de acuerdo con la [Windows](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal) o [Linux](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) instrucciones.

4. En el **administración** ficha **habilitar copia de seguridad**, haga clic en **en**.
5. Copias de seguridad de copia de seguridad de Azure en un almacén de Recovery Services. Haga clic en **crear nuevo** si no tiene un almacén existente.
6. Acepte el nombre del almacén sugerido o especifique el suyo propio.
7. Especifique o cree un grupo de recursos en el que se ubicará el almacén. El almacén del grupo de recursos puede ser diferente del grupo de recursos de máquina virtual.

    ![Habilitar copia de seguridad para una máquina virtual](./media/backup-during-vm-creation/enable-backup.png) 

8. Acepte la directiva de copia de seguridad de forma predeterminada, o modificar la configuración.
    - Una directiva de copia de seguridad especifica la frecuencia con que tomar instantáneas de copia de seguridad de la máquina virtual y cuánto tiempo desea conservar esas copias de seguridad. 
    - La directiva predeterminada realiza una copia de seguridad de la máquina virtual una vez al día.
    - Puede personalizar su propia directiva de copia de seguridad para una máquina virtual de Azure realizar copias de seguridad diarias o semanales.
    - [Obtenga más información](backup-azure-vms-introduction.md#backup-and-restore-considerations) sobre las consideraciones de copia de seguridad de máquinas virtuales de Azure.
    - [Obtenga más información](backup-instant-restore-capability.md) funcionalidad de restauración de la instantánea.

      ![Directiva de copia de seguridad predeterminada](./media/backup-during-vm-creation/daily-policy.png) 


## <a name="start-a-backup-after-creating-the-vm"></a>Iniciar una copia de seguridad después de crear la máquina virtual 

La copia de seguridad de máquina virtual se ejecutará según las directivas de copia de seguridad. Sin embargo, se recomienda que ejecute una copia de seguridad inicial. 

Una vez creada la máquina virtual, realice lo siguiente:

1. En las propiedades de la máquina virtual, haga clic en **copia de seguridad**. El estado de la máquina virtual es inicial copia de seguridad pendiente hasta que se ejecuta la copia de seguridad inicial
2. Haga clic en **copia de seguridad ahora** para ejecutar una copia de seguridad y a petición.

    ![Ejecución de una copia de seguridad a petición](./media/backup-during-vm-creation/run-backup.png) 

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Usar una plantilla de Resource Manager para implementar una máquina virtual protegida

Los pasos anteriores explican cómo usar el portal de Azure para crear una máquina virtual y protegerla en un almacén de Recovery Services. Para implementar una o más máquinas virtuales y protegerlos en un almacén de Recovery Services, vea la plantilla [implementar una máquina virtual de Windows y habilitar la copia de seguridad](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).



## <a name="next-steps"></a>Pasos siguientes 

Ahora que ha protegido su máquina virtual, obtenga información sobre cómo administrar y restaurarlas.

- [Administrar y supervisar las máquinas virtuales](backup-azure-manage-vms.md) 
- [Restauración de máquinas virtuales](backup-azure-arm-restore-vms.md) 

Si tiene algún problema, [revisar](backup-azure-vms-troubleshoot.md) la Guía de solución de problemas.
