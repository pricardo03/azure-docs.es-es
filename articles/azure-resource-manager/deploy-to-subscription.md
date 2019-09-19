---
title: 'Creación de grupos de recursos y recursos en la suscripción: plantilla de Azure Resource Manager'
description: Se describe cómo crear un grupo de recursos en una plantilla de Azure Resource Manager. También se muestra cómo implementar recursos en el ámbito de la suscripción de Azure.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: tomfitz
ms.openlocfilehash: 37f2b04a62d94cce42b095540380460c38bc5b79
ms.sourcegitcommit: a4b5d31b113f520fcd43624dd57be677d10fc1c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772954"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>Creación de grupos de recursos y otros recursos en el nivel de suscripción

Normalmente, implementa los recursos de Azure en un grupo de recursos en su suscripción de Azure. Sin embargo, también puede crear grupos de recursos de Azure y crear recursos de Azure en el nivel de suscripción. Para implementar plantillas en el nivel de suscripción, use la CLI de Azure y Azure PowerShell. Azure Portal no admite la implementación en el nivel de suscripción.

Para crear un grupo de recursos en una plantilla de Azure Resource Manager, defina un recurso [Microsoft.Resources/resourceGroups](/azure/templates/microsoft.resources/allversions) con un nombre y una ubicación para el grupo de recursos. También se puede crear un grupo de recursos e implementar recursos en él en la misma plantilla. Los recursos que se pueden implementar en el nivel de suscripción incluyen: [directivas](../governance/policy/overview.md) y [control de acceso basado en rol](../role-based-access-control/overview.md).

## <a name="deployment-considerations"></a>Consideraciones de la implementación

La implementación en el nivel de suscripción es diferente de la implementación del grupo de recursos en los siguientes aspectos:

### <a name="schema-and-commands"></a>Esquema y comandos

El esquema y los comandos que se usan para las implementaciones de nivel de suscripción son diferentes de las implementaciones de grupo de recursos. 

Para el esquema, use `https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#`.

Para el comando de implementación de la CLI de Azure, use [az deployment create](/cli/azure/deployment?view=azure-cli-latest#az-deployment-create). Por ejemplo, el siguiente comando CLI implementa una plantilla para crear un grupo de recursos:

```azurecli-interactive
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```

Para el comando de implementación de PowerShell, use [New-AzDeployment](/powershell/module/az.resources/new-azdeployment). Por ejemplo, el siguiente comando PowerShell implementa una plantilla para crear un grupo de recursos:

```azurepowershell-interactive
New-AzDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

### <a name="deployment-name-and-location"></a>Nombre y ubicación de la implementación

Cuando realice una implementación en su suscripción, debe proporcionar una ubicación. También puede proporcionar un nombre para la implementación. Si no especifica un nombre para la implementación, se utilizará el nombre de la plantilla. Por ejemplo, al implementar una plantilla llamada **azuredeploy.json**, se crea un nombre de predeterminado **azuredeploy**.

La ubicación de las implementaciones de nivel de suscripción es inmutable. No se puede crear una implementación en una ubicación si ya existe una implementación con el mismo en otra ubicación. Si recibe el código de error `InvalidDeploymentLocation`, use un nombre diferente o utilice la ubicación de la implementación anterior que tenía ese mismo nombre.

### <a name="use-template-functions"></a>Usar funciones de plantillas

En las implementaciones de nivel de suscripción, hay algunas consideraciones importantes que deben tenerse en cuenta al usar las funciones de plantilla:

* La función [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) **no** se admite.
* La función [resourceId()](resource-group-template-functions-resource.md#resourceid) sí se admite. Utilícela para obtener el identificador de los recursos que se utilizan en las implementaciones de nivel de suscripción. Por ejemplo, puede obtener el identificador de recursos de una definición de directiva con `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))`.
* Se admiten las funciones [reference()](resource-group-template-functions-resource.md#reference) y [list()](resource-group-template-functions-resource.md#list).

## <a name="create-resource-groups"></a>Crear grupos de recursos

En la plantilla siguiente se crea un grupo de recursos vacío.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        }
    ],
    "outputs": {}
}
```

El esquema de la plantilla puede encontrarse [aquí](/azure/templates/microsoft.resources/allversions). Encontrará plantillas similares en [GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments).

## <a name="create-multiple-resource-groups"></a>Creación de varios grupos de recursos

Use el [elemento copy](resource-group-create-multiple.md) con grupos de recursos para crear más de un grupo de recursos. 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgNamePrefix": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "instanceCount": {
            "type": "int"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
            "copy": {
                "name": "rgCopy",
                "count": "[parameters('instanceCount')]"
            },
            "properties": {}
        }
    ],
    "outputs": {}
}
```

Para obtener más información sobre la iteración de recursos, consulte [Implementación de varias instancias de un recurso o una propiedad en plantillas de Azure Resource Manager](./resource-group-create-multiple.md) y [Tutorial: Creación de varias instancias de recursos con plantillas de Resource Manager](./resource-manager-tutorial-create-multiple-instances.md).

## <a name="create-resource-group-and-deploy-resources"></a>Creación de un grupo de recursos e implementación de recursos

Para crear el grupo de recursos e implementar recursos en él, utilice una plantilla anidada. La plantilla anidada define los recursos que se van a implementar en el grupo de recursos. Establezca la plantilla anidada como dependiente del grupo de recursos para asegurarse de que el grupo de recursos existe antes de implementar los recursos.

En el ejemplo siguiente se crea un grupo de recursos y se implementa una cuenta de almacenamiento en el grupo de recursos.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.1",
    "parameters": {
        "rgName": {
            "type": "string"
        },
        "rgLocation": {
            "type": "string"
        },
        "storagePrefix": {
            "type": "string",
            "maxLength": 11
        }
    },
    "variables": {
        "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
    },
    "resources": [
        {
            "type": "Microsoft.Resources/resourceGroups",
            "apiVersion": "2018-05-01",
            "location": "[parameters('rgLocation')]",
            "name": "[parameters('rgName')]",
            "properties": {}
        },
        {
            "type": "Microsoft.Resources/deployments",
            "apiVersion": "2018-05-01",
            "name": "storageDeployment",
            "resourceGroup": "[parameters('rgName')]",
            "dependsOn": [
                "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
            ],
            "properties": {
                "mode": "Incremental",
                "template": {
                    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
                    "contentVersion": "1.0.0.0",
                    "parameters": {},
                    "variables": {},
                    "resources": [
                        {
                            "type": "Microsoft.Storage/storageAccounts",
                            "apiVersion": "2017-10-01",
                            "name": "[variables('storageName')]",
                            "location": "[parameters('rgLocation')]",
                            "kind": "StorageV2",
                            "sku": {
                                "name": "Standard_LRS"
                            }
                        }
                    ],
                    "outputs": {}
                }
            }
        }
    ],
    "outputs": {}
}
```

## <a name="create-policies"></a>Creación de directivas

### <a name="assign-policy"></a>Asignación de directiva

En el ejemplo siguiente se asigna una definición de directiva existente a la suscripción. Si la directiva toma parámetros, proporciónelos como un objeto. Si la directiva no toma parámetros, use el objeto vacío predeterminado.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "policyDefinitionID": {
            "type": "string"
        },
        "policyName": {
            "type": "string"
        },
        "policyParameters": {
            "type": "object",
            "defaultValue": {}
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "[parameters('policyName')]",
            "apiVersion": "2018-03-01",
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[parameters('policyDefinitionID')]",
                "parameters": "[parameters('policyParameters')]"
            }
        }
    ]
}
```

Para implementar esta plantilla con la CLI de Azure, use:

```azurecli-interactive
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Para implementar esta plantilla con PowerShell, use:

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="define-and-assign-policy"></a>Definición y asignación de directivas

Puede [definir](../governance/policy/concepts/definition-structure.md) y asignar una directiva en la misma plantilla.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-05-01",
            "properties": {
                "policyType": "Custom",
                "parameters": {},
                "policyRule": {
                    "if": {
                        "field": "location",
                        "equals": "northeurope"
                    },
                    "then": {
                        "effect": "deny"
                    }
                }
            }
        },
        {
            "type": "Microsoft.Authorization/policyAssignments",
            "name": "location-lock",
            "apiVersion": "2018-05-01",
            "dependsOn": [
                "locationpolicy"
            ],
            "properties": {
                "scope": "[subscription().id]",
                "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
            }
        }
    ]
}
```

Para crear la definición de directiva en su suscripción y aplicarla a la suscripción, use el siguiente comando de la CLI:

```azurecli
az deployment create \
  --name demoDeployment \
  --location centralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

Para implementar esta plantilla con PowerShell, use:

```azurepowershell
New-AzDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="next-steps"></a>Pasos siguientes

* Para aprender sobre los roles de asignación, consulte [Administración del acceso a los recursos de Azure mediante RBAC y plantillas de Azure Resource Manager](../role-based-access-control/role-assignments-template.md).
* Para un ejemplo de implementación de la configuración del área de trabajo para Azure Security Center, consulte [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Para más información sobre la creación de plantillas del Administrador de recursos de Azure, consulte [Creación de plantillas](resource-group-authoring-templates.md). 
* Para obtener una lista de las funciones disponibles en una plantilla, consulte [Funciones de plantilla](resource-group-template-functions.md).
