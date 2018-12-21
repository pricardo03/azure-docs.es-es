---
title: 'Ejemplo: Aplicar una etiqueta y su valor predeterminado'
description: Esta directiva de ejemplo asocia un nombre de etiqueta especificado y un valor si no se proporciona la etiqueta.
services: azure-policy
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 10/29/2018
ms.author: dacoulte
ms.openlocfilehash: 53ff81275800bde102e33a4a16dc2c8d87646896
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53318877"
---
# <a name="apply-tag-and-its-default-value"></a>Aplicar etiqueta y su valor predeterminado

Esta directiva anexa un nombre y valor de etiqueta especificado, si no se proporciona esa etiqueta. Se especifica el valor y el nombre de la etiqueta que se va a aplicar.

Puede implementar esta directiva de ejemplo mediante:

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [CLI de Azure](#azure-cli)
- [API DE REST](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>Directiva de ejemplo

### <a name="policy-definition"></a>Definición de directiva

Definición de directiva JSON compuesta completa, utilizada por la API REST, botones "Implementar en Azure" y manualmente en el portal.

[!code-json[main](../../../../policy-templates/samples/built-in-policy/apply-default-tag-value/azurepolicy.json "Apply tag and its default value")]

> [!NOTE]
> Si va a crear manualmente una directiva en el portal, use las partes **properties.parameters** y **properties.policyRule** de los pasos anteriores. Encapsule las dos secciones con llaves `{}` para que sea un valor JSON válido.

### <a name="policy-rules"></a>Reglas de directiva

Código JSON que define las reglas de la directiva, utilizado por la CLI de Azure y Azure PowerShell.

[!code-json[rule](../../../../policy-templates/samples/built-in-policy/apply-default-tag-value/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>Parámetros de directiva

Código JSON que define los parámetros de la directiva, utilizado por la CLI de Azure y Azure PowerShell.

[!code-json[parameters](../../../../policy-templates/samples/built-in-policy/apply-default-tag-value/azurepolicy.parameters.json "Policy parameters (JSON)")]

|NOMBRE |Escriba |Campo |DESCRIPCIÓN |
|---|---|---|---|
|tagName |string |etiquetas |Nombre de la etiqueta, como por ejemplo, costCenter|
|tagValue |string |etiquetas |Valor de la etiqueta, como por ejemplo, headquarter|

Al crear una asignación a través de PowerShell o la CLI de Azure, los valores de los parámetros se pueden pasar como JSON en una cadena o a través de un archivo mediante `-PolicyParameter` (PowerShell) o `--params` (la CLI de Azure).
PowerShell también admite `-PolicyParameterObject`, que requiere que se pase al cmdlet una tabla de hash de nombre y valor donde **Nombre** es el nombre del parámetro y **Valor** es un valor único o una matriz de valores que se pasa durante la asignación.

En este parámetro de ejemplo, se definen el _tagName_ **costCenter** y el _tagValue_ **headquarter**.

```json
{
    "tagName": {
        "value": "costCenter"
    },
    "tagValue": {
        "value": "headquarter"
    }
}
```

## <a name="azure-portal"></a>Azure Portal

[![Implementación en Azure](../media/deploy/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2Fbuilt-in-policy%2Fapply-default-tag-value%2Fazurepolicy.json)
[![Implementación en Azure Gov](../media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2Fbuilt-in-policy%2Fapply-default-tag-value%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh.md)]

### <a name="deploy-with-azure-powershell"></a>Implementación con Azure PowerShell

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzureRmPolicyDefinition -Name 'allowed-custom-images' -DisplayName 'Approved VM images' -description 'This policy governs the approved VM images' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/apply-default-tag-value/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/apply-default-tag-value/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a subscription or management group
$scope = Get-AzureRmResourceGroup -Name 'YourResourceGroup'

# Set the Policy Parameter (JSON format)
$policyparam = '{ "tagName": { "value": "costCenter" }, "tagValue": { "value": "headquarter" } }'

# Create the Policy Assignment
$assignment = New-AzureRmPolicyAssignment -Name 'apply-default-tag-value' -DisplayName 'Apply tag and its default value Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter $policyparam
```

### <a name="remove-with-azure-powershell"></a>Eliminación con Azure PowerShell

Ejecute los siguientes comandos para eliminar la asignación y la definición anteriores:

```azurepowershell-interactive
# Remove the Policy Assignment
Remove-AzureRmPolicyAssignment -Id $assignment.ResourceId

# Remove the Policy Definition
Remove-AzureRmPolicyDefinition -Id $definition.ResourceId
```

### <a name="azure-powershell-explanation"></a>Explicación de Azure PowerShell

Los scripts de implementación y eliminación usan los siguientes comandos. Cada comando de la tabla siguiente crea un vínculo a documentación específica del comando:

| Get-Help | Notas |
|---|---|
| [New-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/new-azurermpolicydefinition) | Crea una nueva definición de directiva de Azure. |
| [Get-AzureRmResourceGroup](/powershell/module/azurerm.resources/get-azurermresourcegroup) | Obtiene un único grupo de recursos. |
| [New-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/new-azurermpolicyassignment) | Crea una nueva asignación de directiva de Azure. En este ejemplo, se proporciona una definición, pero también puede tomar una iniciativa. |
| [Remove-AzureRmPolicyAssignment](/powershell/module/azurerm.resources/remove-azurermpolicyassignment) | Elimina una asignación de directiva de Azure existente. |
| [Remove-AzureRmPolicyDefinition](/powershell/module/azurerm.resources/remove-azurermpolicydefinition) | Elimina una definición de directiva de Azure existente. |

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

```azurecli-interactive
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'apply-default-tag-value' --display-name 'Apply tag and its default value' --description 'Applies a required tag and its default value if it is not specified by the user' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/apply-default-tag-value/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/built-in-policy/apply-default-tag-value/azurepolicy.parameters.json' --mode All)

# Set the scope to a resource group; may also be a subscription or management group
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyparam='{ "tagName": { "value": "costCenter" }, "tagValue": { "value": "headquarter" } }'

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'apply-default-tag-value' --display-name 'Apply tag and its default value Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
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

Hay varias herramientas que se pueden usar para interactuar con la API REST de Resource Manager, como [ARMClient](https://github.com/projectkudu/ARMClient) o PowerShell. Encontrará un ejemplo de llamada a la API REST desde PowerShell en la sección **Alias** de la [Estructura de definición de directiva](../concepts/definition-structure.md#aliases).

## <a name="rest-api"></a>API DE REST

### <a name="deploy-with-rest-api"></a>Implementación con la API de REST

- Cree la definición de directiva (ámbito de la suscripción). Use el código JSON de [definición de directiva](#policy-definition) para el cuerpo de la solicitud.

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/apply-default-tag-value?api-version=2016-12-01
  ```

- Cree la asignación de directiva (ámbito de grupo de recursos)

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/apply-default-tag-value-assignment?api-version=2017-06-01-preview
  ```

  Utilice el siguiente ejemplo JSON para el cuerpo de la solicitud:

  ```json
  {
      "properties": {
          "displayName": "Apply tag and its default value Assignment",
          "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/apply-default-tag-value",
          "parameters": {
              "tagName": {
                  "value": "costCenter"
              },
              "tagValue": {
                  "value": "headquarter"
              }
          }
      }
  }
  ```

### <a name="remove-with-rest-api"></a>Eliminación con la API REST

- Eliminación de la asignación de directiva

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/apply-default-tag-value-assignment?api-version=2017-06-01-preview
  ```

- Eliminación de la definición de directiva

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/apply-default-tag-value?api-version=2016-12-01
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
- Consulte la [Estructura de definición de Azure Policy](../concepts/definition-structure.md)