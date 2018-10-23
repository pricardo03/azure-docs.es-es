---
title: 'Tutorial: Control de máquinas virtuales de Azure con Azure PowerShell | Microsoft Docs'
description: En este tutorial, aprenderá a usar Azure PowerShell para administrar máquinas virtuales de Azure mediante la aplicación de RBAC, directivas, bloqueos y etiquetas
services: virtual-machines-windows
documentationcenter: virtual-machines
author: tfitzmac
manager: jeconnoc
editor: tysonn
ms.service: virtual-machines-windows
ms.workload: infrastructure
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: tutorial
ms.date: 10/12/2018
ms.author: tomfitz
ms.custom: mvc
ms.openlocfilehash: 6377a54cc862bb5f62726c3ce91a41cc6eb0763d
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/13/2018
ms.locfileid: "49311395"
---
# <a name="tutorial-learn-about-windows-virtual-machine-governance-with-azure-powershell"></a>Tutorial: Información acerca del control de máquinas virtuales Windows con Azure PowerShell

[!INCLUDE [Resource Manager governance introduction](../../../includes/resource-manager-governance-intro.md)]

[!INCLUDE [cloud-shell-powershell.md](../../../includes/cloud-shell-powershell.md)]

Los ejemplos de este artículo requieren la versión 6.0 o posterior de Azure PowerShell. Si ejecuta PowerShell localmente y no tiene la versión 6.0 o una posterior, [actualice la versión](/powershell/azure/install-azurerm-ps). También tiene que ejecutar `Connect-AzureRmAccount` para crear una conexión con Azure. En las instalaciones locales, también debe [descargar el módulo PowerShell de Azure AD](https://www.powershellgallery.com/packages/AzureAD/) para crear un nuevo grupo de Azure Active Directory.

## <a name="understand-scope"></a>Descripción del ámbito

[!INCLUDE [Resource Manager governance scope](../../../includes/resource-manager-governance-scope.md)]

En este tutorial, se aplica toda la configuración de administración a un grupo de recursos para poder quitar fácilmente esas opciones al finalizar.

Vamos a crear el grupo de recursos.

```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

Actualmente, el grupo de recursos está vacío.

## <a name="role-based-access-control"></a>Control de acceso basado en rol

Desea asegurarse de que los usuarios de una organización tengan el nivel adecuado de acceso a estos recursos. No desea conceder acceso ilimitado a los usuarios, pero también es necesario garantizar que puedan realizar su trabajo. [El control de acceso basado en rol](../../role-based-access-control/overview.md) le permite administrar los usuarios con permiso para completar acciones específicas en un ámbito.

Para crear y quitar las asignaciones de rol, los usuarios deben tener acceso a `Microsoft.Authorization/roleAssignments/*`. Esta acción se concede mediante los roles Propietario o Administrador de acceso de usuario.

Para administrar las soluciones de máquina virtual, hay tres roles específicos a los recursos, que normalmente proporcionan el acceso necesario:

* [Colaborador de la máquina virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor)
* [Colaborador de la red](../../role-based-access-control/built-in-roles.md#network-contributor)
* [Colaborador de la cuenta de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-account-contributor)

En lugar de asignar roles a usuarios individuales, a menudo resulta más fácil usar un grupo de Azure Active Directory con usuarios que tienen que realizar acciones similares. A continuación, asigne a ese grupo el rol apropiado. Para este artículo puede usar un grupo existente para administrar la máquina virtual o el portal para [crear un grupo de Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

Después de crear un grupo o de buscar uno existente, use el comando [New-AzureRmRoleAssignment](/powershell/module/azurerm.resources/new-azurermroleassignment) para asignar el grupo de Azure Active Directory al rol Colaborador de la máquina virtual para el grupo de recursos.  

```azurepowershell-interactive
$adgroup = Get-AzureRmADGroup -DisplayName <your-group-name>

New-AzureRmRoleAssignment -ObjectId $adgroup.id `
  -ResourceGroupName myResourceGroup `
  -RoleDefinitionName "Virtual Machine Contributor"
```

Si recibe un error que indica **Principal <guid> does not exist in the directory** (La entidad de seguridad no existe en el directorio), el nuevo grupo no se ha propagado en Azure Active Directory. Intente ejecutar el comando de nuevo.

Por lo general, repetirá el proceso para *Colaborador de la red* y *Colaborador de la cuenta de almacenamiento* con el objetivo de asegurarse de que los usuarios se asignan para administrar los recursos implementados. En este artículo, puede omitir esos pasos.

## <a name="azure-policy"></a>Azure Policy

[Azure Policy](../../azure-policy/azure-policy-introduction.md) ayuda a asegurarse de que todos los recursos de la suscripción satisfacen los estándares corporativos. La suscripción ya tiene varias definiciones de directiva. Para ver las definiciones de directivas disponibles, use el comando [Get-AzureRmPolicyDefinition](/powershell/module/AzureRM.Resources/Get-AzureRmPolicyDefinition):

```azurepowershell-interactive
(Get-AzureRmPolicyDefinition).Properties | Format-Table displayName, policyType
```

Ve las definiciones de directiva existentes. El tipo de directiva es **BuiltIn** o **Custom**. Examine las definiciones para buscar las que describan una condición que quiera asignar. En este artículo, puede asignar directivas que:

* Limite las ubicaciones para todos los recursos.
* Limite las SKU para las máquinas virtuales.
* Realice auditorías de las máquinas virtuales que no usen discos administrados.

En el siguiente ejemplo, se recuperan tres definiciones de directivas en función del nombre para mostrar. Puede usar el comando [New-AzureRMPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) para asignar esas definiciones al grupo de recursos. En algunas directivas, puede proporcionar los valores de los parámetros para especificar los valores permitidos.

```azurepowershell-interactive
# Values to use for parameters
$locations ="eastus", "eastus2"
$skus = "Standard_DS1_v2", "Standard_E2s_v2"

# Get the resource group
$rg = Get-AzureRmResourceGroup -Name myResourceGroup

# Get policy definitions for allowed locations, allowed SKUs, and auditing VMs that don't use managed disks
$locationDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed locations"}
$skuDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Allowed virtual machine SKUs"}
$auditDefinition = Get-AzureRmPolicyDefinition | where-object {$_.properties.displayname -eq "Audit VMs that do not use managed disks"}

# Assign policy for allowed locations
New-AzureRMPolicyAssignment -Name "Set permitted locations" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $locationDefinition `
  -listOfAllowedLocations $locations

# Assign policy for allowed SKUs
New-AzureRMPolicyAssignment -Name "Set permitted VM SKUs" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $skuDefinition `
  -listOfAllowedSKUs $skus

# Assign policy for auditing unmanaged disks
New-AzureRMPolicyAssignment -Name "Audit unmanaged disks" `
  -Scope $rg.ResourceId `
  -PolicyDefinition $auditDefinition
```

## <a name="deploy-the-virtual-machine"></a>Implementación de la máquina virtual

Ha asignado roles y directivas, por lo que está listo para implementar la solución. El tamaño predeterminado es Standard_DS1_v2, que es una de las SKU permitidas. Cuando se realiza este paso, se le solicitan las credenciales. Los valores que especifique se configuran como el nombre de usuario y la contraseña de la máquina virtual.

```azurepowershell-interactive
New-AzureRmVm -ResourceGroupName "myResourceGroup" `
     -Name "myVM" `
     -Location "East US" `
     -VirtualNetworkName "myVnet" `
     -SubnetName "mySubnet" `
     -SecurityGroupName "myNetworkSecurityGroup" `
     -PublicIpAddressName "myPublicIpAddress" `
     -OpenPorts 80,3389
```

Una vez finalizada la implementación, puede aplicar más opciones de configuración de administración a la solución.

## <a name="lock-resources"></a>Bloqueo de recursos

Los [bloqueos de recursos](../../azure-resource-manager/resource-group-lock-resources.md) impiden que los usuarios de una organización eliminen o modifiquen de forma accidental recursos críticos. A diferencia del control de acceso basado en rol, los bloqueos de recursos aplican una restricción a todos los usuarios y roles. Puede establecer el bloqueo de nivel en *CanNotDelete* o *ReadOnly*.

Para bloquear la máquina virtual y el grupo de seguridad de red, use el comando [New-AzureRmResourceLock](/powershell/module/azurerm.resources/new-azurermresourcelock):

```azurepowershell-interactive
# Add CanNotDelete lock to the VM
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup

# Add CanNotDelete lock to the network security group
New-AzureRmResourceLock -LockLevel CanNotDelete `
  -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Para probar los bloqueos, intente ejecutar el siguiente comando:

```azurepowershell-interactive 
Remove-AzureRmResourceGroup -Name myResourceGroup
```

Verá un error en el que se indica que la operación de eliminación no se puede realizar debido a un bloqueo. Solo se puede eliminar el grupo de recursos si quita los bloqueos específicamente. Ese paso se muestra en [Limpieza de recursos](#clean-up-resources).

## <a name="tag-resources"></a>Etiquetado de recursos

Se aplican [etiquetas](../../azure-resource-manager/resource-group-using-tags.md) a los recursos de Azure para organizarlos de forma lógica por categorías. Cada etiqueta consta de un nombre y un valor. Por ejemplo, puede aplicar el nombre "Environment" y el valor "Production" a todos los recursos en producción.

[!INCLUDE [Resource Manager governance tags Powershell](../../../includes/resource-manager-governance-tags-powershell.md)]

Para aplicar etiquetas a una máquina virtual, use el comando [Set-AzureRmResource](/powershell/module/azurerm.resources/set-azurermresource):

```azurepowershell-interactive
# Get the virtual machine
$r = Get-AzureRmResource -ResourceName myVM `
  -ResourceGroupName myResourceGroup `
  -ResourceType Microsoft.Compute/virtualMachines

# Apply tags to the virtual machine
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test"; Project="Documentation" } -ResourceId $r.ResourceId -Force
```

### <a name="find-resources-by-tag"></a>Búsqueda de recursos por etiqueta

Para buscar recursos con un valor y nombre de etiqueta, use el comando [Get-AzureRmResource](/powershell/module/azurerm.resources/get-azurermresource):

```azurepowershell-interactive
(Get-AzureRmResource -Tag @{ Environment="Test"}).Name
```

Puede utilizar los valores devueltos para las tareas de administración, como detener todas las máquinas virtuales con un valor de etiqueta.

```azurepowershell-interactive
Get-AzureRmResource -Tag @{ Environment="Test"} | Where-Object {$_.ResourceType -eq "Microsoft.Compute/virtualMachines"} | Stop-AzureRmVM
```

### <a name="view-costs-by-tag-values"></a>Visualización de los costos por valores de etiqueta

[!INCLUDE [Resource Manager governance tags billing](../../../includes/resource-manager-governance-tags-billing.md)]

## <a name="clean-up-resources"></a>Limpieza de recursos

El grupo de seguridad de red bloqueado no se puede eliminar hasta que se quite el bloqueo. Para quitar el bloqueo, utilice el comando [Remove-AzureRmResourceLock](/powershell/module/azurerm.resources/remove-azurermresourcelock):

```azurepowershell-interactive
Remove-AzureRmResourceLock -LockName LockVM `
  -ResourceName myVM `
  -ResourceType Microsoft.Compute/virtualMachines `
  -ResourceGroupName myResourceGroup
Remove-AzureRmResourceLock -LockName LockNSG `
  -ResourceName myNetworkSecurityGroup `
  -ResourceType Microsoft.Network/networkSecurityGroups `
  -ResourceGroupName myResourceGroup
```

Cuando ya no se necesiten, puede usar el comando [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) para quitar el grupo de recursos, la máquina virtual y todos los recursos relacionados.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha creado una imagen de máquina virtual personalizada. Ha aprendido a:

> [!div class="checklist"]
> * Asignar un rol a los usuarios
> * Aplicar directivas que hagan cumplir los estándares
> * Proteger los recursos críticos con bloqueos
> * Etiquetar recursos para la facturación y la administración

Avance al siguiente tutorial para obtener información sobre máquinas virtuales de alta disponibilidad.

> [!div class="nextstepaction"]
> [Supervisión de máquinas virtuales](tutorial-monitoring.md)

