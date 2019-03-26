---
title: Administrar recursos de Azure mediante Azure PowerShell | Microsoft Docs
description: Use Azure PowerShell y Azure Resource Manager para administrar los recursos.
services: azure-resource-manager
documentationcenter: ''
author: mumian
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/11/2019
ms.author: jgao
ms.openlocfilehash: 8e6c4047182901c5282f280f59fa95eca6571ecc
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58417945"
---
# <a name="manage-azure-resources-by-using-azure-powershell"></a>Administrar recursos de Azure mediante Azure PowerShell

Aprenda a usar Azure PowerShell con [Azure Resource Manager](resource-group-overview.md) para administrar los recursos de Azure. Para administrar grupos de recursos, consulte [grupos de recursos de administración de Azure mediante Azure PowerShell](./manage-resource-groups-powershell.md).

Otros artículos sobre la administración de recursos:

- [Administrar recursos de Azure mediante el portal de Azure](./manage-resources-portal.md)
- [Administrar recursos de Azure mediante la CLI de Azure](./manage-resources-cli.md)

## <a name="deploy-resources-to-an-existing-resource-group"></a>Implementar recursos en un grupo de recursos

Puede implementar los recursos de Azure mediante Azure PowerShell, o implementar una plantilla de Resource Manager para crear recursos de Azure.

### <a name="deploy-a-resource"></a>Implementar un recurso

El script siguiente crea una cuenta de almacenamiento.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

# Create the storage account.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroupName `
  -Name $storageAccountName `
  -Location $location `
  -SkuName "Standard_LRS"

# Retrieve the context.
$ctx = $storageAccount.Context
```

### <a name="deploy-a-template"></a>Implementación de una plantilla

Crea el siguiente script de implementación de una plantilla de inicio rápido para crear una cuenta de almacenamiento. Para más información, consulte [Inicio rápido: Creación de plantillas de Azure Resource Manager mediante Visual Studio Code](./resource-manager-quickstart-create-templates-use-visual-studio-code.md?tabs=PowerShell).

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-storage-account-create/azuredeploy.json"
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -Location $location
```

Para más información, consulte [Implementación de recursos con plantillas de Resource Manager y Azure PowerShell](./resource-group-template-deploy.md).

## <a name="deploy-a-resource-group-and-resources"></a>Implementar un grupo de recursos y recursos

Puede crear un grupo de recursos e implementar recursos en el grupo. Para más información, consulte [Creación de un grupo de recursos e implementación de recursos](./deploy-to-subscription.md#create-resource-group-and-deploy-resources).

## <a name="deploy-resources-to-multiple-subscriptions-or-resource-groups"></a>Implementar recursos en varias suscripciones o grupos de recursos

Por lo general, todos los recursos de la plantilla se implementan en un único grupo de recursos. Sin embargo, existen escenarios en los que desea implementar un conjunto de recursos juntos pero colocarlos en distintos grupos de recursos o suscripciones. Para obtener más información, consulte [recursos de Azure implementar a varias suscripciones o grupos de recursos](./resource-manager-cross-resource-group-deployment.md).

## <a name="delete-resources"></a>Eliminar recursos

El siguiente script muestra cómo eliminar una cuenta de almacenamiento.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

Remove-AzStorageAccount -ResourceGroupName $resourceGroupName -AccountName $storageAccountName
```

Para obtener más información acerca de cómo Azure Resource Manager ordena la eliminación de recursos, consulte [eliminación del grupo de recursos de Azure Resource Manager](./resource-group-delete.md).

## <a name="move-resources"></a>Traslado de recursos

El siguiente script muestra cómo quitar una cuenta de almacenamiento de un grupo de recursos a otro grupo de recursos.

```azurepowershell-interactive
$srcResourceGroupName = Read-Host -Prompt "Enter the source Resource Group name"
$destResourceGroupName = Read-Host -Prompt "Enter the destination Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

$storageAccount = Get-AzResource -ResourceGroupName $srcResourceGroupName -ResourceName $storageAccountName
Move-AzResource -DestinationResourceGroupName $destResourceGroupName -ResourceId $storageAccount.ResourceId
```

Para realizar un tutorial, consulte [Tutorial: Traslado de recursos de Azure a otro grupo de recursos o a otra suscripción](./resource-manager-tutorial-move-resources.md). 

Para obtener más información, consulte [Traslado de los recursos a un nuevo grupo de recursos o a una nueva suscripción](resource-group-move-resources.md).

## <a name="lock-resources"></a>Bloqueo de recursos

El bloqueo impide que otros usuarios de su organización eliminen o modifiquen los recursos críticos, como la suscripción de Azure, el grupo de recursos o recurso accidentalmente. 

La siguiente secuencia de comandos bloquea una cuenta de almacenamiento, por lo que no se puede eliminar la cuenta.

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

New-AzResourceLock -LockName LockStorage -LockLevel CanNotDelete -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts 
```

El siguiente script obtiene todos los bloqueos de una cuenta de almacenamiento:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

Get-AzResourceLock -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts
```

El siguiente script elimina un bloqueo de una cuenta de almacenamiento:

```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"

$lockId = (Get-AzResourceLock -ResourceGroupName $resourceGroupName -ResourceName $storageAccountName -ResourceType Microsoft.Storage/storageAccounts).LockId
Remove-AzResourceLock -LockId $lockId
```

Para obtener más información, consulte [Bloqueo de recursos con el Administrador de recursos de Azure](resource-group-lock-resources.md).

## <a name="tag-resources"></a>Etiquetado de recursos

El etiquetado ayuda a organizar lógicamente de su grupo de recursos. Para obtener información, consulte [mediante etiquetas para organizar los recursos de Azure](./resource-group-using-tags.md#powershell).

## <a name="manage-access-to-resources"></a>Administrar el acceso a los recursos

El [control de acceso basado en rol (RBAC)](../role-based-access-control/overview.md) es la forma en que se administra el acceso a los recursos en Azure. Para obtener más información, consulte [administrar el acceso mediante RBAC y Azure PowerShell](../role-based-access-control/role-assignments-powershell.md).

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre Azure Resource Manager, consulte [Introducción a Azure Resource Manager](./resource-group-overview.md).
- Para obtener información sobre la sintaxis de la plantilla de Resource Manager, consulte [entender la estructura y sintaxis de plantillas de Azure Resource Manager](./resource-group-authoring-templates.md).
- Para obtener información sobre cómo desarrollar las plantillas, consulte el [tutoriales paso a paso](/azure/azure-resource-manager/).
- Para ver los esquemas de plantilla de Azure Resource Manager, consulte [referencia de plantilla](/azure/templates/).
