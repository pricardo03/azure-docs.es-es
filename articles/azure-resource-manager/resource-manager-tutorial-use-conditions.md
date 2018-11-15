---
title: Uso de condiciones en plantillas de Azure Resource Manager | Microsoft Docs
description: Aprenda a implementar recursos de Azure según condiciones.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 64891991a96395b712cb99850874d974f8451ce6
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612838"
---
# <a name="tutorial-use-condition-in-azure-resource-manager-templates"></a>Tutorial: Uso de condiciones en plantillas de Azure Resource Manager

Aprenda a implementar recursos de Azure según condiciones.

En el tutorial [Establecimiento del orden de implementación de los recursos](./resource-manager-tutorial-create-templates-with-dependent-resources.md), se crean una máquina virtual, una red virtual y algunos otros recursos dependientes incluidos en una cuenta de almacenamiento. En lugar de crear una nueva cuenta de almacenamiento, cada vez, dejará que la gente elija entre crear una nueva cuenta de almacenamiento y usar una existente. Para lograr este objetivo, definirá un parámetro adicional. Si el valor del parámetro es "new", se crea una nueva cuenta de almacenamiento.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Apertura de una plantilla de inicio rápido
> * Modificación de la plantilla
> * Implementación de la plantilla
> * Limpieza de recursos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/) con la [extensión Resource Manager Tools](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de máquina virtual. Este es un ejemplo para generar una contraseña:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault está diseñado para proteger las claves criptográficas y otros secretos. Para más información, consulte [Tutorial: Integración de Azure Key Vault en la implementación de la plantilla de Resource Manager](./resource-manager-tutorial-use-key-vault.md). También se recomienda actualizar la contraseña cada tres meses.

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

Las plantillas de inicio rápido de Azure consisten en un repositorio de plantillas de Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla que se usa en este tutorial se denomina [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.
4. La plantilla define cinco recursos:

    * `Microsoft.Storage/storageAccounts`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
    * `Microsoft.Network/publicIPAddresses`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
    * `Microsoft.Network/virtualNetworks`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
    * `Microsoft.Network/networkInterfaces`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
    * `Microsoft.Compute/virtualMachines`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    Resulta útil obtener cierta información básica de la plantilla antes de personalizarla.
5. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.

## <a name="modify-the-template"></a>Modificación de la plantilla

Realice dos cambios en la plantilla existente:

* Incorporación de un parámetro de nombre de cuenta de almacenamiento. Los usuarios pueden especificar un nuevo nombre de cuenta de almacenamiento o uno que ya exista.
* Agregue un nuevo parámetro llamado **newOrExisting**. En la implementación se usa este parámetro para determinar dónde crear una cuenta de almacenamiento o usar una cuenta de almacenamiento existente.

Éste es el procedimiento para realizar los cambios:

1. Abra **azuredeploy.json** en Visual Studio Code.
2. Reemplace **variables('storageAccountName')** por **parameters('storageAccountName')** en toda la plantilla.  Hay tres apariciones de **variables('storageAccountName')**.
3. Quite la siguiente definición de variable:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",
    ```
4. Agregue los dos parámetros siguientes a la plantilla:

    ```json
    "storageAccountName": {
      "type": "string"
    },
    "newOrExisting": {
      "type": "string", 
      "allowedValues": [
        "new", 
        "existing"
      ]
    },
    ```
    La definición de parámetros actualizada se parece a esta:

    ![Condición de uso de Resource Manager](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-parameters.png)

5. Agregue la siguiente línea al principio de la definición de la cuenta de almacenamiento.

    ```json
    "condition": "[equals(parameters('newOrExisting'),'new')]",
    ```

    La condición comprueba el valor de un parámetro llamado **newOrExisting**. Si el valor del parámetro es **new**, en la implementación se crea la cuenta de almacenamiento.

    La definición de la cuenta de almacenamiento actualizada se parece a esta:

    ![Condición de uso de Resource Manager](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template.png)
6. Actualice **storageUri** con el siguiente valor:

    ```json
    "storageUri": "[concat('https://', parameters('storageAccountName'), '.blob.core.windows.net')]"
    ```

    Este cambio es necesario cuando se usa una cuenta de almacenamiento existente en otro grupo de recursos.

7. Guarde los cambios.

## <a name="deploy-the-template"></a>Implementación de la plantilla

Para implementar la plantilla, siga las instrucciones que se indican en [Implementación de la plantilla](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template).

Al implementar la plantilla mediante Azure PowerShell, es preciso especificar un parámetro adicional. Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de máquina virtual. Consulte [Requisitos previos](#prerequisites).

```azurepowershell
$deploymentName = Read-Host -Prompt "Enter the name for this deployment"
$resourceGroupName = Read-Host -Prompt "Enter the resource group name"
$storageAccountName = Read-Host -Prompt "Enter the storage account name"
$newOrExisting = Read-Host -Prompt "Create new or use existing (Enter new or existing)"
$location = Read-Host -Prompt "Enter the Azure location (i.e. centralus)"
$vmAdmin = Read-Host -Prompt "Enter the admin username"
$vmPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
$dnsLabelPrefix = Read-Host -Prompt "Enter the DNS Label prefix"

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
New-AzureRmResourceGroupDeployment -Name $deploymentName `
    -ResourceGroupName $resourceGroupName `
    -adminUsername $vmAdmin `
    -adminPassword $vmPassword `
    -dnsLabelPrefix $dnsLabelPrefix `
    -storageAccountName $storageAccountName `
    -newOrExisting $newOrExisting `
    -TemplateFile azuredeploy.json
```

> [!NOTE]
> Se produce un error en la implementación si **newOrExisting** es **new**, pero la cuenta de almacenamiento con el nombre especificado ya existe.

Intente otra implementación con **newOrExisting** establecido en "existing" y especifique una cuenta de almacenamiento existente. Para crear una cuenta de almacenamiento con antelación, consulte [Creación de una cuenta de almacenamiento](../storage/common/storage-quickstart-create-account.md).

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá un total de seis recursos en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha desarrollado una plantilla que permite a los usuarios elegir entre crear una cuenta de almacenamiento y usar una existente. Para aprender a recuperar secretos de Azure Key Vault y usar los secretos como contraseñas en la implementación de plantillas, consulte:

> [!div class="nextstepaction"]
> [Integrate Key Vault in template deployment](./resource-manager-tutorial-use-key-vault.md) (Integración de Key Vault en la implementación de plantillas)
