---
title: Creación de plantillas de Azure Resource Manager con recursos dependientes | Microsoft Docs
description: Aprenda a crear una plantilla de Azure Resource Manager con varios recursos y a implementarla mediante Azure Portal.
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 10/09/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 50f1c81f08787181de2fe3a9f6fb97a96a2bd882
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49114319"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Tutorial: Creación de plantillas de Azure Resource Manager con recursos dependientes

Aprenda a crear una plantilla de Azure Resource Manager para implementar varios recursos.  Después de crear la plantilla, puede implementarla mediante Cloud Shell en Azure Portal.

En este tutorial, creará una cuenta de almacenamiento, una máquina virtual, una red virtual y algunos otros recursos dependientes. Algunos de los recursos no se pueden implementar hasta que existe otro recurso. Por ejemplo, no se puede crear la máquina virtual hasta que existe su cuenta de almacenamiento y la interfaz de red. Esta relación se define al marcar un recurso como dependiente de los otros. Administrador de recursos evalúa las dependencias entre recursos y los implementa en su orden dependiente. Cuando no hay recursos dependientes entre sí, Resource Manager los implementa en paralelo. Para más información consulte [Definición del orden de implementación de recursos en plantillas de Azure Resource Manager](./resource-group-define-dependencies.md).

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Preparación de Key Vault
> * Apertura de una plantilla de inicio rápido
> * Exploración de la plantilla
> * Edición del archivo de parámetros
> * Implementación de la plantilla

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/) con la extensión Resource Manager Tools.  Consulte [Instalación de la extensión ](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

## <a name="prepare-key-vault"></a>Preparación de Key Vault

Para evitar los ataques de protector de contraseña, se recomienda usar una contraseña generada automáticamente para la cuenta de administrador de la máquina virtual y usar Key Vault para almacenarla. El procedimiento siguiente crea una instancia de Key Vault y un secreto para almacenar la contraseña. También configura los permisos necesarios para que la implementación de la plantilla pueda acceder al secreto almacenado en Key Vault. Si Key Vault está en otra suscripción de Azure, se necesitan directivas de acceso adicionales. Para más detalles, consulte [Uso de Azure Key Vault para pasar el valor de parámetro seguro durante la implementación](./resource-manager-keyvault-parameter.md).

1. Inicie sesión en [Azure Cloud Shell](https://shell.azure.com).
2. Cambie al entorno favorito, ya sea **PowerShell** o **Bash** desde la esquina superior izquierda.
3. Ejecute el siguiente comando de Azure PowerShell o de la CLI de Azure.  

    ```azurecli-interactive
    keyVaultName='<your-unique-vault-name>'
    resourceGroupName='<your-resource-group-name>'
    location='Central US'
    userPrincipalName='<your-email-address-associated-with-your-subscription>'
    
    # Create a resource group
    az group create --name $resourceGroupName --location $location
    
    # Create a Key Vault
    keyVault=$(az keyvault create \
      --name $keyVaultName \
      --resource-group $resourceGroupName \
      --location $location \
      --enabled-for-template-deployment true)
    keyVaultId=$(echo $keyVault | jq -r '.id')
    az keyvault set-policy --upn $userPrincipalName --name $keyVaultName --secret-permissions set delete get list

    # Create a secret
    password=$(openssl rand -base64 32)
    az keyvault secret set --vault-name $keyVaultName --name 'vmAdminPassword' --value $password
    
    # Print the useful property values
    echo "You need the following values for the virtual machine deployment:"
    echo "Resource group name is: $resourceGroupName."
    echo "The admin password is: $password."
    echo "The Key Vault resource ID is: $keyVaultId."
    ```

    ```azurepowershell-interactive
    $keyVaultName = "<your-unique-vault-name>"
    $resourceGroupName="<your-resource-group-name>"
    $location='Central US'
    $userPrincipalName="<your-email-address-associated-with-your-subscription>"
    
    # Create a resource group
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
        
    # Create a Key Vault
    $keyVault = New-AzureRmKeyVault `
      -VaultName $keyVaultName `
      -resourceGroupName $resourceGroupName `
      -Location $location `
      -EnabledForTemplateDeployment
    Set-AzureRmKeyVaultAccessPolicy -VaultName $keyVaultName -UserPrincipalName $userPrincipalName -PermissionsToSecrets set,delete,get,list
      
    # Create a secret
    $password = openssl rand -base64 32
    
    $secretValue = ConvertTo-SecureString $password -AsPlainText -Force
    Set-AzureKeyVaultSecret -VaultName $keyVaultName -Name "vmAdminPassword" -SecretValue $secretValue
    
    # Print the useful property values
    echo "You need the following values for the virtual machine deployment:"
    echo "Resource group name is: $resourceGroupName."
    echo "The admin password is: $password."
    echo "The Key Vault resource ID is: " $keyVault.ResourceID
    ```
4. Escriba los valores de salida. Los necesitará más adelante en el tutorial

> [!NOTE]
> Cada servicio de Azure tiene unos requisitos de contraseña concretos. Por ejemplo, los requisitos de la máquina virtual de Azure se encuentran en ¿Cuáles son los requisitos de contraseña al crear una máquina virtual?

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

Las plantillas de inicio rápido de Azure consisten en un repositorio de plantillas de Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla que se usa en este tutorial se denomina [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.
4. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.
5. Repita los pasos 1 a 4 para abrir **https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json** y guarde el archivo como **azuredeploy.parameters.json**.

## <a name="explore-the-template"></a>Exploración de la plantilla

Al explorar la plantilla de esta sección, intente responder estas preguntas:

- ¿Cuántos recursos de Azure se definen en esta plantilla?
- Uno de los recursos es una cuenta de Azure Storage.  ¿Se parece la definición a la que se usó en el último tutorial?
- ¿Encuentra las referencias de la plantilla para los recursos definidos en esta plantilla?
- ¿Encuentra las dependencias de los recursos?

1. En Visual Studio Code, contraiga los elementos hasta que vea únicamente los elementos de primer nivel y los de segundo nivel dentro de **Recursos**:

    ![Plantillas de Azure Resource Manager en Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    La plantilla define cinco recursos.
2. Expanda el primer recurso. Es una cuenta de almacenamiento. La definición será idéntica a la que se usó al principio del último tutorial.

    ![Definición de la cuenta de almacenamiento de las plantillas de Azure Resource Manager en Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png)

3. Expanda el segundo recurso. El tipo de recurso es **Microsoft.Network/publicIPAddresses**. Para buscar la referencia de la plantilla, vaya a la [referencia de la plantilla](https://docs.microsoft.com/azure/templates/), escriba **dirección ip pública** o **direcciones ip públicas** en el campo **Filtrar por título**. Comparar la definición del recurso con la referencia de la plantilla.

    ![Definición de dirección IP pública de las plantillas de Azure Resource Manager en Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png)
4. Repita el último paso para buscar las referencias de la plantilla de los restantes recursos definidos en esta plantilla.  Comparar las definiciones de los recursos con las referencias.
5. Expanda el cuarto recurso:

    ![Elemento dependson de las plantillas de Azure Resource Manager en Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependson.png)

    El elemento dependsOn le permite definir un recurso como dependiente de uno o varios recursos. En este ejemplo, este recurso es un elemento networkInterface.  Depende de otros dos recursos:

    * publicIPAddress
    * virtualNetwork

6. Expanda el quinto recurso. Este recurso es una máquina virtual. Depende de otros dos recursos:

    * StorageAccount
    * networkInterface

El siguiente diagrama muestra los recursos y la información de dependencia de esta plantilla:

![Diagrama de dependencias de las plantillas de Azure Resource Manager en Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Mediante la especificación de las dependencias, Resource Manager implementa de forma eficaz la solución. Este implementa la cuenta de almacenamiento, la dirección IP pública y la red virtual en paralelo porque no tienen ninguna dependencia. Una vez implementadas la dirección IP y la red virtual, se crea la interfaz de red. Cuando se han implementado los recursos restantes, Resource Manager implementa la máquina virtual.

## <a name="edit-the-parameters-file"></a>Edición del archivo de parámetros

No es necesario hacer ningún cambio en el archivo de plantilla. Pero deberá modificar el archivo de parámetros para recuperar la contraseña del administrador de Key Vault.

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
    Reemplace **id** por el identificador del recurso del almacén de claves que creó en el último procedimiento. Es una de las salidas. 

    ![Integrar el almacén de claves y el archivo de parámetros de implementación de máquina virtual de la plantilla de Resource Manager](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Asigne los valores a:

    - **adminUsername**: nombre de la cuenta de administrador de la máquina virtual.
    - **dnsLabelPrefix**: nombre de dnsLablePrefix.
4. Guarde los cambios.

## <a name="deploy-the-template"></a>Implementación de la plantilla

Existen muchos métodos para la implementación de plantillas.  En este tutorial, va a utilizar Cloud Shell desde Azure Portal.

1. Inicie sesión en [Cloud Shell](https://shell.azure.com). También puede iniciar sesión en [Azure Portal](https://portal.azure.com) y seleccionar **Cloud Shell** en la esquina superior derecha, tal como se muestra en la siguiente imagen:

    ![Cloud Shell de Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell.png)
2. Seleccione **PowerShell** en la esquina superior izquierda de Cloud Shell y **Confirmar**.  Va a usar PowerShell en este tutorial.
3. Seleccione **Cargar archivo** en Cloud Shell:

    ![Archivo de carga de Cloud Shell de Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
4. Seleccione el archivo que guardó anteriormente en el tutorial. El nombre predeterminado es **azuredeploy.json** y **azuredeploy.paraemters.json**.  Si tiene archivos con el mismo nombre, se sobrescribirán los anteriores sin notificación.
5. En Cloud Shell, ejecute el siguiente comando para comprobar que el archivo se ha cargado correctamente. 

    ```bash
    ls
    ```

    ![Archivo de lista de Cloud Shell de Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    El nombre de archivo que aparece en la captura de pantalla es azuredeploy.json.

6. En Cloud Shell ejecute el siguiente comando para comprobar el contenido del archivo JSON:

    ```bash
    cat azuredeploy.json
    cat azuredeploy.parameters.json
    ```
7. En Cloud Shell, ejecute los siguientes comandos de PowerShell. El script de ejemplo usa el mismo grupo de recursos creado para Key Vault. Usar el mismo grupo de recursos facilita la eliminación de los recursos.

    ```powershell
    $resourceGroupName = "<Enter the resource group name>"
    $deploymentName = "<Enter a deployment name>"

    New-AzureRmResourceGroupDeployment -Name $deploymentName `
        -ResourceGroupName $resourceGroupName `
        -TemplateFile azuredeploy.json `
        -TemplateparameterFile azuredeploy.parameters.json
    ```
8. Ejecute el siguiente comando de PowerShell para que aparezca la máquina virtual recién creada:

    ```powershell
    Get-AzureRmVM -Name SimpleWinVM -ResourceGroupName $resourceGroupName
    ```

    El nombre de la máquina virtual está codificado de forma rígida como **SimpleWinVM** dentro de la plantilla.

9. Inicie sesión en la máquina virtual para probar las credenciales del administrador. 

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando los recursos de Azure ya no sean necesarios, limpie los recursos que implementó eliminando el grupo de recursos.

1. En Azure Portal, seleccione **Grupos de recursos** en el menú de la izquierda.
2. Escriba el nombre del grupo de recursos en el campo **Filtrar por nombre**.
3. Seleccione el nombre del grupo de recursos.  Verá un total de seis recursos en el grupo de recursos.
4. Seleccione **Eliminar grupo de recursos** del menú superior.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha desarrollado e implementado una plantilla para crear una máquina virtual, una red virtual y los recursos dependientes. Para aprender a implementar recursos de Azure según condiciones, consulte:


> [!div class="nextstepaction"]
> [Condiciones de uso](./resource-manager-tutorial-use-conditions.md)

