---
title: Integración de Azure Key Vault en la implementación de la plantilla de Resource Manager | Microsoft Docs
description: Aprenda a usar Azure Key Vault para pasar valores de parámetros seguros durante la implementación de la plantilla de Resource Manager
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 05/23/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 0d78e6eaca708073c3a216507b320fe8783a25b6
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239236"
---
# <a name="tutorial-integrate-azure-key-vault-in-resource-manager-template-deployment"></a>Tutorial: Integración de Azure Key Vault en la implementación de la plantilla de Resource Manager

Aprenda a recuperar los secretos de Azure Key Vault y a usarlos como parámetros durante la implementación de Resource Manager. El valor nunca se expone debido a que solo hace referencia a su identificador de almacén de claves. Para más información, consulte [Uso de Azure Key Vault para pasar el valor de parámetro seguro durante la implementación](./resource-manager-keyvault-parameter.md)

En el tutorial [Establecimiento del orden de implementación de los recursos](./resource-manager-tutorial-create-templates-with-dependent-resources.md), cree una máquina virtual. Tendrá que proporcionar el nombre de usuario y la contraseña del administrador de la máquina virtual. En lugar de proporcionar la contraseña, puede almacenar previamente la contraseña en Azure Key Vault y, a continuación, personalizar la plantilla para recuperar la contraseña desde el almacén de claves durante la implementación.

![Diagrama de la integración de Key Vault en la plantilla de Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Preparación de un almacén de claves
> * Apertura de una plantilla de inicio rápido
> * Edición del archivo de parámetros
> * Implementación de la plantilla
> * Validación de la implementación
> * Limpieza de recursos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/) con la [extensión Resource Manager Tools](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de máquina virtual. Este es un ejemplo para generar una contraseña:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Compruebe que la contraseña generada reúne los requisitos de las contraseñas de máquinas virtuales. Cada servicio de Azure tiene unos requisitos de contraseña concretos. Para los requisitos de contraseña de una máquina virtual, consulte [¿Cuáles son los requisitos de contraseña cuando se crea una VM?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

## <a name="prepare-a-key-vault"></a>Preparación de un almacén de claves

En esta sección, creará un almacén de claves y agregará un secreto al almacén de claves, de manera que pueda recuperar el secreto al implementar la plantilla. Un almacén de claves se puede crear de muchas maneras. En este tutorial, se usa Azure PowerShell para implementar una [plantilla de Resource Manager](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json). Esta plantilla:

* Cree un almacén de claves con la propiedad `enabledForTemplateDeployment` habilitada. El valor de esta propiedad debe ser true para que el proceso de implementación de plantilla pueda acceder a los secretos definidos en este almacén de claves.
* Agregue un secreto al almacén de claves.  El secreto almacena la contraseña del administrador de la máquina virtual.

> [!NOTE]
> Si usted (como usuario que va a implementar la plantilla de máquina virtual) no es propietario ni colaborador del almacén de claves, para poder acceder a Microsoft.KeyVault/vaults/deploy/action el propietario o colaborador debe concederle el permiso pertinente. Para más información, consulte [Uso de Azure Key Vault para pasar el valor de parámetro seguro durante la implementación](./resource-manager-keyvault-parameter.md)

Para ejecutar el siguiente script de PowerShell, seleccione **Pruébelo** para abrir Cloud Shell. Para pegar el script, haga clic con el botón derecho en el panel del shell y, a continuación, seleccione **Pegar**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name that is used for generating resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$upn = Read-Host -Prompt "Enter your user principal name (email address) used to sign in to Azure"
$secretValue = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$keyVaultName = $projectName
$adUserId = (Get-AzADUser -UserPrincipalName $upn).Id
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/tutorials-use-key-vault/CreateKeyVault.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -keyVaultName $keyVaultName -adUserId $adUserId -secretValue $secretValue
```

Algunos datos importantes:

* El nombre del grupo de recursos es el nombre del proyecto con **rg** anexado. Para que resulte más fácil [limpiar los recursos creados en este tutorial](#clean-up-resources), use el mismo nombre de proyecto y de grupo de recursos al [implementar la siguiente plantilla](#deploy-the-template).
* El nombre predeterminado para el secreto es **vmAdminPassword**. Está codificado de forma rígida en la plantilla.
* Para que la plantilla pueda recuperar el secreto, debe habilitar una directiva de acceso llamada **Habilitar el acceso a Azure Resource Manager para la implementación de plantillas** para el almacén de claves. Esta directiva está habilitada en la plantilla. Para obtener más información acerca de esta directiva de acceso, consulte [Implementación de almacenes de claves y secretos](./resource-manager-keyvault-parameter.md#deploy-key-vaults-and-secrets).

La plantilla tiene un valor de salida denominado **keyVaultId**. Anote el valor. Este identificador es necesario cuando se implementa la máquina virtual. El formato del identificador de recurso es:

```json
/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
```

Cuando copie y pegue el identificador, este puede dividirse en varias líneas. Debe combinar las líneas y recortar los espacios adicionales.

Para validar la implementación, ejecute el siguiente comando de PowerShell en el mismo panel de shell para recuperar el secreto en texto no cifrado. El comando solo funciona en la misma sesión de shell porque usa una variable $keyVaultName definida en el script de PowerShell anterior.

```azurepowershell
(Get-AzKeyVaultSecret -vaultName $keyVaultName  -name "vmAdminPassword").SecretValueText
```

Ahora que ha preparado un almacén de claves y un secreto, las secciones siguientes muestran cómo personalizar una plantilla existente para recuperar el secreto durante la implementación.

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

Las plantillas de inicio rápido de Azure consisten en un repositorio de plantillas de Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla que se usa en este tutorial se denomina [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```

3. Seleccione **Abrir** para abrir el archivo. Es el mismo escenario que se ha usado en [Tutorial: Creación de plantillas de Azure Resource Manager con recursos dependientes](./resource-manager-tutorial-create-templates-with-dependent-resources.md).
4. La plantilla define cinco recursos:

   * `Microsoft.Storage/storageAccounts`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     Resulta útil obtener cierta información básica de la plantilla antes de personalizarla.
5. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.
6. Repita los pasos 1 a 4 para abrir la dirección URL siguiente y, después, guarde el archivo como **azuredeploy.parameters.json**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Edición del archivo de parámetros

No es necesario hacer ningún cambio en el archivo de plantilla.

1. Abra **azuredeploy.parameters.json** en Visual Studio Code, en caso de que no esté abierto.
2. Actualice el parámetro **adminPassword** a:

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```

    > [!IMPORTANT]
    > Reemplace el valor de **id** por el identificador del recurso del almacén de claves que creó en el último procedimiento.

    ![Integrar el almacén de claves y el archivo de parámetros de implementación de máquina virtual de la plantilla de Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Asigne los valores a:

    * **adminUsername**: nombre de la cuenta de administrador de la máquina virtual.
    * **dnsLabelPrefix**: nombre de dnsLabelPrefix.

    Vea un ejemplo en la captura de pantalla anterior.

4. Guarde los cambios.

## <a name="deploy-the-template"></a>Implementación de la plantilla

Para implementar la plantilla, siga las instrucciones que se indican en [Implementación de la plantilla](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template). Es preciso cargar **azuredeploy.json** y **azuredeploy.parameters.json** en el shell en la nube y, después, usar el siguiente script de PowerShell para implementar la plantilla:

```azurepowershell
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$location = Read-Host -Prompt "Enter the same location that is used for creating the key vault (i.e. centralus)"
$resourceGroupName = "${projectName}rg"

New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "$HOME/azuredeploy.json" `
    -TemplateParameterFile "$HOME/azuredeploy.parameters.json"
```

Al implementar la plantilla, use el mismo grupo de recursos que el almacén de claves. Esto facilita la limpieza de los recursos. Solo hay que eliminar un grupo de recursos, en lugar de dos.

## <a name="validate-the-deployment"></a>Validación de la implementación

Cuando haya implementado correctamente la máquina virtual, use la contraseña almacenada en el almacén de claves para probar el inicio de sesión.

1. Abra [Azure Portal](https://portal.azure.com).
2. Seleccione **Grupos de recursos**/**NombreDeSuGrupoDeRecursos>** /**simpleWinVM**
3. Seleccione **conectar** en la parte superior.
4. Seleccione **Descargar archivo RDP** y siga las instrucciones para iniciar sesión en la máquina virtual con la contraseña almacenada en el almacén de claves.

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter the same project name that is used for creating the key vault"
$resourceGroupName = "${projectName}rg"

Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha recuperado un secreto de Azure Key Vault y lo ha usado en la implementación de una plantilla.  Para aprender a crear plantillas vinculadas, consulte:

> [!div class="nextstepaction"]
> [Creación de plantillas vinculadas](./resource-manager-tutorial-create-linked-templates.md)
