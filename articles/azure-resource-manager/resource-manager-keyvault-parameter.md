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
ms.date: 10/10/2018
ms.author: tomfitz
ms.openlocfilehash: e04ee6b74be0bb7ec81166f43295a8a9377f8cf8
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54381517"
---
# <a name="use-azure-key-vault-to-pass-secure-parameter-value-during-deployment"></a>Uso de Azure Key Vault para pasar el valor de parámetro seguro durante la implementación

Cuando tiene que pasar un valor seguro (por ejemplo, una contraseña) como un parámetro durante la implementación, puede recuperar el valor de [Azure Key Vault](../key-vault/key-vault-whatis.md). El valor se recupera haciendo referencia a Key Vault y al secreto del archivo de parámetros. El valor nunca se expone debido a que solo hace referencia a su identificador de almacén de claves. Key Vault puede existir en una suscripción diferente en la que está implementando el grupo de recursos.

## <a name="deploy-a-key-vault-and-secret"></a>Implementación de un almacén de claves y un secreto

Para crear un almacén de claves y un secreto, use la CLI de Azure o PowerShell. `enabledForTemplateDeployment` es una propiedad de Key Vault. Para acceder a los secretos de esta instancia de Key Vault desde la implementación de Resource Manager, `enabledForTemplateDeployment` debe ser `true`. 

Los siguientes scripts de ejemplo de Azure PowerShell y la CLI de Azure muestran cómo crear un almacén de claves y un secreto.

Para la CLI de Azure, utilice:

```azurecli-interactive
keyVaultName='{your-unique-vault-name}'
resourceGroupName='{your-resource-group-name}'
location='centralus'
userPrincipalName='{your-email-address-associated-with-your-subscription}'

# Create a resource group
az group create --name $resourceGroupName --location $location

# Create a Key Vault
az keyvault create \
  --name $keyVaultName \
  --resource-group $resourceGroupName \
  --location $location \
  --enabled-for-template-deployment true
az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list

# Create a secret with the name, vmAdminPassword
password=$(openssl rand -base64 32)
echo $password
az keyvault secret set --vault-name $keyVaultName --name 'vmAdminPassword' --value $password
```

Para PowerShell, use:

```azurepowershell-interactive
$keyVaultName = "{your-unique-vault-name}"
$resourceGroupName="{your-resource-group-name}"
$location='Central US'
$userPrincipalName='{your-email-address-associated-with-your-subscription}'

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location

New-AzureRmKeyVault `
  -VaultName $keyVaultName `
  -resourceGroupName $resourceGroupName `
  -Location $location `
  -EnabledForTemplateDeployment
Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list

$password = openssl rand -base64 32
echo $password
$secretvalue = ConvertTo-SecureString $password -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name "vmAdminPassword" -SecretValue $secretvalue
```

Si se ejecuta el script de PowerShell fuera de Cloud Shell, use el siguiente comando para generar la contraseña en su lugar:

```powershell
Add-Type -AssemblyName System.Web
[System.Web.Security.Membership]::GeneratePassword(16,3)
```

Si quiere usar la plantilla de Resource Manager: Vea [Tutorial: Integración de Azure Key Vault en Resource Manager Template Deployment](./resource-manager-tutorial-use-key-vault.md#prepare-the-key-vault).

> [!NOTE]
> Cada servicio de Azure tiene unos requisitos de contraseña concretos. Por ejemplo, los requisitos de la máquina virtual de Azure se encuentran en [¿Cuáles son los requisitos de contraseña al crear una máquina virtual?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="enable-access-to-the-secret"></a>Habilitación del acceso al secreto

Además de establecer `enabledForTemplateDeployment` en `true`, el usuario que implementa la plantilla debe tener el permiso `Microsoft.KeyVault/vaults/deploy/action` en el ámbito que contiene el almacén de claves, incluidos el grupo de recursos y almacén de claves. Los roles [Propietario](../role-based-access-control/built-in-roles.md#owner) y [Colaborador](../role-based-access-control/built-in-roles.md#contributor) conceden este acceso. Si usted crea el almacén de claves, es el propietario, por lo que tiene el permiso. Si el almacén de claves está en una suscripción diferente, el propietario del almacén debe conceder el acceso.

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
    New-AzureRmRoleDefinition -InputFile "<PathToTheJSONFile>" 
    New-AzureRmRoleAssignment -ResourceGroupName $resourceGroupName -RoleDefinitionName "Key Vault resource manager template deployment operator" -SignInName $userPrincipalName
    ```

    El ejemplo `New-AzureRmRoleAssignment` asigna el rol personalizado al usuario en el nivel de grupo de recursos.  

Cuando se usa una instancia de Key Vault con la plantilla para una [aplicación administrada](../managed-applications/overview.md), debe conceder acceso a la entidad de servicio del **proveedor de recursos de dispositivo**. Para más información, consulte [Acceso al secreto de Key Vault al implementar Azure Managed Applications](../managed-applications/key-vault-access.md).

## <a name="reference-a-secret-with-static-id"></a>Referencia a un secreto con identificador estático

La plantilla que recibe un secreto de Key Vault es como cualquier otra plantilla. Esto se debe a que **se hace referencia al almacén de claves del archivo de parámetros, no de la plantilla**. La siguiente imagen muestra que el archivo de parámetros hace referencia al secreto y pasa dicho valor a la plantilla.

![Identificador estático](./media/resource-manager-keyvault-parameter/statickeyvault.png)

Por ejemplo, la [siguiente plantilla](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.json) implementa una base de datos SQL que incluye una contraseña de administrador. El parámetro de contraseña se establece en una cadena segura. Pero la plantilla no especifica de dónde procede ese valor.

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

Ahora, cree un archivo de parámetros para la plantilla anterior. En el archivo de parámetros, especifique un parámetro que coincida con el nombre del parámetro de la plantilla. Para el valor del parámetro, haga referencia al secreto del almacén de claves. Se hace referencia al secreto pasando el identificador de recurso de almacén de claves y el nombre del secreto. En el [siguiente archivo de parámetros](https://github.com/Azure/azure-docs-json-samples/blob/master/azure-resource-manager/keyvaultparameter/sqlserver.parameters.json) debe existir el secreto del almacén de claves y el usuario debe indicar un valor estático para su identificador de recurso. Copie este archivo localmente y establezca el identificador de suscripción, el nombre de almacén y el nombre de servidor SQL Server.

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
                "id": "/subscriptions/<subscription-id>/resourceGroups/examplegroup/providers/Microsoft.KeyVault/vaults/<vault-name>"
              },
              "secretName": "examplesecret"
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
"secretName": "examplesecret",
"secretVersion": "cd91b2b7e10e492ebb870a6ee0591b68"
```

Ahora, implemente la plantilla y pase el archivo de parámetros. Puede utilizar la plantilla de ejemplo de GitHub, pero debe usar un archivo de parámetros local con los valores establecidos para su entorno.

Para la CLI de Azure, utilice:

```azurecli-interactive
az group create --name datagroup --location $location
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json \
    --parameters @sqlserver.parameters.json
```

Para PowerShell, use:

```powershell-interactive
New-AzureRmResourceGroup -Name datagroup -Location $location
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json `
  -TemplateParameterFile sqlserver.parameters.json
```

## <a name="reference-a-secret-with-dynamic-id"></a>Referencia a un secreto con identificador dinámico

En la sección anterior se mostró cómo pasar un identificador de recurso estático para el secreto del almacén de claves con el parámetro. Sin embargo, en algunos escenarios, debe hacer referencia a un secreto del Almacén de claves que varía en función de la implementación actual. O bien, puede simplemente pasar valores de parámetros a la plantilla en lugar de crear un parámetro de referencia en el archivo de parámetros. En cualquier caso, puede generar dinámicamente el identificador de recurso de un secreto del almacén de claves mediante el uso de una plantilla vinculada.

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

```azurecli-interactive
az group create --name datagroup --location $location
az group deployment create \
    --name exampledeployment \
    --resource-group datagroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json \
    --parameters vaultName=<your-vault> vaultResourceGroupName=examplegroup secretName=examplesecret
```

Para PowerShell, use:

```powershell
New-AzureRmResourceGroup -Name datagroup -Location $location
New-AzureRmResourceGroupDeployment `
  -Name exampledeployment `
  -ResourceGroupName datagroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-key-vault-use-dynamic-id/azuredeploy.json `
  -vaultName <your-vault> -vaultResourceGroupName examplegroup -secretName examplesecret
```

## <a name="next-steps"></a>Pasos siguientes
* Para obtener información general sobre almacenes de claves, consulte el artículo de [introducción a Azure Key Vault](../key-vault/key-vault-get-started.md).
* Para obtener ejemplos completos de secretos de clave de referencia, consulte [Ejemplos de Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).