---
title: Habilitación de la copia de seguridad de máquinas virtuales de Azure durante el proceso de creación
description: Cómo habilitar la copia de seguridad de máquina virtual de Azure durante el proceso de creación.
services: backup, virtual-machines
author: rayne-wiselman
manager: carmonm
tags: azure-resource-manager, virtual-machine-backup
ms.service: backup
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: trinadhk
ms.openlocfilehash: fd2beaa39f03d4f2342c94bf1cd8b8aea7440e62
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57780450"
---
# <a name="enable-backup-when-you-create-an-azure-virtual-machine"></a>Habilitar copia de seguridad cuando se crea una máquina virtual de Azure

Use el servicio Azure Backup para realizar copias de seguridad de máquinas virtuales (VM). Las máquinas virtuales son una copia de seguridad según la programación especificada en una directiva de copia de seguridad y puntos de recuperación se crean a partir de copias de seguridad. Puntos de recuperación se almacenan en almacenes de Recovery Services.

Este artículo describe cómo habilitar la copia de seguridad al crear una máquina virtual (VM) en el portal de Azure.  

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Si aún no se ha registrado en su cuenta, inicie sesión en el [portal Azure](https://portal.azure.com).
 
## <a name="create-a-vm-with-backup-configured"></a>Crear una máquina virtual con la copia de seguridad configurada 

1. En la esquina superior izquierda de Azure portal, seleccione **New**.

1. Seleccione **proceso**y, a continuación, seleccione una imagen de la máquina virtual.

1. Escriba la información de la máquina virtual. El nombre de usuario y la contraseña que proporcione se utilizará para iniciar sesión en la máquina virtual. Cuando haya terminado, seleccione **Aceptar**. 

1. Seleccione un tamaño para la máquina virtual.  

1. En **configuración** > **copia de seguridad**, seleccione **habilitado** para abrir las opciones de configuración de copia de seguridad.

   - Para aceptar los valores predeterminados, seleccione **Aceptar** en el **configuración** página. A continuación, se le dirigirá a la **resumen** página para crear la máquina virtual. Omita los pasos 6 a 8.
   - Para cambiar los valores de configuración de copia de seguridad, siga los pasos siguientes.  

1. Cree o seleccione un almacén de Recovery Services para almacenar las copias de seguridad de la máquina virtual. Si va a crear un almacén de Recovery Services, puede elegir un grupo de recursos para el almacén.  

    ![Opciones de configuración de copia de seguridad en la página de creación de la máquina virtual](./media/backup-during-vm-creation/create-vm-backup-config.png) 

    > [!NOTE] 
    > El grupo de recursos para el almacén de Recovery Services puede ser diferente que el grupo de recursos para la máquina virtual.  

1. De forma predeterminada, se selecciona una directiva de copia de seguridad para que proteger la máquina virtual. Una directiva de copia de seguridad especifica la frecuencia con que tomar instantáneas de copia de seguridad y cuánto tiempo desea conservar esas copias de seguridad. 

   - Puede aceptar la directiva predeterminada, o bien crear o seleccionar una directiva de copia de seguridad diferente. 
   - Para editar la directiva de copia de seguridad, seleccione **directiva de copia de seguridad** y cambiar los valores.  

1. Cuando haya terminado de establecer los valores de configuración de copia de seguridad, seleccione **Aceptar** en el **configuración** página.  

1. En el **resumen** página después de que haya pasado la validación, seleccione **crear** para crear una máquina virtual que usa la configuración de copia de seguridad. 

## <a name="start-a-backup-after-creating-the-vm"></a>Iniciar una copia de seguridad después de crear la máquina virtual 

Incluso si ha configurado una directiva de copia de seguridad para la máquina virtual, es una buena práctica para crear una copia de seguridad inicial. 

Una vez finalizada la plantilla de creación de la máquina virtual, vaya a **operaciones** en el menú izquierdo y seleccione **copia de seguridad** para ver los detalles de copia de seguridad de la máquina virtual. Puede usar esta página para:

- Desencadenar una copia de seguridad y a petición.
- Restaurar una máquina virtual completa o todos sus discos.
- Restaurar archivos desde una copia de seguridad de máquina virtual.
- Cambiar la directiva de copia de seguridad asociada a la máquina virtual.  

## <a name="use-a-resource-manager-template-to-deploy-a-protected-vm"></a>Usar una plantilla de Resource Manager para implementar una máquina virtual protegida

Los pasos anteriores explican cómo usar el portal de Azure para crear una máquina virtual y protegerla en un almacén de Recovery Services. Para implementar una o más máquinas virtuales y protegerlos en un almacén de Recovery Services, vea la plantilla [implementar una máquina virtual de Windows y habilitar la copia de seguridad](https://azure.microsoft.com/resources/templates/101-recovery-services-create-vm-and-configure-backup/).

## <a name="frequently-asked-questions"></a>Preguntas más frecuentes 

### <a name="which-vm-images-support-backup-configuration-during-vm-creation"></a>¿Qué imágenes de máquina virtual compatible con la configuración de copia de seguridad durante la creación de máquinas virtuales?

Se admiten las siguientes imágenes de núcleo publicadas por Microsoft para habilitar la copia de seguridad durante la creación de máquinas virtuales. Para otras máquinas virtuales, puede habilitar la copia de seguridad una vez creada la máquina virtual. Para obtener más información, consulte [habilitar copia de seguridad una vez creada la máquina virtual](quick-backup-vm-portal.md).

- **Windows** -Windows Server 2016 Datacenter, Windows Server 2016 Datacenter Core, Windows Server 2012 Datacenter, Windows Server 2012 R2 Datacenter, Windows Server 2008 R2 SP1 
- **Ubuntu** -servidor Ubuntu con 17.10, servidor de Ubuntu 17.04, Ubuntu Server 16.04 (LTS), Ubuntu Server 14.04 (LTS) 
- **RedHat**: RHEL 6.7, 6.8, 6.9, 7.2, 7.3, 7.4 
- **SUSE**: SUSE Linux Enterprise Server 11 SP4, 12 SP2 y 12 SP3 
- **Debian**: Debian 8 y Debian 9 
- **CentOS**: CentOS 6.9, CentOS 7.3 
- **Oracle Linux**: Oracle Linux 6.7, 6.8, 6.9, 7.2 y 7.3 
 
### <a name="is-the-backup-cost-included-in-the-vm-cost"></a>¿Es el costo de copia de seguridad incluido en el costo de máquina virtual? 

 No. Los costos de copia de seguridad son independientes de los costos de la máquina virtual. Para obtener más información sobre los precios de copia de seguridad, consulte [precios de Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
 
### <a name="which-permissions-are-required-to-enable-backup-on-a-vm"></a>¿Qué permisos son necesarios para habilitar la copia de seguridad en una máquina virtual? 

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

## <a name="next-steps"></a>Pasos siguientes 

Ahora que ha protegido su máquina virtual, consulte los artículos siguientes para obtener información sobre cómo administrar y restaurar máquinas virtuales:

- [Administración y supervisión de las máquinas virtuales](backup-azure-manage-vms.md) 
- [Restauración de máquinas virtuales](backup-azure-arm-restore-vms.md) 
