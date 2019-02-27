---
title: Administración del acceso a los recursos de Azure mediante RBAC y plantillas de Azure Resource Manager | Microsoft Docs
description: Aprenda a administrar el acceso a los recursos de Azure de usuarios, grupos y aplicaciones mediante control de acceso basado en rol (RBAC) y plantillas de Azure Resource Manager.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 537ee35e96a41cd02605319e244d39c6567c3bf1
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/18/2019
ms.locfileid: "56337210"
---
# <a name="manage-access-to-azure-resources-using-rbac-and-azure-resource-manager-templates"></a>Administración del acceso a los recursos de Azure mediante RBAC y plantillas de Azure Resource Manager

El [control de acceso basado en rol (RBAC)](overview.md) es la forma en la que se administra el acceso a los recursos de Azure. Además de usar Azure PowerShell o la CLI de Azure, puede administrar el acceso a los recursos de Azure mediante RBAC y [plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md). Las plantillas se pueden usar si necesita implementar recursos de manera repetida y uniforme. En este artículo se describe cómo puede administrar el acceso mediante RBAC y plantillas.

## <a name="example-template-to-create-a-role-assignment"></a>Plantilla de ejemplo para crear una asignación de roles

En RBAC, para conceder acceso es preciso crear una asignación de roles. La plantilla siguiente muestra:
- Cómo asignar un rol a un usuario, grupo o aplicación, en el ámbito del grupo de recursos
- Cómo especificar los roles de propietario, colaborador y lector como parámetro

Para usar la plantilla, debe especificar las siguientes entradas:
- El nombre de un grupo de recursos
- El identificador único de un usuario, grupo o aplicación a los que se les asignará el rol
- La función para asignar
- Un identificador único que se usará para la asignación del rol

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "principalId": {
      "type": "string",
      "metadata": {
        "description": "The principal to assign the role to"
      }
    },
    "builtInRoleType": {
      "type": "string",
      "allowedValues": [
        "Owner",
        "Contributor",
        "Reader"
      ],
      "metadata": {
        "description": "Built-in role to assign"
      }
    },
    "roleNameGuid": {
      "type": "string",
      "metadata": {
        "description": "A new GUID used to identify the role assignment"
      }
    }
  },
  "variables": {
    "Owner": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', '8e3af657-a8ff-443c-a75c-2fe8c4bcb635')]",
    "Contributor": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'b24988ac-6180-42a0-ab88-20f7382dd24c')]",
    "Reader": "[concat('/subscriptions/', subscription().subscriptionId, '/providers/Microsoft.Authorization/roleDefinitions/', 'acdd72a7-3385-48ef-bd42-f606fba81ae7')]",
    "scope": "[resourceGroup().id]"
  },
  "resources": [
    {
      "type": "Microsoft.Authorization/roleAssignments",
      "apiVersion": "2017-05-01",
      "name": "[parameters('roleNameGuid')]",
      "properties": {
        "roleDefinitionId": "[variables(parameters('builtInRoleType'))]",
        "principalId": "[parameters('principalId')]",
        "scope": "[variables('scope')]"
      }
    }
  ]
}
```

A continuación se muestra un ejemplo de una asignación de rol de lector a un usuario después de implementar la plantilla.

![Asignación de roles mediante una plantilla](./media/role-assignments-template/role-assignment-template.png)

## <a name="deploy-template-using-azure-powershell"></a>Implementación de una plantilla mediante Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Para implementar la plantilla anterior con Azure PowerShell, siga estos pasos.

1. Cree un nuevo archivo denominado rbac-rg.json y copie la plantilla anterior.

1. Inicie sesión en [Azure PowerShell](/powershell/azure/authenticate-azureps).

1. Obtenga el identificador único de un usuario, grupo o aplicación. Por ejemplo, puede usar el comando [Get-AzADUser](/powershell/module/az.resources/get-azaduser) para enumerar los usuarios de Azure AD.

    ```azurepowershell
    Get-AzADUser
    ```

1. Utilice una herramienta de GUID para generar un identificador único que se use para la asignación de roles. El identificador tiene el formato: `11111111-1111-1111-1111-111111111111`

1. Cree un grupo de recursos de ejemplo.

    ```azurepowershell
    New-AzResourceGroup -Name ExampleGroup -Location "Central US"
    ```

1. Utilice el comando [New-AzResourceGroupDeployment](/powershell/module/az.resources/new-azresourcegroupdeployment) para iniciar la implementación.

    ```azurepowershell
    New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json
    ```

    Deberá especificar los parámetros necesarios. El siguiente texto muestra un ejemplo de la salida.

    ```Output
    PS /home/user> New-AzResourceGroupDeployment -ResourceGroupName ExampleGroup -TemplateFile rbac-rg.json
    
    cmdlet New-AzResourceGroupDeployment at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    principalId: 22222222-2222-2222-2222-222222222222
    builtInRoleType: Reader
    roleNameGuid: 11111111-1111-1111-1111-111111111111
    
    DeploymentName          : rbac-rg
    ResourceGroupName       : ExampleGroup
    ProvisioningState       : Succeeded
    Timestamp               : 7/17/2018 7:46:32 PM
    Mode                    : Incremental
    TemplateLink            :
    Parameters              :
                              Name             Type                       Value
                              ===============  =========================  ==========
                              principalId      String                     22222222-2222-2222-2222-222222222222
                              builtInRoleType  String                     Reader
                              roleNameGuid     String                     11111111-1111-1111-1111-111111111111
    
    Outputs                 :
    DeploymentDebugLogLevel :
    ```

## <a name="deploy-template-using-the-azure-cli"></a>Implementación de la plantilla mediante la CLI de Azure

Para implementar la plantilla anterior con la CLI de Azure, siga estos pasos.

1. Cree un nuevo archivo denominado rbac-rg.json y copie la plantilla anterior.

1. Inicie sesión en la [CLI de Azure](/cli/azure/authenticate-azure-cli).

1. Obtenga el identificador único de un usuario, grupo o aplicación. Por ejemplo, puede usar el comando [az ad user list](/cli/azure/ad/user#az-ad-user-list) para enumerar los usuarios de Azure AD.

    ```azurecli
    az ad user list
    ```

1. Utilice una herramienta de GUID para generar un identificador único que se use para la asignación de roles. El identificador tiene el formato: `11111111-1111-1111-1111-111111111111`

1. Cree un grupo de recursos de ejemplo.

    ```azurecli
    az group create --name ExampleGroup --location "Central US"
    ```

1. Utilice la plantilla con el comando [az group deployment create](/cli/azure/group/deployment#az-group-deployment-create) para iniciar la implementación.

    ```azurecli
    az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json
    ```

    Deberá especificar los parámetros necesarios. El siguiente texto muestra un ejemplo de la salida.

    ```Output
    C:\Azure\Templates>az group deployment create --resource-group ExampleGroup --template-file rbac-rg.json
    Please provide string value for 'principalId' (? for help): 22222222-2222-2222-2222-222222222222
    Please provide string value for 'builtInRoleType' (? for help):
     [1] Owner
     [2] Contributor
     [3] Reader
    Please enter a choice [1]: 3
    Please provide string value for 'roleNameGuid' (? for help): 11111111-1111-1111-1111-111111111111
    {
      "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/rbac-rg",
      "name": "rbac-rg",
      "properties": {
        "additionalProperties": {
          "duration": "PT9.5323924S",
          "outputResources": [
            {
              "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/ExampleGroup/providers/Microsoft.Authorization/roleAssignments/11111111-1111-1111-1111-111111111111",
              "resourceGroup": "ExampleGroup"
            }
          ],
          "templateHash": "0000000000000000000"
        },
        "correlationId": "33333333-3333-3333-3333-333333333333",
        "debugSetting": null,
        "dependencies": [],
        "mode": "Incremental",
        "outputs": null,
        "parameters": {
          "builtInRoleType": {
            "type": "String",
            "value": "Reader"
          },
          "principalId": {
            "type": "String",
            "value": "22222222-2222-2222-2222-222222222222"
          },
          "roleNameGuid": {
            "type": "String",
            "value": "11111111-1111-1111-1111-111111111111"
          }
        },
        "parametersLink": null,
        "providers": [
          {
            "id": null,
            "namespace": "Microsoft.Authorization",
            "registrationState": null,
            "resourceTypes": [
              {
                "aliases": null,
                "apiVersions": null,
                "locations": [
                  null
                ],
                "properties": null,
                "resourceType": "roleAssignments"
              }
            ]
          }
        ],
        "provisioningState": "Succeeded",
        "template": null,
        "templateLink": null,
        "timestamp": "2018-07-17T19:00:31.830904+00:00"
      },
      "resourceGroup": "ExampleGroup"
    }
    ```
    
## <a name="next-steps"></a>Pasos siguientes

- [Inicio rápido: Creación e implementación de plantillas de Azure Resource Manager mediante Azure Portal](../azure-resource-manager/resource-manager-quickstart-create-templates-use-the-portal.md)
- [Nociones sobre la estructura y la sintaxis de las plantillas de Azure Resource Manager](../azure-resource-manager/resource-group-authoring-templates.md)
- [Plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/?term=rbac)
