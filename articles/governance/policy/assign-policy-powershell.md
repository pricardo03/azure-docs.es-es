---
title: Creación de una directiva para los recursos no compatibles mediante Azure PowerShell
description: Use Azure PowerShell para crear una asignación de Azure Policy para identificar recursos no compatibles.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/11/2019
ms.topic: quickstart
ms.service: azure-policy
ms.openlocfilehash: 7e86436b1b87ab4eec3a971ec19e7dc0ffb106b3
ms.sourcegitcommit: d7689ff43ef1395e61101b718501bab181aca1fa
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2019
ms.locfileid: "71978125"
---
# <a name="quickstart-create-a-policy-assignment-to-identify-non-compliant-resources-using-azure-powershell"></a>Inicio rápido: Creación de una asignación de directiva para identificar recursos no compatibles mediante Azure PowerShell

El primer paso para entender el cumplimiento en Azure es identificar el estado de sus recursos. En esta guía de inicio rápido, se crea una asignación de directiva para identificar máquinas virtuales que no usan discos administrados. Cuando haya finalizado, podrá identificar máquinas virtuales que *no estén conformes*.

El módulo Azure PowerShell se usa para administrar recursos de Azure desde la línea de comandos o en scripts.
En esta guía se explica cómo usar el módulo Az para crear una asignación de directiva.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

- Antes de comenzar, asegúrese de tener instalada la versión más reciente de Azure PowerShell. Consulte [Instalación del módulo de Azure PowerShell](/powershell/azure/install-az-ps) para más información.
- Registre el proveedor de recursos de Azure Policy Insights mediante Azure PowerShell. El registro del proveedor de recursos garantiza que la suscripción funcionará con él. Para registrar un proveedor de recursos, debe tener permiso para registrar la operación del proveedor de recursos. Esta operación está incluida en los roles Colaborador y Propietario. Para registrar el proveedor de recursos, ejecute el siguiente comando:

  ```azurepowershell-interactive
  # Register the resource provider if it's not already registered
  Register-AzResourceProvider -ProviderNamespace 'Microsoft.PolicyInsights'
  ```

  Para más información acerca del registro y la visualización de los proveedores de recursos, consulte [Tipos y proveedores de recursos](../../azure-resource-manager/resource-manager-supported-services.md)

## <a name="create-a-policy-assignment"></a>Creación de una asignación de directiva

En esta guía de inicio rápido, se crea una asignación de directiva para la definición de *Auditoría de máquinas virtuales sin discos administrados*. Esta definición de directiva identifica las máquinas virtuales que no utilizan discos administrados.

Ejecute los comandos siguientes para crear una nueva asignación de directiva:

```azurepowershell-interactive
# Get a reference to the resource group that will be the scope of the assignment
$rg = Get-AzResourceGroup -Name '<resourceGroupName>'

# Get a reference to the built-in policy definition that will be assigned
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit VMs that do not use managed disks' }

# Create the policy assignment with the built-in definition against your resource group
New-AzPolicyAssignment -Name 'audit-vm-manageddisks' -DisplayName 'Audit VMs without managed disks Assignment' -Scope $rg.ResourceId -PolicyDefinition $definition
```

Los comandos anteriores usan la siguiente información:

- **Nombre**: el nombre real de la asignación. En este ejemplo se usa *audit-vm-manageddisks*.
- **DisplayName**: nombre para mostrar de la asignación de directiva. En este caso, usará *Auditoría de máquinas virtuales sin discos administrados*.
- **Definition**: definición de la directiva, según la opción utilizada para crear la asignación. En este caso, es el identificador de la definición de directiva *Auditoría de máquinas virtuales que no usan discos administrados*.
- **Scope**: un ámbito determina en qué recursos o agrupación de recursos se aplica la asignación de directiva. Puede abarcar desde una suscripción hasta grupos de recursos. Asegúrese de sustituir &lt;scope&gt; por el nombre del grupo de recursos.

Ahora ya está listo para identificar los recursos no compatibles a fin de conocer el estado de cumplimiento de su entorno.

## <a name="identify-non-compliant-resources"></a>Identificación de recursos no compatibles

Utilice la siguiente información para identificar los recursos que no son compatibles con la asignación de directiva que ha creado. Ejecute los comandos siguientes:

```azurepowershell-interactive
# Get the resources in your resource group that are non-compliant to the policy assignment
Get-AzPolicyState -ResourceGroupName $rg.ResourceGroupName -PolicyAssignmentName 'audit-vm-manageddisks' -Filter 'IsCompliant eq false'
```

Para obtener más información sobre cómo obtener el estado de la directiva, consulte [Get-AzPolicyState](/powershell/module/az.policyinsights/Get-AzPolicyState).

Los resultados deben tener una apariencia similar al ejemplo siguiente:

```output
Timestamp                   : 3/9/19 9:21:29 PM
ResourceId                  : /subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachines/{vmId}
PolicyAssignmentId          : /subscriptions/{subscriptionId}/providers/microsoft.authorization/policyassignments/audit-vm-manageddisks
PolicyDefinitionId          : /providers/Microsoft.Authorization/policyDefinitions/06a78e20-9358-41c9-923c-fb736d382a4d
IsCompliant                 : False
SubscriptionId              : {subscriptionId}
ResourceType                : /Microsoft.Compute/virtualMachines
ResourceTags                : tbd
PolicyAssignmentName        : audit-vm-manageddisks
PolicyAssignmentOwner       : tbd
PolicyAssignmentScope       : /subscriptions/{subscriptionId}
PolicyDefinitionName        : 06a78e20-9358-41c9-923c-fb736d382a4d
PolicyDefinitionAction      : audit
PolicyDefinitionCategory    : Compute
ManagementGroupIds          : {managementGroupId}
```

Los resultados coinciden con lo que ve en la pestaña de **cumplimiento de recursos** de una asignación de directiva en la vista de Azure Portal.

## <a name="clean-up-resources"></a>Limpieza de recursos

Para quitar la asignación creada, ejecute el siguiente comando:

```azurepowershell-interactive
# Removes the policy assignment
Remove-AzPolicyAssignment -Name 'audit-vm-manageddisks' -Scope '/subscriptions/<subscriptionID>/resourceGroups/<resourceGroupName>'
```

## <a name="next-steps"></a>Pasos siguientes

En este inicio rápido, se asigna una definición de directiva para identificar los recursos incompatibles en el entorno de Azure.

Para más información sobre la asignación de directivas para garantizar la compatibilidad de los nuevos recursos, continúe con el tutorial para:

> [!div class="nextstepaction"]
> [Crear y administrar directivas](./tutorials/create-and-manage.md)