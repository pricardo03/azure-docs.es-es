---
title: Configuración de Managed Service Identity en un conjunto de escalado de máquinas virtuales de Azure mediante PowerShell
description: Instrucciones paso a paso para configurar identidades asignadas por el sistema y por el usuario en un VMSS de Azure, mediante PowerShell.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/27/2017
ms.author: daveba
ms.openlocfilehash: 5d4539c05d05053ac2ea6cd1c5fadbd161b41173
ms.sourcegitcommit: c2c64fc9c24a1f7bd7c6c91be4ba9d64b1543231
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2018
ms.locfileid: "39257749"
---
# <a name="configure-a-vmss-managed-service-identity-using-powershell"></a>Configuración de Managed Service Identity en un conjunto de escalado de máquinas virtuales con PowerShell

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Managed Service Identity proporciona a los servicios de Azure una identidad administrada automáticamente en Azure Active Directory. Puede usar esta identidad para autenticar a cualquier servicio que admita la autenticación de Azure AD, sin necesidad de tener credenciales en el código. 

En este artículo, aprenderá a realizar las operaciones de Managed Service Identity en un conjunto de escalado de máquinas virtuales de Azure mediante PowerShell:
- Habilitación y deshabilitación de la identidad asignada por el sistema en un conjunto de escalado de máquinas virtuales.
- Adición y eliminación de una identidad asignada por el usuario en un conjunto de escalado de máquinas virtuales.

## <a name="prerequisites"></a>Requisitos previos

- Si no está familiarizado con Managed Service Identity, consulte la [sección de introducción](overview.md). **No olvide revisar la [diferencia entre una identidad asignada por el sistema y una asignada por el usuario](overview.md#how-does-it-work)**.
- Si aún no tiene una cuenta de Azure, [regístrese para una cuenta gratuita](https://azure.microsoft.com/free/) antes de continuar.
- Para llevar a cabo las operaciones de administración de este artículo, su cuenta debe tener las siguientes asignaciones de roles:
    - [Colaborador de máquina virtual](/azure/role-based-access-control/built-in-roles#virtual-machine-contributor) para crear un conjunto de escalado de máquinas virtuales y habilitar y quitar la identidad administrada asignada por el usuario o por el sistema desde un conjunto de escalado de máquinas virtuales.
    - Rol [Colaborador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-contributor) para crear una identidad asignada por el usuario.
    - Rol [Operador de identidad administrada](/azure/role-based-access-control/built-in-roles#managed-identity-operator) para asignar y quitar una identidad asignada por el usuario en un conjunto de escalado de máquinas virtuales.
- Instale [la versión más reciente de Azure PowerShell](https://www.powershellgallery.com/packages/AzureRM) si aún no lo ha hecho. 

## <a name="system-assigned-managed-identity"></a>Identidad administrada asignada por el sistema

En esta sección, aprenderá a habilitar y quitar una identidad asignada por el sistema con Azure PowerShell.

### <a name="enable-system-assigned-identity-during-the-creation-of-an-azure-virtual-machine-scale-set"></a>Habilitación de la identidad asignada por el sistema durante la creación de un conjunto de escalado de máquinas virtuales de Azure

Para crear un VMSS con la identidad asignada por el sistema habilitada:

1. Consulte el *ejemplo 1* en el artículo de referencia del cmdlet [New-AzureRmVmssConfig](/powershell/module/azurerm.compute/new-azurermvmssconfig) para crear un VMSS con una identidad asignada por el sistema.  Agregue el parámetro `-IdentityType SystemAssigned` al cmdlet `New-AzureRmVmssConfig`:

    ```powershell
    $VMSS = New-AzureRmVmssConfig -Location $Loc -SkuCapacity 2 -SkuName "Standard_A0" -UpgradePolicyMode "Automatic" -NetworkInterfaceConfiguration $NetCfg -IdentityType SystemAssigned`
    ```

2. (Opcional) Agregue la extensión VMSS de Managed Service Identity mediante los parámetros `-Name` y `-Type` en el cmdlet [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension). Puede pasar "ManagedIdentityExtensionForWindows" o "ManagedIdentityExtensionForLinux", en función del tipo de conjunto de escalado de máquinas virtuales y asignarle el nombre mediante el parámetro `-Name`. El parámetro `-Settings` especifica el puerto utilizado por el punto de conexión del token de OAuth para la adquisición de tokens:

    > [!NOTE]
    > Este paso es opcional, ya que puede usar el punto de conexión de identidad de Azure Instance Metadata Service (IMDS) para recuperar tokens.

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

## <a name="enable-system-assigned-identity-on-an-existing-azure-virtual-machine-scale-set"></a>Habilitar la identidad asignada en un conjunto de escalado de máquinas virtuales de Azure existente

Si necesita habilitar una identidad asignada por el sistema en un conjunto de escalado de máquinas virtuales de Azure existente:

1. Inicie sesión en Azure con `Login-AzureRmAccount`. Use una cuenta asociada a la suscripción de Azure que contenga el conjunto de escalado de máquinas virtuales. Asegúrese también de que la cuenta pertenece a un rol que le conceda permisos de escritura en el conjunto de escalado de máquinas virtuales, como "Colaborador de la máquina virtual":

   ```powershell
   Login-AzureRmAccount
   ```

2. En primer lugar, recupere las propiedades del conjunto de escalado de máquinas virtuales mediante el cmdlet [`Get-AzureRmVmss`](/powershell/module/azurerm.compute/get-azurermvmss). Luego, para habilitar una identidad asignada por el sistema, use el modificador `-IdentityType` en el cmdlet [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss):

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name -myVmss -IdentityType "SystemAssigned"
   ```

3. Agregue la extensión VMSS de Managed Service Identity mediante los parámetros `-Name` y `-Type` en el cmdlet [Add-AzureRmVmssExtension](/powershell/module/azurerm.compute/add-azurermvmssextension). Puede pasar "ManagedIdentityExtensionForWindows" o "ManagedIdentityExtensionForLinux", en función del tipo de conjunto de escalado de máquinas virtuales y asignarle el nombre mediante el parámetro `-Name`. El parámetro `-Settings` especifica el puerto utilizado por el punto de conexión del token de OAuth para la adquisición de tokens:

   ```powershell
   $setting = @{ "port" = 50342 }
   $vmss = Get-AzureRmVmss
   Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "ManagedIdentityExtensionForWindows" -Type "ManagedIdentityExtensionForWindows" -Publisher "Microsoft.ManagedIdentity" -TypeHandlerVersion "1.0" -Setting $settings 
   ```

### <a name="disable-the-system-assigned-identity-from-an-azure-virtual-machine-scale-set"></a>Deshabilitación de la identidad asignada por el sistema de un conjunto de escalado de máquinas virtuales de Azure

Si tiene un conjunto de escalado de máquinas virtuales que no necesita la identidad asignada por el sistema, pero necesita identidades asignadas por el usuario, use el siguiente cmdlet:

1. Inicie sesión en Azure con `Login-AzureRmAccount`. Use una cuenta asociada a la suscripción de Azure que contenga la máquina virtual. Asegúrese también de que la cuenta pertenece a un rol que le conceda permisos de escritura en el conjunto de escalado de máquinas virtuales, como "Colaborador de la máquina virtual":

2. Ejecute el siguiente cmdlet:

   ```powershell
   Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "UserAssigned"
   ```

Si tiene un conjunto de escalado de máquinas virtuales que no necesita una identidad asignada por el sistema y no tiene identidades asignadas por el usuario, use los siguientes comandos:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```

## <a name="user-assigned-identity"></a>Identidad asignada por el usuario

En esta sección, aprenderá a agregar y quitar una identidad asignada por el usuario de un conjunto de escalado de máquinas virtuales mediante Azure PowerShell.

### <a name="assign-a-user-assigned-identity-during-creation-of-an-azure-virtual-machine-scale-set"></a>Asignación de una identidad asignada por el usuario durante la creación de un conjunto de escalado de máquinas virtuales de Azure

Actualmente, no se puede crear un conjunto de escalado de máquinas virtuales con una identidad asignada por el usuario mediante PowerShell. Consulte la sección siguiente sobre cómo agregar una identidad asignada por el usuario a un conjunto de escalado de máquinas virtuales existente. Compruebe si hay actualizaciones.

### <a name="assign-a-user-identity-to-an-existing-azure-virtual-machine-scale-set"></a>Asignación de una identidad de usuario a un conjunto de escalado de máquinas virtuales de Azure existente

Para asignar una identidad asignada por el usuario a un conjunto de escalado de máquinas virtuales de Azure existente:

1. Inicie sesión en Azure con `Connect-AzureRmAccount`. Use una cuenta asociada a la suscripción de Azure que contenga el conjunto de escalado de máquinas virtuales. Asegúrese también de que la cuenta pertenece a un rol que le conceda permisos de escritura en el conjunto de escalado de máquinas virtuales, como "Colaborador de la máquina virtual":

   ```powershell
   Connect-AzureRmAccount
   ```

2. En primer lugar, recupere las propiedades del conjunto de escalado de máquinas virtuales mediante el cmdlet `Get-AzureRmVM`. A continuación, para asignar una identidad asignada por el usuario al conjunto de escalado de máquinas virtuales, use los modificadores `-IdentityType` y `-IdentityID` en el cmdlet [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss). Reemplace `<VM NAME>`, `<SUBSCRIPTION ID>`, `<RESROURCE GROUP>`, `<USER ASSIGNED ID1>`, `USER ASSIGNED ID2` con sus propios valores.

   [!INCLUDE[ua-character-limit](~/includes/managed-identity-ua-character-limits.md)]

   ```powershell
   Update-AzureRmVmss -ResourceGroupName <RESOURCE GROUP> -Name <VMSS NAME> -IdentityType UserAssigned -IdentityID "<USER ASSIGNED ID1>","<USER ASSIGNED ID2>"
   ```

### <a name="remove-a-user-assigned-managed-identity-from-an-azure-virtual-machine-scale-set"></a>Eliminación de una identidad administrada asignada por el usuario de un conjunto de escalado de máquinas virtuales de Azure

Si el conjunto de escalado de máquinas virtuales tiene varias identidades asignadas por el usuario, puede quitarlas todas, menos la última, utilizando los comandos siguientes. Asegúrese de reemplazar los valores de los parámetros `<RESOURCE GROUP>` y `<VMSS NAME>` con sus propios valores. `<MSI NAME>` es la propiedad de nombre de la identidad asignada por el usuario, que debe permanecer en el conjunto de escalado de máquinas virtuales. Esta información se puede encontrar en la sección de identidad del conjunto de escalado de máquinas virtuales mediante `az vmss show`:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType UserAssigned -IdentityID "<MSI NAME>"
```
Si el conjunto de escalado de máquinas virtuales no tiene una identidad asignada por el sistema y desea quitar de ella todas las identidades asignadas por el usuario, utilice el siguiente comando:

```powershell
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType None
```
Si el conjunto de escalado de máquinas virtuales tiene identidades asignadas tanto por el sistema como por el usuario, puede quitar todas las identidades asignadas por el usuario si cambia para usar solo las asignadas por el sistema.

```powershell 
Update-AzureRmVmss -ResourceGroupName myResourceGroup -Name myVmss -IdentityType "SystemAssigned"
```

## <a name="related-content"></a>Contenido relacionado

- [Información general de Managed Service Identity](overview.md)
- Para ver guías de inicio rápido completas acerca de la creación de máquinas virtuales de Azure, consulte:
  
  - [Creación de una máquina virtual Windows con PowerShell](../../virtual-machines/windows/quick-create-powershell.md) 
  - [Creación de una máquina virtual Linux con PowerShell](../../virtual-machines/linux/quick-create-powershell.md) 

















