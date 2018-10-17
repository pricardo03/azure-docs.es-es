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
ms.date: 10/02/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 216e474f519e57352b017dc3e6bcdd74d48b03de
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238653"
---
# <a name="tutorial-use-condition-in-azure-resource-manager-templates"></a>Tutorial: Uso de condiciones en plantillas de Azure Resource Manager

Aprenda a implementar recursos de Azure según condiciones. 

El escenario que se usa en este tutorial es parecido al utilizado en el [Tutorial: Creación de plantillas de Azure Resource Manager con recursos dependientes](./resource-manager-tutorial-create-templates-with-dependent-resources.md). En este tutorial, creará una cuenta de almacenamiento, una máquina virtual, una red virtual y algunos otros recursos dependientes. En lugar de crear una nueva cuenta de almacenamiento, dejará que la gente elija entre crear una nueva cuenta de almacenamiento y usar una cuenta de almacenamiento existente. Para lograr este objetivo, definirá un parámetro adicional. Si el valor del parámetro es "new", se crea una nueva cuenta de almacenamiento.

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Apertura de una plantilla de inicio rápido
> * Modificación de la plantilla
> * Implementación de la plantilla
> * Limpieza de recursos

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/) con la [extensión de Herramientas de Resource Manager](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

Las plantillas de inicio rápido de Azure consisten en un repositorio de plantillas de Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla que se usa en este tutorial se denomina [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.
4. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.

## <a name="modify-the-template"></a>Modificación de la plantilla

Realice dos cambios en la plantilla existente:

* Agregue un parámetro que se usa para proporcionar un nombre de cuenta de almacenamiento. Este parámetro proporciona al usuario la opción de especificar un nombre de cuenta de almacenamiento existente. También se puede usar como el nuevo nombre de cuenta de almacenamiento.
* Agregue un nuevo parámetro llamado **newOrExisting**. En la implementación se usa este parámetro para determinar dónde crear una cuenta de almacenamiento o usar una cuenta de almacenamiento existente.

1. Abra **azuredeploy.json** en Visual Studio Code.
2. Reemplace **variables('storageAccountName')** por **parameters('storageAccountName')** en toda la plantilla.  Hay tres apariciones de **variables('storageAccountName')**.
3. Quite la siguiente definición de variable:

    ```json
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'sawinvm')]",
    ```
4. Agregue los dos parámetros siguientes a la plantilla:

    ```json
    "newOrExisting": {
      "type": "string"
    },
    "storageAccountName": {
      "type": "string"
    },
    ```
    La definición de parámetros actualizada se parece a esta:

    ![Condición de uso de Resource Manager](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template-parameters.png)

5. Agregue la siguiente línea al principio de la definición de la cuenta de almacenamiento.

    ```json
    "condition": "[equals(parameters('newOrExisting'),'yes')]",
    ```

    La condición comprueba el valor de un parámetro llamado **newOrExisting**. Si el valor del parámetro es **new**, en la implementación se crea la cuenta de almacenamiento.

    La definición de la cuenta de almacenamiento actualizada se parece a esta:

    ![Condición de uso de Resource Manager](./media/resource-manager-tutorial-use-conditions/resource-manager-tutorial-use-condition-template.png)

6. Guarde los cambios.

## <a name="deploy-the-template"></a>Implementación de la plantilla

Para implementar la plantilla, siga las instrucciones que se indican en [Implementación de la plantilla](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template).

Al implementar la plantilla mediante Azure PowerShell, deberá especificar un parámetro adicional:

```powershell
$resourceGroupName = "<Enter the resource group name>"
$storageAccountName = "Enter the storage account name>"
$location = "<Enter the Azure location>"
$vmAdmin = "<Enter the admin username>"
$vmPassword = "<Enter the password>"
$dnsLabelPrefix = "<Enter the prefix>"

New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
$vmPW = ConvertTo-SecureString -String $vmPassword -AsPlainText -Force
New-AzureRmResourceGroupDeployment -Name mydeployment0710 -ResourceGroupName $resourceGroupName `
    -TemplateFile azuredeploy.json -adminUsername $vmAdmin -adminPassword $vmPW `
    -dnsLabelPrefix $dnsLabelPrefix -storageAccountName $storageAccountName -newOrExisting "new"
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

En este tutorial, desarrollará una plantilla que permite a los usuarios elegir entre crear una cuenta de almacenamiento y usar una ya existente. La máquina virtual creada en este tutorial requiere un nombre de usuario y una contraseña de administrador. En lugar de pasar la contraseña durante la implementación, puede almacenarla previamente mediante Azure Key Vault y recuperarla en su transcurso. Para aprender a recuperar secretos de Azure Key Vault y usar los secretos en la implementación de plantillas, consulte:

> [!div class="nextstepaction"]
> [Integrate Key Vault in template deployment](./resource-manager-tutorial-use-key-vault.md) (Integración de Key Vault en la implementación de plantillas)
