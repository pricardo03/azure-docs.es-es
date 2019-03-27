---
title: Configuración de identidades administradas de recursos de Azure en un conjunto de escalado de máquinas virtuales mediante PowerShell
description: Instrucciones paso a paso para configurar identidades administradas asignadas por el sistema y por el usuario en un conjunto de escalado de máquinas virtuales de Azure mediante PowerShell.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4917720af2396b68ccd36cc0410c9acbbba2d9b2
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448583"
---
# <a name="configure-managed-identities-for-azure-resources-on-virtual-machine-scale-sets-using-powershell"></a>Configuración de identidades administradas de recursos de Azure en conjuntos de escalado de máquinas virtuales mediante PowerShell

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Las identidades administradas para los recursos de Azure proporcionan a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, mediante PowerShell, aprenderá a realizar las operaciones de identidades administradas de recursos de Azure en un conjunto de escalado de máquinas virtuales:
- Habilitación y deshabilitación de la identidad administrada asignada por el sistema en un conjunto de escalado de máquinas virtuales
- Adición y eliminación de una identidad administrada asignada por el usuario en un conjunto de escalado de máquinas virtuales

[!INCLUDE [az-powershell-update](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con las identidades administradas de los recursos de Azure, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad administrada asignada por el sistema y una asignada por el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para realizar las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de control de acceso basado en rol:

    > [!NOTE]
    > No se requieren asignaciones de roles del directorio de Azure AD.

    - [Colaborador de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para crear un conjunto de escalado de máquinas virtuales y habilitar y quitar la identidad administrada asignada por el usuario o por el sistema desde un conjunto de escalado de máquinas virtuales.
    - Rol [Colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para crear una identidad administrada asignada por el usuario.
    - Rol [Operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para asignar y quitar una identidad administrada asignada por el usuario en un conjunto de escalado de máquinas virtuales.
- Instale [la versión más reciente de Azure PowerShell](/powershell/azure/install-az-ps) si aún no lo ha hecho. 

## <a name="system-assigned-managed-identity"></a>Identidad administrada asignada por el sistema

En esta sección, aprenderá a habilitar y quitar una identidad administrada asignada por el sistema con Azure PowerShell.

### <a name="enable-system-assigned-managed-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Habilitación de la identidad administrada asignada por el sistema durante la creación de un conjunto de escalado de máquinas virtuales de Azure

Para crear un conjunto de escalado de máquinas virtuales con la identidad administrada asignada por el sistema habilitada:

1. Hacer referencia a *Example 1* en el [New AzVmssConfig](/powershell/module/az.compute/new-azvmssconfig) establece el artículo de referencia de cmdlet para crear una escala de máquina virtual con una identidad administrada asignado por el sistema.  Agregue el parámetro `-IdentityType SystemAssigned` al cmdlet `New-AzVmssConfig`:

    ```powershell
    $VMSS = New-AzVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```
> [!NOTE]
> Opcionalmente, puede aprovisionar las identidades administradas de extensión del conjunto de escalado de máquinas virtuales de los recursos de Azure, pero pronto quedará obsoleta. Se recomienda usar el punto de conexión de Azure Instance Metadata identidad para la autenticación. Para obtener más información, consulte [dejar de usar la extensión de máquina virtual y empezar a usar el punto de conexión IMDS de Azure para la autenticación](howto-migrate-vm-extension.md).


## <a name="enable-system-assigned-managed-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Habilitación de la identidad administrada asignada por el sistema en un conjunto de escalado de máquinas virtuales de Azure existente

Si necesita habilitar una identidad administrada asignada por el sistema en un conjunto de escalado de máquinas virtuales de Azure existente:

1. Inicie sesión en Azure con `Connect-AzAccount`. Use una cuenta asociada a la suscripción de Azure que contenga el conjunto de escalado de máquinas virtuales. Asegúrese también de que la cuenta pertenece a un rol que le conceda permisos de escritura en el conjunto de escalado de máquinas virtuales, como "Colaborador de la máquina virtual":

   ```powershell
   Connect-AzAccount
   ```

2. En primer lugar, recupere las propiedades del conjunto de escalado de máquinas virtuales mediante el cmdlet [`Get-AzVmss`](/powershell/module/az.compute/get-azvmss). Luego, para habilitar una identidad administrada asignada por el sistema, use el modificador `-IdentityType` en el cmdlet [Update-AzVmss](/powershell/module/az.compute/update-azvmss):

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```

> [!NOTE]
> Opcionalmente, puede aprovisionar las identidades administradas de extensión del conjunto de escalado de máquinas virtuales de los recursos de Azure, pero pronto quedará obsoleta. Se recomienda usar el punto de conexión de Azure Instance Metadata identidad para la autenticación. Para obtener más información, consulte [migrar desde la extensión de máquina virtual al punto de conexión de IMDS de Azure para la autenticación](howto-migrate-vm-extension.md).

### <a name="disable-the-system-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Deshabilitación de la identidad administrada asignada por el sistema de un conjunto de escalado de máquinas virtuales de Azure

Si tiene un conjunto de escalado de máquinas virtuales que ya no necesita la identidad administrada asignada por el sistema, pero aún necesita identidades administradas asignadas por el usuario, use el siguiente cmdlet:

1. Inicie sesión en Azure con `Connect-AzAccount`. Use una cuenta asociada a la suscripción de Azure que contenga la máquina virtual. Asegúrese también de que la cuenta pertenece a un rol que le conceda permisos de escritura en el conjunto de escalado de máquinas virtuales, como "Colaborador de la máquina virtual":

2. Ejecute el siguiente cmdlet:

   ```powershell
   Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Si tiene un conjunto de escalado de máquinas virtuales que ya no necesita la identidad administrada asignada por el sistema y no tiene identidades administradas asignadas por el usuario, use los siguientes comandos:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-managed-identity"></a>Identidad administrada asignada por el usuario

En esta sección, aprenderá a agregar y quitar una identidad administrada asignada por el usuario de un conjunto de escalado de máquinas virtuales mediante Azure PowerShell.

### <a name="assign-a-user-assigned-managed-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Asignación de una identidad administrada asignada por el usuario durante la creación de un conjunto de escalado de máquinas virtuales de Azure

Actualmente, no se puede crear un conjunto de escalado de máquinas virtuales con una identidad administrada asignada por el usuario mediante PowerShell. Consulte la sección siguiente sobre cómo agregar una identidad administrada asignada por el usuario a un conjunto de escalado de máquinas virtuales existente. Compruebe si hay actualizaciones.

### <a name="assign-a-user-assigned-managed-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Asignación de una identidad administrada asignada por el usuario a un conjunto de escalado de máquinas virtuales de Azure existente

Para asignar una identidad administrada asignada por el usuario a un conjunto de escalado de máquinas virtuales de Azure existente:

1. Inicie sesión en Azure con `Connect-AzAccount`. Use una cuenta asociada a la suscripción de Azure que contenga el conjunto de escalado de máquinas virtuales. Asegúrese también de que la cuenta pertenece a un rol que le conceda permisos de escritura en el conjunto de escalado de máquinas virtuales, como "Colaborador de la máquina virtual":

   ```powershell
   Connect-AzAccount
   ```

2. En primer lugar, recupere las propiedades del conjunto de escalado de máquinas virtuales mediante el cmdlet `Get-AzVM`. A continuación, para asignar una identidad administrada asignada por el usuario al conjunto de escalado de máquinas virtuales, use los modificadores `-IdentityType` y `-IdentityID` en el cmdlet [Update-AzVmss](/powershell/module/az.compute/update-azvmss). Reemplace `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` con sus propios valores.

   [!INCLUDE [ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Eliminación de una identidad administrada asignada por el usuario de un conjunto de escalado de máquinas virtuales de Azure

Si el conjunto de escalado de máquinas virtuales tiene varias identidades administradas asignadas por el usuario, puede quitarlas todas, menos la última, utilizando los comandos siguientes. Asegúrese de reemplazar los valores de los parámetros `<RESOURCE GROUP>` y `<VIRTUAL MACHINE SCALE SET NAME>` con sus propios valores. `<USER ASSIGNED IDENTITY NAME>` es la propiedad de nombre de la identidad administrada asignada por el usuario, que debe permanecer en el conjunto de escalado de máquinas virtuales. Esta información se puede encontrar en la sección de identidad del conjunto de escalado de máquinas virtuales mediante `az vmss show`:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<USER ASSIGNED IDENTITY NAME>"
```
Si el conjunto de escalado de máquinas virtuales no tiene una identidad administrada asignada por el sistema y desea quitar de ella todas las identidades administradas asignadas por el usuario, utilice el siguiente comando:

```powershell
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Si el conjunto de escalado de máquinas virtuales tiene identidades administradas asignadas tanto por el sistema como por el usuario, puede quitar todas las identidades administradas asignadas por el usuario si cambia para usar solo las identidades administradas asignadas por el sistema.

```powershell 
Update-AzVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="next-steps"></a>Pasos siguientes

- [Información general sobre las identidades administradas de recursos de Azure](overview.md)
- Para ver guías de inicio rápido completas acerca de la creación de máquinas virtuales de Azure, consulte:
  
  - [Creación de una máquina virtual Windows con PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Creación de una máquina virtual Linux con PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















