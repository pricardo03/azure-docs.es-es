---
title: 'Ejemplo: Imágenes de máquina virtual aprobadas'
description: Esta definición de directiva de ejemplo requiere que solo las imágenes personalizadas aprobadas se puedan implementar en su entorno.
author: DCtheGeek
manager: carmonm
ms.service: azure-policy
ms.topic: sample
ms.date: 01/26/2019
ms.author: dacoulte
ms.openlocfilehash: 8def11c2d92af618054d0353fa2687d2e88e1134
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59266722"
---
# <a name="sample---approved-virtual-machine-images"></a>Ejemplo: Imágenes de máquinas virtuales admitidas

Esta directiva requiere que solo las imágenes personalizadas aprobadas se puedan implementar en su entorno. Se especifica una matriz de identificadores de las imágenes aprobadas.

Puede implementar esta directiva de ejemplo mediante:

- [Azure Portal](#azure-portal)
- [Azure PowerShell](#azure-powershell)
- [CLI de Azure](#azure-cli)
- [API DE REST](#rest-api)

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-policy"></a>Directiva de ejemplo

### <a name="policy-definition"></a>Definición de directiva

Definición de directiva JSON compuesta completa, utilizada por la API REST, botones "Implementar en Azure" y manualmente en el portal.

[!code-json[full](../../../../policy-templates/samples/compute/allowed-custom-images/azurepolicy.json "Complete policy definition (JSON)")]

> [!NOTE]
> Si va a crear manualmente una directiva en el portal, use las partes **properties.parameters** y **properties.policyRule** de los pasos anteriores. Encapsule las dos secciones con llaves `{}` para que sea un valor JSON válido.

### <a name="policy-rules"></a>Reglas de directiva

Código JSON que define las reglas de la directiva, utilizado por la CLI de Azure y Azure PowerShell.

[!code-json[rule](../../../../policy-templates/samples/compute/allowed-custom-images/azurepolicy.rules.json "Policy rules (JSON)")]

### <a name="policy-parameters"></a>Parámetros de directiva

Código JSON que define los parámetros de la directiva, utilizado por la CLI de Azure y Azure PowerShell.

[!code-json[parameters](../../../../policy-templates/samples/compute/allowed-custom-images/azurepolicy.parameters.json "Policy parameters (JSON)")]

## <a name="parameters"></a>Parámetros

|NOMBRE |Type |Campo |DESCRIPCIÓN |
|---|---|---|---|
|imageIds |Matriz |Microsoft.Compute/imageIds |Lista de imágenes de máquina virtual aprobadas|

Al crear una asignación a través de PowerShell o la CLI de Azure, los valores de los parámetros se pueden pasar como JSON en una cadena o a través de un archivo mediante `-PolicyParameter` (PowerShell) o `--params` (la CLI de Azure).
PowerShell también admite `-PolicyParameterObject`, que requiere que se pase al cmdlet una tabla de hash de nombre y valor donde **Nombre** es el nombre del parámetro y **Valor** es un valor único o una matriz de valores que se pasa durante la asignación.

En este parámetro de ejemplo, solo se permitirán la versión de imagen _ContosoStdImage_ del grupo de recursos _YourResourceGroup_ o la versión de mayo de 2018 de Windows Server 2016 Datacenter ubicadas en "Central EE. UU.".

```json
{
    "imageIds": {
        "value": [
            "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage",
            "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510"
        ]
    }
}
```

## <a name="azure-portal"></a>Azure Portal

[![Implementación del ejemplo de directiva en Azure](../media/deploy/deploybutton.png)](https://portal.azure.com/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FCompute%2Fallowed-custom-images%2Fazurepolicy.json)
[![Implementación del ejemplo de directiva en Azure Gov](../media/deploy/deployGovbutton.png)](https://portal.azure.us/?#blade/Microsoft_Azure_Policy/CreatePolicyDefinitionBlade/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-policy%2Fmaster%2Fsamples%2FCompute%2Fallowed-custom-images%2Fazurepolicy.json)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [sample-powershell-install](../../../../includes/sample-powershell-install-no-ssh-az.md)]

### <a name="deploy-with-azure-powershell"></a>Implementación con Azure PowerShell

```azurepowershell-interactive
# Create the Policy Definition (Subscription scope)
$definition = New-AzPolicyDefinition -Name 'allowed-custom-images' -DisplayName 'Approved VM images' -description 'This policy governs the approved VM images' -Policy 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.rules.json' -Parameter 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.parameters.json' -Mode All

# Set the scope to a resource group; may also be a resource, subscription, or management group
$scope = Get-AzResourceGroup -Name 'YourResourceGroup'

# Set the Policy Parameter (JSON format)
$policyparam = '{ "imageIds": { "value": [ "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage", "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510" ] } }'

# Create the Policy Assignment
$assignment = New-AzPolicyAssignment -Name 'allowed-custom-images-assignment' -DisplayName 'Approved VM images Assignment' -Scope $scope.ResourceId -PolicyDefinition $definition -PolicyParameter $policyparam
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

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [sample-cli-install](../../../../includes/sample-cli-install.md)]

### <a name="deploy-with-azure-cli"></a>Implementación con la CLI de Azure

```azurecli-interactive
# Create the Policy Definition (Subscription scope)
definition=$(az policy definition create --name 'allowed-custom-images' --display-name 'Approved VM images' --description 'This policy governs the approved VM images' --rules 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.rules.json' --params 'https://raw.githubusercontent.com/Azure/azure-policy/master/samples/Compute/allowed-custom-images/azurepolicy.parameters.json' --mode All)

# Set the scope to a resource group; may also be a resource, subscription, or management group
scope=$(az group show --name 'YourResourceGroup')

# Set the Policy Parameter (JSON format)
policyparam='{ "imageIds": { "value": [ "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage", "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510" ] } }'

# Create the Policy Assignment
assignment=$(az policy assignment create --name 'allowed-custom-images-assignment' --display-name 'Approved VM images Assignment' --scope `echo $scope | jq '.id' -r` --policy `echo $definition | jq '.name' -r` --params "$policyparam")
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

Hay varias herramientas que se pueden usar para interactuar con la API REST de Resource Manager, como [ARMClient](https://github.com/projectkudu/ARMClient) o PowerShell. Encontrará un ejemplo de llamada a la API REST desde PowerShell en la sección **Alias** de la [Estructura de definición de directiva](../concepts/definition-structure.md#aliases).

### <a name="deploy-with-rest-api"></a>Implementación con la API de REST

- Cree la definición de directiva (ámbito de la suscripción). Use el código JSON de [definición de directiva](#policy-definition) para el cuerpo de la solicitud.

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images?api-version=2016-12-01
  ```

- Cree la asignación de directiva (ámbito de grupo de recursos)

  ```http
  PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/YourResourceGroup/providers/Microsoft.Authorization/policyAssignments/allowed-custom-images-assignment?api-version=2017-06-01-preview
  ```

  Utilice el siguiente ejemplo JSON para el cuerpo de la solicitud:

  ```json
  {
      "properties": {
          "displayName": "Approved VM images Assignment",
          "policyDefinitionId": "/subscriptions/<subscriptionId>/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images",
          "parameters": {
              "imageIds": {
                  "value": [
                      "/subscriptions/<subscriptionId>/resourceGroups/YourResourceGroup/providers/Microsoft.Compute/images/ContosoStdImage",
                      "/Subscriptions/<subscriptionId>/Providers/Microsoft.Compute/Locations/centralus/Publishers/MicrosoftWindowsServer/ArtifactTypes/VMImage/Offers/WindowsServer/Skus/2016-Datacenter/Versions/2016.127.20180510"
                  ]
              }
          }
      }
  }
  ```

### <a name="remove-with-rest-api"></a>Eliminación con la API REST

- Eliminación de la asignación de directiva

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyAssignments/allowed-custom-images-assignment?api-version=2017-06-01-preview
  ```

- Eliminación de la definición de directiva

  ```http
  DELETE https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/policyDefinitions/allowed-custom-images?api-version=2016-12-01
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
- Puede consultar más ejemplos de Azure Policy para máquinas virtuales en [Aplicación de directivas a máquinas virtuales Windows](../../../virtual-machines/windows/policy.md)