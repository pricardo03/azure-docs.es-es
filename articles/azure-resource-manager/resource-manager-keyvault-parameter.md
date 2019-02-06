---
title: Secreto de Key Vault con la plantilla de Azure Resource Manager | Microsoft Docs
description: Muestra cómo pasar un secreto de un almacén de claves como un parámetro durante la implementación.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/30/2019
ms.author: tomfitz
ms.openlocfilehash: 6e9ad6f74970b6c72b96ae142f02bee6b07fb558
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55455480"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Uso de Azure Key Vault para pasar el valor de parámetro seguro durante la implementación

En lugar de pasar un valor seguro (por ejemplo, una contraseña) directamente en el archivo del parámetro, puede recuperar el valor de [Azure Key Vault](../key-vault/key-vault-whatis.md) durante una implementación. El valor se recupera haciendo referencia a Key Vault y al secreto del archivo de parámetros. El valor nunca se expone debido a que solo hace referencia a su identificador de almacén de claves. Key Vault puede existir en una suscripción diferente en la que está implementando el grupo de recursos.

Para realizar un tutorial, consulte [Tutorial: Integración de Azure Key Vault en Resource Manager Template Deployment](./resource-manager-tutorial-use-key-vault.md#prepare-a-key-vault).

## <a name="deploy-key-vaults-and-secrets"></a>Implementación de almacenes de claves y secretos

Para crear almacenes de claves y agregar secretos, consulte:

- [Establecimiento y recuperación de un secreto mediante la CLI](../key-vault/quick-create-cli.md)
- [Establecimiento y recuperación de un secreto mediante Powershell](../key-vault/quick-create-powershell.md)
- [Establecimiento y recuperación de un secreto mediante Portal](../key-vault/quick-create-portal.md)
- [Establecimiento y recuperación de un secreto mediante .NET](../key-vault/quick-create-net.md)
- [Establecimiento y recuperación de un secreto mediante Node.js](../key-vault/quick-create-node.md)

Hay algunos requisitos y consideraciones adicionales respecto a la integración de Key Vault con la implementación de la plantilla de Resource Manager:

- `enabledForTemplateDeployment` es una propiedad de Key Vault. Para acceder a los secretos de esta instancia de Key Vault desde la implementación de Resource Manager, `enabledForTemplateDeployment` debe ser `true`. 
- Si no es el propietario del almacén de claves, este debe actualizar la configuración de la directiva de seguridad del almacén de claves para agregar secretos.

Los ejemplos de la CLI de Azure y Azure PowerShell siguientes muestran cómo hacerlo:

```azurecli
# Create a Key Vault
az keyvault create \
  --name $keyVaultName \
  --resource-group $resourceGroupName \
  --location $location \
  --enabled-for-template-deployment true
az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list
```

```azurepowershell
New-AzKeyVault `
  -VaultName $keyVaultName `
  -resourceGroupName $resourceGroupName `
  -Location $location `
  -EnabledForTemplateDeployment
Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list
```

## <a name="grant-access-to-the-secrets"></a>Concesión de acceso a los secretos

El usuario que implementa la plantilla debe tener el permiso `Microsoft.KeyVault/vaults/deploy/action` en el ámbito que contiene Key Vault, incluidos el grupo de recursos y Key Vault. Los roles [Propietario](../role-based-access-control/built-in-roles.md#owner) y [Colaborador](../role-based-access-control/built-in-roles.md#contributor) conceden este acceso. Si usted crea el almacén de claves, es el propietario, por lo que tiene el permiso. Si el almacén de claves está en una suscripción diferente, el propietario del almacén debe conceder el acceso.

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
    Reemplace "00000000-0000-0000-0000-000000000000" por el identificador de suscripción del usuario que necesita implementar las plantillas.

2. Cree el nuevo rol con el archivo JSON:

    ```azurepowershell
    $resourceGroupName= "<Resource Group Name>" # the resource group which contains the Key Vault
    $userPrincipalName = "<Email Address of the deployment operator>"
    New-AzRoleDefinition -InputFile "<PathToTheJSONFile>" 
    New-AzRoleAssignment -ResourceGroupName $resourceGroupName -RoleDefinitionName "Key Vault resource manager template deployment operator" -SignInName $userPrincipalName
    ```

    El ejemplo `New-AzRoleAssignment` asigna el rol personalizado al usuario en el nivel de grupo de recursos.  

Cuando se usa una instancia de Key Vault con la plantilla para una [aplicación administrada](../managed-applications/overview.md), debe conceder acceso a la entidad de servicio del **proveedor de recursos de dispositivo**. Para más información, consulte [Acceso al secreto de Key Vault al implementar Azure Managed Applications](../managed-applications/key-vault-access.md).

## <a name="reference-secrets-with-static-id"></a>Referencia a secretos con identificador estático

Con este enfoque, se hace referencia al almacén de claves del archivo de parámetros, no de la plantilla. La siguiente imagen muestra que el archivo de parámetros hace referencia al secreto y pasa dicho valor a la plantilla.

![Diagrama de identificador estático de integración de almacén de claves de Resource Manager](./media/resource-manager-keyvault-parameter/statickeyvault.png)

[Tutorial: Integración de Azure Key Vault en Template Deployment de Resource Manager](./resource-manager-tutorial-use-key-vault.md) usa este método. El tutorial implementa una máquina virtual que incluye una contraseña de administrador. El parámetro de contraseña se establece en una cadena segura:

![Archivo de la plantilla de identificador estático de integración de almacén de claves de Resource Manager](./media/resource-manager-keyvault-parameter/resource-manager-key-vault-static-id-template-file.png)

Ahora, cree un archivo de parámetros para la plantilla anterior. En el archivo de parámetros, especifique un parámetro que coincida con el nombre del parámetro de la plantilla. Para el valor del parámetro, haga referencia al secreto del almacén de claves. Se hace referencia al secreto pasando el identificador de recurso de almacén de claves y el nombre del secreto:

![Archivo del parámetro de identificador estático de integración de almacén de claves de Resource Manager](./media/resource-manager-keyvault-parameter/resource-manager-key-vault-static-id-parameter-file.png)

Si necesita utilizar una versión del secreto que no sea la versión actual, use la propiedad `secretVersion`.

```json
"secretName": "examplesecret",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Implemente la plantilla y pase el archivo de parámetros:

Para la CLI de Azure, utilice:

```azurecli
az group create --name $resourceGroupName --location $location
az group deployment create \
    --name $deploymentName \
    --resource-group $resourceGroupName \
    --template-uri <The Template File URI> \
    --parameters <The Parameter File>
```

Para PowerShell, use:

```powershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -Name $deploymentName `
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
    --name $deploymentName \
    --resource-group $resourceGroupName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json \
    --parameters vaultName=$keyVaultName vaultResourceGroupName=examplegroup secretName=examplesecret
```

Para PowerShell, use:

```powershell
New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
  -Name $deploymentName `
  -ResourceGroupName $resourceGroupName `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json `
  -vaultName $keyVaultName -vaultResourceGroupName $keyVaultResourceGroupName -secretName $secretName
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general sobre almacenes de claves, consulte el artículo de [introducción a Azure Key Vault](../key-vault/key-vault-get-started.md).
- Para obtener ejemplos completos de secretos de clave de referencia, consulte [Ejemplos de Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
