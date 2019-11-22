---
title: 'Ejemplo: Almacenes de claves sin puntos de conexión de red virtual'
description: Esta definición de directiva de ejemplo audita los almacenes de Key Vault para detectar instancias que no tienen ningún punto de conexión de servicio de red virtual.
ms.date: 01/26/2019
ms.topic: sample
ms.openlocfilehash: 078a781ca96df421d52179f5e8d25650def8a2f3
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74076200"
---
# <a name="sample---key-vault-vaults-with-no-virtual-network-endpoints"></a>Ejemplo: almacenes de Key Vault sin ningún punto de conexión de red virtual

En este ejemplo de cómo [integrar Key Vault con Azure Policy](../../../key-vault/azure-policy.md) se realiza una auditoría de los almacenes de claves que no tienen puntos de conexión de red virtual. Utilice esta opción para aplicar los requisitos de seguridad. Para más información, consulte [Puntos de conexión de servicio de Key Vault](../../../key-vault/key-vault-overview-vnet-service-endpoints.md).

Puede implementar esta directiva de ejemplo mediante:

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [CLI de Azure](#azure-cli)
- [API DE REST](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>Directiva de ejemplo

### <a name="policy-definition"></a>Definición de directiva

Definición de directiva JSON compuesta completa, utilizada por la API REST, botones "Implementar en Azure" y manualmente en el portal.

[!code-json[full](../../../../policy-templates/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.json "KeyVault vnet rules")]

> [!NOTE]
> Si va a crear manualmente una directiva en el portal, use las partes **properties.parameters** y **properties.policyRule** de los pasos anteriores. Encapsule las dos secciones con llaves `{}` para que sea un valor JSON válido.

### <a name="policy-rules"></a>Reglas de directiva

Código JSON que define las reglas de la directiva, utilizado por la CLI de Azure y Azure PowerShell.

[!code-json[rule](../../../../policy-templates/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>Parámetros de directiva

Esta definición de directiva de ejemplo no tiene ningún parámetro definido.

## <a name="azure-portal"></a>Portal de Azure

[![Implementación del ejemplo de directiva en Azure](https://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FKeyVault%2Faudit-keyvault-vnet-rules%2Fazurepolicy.json)
[![Implementación del ejemplo de directiva en Azure Gov](https://docs.microsoft.com/azure/governance/policy/media/deploy/deployGovbutton.png)](https://portal.azure.us/#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FKeyVault%2Faudit-keyvault-vnet-rules%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Implementación con Azure PowerShell

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name "audit-keyvault-vnet-rules" -DisplayName "Audit if Key Vault has no virtual network rules" -description "Audits Key Vault vaults if they do not have virtual network service endpoints set up. More information on virtual network service endpoints in Key Vault is available here: https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints" -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json' -Mode Indexed

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'audit-keyvault-vnet-rules-assignment' -DisplayName 'Audit Key Vault Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition
```

### <a name="remove-with-azure-powershell"></a>Eliminación con Azure PowerShell

Ejecute los siguientes comandos para eliminar la asignación y la definición anteriores:

```azurepowershell-interactive
# Remove the Policy Assignment
Remove-AzPolicyAssignment -Id $assignment.ResourceId

# Remove the Policy Definition
Remove-AzPolicyDefinition -Id $definition.ResourceId
```

### <a name="azure-powershell-explanation"></a>Explicación de Azure PowerShell

Los scripts de implementación y eliminación usan los siguientes comandos. Cada comando de la tabla siguiente crea un vínculo a documentación específica del comando:

| Get-Help | Notas |
|---|---|
| [New-AzPolicyDefinition](/powershell/module/az.resources/New-Azpolicydefinition) | Crea una nueva definición de directiva de Azure. |
| [Get-AzResourceGroup](/powershell/module/az.resources/Get-Azresourcegroup) | Obtiene un único grupo de recursos. |
| [New-AzPolicyAssignment](/powershell/module/az.resources/New-Azpolicyassignment) | Crea una nueva asignación de directiva de Azure. En este ejemplo, se proporciona una definición, pero también puede tomar una iniciativa. |
| [Remove-AzPolicyAssignment](/powershell/module/az.resources/Remove-Azpolicyassignment) | Elimina una asignación de directiva de Azure existente. |
| [Remove-AzPolicyDefinition](/powershell/module/az.resources/Remove-Azpolicydefinition) | Elimina una definición de directiva de Azure existente. |

## <a name="azure-cli"></a>CLI de Azure

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

```azurecli-interactive
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'audit-keyvault-vnet-rules' --display-name 'Audit if Key Vault has no virtual network rules' --description 'Audits Key Vault vaults if they do not have virtual network service endpoints set up. More information on virtual network service endpoints in Key Vault is available here: https://docs.microsoft.com/azure/key-vault/key-vault-overview-vnet-service-endpoints' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/KeyVault/audit-keyvault-vnet-rules/azurepolicy.rules.json' --mode Indexed)

# Set the scope to a resource group; may also be a subscription or management group
scope=$(az group show --name 'YourResourceGroup')

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'audit-keyvault-vnet-rules-assignment' --display-name 'Audit Key Vault Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r`)
```

### <a name="remove-with-azure-cli"></a>Eliminación con la CLI de Azure

Ejecute los siguientes comandos para eliminar la asignación y la definición anteriores:

```azurecli-interactive
# Remove the Policy Assignment
az policy assignment delete --name `echo $assignment | jq '.name' -r`

# Remove the Policy Definition
az policy definition delete --name `echo $definition | jq '.name' -r`
```

### <a name="azure-cli-explanation"></a>Explicación de la CLI de Azure

| Get-Help | Notas |
|---|---|
| [az policy definition create](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-create) | Crea una nueva definición de directiva de Azure. |
| [az group show](/cli/azure/group?view=azure-cli-latest#az-group-show) | Obtiene un único grupo de recursos. |
| [az policy assignment create](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-create) | Crea una nueva asignación de directiva de Azure. En este ejemplo, se proporciona una definición, pero también puede tomar una iniciativa. |
| [az policy assignment delete](/cli/azure/policy/assignment?view=azure-cli-latest#az-policy-assignment-delete) | Elimina una asignación de directiva de Azure existente. |
| [az policy definition delete](/cli/azure/policy/definition?view=azure-cli-latest#az-policy-definition-delete) | Elimina una definición de directiva de Azure existente. |

## <a name="rest-api"></a>API DE REST

Hay varias herramientas que se pueden usar para interactuar con la API REST de Resource Manager, como [ARMClient](https://github.com/projectkudu/ARMClient) o PowerShell.

### <a name="deploy-with-rest-api"></a>Implementación con la API de REST

- Cree la definición de directiva (ámbito de la suscripción). Use el código JSON de [definición de directiva](#policy-definition) para el cuerpo de la solicitud.

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules?api-version=2018-05-01
  ```

- Cree la asignación de directiva (ámbito de grupo de recursos)

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/audit-keyvault-vnet-rules-assignment?api-version=2018-05-01
  ```

  Utilice el siguiente ejemplo JSON para el cuerpo de la solicitud:

  ```json
  {
      "properties": {
          "displayName": "Audit Key Vault Assignment",
          "policyDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules"
      }
  }
  ```

### <a name="remove-with-rest-api"></a>Eliminación con la API REST

- Eliminación de la asignación de directiva

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/audit-keyvault-vnet-rules-assignment?api-version=2018-05-01
  ```

- Eliminación de la definición de directiva

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/audit-keyvault-vnet-rules?api-version=2018-05-01
  ```

### <a name="rest-api-explanation"></a>Explicación de la API REST

| Servicio | Grupo | Operación | Notas |
|---|---|---|---|
| Administración de recursos | Definiciones de directiva | [Creación](/rest/api/resources/policydefinitions/createorupdate) | Crea una nueva definición de directiva de Azure en una suscripción. Alternativa: [Creación de un grupo de administración](/rest/api/resources/policydefinitions/createorupdateatmanagementgroup) |
| Administración de recursos | Asignaciones de directiva | [Creación](/rest/api/resources/policyassignments/create) | Crea una nueva asignación de directiva de Azure. En este ejemplo, se proporciona una definición, pero también puede tomar una iniciativa. |
| Administración de recursos | Asignaciones de directiva | [Eliminar](/rest/api/resources/policyassignments/delete) | Elimina una asignación de directiva de Azure existente. |
| Administración de recursos | Definiciones de directiva | [Eliminar](/rest/api/resources/policydefinitions/delete) | Elimina una definición de directiva de Azure existente. Alternativa: [Eliminación de un grupo de administración](/rest/api/resources/policydefinitions/deleteatmanagementgroup) |

## <a name="next-steps"></a>Pasos siguientes

- Consulte otros [ejemplos de directivas de Azure](index.md)
- Aprenda a [integrar Azure Key Vault con Azure Policy](../../../key-vault/azure-policy.md).
- Consulte la [Estructura de definición de Azure Policy](../concepts/definition-structure.md)