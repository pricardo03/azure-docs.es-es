---
title: 'Referencias de Key Vault: Azure App Service | Microsoft Docs'
description: Guía de configuración y referencia conceptual para referencias de Azure Key Vault en Azure App Service y Azure Functions
services: app-service
author: mattchenderson
manager: jeconnoc
editor: ''
ms.service: app-service
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 11/20/2018
ms.author: mahender
ms.custom: seodec18
ms.openlocfilehash: e7a049c8def0a5014aeb8a0e7a16aaa8def28009
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705697"
---
# <a name="use-key-vault-references-for-app-service-and-azure-functions-preview"></a>Uso de referencias de Key Vault para App Service y Azure Functions (versión preliminar)

> [!NOTE] 
> Las referencias de Key Vault están actualmente en versión preliminar.

En este tema se muestra cómo trabajar con secretos de Azure Key Vault en la aplicación de App Service o Azure Functions sin necesidad de realizar cambios en el código. [Azure Key Vault](../key-vault/key-vault-overview.md) es un servicio que proporciona administración centralizada de los secretos, con control total sobre las directivas de acceso y el historial de auditoría.

## <a name="granting-your-app-access-to-key-vault"></a>Concesión a la aplicación de acceso a Key Vault

Para leer secretos desde Key Vault, debe tener creado un almacén y proporcionar a la aplicación permiso para acceder a él.

1. Para crear un almacén de claves, siga la [guía de inicio rápido de Key Vault](../key-vault/quick-create-cli.md).

1. Cree una [identidad administrada asignada por el sistema](overview-managed-identity.md) para la aplicación.

   > [!NOTE] 
   > Las referencias de Key Vault solo admiten actualmente identidades administradas asignadas por el sistema. No se pueden usar identidades asignadas por el usuario.

1. Cree una [directiva de acceso en Key Vault](../key-vault/key-vault-secure-your-key-vault.md#key-vault-access-policies) para la identidad de aplicación que creó anteriormente. Habilite el permiso secreto "Get" en esta directiva. No configure la "aplicación autorizada" o la configuración `applicationId`, ya que no es compatible con una identidad administrada.

    La operación para conceder acceso a una identidad de aplicación del almacén de claves solo se realiza una vez, por lo que será igual en todas las suscripciones de Azure. Puede usarlo para implementar tantos certificados como desee. 

## <a name="reference-syntax"></a>Sintaxis de referencia

Una referencia de Key Vault tiene el formato `@Microsoft.KeyVault({referenceString})`, donde `{referenceString}` se sustituye por una de las opciones siguientes:

> [!div class="mx-tdBreakAll"]
> | Cadena de referencia                                                            | DESCRIPCIÓN                                                                                                                                                                                 |
> |-----------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | SecretUri=_secretUri_                                                       | **SecretUri** debe ser el URI completo del plano de datos de un secreto en Key Vault, incluida una versión, por ejemplo, https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931.  |
> | VaultName=_vaultName_;SecretName=_secretName_;SecretVersion=_secretVersion_ | **VaultName** debe ser el nombre del recurso de Key Vault. **SecretName** debe ser el nombre del secreto de destino. **SecretVersion** debe ser la versión del secreto que se va a usar. |

> [!NOTE] 
> En la versión preliminar actual, se requieren versiones. Al rotar secretos, deberá actualizar la versión en la configuración de la aplicación.

Por ejemplo, una referencia completa se parecería a la siguiente:

```
@Microsoft.KeyVault(SecretUri=https://myvault.vault.azure.net/secrets/mysecret/ec96f02080254f109c51a1f14cdb1931)
```

O bien:

```
@Microsoft.KeyVault(VaultName=myvault;SecretName=mysecret;SecretVersion=ec96f02080254f109c51a1f14cdb1931)
```


## <a name="source-application-settings-from-key-vault"></a>Configuración de la aplicación de origen desde Key Vault

Las referencias de Key Vault pueden usarse como valores en [Configuración de la aplicación](configure-common.md#configure-app-settings), lo que le permite conservar los secretos en Key Vault en lugar de en la configuración del sitio. La configuración de la aplicación se cifra de forma segura en reposo, pero si necesita funcionalidades de administración de secretos, debe incluirse en Key Vault.

Para usar una referencia de Key Vault para la configuración de la aplicación, establezca la referencia como el valor de la configuración. La aplicación puede hacer referencia al secreto mediante su clave de la forma habitual. No se requiere ningún cambio de código.

> [!TIP]
> La mayoría de los valores de configuración de la aplicación que usan referencias de Key Vault se deben marcar como configuración de ranuras, así que debe tener distintos almacenes para cada entorno.

### <a name="azure-resource-manager-deployment"></a>Implementación de Azure Resource Manager

Al automatizar las implementaciones de recursos mediante plantillas de Azure Resource Manager, puede que deba secuenciar sus dependencias en un orden determinado para que esta característica funcione. Cabe señalar que deberá definir la configuración de la aplicación como su propio recurso en lugar de usar una propiedad `siteConfig` en la definición del sitio. El motivo es que primero es necesario definir el sitio para que la identidad asignada por el sistema se cree con él y se pueda usar en la directiva de acceso.

Una seudoplantilla de ejemplo para una aplicación de función podría tener este aspecto:

```json
{
    //...
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[variables('storageAccountName')]",
            //...
        },
        {
            "type": "Microsoft.Insights/components",
            "name": "[variables('appInsightsName')]",
            //...
        },
        {
            "type": "Microsoft.Web/sites",
            "name": "[variables('functionAppName')]",
            "identity": {
                "type": "SystemAssigned"
            },
            //...
            "resources": [
                {
                    "type": "config",
                    "name": "appsettings",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('storageConnectionStringName'))]",
                        "[resourceId('Microsoft.KeyVault/vaults/secrets', variables('keyVaultName'), variables('appInsightsKeyName'))]"
                    ],
                    "properties": {
                        "AzureWebJobsStorage": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_CONTENTAZUREFILECONNECTIONSTRING": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('storageConnectionStringResourceId')).secretUriWithVersion, ')')]",
                        "APPINSIGHTS_INSTRUMENTATIONKEY": "[concat('@Microsoft.KeyVault(SecretUri=', reference(variables('appInsightsKeyResourceId')).secretUriWithVersion, ')')]",
                        "WEBSITE_ENABLE_SYNC_UPDATE_SITE": "true"
                        //...
                    }
                },
                {
                    "type": "sourcecontrols",
                    "name": "web",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]",
                        "[resourceId('Microsoft.Web/sites/config', variables('functionAppName'), 'appsettings')]"
                    ],
                }
            ]
        },
        {
            "type": "Microsoft.KeyVault/vaults",
            "name": "[variables('keyVaultName')]",
            //...
            "dependsOn": [
                "[resourceId('Microsoft.Web/sites', variables('functionAppName'))]"
            ],
            "properties": {
                //...
                "accessPolicies": [
                    {
                        "tenantId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').tenantId]",
                        "objectId": "[reference(concat('Microsoft.Web/sites/',  variables('functionAppName'), '/providers/Microsoft.ManagedIdentity/Identities/default'), '2015-08-31-PREVIEW').principalId]",
                        "permissions": {
                            "secrets": [ "get" ]
                        }
                    }
                ]
            },
            "resources": [
                {
                    "type": "secrets",
                    "name": "[variables('storageConnectionStringName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
                    ],
                    "properties": {
                        "value": "[concat('DefaultEndpointsProtocol=https;AccountName=', variables('storageAccountName'), ';AccountKey=', listKeys(variables('storageAccountResourceId'),'2015-05-01-preview').key1)]"
                    }
                },
                {
                    "type": "secrets",
                    "name": "[variables('appInsightsKeyName')]",
                    //...
                    "dependsOn": [
                        "[resourceId('Microsoft.KeyVault/vaults/', variables('keyVaultName'))]",
                        "[resourceId('Microsoft.Insights/components', variables('appInsightsName'))]"
                    ],
                    "properties": {
                        "value": "[reference(resourceId('microsoft.insights/components/', variables('appInsightsName')), '2015-05-01').InstrumentationKey]"
                    }
                }
            ]
        }
    ]
}
```

> [!NOTE] 
> En este ejemplo, la implementación del control de código fuente depende de la configuración de la aplicación. Este comportamiento es normalmente poco seguro, ya que la actualización de la configuración de la aplicación se comporta de forma asincrónica. Sin embargo, como hemos incluido la configuración de la aplicación `WEBSITE_ENABLE_SYNC_UPDATE_SITE`, la actualización es sincrónica. Esto significa que la implementación del control de código fuente solo comenzará una vez que la configuración de la aplicación se haya actualizado completamente.
