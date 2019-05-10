---
title: 'Ejemplo: Auditar si las aplicaciones no están instaladas en máquinas virtuales Linux'
description: Este ejemplo de iniciativa y definiciones de configuración de invitado de directiva audita si las aplicaciones especificadas no están instaladas dentro de máquinas virtuales Linux.
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 05/02/2019
ms.author: dacoulte
ms.openlocfilehash: eda5a2a6d2dae58f8da72deccbb89a34c7f21dae
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204008"
---
# <a name="sample---audit-if-specified-applications-arent-installed-inside-linux-vms"></a>Ejemplo: Auditar si las aplicaciones especificadas no están instaladas en máquinas virtuales Linux.

Esta iniciativa de configuración de invitado de directiva crea un evento de auditoría si las aplicaciones especificadas no están instaladas dentro de máquinas virtuales Linux. El identificador de esta iniciativa integrada es `/providers/Microsoft.Authorization/policySetDefinitions/c937dcb4-4398-4b39-8d63-4a6be432252e`.

> [!IMPORTANT]
> Todas las iniciativas de configuración de invitado se componen de las definiciones de directiva **audit** y **deployIfNotExists**. La asignación de solo una de las definiciones de directiva hace que la configuración de invitado no funcione.

Puede asignar este ejemplo mediante:

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="components-of-the-initiative"></a>Componentes de la iniciativa

Esta iniciativa de [configuración de invitado](../concepts/guest-configuration.md) se compone de las siguientes directivas:

- [audit](#audit-definition): auditar si las aplicaciones no están instaladas en máquinas virtuales Linux.
  - Identificador: `/providers/Microsoft.Authorization/policyDefinitions/fee5cb2b-9d9b-410e-afe3-2902d90d0004`
- [deployIfNotExists](#deployIfNotExists-definition): implementar una extensión de máquina virtual para auditar si las aplicaciones no están instaladas dentro de máquinas virtuales Linux.
  - Identificador: `/providers/Microsoft.Authorization/policyDefinitions/4d1c04de-2172-403f-901b-90608c35c721`

### <a name="initiative-definition"></a>Definición de iniciativa

Para crear la iniciativa se combinan las definiciones **audit** y **deployIfNotExists** y los [parámetros de iniciativa](#initiative-parameters). Este es el código JSON de la definición.

[!code-json[initiative-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/azurepolicyset.json "Initiative definition (JSON)")]

### <a name="initiative-parameters"></a>Parámetros de iniciativa

|NOMBRE |Type |DESCRIPCIÓN |
|---|---|---|
|applicationName |string |Nombres de aplicación. Por ejemplo: "python", "powershell" o una lista de valores separados por coma, como "python,powershell". Use \* para la búsqueda de coincidencia con comodines, como "power\*". |

Al crear una asignación a través de PowerShell o la CLI de Azure, los valores de los parámetros se pueden pasar como JSON en una cadena o a través de un archivo mediante `-PolicyParameter` (PowerShell) o `--params` (la CLI de Azure).
PowerShell también admite `-PolicyParameterObject`, que requiere que se pase al cmdlet una tabla de hash de nombre y valor donde **Nombre** es el nombre del parámetro y **Valor** es un valor único o una matriz de valores que se pasa durante la asignación.

En este parámetro de ejemplo, se audita la instalación de aplicaciones _python_ y _powershell_.

```json
{
    "applicationName": {
        "value": "python,powershell"
    }
}
```

Solo la definición de directiva **deployIfNotExists** hace uso de los parámetros de iniciativa.

### <a name="audit-definition"></a>Definición audit

El archivo JSON que define las reglas de la definición de directiva **audit**.

[!code-json[audit-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/audit/azurepolicy.rules.json "audit policy rules (JSON)")]

### <a name="deployifnotexists-definition"></a>Definición deployIfNotExists

El archivo JSON que define las reglas de la definición de directiva **deployIfNotExists**.

[!code-json[deployIfNotExists-definition](../../../../policy-templates/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.rules.json "deployIfNotExists policy rules (JSON)")]

La definición de directiva **deployIfNotExists** define las imágenes de Azure en las que se ha validado la directiva:

|Publicador |Oferta |SKU |
|-|-|-|
|OpenLogic |CentOS\* |Todos excepto 6\* |
|Redhat |RHEL |Todos excepto 6\* |
|Redhat |osa | Todo |
|credativ |Debian | Todos excepto 7\* |
|Suse |SLES\* |Todos excepto 11\* |
|Canonical| UbuntuServer |Todos excepto 12\* |
|microsoft-dsvm |linux-data-science-vm-ubuntu |Todo |
|microsoft-dsvm |azureml |Todo |
|cloudera |cloudera-centos-os |Todos excepto 6\* |
|cloudera |cloudera-altus-centos-os |Todo |
|microsoft-ads |linux\* |Todo |
|microsoft-aks |Todo |Todo |
|AzureDatabricks |Todo |Todo |
|qubole-inc |Todo |Todo |
|datastax |Todo |Todo |
|couchbase |Todo |Todo |
|scalegrid |Todo |Todo |
|checkpoint |Todo |Todo |
|paloaltonetworks |Todo |Todo |

La parte **deployment** de la regla pasa el parámetro _installedApplication_ al agente de configuración de invitado en la máquina virtual. Esta configuración permite que el agente realice las validaciones y notifique el cumplimiento a través de la definición de directiva **audit**.

## <a name="azure-portal"></a>Azure Portal

Una vez que se crean las definiciones **audit** y **deployIfNotExists** en el portal, se recomienda agruparlas en una [iniciativa](../concepts/definition-structure.md#initiatives) para su asignación.

### <a name="create-copy-of-audit-definition"></a>Creación de una copia de la definición audit

[![Implementación del ejemplo de directiva en Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2Faudit%2Fazurepolicy.json)
[![Implementación del ejemplo de directiva en Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2Faudit%2Fazurepolicy.json)

El uso de estos botones para realizar la implementación mediante el portal crea una copia de la definición de directiva **audit**.
Sin la definición de directiva **deployIfNotExists** emparejada, la configuración de invitado no funcionará correctamente.

### <a name="create-copy-of-deployifnotexists-definition"></a>Creación de una copia de la definición deployIfNotExists

[![Implementación del ejemplo de directiva en Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2FdeployIfNotExists%2Fazurepolicy.json)
[![Implementación del ejemplo de directiva en Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FGuestConfiguration%2Finstalled-application-linux%2FdeployIfNotExists%2Fazurepolicy.json)

El uso de estos botones para realizar la implementación mediante el portal crea una copia de la definición de directiva **deployIfNotExists**. Sin la definición de directiva **audit** emparejada, la configuración de invitado no funcionará correctamente.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Implementación con Azure PowerShell

#### <a name="copy-and-assign-the-initiative"></a>Copia y asignación de la iniciativa

Estos pasos crean una copia de la iniciativa que incluye las directivas integradas de **audit** y **deployIfNotExists** y asignan la iniciativa a un grupo de recursos.

```azurepowershell-interactive
# Create the policy initiative (Subscription scope)
$initDef = New-AzPolicySetDefinition -Name 'guestconfig-installed-application-linux' -DisplayName 'GuestConfig - Audit that an application is installed inside Linux VMs' -description 'This initiative will both deploy the policy requirements and audit that the specified application is installed inside Linux virtual machines.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/azurepolicyset.definitions.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/azurepolicyset.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the initiative parameter (JSON format)
$initParam = '{ "applicationName": { "value": "python,powershell" } }'

# Create the initiative assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-assignment' -DisplayName 'GuestConfig - Python and PowerShell apps on Linux' -Scope $scope.ResourceID -PolicySetDefinition $initDef -PolicyParameter $initParam -AssignIdentity -Location 'westus2'

# Get the system-assigned managed identity created by the assignment with -AssignIdentity
$saIdentity = $assignment.Identity.principalId

# Give the system-assigned managed identity the 'Contributor' role on the scope (needed by deployIfNotExists)
$roleAssignment = New-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'
```

Ejecute los siguientes comandos para eliminar la asignación y la definición anteriores:

```azurepowershell-interactive
# Remove the initiative assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the 'Contributor' role from the system-assigned managed identity
Remove-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'

# Remove the initiative definition
Remove-AzPolicySetDefinition -Id $initDef
```

#### <a name="copy-and-assign-the-audit-definition"></a>Copia y asignación de la definición audit

Estos pasos crean una copia de la definición **audit** y la asignan a un grupo de recursos. Esta definición no funcionará correctamente si no se asigna también la definición **deployIfNotExists** emparejada.

```azurepowershell-interactive
# Create the policy definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'guestconfig-installed-application-linux-audit' -DisplayName 'GuestConfig - Audit that an application is installed inside Linux VMs' -description 'This policy audits that the specified application is installed inside Linux virtual machines. This policy should only be used along with its corresponding deploy policy in an initiative/policy set.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/audit/azurepolicy.rules.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the policy assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-audit-assignment' -DisplayName 'GuestConfig - Python and PowerShell apps on Linux' -Scope $scope.ResourceID -PolicyDefinition $definition
```

Ejecute los siguientes comandos para eliminar la asignación y la definición anteriores:

```azurepowershell-interactive
# Remove the policy definition
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the policy definition
Remove-AzPolicyDefinition -Id $definition
```

#### <a name="copy-and-assign-the-deployifnotexists-definition"></a>Copia y asignación de la definición deployIfNotExists

Estos pasos crean una copia de la definición **deployIfNotExists** y la asignan a un grupo de recursos.
Esta definición no funcionará correctamente si no se asigna también la definición **audit** emparejada.

```azurepowershell-interactive
# Create the policy definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'guestconfig-installed-application-linux-deployIfNotExists' -DisplayName 'GuestConfig - Deploy VM extension to audit that an application is installed inside Linux VMs' -description 'Include this rule to deploy the VM extension for Microsoft Guest Configuration, the VM extension for Microsoft Azure Managed Service Identity, and the content required to audit that an application is installed inside Linux virtual machines. This policy should only be used along with its corresponding audit policy in an initiative/policy set.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/GuestConfiguration/installed-application-linux/deployIfNotExists/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the definition parameter (JSON format)
$policyParam  = '{ "applicationName": { "value": "python,powershell" } }'

# Create the policy assignment
$assignment = New-AzPolicyAssignment -Name 'guestconfig-installed-application-linux-deployIfNotExists-assignment' -DisplayName 'GuestConfig - Deploy VM extension to audit that Python and PowerShell are installed inside Linux VMs' -Scope $scope.ResourceID -PolicyDefinition $definition -PolicyParameter $policyParam -AssignIdentity -Location 'westus2'

# Get the system-assigned managed identity created by the assignment with -AssignIdentity
$saIdentity = $assignment.Identity.principalId

# Give the system-assigned managed identity the 'Contributor' role on the scope (needed by deployIfNotExists)
$roleAssignment = New-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'
```

Ejecute los siguientes comandos para eliminar la asignación y la definición anteriores:

```azurepowershell-interactive
# Remove the policy assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the 'Contributor' role from the system-assigned managed identity
Remove-AzRoleAssignment -ObjectId $saIdentity -Scope $scope.ResourceId -RoleDefinitionName 'Contributor'

# Remove the policy definition
Remove-AzPolicyDefinition -Id $definition
```

### <a name="azure-powershell-explanation"></a>Explicación de Azure PowerShell

Los scripts de implementación y eliminación usan los siguientes comandos. Cada comando de la tabla siguiente crea un vínculo a documentación específica del comando:

| Get-Help | Notas |
|---|---|
| [New-AzPolicySetDefinition](/powershell/module/az.resources/New-AzPolicySetDefinition) | Crea una iniciativa de Azure Policy. |
| [New-AzPolicyDefinition](/powershell/module/az.resources/New-AzPolicyDefinition) | Crea una definición de Azure Policy. |
| [Get-AzResourceGroup](/powershell/module/az.resources/Get-AzResourceGroup) | Obtiene un único grupo de recursos. |
| [New-AzPolicyAssignment](/powershell/module/az.resources/New-AzPolicyAssignment) | Crea una asignación de Azure Policy de una iniciativa o definición. |
| [New-AzRoleAssignment](/powershell/module/az.resources/New-AzRoleAssignment) | Proporciona una asignación de roles existentes a la entidad de seguridad específica. |
| [Remove-AzPolicyAssignment](/powershell/module/az.resources/Remove-AzPolicyAssignment) | Elimina una asignación de directiva de Azure existente. |
| [Remove-AzPolicySetDefinition](/powershell/module/az.resources/Remove-AzPolicySetDefinition) | Quita una iniciativa. |
| [Remove-AzPolicyDefinition](/powershell/module/az.resources/Remove-AzPolicyDefinition) | Quita una definición. |

## <a name="next-steps"></a>Pasos siguientes

- Consulte otros [ejemplos de Azure Policy](index.md).
- Más información sobre la [configuración de invitado de Azure Policy](../concepts/guest-configuration.md).
- Revise la [estructura de definición de Azure Policy](../concepts/definition-structure.md).
