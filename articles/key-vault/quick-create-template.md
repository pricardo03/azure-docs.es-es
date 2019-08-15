---
title: 'Inicio rápido de Azure: Creación de un almacén de claves de Azure y un secreto mediante la plantilla de Azure Resource Manager | Microsoft Docs'
description: Inicio rápido que muestra cómo crear almacenes de claves de Azure y agregar secretos a los almacenes mediante una plantilla de Azure Resource Manager.
services: key-vault
author: mumian
manager: dougeby
tags: azure-resource-manager
ms.service: key-vault
ms.topic: quickstart
ms.custom: mvc
ms.date: 05/22/2019
ms.author: jgao
ms.openlocfilehash: b27caa3d91d67ad63bfbf5e7c549d690980cdd7b
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68934436"
---
# <a name="quickstart-set-and-retrieve-a-secret-from-azure-key-vault-using-resource-manager-template"></a>Inicio rápido: Establecimiento y recuperación de un secreto de Azure Key Vault mediante una plantilla de Resource Manager

[Azure Key Vault](./key-vault-overview.md) es un servicio en la nube que proporciona un almacén seguro para secretos, como claves, contraseñas y certificados, entre otros. Este inicio rápido se centra en el proceso de implementación de una plantilla de Resource Manager para crear un almacén de claves y un secreto. Para más información sobre el desarrollo de plantillas de Resource Manager, consulte la [documentación de Resource Manager](/azure/azure-resource-manager/) y la [referencia de plantilla](/azure/templates/microsoft.keyvault/allversions).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* La plantilla necesita el identificador de objeto de usuario de Azure AD para configurar los permisos. El procedimiento siguiente permite obtener el identificador de objeto (GUID).

    1. Ejecute el siguiente comando de Azure PowerShell o la CLI de Azure, para lo que debe seleccionar **Pruébelo** y, a continuación, pegar el script en el panel de shell. Para pegar el script, haga clic con el botón derecho en el shell y, a continuación, seleccione **Pegar**. 

        ```azurecli-interactive
        echo "Enter your email address that is used to sign in to Azure:" &&
        read upn &&
        az ad user show --upn-or-object-id $upn --query "objectId" 
        ```

        ```azurepowershell-interactive
        $upn = Read-Host -Prompt "Enter your email address used to sign in to Azure"
        (Get-AzADUser -UserPrincipalName $upn).Id
        ```

    2. Anote el identificador de objeto. Lo necesitará en la sección siguiente de este inicio rápido.

## <a name="create-a-vault-and-a-secret"></a>Creación de un almacén y un secreto

La plantilla usada en este inicio rápido forma parte de las [plantillas de inicio rápido de Azure](https://azure.microsoft.com/resources/templates/101-key-vault-create/). Puede encontrar más ejemplos de plantillas de Azure Key Vault [aquí](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Keyvault).

1. Seleccione la imagen siguiente para iniciar sesión en Azure y abrir una plantilla. La plantilla crea un almacén de claves y un secreto.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-key-vault-create%2Fazuredeploy.json"><img src="./media/quick-create-template/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Seleccione o escriba los siguientes valores.  

    ![Portal de implementación de la integración de Key Vault en la plantilla de Resource Manager](./media/quick-create-template/create-key-vault-using-template-portal.png)

    A menos que se especifique, utilice el valor predeterminado para crear el almacén de claves y un secreto.

    * **Suscripción**: seleccione una suscripción de Azure.
    * **Grupo de recursos**: seleccione **Crear nuevo**, escriba un nombre único para el grupo de recursos y, a continuación, haga clic en **Aceptar**. 
    * **Ubicación**: seleccione una ubicación.  Por ejemplo, **Centro de EE. UU**.
    * **Nombre del almacén de claves**: escriba un nombre para el almacén de claves que debe ser globalmente único dentro del espacio de nombres .vault.azure.net.  
    * **Id. de inquilino**: la función de plantilla recupera automáticamente el identificador del inquilino.  No cambie el valor predeterminado.
    * **Id. de usuario de AD**: escriba el identificador del objeto de usuario de Azure AD que recuperó de [Requisitos previos](#prerequisites).
    * **Nombre del secreto**: escriba un nombre para el secreto que almacenará en el almacén de claves.  Por ejemplo, **adminpassword**.
    * **Valor del secreto**: escriba el valor del secreto.  Si almacena una contraseña, se recomienda usar la contraseña generada que creó en Requisitos previos.
    * **Acepto los términos y condiciones anteriores**: Seleccionar.
3. Seleccione **Comprar**.

## <a name="validate-the-deployment"></a>Validación de la implementación

Puede usar Azure Portal para comprobar el almacén de claves y el secreto o usar el siguiente script de la CLI de Azure o Azure PowerShell para enumerar el secreto que ha creado.

```azurecli-interactive
echo "Enter your key vault name:" &&
read keyVaultName &&
az keyvault secret list --vault-name $keyVaultName
```

```azurepowershell-interactive
$keyVaultName = Read-Host -Prompt "Enter your key vault name"
Get-AzKeyVaultSecret -vaultName $keyVaultName
```

## <a name="clean-up-resources"></a>Limpieza de recursos

Otras guías de inicio rápido y tutoriales de Key Vault se basan en esta. Si tiene pensado seguir trabajando en otras guías de inicio rápido y tutoriales, considere la posibilidad de dejar estos recursos activos.
Cuando ya no lo necesite, elimine el grupo de recursos; de este modo se eliminarán también Key Vault y los recursos relacionados. Para eliminar el grupo de recursos mediante Azure PowerShell o la CLI de Azure:

```azurecli-interactive
echo "Enter the Resource Group name:" &&
read resourceGroupName &&
az group delete --name $resourceGroupName 
```
```azurepowershell-interactive
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
Remove-AzResourceGroup -Name $resourceGroupName 
```

## <a name="next-steps"></a>Pasos siguientes

* [Página principal de Azure Key Vault](https://azure.microsoft.com/services/key-vault/)
* [Documentación de Azure Key Vault](index.yml)
* [Azure SDK para Node](https://docs.microsoft.com/javascript/api/overview/azure/key-vault)
* [Azure REST API Reference](https://docs.microsoft.com/rest/api/keyvault/) (Referencia de API de REST en Azure)
