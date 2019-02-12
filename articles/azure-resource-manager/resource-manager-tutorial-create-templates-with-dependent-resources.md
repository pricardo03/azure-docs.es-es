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
ms.date: 11/13/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: 5cfda4ddbf51f51d76b4ede2e44f768bd3261780
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55491763"
---
# <a name="tutorial-create-azure-resource-manager-templates-with-dependent-resources"></a>Tutorial: Creación de plantillas de Azure Resource Manager con recursos dependientes

Aprenda a crear una plantilla de Azure Resource Manager para implementar varios recursos.  Después de crear la plantilla, puede implementarla mediante Cloud Shell en Azure Portal.

En este tutorial, creará una cuenta de almacenamiento, una máquina virtual, una red virtual y algunos otros recursos dependientes. Algunos de los recursos no se pueden implementar hasta que existe otro recurso. Por ejemplo, no se puede crear la máquina virtual hasta que existe su cuenta de almacenamiento y la interfaz de red. Esta relación se define al marcar un recurso como dependiente de los otros. Administrador de recursos evalúa las dependencias entre recursos y los implementa en su orden dependiente. Cuando no hay recursos dependientes entre sí, Resource Manager los implementa en paralelo. Para más información consulte [Definición del orden de implementación de recursos en plantillas de Azure Resource Manager](./resource-group-define-dependencies.md).

En este tutorial se describen las tareas siguientes:

> [!div class="checklist"]
> * Apertura de una plantilla de inicio rápido
> * Exploración de la plantilla
> * Implementación de la plantilla

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/) con la extensión Resource Manager Tools.  Consulte [Instalación de la extensión ](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de máquina virtual. Este es un ejemplo para generar una contraseña:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Azure Key Vault está diseñado para proteger las claves criptográficas y otros secretos. Para más información, consulte [Tutorial: Integración de Azure Key Vault en Resource Manager Template Deployment](./resource-manager-tutorial-use-key-vault.md). También se recomienda actualizar la contraseña cada tres meses.

## <a name="open-a-quickstart-template"></a>Abra una plantilla de inicio rápido.

Las plantillas de inicio rápido de Azure consisten en un repositorio de plantillas de Resource Manager. En lugar de crear una plantilla desde cero, puede buscar una plantilla de ejemplo y personalizarla. La plantilla que se usa en este tutorial se denomina [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/).

1. En Visual Studio Code, seleccione **Archivo**>**Abrir archivo**.
2. En **Nombre de archivo**, pegue el código URL siguiente:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Seleccione **Abrir** para abrir el archivo.
4. Seleccione **Archivo**>**Guardar como** para guardar una copia del archivo en la máquina local con el nombre **azuredeploy.json**.

## <a name="explore-the-template"></a>Exploración de la plantilla

Al explorar la plantilla de esta sección, intente responder estas preguntas:

* ¿Cuántos recursos de Azure se definen en esta plantilla?
* Uno de los recursos es una cuenta de Azure Storage.  ¿Se parece la definición a la que se usó en el último tutorial?
* ¿Encuentra las referencias de la plantilla para los recursos definidos en esta plantilla?
* ¿Encuentra las dependencias de los recursos?

1. En Visual Studio Code, contraiga los elementos hasta que vea únicamente los elementos de primer nivel y los de segundo nivel dentro de **Recursos**:

    ![Plantillas de Azure Resource Manager en Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code.png)

    La plantilla define cinco recursos:

    * `Microsoft.Storage/storageAccounts`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
    * `Microsoft.Network/publicIPAddresses`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
    * `Microsoft.Network/virtualNetworks`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
    * `Microsoft.Network/networkInterfaces`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
    * `Microsoft.Compute/virtualMachines`. Consulte la [referencia de plantilla](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

    Resulta útil obtener cierta información básica de la plantilla antes de personalizarla.

2. Expanda el primer recurso. Es una cuenta de almacenamiento. Compare la definición del recurso con la [referencia de la plantilla](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).

    ![Definición de la cuenta de almacenamiento de las plantillas de Azure Resource Manager en Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-storage-account-definition.png)

3. Expanda el segundo recurso. El tipo de recurso es `Microsoft.Network/publicIPAddresses`. Compare la definición del recurso con la [referencia de la plantilla](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).

    ![Definición de dirección IP pública de las plantillas de Azure Resource Manager en Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-public-ip-address-definition.png)
4. Expanda el cuarto recurso. El tipo de recurso es `Microsoft.Network/networkInterfaces`:  

    ![Elemento dependson de las plantillas de Azure Resource Manager en Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependson.png)

    El elemento dependsOn le permite definir un recurso como dependiente de uno o varios recursos. El recurso depende de otros dos recursos:

    * `Microsoft.Network/publicIPAddresses`
    * `Microsoft.Network/virtualNetworks`

5. Expanda el quinto recurso. Este recurso es una máquina virtual. Depende de otros dos recursos:

    * `Microsoft.Storage/storageAccounts`
    * `Microsoft.Network/networkInterfaces`

El siguiente diagrama muestra los recursos y la información de dependencia de esta plantilla:

![Diagrama de dependencias de las plantillas de Azure Resource Manager en Visual Studio Code](./media/resource-manager-tutorial-create-templates-with-dependent-resources/resource-manager-template-visual-studio-code-dependency-diagram.png)

Mediante la especificación de las dependencias, Resource Manager implementa de forma eficaz la solución. Este implementa la cuenta de almacenamiento, la dirección IP pública y la red virtual en paralelo porque no tienen ninguna dependencia. Una vez implementadas la dirección IP y la red virtual, se crea la interfaz de red. Cuando se han implementado los recursos restantes, Resource Manager implementa la máquina virtual.

## <a name="deploy-the-template"></a>Implementación de la plantilla

Existen muchos métodos para la implementación de plantillas.  En este tutorial, va a utilizar Cloud Shell desde Azure Portal.

1. Inicie sesión en [Cloud Shell](https://shell.azure.com). 
2. Seleccione **PowerShell** en la esquina superior izquierda de Cloud Shell y **Confirmar**.  Va a usar PowerShell en este tutorial.
3. Seleccione **Cargar archivo** en Cloud Shell:

    ![Archivo de carga de Cloud Shell de Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
4. Seleccione la plantilla que guardó anteriormente en el tutorial. El nombre predeterminado es **azuredeploy.json**.  Si tiene un archivo con el mismo nombre, el archivo anterior se sobrescribe sin ninguna notificación.
5. En Cloud Shell, ejecute el siguiente comando para comprobar que el archivo se ha cargado correctamente. 

    ```bash
    ls
    ```

    ![Archivo de lista de Cloud Shell de Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    El nombre de archivo que aparece en la captura de pantalla es azuredeploy.json.

6. En Cloud Shell ejecute el siguiente comando para comprobar el contenido del archivo JSON:

    ```bash
    cat azuredeploy.json
    ```
7. En Cloud Shell, ejecute los siguientes comandos de PowerShell. Para aumentar la seguridad, utilice una contraseña generada para la cuenta de administrador de máquina virtual. Consulte [Requisitos previos](#prerequisites).

    ```azurepowershell
    $deploymentName = Read-Host -Prompt "Enter the name for this deployment"
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $adminUsername = Read-Host -Prompt "Enter the virtual machine admin username"
    $adminPassword = Read-Host -Prompt "Enter the admin password" -AsSecureString
    $dnsLabelPrefix = Read-Host -Prompt "Enter the DNS label prefix"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -Name $deploymentName `
        -ResourceGroupName $resourceGroupName `
        -adminUsername $adminUsername `
        -adminPassword $adminPassword `
        -dnsLabelPrefix $dnsLabelPrefix `
        -TemplateFile azuredeploy.json
    ```
8. Ejecute el siguiente comando de PowerShell para que aparezca la máquina virtual recién creada:

    ```azurepowershell
    $resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
    Get-AzVM -Name SimpleWinVM -ResourceGroupName $resourceGroupName
    ```

    El nombre de la máquina virtual está codificado de forma rígida como **SimpleWinVM** dentro de la plantilla.

9. RDP a la máquina virtual para comprobar que la máquina virtual se ha creado correctamente.

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
