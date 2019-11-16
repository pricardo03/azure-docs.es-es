---
title: 'Ejemplo: Aplicación de una etiqueta y su valor en grupos de recursos'
description: Esta definición de directiva de ejemplo requiere una etiqueta y un valor en un grupo de recursos.
ms.date: 01/31/2019
ms.topic: sample
ms.openlocfilehash: 1a4bf9d27971b149e3df422987f58d0f184181c2
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/14/2019
ms.locfileid: "74076274"
---
# <a name="sample---enforce-tag-and-its-value-on-resource-groups"></a>Ejemplo: Aplicar una etiqueta y su valor en grupos de recursos

Esta directiva requiere una etiqueta y un valor en un grupo de recursos. Se especifica el valor y el nombre de etiqueta que se requieren.

Puede implementar esta directiva de ejemplo mediante:

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [CLI de Azure](#azure-cli)
- [API DE REST](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>Directiva de ejemplo

### <a name="policy-definition"></a>Definición de directiva

Definición de directiva JSON compuesta completa, utilizada por la API REST, botones "Implementar en Azure" y manualmente en el portal.

[!code-json[main](../../../../policy-templates/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.json "Enforce tag and its value on resource groups")]

> [!NOTE]
> Si va a crear manualmente una directiva en el portal, use las partes **properties.parameters** y **properties.policyRule** de los pasos anteriores. Encapsule las dos secciones con llaves `{}` para que sea un valor JSON válido.

### <a name="policy-rules"></a>Reglas de directiva

Código JSON que define las reglas de la directiva, utilizado por la CLI de Azure y Azure PowerShell.

[!code-json[rule](../../../../policy-templates/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>Parámetros de directiva

Código JSON que define los parámetros de la directiva, utilizado por la CLI de Azure y Azure PowerShell.

[!code-json[parameters](../../../../policy-templates/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.parameters.json "Policy parameters (JSON)")]

|NOMBRE |type |Campo |DESCRIPCIÓN |
|---|---|---|---|
|tagName |Cadena |etiquetas |Nombre de la etiqueta, como por ejemplo, costCenter|
|tagValue |Cadena |etiquetas |Valor de la etiqueta, como por ejemplo, headquarter|

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

## <a name="azure-portal"></a>Portal de Azure

[![Implementación del ejemplo de directiva en Azure](../media/deploy/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FResourceGroup%2Fenforce-resourceGroup-tags%2Fazurepolicy.json)
[![Implementación del ejemplo de directiva en Azure Gov](../media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FResourceGroup%2Fenforce-resourceGroup-tags%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Implementación con Azure PowerShell

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'enforce-resourceGroup-tags' -DisplayName 'Enforce tag and its value on resource groups' -description 'Enforces a required tag and its value on resource groups.' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the Policy Parameter (JSON format)
$policyParam = '{ "tagName": { "value": "costCenter" }, "tagValue": { "value": "headquarter" } }'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'enforce-resourceGroup-tags-assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter $policyParam
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
definition=$(az policy definition create --name 'enforce-resourceGroup-tags' --display-name 'Enforce tag and its value on resource groups' --description 'Enforces a required tag and its value on resource groups.' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/ResourceGroup/enforce-resourceGroup-tags/azurepolicy.parameters.json' --mode All)

# Set the scope to a resource group; may also be a resource, subscription, or management group
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyParam='{ "tagName": { "value": "costCenter" }, "tagValue": { "value": "headquarter" } }'

# Create the Policy Assignment
assignment=$(
az policy assignment create --name 'enforce-resourceGroup-tags-assignment' --display-name 'Enforce tag and its value on resource groups'  --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
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
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/enforce-resourceGroup-tags?api-version=2016-12-01
  ```

- Cree la asignación de directiva (ámbito de grupo de recursos)

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/enforce-resourceGroup-tags-assignment?api-version=2017-06-01-preview
  ```

  Utilice el siguiente ejemplo JSON para el cuerpo de la solicitud:

```json
  {
      "properties": {
          "displayName": "Enforce tag and its value Assignment",
          "policyDefinitionId": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/enforce-resourceGroup-tags",
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
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/enforce-resourceGroup-tags-assignment?api-version=2017-06-01-preview
  ```

- Eliminación de la definición de directiva

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/enforce-resourceGroup-tags?api-version=2016-12-01
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