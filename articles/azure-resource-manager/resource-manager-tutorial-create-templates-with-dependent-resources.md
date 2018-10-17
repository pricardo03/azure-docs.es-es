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
ms.date: 09/07/2018
ms.topic: tutorial
ms.author: jgao
ms.openlocfilehash: fe6313c059a1dd1050240ead5f7ca8e3e1512aa6
ms.sourcegitcommit: 5843352f71f756458ba84c31f4b66b6a082e53df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2018
ms.locfileid: "47584520"
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

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Visual Studio Code](https://code.visualstudio.com/)
* Extensión de herramientas de Resource Manager  Consulte [Instalación de la extensión ](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites)

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

## <a name="deploy-the-template"></a>Implementación de la plantilla

Existen muchos métodos para la implementación de plantillas.  En este tutorial, va a utilizar Cloud Shell desde Azure Portal.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com)
2. Seleccione **Cloud Shell** en la esquina superior derecha, tal como se muestra en la siguiente imagen:

    ![Cloud Shell de Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell.png)
3. Seleccione **PowerShell** en la esquina superior izquierda de Cloud Shell.  Va a usar PowerShell en este tutorial.
4. Seleccione **Reiniciar**.
5. Seleccione **Cargar archivo** en Cloud Shell:

    ![Archivo de carga de Cloud Shell de Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-upload-file.png)
6. Seleccione el archivo que guardó anteriormente en el tutorial. El nombre predeterminado es **azuredeploy.json**.  Si tiene un archivo con el mismo nombre, se sobrescribirá el archivo anterior sin ninguna notificación.
7. En Cloud Shell, ejecute el siguiente comando para comprobar que el archivo se ha cargado correctamente. 

    ```shell
    ls
    ```

    ![Archivo de lista de Cloud Shell de Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-list-file.png)

    El nombre de archivo que aparece en la captura de pantalla es azuredeploy.json.

8. En Cloud Shell ejecute el siguiente comando para comprobar el contenido del archivo JSON:

    ```shell
    cat azuredeploy.json
    ```
9. En Cloud Shell, ejecute los siguientes comandos de PowerShell:

    ```powershell
    $resourceGroupName = "<Enter the resource group name>"
    $location = "<Enter the Azure location>"
    $vmAdmin = "<Enter the admin username>"
    $vmPassword = "<Enter the password>"
    $dnsLabelPrefix = "<Enter the prefix>"

    New-AzureRmResourceGroup -Name $resourceGroupName -Location $location
    $vmPW = ConvertTo-SecureString -String $vmPassword -AsPlainText -Force
    New-AzureRmResourceGroupDeployment -Name mydeployment0710 -ResourceGroupName $resourceGroupName `
        -TemplateFile azuredeploy.json -adminUsername $vmAdmin -adminPassword $vmPW `
        -dnsLabelPrefix $dnsLabelPrefix
    ```
    Esta es la captura de pantalla de una implementación de ejemplo:

    ![Plantilla de implementación de Cloud Shell de Azure Portal](./media/resource-manager-tutorial-create-templates-with-dependent-resources/azure-portal-cloud-shell-deploy-template.png)

    En la captura de pantalla, se utilizan estos valores:

    * **$resourceGroupName**: myresourcegroup0710. 
    * **$location**: eastus2
    * **&lt;DeployName>**: mydeployment0710
    * **&lt;TemplateFile>**: azuredeploy.json
    * **Parámetros de plantilla**:

        * **adminUsername**: JohnDole
        * **adminPassword**: Pass@word123
        * **dnsLabelPrefix**: myvm0710

10. Ejecute el siguiente comando de PowerShell para que aparezca la máquina virtual recién creada:

    ```powershell
    Get-AzureRmVM -Name SimpleWinVM -ResourceGroupName <ResourceGroupName>
    ```

    El nombre de la máquina virtual está codificado de forma rígida como **SimpleWinVM** dentro de la plantilla.

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

