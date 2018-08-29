---
title: Implementación de recursos en la suscripción de Azure | Microsoft Docs
description: Describe cómo crear una plantilla de Azure Resource Manager que implementa los recursos en el ámbito de la suscripción.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2018
ms.author: tomfitz
ms.openlocfilehash: 6166161f6d50e747681217281a0afc6514df78fb
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617458"
---
# <a name="deploy-resources-to-an-azure-subscription"></a>Implementación de recursos en una suscripción de Azure

Normalmente, implementa los recursos en un grupo de recursos en su suscripción de Azure. Sin embargo, algunos recursos se pueden implementar en el nivel de su suscripción de Azure. Estos recursos se aplican a través de su suscripción. Las [directivas](../azure-policy/azure-policy-introduction.md), el [control de acceso basado en rol](../role-based-access-control/overview.md) y [Azure Security Center](../security-center/security-center-intro.md) son servicios que puede que desee aplicar en el nivel de suscripción, en lugar de en el nivel de grupo de recursos.

En este artículo, se utiliza la CLI de Azure y PowerShell para implementar las plantillas.

## <a name="name-and-location-for-deployment"></a>Nombre y ubicación de la implementación

Cuando realice una implementación en su suscripción, debe proporcionar una ubicación. También puede proporcionar un nombre para la implementación. Si no especifica un nombre para la implementación, se utilizará el nombre de la plantilla. Por ejemplo, al implementar una plantilla llamada **azuredeploy.json**, se crea un nombre de predeterminado **azuredeploy**.

La ubicación de las implementaciones de nivel de suscripción es inmutable. No se puede crear una implementación en una ubicación si ya existe una implementación con el mismo en otra ubicación. Si recibe el código de error `InvalidDeploymentLocation`, use un nombre diferente o utilice la ubicación de la implementación anterior que tenía ese mismo nombre.

## <a name="using-template-functions"></a>Uso de funciones de plantilla

En las implementaciones de nivel de suscripción, hay algunas consideraciones importantes que deben tenerse en cuenta al utilizar las funciones de plantilla:

* La función [resourceGroup()](resource-group-template-functions-resource.md#resourcegroup) **no** se admite.
* La función [resourceId()](resource-group-template-functions-resource.md#resourceid) sí se admite. Utilícela para obtener el identificador de los recursos que se utilizan en las implementaciones de nivel de suscripción. Por ejemplo, puede obtener el identificador de recursos de una definición de directiva con `resourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))`.
* Se admiten las funciones [reference()](resource-group-template-functions-resource.md#reference) y [list()](resource-group-template-functions-resource.md#list).

## <a name="assign-policy"></a>Asignación de directiva

En el ejemplo siguiente se asigna una definición de directiva existente a la suscripción. Si la directiva toma parámetros, proporciónelos como un objeto. Si la directiva no toma parámetros, use el objeto vacío predeterminado.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
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

Para aplicar una directiva integrada a su suscripción de Azure, use los siguientes comandos de la CLI de Azure. En este ejemplo, la directiva no tiene parámetros.

```azurecli-interactive
# Built-in policy that does not accept parameters
definition=$(az policy definition list --query "[?displayName=='Audit resource location matches resource group location'].id" --output tsv)

az deployment create \
  -n policyassign \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=auditRGLocation
```

Para implementar esta plantilla con PowerShell, use:

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Audit resource location matches resource group location' }

New-AzureRmDeployment `
  -Name policyassign `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName auditRGLocation
```

Para aplicar una directiva integrada a su suscripción de Azure, use los siguientes comandos de la CLI de Azure. En este ejemplo, la directiva tiene parámetros.

```azurecli-interactive
# Built-in policy that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment create \
  -n policyassign \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

Para implementar esta plantilla con PowerShell, use:

```azurepowershell-interactive
$definition = Get-AzureRmPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzureRmDeployment `
  -Name policyassign `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

## <a name="define-and-assign-policy"></a>Definición y asignación de directivas

Puede [definir](../azure-policy/policy-definition.md) y asignar una directiva en la misma plantilla.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {},
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/policyDefinitions",
            "name": "locationpolicy",
            "apiVersion": "2018-03-01",
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
            "apiVersion": "2018-03-01",
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

Para crear la definición de directiva en su suscripción, y aplicarla a la suscripción, use el siguiente comando de la CLI.

```azurecli-interactive
az deployment create \
  -n definePolicy \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

Para implementar esta plantilla con PowerShell, use:

```azurepowershell-interactive
New-AzureRmDeployment `
  -Name definePolicy `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json
```

## <a name="assign-role"></a>Asignación de un rol

En el ejemplo siguiente se asigna un rol a un usuario o grupo.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "principalId": {
            "type": "string"
        },
        "roleDefinitionId": {
            "type": "string"
        }
    },
    "variables": {},
    "resources": [
        {
            "type": "Microsoft.Authorization/roleAssignments",
            "name": "[guid(parameters('principalId'), deployment().name)]",
            "apiVersion": "2017-05-01",
            "properties": {
                "roleDefinitionId": "[resourceId('Microsoft.Authorization/roleDefinitions', parameters('roleDefinitionId'))]",
                "principalId": "[parameters('principalId')]"
            }
        }
    ]
}
```

Para asignar a un grupo de Active Directory a un rol de la suscripción, use los siguientes comandos de la CLI de Azure.

```azurecli-interactive
# Get ID of the role you want to assign
role=$(az role definition list --name Contributor --query [].name --output tsv)

# Get ID of the AD group to assign the role to
principalid=$(az ad group show --group demogroup --query objectId --output tsv)

az deployment create \
  -n demoRole \
  -l southcentralus \
  --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json \
  --parameters principalId=$principalid roleDefinitionId=$role
```

Para implementar esta plantilla con PowerShell, use:

```azurepowershell-interactive
$role = Get-AzureRmRoleDefinition -Name Contributor

$adgroup = Get-AzureRmADGroup -DisplayName demogroup

New-AzureRmDeployment `
  -Name demoRole `
  -Location southcentralus `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/roleassign.json `
  -roleDefinitionId $role.Id `
  -principalId $adgroup.Id
```

## <a name="next-steps"></a>Pasos siguientes
* Para un ejemplo de implementación de la configuración del área de trabajo para Azure Security Center, consulte [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json).
* Para crear un grupo de recursos, consulte [Creación de grupos de recursos en las plantillas de Azure Resource Manager](create-resource-group-in-template.md).
* Para más información sobre la creación de plantillas del Administrador de recursos de Azure, consulte [Creación de plantillas](resource-group-authoring-templates.md). 
* Para obtener una lista de las funciones disponibles en una plantilla, consulte [Funciones de plantilla](resource-group-template-functions.md).

