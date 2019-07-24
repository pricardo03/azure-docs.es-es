---
title: Secreto de Key Vault con la plantilla de Azure Resource Manager | Microsoft Docs
description: Muestra cómo pasar un secreto de un almacén de claves como un parámetro durante la implementación.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: tomfitz
ms.openlocfilehash: de52dbb10d515a2255b5886df5bf0a0faa454f6b
ms.sourcegitcommit: 2e4b99023ecaf2ea3d6d3604da068d04682a8c2d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67672756"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Uso de Azure Key Vault para pasar el valor de parámetro seguro durante la implementación

En lugar de pasar un valor seguro (como una contraseña) directamente en la plantilla o el archivo de parámetro, puede recuperar el valor de [Azure Key Vault](../key-vault/key-vault-whatis.md) durante una implementación. El valor se recupera haciendo referencia a Key Vault y al secreto del archivo de parámetros. El valor nunca se expone debido a que solo hace referencia a su identificador de almacén de claves. El almacén de claves puede existir en una suscripción distinta a la que usa para implementar el grupo de recursos.

## <a name="deploy-key-vaults-and-secrets"></a>Implementación de almacenes de claves y secretos

Para acceder a un almacén de claves durante la implementación de plantilla, establezca `enabledForTemplateDeployment` en el almacén de claves en `true`.

En los ejemplos de la CLI de Azure y Azure PowerShell siguientes se muestra cómo crear el almacén de claves y agregar un secreto.

```azurecli
az group create --name $resourceGroupName --location $location
az keyvault create \
  --name $keyVaultName \
  --resource-group $resourceGroupName \
  --location $location \
  --enabled-for-template-deployment true
az keyvault secret set --vault-name $keyVaultName --name "ExamplePassword" --value "hVFkk965BuUv"
```

```azurepowershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzKeyVault `
  -VaultName $keyVaultName `
  -resourceGroupName $resourceGroupName `
  -Location $location `
  -EnabledForTemplateDeployment
$secretvalue = ConvertTo-SecureString 'hVFkk965BuUv' -AsPlainText -Force
$secret = Set-AzKeyVaultSecret -VaultName $keyVaultName -Name 'ExamplePassword' -SecretValue $secretvalue
```

Como propietario del almacén de claves, tiene acceso de forma automática a la creación de secretos. Si el usuario que trabaja con los secretos no es el propietario del almacén de claves, conceda acceso con:

```azurecli
az keyvault set-policy \
  --upn $userPrincipalName \
  --name $keyVaultName \
  --secret-permissions set delete get list
```

```azurepowershell
$userPrincipalName = "<Email Address of the deployment operator>"

Set-AzKeyVaultAccessPolicy `
  -VaultName $keyVaultName `
  -UserPrincipalName $userPrincipalName `
  -PermissionsToSecrets set,delete,get,list
```

Para más información sobre cómo crear almacenes de claves y agregar secretos, vea:

- [Establecimiento y recuperación de un secreto mediante la CLI](../key-vault/quick-create-cli.md)
- [Establecimiento y recuperación de un secreto mediante Powershell](../key-vault/quick-create-powershell.md)
- [Establecimiento y recuperación de un secreto mediante Portal](../key-vault/quick-create-portal.md)
- [Establecimiento y recuperación de un secreto mediante .NET](../key-vault/quick-create-net.md)
- [Establecimiento y recuperación de un secreto mediante Node.js](../key-vault/quick-create-node.md)

## <a name="grant-access-to-the-secrets"></a>Concesión de acceso a los secretos

El usuario que implementa la plantilla debe tener el permiso `Microsoft.KeyVault/vaults/deploy/action` en el ámbito del grupo de recursos y el almacén de claves. Los roles [Propietario](../role-based-access-control/built-in-roles.md#owner) y [Colaborador](../role-based-access-control/built-in-roles.md#contributor) conceden este acceso. Si ha creado el almacén de claves, es el propietario, por lo que tiene el permiso.

El siguiente procedimiento muestra cómo crear un rol con los permisos mínimos y cómo asignar el usuario:

1. Creación de un archivo JSON de definición de rol personalizado

    ```json
    {
      "Name": "Key Vault resource manager template deployment operator",
      "IsCustom": true,
      "Description": "Lets you deploy a resource manager template with the access to the secrets in the Key Vault.",
      "Actions": [
        "Microsoft.KeyVault/vaults/deploy/action"
      ],
      "NotActions": [],
      "DataActions": [],
      "NotDataActions": [],
      "AssignableScopes": [
        "/subscriptions/00000000-0000-0000-0000-000000000000"
      ]
    }
    ```
    Reemplace "00000000-0000-0000-0000-000000000000" por el identificador de la suscripción.

2. Cree el nuevo rol con el archivo JSON:

    ```azurecli
    az role definition create --role-definition "<PathToRoleFile>"
    az role assignment create \
      --role "Key Vault resource manager template deployment operator" \
      --assignee $userPrincipalName \
      --resource-group $resourceGroupName
    ```

    ```azurepowershell
    New-AzRoleDefinition -InputFile "<PathToRoleFile>" 
    New-AzRoleAssignment `
      -ResourceGroupName $resourceGroupName `
      -RoleDefinitionName "Key Vault resource manager template deployment operator" `
      -SignInName $userPrincipalName
    ```

    En los ejemplos se asigna el rol personalizado al usuario en el nivel de grupo de recursos.  

Cuando se usa una instancia de Key Vault con la plantilla para una [aplicación administrada](../managed-applications/overview.md), debe conceder acceso a la entidad de servicio del **proveedor de recursos de dispositivo**. Para más información, consulte [Acceso al secreto de Key Vault al implementar Azure Managed Applications](../managed-applications/key-vault-access.md).

## <a name="reference-secrets-with-static-id"></a>Referencia a secretos con identificador estático

Con este enfoque, se hace referencia al almacén de claves del archivo de parámetros, no de la plantilla. La siguiente imagen muestra que el archivo de parámetros hace referencia al secreto y pasa dicho valor a la plantilla.

![Diagrama de identificador estático de integración de almacén de claves de Resource Manager](./media/resource-manager-keyvault-parameter/statickeyvault.png)

[Tutorial: Integración de Azure Key Vault en Template Deployment de Resource Manager](./resource-manager-tutorial-use-key-vault.md) usa este método.

En la plantilla siguiente se implementa un servidor SQL que incluye una contraseña de administrador. El parámetro de contraseña se establece en una cadena segura. Pero en la plantilla no se especifica de dónde procede ese valor.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminLogin": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    },
    "sqlServerName": {
      "type": "string"
    }
  },
  "resources": [
    {
      "name": "[parameters('sqlServerName')]",
      "type": "Microsoft.Sql/servers",
      "apiVersion": "2015-05-01-preview",
      "location": "[resourceGroup().location]",
      "tags": {},
      "properties": {
        "administratorLogin": "[parameters('adminLogin')]",
        "administratorLoginPassword": "[parameters('adminPassword')]",
        "version": "12.0"
      }
    }
  ],
  "outputs": {
  }
}
```

Ahora, cree un archivo de parámetros para la plantilla anterior. En el archivo de parámetros, especifique un parámetro que coincida con el nombre del parámetro de la plantilla. Para el valor del parámetro, haga referencia al secreto del almacén de claves. Se hace referencia al secreto pasando el identificador de recurso de almacén de claves y el nombre del secreto:

En el siguiente archivo de parámetros, debe existir el secreto del almacén de claves y tendrá que proporcionar un valor estático para su identificador de recurso.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "adminLogin": {
            "value": "exampleadmin"
        },
        "adminPassword": {
            "reference": {
              "keyVault": {
                "id": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.KeyVault/vaults/<vault-name>"
              },
              "secretName": "ExamplePassword"
            }
        },
        "sqlServerName": {
            "value": "<your-server-name>"
        }
    }
}
```

Si necesita utilizar una versión del secreto que no sea la versión actual, use la propiedad `secretVersion`.

```json
"secretName": "ExamplePassword",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Implemente la plantilla y pase el archivo de parámetros:

Para la CLI de Azure, utilice:

```azurecli
az group create --name $resourceGroupName --location $location
az group deployment create \
    --resource-group $resourceGroupName \
    --template-uri <The Template File URI> \
    --parameters <The Parameter File>
```

Para PowerShell, use:

```powershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri <The Template File URI> `
  -TemplateParameterFile <The Parameter File>
```

## <a name="reference-secrets-with-dynamic-id"></a>Referencia a secretos con identificador dinámico

En la sección anterior se mostró cómo pasar un identificador de recurso estático para el secreto del almacén de claves con el parámetro. Sin embargo, en algunos escenarios, debe hacer referencia a un secreto del Almacén de claves que varía en función de la implementación actual. O bien, puede pasar valores de parámetros a la plantilla en lugar de crear un parámetro de referencia en el archivo de parámetros. En cualquier caso, puede generar dinámicamente el identificador de recurso de un secreto del almacén de claves mediante el uso de una plantilla vinculada.

El identificador del recurso no se puede generar dinámicamente en el archivo de parámetros, ya que no se permiten expresiones de plantilla en este tipo de archivos. 

En la plantilla primaria, agregue la plantilla vinculada y pase un parámetro que contenga el identificador de recurso generado dinámicamente. La siguiente imagen muestra la forma en que un parámetro en la plantilla vinculada hace referencia el secreto.

![Identificador dinámico](./media/resource-manager-keyvault-parameter/dynamickeyvault.png)

La [siguiente plantilla](https://github.com/Azure/azure-quickstart-templates/tree/master/201-key-vault-use-dynamic-id) crea dinámicamente el identificador de almacén de claves y lo pasa como parámetro.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]",
            "metadata": {
                "description": "The location where the resources will be deployed."
            }
        },
        "vaultName": {
            "type": "string",
            "metadata": {
                "description": "The name of the keyvault that contains the secret."
            }
        },
        "secretName": {
            "type": "string",
            "metadata": {
                "description": "The name of the secret."
            }
        },
        "vaultResourceGroupName": {
            "type": "string",
            "metadata": {
                "description": "The name of the resource group that contains the keyvault."
            }
        },
        "vaultSubscription": {
            "type": "string",
            "defaultValue": "[subscription().subscriptionId]",
            "metadata": {
                "description": "The name of the subscription that contains the keyvault."
            }
        },
        "_artifactsLocation": {
            "type": "string",
            "metadata": {
                "description": "The base URI where artifacts required by this template are located. When the template is deployed using the accompanying scripts, a private location in the subscription will be used and this value will be automatically generated."
            },
            "defaultValue": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/"
        },
        "_artifactsLocationSasToken": {
            "type": "securestring",
            "metadata": {
                "description": "The sasToken required to access _artifactsLocation.  When the template is deployed using the accompanying scripts, a sasToken will be automatically generated."
            },
            "defaultValue": ""
        }
    },
    "resources": [
        {
            "apiVersion": "2018-05-01",
            "name": "dynamicSecret",
            "type": "Microsoft.Resources/deployments",
            "properties": {
                "mode": "Incremental",
                "templateLink": {
                    "contentVersion": "1.0.0.0",
                    "uri": "[uri(parameters('_artifactsLocation'), concat('./nested/sqlserver.json', parameters('_artifactsLocationSasToken')))]"
                },
                "parameters": {
                    "location": {
                        "value": "[parameters('location')]"
                    },
                    "adminLogin": {
                        "value": "ghuser"
                    },
                    "adminPassword": {
                        "reference": {
                            "keyVault": {
                                "id": "[resourceId(parameters('vaultSubscription'), parameters('vaultResourceGroupName'), 'Microsoft.KeyVault/vaults', parameters('vaultName'))]"
                            },
                            "secretName": "[parameters('secretName')]"
                        }
                    }
                }
            }
        }
    ],
    "outputs": {
        "sqlFQDN": {
            "type": "string",
            "value": "[reference('dynamicSecret').outputs.sqlFQDN.value]"
        }
    }
}
```

Implemente la plantilla anterior y proporcione valores para los parámetros. Puede utilizar la plantilla de ejemplo de GitHub, pero debe especificar los valores de los parámetros para su entorno.

Para la CLI de Azure, utilice:

```azurecli
az group create --name $resourceGroupName --location $location
az group deployment create \
    --resource-group $resourceGroupName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json \
    --parameters vaultName=$keyVaultName vaultResourceGroupName=examplegroup secretName=examplesecret
```

Para PowerShell, use:

```powershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json `
  -vaultName $keyVaultName -vaultResourceGroupName $keyVaultResourceGroupName -secretName $secretName
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre los almacenes de claves, consulte [¿Qué es Azure Key Vault?](../key-vault/key-vault-overview.md)
- Para obtener ejemplos completos de secretos de clave de referencia, consulte [Ejemplos de Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
